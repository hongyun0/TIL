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

=> 확장된 Observer 패턴

```
대표주자 : ReactiveX vs Reactor
```


## Reactive Streams API 기본 스펙
**Processor**

**Publisher** : subscribe()

**Subscriber** : onSubscribe() onNext()* ( onError() | onComplete() )

**Subscription** : request() cancel()



## Process 정리

Subscriber가 Publisher의 **subscribe()** 를 호출한다.

Publisher는 Subscription을 만든 후 Subscriber의 **onSubscribe()** 를 호출해서 넘겨준다.

Subscriber의 onSubscribe()에서 Subscription의 **request()** 를 실행시킨다.

Subscription의 request()에서는 Subscriber의 **onNext()** (+ **onComplete()**)를 실행시킨다.

Subscribe 모든 과정에서 일어나는 에러는 **onError()** 로 전달된다.


## BackPressure

Publisher와 Subscriber 간에 성능 차이가 있을 때,

Subscriber에서 처리할 수 있는 데이터 양만큼만 Publisher가 건네주도록 조절한다.
   

장점 : 메모리 사용량을 줄일 수 있다.

처리량을 넘는 데이터가 들어올 때 직접 버퍼를 두고 쌓아두었다가 처리하게 되면

메모리 사용량이 확 늘어났다가 처리되면서 점점 줄어들고 또다시 확 늘어났다가 줄어들기를 반복하는데

미리 처리가능한 데이터 양만큼만 받아오도록 하면 캐싱에 필요한 메모리 사용량이 줄어들어 일정한 메모리 사용량을 유지할 수 있다는 이점이 있다.



#### Q. Publisher에서 여러 스레드로 쪼개서 데이터를 팡팡 보내도 되나요?

```
스펙 상 불가능!

Subscriber는 sequential하게 데이터가 날아오기를 기대하면 된다.

한 Subscription에 대해서 한 스레드가 데이터를 보내는 작업을 맡는다.
```





