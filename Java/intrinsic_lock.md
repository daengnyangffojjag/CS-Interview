### 고유 락
- 자바의 모든 객체는 락(lock)을 갖고 있다.
- 모든 객체가 갖고 있으니 고유 락(intrinsic lock), 모니터처럼 동작한다고 하여 모니터 락(monitor lock), 혹은 그냥 모니터(monitor)라고도 한다.

### synchronized 블록
- 동시성 문제를 해결하는 가장 간편한 방법
- 고유 락을 이용하여 여러 스레드의 접근을 제어한다.

### 예제

> 자바에서 여러 스레드가 하나의 cpu에 접근할때 동시성 문제를 어떻게 해결할 수 있을까?

```java
public class Counter {
  private int count;

  public int increase() {
    return ++count; // 스레드 안전하지 않은 연산.
  }
}
```

위의 코드는 원자적(atomic)으로 동작할 것 같지만 사실은 그렇지 않다.

스레드가 접근할 수 있는 환경에서 스레드를 사용할때 제어를 하지 않으면 원하지 않는 값을 반환할 수 있다.

#### 1. Object 인스턴스를 사용하여 lock을 구현

```java
public class Counter {
  private Object lock = new Object();
  private int count;

  public int increase() {
    synchronized(lock) {
      return ++count;
    }
  }
}
```

한 스레드가 먼저 락을 획득한 경우 다른 스레드는 기다려야 한다.


#### 2. 별도의 객체를 생성할 필요없이 Counter의 인스턴스도 자바 객체이므로 락으로 사용할 수 있다.

```java
public class Counter {
  private int count;

  public int increase() {
    synchronized(this) {
      return ++count;
    }
  }
}
```

#### 3. synchronized 블록을 구현

```java
public class Counter {
  private int count;

  public synchronized int increase() {
    return ++count;
  }
}
```

### 재진입 가능성(Reentrancy)
- 자바의 고유 락은 재진입 가능하다.
- 락의 획득이 호출 단위가 아닌 스레드 단위로 일어나기 때문
- 이미 락을 획득한 스레드는 같은 락을 얻기 위해 대기할 필요 없다.
- 이미 락을 갖고 있으므로 같은 락에 대한 synchronized 블록을 만났을 때 대기없이 통과한다.

```java
public class Reentrancy {
  public synchronized void a() {
    System.out.println("a");
    // b가 synchronized로 선언되어 있지만 a진입시 이미 락을 획득하였으므로,
    // b를 호출할 수 있다.
    b();
  }

  public synchronized void b() {
    System.out.println("b");
  }

  public static void main(String[] args) {
    new Reentrancy().a();
  }
}
```

> 만약 자바의 고유 락이 재진입 가능하지 않다면 위 코드는 a 메서드 내부의 b를 호출하는 지점에서 데드락이 발생한다.

### 구조적인 락(structured lock)


- 고유 락을 이용한 동기화를 구조적인 락(structured lock)이라고 한다.

- synchronized 블록 단위로 락의 획득/해제가 일어나므로 구조적이라고 한다.
- synchronized 블록을 진입할 때 락의 획득이 일어나고, 블록을 벗어날 때 락의 해제가 일어난다.
- 따라서 구조적인 락 A와 B가 있을 때 A 획득 -> B 획득 -> B 해제 -> A 해제는 가능하지만 A 획득 -> B 획득 -> A 해제 -> B 해제는 불가능 하다.

> A 획득 -> B 획득 -> A 해제 -> B 해제 순서로 락을 사용해야 하는 경우라면 ReentrantLock과 같은 명시적인 락을 사용해야 한다.

### 가시성(visibility)
> 가시성이란?

- 동시성 프로그램의 이슈 중 하나는 가시성이다.
- 값을 사용한 다음 블록을 빠져나가고 나면 다른 쓰레드가 변경된 값을 즉시 사용할 수 있게 해야 한다는 의미이다.


> 자바에서는 스레드가 락을 획득하는 경우 그 이전에 쓰였던 값들의 가시성을 보장한다.

- synchronized가 적용된 Counter 예제에서 스레드 A, 스레드 B 순서로 increase()를 호출했을 때, 스레드 B는 스레드 A가 쓴 값을 읽을 수 있다(visible 하다).

- 이는 고유 락 뿐만 아니라 ReentrantLock 같은 명시적인 락에서도 똑같이 적용된다.

### 참고
http://happinessoncode.com/2017/10/04/java-intrinsic-lock/

### 면접 예상 질문
> 자바에서 고유 락이란 무엇인가요?

자바에서 고유 락은 또는 뮤텍스(mutex)라고도 불리며, 멀티스레딩 환경에서 한 번에 하나의 스레드만이 접근할 수 있는 동기화 메커니즘입니다. 객체 단위로 락을 가지며, 한 스레드가 고유 락을 확보하면 다른 스레드는 그 락을 얻을 때까지 대기해야 합니다.

> synchronized 키워드는 무엇이고 어떻게 작동하나요?

synchronized 키워드는 메서드나 블록을 동기화하기 위해 사용되며, 한 스레드가 synchronized 블록을 실행하면 다른 스레드들은 해당 블록의 실행이 끝날 때까지 대기하게 됩니다. 이는 메서드나 블록 내에서 특정 객체의 고유 락을 확보하여 다른 스레드의 접근을 차단하는 역할을 합니다.

> synchronized 메서드와 synchronized 블록의 차이점은 무엇인가요?

synchronized 메서드는 메서드 전체를 임계 영역으로 설정하며, 해당 메서드를 호출하는 모든 스레드들은 객체의 고유 락을 확보하게 됩니다. 반면에 synchronized 블록은 메서드 내부의 특정 블록만을 임계 영역으로 설정할 수 있으며, 객체의 일부분에 대한 락을 설정할 수 있습니다. 이를 통해 동기화 영역을 더 세밀하게 제어할 수 있습니다.