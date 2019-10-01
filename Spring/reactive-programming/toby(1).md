>거대한 프레임워크도 시작은 단순하고 분명한 목표로부터 나온다!


```java
* 알아가야 할 것

 - Reactive Programming?

 - event 기반?

 - Functional Reactive Programming? FRP

 - Reactive Functional Programming? RFP

   뭐가 다르죠?
```
 

## Duality
**쌍대성** : 궁극적으로는 같은 기능인데 그것을 서로 반대방향에서 구현한 것 (에릭 마이어)
   
   
## Iterable <---> Observable

```java
Iterable <-- Collection <-- List

Iterator : hasNext(), next()
```
```java
Observable : addObserver(Observer), notifyObservers(Object)

Observer : update(Observable, Object)
```

### Iterable <---> Observable

```java
           DATA method() <---> void method(DATA)

int i = iterator.next(); <---> notifyObservers(i);

         사용하는 쪽에서 호출 <---> 제공하는 쪽에서 호출

                pull 방식 <---> push 방식
```

   
## Observer Pattern
```java
Observable(=Data Source) -> Event/Data -> Observer
```


## Reactive Streams
**표준** : Java + JVM 기반 언어 개발 시 이렇게 만들자한 것   `-> Java9 API에도 들어왔다!`

 
### 기존 Observer Pattern에 대한 문제인식
  ```java
  더 좋은 Observer Pattern을 만들 수 있다!

  * 2가지 개선 지점
    - Complete 시점을 알 수 있게 만들자.
    - Error handling!
  ```
