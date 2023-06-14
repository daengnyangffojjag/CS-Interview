## Java 컴파일 과정

<img width="689" alt="스크린샷 2023-06-14 오후 9 57 54" src="https://github.com/daengnyangffojjag/CS-Interview/assets/68041758/72418cfb-30b0-4954-893d-5b3b2735ef1b">

### 자바 컴파일 순서

1. 개발자가 자바 소스코드(.java)를 작성합니다.
2. 자바 컴파일러(Java Compiler)가 자바 소스파일을 컴파일 >> 자바 바이트코드(.class) 반환
    1. 자바 바이트 코드(.class)파일 
        - 아직 컴퓨터가 읽을 수 없는 자바 가상 머신이 이해할 수 있는 코드
        - 바이트 코드의 각 명령어는 1바이트 크기의 Opcode와 추가 피연산자로 이루어져 있습니다.
    
    <img width="692" alt="스크린샷 2023-06-14 오후 9 59 03" src="https://github.com/daengnyangffojjag/CS-Interview/assets/68041758/8e9fde71-f2e1-4a2e-b37e-a4b4fa23cd23">
    
3. 컴파일된 바이트 코드를 JVM의 클래스로더(Class Loader)에게 전달합니다.
4. 클래스 로더는 동적로딩(Dynamic Loading)을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역(Runtime Data area), 즉 JVM의 메모리에 올립니다.
    - 클래스 로더 세부 동작
        1. 로드 : 클래스 파일을 가져와서 JVM의 메모리에 로드합니다.
        2. 검증 : 자바 언어 명세(Java Language Specification) 및 JVM 명세에 명시된 대로 구성되어 있는지 검사합니다.
        3. 준비 : 클래스가 필요로 하는 메모리를 할당합니다. (필드, 메서드, 인터페이스 등등)
        4. 분석 : 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경합니다.
        5. 초기화 : 클래스 변수들을 적절한 값으로 초기화합니다. (static 필드)
5. 실행엔진(Execution Engine)은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행합니다. 이때, 실행 엔진은 두가지 방식으로 변경합니다.
    1. 인터프리터 : 바이트 코드 명령어를 하나씩 읽어서 해석하고 실행합니다. 하나하나의 실행은 빠르나, 전체적인 실행 속도가 느리다는 단점을 가집니다.
    2. JIT 컴파일러(Just-In-Time Compiler) : 인터프리터의 단점을 보완하기 위해 도입된 방식으로 바이트 코드 전체를 컴파일하여 바이너리 코드로 변경하고 이후에는 해당 메서드를 더이상 인터프리팅 하지 않고, 바이너리 코드로 직접 실행하는 방식입니다. 하나씩 인터프리팅하여 실행하는 것이 아니라 바이트 코드 전체가 컴파일된 바이너리 코드를 실행하는 것이기 때문에 전체적인 실행속도는 인터프리팅 방식보다 빠릅니다.
    - JVM은 기본적으로 인터프리터 방식을 사용
    - 내부적으로 특정 메서드가 얼마나 자주 수행되는지 체크하고 일정 정도를 넘을 때만 JIT 컴파일러 방식을 사용
    - 다만 실행 엔진이 어떻게 동작하는 지는 JVM 명세에 규정되어있지 않기 때문에 JVM을 만들어내는 곳에서는 다양한 방식으로 실행 엔진을 최적화하고 있다고 합니다.

출처

- https://ssocoit.tistory.com/270
- [https://github.com/gyoogle/tech-interview-for-developer/blob/master/Language/[java] 자바 컴파일 과정.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Language/%5Bjava%5D%20%EC%9E%90%EB%B0%94%20%EC%BB%B4%ED%8C%8C%EC%9D%BC%20%EA%B3%BC%EC%A0%95.md)

### 예상질문

1. Java 컴파일 과정에 대해 설명해주세요.

> 배경 & 질문 의도
> 
- CS 에 가까운 질문

> 답변
> 
1. 컴파일러가 변환: 소스코드 -> 자바 바이트 코드(.class)
2. JVM이 변환: 바이트코드 -> 기계어
3. 인터프리터 방식으로 애플리케이션 실행

> 키워드 & 꼬리 질문
> 
- JIT 컴파일러
