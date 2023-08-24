## Thread 활용

### Thread 활용

---

자바에서 Thread는 동시에 실행되는 코드의 단위

- 스레드는 하나의 작업단위
- 스레드를 활용하면 동시에 여러 작업을 처리하거나 병렬로 작업을 수행할 수 있음
- **동시에 여러가지 작업을 진행하기 위함**
- 크게 두가지 방법

### 1. Thread 클래스를 활용하는 방법

- Thread 클래스를 상속받아 스레드를 생성하는 방법
- 상속받은 클래스에서 **run() 메소드**를 오버라이딩하여 스레드가 수행할 작업을 설정
- **start() 메소드**를 호출하여 스레드를 시작

```java
public class MyThread extends Thread {
    public void run() {
        // 스레드가 수행할 작업
    }
}

// 스레드 생성 및 시작
MyThread thread = new MyThread();
thread.start();
```

- 스레드를 생성하고 관리하는 기능이 Thread 클래스에 내장되어 있어 다양한 기능을 활용
- 자바는 단일 상속만을 지원
    - 다른 클래스를 상속받고 있는 경우 Thread 클래스를 상속받을 수 없음
    - 반대 경우 동일

### 2. Runnable 인터페이스 활용하는 방법

- Runnable 인터페이스를 구현하여 스레드를 생성하는 방법
- 구현한 클래스에서 **run() 메소드를 정의**
- **Thread 클래스의 생성자에 Runnable 객체를 전달**하여 스레드를 생성
- **start() 메소드**를 호출하여 스레드를 시작

```java
class MyRunnable implements Runnable {
    public void run() {
        // 스레드가 수행할 작업
    }
}

// 스레드 생성 및 시작
Thread thread = new Thread(new MyRunnable());
thread.start();
```

- 인터페이스의 다중 구현이 가능하기 때문에 이미 다른 클래스를 상속받고 있어도 스레드를 생성 가능
- Runnable 인터페이스는 스레드 생성과 관련된 기능을 직접 제공하지 않음
    - 스레드의 동작과 관련된 기능은 Thread 클래스의 인스턴스를 생성해서 사용해야 함

### 스레드의 실행제어 및 상태

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/811e11e6-faea-4e95-a8f0-b7b9e5e9d982/Untitled.png)

1. 스레드의 실행제어
- 스레드를 시작하거나 중지시키는 등 스레드의 실행을 제어하는 방법
- Thread 클래스의 메소드를 활용
    - start() : 스레드를 시작, 스레드는 run() 메소드 내의 코드를 실행
    - join() : 다른 스레드가 종료될 때 까지 현재 스레드를 대기
    - sleep(long mills) : 지정된 시간만큼 스레드를 대기
    - interrupt() : 스레드의 작업을 중단
    - setPriority(int new Priority) : 스레드의 우선수위를 지정
    - yield() : 수행중인 스레드 중 우선순위가 같은 다른 스레드에게 제어권을 넘겨줌
1. 스레드의 상태
- 스레드는 다양한 상태를 가지며, 현재 상태에 따라 실행제어가 가능
- **NEW** : 스레드가 생성되고 아직 start()가 호출되지 않은 상태
- **RUNNABLE** : 실행 중 또는 실행 가능 상태
- **BLOCKED** : 동기화 블럭에 의해 일시정지된 상태(lock이 풀릴 때까지 기다림)
- **WAITING, TIME_WAITING** : 실행가능하지 않은 일시정지 상태
- **TERMINATED** : 스레드 작업이 종료된 상태

### 스레드의 동기화

---

여러 스레드가 공유된 자원에 동시에 접근할 때, 스레드 간의 순서를 조절하는 메커니즘

- 임계 영역과 잠금을 활용
- synchronized 와 lock 객체를 사용해 구현
1. **synchronized**
- 서로 다른 두 객체가 동기화를 하지 않은 메소드를 같이 오버라이딩해서 사용하면 두 스레드가 동시에 진행되므로 원하는 결과를 얻지 못함
- synchronized 키워드를 사용해서 해당 블록을 하나의 스레드만 실행할 수 있도록 보장
- 한 객체의 synchronized 메소드나 블록에 진입한 스레드는 해당객체의 락을 획득

```java
public class Thread {
    private int number;

    public synchronized void increaseNumber() {
        count++;
    }

    public synchronized void decreaseNumber() {
        count--;
    }
}
```

1. **lock 객체**
- Lock 인터페이스를 구현한 객체를 사용하여 동기화 수행
- lock(), unlock() 메소드를 통해 락을 획득하고 해제

### 예상질문

1. 자바에서 스레드를 생성하는 방법과 차이점에 대해서 설명해주세요

자바에서 스레드를 생성하는 방법은 두가지가 있습니다. 첫번째는 Thread 클래스를 상속받거나 다음은 Runnable 인터페이스를 구현하는 두가지 방법이 있습니다. 자바의 단일 상속 제약 때문에 Thread 클래스를 상속받을 경우는 다른 클래스를 상속받을 수 없고 Runnable 인터페이스를 구현하는 경우에는 다른 클래스를 상속받을 수 있습니다

1. 스레드의 동기화에 대해 설명해주세요

여러 스레드가 공유된 자원에 동시에 접근하면 동시에 여러 스레드가 진행되어 원하는 결과를 얻지 못합니다. 그래서 동기화를 통해 스레드간의 순서를 조절해야합니다. synchronized 키워드나 Lock 객체를 통해 임계 영역을 정하고 그 영역에는 한번에 하나의 스레드만 접근할 수 있도록 하는 과정이 동기화입니다.

[[Java] Thread 활용하기](https://mollangpiu.tistory.com/286)
