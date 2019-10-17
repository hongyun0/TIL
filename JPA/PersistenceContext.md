# PersistenceContext
JPA는 Repository에서 save / delete를 실행하더라도 실제 DB 쿼리가 바로 실행되지 않는다.   
PersistenceContext에 persist or merge / remove 된다.   

https://umanking.github.io/2019/04/12/jpa-persist-merge/

Spring Webflux에서 주의할 점은 이 PersistenceContext가 유지되지 않는다는 점이다.   
