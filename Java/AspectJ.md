# Annotation으로 조인포인트 정의하기
```java
@Around("@annotation(Foo) || @within(Foo)")   
```   

```java
@within
```
- 매칭할 조인포인트를 어노테이션이 붙은 타입과 그 안에 있는 멤버들로 제한한다.   

```java
@annotation
```
- 매칭할 조인포인트를 어노테이션이 붙어있는 부분만으로 제한한다.   
