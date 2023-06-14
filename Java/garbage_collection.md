### Garbage Collection이란?
자바의 메모리 관리 방법 중의 하나로 JVM의 Heap 영역에서 동적으로 할당했던 메모리 영역 중 필요 없게 된 메모리 영역을 주기적으로 삭제하는 프로세스를 말합니다.

C나 C++에서는 이러한 가비지 컬렉션이 없어 프로그래머가 수동으로 메모리 할당과 해제를 일일이 해줘야함


Java는 JVM에 탑재되어 있는 가비지 컬렉터가 메모리 관리를 대행해주기 때문에 개발자 입장에서 메모리 관리, 메모리 누수(Memory Leak) 문제에서 대해 완벽하게 관리하지 않아도 되어 오롯이 개발에만 집중할 수 있다는 장점이 있습니다.

### 누가 쓰레기인가?
Reachable : 누군가 사용하고 있는 것<br/>
Unreachable : 아무도 사용하지 않는 것
![](https://velog.velcdn.com/images/tkdtkd97/post/8ac69948-1643-413a-bb7a-5445c9f5e01d/image.png)
> 가비지 컬렉션(Garbage Collection)은 특정 객체가 가비지인지 아닌지 판단하기 위해서 도달성, 도달능력(Reachability) 이라는 개념을 적용한다. 객체에 유효한 레퍼런스가 없다면 Unreachable로 구분해버리고 수거해버린다. 레퍼런스가 있다면 Reachable로 구분된다.


### Reference Counting & Mark and Sweep
GC가 어떻게 Reachable과 Unreachable을 판단할 수 있을까?
가장 간단해 보이는 알고리즘 두개를 알아보자
#### Reference Counting
![](https://velog.velcdn.com/images/tkdtkd97/post/0e3b04b2-c733-4e17-ac54-f1da0bdc7961/image.png)
초기의 GC 알고리즘으로 Object마다 Reference Count를 관리하여 Reference Count가 0이 되면 GC를 수행한다.

Reference Count의 관리 비용이 크다.
또한 순환 참조 구조에서 Memory Leak이 발생할 가능성도 크다.

#### Mark and Sweep
![] (https://miro.medium.com/v2/resize:fit:660/0*-dB_3FTm5N-5kjN6.gif)

Mark: 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
Sweep: Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

Mark and Sweep을 하기전에 Stop The World를 통해 모든 작업을 중단시키면, GC는 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각이 어떤 객체를 참고하고 있는지를 탐색하게 된다.
> 대개의 경우 GC 튜닝이란 이 stop-the-world 시간을 줄이는 것이다.

### GC HEAP 구조
![](https://velog.velcdn.com/images/tkdtkd97/post/032f5500-4941-489d-9e5d-4d42ddaedef4/image.png)
- young - 비교적 젊은 reference가 살아있는 곳
    - eden - young영역중에서도 특히 방금 막 생성된 녀석들이 있는 곳
    - survivor - 영역이 두개 존재하는데 eden에서 생존된 녀석들이 당분간 생존해 있는 곳

- old - 특정 횟수 이상을 살아남은 reference가 살아있는 곳

- permanent - Method Area의 메타정보가 기록된 곳

### Minor GC / Major GC
일단 기본적으로 Mark-Sweap을 하는건 동일하다.

#### Minor GC
![](https://velog.velcdn.com/images/tkdtkd97/post/66dab2c1-2510-4d5e-b470-85f26b851a38/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/035e4b85-cc13-41f0-b4b5-ae8f36e7b239/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/d460e456-3fbe-44b4-9048-f69cb20424f0/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/6f8b7e82-b39e-4350-a3c7-5bd844d05c46/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/628fa22d-a382-40d0-9b58-e66a4a609d2f/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/55916593-d31d-438f-af89-f65eafe754e0/image.png)
살아남은 객체들은 age값이 1씩 증가한다. 얼마나 오래있었는지 나타냄

![](https://velog.velcdn.com/images/tkdtkd97/post/c4cededc-d959-4762-a798-9ba67e916c7c/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/9325dca3-e1cc-4140-ab01-5cfaecc77ef0/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/b7963ca1-fbe0-48e4-b9ec-7d336b799878/image.png)
이 과정을 반복
![](https://velog.velcdn.com/images/tkdtkd97/post/95fea56e-49f0-453b-8616-397e6613d690/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/141274cd-a723-47c9-a3ab-94aa03e27a6e/image.png)
Young Generation에서 Old Generation 으로 이동된다. 이를 promotion 이라한다.

- Minor GC에서 발동되는 알고리즘을 Stop And Copy알고리즘이라고 부른다.
- Young Generation 영역은 짧게 살아남는 메모리들이 존재하는 공간이다.
- Young Generation의 공간은 Old Generation에 비해 상대적으로 작기 때문에 메모리 상의 객체를 찾아 제거하는데 적은 시간이 걸린다. (작은 공간에서 데이터를 찾으니까)


#### Major GC
![](https://velog.velcdn.com/images/tkdtkd97/post/5146dec2-140a-42cc-bb41-13d746127663/image.png)

- Major GC는 Old 영역은 데이터가 가득 차면 GC를 실행하는 단순한 방식이다.

- 하지만 Old Generation은 Young Generation에 비해 상대적으로 큰 공간을 가지고 있어, 이 공간에서 메모리 상의 객체 제거에 많은 시간이 걸리게 된다.

- 바로 여기서 Stop-The-World 문제가 발생하게 된다.

- Major GC가 일어나면 Thread가 멈추고 Mark and Sweep 작업을 해야 해서 CPU에 부하를 주기 때문에 멈추거나 버벅이는 현상이 일어나기 때문이다.

- 따라서 자바 개발진들은 끊임 없이 가비지 컬렉션 알고리즘을 발전 시켜왔다.

### GC 종류
https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC

### 참고
https://velog.io/@recordsbeat/Garbage-Collector-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B8%B0
https://medium.com/@leeyh0216/reference-counting%EA%B3%BC-mark-and-sweep-2d046f73da4f
https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC

### 면접 예상 질문
> 자바에서 가비지 컬렉션이란?

자바의 메모리 관리 방법 중의 하나로 JVM의 Heap 영역에서 동적으로 할당했던 메모리 영역 중 필요 없게 된 메모리 영역을 주기적으로 삭제하는 프로세스를 말합니다.

> 가비지 컬렉션의 동작 방식

Young 영역과 Old 영역은 서로 다른 메모리 구조로 되어 있기 때문에 세부적인 동작 방식은 다르지만 가비지 컬렉션이 실행된다고 하면 공통적으로 2가지 단계를 따른다.

1. Stop The World
2. Mark and Sweep

- Stop The World

가비지 컬렉션을 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다.

GC가 실행될 때는 GC를 실행하는 스레드를 제외한 모든 스레드들의 작업이 중단되고, GC가 완료되면 작업이 재개된다.

모든 스레드의 작업이 중단되면 애플리케이션이 멈추기 때문에 성능 문제가 있다.

- Mark and Sweep

Mark: 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
Sweep: Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

Stop The World를 통해 모든 작업을 중단시키면, GC는 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 어떤 객체를 참고하고 있는지 탐색한다.

그리고 사용되고 있는 메모리를 식별하는데, 이러한 과정을 Mark라고 한다.

이후에 Mark가 되지 않은 객체들을 메모리에서 제거하는데, 이 과정을 Sweep이라고 한다.

> Minor GC가 일어나는 과정

1. 새롭게 생성된 객체는 Eden 영역에 할당된다.

2. 객체가 계속 새로 생성되어 Eden 영역이 꽉 차게 되면 Minor GC가 실행된다.
   참조되지 않는 객체의 메모리 할당을 해제하고 참조되고 있는 객체는 Survivor 영역으로 이동한다.

3. 1~2번 과정이 반복되다가 Survivor 영역이 꽉차게 되면 Survivor 영역의 살아남은 객체를 다른 Survivor 영역으로 이동시킨다.
   (1개의 Survivor 영역은 반드시 빈 상태여야 한다. 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 두 영역 모두 사용량이 0이라면 정상적인 상황이 아니다.)

4. 이러한 과정을 반복해서 계속 살아남은 객체는 Old 영역으로 이동(Promotion)한다.

> Major GC가 일어나는 과정

1. Promotion 작업이 계속되다보면 Old Generation이 가득 차게 되면서 Major GC가 발생하게 된다.

2. 참조되지 않는 객체는 메모리 할당을 해제한다.

> Minor GC와 Major GC의 차이점

Young Generation은 Old Generation보다 크기가 작기 때문에 Minor GC가 일어나는 속도가 빠르다. (대략 0.5초에서 1초) 그렇기 때문에 애플리케이션에 큰 영향을 주지 않는다.

하지만 Old Generation에서의 Major GC는 Minor GC에 비해 10배 이상의 시간이 소요된다.
