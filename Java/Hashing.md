https://d2.naver.com/helloworld/831311   
https://en.wikipedia.org/wiki/Open_addressing   
https://ict-nroo.tistory.com/76   
https://ratsgo.github.io/data%20structure&algorithm/2017/10/25/hash/   

## HashMap vs HashTable
HashTable은 Java 초창기부터 있던 클래스다.

HashMap은 Java 2에서 추가된 Collection API 중 하나다.

HashMap이 더 나중에 생긴 만큼 기존의 단점을 해결하기 위해 '보조 해시 함수(Additional Hash Function)'가 적용되었다.

이외에도 HashMap은 지속적으로 개선되어오고 있고, HashTable은 그렇지 않기 때문에 현재는 HashMap만 사용해도 무방하다.

 

## 해시충돌(Collision)
해싱은 거의 필연적으로 해시충돌(collision)이 발생한다.

충돌이란 서로 다른 키(key)에 대한 해시값이 같아 겹치는 상황을 의미한다.

 

## Hashing의 과제
해시충돌을 최대한 줄이는 것이 해싱이 해결해야하는 과제다.

하지만, 모든 경우에 해시값이 겹치지 않는 완전한 해시함수를 만든다고 해도 현실적으로는 충돌을 해결할 수 없다.

효율성을 위해서 해시값의 범위를 제한할 수 밖에 없기 때문이다.

따라서 충돌을 줄인다는 것은 다시 말해 해시값 사이에 균등하게 충돌이 일어나도록 만든다는 것과 같다.

또 하나의 과제는 해시충돌이 일어났을 때 성능에 최소한의 영향만 주도록 처리하는 것이다.

해시값이 충돌될 때 그 해결방법에 따라 이후 삽입, 탐색, 삭제 성능이 달라진다.

 

## 해시함수
#### Division Method(나눗셈법)
```
h(k) = k mod m
```

키값을 해시테이블의 크기 m으로 나눈 나머지값을 해시값으로 가진다.

장점 : 간단하고 빠르다.

단점 : 해시테이블의 크기(m)이 2의 제곱수에서 거리가 먼 소수(prime number)여야 성능이 좋다.

#### Multiplication Method
```
h(k) = (kA mod 1) × m
```

키값에 특정 소수값 A(0 < A < 1)를 곱한 후, 소수부분만 남겨 해시테이블의 크기 m을 다시 곱한 다음, 그 정수부분을 해시값으로 가진다.

장점 : 해시테이블의 크기(m)에 제약이 적다. 컴퓨터의 2진수 연산에 적합한 방법이다.

단점 : 나눗셈법보다는 느리다.

#### Universal Hasing
해시함수를 여러 개 두고 그 중 무작위로 선택한 해시함수를 사용하는 기법이다.
 

## 해시테이블 구조
#### Direct-address Table (SUHA (Simple Uniform Hashing Assumption))
키의 개수만큼의 버킷 수를 가진 해시테이블을 만든다.

장점 : 충돌이 일어나지 않는다.

단점 : 현실에서 불가능.

#### Separate Chaining
연결리스트를 이용해 한 버킷에 여러 개의 값을 넣는다.

장점 : 유연하다.

단점 : 메모리 비효율?

#### Open Addressing
충돌이 일어날 경우 다른 버킷에 저장한다.

장점 : 메모리 효율?, 캐싱 효율?

단점 : 
