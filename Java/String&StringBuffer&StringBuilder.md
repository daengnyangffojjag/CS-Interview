## String & StringBuffer & StringBuilder

<img width="499" alt="스크린샷 2023-06-14 오후 10 02 59" src="https://github.com/daengnyangffojjag/CS-Interview/assets/68041758/8a8d874b-6710-41bf-91d7-7d9429f2cf7f">

### 1. String

<img width="485" alt="스크린샷 2023-06-14 오후 10 03 26" src="https://github.com/daengnyangffojjag/CS-Interview/assets/68041758/8c90578d-a7c9-4bdd-9e31-e35255095942">

> String 클래스는 문자열 연산이 적고, 조회가 많은 멀티쓰레드 환경에서 좋음
> 
- new 연산을 통해 생성된 인스턴스 메모리 공간 변화 x (Immutable)
- Garbage Collector로 제거되어야 함.
- 객체가 불변하므로, Multi Thread에서 동기화를 신경안써도 됨.
- 문자열 연산 시 새로운 객체 만드는 Overhead 발생
    - 문자**열 추가,수정,삭제 등의 연산이 빈번하게 발생**하는 알고리즘에 String 클래스를 사용하면 **힙 메모리(Heap)에 많은 임시 가비지(Garbage)가 생성**되어 힙메모리가 부족으로 어플리케이션 성능에 치명적인 영향
    
    ```java
    String s = "hello";
    s += ' world'; //hello world
    ```
    

### 2. StringBuffer, StringBuilder

> StringBuilder는 **단일 스레드 환경** 과 **문자열의 추가, 수정, 삭제 등이 빈번히 발생하는 경우
StringBuffer는 멀티 스레드 환경** 과 **문자열의 추가, 수정, 삭제 등이 빈번히 발생하는 경우**
> 

<img width="601" alt="스크린샷 2023-06-14 오후 10 03 54" src="https://github.com/daengnyangffojjag/CS-Interview/assets/68041758/3aa472e4-6350-462a-9fef-0257ac20624e">

- 공통점
    - new 연산으로 클래스를 한 번만 만듬 (Mutable)
    - 문자열 연산시 새로 객체를 만들지 않고, 크기를 변경시킴
        - 동일 객체 내에서 문자열 변경
    - StringBuffer와 StringBuilder 클래스의 메서드가 동일함.
- 차이점
    - StringBuffer는 Thread-Safe함
        - 메서드에서 `synchronized` 키워드 사용
        - `synchronized` 키워드는 여러개의 스레드가 한 개의 자원에 접근할려고 할 때, 현재 데이터를 사용하고 있는 스레드를 제외하고 **나머지 스레드들이 데이터에 접근할 수 없도록 막는 역할을 수행**
        - 예시
        
        ```java
        예를 들어 멀티스레드 환경에서 A 스레드와 B스레드 모두 같은 StringBuffer 클래스 객체 sb의 append() 메서드를 사용하려고 하면, 다음과 같은 절차를 수행하게 된다.
        
        A 스레드 : sb의 append() 동기화 블록에 접근 및 실행
        B 스레드 : A 스레드 sb 의 append() 동기화 블록에 들어가지 못하고 block 상태가 됨.
        A 스레드 : sb의 append() 동기화 블록에서 탈출
        B 스레드 : block 에서 running 상태가 되며 sb 의 append() 동기화 블록에 접근 및 실행.
        ```
        
    - StringBuilder는 Thread-safe하지 않음 (불가능)

### 예상질문

1. StringBuilder와 StringBuffer의 차이는?

> 배경 & 질문 의도
> 
- mutation(가변), immutation(불변) 이해
- 불변 객체인 String 의 연산에서 오는 퍼포먼스 이슈 이해
- String
    - immutation
    - String 문자열을 연산하는 과정에서 불변 객체의 반복 생성으로 퍼포먼스가 낮아짐.

> 답변
> 
- 같은점
    - mutation
    - append() 등의 api 지원
- 차이점
    - StringBuilder 는 동기화를 지원하지 않아 싱글 스레드에서 속도가 빠릅니다.
    - StringBuffer 는 멀티 스레드 환경에서의 동기화를 지원하지만 이런 구현은 로직을 의심해야 합니다.

> 키워드 & 꼬리 질문
> 
- [실무에서의 String 연산](https://hyune-c.tistory.com/entry/String-%EC%9D%84-%EC%9E%98-%EC%8D%A8%EB%B3%B4%EC%9E%90)
