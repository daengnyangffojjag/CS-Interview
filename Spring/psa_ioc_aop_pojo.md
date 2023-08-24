# IoC
### 스프링을 사용하지 않고 순수한 자바 코드로 제어
![](https://velog.velcdn.com/images/tkdtkd97/post/7f98911b-5206-45de-97ac-fc1901630b4c/image.png)

```java
// 설정정보를 엮어줄 Configuration 클래스를 생성한다.
public class AppConfig {

    public MemberService memberService() {
        return new MemberServiceImpl(new MemoryMemberRepository());
	}

    public OrderService orderService() {
        return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
	} 
}

// 사용 예시
public class OrderServiceImpl implements OrderService {

  private final MemberRepository memberRepository;
  private final DiscountPolicy discountPolicy;

  public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
      this.memberRepository = memberRepository;
      this.discountPolicy = discountPolicy;
  }
}
```
- 자바코드로 의존성을 정리, **개발자가 제어**를 하고 있음



### IoC 란?
- Inversion of Control의 줄임말, 제어의 역전
- 스프링 애플리케이션에서는 오브젝트(빈)의 생성과 의존 관계 설정, 사용, 제거 등의 작업을 **애플리케이션 코드 대신 스프링 컨테이너가 담당**
- 이를 **스프링 컨테이너**가 코드 대신 오브젝트에 대한 **제어권**을 갖고 있다고 해서 IoC라고 부름
- 따라서, 스프링 컨테이너를 IoC 컨테이너라고도 부름

### IoC 컨테이너
- 스프링에서는 IoC를 담당하는 컨테이너를 빈 팩토리, DI 컨테이너, 애플리케이션 컨텍스트라고 부름
- 오브젝트의 생성과 오브젝트 사이의 런타임 관계를 설정하는 DI 관점으로 보면, 컨테이너를 빈 팩토리 또는 DI 컨테이너라고 부름
- 그러나 스프링 컨테이너는 단순한 DI 작업보다 더 많은 일을 하는데, DI를 위한 빈 팩토리에 여러 가지 기능을 추가한 것을 애플리케이션 컨텍스트라고 함
- 정리하자면, 애플리케이션 컨텍스트는 그 자체로 IoC와 DI 그 이상의 기능을 가졌다고 보면 됨

### 빈 팩토리와 애플리케이션 컨텍스트
![](https://velog.velcdn.com/images/tkdtkd97/post/d9c5e987-519b-4c40-b04c-dfd299ac15b7/image.png)

- 빈 팩토리
    - 스프링 컨테이너의 최상위 인터페이스
    - **스프링 빈**을 **관리**하고 **조회**하는 역할 담당
    - 대표적으로 **getBean()** 메서드 제공


### 설정 메타 정보
- IoC 컨테이너의 가장 기초적인 역할을 **오브젝트를 생성하고 이를 관리**하는 것이다.
- 스프링 컨테이너가 관리하는 이런 오브젝트는 **빈**이라 부른다.
- **설정 메타 정보**는 바로 이 빈을 어떻게 만들고 어떻게 동작하게 할 것인가에 관한 정보이다. ex. config.class

- 스프링 컨테이너는 자바 코드, XML, Groovy 등 다양한 형식의 설정 정보를 받아들일 수 있도록 유연하게 설계되어 있다.

![](https://velog.velcdn.com/images/tkdtkd97/post/060ee6fd-7d5c-438d-8f69-0c18ee4df6cb/image.png)

### 어노테이션 기반 자바 코드 설정
```java
@Configuration
public class AppConfig {

        @Bean
        public MemberService memberService() {
                return new MemberServiceImpl(memberRepository());
        }
}
```
- @Configuration : 1개 이상의 빈을 제공하는 클래스의 경우 반드시 명시해야 한다.
- @Bean : 클래스를 빈으로 등록할 때 사용한다.

### XML 기반의 스프링 빈 설정
```
<beans xmlns="http://www.springframework.org/schema/beans"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xsi:schemaLocation="http://www.springframework.org/schema/beans http://
www.springframework.org/schema/beans/spring-beans.xsd">

     <bean id="memberService" class="hello.core.member.MemberServiceImpl">
             <constructor-arg name="memberRepository" ref="memberRepository"/>
     </bean>
</beans>
```

- XML 기반의 설정 파일을 보면 자바 코드로 된 설정 파일과 거의 비슷하다는 것을 알 수 있다.
- XML 기반으로 설정하는 것은 최근에 잘 사용하지 않는다.

# AOP
### AOP(Aspect Oriented Programming)란?
- AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다.
- 관점 지향은 어떤 로직을 기준으로 **공통관심사항, 핵심관심사항**으로 나누어서 보고 그 관점을 기준으로 모듈화 하겠다는 것이다.
- 핵심관심사항에 집중할 수 있도록 공통 관심 사항을 **한곳에서 컨트롤** 할 수 있다.
- 단일책임의 원칙을 지킬 수 있게 도와줌

### AOP 적용 방법
1. 컴파일 타임 적용
   -> 컴파일 시점에 바이트 코드를 조작하여 AOP가 적용된 바이트 코드를 생성하는 방법.

2. 로드 타임 적용
   -> 순수하게 컴파일한 뒤, 클래스를 로딩하는 시점에 클래스 정보를 변경하는 방법

3. **런타임 적용**
   -> 스프링 AOP가 주로 사용하는 방법. A라는 클래스 타입의 Bean을 만들 때 A 타입의 Proxy Bean을 만들어 Proxy Bean이 Aspect 코드를 추가하여 동작하는 방법.

### 스프링 AOP
- 스프링에서 제공하는 스프링 AOP는 프록시 기반의 AOP 구현체이다.
- **프록시 객체**를 사용하는 것은 접근 제어 및 **부가 기능**을 추가하기 위해서이다.
- 스프링 AOP는 **스프링 Bean**에만 적용할 수 있다.
- 모든 AOP 기능을 제공하는 것이 목적이 아닌, 중복 코드, 프록시 클래스 작성의 번거로움 등 흔한 문제를 해결하기 위한 솔루션을 제공하는 것이 목적이다.
- 스프링 AOP는 순수 자바로 구현되었기 때문에 특별한 컴파일 과정이 필요하지 않다.
- **메서드 지원, 런타임시 지원**

![](https://velog.velcdn.com/images/tkdtkd97/post/49b3d7d2-2a24-4604-ae7f-ac6a80e841ff/image.png)

- 프록시 패턴에서는 interface가 존재하고 Client는 이 interface 타입으로 Proxy 객체를 사용한다.
- Proxy 객체는 기존의 타겟 객체(Real Subject)를 참조한다.
- Proxy 객체와 기존의 타겟 객체의 타입은 같고, Proxy는 원래 할 일을 가지고 있는 Real Subject를 감싸서 Client의 요청을 처리한다.
### 스프링 AOP 구현
```java
// 타켓 메서드의 실행 시간을 측정해주는 로직 작성
@Component // AOP는 @Bean에서만 동작하기 때문에 Bean으로 등록 해준다.
@Aspect // AOP로 쓸거라는 의미
public class PerfAspect {

    @Around("execution(* com.example..*.EventService.*(..))") // 적용범위
    public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
        long begin = System.currentTimeMillis();
        Object reVal = pjp.proceed();
        System.out.println(System.currentTimeMillis() - begin);
        return reVal;
    }
}
```
ex. Transactional 도 AOP의 예시
- 프록시 객체를 통해 공통 관심사항을 해결 -> 트랜잭션 열기, 종료시 commit()


# PSA
### PSA(Portable Service Abstraction)란?
- 환경의 변화와 관계없이 일관된 방식의 기술로의 접근 환경을 제공하는 추상화 구조
- POJO 원칙을 철저히 따른 Spring의 기능으로 Spring에서 동작할 수 있는 Library들은 POJO원칙을 지키게끔 PSA형태의 추상화가 되어있음을 의미
- PSA = 잘 만든 인터페이스
- PSA가 적용된 코드라면 나의 코드가 바뀌지 않고, 다른 기술로 간편하게 바꿀 수 있도록 확장성이 좋고, 기술에 특화되어 있지 않는 코드를 의미합니다.

### Spring PSA 를 왜 사용할까?
- 서비스를 추상화함으로써 개발자가 실제 구현부를 알지 못하더라도 해당 기능을 사용할 수 있게된다.
- 즉, 추상화 계층인 인터페이스 API 의 정보를 활용해 해당 서비스의 모든 기능을 이용하면 되는 것이다.
- PSA 는 해당 추상화 계층을 구현하는 또 다른 서비스로 언제든지 교체할 수 있게 해준다. 비즈니스 로직의 수정도 없이 말이다.

### PSA 예시
### Spring Transaction
```java
// Transaction를 Low level로 구현
try (
			Connection conn = DriverManager.getConnection(
             "jdbc:coco://127.0.0.1:5432/test", "coco", "password");
             Statement statement = conn.createStatement();
             ) {

             //start transaction block
             conn.setAutoCommit(false); //default true

             String SQL = "INSERT INTO Employees " +
             			  "VALUES (101, 20, 'Rita', 'Tez')";
             stmt.executeUpdate(SQL);

             String SQL = "INSERTED INT Employees " +
             			  "VALUES (107, 22, 'Kita', 'Tez')";
             stmt.executeUpdate(SQL);

             // end transaction block, commit changes
             conn.commit();

             // good practice to set it back to default true
             conn.setAutoCommit(true);

             } catch(SQLException e) {

                System.out.println(e.getMessage());
                conn.rollback();
             }
```

- 위의 코드와 같이 Low level로 트랜잭션 처리를 하려면 commit() 또는 rollback()등 직접 해줘야함
- 하지만 Spring이 제공하는 @Transactional 어노테이션을 사용하면 단순하게 메소드에 어노테이션을 붙여줌으로써
  트랜잭션 처리가 간단하게 이루어집니다.

```java
@Transactional(readOnly = true)
Employees findById(Integer id);
```
- 이 또한 PSA로써 다양한 기술 스택으로 구현체를 바꿀 수 있습니다.
- 예를들어, JDBC를 사용하는 DatasourceTransactionManager, JPA를 사용하는 JpaTransactionManager, Hibernate를 사용하는 HibernateTransactionManager를 유연하게 바꿔서 사용할 수 있습니다.
- 즉, 기존 코드는 변경하지 않은 채로 트랜잭션을 실제로 처리하는 구현체를 사용 기술에 따라 바꿀 수 있는 것입니다.

# POJO
### POJO란?
- Plain Old Java Object의 약자로 다른 클래스나 인터페이스를 상속/implements 받아 메서드가 추가된 클래스가 아닌 일반적으로 우리가 알고 있는 getter, setter 같이 기본적인 기능만 가진 자바 객체를 말한다.
```java
// 순수한 POJO
public class User {
    private int id;
    private String name;
    private String email;
    
    public int getId() {
    	return id;
    }
    public String getName() {
    	return name;
    }
    public String getEmail() {
    	return email;
    }
    
    public void setId(int id) {
    	this.id = id;
    }
    public void setName(String name) {
    	this.name = name;
    }
    public void setEmail(String email) {
    	this.email = email;
    }
}
```
- 고대에 자바를 이용해 비즈니스 서비스를 개발할 때 비즈니스 로직 뿐만 아니라 트랜잭션, 보안 등 로우레벨의 로직까지 작성해야하는 부담감을 없애고자 EJB(Enterprise Java Beans)를 만들게 되었다.
- EJB를 사용하면서 로우레벨의 로직 개발에 대한 수고를 덜 수 있었지만, 한 두가지 기능을 사용하기 위해 거대한 EJB를 상속받거나 implements 하게 되어 가벼운 서비스조차도 무겁게 만들어졌고, 다른 기능으로 대체하기 위해선 전체 코드를 수정해야 하는 문제점이 발생하였다. -> 객체지향이 흐려지는 시점
- JAVA의 기본 개념인 객체지향에 집중하고, 특정 클래스나 라이브러리에 종속되지 않는 POJO 구성으로 코드를 작성한다면 이런 문제점을 해결할 수 있을 것이라고 생각했다.
- 따라서 Spring은 POJO 방식을 기반으로 한 웹 프레임워크이고, IoC와 DI, AOP 등 Spring의 주요 기술을 활용해 POJO 기반의 구성을 이루게 되었다.

### POJO 개념을 사용하지 않은 예시
```java
public class ExampleListener implements MessageListener {

  public void onMessage(Message message) {
    if (message instanceof TextMessage) {
      try {
        System.out.println(((TextMessage) message).getText());
      }
      catch (JMSException ex) {
        throw new RuntimeException(ex);
      }
    }
    else {
      throw new IllegalArgumentException("Message must be of type TextMessage");
    }
  }

}
```
- 위 코드는 JMS 라는 기능을 사용하기 위해 MessageListener 인터페이스를 상속받을 것을 알 수 있다.
- 이렇게 POJO 기반의 코드를 작성하지 않게 되면 이후 JMS와 기능을 비슷하지만 다른 솔루션을 사용하고자 할 때 교체하기가 굉장히 힘들어지게 된다.

### POJO 기반으로 작성한 예시
```java
@Component
public class ExampleListener {

  @JmsListener(destination = "myDestination")
  public void processOrder(String message) {
    System.out.println(message);
  }
}
```

- 위 예제는 JmsListner를 상속받지 않고 어노테이션을 통해 객체를 주입받은 상황이다.
- 이런식으로 코드를 작성하게 되면 해당 클래스와의 결합도가 낮아져 다른 솔루션으로 변경하고 할 경우 @JmsListener를 @(다른 솔루션)으로 코드를 수정만 하면 가능하므로 유지 보수에 있어 좀 더 유용하게 활용할 수 있다.
- 스프링 프레임워크는 위의 예제처럼 우리의 코드와 라이브러리와의 결합성을 줄이도록 도와준다.

### POJO의 조건
#### 1. 특정 규약에 종속되지 않는다.

- 위의 예제처럼 JMS를 사용하기 위해 MessageListener를 상속받아서는 안된다.
- 단일 상속 제한 때문에 객체지향적인 설계기법 적용하기 어려워짐
- 다른 환경으로의 이전이 어려움

#### 2. 특정 환경에 종속되지 않는다.
- 예를 들어 웹환경에 종속되는 HttpServletRequest나 HttpSession와 관련된 API를 직접 이용해서는 안된다.
- 다른 환경에서 사용하기 어려움
- 비즈니스 로직과 기술적인 내용을 담은 웹정보 코드가 섞여 이해하기 어려워짐
  웹서버에 올리지 않고 독립적으로 테스트하기 어려워짐

#### 3. 단일 책임 원칙을 지키는 클래스
- 단순히 1,2번을 지켰다고 POJO라 할 수 없다.
- 책임과 역할이 각기 다른 코드를 하나의 클래스에 넣는 경우 진정한 POJO라 할 수 없다.
- 즉, POJO란 객체지향적인 원리에 충실하면서, 특정 환경과 규약에 종속되지 않아 필요에 따라 재사용될 수 있는 방식으로 설계된 오브젝트라 할 수 있다.


# 참고
https://steady-coding.tistory.com/600
https://www.youtube.com/watch?v=Hm0w_9ngDpM
https://code-lab1.tistory.com/193
https://dev-coco.tistory.com/83
https://ch4njun.tistory.com/270
https://happyer16.tistory.com/entry/POJOplain-old-java-object%EB%9E%80
https://velog.io/@galaxy/Spring%EC%9D%98-%EA%B8%B0%EB%B3%B8-%ED%8A%B9%EC%A7%95-POJO

# 면접 예상 질문
> IoC가 무엇인가요?

IoC(Inversion of Control)이란 "제어의 역전"이란 의미로, 메서드나 객체의 호출작업을 개발자가 하는 것이 아닌, 외부에서 결정되는 것을 의미합니다. 즉 인스턴스의 생성부터 소멸까지를 IoC 컨테이너가 대신 관리해줍니다.이 과정에서 DI(의존성 주입)를 통해 IoC컨테이너에 주입시킵니다.

new Repository() 처럼 직접 생성하는게 아니라, 스프링이 직접 생성해주고 연결을 해줍니다.

> 스프링에서 AOP가 뭔가요?

AOP(Aspect Oriented Programming) 는 관점 지향 프로그래밍으로, 로직을 기준으로 중복된 코드들(공통 관심사항)을 뽑아서 Aspect로 모듈화하여 재사용하는 것입니다.

> PSA가 뭔가요?

스프링 PSA는 "Portable Service Abstraction"의 약어로, 다양한 서비스 프로바이더들과의 통합을 편리하게 하기 위한 추상화 계층을 의미합니다. 스프링 PSA는 서로 다른 백엔드 시스템과의 통합을 위한 일관된 인터페이스를 제공하여 애플리케이션의 이식성을 높이고 유연성을 제공합니다.
PSA가 적용된 대표적인 곳은 JDBC, JPA, Transaction Manager 등이 존재합니다.


> PSA에 어떤 서브프로젝트가 포함되어 있나요?

Spring JDBC: 데이터베이스와의 통합을 위한 추상화 계층을 제공합니다.
Spring JMS: 메시징 시스템과의 통합을 지원합니다.
Spring JPA: Java Persistence API를 지원하며 ORM 프레임워크와의 통합을 위한 추상화 계층을 제공합니다.
Spring Transactions: 트랜잭션 관리를 위한 표준 인터페이스와 추상화 계층을 제공합니다.
Spring Cache: 캐싱을 위한 추상화 계층을 제공하여 다양한 캐시 프로바이더와 통합합니다.

> PSA의 장점과 단점은 무엇인가요?

장점
다양한 백엔드 시스템과의 통합이 용이하고 일관된 방법을 제공합니다.
개발자가 여러 시스템 간의 차이점을 다루는 복잡성을 줄여줍니다.
유연성과 이식성을 높여 애플리케이션의 유지보수와 확장성을 개선합니다.

단점
모든 백엔드 시스템과의 특정 기능을 모두 추상화하기는 어려울 수 있습니다.
일부 기능이나 설정이 백엔드 시스템마다 다르게 적용될 수 있습니다.

> POJO란 무엇인가요?

스프링에서의 POJO는 "Plain Old Java Object"의 약어로, 특정 프레임워크나 라이브러리에 의존하지 않는 순수한 자바 객체를 말합니다. 스프링은 이러한 POJO들을 사용하여 애플리케이션을 구성하고 관리하는데 중점을 둡니다.

> 참고: 개발자는 스프링 POJO, IOC, AOP, PSA를 사용하여 어떻게 개발해야할까?

스프링은 개발자가 POJO 형태로 객체를 만들고 IOC와 AOP를 통해 의존성을 주입하며, PSA를 사용하여 이러한 POJO 객체들을 다양한 백엔드 서비스들과 통합하는 방식을 지향합니다.

스프링은 POJO를 사용하여 개발자가 일반적인 자바 객체를 만들도록 유도합니다.
POJO는 순수하게 비즈니스 로직에 집중할 수 있도록 하며, 스프링의 IOC를 통해 컨테이너에서 관리됩니다.

스프링의 IOC 컨테이너는 POJO 객체들을 생성하고, 객체 간의 의존성을 주입합니다.
개발자는 객체 간의 의존성을 설정하는 데 집중할 필요 없이, 컨테이너에 위임할 수 있습니다.

AOP는 POJO의 핵심 비즈니스 로직과는 별개로 공통 관심사를 분리하여 모듈화합니다.
개발자는 POJO에 공통 기능을 중복하지 않고도 적용할 수 있습니다.

PSA는 POJO 객체들을 다양한 백엔드 서비스와 통합하는 추상화 계층을 제공합니다.
스프링의 다양한 PSA 구현체를 사용하여 POJO 객체들이 다양한 백엔드 시스템과 상호작용할 수 있습니다.

스프링의 이러한 방식은 개발자에게 유연하고 효율적인 개발 환경을 제공하며, 모듈화와 재사용성을 높이고 높은 유지보수성을 확보할 수 있도록 돕습니다.