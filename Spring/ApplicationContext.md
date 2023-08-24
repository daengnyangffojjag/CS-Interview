## 애플리케이션 컨텍스트(ApplicationContext)

---

Spring에서 **빈의 생성**과 **관계설정** 같은 **제어를 담당**하는 스프링 컨테이너(IoC Container)

- ApplicationContext는 BeanFactory의 기능을 상속받음
- ApplicationContext는 빈 관리기능 + 편리한 부가 기능을 제공
    - 메시지소스를 활용한 국제화 기능
    - 환경변수
    - 애플리케이션 이벤트
    - 편리한 리소스 조회

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4ad54742-2722-42e2-9b24-1a4f777bac8d/Untitled.png)

- BeanFactory를 직접 사용하기보단 부가기능이 포함된 ApplicationContext를 사용

### ApplicationContext의 빈 생성 방식

1. XML 설정
- XML 파일을 사용해 빈 생성
- ApplicationContext는 XML파일을 읽고 빈을 생성
- XML 파일에 빈의 클래스, 의존 관계, 프로퍼티 값 등을 설정
1. **JavaConfig**
- 자바 기반의 설정 클래스에 @Configuration 어노테이션 사용
- @Bean 어노테이션을 메소드에 적용해 빈으로 등록
1. **Component Scanning**
- @Component, @Service, @Repository, @Controller, @Autowired와 같은 어노테이션 사용
- 클래스를 자동으로 검색하고 빈으로 등록

### Bean 요청 시 처리 과정

---

1. **@Configuration**

ApplicationContext는 **@Configuration**이 붙은 클래스를 설정 정보로 등록

**Key-Value 형태**로 저장되며 @Bean이 붙은 메소드의 이름이 빈 이름이 Key(빈 이름)

return하는 실제 객체를 value(빈 객체)에 저장

```java
@Configuration
public class AwsS3Configuration {

...

	@Bean
	public AmazonS3Client amazonS3Client() {
		
	...

	}
}
```

1. 클라이언트가 해당 빈 요청
2. ApplicationContext가 빈 목록에서 요청한 이름이 있는지 확인
3. ApplicationContext는 설정 클래스로부터 빈 생성을 요청하고, 생성된 빈을 돌려줌

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/853dc024-4430-4795-afd4-e96e70e4db7e/Untitled.png)

### ApplicationContext의 장점

1. Dependency Injection(의존성 주입)
- 객체 간의 결합도를 낮출 수 있음
- 컨테이너가 객체를 생성하고 관리
- 의존성 주입을 통해 객체를 관리하기 때문에 클라이언트는 객체의 생성과 관리에 대해 몰라도 됨
- @Configuration이 붙은 구체적인 팩토리 클래스를 알 필요가 없음
1. 객체의 생명주기 관리
- 컨테이너가 객체의 생성, 소멸 등을 관리
- 개발자는 이에 대한 로직을 신경쓰지 않아도 됨
1. 다양하고 편리한 부가 기능 제공
2. **싱글톤** 객체 관리
- ApplicationContext는 Bean 객체를 싱글톤으로 관리
- 여러 요청이 동일한 객체 인스턴스를 공유
- 메모리 사용량 감소, 성능 향상
- 대규모 트래픽 처리 가능

### 면접 질문

1. ApplicationContext가 빈을 생성하는 방식에는 어떤 것이 있나요?
2. ApplicationContext가 Bean 요청 시 처리하는 과정에 대해서 설명해주세요

[[Spring] 애플리케이션 컨텍스트(Application Context)와 스프링의 싱글톤(Singleton)](https://mangkyu.tistory.com/151)

[스프링 핵심 원리 - 기본편 - 인프런 | 강의](https://www.inflearn.com/course/스프링-핵심-원리-기본편)
