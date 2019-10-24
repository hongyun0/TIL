안깨지던 에러 메시지가 갑자기 한글 인코딩이 깨져보이는 이슈.

찾아보니 SpringBoot 2.2 버전으로 업그레이드 되면서 디폴트 ContentType이 변경되었다.   


```java
package org.springframework.http;

public class MediaType extends MimeType implements Serializable {
  ...

  /**  
  * Public constant media type for {@code application/json;charset=UTF-8}.  
  * @deprecated Deprecated as of Spring Framework 5.2 in favor of {@link #APPLICATION_JSON}  
  * since major browsers like Chrome  * <a href="https://bugs.chromium.org/p/chromium/issues/detail?id=438464">  
  * now comply with the specification</a> and interpret correctly UTF-8 special  
  * characters without requiring a {@code charset=UTF-8} parameter.  
  */ 
  @Deprecated public static final MediaType APPLICATION_JSON_UTF8;

  ...
 }
 ```   
    
링크를 따라가서 Chromium 이슈를 확인해보니

https://bugs.chromium.org/p/chromium/issues/detail?id=438464

https://chromium-review.googlesource.com/c/chromium/src/+/587829



크롬이 그동안 UTF-8 스펙에 안따라줘서 스프링이 뒷처리 해주고 있었는데,

크롬에서 드디어 UTF-8을 디폴트로 적용해줘서 스프링 5.2버전부터 그부분을 제거하게 된 것!

크롬 영향력 대단하넹 재밌당





그런데 에러메시지가 깨져보였던 이유는,

UTF-8에 대한 명시적인 설정이 사라지는 바람에 RestClientResponseException에서 DEFAULT_CHARSET(ISO-8859-1)을 사용하게 되었고 이게 인코딩이 안맞았던 것!!

https://github.com/spring-projects/spring-framework/pull/23764


다른사람들이 이미 이슈 올려놔서 UTF-8로 변경은 됐는데 다음 버전에 묻혀나올듯.

근데 API를 호출하는 타 서버들의 스프링 버전이 언제 올라갈지 알 수 없으니...

하위호환성을 위해서 다시 UTF-8 charset을 설정해주어야 한다.



WebFilter를 이용해 header를 확인해서 application/json일 경우, application/json;charset=UTF-8로 변경해주면 된다.   

```java
@Component
public class ContentTypeFilter implements WebFilter {

  @Override
  public Mono<Void> filter(ServerWebExchange exchange, WebFilterChain chain) {

    exchange.getResponse().beforeCommit(() -> {
      var headers = exchange.getResponse().getHeaders();
      if (MediaType.APPLICATION_JSON.equals(headers.getContentType())) {
        headers.setContentType(MediaType.APPLICATION_JSON_UTF8);
      }
      return Mono.empty();
    });

    return chain.filter(exchange);
  }
}
```   

beforeCommit()은 ServerResponse가 write작업을 하기 직전에 호출되어야하는 일을 추가하는 메서드다.

Webflux에서는 ServerResponse의 구현체로 ReactorServerHttpResponse를 사용하는데 여기서는 netty로 dataStream을 보내는 게 write작업이다.

```java
package org.springframework.http.server.reactive;

class ReactorServerHttpResponse extends AbstractServerHttpResponse implements ZeroCopyHttpOutputMessage {
	...
    
    @Override
	protected Mono<Void> writeWithInternal(Publisher<? extends DataBuffer> publisher) {
		return this.response.send(toByteBufs(publisher)).then();
	}

	...
}
```   
   
   



\+

스프링부트 2.2는 추가적인 변경이었고, 스프링 프레임워크 5.2의 변경이 선행작업이었다.

https://github.com/spring-projects/spring-framework/commit/89454e69c30e41db84dfa0dc4664b8481e29770e

Jackson2JsonEncoder의 MimeType 디폴트값이 변경됨!


```java
package org.springframework.boot.autoconfigure.http.codec;

@Configuration(proxyBeanMethods = false)
@ConditionalOnClass({ CodecConfigurer.class, WebClient.class })
@AutoConfigureAfter(JacksonAutoConfiguration.class)
public class CodecsAutoConfiguration {

	...
    
	@Configuration(proxyBeanMethods = false)
	@ConditionalOnClass(ObjectMapper.class)
	static class JacksonCodecConfiguration {
        @Bean
		@Order(0)
		@ConditionalOnBean(ObjectMapper.class)
		CodecCustomizer jacksonCodecCustomizer(ObjectMapper objectMapper) {
			return (configurer) -> {
				CodecConfigurer.DefaultCodecs defaults = configurer.defaultCodecs();
				defaults.jackson2JsonDecoder(new Jackson2JsonDecoder(objectMapper, EMPTY_MIME_TYPES));
				defaults.jackson2JsonEncoder(new Jackson2JsonEncoder(objectMapper, EMPTY_MIME_TYPES));
			};
		}

	}
    
    ...
}
```   


```java
package org.springframework.http.codec.json;

public abstract class Jackson2CodecSupport {

	...
    
    private static final List<MimeType> DEFAULT_MIME_TYPES = Collections.unmodifiableList(
		Arrays.asList(
			new MimeType("application", "json"),
			new MimeType("application", "*+json")));
            
    ...
    
    protected Jackson2CodecSupport(ObjectMapper objectMapper, MimeType... mimeTypes) {
		Assert.notNull(objectMapper, "ObjectMapper must not be null");
		this.objectMapper = objectMapper;
		this.mimeTypes = !ObjectUtils.isEmpty(mimeTypes) ?
			Collections.unmodifiableList(Arrays.asList(mimeTypes)) : DEFAULT_MIME_TYPES;
	}
    
    ...
 
 }
 ```
 
DEFAULT_MIME_TYPES의 변경으로 인해 디폴트 헤더정보가 변경되었다.


```java
@Configuration(proxyBeanMethods = false)
public class HttpCodecMediaTypeConfiguration {

  @Bean
  public CodecCustomizer MediaTypeCodecCustomizer(ObjectMapper objectMapper) {
    return codecConfigurer -> {
      codecConfigurer.defaultCodecs().jackson2JsonEncoder(new Jackson2JsonEncoder(objectMapper, MediaType.APPLICATION_JSON_UTF8));
    };
  }

}
```
CodecCustomizer를 이용하면 기본 Encoder를 변경할 수 있다!

(저기 붙어있는 proxyBeanMethods도 요번에 새로 생김. 아직 무슨 용도인지는 이해하지 못했음.)

https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-webflux-httpcodecs

