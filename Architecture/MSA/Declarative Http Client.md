## Declarative Http Client - Feign

Ribbon + Eureka + Hystrix 통합 + RestTemplate 대체

RestTemplate - concreate class -> test 어려움

서비스의 관심 - 다른 리소스의 값 갖다쓰기   

url, parsing 노관심

restTemplate을 대체하는 인터페이스



네임(서비스아이디), url 설정 - url없으면 ribbon 이용해서 유레카 불러줌

@RequestMapping을 이용해 파라미터 설정

컨트롤러에서 Feign 인터페이스를 DI받아 사용

메소드 형태로 사용 가능



FallbackMethod 인터페이스 상속 받을 때 

FallbackFactory를 통해서 Throwable을 받아서 처리할 수 있다.
