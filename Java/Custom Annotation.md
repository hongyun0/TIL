# Custom Annotation

속성은 눈에 명확히 보이면서, 복잡한 로직은 한 꺼풀 숨겨두고 싶을 때 유용하게 쓸 수 있다.

if문을 사용할 경우, 비즈니스 로직과 얽혀 코드가 복잡해진다.

AOP만 사용할 경우, 필터링 작업이 눈에 보이지 않아 추후에 혼란을 야기할 수 있다.

Custom Annotation과 AOP를 함께 사용하면 코드는 분리하면서, 작업을 따로 정의해두었다는 표시도 눈에 잘 띄도록 할 수 있다.
 

## Example
### Custom Annotation
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface StatusInclude {
  Status[] value();
}
``` 

### Aspect
```java
@Aspect
@Component
public class StatusIncludeAspect {
  @Around("@annotation(statusInclude) && execution(* *.SomeService.process(..)) && args(*, status)")
  public void proxy(ProceedingJoinPoint jp, StatusInclude include, Status status) throws Throwable {
    if (Arrays.asList(include.value()).contains(status)) {
      jp.proceed();
    }
  }
}
``` 

### Enum
```java
public enum Status {
  CREATED, READY, RUN, WAIT, COMPLETE
}
``` 

### Usage
```java
public class SomeService {
  @StatusInclude({READY, COMPLETE})
  public void process(Status status, Object object) {
    doSomething();
  }
}
``` 


메서드 내부에서는 실질적인 로직만 구현하고,

해당 로직이 실행되어야 하는 조건은 Annotation으로 분리해서 설정할 수 있어서 가독성과 코드 유연성을 높일 수 있었다.
