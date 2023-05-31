> 📌 **Inter Process Communication**
프로세스 간 소통 방법

프로세스는 독립적 (independent)이거나 다른 프로세스와 협력(cooperating)한다.
독립적인 프로세스는 동시에 실행 중인 다른 프로세스에 영향을 주지 않지만, 협력이 필요한 프로세스의 경우 다른 프로세스에 영향을 주거나 받을 수 있다.

### ○ IPC가 필요한 이유
#### 1. 정보 공유
여러 사용자가 동일한 정보에 관심을 가지면, 이 정보에 동시에 접근할 수 있는 환경을 제공해야 한다.

#### 2. 계산 속도 증가
특정 작업 (연산)을 빠르게 실행하기 위해서는 작업은 작은 단위로 나누어서 병렬로 실행할 수 있다.

#### 3. 모듈성
시스템 기능을 프로세스 단위 또는 스레드 단위로 나눈 모듈 방식으로 시스템을 구성할 수 있다.

#### 4. 편리성
개별 사용자도 동시에 많은 작업을 수행할 수 있다. (예, 편집, 인쇄, 컴파일 등)

> 그러나 일반적으로 각 프로세스에 할당된 메모리 공간은 고유하기 때문에 직접 다른 프로세스가 접근할 수 없다. 이를 해결하기 위한 것이 Inter Process Communication이다.


IPC에는 전통적으로 두 가지 모델이 있다.
1. Shared memory
2. Message Passing
![](https://velog.velcdn.com/images/coastby/post/4969e8ec-4e86-4f1d-a5f6-205e2aa66668/image.png)

## 📌 Shared Memory model
협력 프로세스 간 공유되는 메모리를 생성 후 이를 이용하여 정보를 교환한다. 데이터의 형식과 위치는 프로세스들에 의해 결정되며 운영체제 소관은 아니다 (커널의 도움이 거의 필요 없다). 주로 전역 변수, 공유 변수, 공유 파일을 통해 통신을 이룬다. 
성능은 좋지만 프로세스간의 동기화 문제가 발생하여 어플리케이션에서 직접 동기화를 해야한다. (충돌 가능성이 있다.)

### ○ 방법
- Shared Memory model에는 `producer 프로세스`와 `consumer 프로세스`가 있다.
- 공유 메모리(버퍼)를 통해 동시에 (concurrently) 통신한다.
- 생산자가 item을 생산하고 buffer에 저장하면, 소비자는 buffer의 item을 소비한다.
- 가용한 항목이 있을 때만 소비가 이루어지도록, 생산자/소비자 간의 프로세스는 동기화되어야 한다.

---

## 📌 Messaging Passing Model
OS가 프로세스 간 통신 방법을 제공하고, 메세지를 대리 전달해준다. OS가 동기화를 해주기 때문에 안전하고 동기화 문제가 없으나, 시스템 콜을 사용하기 때문에 성능이 떨어진다.

### ○ 분류

#### Direct vs Indirect
- Direct :
  프로세스들은 서로의 이름을 명시한다. 한 쌍에는 하나의 링크만 존재하고, 1대 1통신에 사용된다.
  - send(P, message): P로 메시지를 보내라
  - receive(Q, message): Q로부터 메시지를 받아라.

- Indirect:
  메세지들이 mailbox(port)를 이용하여 send와 receive가 이루어진다. 각 mailbox는 고유의 id를 가진다.
  - send(A, message) : mailbox A 로 메시지를 보내라.
  - receive(A, message): mailbox A로부터  메시지를 받아라. 
  
  따라서 여러 프로세스가 mailbox를 공유할 수 있다.

#### Synchronization (동기화) / Blocking
- Blocking send : 송신자는 수신자가 메시지를 받을 때까지 blocked 되는 것
- Non-blocking send : 송신자는 메시지를 보내고 자기 할 일 하는 것
- Blocking receive : 수신자는 메시지가 모두 다 받을 때까지 blocked 되는 것
- Non-blocking receive : 수신자가 메시지가 왔는지 수시로 검사하는 것 (the receiver retrieves either a valid message or a null message )
 
# IPC 종류

## 📌 Pipe
<img src="https://velog.velcdn.com/images/coastby/post/e21d81dc-65c2-4e1d-b5cf-0593511c1d0a/image.png" width="50%">

### ○ 익명 Pipe
Pipe는 단방향 (unidirectional) 통신 채널이다. 한 쪽은 보내기만하며, 반대쪽은 받기만 한다. 
이 때 pipe는 오로지 부모 자식간에만 생성 가능하다. 양방향 통신을 위해서는 pipe가 2개 필요하다. 대신 read()와 write()가 기본적으로 block 모드로 작동한다. (프로세스가 read 대기중이라면 write를 할 수 없다.)

### ○ Named Pipe (FIFO)
이름을 가진 pipe를 통해서 프로세스들 간의 양방향 통신을 지원하다. 기본적으로는 단방향을 많이 사용한다.
서로 다른 프로세스들이 pipe의 이름만 알면 통신 가능하다.

## 📌 Message Queues
메모리를 사용한 pipe이며, 구조체를 기반으로 통신(stream X)한다. 
메세지 타입(msgtype)에 따라 구조체(데이터?) 종류를 다르게 받을 수 있으며, 여러 프로세스가 동시에 데이터를 다룰 수 있다. 
커널에서 제공하기 때문에 메모리 제한이 있을 수 있다.
<img src="https://velog.velcdn.com/images/coastby/post/72292f52-9ae8-47c6-8e4a-f11427e69901/image.png" width="50%">

## 📌 Shared Memory
위 참고

## 📌 Memory Map
공유 메모리처럼 메모리를 공유하지만, 메모리 맵은 열린 파일을 메모리에 매핑시켜서 공유하는 방식이다. (파일 + 메모리)
주로 파일로 대용량 데이터를 공유해야할 때 사용한다.

## 📌 Client - Server System
### ○ Socket
네트워크 소켓 통신을 이용하여 데이터를 통신한다. pipe 개념을 네트워크로 확장시킨 것이다.
원격에서 프로세스 간의 데이터를 공유할 때 사용한다.
네트워크 프로그래밍이 가능해야하며, 데이터 세그먼트 처리를 잘해야한다.

### ○ RPC (Remote Procedure Call) - 중요x
원격지 프로세스 간에 함수의 호출에 기반을 둔 기법이다. RPC 서버에 함수가 실제로 구현이 되어있고 클라이언트의 코드에서 함수를 호출하면 서버에 있는 함수가 실행되는 형태를 띈다. 서버와 클라이언트 간에는 Stub이라는 것을 이용해 서로 네트워크를 연결한다. 
LPC (Local Procedure Call)은 동일 시스템내에서 호출 기법을 말한다.


### 참고
http://www.ktword.co.kr/test/view/view.php?m_temp1=302
[Inter-Process Communication(IPC) 프로세스 간 통신](https://velog.io/@chanyoung1998/Inter-Process-CommunicationIPC-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EA%B0%84-%ED%86%B5%EC%8B%A0)
https://hidemasa.tistory.com/49
[Gyoogle](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Operating%20System/IPC(Inter%20Process%20Communication).md)
https://jooona.tistory.com/7

### 질문
> ⭕️ **IPC란 무엇인가?**

IPC는 프로세스들 사이에 서로 데이터를 주고 받는 방식, 즉 프로세스 간의 통신을 의미한다. 각 프로세스는 독립적인 실행 객체이기 때문에 프로세스 간 통신을 하려면 커널이 제공하는 IPC 모델 방식을 사용해서 통신을 해야 한다.
  
> ⭕️ **IPC 모델에 메세지 전달방식과 공유메모리방식이 있는데 두 방식은 각각 언제 쓰이나요?**

공유 메모리 같은 경우에는 프로세스 간에 공유가 되도록 설정해놓은 메모리이며, 모든 프로세스들이 접근이 가능합니다. 처음 생성할 때만 시스템콜을 사용하기 때문에 속도가 빨라요. 그래서 많은 양의 데이터를 전달하는 경우에 많이 사용합니다. 하지만 동기화 문제를 생각해야겠죠.
메세지 전달 방식은 공유자원을 그때그때 프로세스사이에서 전달하는 방식이에요. 이러한 방법의 장점은 동기화를 전혀 고민할 필요가 없어서 구현하기가 편하다는 거죠. 그치만 공유메모리에 비해 속도가 느립니다. 그래서 메세지 패싱 방식은 동기화를 신경쓰지않고 적은 양의 데이터를 전달할 때 쓰입니다.

> ⭕️ **메시지 패싱과 공유메모리 방식말고 다른 IPC 모델도 존재하나요?**

해당 포스팅에서는 메세지 패싱 모델과 공유 메모리 모델의 흐름을 설명을 하였고 해당 모델에서는 주요기법이 존재합니다. 메세지 패싱 모델에서의 주요 기법은 PIPE, FIFO, 메시지 큐, 소켓, 세마포어가 존재하구요. 공유 메모리 모델에서의 기법은 말 그대로 공유 메모리가 쓰입니다. 

