### 프로세스 주소 공간

> 운영체제에서 실행 중인 **각 프로세스에 할당된 메모리 공간**
> 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7a5f65ba-5cef-486d-bd7f-68cbc510202d/Untitled.png)

프로그램이 CPU에 의해 실행 → 프로세스 생성 → 메모리에 저장

→ **운영체제가 프로세스 주소 공간을** 메모리에 할당

### 프로세스 주소 공간이 필요한 이유

1. **메모리**
- 운영체제는 각 프로세스의 메모리에서 사용할 영역을 구분해야 함
    - 각 프로세스가 서로의 공간에 영향을 주지 않고 독립적
- 다중 프로세스를 동시에 실행하기 위함
- 한정된 메모리를 절약
1. 보호 및 격리
- 각 프로세스는 자신만의 주소 공간을 가짐
- 다른 프로세스의 주소 공간에 접근 불가

### 프로세스 주소 공간 구성

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f9c4cae9-71be-4cb4-a4b1-23ffb30f4d6f/Untitled.png)

### 1. Stack

> 메소드 호출과 관련된 데이터를 저장하는 영역
> 
- 지역변수, 매개변수, 리턴 값 등이 저장됨
- 원시타입의 데이터가 값과 함께 할당
- 힙 영역에 생성된 Object 타입의 데이터 참조값이 할당됨
- 함수의 호출과 함께 할당되고, 호출이 완료되면 소멸
- 함수가 호출될 때 **스택 프레임**이 생성되고 관련 정보(지역변수, 매개변수)가 저장됨
- 함수가 호출이 완료되면 **스택 프레임**이 제거
- 메모리의 상위 주소에서 하위 주소 방향으로 할당됨
- 컴파일 할 때 크기가 결정되기 때문에 무한히 할당할 수 없음
    - 지역 변수나 매개변수에 과도한 데이터를 저장했을 때
    - 재귀 호출이 너무 깊게 이루어 질 때

### 2. Data

> 프로그램이 구동되는 동안 항상 접근 가능한 변수가 저장되는 영역
> 
- 전역 변수, static 변수의 초기값이 저장되는 영역
- 프로그램이 시작될 때 할당됨
- 프로그램이 종료되면 삭제
- 초기화 되지 않은 변수는 **BSS(Block Stated Symbol)영역**에 저장됨

<aside>
💡 Data 영역과 BSS 영역을 구분하는 이유?

</aside>

초기화되지 않은 변수는 메모리에 공간만 할당되고 실행 시에 초기화된다. 이로 인해 프로그램의 크기를 효율적으로 관리할 수 있게 해준다

### 3. Code

> 프로세스가 실행될 때 필요한 명령어(코드)가 저장되는 영역
> 
- 실행 파일의 바이너리 코드가 저장됨
- 읽기 전용(Read-Only)
- 프로그램의 명령어들이 메모리에 로드되는 곳
- 프로세스가 실행 중일 땐 수정되지 않음

### 구역을 나눈 이유

**Code 영역을 따로 둔 이유**

---

프로그램의 실행 코드, 명령어 들은 프로그램이 실행되고 나서 바뀔 일이 없기 때문에 Code 영역은 읽기 전용(Read-Only)이다. 그리고 여러 프로세스 동일한 코드를 공유할 수 있어 메모리 사용을 효율적으로 관리 할 수 있다

**Stack 영역과 Data 영역을 나눈 이유**

---

전역 변수는 어떤 함수에도 접근할 수 있기 때문에 Data 영역으로 따로 관리해 주고 함수의 호출 면에서 마지막에 호출된 함수를 먼저 실행할 수 있도록 스택의 후입 선출 구조를 활용할 수 있다.

### 예상질문

1. 프로세스 주소 공간에 대해서 설명해주세요

프로세스 주소 공간은 프로세스가 실행되는 동안 사용하는 메모리 공간 입니다. 각 프로세스 마다 독립적으로 할당되며 다른 프로세스의 주소 공간에 접근이 불가능합니다. 이를 통해 메모리 관리나 프로세스의 보호 및 격리가 가능합니다.

1. 프로세스 주소 공간은 어떤 것으로 구성되어 있는지 설명해주세요

프로세스 주소 공간은 코드 영역, 데이터 영역, 스택 영역으로 이루어져 있습니다. 코드 영역은 프로그램 코드가 저장되는 영역이고 읽기 전용 영역입니다. 데이터 영역은 전역 변수와 정적 변수가 저장되고 프로그램이 시작될 때 할당되고 종료되면 삭제됩니다. 스택 영역은 메소드의 호출과 관련된 데이터가 저장되는 영역입니다.

1. Code 영역을 따로 둔 이유가 있을까요? - 본문
2. Stack 영역과 Data 영역을 나눈 이유가 있을까요? - 본문

[[운영체제] 프로세스 주소 공간](https://yaelimeee.tistory.com/47)

[프로세스(Process)의 주소 공간(Address Space)](https://whereisusb.tistory.com/10)