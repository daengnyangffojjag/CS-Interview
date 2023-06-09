## 운영체제(Operating System)란

> **사용자가 컴퓨터를 편리하고 효과적으로 사용할 수 있도록 환경을 제공하는 시스템 소프트웨어
하드웨어 관리, 컴퓨터 자원 관리, 응용 프로그램과 하드웨어 간 인터페이스**
*종류 : Windows, Linux, Mac Os, iOS*
> 

![스크린샷 2023-05-31 오후 7.45.44.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/07a09cc8-3fb5-4adb-b603-930d0ceadd03/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-05-31_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.45.44.png)

### 운영체제의 목적

1. 효율성(Efficiency) : **컴퓨터 하드웨어 관리**
    1. CPU, 메모리, 디스크, 키보드, 마우스, 모니터 등의 하드웨어 관리
    2. 이런 하드웨어를 잘 관리해야 컴퓨터 효율적 사용 가능
    3. OS 성능 ↑ → 컴퓨터 성능 ↑
2. Convenience : **사용자에게 편의를 제공**
    1. 운영체제가 없다면 하드웨어 관리를 사용자가 직접
    2. GUI 환경 x >> 컴퓨터 사용 불편

<aside>
💡 운영체제는 컴퓨터의 성능을 높이고(**performance**), 사용자에게 편의성 제공(**Convenience**)을 목적으로 하는 **컴퓨터 하드웨어 관리하는 프로그램**이다.

</aside>

### 운영체제 서비스

1. 프로그램 실행
2. 컴퓨터 자원에 접근
    1. 하드웨어 : I/O device나 다른 하드웨어 리소스
    2. 데이터
3. 에러 탐지와 오류 알림( Error detection and response)
4. Accounting
    1. 컴퓨터 자원이 얼마나 사용되었는지, 통계 기능 지원 ex) window 작업관리자
    2. 성능 모니터링

### 운영체제의 역할

### 1. 프로세스 관리

- 프로세스, 스레드
- 스케줄링
- 동기화
- IPC 통신

### 2. 저장장치 관리

- 메모리 관리
- 가상 메모리
- 파일 시스템

### 3. 네트워킹

- TCP/IP
- 기타 프로토콜

### 4. 사용자 관리

- 계정 관리
- 접근권한 관리

### 5. 디바이스 드라이버

- 순차접근 장치
- 임의접근 장치
- 네트워크 장치

### 운영체제의 위치

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b59dffd7-50e9-4e76-9f86-2328a0caf80e/Untitled.png)

- Application은 특정 OS에 맞춰서 생성
    - 한 어플리케이션은 서로 다른 OS에서 수행 x
    - ex) Windows 프로그램을 그대로 Linux에 옮기면 수행 x
    - but, 하드웨어는 상관 x >> ex) Intel칩 + Window 프로그램 - 메모장, AMD칩 + Windows 메모장 프로그램
- 운영체제는 실제 세상의 정부(Goverment)와 유사
    - 국토, 인력, 예산과같은 자원이 존재하며 이를 효율적으로 사용해야한다.
    - 효율적인 자원 관리를 위해 행정부, 국토부, 교육부, 국방부 등 부서로 나눠 관리한다.
    - 각 부서들은 국민들에게 자원을 요청받고 적절히 배분한다.
- 운영체제가 하는 일은 다음과 같다.
    - 프로세스, 메모리, 하드디스크 등 하드웨어 자원이 존재하고, 이를 효율적으로 사용해야한다.
    - 자원 관리를 위해 프로세스 관리, 메모리 관리, 디스크 관리, 네트워크, 보안 등 기능이 나눠져 있다.
    - 애플리케이션들의 요청에 따라 각 기능들이 수행하여 적절히 자원을 분배한다.

### 예상 질문

1. 운영체제란 무엇인지 설명해 주세요.
- 사용자가 컴퓨터를 편리하고 효과적으로 사용할 수 있도록 환경을 제공하는 시스템 소프트웨어로, 하드웨어를 관리하여 컴퓨터의 성능을 높이고(**performance**), 사용자에게 편의성 제공(**Convenience**)을 목적으로 합니다.
2. 운영체제의 역할에 대해 2가지 말씀해 주세요.
- 프로세스 관리, 저장장치 관리, 네트워킹, 사용자 관리, 디바이스 드라이버
