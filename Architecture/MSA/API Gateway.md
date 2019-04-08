##  API Gateway

### cross-service concerns

보안, 인증 authentication, 인가 authorization

일정량 이상의 요청 제한 rate limiting

계측 metering - log? 분석?

 \+ 에러 필터 이용해서 사용자에게 에러 정리해서 보여주기



### Netflix Zuul

마이크로 프록시

AWS ELB 앞단에 위치해있음

한 region에서 장애가 나면 안에서 다른 region으로 처리해줌

각 API 경로(서버군) 별로 Circuit Breaker 생성. (분리)

Hystrix, Ribbon, Eureka

Eureka + Riboon에 의해서 결정된 주소로 Http Client 요청

Ribbon에 의해 자동 Retry



##### Semaphore??????????

os에서 리소스를 사용하기 위해 부여받아야 하는 것. 소위, 화장실키

Thread 기준이 아닌 Semaphore를 기준으로 하면 thread에 에러가 났을 경우 다시 그 thread를 인터럽트 해올 수 없는 단점.



### Spring Cloud Config

git <-> microservice 사이에 Properties를 하나로 모아서 같은 설정을 공유

설정 서버에서 각각의 서비스가 properties를 읽어감

Refresh event 쏘기 -> Refresh scope에 해당하는 Bean 전부 재생성 : 근데 검증 아직 못함....



### Zipkin, Spring Cloud Sleuth





## +a

코드리뷰

TDD

DB 분리 방법

분산 트랜잭션 or 보상 트랜잭션? or Event 기반으로 처리?

Event Driven Architecture

...

