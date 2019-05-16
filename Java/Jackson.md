# UnrecognizedPropertyException 발생할 때
 읽으려는 JSON 스트링에 바인딩 객체에 없는 프로퍼티가 존재하게 되면 예외가 발생한다.   
 간단하게 해결하는 방법은 필요없는 값을 무시하는 것이다.   
 ```java
 @JsonIgnoreProperties(ignoreUnknown = true)
 ```
 바인딩할 클래스마다 각각 애너테이션을 추가하면 된다.   
 
 혹은 ObjectMapper Config에서 ```DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES``` 를 false로 설정해주면 된다.
 
 ## Jackson In Spring
 
 ObjectMapper를 스프링에서 주입 받아 사용했을 때는 예외가 나지 않다가 new로 생성해서 테스트에 사용했더니 바로 예외가 나서 스프링 레퍼런스를 뒤져보았다.   
 
 ObjectMapper를 스프링을 통해 주입받아 사용하게 되면   
 스프링에서 Jackson 기본 설정을 변경해주고 있기 때문에 예외가 발생하지 않는다.   
 
 [Spring Framework 5.0.1 Reference](https://docs.spring.io/spring-integration/docs/5.0.1.RELEASE/api/org/springframework/integration/support/json/Jackson2JsonObjectMapper.html)
 
 ```
It customizes Jackson's default properties with the following ones:

  - MapperFeature.DEFAULT_VIEW_INCLUSION is disabled
  - DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES is disabled
  - The well-known modules are registered through the classpath scan
 ```
 
 DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES 설정이 false로 처리되기 때문에   
 바인딩할 클래스에 없는 값은 자동으로 무시한다.
