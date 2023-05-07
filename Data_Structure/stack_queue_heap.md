# Stack & Queue & Heap
## 스택(Stack)

- 후입선출(LIFO, Last In First Out), 박스 쌓기
- 사용 예시 :  웹 브라우저의 방문기록(뒤로가기), 실행 취소(undo), 컴퓨터 구조에서 stack memory

![Untitled](https://user-images.githubusercontent.com/68041758/236683078-f226712a-ad3b-47ed-9e30-912ae150b4a5.png)

### Java에서의 Stack

- `import java.util.Stack;` 로 사용

### 주요 메서드

1. `push()` : 삽입
    1. 스택에 새로운 원소 삽입
2.  `pop()` : 삭제
    1. 최상층에 위치한 데이터 읽어오고, 스택에서 제거
3. `clear()` : 모두 삭제
    1. 스택의 값 모두 삭제
4. `peek()` : 최상층 데이터 읽기
    1. 스택의 가장 위에 있는 값 읽기
5. `isEmpty()`, `isFull()`
    1. 각각 스택이 비어있는지, 가득찼는지를 boolean 형태로 리턴하는 메서드

```java
Stack<Integer> st = new Stack<Integer>(); // 스택의 생성

// push() 메소드를 이용한 요소의 저장
st.push(4);
st.push(2);
st.push(3);
st.push(1);

// peek() 메소드를 이용한 요소의 반환
System.out.println(st.peek());
System.out.println(st);

// pop() 메소드를 이용한 요소의 반환 및 제거
System.out.println(st.pop());
System.out.println(st);
 
// search() 메소드를 이용한 요소의 위치 검색
System.out.println(st.search(4));
System.out.println(st.search(3));

[실행결과]
1
[4, 2, 3, 1]
1
[4, 2, 3]
3
1
```

## 큐(Queue)

- 선입선출(FIFO, First In First Out), 대기줄
- 큐의 사용 예시 : 프린터의 인쇄 대기, 콜센터 고객 대기 시간, 컴퓨터 버퍼 등

![queue](https://user-images.githubusercontent.com/68041758/236683114-351ac39c-9696-429c-b90e-d815a5e8c362.png)
- 큐 용어

(1) Enqueue : 큐 맨 뒤에 데이터 추가

(2) Dequeue : 큐 맨 앞쪽의 데이터 삭제

### Java에서의 Queue

- 클래스가 아닌 인터페이스 형태로 제공
    - Queue
- 보통 LinkedList로 생성해 Queue로 선언하여 사용

### 주요 메서드

1. `add()` , `offer()`: 추가
    1. 큐에 새로운 원소 삽입
2.  `remove()` , `poll()`: 삭제
    1. 큐의 첫번째 값 삭제
    2. `poll()` : 큐가 비어있으면 Null 반환
3. `clear()` : 모두 삭제
    1. 큐의 값 모두 삭제
4. `peek()` : 큐의 첫번째 데이터 읽기
    1. 큐의 제일 앞에 있는 값 읽기
5. `isEmpty()`, `isFull()`
    1. 각각 큐이 비어있는지, 가득찼는지를 boolean 형태로 리턴하는 메서드

```java
import java.util.LinkedList; //import
import java.util.Queue; //import

Queue<String> qu = new LinkedList<>(); //int형 queue 선언, linkedlist 이용

// add() 메소드를 이용한 요소의 저장
qu.add("넷");
qu.add("둘");
qu.add("셋");
qu.add("하나");

// peek() 메소드를 이용한 요소의 반환
System.out.println(qu.peek());
System.out.println(qu);

// poll() 메소드를 이용한 요소의 반환 및 제거
System.out.println(qu.poll());
System.out.println(qu);

// remove() 메소드를 이용한 요소의 제거
qu.remove("하나");
System.out.println(qu);

[실행결과]
넷
[넷, 둘, 셋, 하나]
넷
[둘, 셋, 하나]
[둘, 셋]
```

### Deque

![deque](https://user-images.githubusercontent.com/68041758/236683145-018ae2bc-9b4e-4369-979b-13b15b56a447.png)

- Double ended Queue라는 의미로, 양쪽에서 삽입삭제가 가능한 자료구조
    - head에서도 접근가능하며 tail에서도 접근 가능한 양방향 큐
- Queue를 상속하고 있는 하위 interface
- 예시 : 카드 덱(deck)

**<Queue/Deque Interface를 구현하는 클래스>**

**1. LinkedList**

**2. ArrayDeque**

**3. PriorityQueue**

## Priority Queue

- 우선순위 큐
- 데이터 우선순위에 기반해 우선순위가 높은 데이터가 먼저 나오는 원리
- 최소 힙 구조로 default로 낮은 숫자가 높은 우선순위
- 시간복잡도 : O(nlogn)

### Java에서의 Priority Queue

- `java.util.PriorityQueue`를 import
- 기본이 우선순위가 낮은 숫자 순이므로 우선순위가 높은 숫자 순으로 하기 위해선 `Collections.reverseOrder()` 사용

```java
import java.util.PriorityQueue; //import

//int형 priorityQueue 선언 (우선순위가 낮은 숫자 순)
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();
//int형 priorityQueue 선언 (우선순위가 높은 숫자 순)
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(Collections.reverseOrder());
```

## Heap

![heap](https://user-images.githubusercontent.com/68041758/236683173-73904468-80fa-4728-91cc-7e3c405e27b1.png)

- 완전 이진 트리의 일종으로 우선순위 큐를 위해 만들어진 자료구조
- 최댓값, 최솟값을 빠르게 찾아내도록 만들어진 자료구조
- 반정렬 상태(느슨한 정렬 상태)
    - 큰 값이 상위 레벨에 있고 작은 값이 하위 레벨에 있다는 정도
    - 즉, 부모 노드의 키 값이 자식 노드의 키 값보다 항상 큰(작은) 이진트리
        - 부모노드와 자식노드간의 관계만 신경쓰면 되기 때문에 형제 간 우선순위는 고려 X
        - ❓왜 형제 간 대소비교 필요 없는지?
            - 우선순위가 높은 순서대로 뽑는 것이 포인트다. 즉, 원소를 넣을 때도 우선순위가 높은 순서대로 나올 수 있도록 유지가 되어야하고 뽑을 때 또한 우선순위가 높은 순서 차례대로 나오기만 하면 된다
- 중복된 값 허용
    - 이진 탐색 트리는 중복값 허용 X)

### 힙의 활용 예시

- 시뮬레이션 시스템
- 네트워크 트래픽 제어
- 운영체제에서의 작업 스케줄링
- 수치 해석적인 계산

### Heap의 종류

- 최대 힙(max heap)
    - 부모 노드 키 값 ≥ 자식 노드 키 값
- 최소 힙(min heap)
    - 부모 노드 키 값 ≤ 자식 노드 키 값

![min_max_heap](https://user-images.githubusercontent.com/68041758/236683210-34107316-1aeb-4b45-8cb6-6872f9096ecd.png)

## Reference

- https://github.com/ksundong/backend-interview-question
- [https://github.com/Fancy96/2023-CS-Study/blob/main/Interview/algorithm_expected_question.md](https://github.com/Fancy96/2023-CS-Study/blob/main/Interview/algorithm_expected_question.md)
- [https://st-lab.tistory.com/142](https://st-lab.tistory.com/142)
- [http://www.tcpschool.com/java/java_collectionFramework_stackQueue](http://www.tcpschool.com/java/java_collectionFramework_stackQueue)

## 예상 질문

1. Stack, Queue에 대해 설명해주세요
    
    Stack
    
    - 스택은 선형 자료구조의 일종으로 마지막에 저장한 데이터를 가장 먼저꺼내는 LIFO 방식의 자료구조
    - 스택의 사용 예시로는 웹 브라우저의 방문기록(뒤로가기), 실행 취소(undo) 등이 있다.
    
    Queue
    
    - 큐는 선형 자료구조의 일종으로 처음에 저장한 데이터를 가장 먼저 꺼내는 FIFO 방식의 자료구조
    - 큐의 사용 예시로는 프린터의 인쇄 대기, 콜센터 고객 대기 시간 등
2. Heap, Priority Queue에 대해 설명해주세요.
3. 스택으로 큐를 구현할 수 있나요? 그 반대는?
    - 스택 2개를 사용해 구현 가능
4. 스택과 큐의 특성을 모두 사용해야한다면?
    - Deque 자료구조를 사용 가능
