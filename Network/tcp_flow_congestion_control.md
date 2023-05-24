## TCP 흐름제어

- TCP는 받은 것에 대해서 ACK을 반드시 해야한다.
- 하지만 실제로는 보낼 세그먼트를 계속 보내고, 보내는 중에 ACK이 날라온다.

<img src="https://velog.velcdn.com/images/coastby/post/1485a961-fae6-47b4-af99-b9ce6552a3d3/image.png" width="40%">


### **TCP 버퍼**

송신 TCP는 버퍼에 세그먼트를 보관하고 이것들을 순차적으로 전송하고, 수신 TCP는 도착한 세그먼트를 응용 프로세스가 읽을 때까지 버퍼에 보관한다.

![](https://velog.velcdn.com/images/coastby/post/1da1b05b-f63d-4b3e-9521-5c892820cc1c/image.png)



### **TCP 윈도우 크기 (window size)**

송신 TCP 윈도우 크기 = 수신 TCP 버퍼의 크기

즉, 송신 TCP는 ACK를 받기 전까지 윈도우 크기 만큼의 세그먼트를 연속해서 보낼 수 있다. ACK를 수신하면 다시 보낼 수 있는 세그먼트의 수 만큼 윈도우 크기를 변경한다. (sliding window flow control)

![](https://velog.velcdn.com/images/coastby/post/505d8623-b238-45fc-a246-3de16549f995/image.png)


윈도우 크기가 0이 되면 (zero window) 더 이상 전송을 하지 못하고 ACK를 받을 때까지 대기한다. 

### 재전송

TCP는 ACK를 수신하지 못할 경우 무한정 기다리는 것(deadlock)을 피하기 위해 일정 시간이 지나면 (timeout) 재전송을 한다. 이때 상대방이 ACK를 못 보내는 것이 아직 버퍼에 공간이 없어서라면 상황을 더 악화시킬 수 있다.

이를 알리기 위해 수신 TCP는 현재 남아있는 버퍼 크기를 ACK를 전송할 때 window size 필드를 통해서 알려준다.

![](https://velog.velcdn.com/images/coastby/post/4ab2b59b-dedc-4c1b-8082-bb5bfdfe8e4f/image.png)


### 참고

https://www.youtube.com/watch?v=3Rm-wIRXh_8

# TCP/IP 혼잡제어

네트워크에 입력되는 트래픽 양이 네트워크가 처리할 수 있는 한도(capacity)를 초과할 경우 체증이 발생한다.

정상적으로는 input만큼 output이 나와야하지만, congestion 상태에서는 input이 늘어날수록 혼잡도 때문에 output이 떨어지는 상황이 발생한다.  

![](https://velog.velcdn.com/images/coastby/post/5944f1c8-c9d5-4d4b-8487-5b9c922c5041/image.png)


패킷망은 트래픽의 흐름은 일정하지 않다 (교통망처럼). 회선망은 회선을 미리 할당하고 트래픽을 전송하기 때문에 회선이 없는 경우가 아니면 congestion이 발생하지 않는다.

보통은 capacity 이하로 트래픽이 유입되지만 특정 시간에는 capacity를 초과하기도 한다. 따라서 망의 트래픽 처리 용량 (capacity)를 최대 트래픽 전송율보다 크게 유지할 수 있으면 congestion을 피할 수 있다.

![](https://velog.velcdn.com/images/coastby/post/282aadf2-e748-4afe-b574-31eda06fd5fb/image.png)


어느정도 해결하는 것이지 완전히 해결할 수는 없다.


> 📌 흐름 제어는 송신 호스트와 수신 호스트 간에 처리 속도 간의 문제 해결이다.

## 혼잡 제어 접근법

### 예방적 (Preventive) 혼잡 제어

사전에 망에 입력되는 트래픽 양을 조절한다.

두 가지 단계로 이루어진다.

- 사전에 전송할 데이터 양을 정한다. 망 사업자와 사용자와의 계약 (call admission control)
- 망 사업자는 사용자가 사전에 약속된 내용을 준수하는지 감시 (policing)

라우터에서 패킷을 전송할 때, 사전에 약속된 계약에 따라 패킷의 전송 순위를 결정한다. (priority control) 

- 사용자가 사용할 수 있는 대역을 차별을 둔다.

### ✅ 반응적 (Reactive) 혼잡 제어

congestion이 발생할 때 트래픽 양을 감소시킨다.

○ **Congestion의 발견 (detection)**

Congestion이 발생하는 것을 인식한다.

패킷의 지연시간, 라우터의 버퍼의 길이 등을 계속 측정하며 congestion 초기 상태를 발견해낼 수 있다.

○ **Congestion에 대한 대응 (notification / rate control)**

트래픽을 과도하게 유발시키는 소스들이 체증이 해소될 때까지 트래픽을 보내는 것을 보류하도록 통보한다. (notification)

후방 압박 (backpressure)이나 초크(choke) 패킷 전송이 그 예이다.


> 📌 **End-to-end congestion control**
반응적 혼잡 제어를 수행하기에 적합한 계층은 네트워크 계층 (라우터)이라 할 수 있다. 하지만 인터넷 프로토콜에서는 혼잡 제어 임무를 TCP가 수행하도록 하고 있다.


## TCP congestion control

### Congestion의 발견 (Detection)

TCP는 송신한 패킷에 대해서 ACK를 수신한다. 만약 정해진 시간이 지날 때까지 ACK가 도착하지 않으면 congestion이 발생한 것으로 판단한다. (timeout)

### Congestion에 대한 두 가지 대응 (Rate control)

- **Slow Start** : TCP 호스트는 처음에는 적은 양의 패킷을 전송하고 점차 양을 늘려나간다. Timeout과는 상관없이 모든 호스트는 Slow Start로 패킷을 전송한다.
- **Congestion avoidance** : congestion이 발생한 것으로 판단되면 (timeout), 전송되는 패킷의 양을 초기 상태로 줄여서 다시 시작한다.

### Congestion control의 두 개의 윈도우

- **Awnd (advertised window)**
    
    초기 연결 설정 단계에서, TCP는 상대방 TCP에게 자신의 최대 버퍼 크기 (초기 Awnd)를 알려준다. 세그먼트를 수신할 때마다 TCP는 현재 자신의 버퍼 중 비어있는 공간의 크기 (Awnd)를 알려준다.
    
    → TCP 흐름 제어에서 사용하는 window size
    
- **Cwnd (congestion window)**
    
    TCP가 세그먼트를 전송할 때 ACK을 받지않고 연속해서 보낼 수 있는 세그먼트의 양을 결정한다. 즉, TCP 흐름 제어에 의하면 TCP는 Awnd 만큼 연속해서 세그먼트를 전송할 수 있다. 하지만 congestion control에 의해서 Awnd만큼 전송할 수 없고 Cwnd만큼 전송하게 된다.
    

### Slow start

초기 윈도우는 1로 시작한다. 그리고 ACK를 받을 때마다 윈도우를 하나씩 늘려서 cwnd와 awnd 중 낮은 값이 될 때까지 늘려나간다.

```java
initialize : cwnd = 1 (one maximum segment size)
for each segment that is acknowledge
		cwnd = cwnd + 1 (until min(cwnd, awnd))
```

<img src="https://velog.velcdn.com/images/coastby/post/ce872de7-a774-49cd-bd0a-3e56f85ba87c/image.png" width="50%">

### Congestion avoidance

Congestion이 발생했을 때, rate를 조절하는 알고리즘이다.

TCP는 timeout이 될 때까지 ACK을 받지 못하면 congestion이 발생 것으로 판단하고, congestion avoidance를 수행한다. 

**Rate control**

timeout이 발생하면 threshold를 반으로 감소시킨다. 그리고 cwnd를 1로 설정하고 threshold까지 slow-start를 진행한다. cwnd가 threshold를 넘으면 매 RTT마다 1씩 증가시킨다.

```java
If (segment timeout)
		1. see threshold = cwnd / 2
		2. set cwnd = 1 and restart "slow-start" until (cwnd = threshold)
		3. if (cwnd >= threshold)
				cwnd = cwnd + 1 every roundtrip time
```

![](https://velog.velcdn.com/images/coastby/post/c22e0f49-1fec-4660-9710-a94beadc5f4d/image.png)


### Fast Retransmit과 Fast Recovery

추후에 추가되었다.

- 동일한 ACK을 3개 받을 경우
![](https://velog.velcdn.com/images/coastby/post/66fbfade-ca3e-4be5-afc4-6ca550ab9063/image.png)

    
    - 동일한 ACK이 계속 도착한다는 것은 congestion으로 판단하지만, 그렇더라도 상태가 심각하지 않다고 해석할 수 있다.
    - 그래서 congestion avoidance 보다는 좀 더 적극적으로 전송한다.
- **Fast Retransmit**
    - timeout되기 전이라도 바로 재전송한다. (에러 컨트롤)
- **Fast Recovery**
    - Cwnd를 현재의 Cwnd의 반으로 줄인다.
    - 그리고 줄인 Cwnd에서부터 linear하게 증가시킨다.

<img src="https://velog.velcdn.com/images/coastby/post/b025d2eb-373a-4475-b70c-5a87705860eb/image.png" width="80%">

<img src="https://velog.velcdn.com/images/coastby/post/31ccefa6-922a-41ec-8ff5-9093888bf642/image.png" width="80%">

보수적으로 조절을 한다.

### 혼잡제어 알고리즘들

- Tahoe : slow-start, 고속 재송 알고리즘 등이 구현되었다.
- Reno : Tahoe를 개량한 고속 회피 알고리즘이 도입되었다.
- BIC, CUBIC, H-TCP, Fst TCP, Illinois : 고정 지연, 광대역 (롱 팻 파이프)에서 효율이 높다.
- DCTCP : 지연, 광대역인 데이터 센터를 위해 최적화
- Veno, Westwood : 모바일 회선 및 Wifi 등 무선 환경에 적합

### 구분

1. 패킷 로스를 관측하는 **손실 기반 방식**
    
    손실 기반 방식은 패킷 로스를 관측하고 손실이 늘어나면 혼잡이 발생했다고 간주하고 전송량을 억제하는 방법이다.
    
    ex) Reno, NewReno, H-TCP, BIC, CUBIC
    
2. RTT를 지표로 하는 **지연 기반 방식**
    
    지연 기반 방식은 패킷의 RTT을 관측하고 RTT가 늘어나면 혼잡 상태가 된 것으로 판단하고 전송량을 줄인다.
    
    ex) Vegas, Westwood, Fast TCP
    
3. 양쪽 방식을 사용하는 **하이브리드 방식**
    
    ex) Illinois, DCTCP, CTCP, YeAH
    

### 참고

https://www.youtube.com/watch?v=4TvP_-3KBH4
[https://velog.io/@jsj3282/TCP-흐름제어혼잡제어-오류제어](https://velog.io/@jsj3282/TCP-%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4-%EC%98%A4%EB%A5%98%EC%A0%9C%EC%96%B4)
https://roka88.dev/114
[TCP 혼잡 제어 알고리즘들](https://jacking75.github.io/network_Congestion_control_algorithm/)

### 예상 질문

- **TCP의 흐름제어에 대해 설명해주세요**
  
  송신측과 수신측 사이의 데이터 처리 속도 차이(흐름)을 제어하기 위한 기법으로 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지한다.
    **흐름을 제어하기 위해 수신버퍼가 현재 얼마나 데이터를 받을 수 있는지 알려주는 수신 윈도우 변수를 이용한다.**

- **TCP의 혼잡제어에 대해 설명해주세요.**
  
  네트워크망의 혼잡을 악화시켜 통신에 충돌이 나게 하는 것을 줄이고, 한정된 자원을 잘 분배하여 원활히 돌아갈 수 있도록 제어하는 것이다.
    만약 네트워크 혼잡이 예상되면 송신의 속도를 조절하여 혼잡 붕괴 현상이 일어나는 것을 막습니다.
