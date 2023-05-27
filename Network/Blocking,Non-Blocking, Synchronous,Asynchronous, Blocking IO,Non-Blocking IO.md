## Blocking/Non-blocking

- `호출된 함수`가 `호출한 함수`에게 제어권을 건네주는 유무의 차이

함수 A, B가 있고, A 안에서 B를 호출했다고 가정해보자. 이때 호출한 함수는 A고, 호출된 함수는 B가 된다. 현재 B가 호출되면서 B는 자신의 일을 진행해야 한다. (제어권이 B에게 주어진 상황)

- **Blocking** : 함수 B는 내 할 일을 다 마칠 때까지 제어권을 가지고 있는다. A는 B가 다 마칠 때까지 기다려야 한다.
- **Non-blocking** : 함수 B는 할 일을 마치지 않았어도 A에게 제어권을 바로 넘겨준다. A는 B를 기다리면서도 다른 일을 진행할 수 있다.

즉, 호출된 함수에서 일을 시작할 때 바로 제어권을 리턴해주느냐, 할 일을 마치고 리턴해주느냐에 따라 블럭과 논블럭으로 나누어진다고 볼 수 있다.

## Synchronous/Asynchronous

- `동시성`

아까처럼 함수 A와 B라고 똑같이 생각했을 때, B의 수행 결과나 종료 상태를 A가 신경쓰고 있는 유무의 차이라고 생각하면 된다.

- **Synchronous** : 함수 A는 함수 B가 일을 하는 중에 기다리면서, 현재 상태가 어떤지 계속 체크한다.
- **Asynchronous** : 함수 B의 수행 상태를 B 혼자 직접 신경쓰면서 처리한다. (Callback)

즉, 호출된 함수(B)를 호출한 함수(A)가 신경쓰는지, 호출된 함수(B) 스스로 신경쓰는지를 동기/비동기라고 생각하면 된다.

비동기는 호출시 Callback을 전달하여 작업의 완료 여부를 호출한 함수에게 답하게 된다. (Callback이 오기 전까지 호출한 함수는 신경쓰지 않고 다른 일을 할 수 있음)

### Blocking/Non-blocking, Synchronous/Asynchronous 예시

- https://musma.github.io/2019/04/17/blocking-and-synchronous.html

## I/O 란?

- I/O란 데이터의 입력(Input)과 출력(Output)을 함께 일컫는 말
- 파일 I/O 뿐만 아니라, 어떤 디바이스를 통해 입력, 출력하는 작업 모두 I/O
- 네트워크 : 다른 서버로 데이터 전송(send)하거나, 전송 받는 것(read)도 I/O
    - 소켓 read, send
- I/O는 어플리케이션 성능에 영향
    - I/O로 인해 blocking이 생기고 CPU를 긴 시간동안 idle하게 둠
- I/O 작업은 User 레벨에서 직접 수행할 수 없고, 실제 I/O 작업을 수행하는 위치는 Kernel(커널, 운영체제) 에서만 가능
    - 유저 프로세스는 (or 스레드)는 커널에게 요청하고 작업 완료 후 커널에 반환하는 결과를 기다릴 뿐

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4ce357c4-c6c8-473b-b847-ae4e405936ca/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5d2bb5e5-2ba1-4764-8dbb-c8ca3fbcc3bf/Untitled.png)

## Blocking I/O

- I/O 작업이 진행되는 동안 유저 프로세스가 자신의 작업을 중단한 채, I/O가 끝날 때까지 대기하는 방식
- block이 되어 어플리케이션에서 다른 작업을 수행하지 못하고 대기하므로 resource 낭비가 심함

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d2116b39-49fd-4599-b6ad-7fc24cb0cfe6/Untitled.png)

<aside>
💡 1. 유저는 커널에게 read 작업을 요청하고(System call)**(CPU 제어권을 넘겨준다)**

2. Process는 데이터가 입력될 때 까지 대기하다가 **(제어권을 넘겨주었기 때문에 대기한다. 자기 작업을 제어할 수 없다.)**

3. 데이터가 입력되면 유저에게 결과가 전달되어야만 유저 자신의 작업에 비로소 복귀할 수 있다.

**(제어권을 넘겨받는다)**

</aside>

## non blocking I/O

- I/O 작업이 진행되는 동안 유저 프로세스 작업을 중단시키지 않고 I/O 호출에 대해 즉시 리턴하고, 유저 프로세스가 이어서 다른 일을 수행할 수 있도록 하는 방식
    - 어플리케이션은 다른 작업들을 수행하다가 중간중간 시스템 콜을 보내 I/O가 완료됐는지 커널에게 물어보고, 완료되면 I/O 작업 완료
- 장점 : 모든 작업 수행이 I/O의 진행시간과 관계없이 빠르게 동작하기 때문에 User Process는 자신의 작업을 오랫동안 중지하지 않고 I/O 처리 수행 가능
- 단점 : 반복적인 system call 발생으로 resource 낭비

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d172c5bb-1b4a-4b7c-8a04-15ee8e6171bb/Untitled.png)

<aside>
💡 1. 유저가 커널에게 read작업을 요청하면

2. 데이터가 입력이 됐든 안됐든 요청하는 그 순간, 바로 결과가 반환된다.

이때 입력 데이터가 없으면 입력 데이터가 없다는 결과 메세지 (EWOULDBLOCK)을 반환한다.

3. 입력 데이터가 있을 때 까지 1,2번을 반복한다. → 2번에서 결과 메세지를 받은 유저는 다른 작업 진행이 가능하다.

**(제어권을 넘겨주지 않았기 때문에, 계속해서 본인의 작업을 이어갈 수 있다.)**

4. 입력데이터가 있으면 유저에게 결과가 전달된다.

</aside>

### I/O 이벤트 통지 모델

- Non-Blocking I/O의 문제인 반복적인 system call 호출 해결 위함
- 이벤트 : 수신 버퍼나 출력 버퍼에 데이터를 처리하는 동작

> 수신 버퍼의 이벤트 : 입력 버퍼에 데이터 수신되었다는 것을 알림
출력 버퍼의 이벤트 : 출력 버퍼가 비었으니 데이터 전송이 가능한 상황을 알림
> 

ex) 카카오톡

- non-blocking 방식 : “너 보낸 메시지 있어?” 계속 물어봄
- 이벤트 통지 : 입력 버퍼에서 “사용자가 보낸 메시지가 있습니다”라고 알림 제공

## 예상 질문

1. 동기와 비동기에 차이점에 대해 설명해주세요.
2. Blocking I/O에 대해 설명해주세요(장단점 포함)
3. Non Blocking I/O와 Blocking I/O의 차이를 설명해주세요(장단점 위주로)
4. Non Blocking I/O의 단점을 극복하기 위해 나온 모델에 대해 설명해주세요.
