Service Discovery

## Server Side LoadBalancer

L4 Switch - 보통 H/W

서버 목록 추가가 어려움

12 factors의 dev/prod 똑같이 가는게 불가능



## Client LoadBalancer - Ribbon

Client에서 서버 목록을 가지고 로드밸런싱

### Ribbon

S/W로만 가능

서버 목록의 동적 변경이 자유로움

Load Balancing Schema 마음대로 구성 가능

interceptor를 기반으로 동작함

LoadBalancerInterceptor를 통과하면서 

Retry 기능

기본적으로 RoundRobin으로 동작한다.

리스트 중 실패하는 경우가 있을 경우 설정된 retry 횟수만큼 다른 서버주소로 재시도 함.

BackOff - 실패한 서버로의 호출은 특정 시간동안 Skip, 그 간격은 조정됨

서버선택, Skip 시간, Ping 체크



## Eureka

netflix

### Service Registry

소위, 클라우드의 전화번호부

### DiscoveryClient

spring-cloud에서 제공

service registry를 사용하기 위한 인터페이스를 추상화



