## MySQL Keywords and Reserved Words
키워드로 정해놓은 단어는 컬럼명 등에 사용할 수 없다.   
워크벤치를 통해 DDL을 실행하면 키워드 제한 없이 테이블이 생성되지만,   
실제 DML을 실행하려고 하면 MySQL Syntax error가 나면서 실행되지 않는다.   
```com.mysql.jdbc.exceptions.MySQLSyntaxErrorException: You have an error in your SQL syntax;```   
특히 JPA와 함께 사용하느라 어느 단계에서 일어난 에러인지 헷갈렸다.   
예외 로그에는 keyword에 대한 이야기 없이 Syntax Error로만 표시되니 주의하자.   
https://dev.mysql.com/doc/refman/5.5/en/keywords.html
