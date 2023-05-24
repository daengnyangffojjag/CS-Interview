# [TCP](https://datatracker.ietf.org/doc/html/rfc793) handshaking

TCP는 연결지향 프로토콜, 연속적으로 패킷의 상태 정보를 확인하고 유지한다.

## TCP 3-way handshaking

- 장치간에 데이터를 전송하기 전에 세션을 수립한다.
- 클라이언트가 서버에 연결을 요청하고, 서버가 요청을 수락하여 연결을 설정하는 과정이다.

### 목적

- 클라이언트와 서버 간에 신뢰성 있는 연결을 설정하고 데이터를 안정적으로 전송하는데 중요한 역할을 한다.
- 데이터 손실이나 왜곡을 최소화하고, 안정적인 통신 환경을 제공한다.

### 단계

![](https://velog.velcdn.com/images/coastby/post/6afde5cc-9c4a-4644-a99f-8288c7cf5b08/image.png)


1. 클라이언트가 `SYN` 패킷을 보내고, 초기 순차 번호(a)를 포함한다.
    - Sequence number는 **임의의 랜덤 수**
    - SYN 플래그비트를 1로 설정한 세그먼트 전송
2. 서버는 SYN 패킷을 받고, `SYN-ACK` 패킷을 보낸다. 초기 응답 순차 번호(a+1)를 포함한다.
    - 접속요청을 받은 서버가 요청을 수락하고, 클라이언트도 포트를 열어달라는 메세지 전송
    - ACK number 필드를 순차번호 + 1로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 세그먼트 전송
3. 클라이언트는 `SYN-ACK` 패킷을 받고, `ACK` 패킷을 보내서 연결 설정을 완료한다.
    - 클라이언트가 수락 확인(ACK)을 보내 연결을 맺음
    - 전송할 데이터가 있으면 이 단계에 데이터를 전송할 수 있다.

`SYN` : Synchronize sequence numbers

`ACK` : Acknowledgement

`세그먼트` : TCP 세그먼트, TCP 세션으로 연결된 양 끝단 간에 교환, 전달되는 데이터 단위 (= 세그먼트 헤더 (TCP 헤더) + 데이터)

## TCP 4-way handshaking

- 연결 종료를 위해 사용된다.
- 클라이언트와 서버 간의 연결을 안전하게 종료하는 과정이다.

### 단계

![](https://velog.velcdn.com/images/coastby/post/06a64869-6a03-4a64-a0df-fdcda37e0d68/image.png)


1. 클라이언트가 연결 종료를 요청하는 FIN 패킷을 보낸다.
    - 클라이언트는 `close()`를 호출한다.
2. 서버는 FIN 패킷을 받고, ACK 패킷을 보내서 수락을 확인한다. 
    - 서버는 클라이언트에 응답을 보내고 `CLOSE_WAIT` 상태에 들어간다. 그리고 아직 남은 데이터가 있다면 마저 전송을 마친 후에 `close()`를 호출한다.
    - 클라이언트는 서버가 FIN을 보낼 때까지 기다린다. (`FIN_WAIT_2`)
3. 서버가 더 이상의 데이터 전송이 없음을 나타내기 위해 `FIN` 패킷을 보냅니다.
    - 서버가 데이터 전송을 마치면 FIN을 보내고, `LAST_ACK` 상태로 들어간다.
4. 클라이언트는 `FIN` 패킷을 받고, `ACK` 패킷을 보내 연결 종료를 완료합니다.
    - 클라이언트는 아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 `TIME_WAIT`으로 기다린다.
        
        → 에러로 인해 연결이 데드락으로 빠지는 것을 방지하기 위해 일정 시간이 넘어가면 `CLOSED`로 들어간다.
        
    - `CLOSED` :
        - 서버 : ACK을 받은 이후 소켓을 닫는다.
        - 클라이언트 : `TIME_WAIT` 시간이 끝나면 닫는다.

### 참고

[https://mindnet.tistory.com/entry/네트워크-쉽게-이해하기-22편-TCP-3-WayHandshake-4-WayHandshake](https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake)
[https://velog.io/@averycode/네트워크-TCPUDP와-3-Way-Handshake4-Way-Handshake](https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake)
https://sjlim5092.tistory.com/37

### 질문

- **TCP의 3-way handshake와 4-way handshake에 대해 설명하시오.**
    - 3-way handshake:
        - 연결 설정을 위해 사용됩니다.
        - 클라이언트가 서버에 연결을 요청하고, 서버가 요청을 수락하여 연결을 설정하는 과정입니다.
        - 단계:
            1. 클라이언트는 SYN 패킷을 보내고, 초기 순차 번호를 포함합니다.
            2. 서버는 SYN 패킷을 받고, SYN-ACK 패킷을 보내고, 초기 응답 순차 번호를 포함합니다.
            3. 클라이언트는 SYN-ACK 패킷을 받고, ACK 패킷을 보내서 연결 설정을 완료합니다.
    - 4-way handshake:
        - 연결 종료를 위해 사용됩니다.
        - 클라이언트와 서버 간의 연결을 안전하게 종료하는 과정입니다.
        - 단계:
            1. 클라이언트나 서버 중 하나가 연결 종료를 요청하는 FIN 패킷을 보냅니다.
            2. 수신자는 FIN 패킷을 받고, ACK 패킷을 보내서 수락을 확인합니다. 이때도 아직 데이터를 전송할 수 있습니다.
            3. 수신자가 더 이상의 데이터 전송이 없음을 나타내기 위해 FIN 패킷을 보냅니다.
            4. 전송자는 FIN 패킷을 받고, ACK 패킷을 보내 연결 종료를 완료합니다.

- **TCP 3,4 way handshake의 단계수 차이가 나는 이유**
    Client가 데이터 전송을 마쳤다고 하더라도 Server는 아직 보낼 데이터가 남아있을 수 있기 때문에 일단 FIN에 대한 ACK만 보내고, 데이터를 모두 전송한 후에 자신도 FIN 메시지를 보내기 때문이다.
    
- **만약 Server에서 FIN 플래그를 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황이 발생하면 어떻게 될까?**
이러한 현상에 대비하여 Client는 Server로부터 FIN 플래그를 수신하더라도 일정시간(Default: 240sec)동안 세션을 남겨 놓고 잉여 패킷을 기다리는 과정을 거친다. (TIME_WAIT 과정)

- **초기 Sequence Number인 ISN을 0부터 시작하지 않고 난수를 생성해서 설정하는 이유?**
Connection을 맺을 때 사용하는 포트(Port)는 유한 범위 내에서 사용하고 시간이 지남에 따라 재사용된다. 따라서 두 통신 호스트가 과거에 사용된 포트 번호 쌍을 사용하는 가능성이 존재한다. 서버 측에서는 패킷의 SYN을 보고 패킷을 구분하게 되는데 난수가 아닌 순처적인 Number가 전송된다면 이전의 Connection으로부터 오는 패킷으로 인식할 수 있다. 이런 문제가 발생할 가능성을 줄이기 위해서 난수로 ISN을 설정한다.
