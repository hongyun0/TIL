## Netflix OSS

50개 이상의 프로젝트를 오픈소스로 공개

Hystrix

EvCache

Spectator

RxJava



### spring-cloud-netflix

Spring Cloud와 Netflix OSS의 교집합

Hystrix

Eureka

Ribbon

Zuul



### 모놀리틱에서의 호출

무조건 신뢰. ex) controller - service - repository



### 클라우드 환경에서의 호출

***Failure as a First Class Citizen***

분산 시스템, 특히 클라우드 환경에선 **실패는** 일반적인 **표준이다.**

가동율은 최대 99.99%

 -> 이상적으로 한달에 99.7% uptime을 갖고있어도 10억 요청중 300만 요청은 실패한다.

-> 매달 2시간 이상의 downtime이 발생

감안해야 한다.



### Circuit Breaker - Hystrix

일명 누전차단기!

분산처리할 때 저 안쪽에서 failure이 발생하면 요청한 쪽에서는 대기에 빠지고

계속 요청을 보내서 결국 Network Failure 하~~얀 화면

Hystrix는 요청들을 통계내서 circuit open 제어



#### Circuit Open

Circuit이 Open되면 호출이 제한되며 즉시 에러를 반환한다.

리소스를 아끼기 위한 것. -> 특정 부분의 지연때문에 리소스를 전부 소모하여 시스템 전체의 장애가 유발되지 않도록

장애를 유발하는 외부 시스템에 대한 연동을 조기에 차단(Fail Fast)

초기값 - 10초에 20번 이상 요청보냈을 때 50% 이하 성공률이면 차단

@HystrixCommand(fallbackMethod="에러전달 메소드")

서킷오픈 시 fallbackMethod를 대신 실행

Timeout

초기값 - 1000ms => 주의! 1초 이상이 정상일 수 있는 경우

cf. 세마포어?????? 작업큐?



#### Timeout

각각의 연동의 최대 응답시간을 설정해야 한다. 이부분을 잘 논의해야함. ex) 외부 API, DB 등등

