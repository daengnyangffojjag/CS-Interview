# 개념
- 직렬화(serialize) : 자바 언어에서 사용되는 Object 또는 Data를 다른 컴퓨터의 자바 시스템에서도 사용 할수 있도록 바이트 스트림(stream of bytes) 형태로 연속전인(serial) 데이터로 변환하는 포맷 변환 기술

- 역직렬화(Deserialize) : 바이트로 변환된 데이터를 원래대로 자바 시스템의 Object 또는 Data로 변환하는 기술

예를들어
1. JVM 힙이나 스택 메모리에 상주하는 객체 데이터를 직렬화를 통해 바이트 형태로 변환
2. 데이터베이스나 파일과 같은 외부 저장소에 저장
3. 다른 컴퓨터에서 이 파일을 가져와 역직렬화를 통해 자바 객체로 변환
4. JVM 메모리에 적재

![](https://velog.velcdn.com/images/tkdtkd97/post/0bd03f7c-bb6e-457b-8d6f-0567b30faff4/image.png)

직렬화 예시
1. 서블릿 세션 : 단순히 세션을 서블릿 메모리 위에서 운용하지 않고 세션 데이터를 저장하거나 공유가 필요할 때
2. 캐시 : 최초에 데이터베이스에서 조회한 객체 데이터를 재차 조회할때 DB에서 조회하지 않고 객체를 직렬화하여 메모리나 외부 파일에 저장해 두었다가 역직렬화하여 사용
3. 자바 RMI(Remote Method Invocation) : 원격 시스템 간의 메시지 교환을 위해서 사용하는 기술(자바에서 지원)(요즘 잘 사용하지 않음)

# 직렬화 vs JSON
자바 직렬화는 외부 파일이나 네트워크를 통해 클라이언트 간에 객체 데이터를 주고 받을 때 사용된다.

하지만 CSV, JSON 이라는 훌륭한 데이터 포맷이 있는데 굳이 직렬화가 필요할까?
![](https://velog.velcdn.com/images/tkdtkd97/post/8014f1ee-8e07-43ce-8662-e75a7e1320c2/image.png)

### 자바 직렬화 장점
1. 직렬화는 자바의 고유 기술인 만큼 당연히 자바 시스템에서 개발에 최적화되어 있다.
2. 자바의 광활한 레퍼런스 타입에 대해 제약 없이 외부에 내보낼 수 있다는 것이다.
- 기본형이나 배열 같은 왠만한 타입은 JSON으로도 충분히 상호 이용가능하지만 자바의 온갖 컬렉션이나 클래스, 인터페이스 타입은 JSON을 사용하면 이것들을 외부로 내보내기 위해 각 데이터를 매칭시키는 별도의 파싱이 필요.
  하지만 자바의 직렬화는 기본 조건만 지킨다면 하드한 작업없이 외부에 보낼 수 있음

# 자바 직렬화 사용법
### 1. Serializable 인터페이스를 implements 해줌

### 2. ObjectOutputStream 객체 직렬화
```java
public static void main(String[] args) {
    // 직렬화할 고객 객체
    Customer customer = new Customer(1, "홍길동", "123123", 40);

    // 외부 파일명, 통상적으로 .ser, .obj라고 한다.
    String fileName = "Customer.ser";

    // 파일 스트림 객체 생성 (try with resource)
    try (
            FileOutputStream fos = new FileOutputStream(fileName);
            ObjectOutputStream out = new ObjectOutputStream(fos)
    ) {
        // 직렬화 가능 객체를 바이트 스트림으로 변환하고 파일에 저장
        out.writeObject(customer);

    } catch (IOException e) {
        e.printStackTrace();
    }
}
```
![](https://velog.velcdn.com/images/tkdtkd97/post/9ecf3d99-aac8-4ef2-817d-db90f2ee26ae/image.png)

### 3. ObjectInputStream 객체 역직렬화
```java
public static void main(String[] args) {
    // 외부 파일명
    String fileName = "Customer.ser";

    // 파일 스트림 객체 생성 (try with resource)
    try(
            FileInputStream fis = new FileInputStream(fileName);
            ObjectInputStream in = new ObjectInputStream(fis)
    ) {
        // 바이트 스트림을 다시 자바 객체로 변환 (이때 캐스팅이 필요)
        Customer deserializedCustomer = (Customer) in.readObject();
        System.out.println(deserializedCustomer);

    } catch (IOException | ClassNotFoundException e) {
        e.printStackTrace();
    }
}
```
![](https://velog.velcdn.com/images/tkdtkd97/post/e7568d66-a5b2-4ec0-a855-589d178ab236/image.png)
이렇게 역직렬화를 이용하게되면, 직렬화된 외부 파일만 있으면 생성자로 객체 초기화 없이 바로 객체에 정보를 가져와 인스턴스화 하여 사용할 수 있음

### SerialVersionUID
Serializable 인터페이스를 구현하는 모든 직렬화된 클래스는 serialVersionUID(이하 SUID) 이라는 고유 식별번호를 부여 받는다.

#### 식별 ID의 목적 : 클래스를 직렬화, 역질렬화 과정에서 동일한 특성을 갖는지 확인 용도로 사용(권장)
클래스 내부 구성이 수정될 경우 -> 기존 직렬화한 SUID버전 != 현재 클래스의 SUID버전 -> InvalidClassException 예외가 발생시켜 값 불일치를 미연에 방지

#### serialVersionUID 값 명시는 필수가 아님
명시하지 않는다면 -> 시스템 런타임에 클래스의 구조를 이용해 암호 해시함수를 적용해 자동으로 클래스 안에 생성하게 함

![](https://velog.velcdn.com/images/tkdtkd97/post/f6da518c-5f05-4681-8e52-70d669d03794/image.png)
```java
class Member implements Serializable {
    // serialVersionUID 꼭 명시 할 것
    private static final long serialVersionUID = 123L;
    
    private String name;
    private int age;
    private String address;
    
    // private String email; // 새로 추가한 클래스 구성 요소

    ...
}
```

# 자바 직렬화 문제점
Serializable 인터페이스 클래스에 implements하면 아주 간단하게 직렬화 가능한 클래스가 되어 외부로 보낼 수 있다.

하지만 간단한 방법과 다르게 직렬화 구현 대가는 매우 비싸다.

### 1. 직렬화는 용량이 크다
JSON으로 저장한 파일 용량에 비해 거의 2배 차이남

### 2. 역직렬화는 위험하다.
객체가 직렬화하여 외부로 전송하는 과정 누가 중간에 가로채서 내용을 조작할 가능성 있음

> 신뢰할 수 없는 데이터는 절대 역직렬화 하면 안됨

### 3. 클래스 캡슐화가 깨진다.
만일 직렬화할 클래스에 private 멤버가 있어도 직렬화를 하게 되면 그대로 외부로 노출되게 된다.
> 직렬화를 제외하려면 별도로 transient 설정하면 된다.

# 참고
https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A7%81%EB%A0%AC%ED%99%94Serializable-%EC%99%84%EB%B2%BD-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0

# 면접 예상 질문
> 직렬화란 무엇인가요 ?

자바에서 입출력에 사용되는 것은 스트림이라는 데이터 통로를 통해 이동합니다. 하지만 객체는 바이트형이 아니기 때문에 스트림을 통해서 저장하거나 네트워크로 전송하는 것이 불가능합니다. 따라서 객체를 스트림으로 입출력하기 위해서 바이트 배열로 변환하는 것을 직렬화라고 합니다.

반대로 스트림으로 받은 직렬화된 객체를 다시 원래로 돌리는 건 역직렬화라고 말합니다.

> serialVersionUID를 선언해야 하는 이유는 뭔가요?

JVM은 직렬화나 역직렬화를 하는 시점의 클래스에 대해 version 번호를 부여합니다. 그런데 만약 이 시점에서 클래스의 정의가 바뀌게 되면, version 번호도 새롭게 할당해주는데요. 직렬화와 역직렬화의 version 번호가 서로 다르면 안되기 때문에 serialVersionUID를 선언해서 문제를 해결할 수 있습니다.

즉, 클래스 버전이 맞는지 확인하기 위한 용도로 사용된다고 말씀드릴 수 있습니다.

> 직렬화의 단점 2가지 말해보세요

첫번째 JSon형식과 비교해서 약 2배가량 용량이 큽니다.
두번째 직렬화하여 외부로 파일을 전송하는 과정에서 파일 내용이 조작될 가능성이 있습니다.
세번째 직렬화 과정에서 별도로 transient 설정을 해주지 않으면 private 멤버는 외부로 노출됩니다. 즉, 클래스의 캡슐화가 깨집니다.