## Primitive Type & Reference Type

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/67bc83f4-4acc-46b5-8f12-438a02b9e46e/Untitled.png)

### Primitive Type

---

총 8가지의 정수, 실수, 문자 등의 기본형 타입

| 구분 | 타입 | 메모리 크기 | 기본값 | 범위 |
| --- | --- | --- | --- | --- |
| 정수형 | byte | 1byte | 0 | -128 ~ 127 |
|  | short | 2byte | 0 | -32,768 ~ 32,767 |
|  | int(기본) | 4byte | 0 | 2,147,483,648 ~ 2,147,483,647 |
|  | long | 8byte | 0 | -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807 |
| 실수형 | short | 4byte | 0.0F | 약 (3.410^-38) ~ (3.410^38) |
|  | double(기본) | 8byte | 0.0 | 약 (1.7*10^-308) ~ (1.7*10^308) |
| 문자형(정수형) | char | 2byte | '\u0000’ |  0 ~ 65535 (unsigned) |
| 논리형 | boolean | 1byte | false | true, false |

### 특징

- **기본값**이 있기 때문에 **Null값이 존재하지 않음**
- **스택(Stack) 메모리**에 **실제값**을 저장
- Primitive Type에 Null을 넣고 싶다면 **래퍼클래스**를 이용해야함
- 래퍼클래스로 감싸서 객체로 사용할 수 있음
- 기본형은 반드시 사용하기 전에 선언되어야 함
- 메모리에 고정된 크기를 가지고 저장 가능한 값의 범위가 있음
- 객체를 생성하고 관리할 필요가 없기 때문에 처리 속도가 빠름
- Primitive Type은 자체적으로 기능이나 메소드가 없음

### 장단점

**장점**

- 단순하고 간편하게 변수 선언과 사용이 가능
- 고정된 크기로 메모리 사용이 효율적이고 메모리 할당과 해제에 오버헤드가 없음
- 값에 직접 접근하므로 연산 속도가 빠름 (객체를 생성하고 관리하는 과정X)

**단점**

- 객체로 사용할 수 없음
- 객체 관련 메소드나 기능 사용 불가
- Null값 표현 불가

### Reference Type

---

자바에서 Primitive Type을 제외한 문자열, 배열, 클래스, 인터페이스 등 모든 타입

- new 키워드를 통해 생성된 **객체의 주소를 참조**하는 타입

| 타입 | 기본값 |
| --- | --- |
| 배열 | Null |
| Enum | Null |
| 클래스 | Null |
| 인터페이스 | Null |

### 특징

- **객체의 메모리 주소**를 저장
- 힙 영역에 객체의 주소가 저장되고 변수 자체는 스택 영역에 저장

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d7bb099-0391-486d-89b5-91ff7864b75b/Untitled.png)

- 실행 시점에 동적으로 메모리를 할당받음
- Null 값으로 초기화 할 수 있고, Null은 객체를 가리키지 않음을 의미

### 장단점

**장점**

- 객체를 다루기 때문에 상속, 다형성, 메소드 등의 기능 활용 가능
- 객체의 생성과 소멸을 통해 메모리를 유연하게 관리
- Null값 표현 가능

**단점**

- 객체의 주소를 저장하기 위해 추가적인 메모리를 사용하므로 Primitive Type보다 더 많은 메모리 사용
- 객체의 생성과 소멸에 오버헤드 발생 가능
- 객체에 대한 참조를 통해 접근하므로 연산이 느릴 수 있음

### 예상질문

1. 프리미티브 타입과 레퍼런스 타입의 차이점을 설명해주세요
2. 기본형과 참조형 타입을 간단하게 설명해주세요

[[java] 프리미티브 타입과 레퍼런스 타입](https://league-cat.tistory.com/408)

[https://github.com/GimunLee/tech-refrigerator/blob/master/Language/JAVA/Primitive type %26 Reference type.md#primitive-type--reference-type](https://github.com/GimunLee/tech-refrigerator/blob/master/Language/JAVA/Primitive%20type%20%26%20Reference%20type.md#primitive-type--reference-type)
