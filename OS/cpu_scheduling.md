모든 프로세스는 CPU를 필요로 하고 모든 프로세스는 먼저 CPU를 사용하고 싶어한다. 이러한 프로세스들에게 공정하게 CPU 자원을 할당하기 위해 운영체제는 `CPU 스케줄링`을 한다.
CPU 이용률을 극대화하기 위해서는 효율적으로 CPU 스케줄링을 하여 항상 실행 중인 프로세스를 가져야 한다. 어떤 프로세스가 대기해야 할 경우, 운영체제는 CPU를 그 프로세스로부터 회수해 다른 프로세스에 할당한다.

## 📌 용어
### ○ CPU burst, I/O Burst

프로세스 종류마다 입출력장치를 이용하는 시간과 CPU를 이용하는 시간의 양에는 차이가 있다. 비디오 재생, 디스크 백업 작업 등 입출력이 많은 작업이 있으며, 연산, 컴파일, 그래픽 처리 작업 등 CPU 작업이 많은 프로세스도 있다. 입출력 집중 프로세스는 입출력을 위한 대기 상태에 더 많이 머무르게 되고, CPU 집중 프로세스는 대기 상태보다 실행 상태에 더 많이 머무르게 된다.

- CPU burst : CPU를 이용하는 작업
- I/O burst : 입출력장치를 기다리는 작업

대부분의 프로세스는 CPU와 입출력장치를 모두 사용하며 실행된다. 즉, 프로세스의 실행은 CPU 실행 상태와 입출력 대기 상태를 반복하며 실행된다.
(예, 명령어 실행 -> 보조기억장치에 저장 -> 명령어 실행 -> 화면에 출력..)

<img src="https://velog.velcdn.com/images/coastby/post/2aafc4ff-a7f4-417c-947a-6ff302109f3a/image.png" width="40%">

> CPU 스케줄링을 통해 이 CPU burst distribution을 관리한다.

### ○ Ready Queue, Waiting Queue

<img src="https://velog.velcdn.com/images/coastby/post/70027604-eafe-4d6c-a993-18645d1aa0d4/image.png" src="40%">

- Ready Queue (준비큐) : CPU를 이용하고 싶은 프로세스들이 서는 줄
- Waiting Queue (대기큐) : 입출력장칠를 이용하기 위해 대시 상태에 접어든 프로세스들이 서는 줄

준비 상태에 있는 프로세스들의 PCB는 준비큐의 삽입되어 CPU를 사용할 차례를 기다린다. 운영체제는 큐에서 우선순위가 높은 프로세스를 먼저 실행한다.

> 🔖 Queue는 반드시 FIFO 방식의 큐가 아니어도 되며, 우선순위 큐, 트리 등으로 구현될 수 있다.

### ○ Dispatcher
디스패처(Dispatcher)는 CPU 코어의 제어를 CPU 스케줄러가 선택한 프로세스에 주는 모듈이다.
스케줄러에는 멀티 프로그래밍에 이용되는 long-term 스케줄러와 CPU 스케줄링에 이용되는 short-term 스케줄러가 있다.
넓은 의미의 스케줄링 = 스케줄링(Ready Queue 정렬) + Dispatch(프로세스를 CPU에 올림)

> 📌 **CPU 스케줄링은 Ready queue에 있는 프로세스를 대상으로 CPU를 할당하는 순서와 방식을 결정하는 것을 의미한다.**

## 📌 스케줄링의 Criteria
좋은 스케줄링과 나쁜 스케줄링을 평가할 수 있는 기준은 다음과 같다.
- `CPU Utilization`: CPU가 쉬고 있지 않게 해야 한다.
- `Throughput`: 단위 시간 당 처리량 (of instructions / second)
- `Turnaround Time`: 수행을 요청한 후 작업이 끝날 때까지 걸린 시간
- `Waiting Time`: 수행을 요청한 후 작업이 시작될 때까지 걸린 시간 (Running이 되고 Ready Queue에 들어간 시간)
- `Response Time`: 요청한 후 응답이 올 때까지 걸린 시간

<img src="https://velog.velcdn.com/images/coastby/post/48371bbe-9519-4b65-b5f8-6162bbdbc54d/image.png" width="60%">


CPU Utilitzation, Throughput은 늘리고, Time 지표들은 감소하는 것이 스케줄링 목표가 된다.

## 📌 스케줄링의 목표
스케줄링의 세부적인 목표는 시스템의 특성마다 다르다. 

- Batch System

   배치 시스템은 한 번에 하나의 프로그램만 수행하는 것이다. 가능한 한 많은 일을 수행하기 위해 throughout과 CPU utilization이 중요하다.
   
- Interactive System

  사용자가 컴퓨터 앞에서 대화형으로 동작하는 시스템이다.
  - Response Time → 프로세스가 Ready Queue에서 대기하는 시간을 최소화한다.
  - Waiting Time → 프로세스가 Wait Queue에 서 대기하는 시간을 최소화한다.
  - Proportionality → 사용자가 요구하는 바를 이루어야 한다.
  이러한 시스템의 경우, Time sharing 기법을 이용한다.

- Real-time system

  시간 제약 조건이 걸려 있는 시스템이다.
  - Deadline
  - Predictability


## 📌 스케줄링 분류
### ○ 선점형 스케줄링 (Preemptive Scheduling)
프로세스가 CPU를 비롯한 자원을 사용하고 있더라도 운영체제가 프로세스로부터 자원을 강제로 빼앗아 프로세스에 할당할 수 있는 스케줄링 방식이다. 현재 대부분의 운영체제는 선점형 스케줄링을 사용하고 있다.
#### 장점
- 높은 우선 순위를 가진 프로세스를 빠르게 처리하려는 시스템에 유용하다.
- 빠른 응답 시간을 요구하는 시분할 시스템에 유용하다.
- 한 프로세스의 자원 독점을 막을 수 있다.

#### 단점
- 높은 우선 순위를 가진 프로세스들만 들어오는 경우 문맥 교환 과정에서 overhead가 발생할 수 있다.
- 데이터가 다수의 프로세스에 의해 공유될 때 race condition이 발생될 수 있다.
→ mutex lock, monitor 등의 기법을 사용해서 race condition을 피한다.

> 🔖 **race condition**
경쟁 상태(race condition)란 둘 이상의 입력 또는 조작의 타이밍이나 순서 등이 결과값에 영향을 줄 수 있는 상태를 말한다. 입력 변화의 타이밍이나 순서가 예상과 다르게 작동하면 정상적인 결과가 나오지 않게 될 위험이 있는데 이를 경쟁 위험이라고 한다.

### ○ 비선점형 스케줄링 (Non-preemptive scheduling)
프로세스가 자원을 사용하고 있다면 그 프로세스가 종료되거나 스스로 대기 상태에 접어들기 전까지 다른 프로세스가 끼어들 수 없다.
#### 장점
- 모든 프로세스들에게 공정하다.
- 응답 시간을 예측할 수 있다.

#### 단점
- 짧은 작업을 수행하는 프로세스라도 긴 작업이 종료될 때까지 기다려야할 수 있다.


## 📌 스케줄링 알고리즘
### ○ FCFS (First Come First Served)
큐에 도착한 순서대로 CPU를 할당한다. 단순히 준비큐에 삽입된 순서대로 프로세스를 처리하는 비선점형 스케줄링 방식이다. 
#### 장점
- 개발이 용이하다.
- 공평하다.
- Starvation 이슈가 발생하지 않는다.

#### 단점
- `Convoy Effect` (호위 효과) : 소요 시간이 긴 프로세스가 먼저 도달할 경우 효율성이 낮아진다. 실행 시간이 짧은 작업이어도 뒤로 가면 대기 시간이 길어진다. 

### ○ SJF (Shortest Job First, 최단 작업 우선)
호위효과를 방지하기 위해 만들어졌다. 수행 시간이 가장 짧은 프로세스 먼저 실행한다 (비선점형 스케줄링).
#### 장점
- 평균 대기 시간이 짧다.
#### 단점
- 사용 시간이 긴 프로세스는 영원히 기다린다. (Starvation)

### ○ HRN (Hightest Response-ratio Next)
점유 불평등 현상이 발생하는 SJF 알고리즘을 보와하기 위해 만들어졌다. 아래의 우선 순위를 계산하여 동작한다.

💡 우선 순위 = (대기시간 + 실행시간) / (실행시간)

### ○ Priority 스케줄링
정적/동적으로 우선 순위를 부여하여 우선 순위가 높은 순서대로 처리한다. 선점형/비선점형 모두 가능하다. 앞의 SJF와 뒤의 SRTF가 우선순위 스케줄링의 일종으로 볼 수 있다.
이의 문제는 우선순위가 높은 프로세스들에 의해 계속해서 연기되어 실행되지 못하는 Starvation (기아) 현상이 발생할 수 있다는 것이다. 이를 방지하기 위한 대표적인 기법으로 Aging(에이징)이 있다. 이는 오랫동안 대기한 프로세스의 우선순위를 점차 높이는 방법이다.

### ○ Round Robin
라운드 로빈 스케줄링은 정해진 시간 (Time Quantum, Time slice)만큼 돌아가며 CPU를 이용하는 선점형 스케줄링이다. 현대적인 CPU 스케줄링 방식이며 할당 시간이 끝나면 프로세스는 Ready Queue 제일 뒤에 삽입된다. 이 때 문맥 교환이 발생한다. 또한, CPU 사용 시간이 랜덤한 프로세스들이 섞여있을 경우에 효율적이며, 프로세스의 context를 저장할 수 있다.

#### 장점
- Response time이 빨라진다.
- N개의 프로세스가 Ready queue에 있고 할당 시간이 q인 경우, 각 프로세스는 q단위로 1/n CPU 시간을 얻는다. 
  → 어떤 프로세스도 (n-1) * q * time unit이상 기다리지 않는다.
- 프로세스가 기다리는 시간이 CPU를 사용할만큼 증가하는 공정한 스케줄링이다.

#### 단점
- time quantum이 크면 FCFS와 같아진다.
- time quantum이 작으면 context switching이 잦아져서 overhead가 발생한다.

> ✅ 라운드 로빈 스케줄링에서는 time quantum의 크기가 매우 중요하다. 

### ○ Multilevel Queue (다단계 큐)
작업들을 여러 그룹으로 나누어 여러 개의 Queue를 사용한다. 큐마다 다른 time quantum과 다른 스케줄링 알고리즘을 설정할 수 있다.
<img src="https://velog.velcdn.com/images/coastby/post/2564b80d-971e-4663-b71f-ad475d03e42e/image.png" width="40%">

예) 우선 순위가 높은 큐(전면 프로세스)에는 라운드 로빈 스케줄링, time quantum, 낮은 큐(후면 프로세스)는 FCFS, 큰 time quantum을 할당한다.

### ○ Multilevel Feedback Queue (다단계 피드백 큐)
다단계 큐의 공평성 문제를 완화하기 위해 프로세스들이 큐 사이를 이동할 수 있다. 

- 기아 상태 우려 시 (너무 오래 기다림) 우선순위 높은 queue로 (aging)
- 너무 많은 CPU time 사용 시 아래 queue로


### 참고
https://steady-coding.tistory.com/509
https://bnzn2426.tistory.com/65
https://dkswnkk.tistory.com/405
https://zzsza.github.io/development/2018/07/29/cpu-scheduling-and-process/

### 질문
> ⭕️ **CPU 스케줄링이란 무엇인가요?**

여러 프로세스가 있고, 이 프로세스들이 자원(CPU 등)을 동시에 요구하는데 자원은 제한되어 있습니다. 그럴 때 제한된 자원들을 어떻게(순서를 할당하는 등) 나눠줄 것인지에 대한 정책을 말합니다. 

> ⭕️ **선점 스케줄링과 비선점 스케줄링의 차이?**
- 선점 : CPU를 할당받아 실행 중인 프로세스로부터 CPU를 선점(빼앗는 것)하여 다른 프로세스를 할당 할 수 있는 방식입니다.
- 비선점 : CPU를 할당받은 프로세스가 스스로 CPU를 반납할 때까지 CPU를 독점하여 사용 방식입니다.

> ⭕️ **CPU 스케줄링 알고리즘은 어떤 것들이 있나요?**
- FCFS : 큐에 도착한 순서대로 CPU 할당
  - 공평하지만, Convoy effect 발생
- SJF : 수행시간이 짧은 것 부터 CPU 할당
  - FCFS의 단점을 보완하지만, 기아현상 발생
- 우선순위 스케줄링 : 우선순위가 높은 순서대로 처리
  - 기아현상 발생, aging으로 극복
- Round Robin : 동일한 시간의 time quantum만큼 할당
  - time quantum이 길면 FCFS와 같으며, 짧으면 overhead 발생
- Multilevel Queue : 작업을 여러 종류의 큐로 나누어 큐마다 다른 time quantum 할당
- Multilevel-feedback Queue : Multilevel에서 time quantum을 채우면 다음 level로 내려감

