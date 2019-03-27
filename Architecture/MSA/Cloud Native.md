## Cloud Native

DevOps?

민첩하고 확장 가능한 구성요소를 사용해서 재사용 가능한 개별적인 기능을 제공

기술 경계 간에 매끄럽게 통합 - 반복 가능한 자동화



신축성 - 시스템이 중단되어도 전체적인 장애X 빠르게 복구

민첩성 - 빨리 배포

확장 가능성 - Scale out

자동화 - 운영리소스 감소

무상태 - 완전히 독립적?



## DevOps

You run it, you build it. 만들면 운영까지 - 베르너 보겔스, 아마존 CTO

하나의 조직 안에 개발하는 사람과 운영하는 사람이 함께 있어야 한다. 빠르게 의사소통 할 수 있는.



## 12 Factors

제약조건

best practices for elastic, portable release



선언적 형식으로 설정 자동화 (<-> 명령적) => 새로운 동료가 빨리 적응



### 코드 베이스

코드베이스와 앱 사이에는 항상 1:1

### 의존성

의존관게는 명시적으로 선언. 모르게 jar파일 갖다쓰고 X

### 설정

설정정보는 실행 환경에 저장. 코드와 분리. ex) yaml

### Backing Service

지원 서비스(DBMS, 외부 API 등)는 자원으로 취급. 변경이 쉽게. 

### 빌드, 릴리즈, 실행

코드베이스는 3단계를 거쳐 배포로 변환된다.

릴리즈 단계는 환경설정, 버저닝.

### 포트 바인딩

스스로 웹서버를 포함하고 있어야 함. self-contained

### 동시성

concurrency. 프로세스나 스레드를 수평적으로 확장해서 병렬로 실행할 수 있어야 한다.

### 처분성

Disposability. graceful Shutdown을 위한 최적화.

### dev/prod 일치

dev, stage, prod 환경을 최대한 비슷하게 유지.

parity를 통해 divergence 예방

### 로그

로그는 stdout에 남긴다.

애플리케이션은 로그 파일 저장에 관여하지 않는다. 실행 환경에 의해 처리되어야 한다. ex) Kibana

### Admin

admin/maintenance 작업을 일회성 프로세스로 실행

admin 코드는 애플리케이션과 함께 배포되어야한다.



## HTTP, REST API

### HTTP

클라이언트의 상태를 갖지 않음(stateless)

각 요청은 self-contained

=> 서비스가 수평적으로 확장되기 쉬움

REST vs SOAP, EJB

