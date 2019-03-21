## @EmbeddedId & @Embeddable
jpa에서 복합PK를 객체에서 표현하는 방식 중 하나.   
복합PK를 모아 하나의 클래스로 정의한 후, 그 클래스를 Id로 삼는 방식이다.
```java
public class Schedule {
  @EmbeddedId
  ScheduleId id;
}
```
```java
@Embeddable
public class ScheduleId {
  String owner;
  LocalDateTime dateTime;
}
```   


## @IdClass와 @EmbeddedId의 차이
### @EmbeddedId   
장점 : 구조를 객체지향적으로 만들 수 있다. 복합키 자체가 의미있는 객체일 때 좋음.   
단점 : 복합키 안에 포함된 컬럼을 조회하려 할 때 불편함.   

### @IdClass   
장점 : 외래키를 가지고 있는 쪽에서도 명시적으로 각각의 컬럼을 볼 수 있다. 간단하다.   
단점 : 코드가 지저분해보인다.```updatable = false, insertable = false```  복합키가 한눈에 세트로 안보임.   

http://woowabros.github.io/experience/2019/01/04/composit-key-jpa.html
