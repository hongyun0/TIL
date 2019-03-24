## Validation
```java
  @PostMapping("/validTest")
  public void save(@RequestBody @Valid Customer customer, BindingResult bindingResult){
    if(bindingResult.hasErrors()){
      FieldError fieldError = bindingResult.getFieldError();
      throw new RuntimeException(fieldError.getDefaultMessage());
    }
  }
```
http://wonwoo.ml/index.php/post/1082

### @어노테이션
#### 자바EE 8
[javax.validation.constraints](https://javaee.github.io/javaee-spec/javadocs/javax/validation/constraints/package-frame.html)   

@AssertFalse  거짓인지?   
@AssertTrue 참인지?   
@DecimalMax 지정 값 이하의 실수인지?   
@DecimalMin 지정 값 이상의 실수인지?   
@Digits(integer=,fraction=) 정수 여부   
@Future 미래 날짜인지?   
@Max 지정 값 이상인지?   
@Min 지정 값 이하인지?   
@NotNull  Null이 아닌지?   
@Null Null인지?   
@Pattern(regex=,flag=) 정규식을 만족하는지?   
@Past   과거날짜인지?   
@Size(min=,max=)  문자열 또는 배열등의 길이 만족 여부       

#### 하이버네이트
@Email      email 형식에 맞는지?   
@Length(min=,max=) 문자열 길이가 최소~최대 인지?   
@NotEmpty empty가 아닌지?   
@Range(min=,max=) 숫자범위   
@URL  url 형식에 맞는지?   

https://appsnuri.tistory.com/115

### Validator 구현
validate(), support()
