# JPA EntityListener가 Bean을 주입 받지 못하는 이유

EntityListener는 JPA Entity에 변경사항이 발생할 때, 로깅 등의 전/후처리를 하기 위해서 사용한다.

Entity 자체에 메서드를 선언할 수도 있고, 별도 클래스에 정의한 후 @EntityListeners 애너테이션의 인자로 전달하는 방법도 있다.

이 기능을 사용하다보면 전/후처리 시 Spring Bean을 사용하고 싶은 상황이 생기기도 한다.

하지만 EntityListener에 Repository Bean을 주입할 경우, 사용하려 할 때 NPE가 터지고 만다.

 

## 그렇다면 EntityListener는 POJO인걸까?

아니다. EntityListener를 대상으로 AOP를 걸어놓을 경우 정상적으로 동작한다.

Spring Bean임에도 불구하고 Bean 주입은 불가능한 불가사의한 객체인 것이다.

 

구글 검색을 해보면, 이 상황을 회피하는 다양한 방법을 찾아볼 수 있다.

https://stackoverflow.com/questions/12155632/injecting-a-spring-dependency-into-a-jpa-entitylistener

(크게 static 영역에 Bean을 등록해놓고 사용하는 방법과 AOP를 이용하는 방법이 있다.)   


## 하이버네이트의 Bean 초기화 시점   
어딘가 찝찝한 위 해결방법을 가지고 고민하고 있을 때, 동료를 통해 알게된 부분이 하이버네이트의 Bean 생성 시점 탓에 의존성 주입이 되지 않는다는 사실이었다.
생성 시점은 다음과 같다.   

```java
package org.springframework.orm.hibernate5;

public final class SpringBeanContainer implements BeanContainer {
	...
    
    	private SpringContainedBean<?> createBean(
                Class<?> beanType, LifecycleOptions lifecycleOptions, BeanInstanceProducer fallbackProducer) {

            try {
                if (lifecycleOptions.useJpaCompliantCreation()) {
                    return new SpringContainedBean<>(
                            this.beanFactory.createBean(beanType, AutowireCapableBeanFactory.AUTOWIRE_CONSTRUCTOR, false),
                            this.beanFactory::destroyBean);
                }
                else {
                    return new SpringContainedBean<>(this.beanFactory.getBean(beanType));
                }
            }
            catch (BeansException ex) {
                ...
                try {
                    return new SpringContainedBean<>(fallbackProducer.produceBeanInstance(beanType));
                }
                catch (RuntimeException ex2) {
                    ...
                }
            }
        }
    ...
  }
  ```   
  하이버네이트는 내부적으로 먼저 ApplicationContext를 활용해서 빈을 생성하려고 한다.   
  하지만, Repository 생성 이전에 EntityListener를 생성하기 때문에 리스너 내부에서 아직 생성되지 않은 빈을 가져오려다가 예외가 발생하고 만다.   
  그런데 여기서 끝내지 않고 한번 더 예외를 잡아서 선언된 의존성과는 별도의 빈으로 등록하게 된다.   
  
  주입받아야하는 객체에서는 NPE가 터지지만, AOP는 잘 동작하는 이유가 여기에 있었다.
  
  ## @Lazy
  이 이슈의 해결방법은 lazy initialization이다.   
  startup 시점이 아니라 runtime 시점에 해당 객체를 사용하려고 할 때 초기화하는 것이다.   
  간단히 @Lazy 애너테이션을 통해 설정할 수 있다.   
  
  @Autowired를 걸어둔 필드에 @Lazy 애너테이션을 추가하면 해당 필드를 처음 사용하는 시점에 의존성 주입을 시도하게 되어 이미 초기화된 Repository 객체를 받아올 수 있다.   
  https://www.baeldung.com/spring-lazy-annotation
