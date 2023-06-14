### 1. 객체 생성 방법에 따른 차이
```java
// 1. 
String s1 = "Hello";

// 2. 
String s2 = "Hello";

// 3. 
String s3 = new String("Hello");

// 4. 
String s4 = new String("Hello");

System.out.println(s1 == s2); // true
System.out.println(s3.equals(s4)); // false

```
s1과 s2처럼 문자열로 생성된 String 객체는 해당 문자열이 동일한 경우, 그 문자열을 공유한다.

s3와 s4처럼 new 연산자를 통해 String 객체를 생성하게 되면, 해당 문자열이 동일한 내용일지라도 각 객체마다 문자열이 새롭게 생성된다.
![](https://velog.velcdn.com/images/tkdtkd97/post/a46df8ce-ec91-4328-ac5d-eda0473fbf1f/image.png)

### 2. String 객체는 내용을 수정할 수 없는 immutable 클래스
> String 객체를 한번 생성하고 나면, 그 객체의 내용은 수정할 수 없다.

![](https://velog.velcdn.com/images/tkdtkd97/post/04fc0ef8-28f2-4cfb-9d08-b6796c963988/image.png)
→ 그런데 만약, 자주 문자열 내용을 변경하게 되면 그림과 같이 새로운 문자열이 계속 생기게 되므로 효율이 떨어진다.

### 3. 객체가 불변하므로, Multithread에서 동기화를 신경 쓸 필요가 없음. (조회 연산에 매우 큰 장점)

- 자연스럽게 thread-safe한 특성을 갖게 됨
- 동기화와 관련된 위험 요소에서 벗어날 수 있음
- 여러 스레드에서 동시에 접근해도 별다른 문제가 없음
- 또한 String의 경우 한 스레드에서 값을 바꾸면, 해당 객체의 값을 수정하는 것이 아니라 새로운 객체를 String Pool에 생성한다. 따라서 thread-safe하다고 볼 수 있다.

### 4. Garbage Collector로 제거되어야 함
![](https://velog.velcdn.com/images/tkdtkd97/post/872b2a8e-ac4b-4c62-9607-12c51df8d878/image.png)


### String을 사용해야 할 때
- 짧은 문자열을 더할 경우
- 문자열 연산이 적고 멀티스레드 환경일 경우

### 참고
https://velog.io/@ummchicken/String-StringBuffer-StringBuilder

### 면접 예상 질문
> 자바에서 String은 무슨 인가? 데이터 타입인가?

String은 int, long, double과 같은 primitive data type은 아니고 클래스 혹은 더 간단히 사용자 정의 타입이다.

> 자바에서 String은 왜 final로 선언되어 있나?

String은 보안, 최적화, String pool 유지를 위해서 final로 선언되어 있다.

> 동일한 내용의 문자열을 new 연산자를 통해 String 객체를 생성한 A와 B가 있다. A.equals(B)하면 결과가 어떻게 되는가?

동일한 내용을 가지고 있지만 서로다른 참조값을 가리키므로 false가 출력된다.