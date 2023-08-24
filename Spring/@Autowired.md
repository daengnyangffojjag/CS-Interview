## @Autowired

---

Spring에서 의존성 주입(Dependency Injection)을 위해 사용하는 어노테이션

- 필드, 메소드, 생성자등에 적용
- 스프링 컨텍스트에 등록된 빈(Bean)객체들 사이에서 의존성을 자동으로 주입하기 위해 사용
- 의존하는 객체를 자동으로 주입받을 수 있게 해줌

### @Autowired 동작 방식

**타입 매칭(Type Matching)**

- @Autowired 어노테이션은 주입할 대상 객체의 타입을 기준으로 매칭
- ApplicationContext 내에서 동일한 타입의 빈을 찾아 주입
1. 해당 타입의 빈이 **하나만** 존재하는 경우
- 그 빈을 자동으로 주입
- 별다른 설정 필요X
1. 해당 타입의 빈이 **여러 개** 존재하는 경우
- 추가적인 설정 필요
    - @Primary 어노테이션을 통한 우선순위 부여
        - @Primary 어노테이션이 지정된 빈이 자동으로 주입
    - @Qualifier 어노테이션을 통해 특정 빈 지정
        - 빈의 이름이나 구분자를 지정하여 주입할 빈을 지정

### @Autowired의 주입 방식

1. 수정자 주입(Setter Injection)

```java
public class MyClass {
    private MyDependency myDependency;

    @Autowired
    public void setMyDependency(MyDependency myDependency) {
        this.myDependency = myDependency;
		// MyDependency 타입의 빈을 주입받기 위해 setMyDependency 메소드 사용
    }
    ...
}
```

장점

- 객체의 의존성을 주입받는 메소드를 명시적으로 정의해 의존 주입의 시점 제어 가능
- 선택적인 의존성에 대처 가능

단점

- **외부에서 의존성을 변경하기 어려움**
- **세터 메소드에 의해 객체의 일관성**
- **Setter 메소드로 인한 코드의 가독성 감소**
1. 필드 주입(Field Injection)

```java
public class MyClass {
    @Autowired
    private MyDependency myDependency;
		...
}
```

장점

- 스프링 컨테이너가 필드에 직접 접근하여 주입
- 코드가 간결하고 읽기 쉬움

단점

- **클래스가 해당 필드에 직접적으로 의존해 클래스와 주입 객체간의 결합도가 높아짐**
    - 테스트나 확장성 측면의 문제
- **필드에 직접 접근하므로 외부에서 의존성을 변경하기 어려움**
1. **생성자 주입(Constructor Injection)**

```java
public class MyClass {
    private MyDependency myDependency;

    @Autowired
    public MyClass(MyDependency myDependency) {
        this.myDependency = myDependency;
    }

		// 생성자가 1개인 경우 @Autowired 생략가능
    public MyClass(MyDependency myDependency) {
        this.myDependency = myDependency;
    }    
		...
}
```

장점

- 명확하고 가독성 있는 의존성 표현
- **객체 생성 시점에 모든 의존성을 주입받기 때문에 의존성 변경에 용이**
    - 새로운 의존성을 주입하는 생성자만 수정하면 됨
- **Mock 객체 등을 주입한 테스트 용이성**
- **객체의 생성 시점에 의존성을 주입받음**
    - 객체가 생성되면서 한번만 주입되고 이후에는 변경되지 않음
    - 불변성 보장
- **필수적인 의존성을 강제로 주입받기 때문에 객체의 일관성 보장**
- **final 키워드를 사용 가능**
    - 불변성 보장

```java
public class MyClass {
    private final MyDependency myDependency;

    public MyClass(MyDependency myDependency) {
        this.myDependency = myDependency;
    }
		...
}
```

단점

- 생성자 매개변수가 증가할 경우 번거러움

```java
@Service
public class RecordService {

	private final RecordRepository recordRepository;
	private final RecordFileRepository recordFileRepository;
	private final Validator validator;
	private final ApplicationEventPublisher applicationEventPublisher;

	public RecordService(final RecordRepository recordRepository, final RecordFileRepository recordFileRepository,
		final Validator validator, final ApplicationEventPublisher applicationEventPublisher) {
    this.recordRepository = recordRepository;
    this.recordFileRepository = recordFileRepository;
    this.validator = validator;
    this.applicationEventPublisher = applicationEventPublisher;
		}
	}
```

### 면접질문

1. @Autowired 어노테이션의 동작 방식과 주입 순서는 어떻게 되나요?

@Autowired 어노테이션은 스프링 컨테이너에서 주입 대상의 타입과 일치하는 빈을 찾아 자동으로 주입합니다. 주입 순서는 타입에 일치하는 빈이 하나일 경우 해당 빈을 주입하고 여러개일 경우 필드나 메소드이름을 기준으로 하거나 @Primary 또는 @Qualifier 어노테이션에 따라 주입합니다

1. @Autowired의 주입 방식 중 생성자 주입이 주로 사용되는 이유를 설명해주세요

생성자 주입은 객체 생성 시점에 모든 의존성을 주입 받는데 그로 인해 의존성을 변경 할 수 없는 불변 객체를 생성할 수 있고 코드의 가독성과 테스트 용이성을 높이고 컴파일 시점에 의존성 오류를 잡아내는 데 용이 합니다. 또한 final 키워드를 통해 불변성을 보장할 수 있는 장점도 있습니다

[[스프링 핵심 원리 - 기본편] @Autowired 필드 명, @Qualifier, @Primary](https://smpark1020.tistory.com/345)

[[스프링 핵심기술] - @Autowired](https://jjingho.tistory.com/6)

[](https://chat.openai.com/)
