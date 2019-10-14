# JPA EntityListener가 Bean을 주입 받지 못하는 이유

EntityListener는 JPA Entity에 변경사항이 발생할 때, 로깅 등의 전/후처리를 하기 위해서 사용한다.

Entity 자체에 메서드를 선언할 수도 있고, 별도 클래스에 정의한 후 @EntityListeners 애너테이션의 인자로 전달하는 방법도 있다.

이 기능을 사용하다보면 전/후처리 시 Spring Bean을 사용하고 싶은 상황이 생기기도 한다.

하지만 EntityListener에 Repository Bean을 주입할 경우, 사용하려 할 때 NPE가 터지고 만다.

 

### 그렇다면 EntityListener는 POJO인걸까?

아니다. EntityListener를 대상으로 AOP를 걸어놓을 경우 정상적으로 동작한다.

Spring Bean임에도 불구하고 Bean 주입은 불가능한 불가사의한 객체인 것이다.

 

구글 검색을 해보면, 이 상황을 회피하는 다양한 방법을 찾아볼 수 있다.

https://stackoverflow.com/questions/12155632/injecting-a-spring-dependency-into-a-jpa-entitylistener

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
