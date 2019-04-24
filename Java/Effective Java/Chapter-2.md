# Effective Java - 2장

## 1. 생성자 대신 static factory method를 고려하라.

**장점**

- 이름을 가질 수 있다.
- 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.
- 반환 타입의 하위 타입 객체를 반환할 수 있다. ex) java.util.Collections
- 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
- 정적 팩터리 매서드를 작성하는 시점에는 구현체가 존재하지 않아도 된다. ex) JDBC

**단점**

- 상속을 하려면 public/protected 생성자가 필요하기 때문에 하위 클래스를 만들 수 없다.
- 프로그래머가 찾기 어렵다.
  - 공유된 규약을 따라 메서드 이름을 짓자.
    - from, of, valueOf, getInstance, create, get*Type*, new*Type*, *type*



## 2. 생성자에 매개변수가 많다면 빌더를 고려하라.

- 점층적 생성자 패턴 => 매개변수가 많아지면 알아보기 어렵고 실수가 쉽게 생긴다.
- 자바빈즈 패턴 => 객체 하나를 만들기 위해 호출해야하는 메서드가 많아진다. 불변으로 만들 수 없다.
- 빌더 패턴 => 읽기 쉽고 유연하다.
  - fluent API, method chaining



## 3. private 생성자나 열거 타입으로 싱글턴임을 보증하라.

- public static final 필드 방식
  - 싱글턴임이 API에 명백히 드러난다.
  - 간결하다.
- static factory 방식(private static final 필드)
  - 싱글턴이 아니게 변경할 수 있다.
  - 제네릭 싱글턴 팩터리로 만들 수 있다.
  - 메서드 참조를 Supplier로 사용할 수 있다.

=> 위 두 가지 방식은 직렬화 할 때,

1. Serializable 구현
2. 인스턴스 필드를 transient라고 선언
3. readResolve 메서드를 제공해야한다.

=> 간결한 직렬화와 리플렉션 공격 방어를 위한 가장 좋은 방법은

- 원소가 하나인 enum 방식
  - 단, 싱글턴이 Enum 외의 클래스를 상속해야 한다면 이 방법은 사용할 수 없다.)



## 4. 인스턴스화를 막으려면 private 생성자를 사용하라.

static 멤버만으로 이루어진 유틸리티 클래스를 만들 때에는 private 생성자를 선언해 인스턴스화를 막는다.



## 5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라.

- 사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않다.
- 인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨준다.
- 팩터리 메서드 패턴을 적용할 수 있다. -> 파라미터에 Supplier<? extends Object>



## 6. 불필요한 객체 생성을 피하라.

- String new하지 말기
- 생성 비용이 비싼 객체(Pattern - 정규표현식 객체)는 캐싱하여 재사용하기
- 불변 객체거나 같은 역할을 하는 인스턴스일 경우 재사용하기
- 오토박싱 조심하기 ex) Long 객체에 long 타입 변수를 반복해서 더하는 경우
- 하지만, 재사용할 경우 위험한 객체는 새로 만드는 게 낫다!


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

