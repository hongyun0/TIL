# Effective Java - 2장

## 7. 다 쓴 객체 참조를 해제하라.

- 명시적으로 null 처리 하기
- 변수의 scope을 최소화 하기
- 캐시 & 콜백 주의하기
- Weak Reference 사용 하기
- 오랫동안 사용하지 않은 값 정리하기



#### Weak Reference – WeakHashMap

```java
WeakHashMap<Integer,String> map = new WeakHashMap<>();Integer key1 = 1000;

Integer key1 = 1000;
Integer key2 = 2000;

map.put(key1, "test a");
map.put(key2, "test b");

key1 = null;

System.gc();  //강제 Garbage Collection

map.entrySet().forEach(System.out::println);
```

<http://blog.breakingthat.com/2018/08/26/java-collection-map-weakhashmap/>



## 8.Finalizer와 Cleaner의 사용을 피하라.

- 수행 될지, 언제 수행 될지 알 수 없다.
- Finalizer thread는 우선순위가 낮아 실행되지 않을 가능성이 높다.
- Cleaner는 thread를 제어할 수 있지만 그래도 수행 여부를 보장할 수 없다.
- Finalizer 동작 중 발생한 예외가 무시된다.
- 심각한 성능 문제 – AutoCloseable 구현에 비해 50배 느리다.
- Finalizer 공격에 노출될 수 있다.



## 9.Try-with-resources를 사용하라.

- Try-finally를 사용할 경우, Try 블록과 Finally 블록에서 둘다 예외가 생긴다면 Try 블록의 예외가 무시된다.
- AutoCloseable을 구현하여 close()를 이용해 자원을 회수하면,
  - Try-with-resources 문법을 사용해 블록이 끝나면 자동으로 close()를 호출
  - 예외가 버려지지 않는다.
  - Try-with-resources 뒤에 catch 절을 붙일 수 있다.



#### Try-with-resources – 중첩된 경우

```java
try (B b =new B(new A())){
  
   }catch(Exception e){

 }
```

=> A 자원이 회수되지 않는다.

```java
try (A a =new A();
     B b =new B(a)){

 }catch(Exception e){

 }
```

=> A, B 둘다 close()가 불리며 자원 회수됨.

<https://multifrontgarden.tistory.com/192>

