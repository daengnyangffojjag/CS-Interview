# 배경지식
우리가 유튜브로 음악을 틀어놓고 동시에 게임을 하는건 너무 당연한 일이다.

> 과연 어떻게 이런 일이 가능할까?

![](https://velog.velcdn.com/images/tkdtkd97/post/5c733342-7c5b-4b4d-91c6-9a95b4762463/image.png)

이는 마치 동시에 실행되는 것처럼 보이겠지만 사실은 빠른 속도로 유튜브와 게임이 번갈아 실행되는 것이다.

### 프로세스란?
프로세스는 실행 중인 프로그램(program)을 뜻한다.

### 프로세스 상태(Process State)
![](https://velog.velcdn.com/images/tkdtkd97/post/e1efbc0e-ac84-4038-9f65-cef23c842d4f/image.png)
- new(생성) : 프로세스가 생성된 상태이다. OS 커널에 존재하는 Ready Queue에 올라가면 ready상태가 된다.
- **ready(준비)** : 프로세스가 CPU로부터 메모리 공간을 할당받길 기다리는 상태이다. 이때 프로세스 스케줄러에 의해 프로세스가 할당을 받게 되면 running 상태가 된다. 이것을 dispatch 라고 한다.
- **running(실행)** : 명령어들이 실행되는 상태이다. 이때 interrupt(간섭)이 발생하면 ready 상태로 변한다. 실행을 끝마치면 exit(종료)되고terminated 상태로 변한다. I/O(입출력)이나 event가 발생하면 waiting 상태로 변한다.
- waiting(대기) : 특정 event가 발생하길 기다리는 상태이다. 이때 I/O 나 특정 event가 완료되면 ready상태로 변한다.
- terminated(종료) : 프로세스가 실행을 끝마친 상태이다.
- **dispatching : 준비상태에서 실행상태로 상태전이(state transition)되는 것.**


# 요약
하나의 CPU를 가지고 있다고 가정할때 컴퓨터는 프로세스를 동시에 실행시킬 수 없다.

1. 일정 순서에 맞게 프로세스를 멈췄다가 실행했다가 하는데 이 과정을 Context Switching라고 한다.
2. 프로세스를 멈출때 각각의 프로세스 정보를 저장해놓는곳이 PCB(Process Control Block)이다.
3. 프로세스를 멈추는 경우들 중 하나가 System call 이다.

# Context Switching(문맥 교환)
### Context Switching이란?
CPU가 현재 작업 중인 프로세스에서 다른 프로세스로 넘어갈 때 지금까지의 프로세스의 상태를 저장하고, 새 프로세스의 저장된 상태를 다시 적재하는 작업을 Context Switch(문맥 교환)이라 한다. (프로세스의 정보는 PCB에 저장된다.)

### Context란?
Process의 경우 현재 프로세스가 중단 되었을 때, 중단된 시점 부터 다시 프로세스를 실행하기 위한 정보를 Context라고 부른다.

### 언제 발생하는가?
> 멀티태스킹(Multitasking)

- 실행 가능한 프로세스들이 운영체제의 스케줄러에 의해 조금씩 번갈아가며 수행되는 것을 말한다.
- 번갈아 가며 프로세스가 CPU를 할당 받는데 이때 Context Switching 한다.
- 사용자가 체감하기 힘든 속도로 Context Switching되며 프로세스가 처리되기 때문에 동시에 처리되는 것처럼 느껴진다.

> 인터럽트 핸들링(Interrupt handling)

- 인터럽트란 컴퓨터 시스템에서 예외 상황이 발생했을때 CPU에게 알려 처리할 수 있도록 하는 것을 말한다.
- 인터럽트가 발생하면 Context Switching한다.
- I/O request : 입출력 요청
- time slice expired : CPU 사용시간이 만료
- fork a child : 자식 프로세스 생성
- wait for an interrupt : 인터럽트 처리 대기

> 사용자와 커널 모드 전환(User and kernel mode switching)

- 사용자와 커널 모드 전환은 Context Switch가 필수는 아니지만 운영체제에 따라 발생할수 있다 = system call

### Context Switching 과정
![](https://velog.velcdn.com/images/tkdtkd97/post/c60fe5c0-4b29-4b21-8473-691ebb79b587/image.png)
1. 프로세스P0가 유저코드에서 실행되고 있음
2. interrupt나 system call이 발생하면 운영체제에게 권한을 넘김
3. 운영체제는 프로세스P0의 프로세스 정보를 PCB0에 저장
4. 운영체제는 프로세스P1의 프로세스 정보를 PCB1에 불러옴
5. 프로세스P1이 유저코드에 실행됨
6. 이 과정 반복

사용자 모드 = 1, 5
커널 모드 = 2, 3, 4

이때 각 프로세스마다 하나의 PCB를 갖는다.

>  위 과정을 반복적으로 수행한다. Context Switching하는 동안 CPU는 아무일도 하지않는 시간이 발생하는데 이를 오버헤드(Overhead)라 하고 오버헤드가 잦아지면 성능이 떨어질수 있다.

# PCB(Process Control Block)

### PCB란?
운영체제가 프로세스를 제어하기 위해 정보를 저장해 놓는 곳으로, 프로세스의 상태 정보를 저장하는 자료구조입니다.

> Process의 경우 현재 프로세스가 중단 되었을 때, 중단된 시점 부터 다시 프로세스를 실행하기 위한 정보를 Context라고 부른다. 이러한 Process의 Context 정보는 PCB(Proccess Control Block)이라는 구조체에 저장된다.

### PCB에 저장되어 있는 정보
![](https://velog.velcdn.com/images/tkdtkd97/post/b1592030-055c-4c3f-9d40-f28d977a9360/image.png)

포인터 : 프로세스의 현재 위치를 저장하는 포인터 정보입니다.

프로세스 상태 : 프로세스의 각 상태 (생성(New), 준비(Ready), 실행(Running), 대기(Waiting), 종료(Terminated))를 저장합니다.

프로세스 번호 : 모든 프로세스에는 프로세스 식별자를 저장하는 프로세스 ID 또는 PID라는 고유 한 ID가 할당됩니다.

프로그램 카운터 : 프로세스를 위해 실행될 다음 명령어의 주소를 포함하는 카운터를 저장합니다.

레지스터 : 누산기, 베이스, 레지스터 및 범용 레지스터를 포함하는 CPU 레지스터에 있는 정보입니다.

메모리 제한 : 이 필드에는 운영 체제에서 사용하는 메모리 관리 시스템에 대한 정보가 포함됩니다. 여기에는 페이지 테이블, 세그먼트 테이블 등이 포함될 수 있습니다.

열린 파일 목록 : 이 정보에는 프로세스를 위해 열린 파일 목록 이 포함됩니다.

> 운영체제는 빠르게 PCB에 접근하기 위해 프로세스 테이블을 사용하여 각 프로세스의 PCB를 관리합니다.

![](https://velog.velcdn.com/images/tkdtkd97/post/903e441f-e07f-4885-ab70-5cc35408aa94/image.png)


# System call
### 운영체제란?
운영체제 는 컴퓨터의 자원들을 효율적으로 관리하며 사용자가 컴퓨터를 편리하고 효과적으로 사용할 수 있도록 환경을 제공하는 여러 프로그램의 모임이라고 이해하면 된다.

운영 체제는 컴퓨터 사용자와 컴퓨터 하드웨어 간의 인터페이스로서 동작하는 시스템 소프트웨어의 일종으로, 다른 응용프로그램이 유용한 작업을 할 수 있도록 환경을 제공해 줍니다.
![](https://velog.velcdn.com/images/tkdtkd97/post/0e5245a0-373a-49c6-be3b-a72734409d23/image.png)

### 커널(kernel)이란?
운영체제 중 항상 필요한 부분만을 전원이 켜짐과 동시에 메모리에 올려놓고, 그렇지 않은 부분은 필요할 때 메모리에 올려서 사용하게 된다. 이때 메모리에 상주하는 운영체제의 부분을 kernel(커널)이라고 한다.

### CPU 모드
CPU에 있는 Mode bit로 모드를 구분하여 0은  '커널모드(kernel mode)', 1은 '사용자모드 (user mode)'로 나뉘어서 구동된다.

#### 사용자 모드 (User Mode)
사용자 모드에서 사용자 애플리케이션 코드가 실행된다. 사용자가 접근할 수 있는 영역에 제한이 있기 때문에 해당 모드에서는 하드웨어(디스크, I/O 등)에 접근할 수 없다.

접근을 위해서는 '시스템 콜(System Call)'을 사용해야 한다.

#### 커널 모드 (Kernel Mode)
운영체제(OS)가 CPU를 사용하는 모드이다. 시스템 콜을 통해 커널모드로 전환이 되면 운영체제는 하드웨어를 제어하는 명령어(Privileged Instructions)를 실행한다.

![](https://velog.velcdn.com/images/tkdtkd97/post/888be102-89af-480c-98ef-125ae9c34927/image.png)

### System call이란?
운영체제는 다양한 서비스 들을 수행하기 위해 하드웨어를 직접적으로 관리한다. 이와 반면 응용 프로그램은 운영체제가 제공하는 인터페이스를 통해서만 자원을 사용할 수 있다. 운영체제가 제공하는 이러한 인터페이스를 '시스템 콜 (system call)' 이라고 한다.

### 목적
시스템콜은 이러한 커널 영역의 기능을 사용자 모드가 사용 가능하게, 즉, 프로세스가 하드웨어에 직접 접근해서 필요한 기능을 할 수 있게 해준다. (즉, 응용프로그램은 시스템 콜을 사용해서 원하는 기능을 수행할 수 있음.)

### 사용법
![](https://velog.velcdn.com/images/tkdtkd97/post/014cbbbc-7c0d-475a-bd6b-3164b203388c/image.png)
위 그림처럼 운영체제(OS)는 메모리에 프로그램 적재, I/O처리, 파일시스템 처리 등 여러 서비스들을 제공하는데 사용자 프로세스는 이에 직접적인 접근이 아닌 시스템 콜 호출을 통해 서비스를 제공받을 수 있다.

### 시스템콜 종류
시스템콜은 크게 6가지로 분류할 수 있다.

> 프로세스 제어 (Process Control)

끝내기(exit), 중지 (abort)
적재(load), 실행(execute)
프로세스 생성(create process) - fork
프로세스 속성 획득과 속성 설정
시간 대기 (wait time)
사건 대기 (wait event)
사건을 알림 (signal event)
메모리 할당 및 해제

> 파일 조작 (File Manipulation)

파일 생성 / 삭제 (create, delete)
열기 / 닫기 / 읽기 / 쓰기 (open, close, read, wirte)
위치 변경 (reposition)
파일 속성 획득 및 설정 (get file attribute, set file attribute)

> 장치 관리 (Device Manipulation)

하드웨어의 제어와 상태 정보를 얻음 (ioctl)
장치를 요구(request device), 장치를 방출 (relese device)
읽기 (read), 쓰기(write), 위치 변경
장치 속성 획득 및 설정
장치의 논리적 부착 및 분리

> 정보 유지 (Information Maintenance)

getpid(), alarm(), sleep()
시간과 날짜의 설정과 획득 (time)
시스템 데이터의 설정과 획득 (date)
프로세스 파일, 장치 속성의 획득 및 설정

> 통신 (Communication)

pipe(), shm_open(), mmap()
통신 연결의 생성, 제거
메시지의 송신, 수신
상태 정보 전달
원격 장치의 부착 및 분리

> 보호 (Protection)

chmod()
umask()
chown()

### 실제 사용 예시 (C언어)
![](https://velog.velcdn.com/images/tkdtkd97/post/e4af4fcd-c000-4e83-89c6-5bb1ffbfc1c3/image.png)

# 참고
https://code-lab1.tistory.com/38
https://yoongrammer.tistory.com/52
https://spurdev.tistory.com/13
https://didu-story.tistory.com/311


# 면접 예상 질문
> 시스템 콜(system call)이 무엇인가요?

System Call(시스템 콜)은 사용자나 응용프로그램이 커널에서 제공하는 기능을 사용하기 위한 인터페이스입니다.

운영체제는 커널이 제공하는 서비스를 '시스템 콜'을 사용해야만 사용할 수 있도록 제한함으로써 컴퓨터 자원을 보호하면서 사용자나 응용프로그램에게 서비스를 제공할 수 있습니다.

> 문맥 교환(Context-switching)이란 무엇인가요?

문맥 교환은, 인터럽트나 시스템 콜 등으로 현재 실행 중인 프로세스의 제어를 다른 프로세스에 넘겨서 그 프로세스가 실행 가능하도록 하는 것을 말합니다. 즉, 실행권을 다른 곳에 넘기는 과정입니다. 프로세서의 레지스터에 있던 내용은 나중에 다시 사용할 수 있도록 PCB에 저장됩니다.

이렇게 PCB의 이전 상태를 저장하는 등, 컨텍스트 스위칭 동안에는 CPU가 일을 하지 못하기 때문에 이것이 자주 일어난다면 성능 저하를 유발할 수 있습니다.

```
참고.. 쓰레드 vs 프로세스
쓰레드의 경우는 문맥 교환에 있어서 프로세스보다는 좋은 성능을 낼 수 있습니다.
이미 쓰레드 자체가 프로세스 내의 대부분을 공유하고 있기 때문입니다.
따라서 이전 상태값을 전부 저장하거나 할 필요가 없어지고,
문맥 교환에 따른 성능 저하가 줄어들 수 있습니다.

멀티 스레드는 멀티 프로세스보다 적은 메모리 공간을 차지하고 문맥 전환이 빠르다는 장점이 있지만,
오류로 인해 하나의 스레드가 종료되면 전체 스레드가 종료될 수도 있다는 점과
동기화 문제를 안고 있습니다.
반면 멀티 프로세스 방식은 하나의 프로세스가 죽더라도
다른 프로세스에는 영향을 끼치지 않고 정상적으로 수행된다는 장점이 있지만,
멀티 스레드보다 많은 메모리 공간과 CPU 시간을 차지한다는 단점이 있습니다.
```

> PCB가 무엇입니까?

PCB는 프로세스의 각종 상태 정보들을 저장해 두는 곳입니다. 특히 PCB에서는 Program counter라고 해서 다음 명령어의 주소를 가지고 있기도 합니다.

> Context Switching 과정을 설명해보세요

1. 프로세스P0가 유저코드에서 실행되고 있음
2. interrupt나 system call이 발생하면 운영체제에게 권한을 넘김
3. 운영체제는 프로세스P0의 프로세스 정보를 PCB0에 저장
4. 운영체제는 프로세스P1의 프로세스 정보를 PCB1에 불러옴
5. 프로세스P1이 유저코드에 실행됨
6. 이 과정 반복
