# Java 17, SpringBoot 3

### LTS(Long Term Support)

**JDK8, JDK11, JDK17**

- 장기 지원 버전

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d92cd29c-4ec1-49ad-8a4e-c2c341decd55/Untitled.png)

### Java 17

> **Sealed 클래스**
>

상속하거나 구현할 클래스를 지정해두고 해당 클래스만 상속 또는 구현을 허용하는 키워드

```java
public sealed interface/class Daengnyang permits Cat, Dog, ... {
}
// 해당 sealed 클래스를 상속하는 클래스가 반드시 permits에 나열된 하위 클래스 중 하나여야 함
```

```java
public class Rabbit implements Daengnyang // 불가능

public class Cat extends Daengnyang //가능
```

- 개발자가 자유롭게 클래스를 상속하여 사용하는 것을 막음
    - 계층 구조를 이해하지 못한 클래스 사용 및 오류를 방지하여 안정성 확보
- 하위 클래스의 제한과 클래스 계층 구조가 명확하게 나타나므로 코드의 가독성이 향상되고 유지보수하기 쉬움
- 허용을 받은 하위 클래스는 sealed, non sealed, final 로 나뉨

```java
public sealed class Daengnyang permits Cat, Dog 
// Cat, Dog만이 Daengnyang 클래스를 상속가능

public sealed class Dog extends Daengnyang permits 말티즈, 시츄
// 말티즈, 시츄만이 Dog class 를 상속가능

public non-sealed class Cat extends Daengnyang
// non-sealed 로 선언된 Cat은 어떤 클래스든 상속가능

public final class 말티즈 extends Dog
public final class 시츄 extends Dog
// Dog 클래스는 말티즈, 시츄 클래스만 상속 받을 수 있음

public class 훈이 extends Dog
// 상속 불가능
public class 훈이 extends 말티즈
// 상속 불가능
// 말티즈 클래스는 final로 선언된 클래스기 때문에 어떤 클래스도 상속 불가능

public class 훈이 extends Cat
// 상속 가능
```

> **Swith문 개선**
>

```java
switch(num) {
		case 1:
		sout("1");
		break;
		case 2:
		sout("2");
		break;
		default:
		sout("?");
		break;
}

// Java 17

String result = switch(num) {
		case 1 -> "1";
		case 2 -> "2";
		default -> "?";
};

// switch문 값 직접 반환 가능
// '->'(람다식)을 사용해서 간결하게 작성
// break문이 필요없음
// 더 간결하고 가독성이 좋아짐
```

> **Record 데이터 클래스**
>
- Java 14부터 도입
- 불변한(Immutable)한 클래스
    - final 클래스로 한번 생성되면 상태를 변경할 수 없음
    - 클래스 내부의 필드들이 final 변수로 선언되어 있어서 값이 할당되면 변경될 수 없음
- 모든 값이 생성자를 통해 생성되어야 함
- DTO와 같은 Data Object 용도로 활용 가능

```java
public record UserDto(Long Id, String name)

UserDto userDto = new UserDto( 1L, "근이");

// 필드에 대한 getter, setter 메소드 없이 생성자에서 필드를 초기화 하여 사용
// Record 클래스의 필드가 final이므로 불변성
```

> **개선된 ZGC(Z Garbage Collector)**
>
1. 기본 스레드 수 증가
    1. 높은 메모리 부하에서 호율적인 GC가 가능
2. 비트맵 형태 메모리 매핑
    1. 메모리 매핑 속도가 빨라짐
3. 힙 크기 동적 조정
    1. 8MB ~ 16TB까지의 heap 크기 지원
4. 대기 시간이 짧은 어플리케이션에 적합한 GC

> **TextBlock**
>

```java
String str = "승근\n"
					 + "백승근\n" 
					 + "근이\n";

//텍스트 블록
String str = """
						 승근
						 백승근
						 근이
						 """;
// SQL 쿼리나 JSON등에서 유용하게 사용가능
```

[](https://blogs.oracle.com/javakr/post/java-17-webcast-brief)

### SpringBoot 3. ,  Spring 6.

> **Spring 6.0**
>

**핵심 변경 사항**

- **Java 17**을 Baseline으로 결정
- Java EE → **Jakarta EE**
    - javax.* → jakarta.*
- Hibernate ORM 6.1 적용해야 함
- **AOT (Ahead-Of-Time)** 도입
- **GraalVM 네이티브 이미지** 지원

> **SpringBoot 3의 요구사항**
>
- 자바 버전 : 17 이상
- Spring 6
- Gradle 7.5+, Maven 3.5+
- Hibernate 6.1
- Jakarta EE 9+
- Spring Security 6+

### AOT vs JIT

1. **JIT 컴파일러**
- Java HotspotVM의 기본설정
- 프로그램 실행 시(런타임)에 바이트 코드를 기계 코드로 컴파일
- 실행 시간에 최적화가 이루어지므로 상황에 맞춘 최적화 코드를 생성할 수 있음
- 실행 시간에 최적화를 수행하는 것이 중요하기 때문에 제한된 환경에서 주로 유용 (모바일 기기)
1. **AOT 컴파일러**
- GraalVm에서 지원
- 실행 전(빌드 타임)에 바이트 코드를 기계 코드로 컴파일
- 실행 전 코드에 대한 정적 코드 테스트를 진행
- 실행 시에 대부분의 최적화가 이미 이루어진 컴파일된 코드가 생성되어 실행속도가 빠름
- 미리 알고 있어야 할 정보가 많고, 프로그램 실행 시간보다 이전에 컴파일 해야함
- 클라우드 환경에서 서비스의 인스턴스 수가 매우 많을 때 서비스의 시작 시간이 빨라야 하기 때문에 유용
- GraalVM이 지원

> **GraalVM**
>

Java로 구현된 OpenJDK 기반의 Java VM 및 JDK

- **Native Image**를 사용해서 Native 애플리케이션으로 확장 가능
- Truffler 언어 구현 프레임워크 및 GraalVM SDk 제공
    - 다른 프로그래밍 언어(JavaScript, Ruby 등)를 런타임으로 동작할 수 있게 지원

> **DateTime 포맷 변경**
>

```java
yyyy-MM-dd'T’HH:mm:ss.SSSXXX
// ISO-8601 표준
// 공백문자 대신 'T' 문자를 사용하여 날짜 시간 구분
```

> **Controller Mapping 시에 후행 슬래시**
>

```java
@RestController
public class Controller {

	@PostMapping("/a/b/")

	@PostMapping("/a/b")

// 다른 것으로 인식
```

> **Rest http interface**
>

[Spring 6의 새로운 HTTP Interface와 3 가지 REST Clients 라이브 코딩](https://www.youtube.com/watch?v=Kb37Q5GCyZs)

> **다양한 새로운 기능**
>
- 향상된 WebFlux 기능
- 데이터베이스 마이그레이션을 위한 Flyway9.0 지원
- 데이터베이스와의 상호작용을 비동기적으로 처리할 수 있는 R2DBC 1.0 지원

### 예상 질문

1. 자바17에 어떤 게 도입되고 달라졌는가? ( 1~2개)
2. 새롭게 도입된 것들을 백엔드 개발자로써 어떻게 사용하는게 좋을지
3. Spring6.0 버전에 도입된 AOT 컴파일러란?
4. AOT컴파일러와 JIT컴파일러를 비교