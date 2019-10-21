# Garbage Collection   

https://d2.naver.com/helloworld/1329   

### stop-the-world!   

GC 실행 중에는 애플리케이션이 멈춘다. GC를 실행하는 쓰레드를 제외한 나머지 모든 쓰레드가 작업을 멈춘다.   

어떤 GC 알고리즘이라도 stop-the-world는 발생한다. GC 튜닝은 대게 이 stop-the-world 시간을 줄이는 것이다.   



### 메모리를 명시적으로 해제할 때?   

해당 객체를 null로 지정하기   

System.gc() 호출하기 -> 절대 하지 말 것. 성능저하!   



### 가비지 컬렉터의 두 가지 가설 - Weak Generational Hypothesis   

1. 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.   

2. 오래된 객체에서 젊은 객체로의 참조는 아주 드물게 존재한다.   


## Young Generation / Old Generation   

HotSpot VM에서는 위 가설에 따라 젊은 객체와 오래된 객체의 물리적 공간을 나누었다.   

Young Generation : 새롭게 생성된 객체의 대부분이 위치한다. 여기서 일어나는 GC는 Minor GC.   

Old Generation : Young 영역에서 살아남은 객체가 여기로 복사된다. 대부분 Young 영역보다 크게 할당하기 때문에 GC는 적게 발생한다. 여기서 일어나는 GC는 Major GC(혹은 Full GC).   

Permanent Generation (Method Area) : 클래스 정보나 intern된 문자열 정보 등을 저장하는 곳. 여기서 일어나는 GC도 Major GC.   

=> Java 8부터 Permgen을 Metaspace가 대신하게 되었다. (Metaspace는 메모리 할당량을 유동적으로 조절한다.)   

https://www.baeldung.com/java-permgen-metaspace   


### Old 영역에 있는 객체가 Young 영역의 객체를 참조할 때?   
Old 영역에는 512바이트 짜리 Card Table이 존재한다.   

Old 영역의 객체가 Young 영역의 객체를 참조할 때마다 이 곳에 표시한다.   

Card Table은 Minor GC가 일어날 때 Young 영역의 객체가 참조되고 있는지 확인하는 용도로 사용된다.   

카드 마킹 시 Write Barrier를 사용하기 때문에 오버헤드가 발생하지만, GC 시간을 줄이기 위한 기회비용이라고 본다.   



Write Barrier : 메모리에 쓰기 실행을 시도한 순서와 실제 메모리에 써지는 순서가 같도록 보장하는 것.   



https://en.wikipedia.org/wiki/Write_barrier   

http://psy-lob-saw.blogspot.com/2014/10/the-jvm-write-barrier-card-marking.html   
