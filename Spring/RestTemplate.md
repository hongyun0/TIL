# RestTemplate Initialization
기본적으로 생성은 한번만 한다.   
BaseUrl을 설정하고 싶으면 DefaultUriTemplateHandler를 이용한다.   
```java
public class MyRestTemplate {
  private static final String MY_URL = "http://my.url.com/sample";

  private final RestTemplate restTemplate;

  public IsApiClient() {
    this.restTemplate = this.initializeRestTemplate();
  }

  private RestTemplate initializeRestTemplate() {
    DefaultUriTemplateHandler defaultUri = new DefaultUriTemplateHandler();
    defaultUri.setBaseUrl(MY_URL);

    return new RestTemplateBuilder()
        .uriTemplateHandler(defaultUri)
        .build();
  }
}
```

# Response with List of Objects
기본적으로 List의 제네릭을 포함해서 Class 파라미터를 넘길 수 없다.   
이를 해결하기 위한 방법은 여러 가지가 있다.   
간단한 방법으로는   
```java
client.getForObject(uri, MyClass[].class);
```   
Array로 먼저 값을 받은 후, Arrays.asList()를 이용해 변환하여 사용하는 방법이 있다.   

이거 말고!   
메서드 파라미터에서 제네릭에 해당하는 클래스를 받아서 그 제네릭에 맞는 리스트로 바로 반환하고 싶었다.   
ParameterizedTypeReference와 Response를 한번 감싸줄 Wrapper클래스를 이용해서 구현할 수 있었다.   
```java
public class ResponseWrapper <T> {
    T response;

    public T getResponse () { return response; }
    public void setResponse(T response) {this.response = response;}
}
```   
```java
public <T> ResponseWrapper<T> makeRequest(URI uri, Class<T> clazz) {
   ResponseEntity<ResponseWrapper<T>> response = template.exchange(
        uri,
        HttpMethod.GET,
        null,
        ParameterizedTypeReference.forType(ResolvableType.forClassWithGenerics(ResponseWrapper.class, clazz).getType()));
    return response;
}
```   
https://stackoverflow.com/questions/21987295/using-spring-resttemplate-in-generic-method-with-generic-parameter

WrapperClass에서 제네릭을 이용해 List를 필드로 갖고있게 했더니 data라는 jsonObject에 들어있던 jsonArray를 꺼내올 수 있었다.   
```java
public class ResponseList<T> {
  @JsonProperty("data")
  List<T> responseList;
}
```
