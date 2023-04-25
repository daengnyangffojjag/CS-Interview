# SpringBoot 발표

# 공식 홈페이지([링크](https://spring.io/projects/spring-boot))

> **Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".**
Spring Boot를 사용하면 "그냥 실행"할 수 있는 독립 실행형 프로덕션 등급 Spring 기반 애플리케이션을 쉽게 만들 수 있습니다.
> 

### 📌[Primary goals](https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started):

- Provide a radically faster and widely accessible getting-started experience for all Spring development.
- Be opinionated out of the box but get out of the way quickly as requirements start to diverge from the defaults.
- Provide a range of non-functional features that are common to large classes of projects (such as embedded servers, security, metrics, health checks, and externalized configuration).
- Absolutely no code generation and no requirement for XML configuration.
- 번역
    - 모든 Spring 개발을 위해 근본적으로 더 빠르고 광범위하게 액세스할 수 있는 시작 경험을 제공합니다.
    - 요구 사항이 기본값에서 벗어나기 시작하면 즉시 의견을 제시하되 신속하게 비켜야 합니다.
    - 임베디드 서버, 보안, 메트릭, 상태 확인 및 외부화된 구성과 같은 대규모 프로젝트 클래스에 공통적인 다양한 비기능 기능을 제공합니다.
    - 코드 생성 및 XML 구성에 대한 요구 사항이 전혀 없습니다.

## ✨**[Features](https://spring.io/projects/spring-boot)**

### 1️⃣독립형 Spring 애플리케이션 생성

Create stand-alone Spring applications

스프링 부트는 단지 실행만 하면 되는 **스프링 기반의 어플리케이션을 쉽게 만들 수 있다.**

### 2️⃣Auto-configuration

Automatically configure Spring and 3rd party libraries whenever possible

스프링 및 스프링 security, Data JPA 등의 다른 스프링 프레임워크 요소를 쉽게 사용 가능하다.

개발자는 `application.properties` 또는 `application.yml`만 설정하고, 스프링 부트에서 해당 설정으로 통해 생성한 빈을 사용하기만 하면 된다.

### 3️⃣starter dependencies

Provide opinionated 'starter' dependencies to simplify your build configuration

편리한 의존성 관리 & 자동 권장 버전 관리

1. dependency 작성이 간단해진다. (작성해야하는 줄이 압축된다.) 
2. starter를 추가함으로써 애플리케이션에 **어떤 기능이 필요한지 특정할 수 있게 된다.** (web, JPA, security..)
3. 각 라이브러리끼리 호환되는 권장 버전으로 미리 세팅되어 있기 때문에 해당 라이브러리의 어떤 버전이 필요한지 고민할 필요가 없어진다. 

### 4️⃣내장 서버로 인한 간단한 배포 서버 구축

Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)

기존에는 외부에서 서버를 가져와서 설치해야 했는데, 내장 서블릿 컨테이너 덕분에 jar 파일로 간단히 배포할 수 있다. 

```java
java -jar $REPOSITORY/$JAR_NAME &
```

서버 구동 시간도 단축되었다.

### 5️⃣Actuator

Provide production-ready features such as metrics, health checks, and externalized configuration

actuator는 애플리케이션의 메트릭, 상태를 쉽게 확인할 수 있는 모니터링 기능이다.

### 6️⃣코드 생성이 전혀 없고 XML 구성이 필요하지 않습니다.

Absolutely no code generation and no requirement for XML configuration

> 개발자들이 개발에만 더욱 집중하고, 스프링 기반 어플리케이션을 쉽게 만들 수 있다.
> 

---

# Spring Boot in Action

- *Auto-configuration*
    
    많은 Spring 애플리케이션에 **공통적인 애플리케이션 기능에 대한 구성을 자동으로 제공한다.**
    
- *Starter dependencies*
    
    Spring Boot에 어떤 기능이 필요한지 알려주면 필요한 라이브러리가 빌드에 추가된다.
    
- *Actuator*
    
    실행 중인 Spring Boot 애플리케이션 내부에서 무슨 일이 일어나고 있는지에 대한 insight를 제공한다.
    
- *command-line interface*
    
    기존의 프로젝트 빌드 과정 없이, 간단한 어플리케이션 코드만으로 어플리케이션을 개발할 수 있다.
    

## 1️⃣ **AUTO-CONFIGURATION**

기존의 Spring 어플리케이션에는 특정 지원 기능을 활성화하는 Java configuration 또는 XML configuration이 필요하다. 

예를 들어, JDBC로 관계형 데이터베이스에 접근하는 애플리케이션을 작성한다면 Spring의 `JdbcTemplate`을 bean으로 구성해야 한다.

```java
@Bean
public JdbcTemplate jdbcTemplate(DataSource dataSource) {
		return new JdbcTemplate(dataSource);
}
```

위의 코드로 `JdbcTemplate`의 인스턴스를 생성하고 `DataSource`를 주입한다. 

그리고 이 dependency를 위해,  `DataSource` bean을 구성해야 한다. 아래의 bean configuration은 내장 데이터베이스에서 실행할 두 개의 SQL 스트립트를 지정하여 내장 데이터베이스를 생성한다. 그리고 `build()` 메서드는 내장 데이터베이스를 참조하는 `DataSource`를 반환한다.

```java
@Bean
public DataSource dataSource() {
		return new EmbeddedDatabaseBuilder()
		          .setType(EmbeddedDatabaseType.H2)
		          .addScripts('schema.sql', 'data.sql')
		          .build();
}
```

두 코드는 길거나 복잡하지 않다. 하지만 일반적인 Spring 애플리케이션의 일부이며 똑같은 메서드를 가진 Spring 애플리케이션들이 무수히 많다.

즉, 일반적이지만 필수적인 설정들이다.

**→ bolierplate configuration**

### ✅Spring Boot의 역할

Spring Boot는 이러한 구성을 자동으로 해준다. 

Spring Boot가 애플리케이션의 class-path에 H2 데이터베이스 라이브러리가 있음을 감지하면 임베디드 H2 데이터베이스를 자동으로 구성한다. JdbcTemplate이 class-path에 있으면, JdbcTemplate 빈도 구성한다.

이 외에도 JPA(Java Persistence API), Thymeleaf 템플릿, security 및 Spring MVC에 대한 auto-configuration도 포함하고 있다.

개발자는 `application.yml`을 작성하고, Spring boot가 자동으로 구성해준 빈을 사용하기만 하면 된다.

## 2️⃣**STARTER DEPENDENCIES**

프로젝트에서 빌드에 dependency를 추가하기 위해서는 어떤 라이브러리가 필요한지, group과 artifact는 무엇인지, 필요한 버전이 무엇인지, 다른 dependency들과의 호환이 되는 버전인지를 고민해야 한다.

예를 들어, JSON resource representation을 이용하는 Spring MVC로 REST API를 구축한다고 가정한다. 또한 JSR-303 사양에 따라 선언적 유효성 검사 (declarative validation)를 적용하고 임베디드 Tomcat 서버를 사용하여 애플리케이션을 제공하려고 합니다. 이를 위해서는 Maven 또는 Gradle 빌드에 적어도 8개의 dependencies가 필요하다.

■ org.springframework:spring-core
■ org.springframework:spring-web
■ org.springframework:spring-webmvc
■ com.fasterxml.jackson.core:jackson-databind
■ org.hibernate:hibernate-validator
■ org.apache.tomcat.embed:tomcat-embed-core
■ org.apache.tomcat.embed:tomcat-embed-el
■ org.apache.tomcat.embed:tomcat-embed-logging-juli

```java
// maven
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>6.0.8</version>
</dependency>

// gradle
implementation group: 'org.springframework', name: 'spring-core', version: '6.0.8'
```

### ✅Spring Boot의 역할

Spring Boot에서는 특정 기능을 위한 라이브러리 종류를 생각할 필요없이, starter dependencies를 통해 기능을 받아오기만 하면 된다. 위의 경우, `org.springframework.boot:spring-boot-starter-web`만 dependency에 추가하면 된다. 

```java
implementation group: 'org.springframework.boot', name: 'spring-boot-starter-web', version: '3.0.6'
```

> **Starter dependencies are really just special Maven (and Gradle) depen-
dencies that take advantage of transitive dependency resolution to aggregate commonly used libraries under a handful of feature-defined dependencies.**
Spring Boot는 프로젝트의 dependency 관리를 위해 `starter dependencies`를 사용한다. `starter dependencies`는 흔히 사용되는 라이브러리를 모아서 만든 특별한 Maven (or Gradle) dependency이다.
> 

<aside>
📌 ****Transitive dependency****

- direct dependencies are *directly required by the component*. A direct dependency is also referred to as a *first level dependency*. For example, if your project source code requires Guava, Guava should be declared as *direct dependency*.
- transitive dependencies are dependencies that your component needs, but only because another dependency needs them.
</aside>

이를 통해 아래와 같은 장점이 있다.

1. dependency 작성이 간단해진다. (작성해야하는 줄이 압축된다.) 
2. starter를 추가함으로써 애플리케이션에 **어떤 기능이 필요한지 특정할 수 있게 된다.** (web, JPA, security..)
3. 해당 라이브러리의 어떤 버전이 필요한지 고민할 필요가 없어진다. 각 라이브러리끼리 호환되는 권장 버전으로 미리 세팅되어 있기 때문이다.

## 3️⃣**THE ACTUATOR**

다른 기능들이 Spring 개발을 단순화하는 반면 Actuator는 런타임에 애플리케이션 내부를 검사하는 기능을 제공한다. Actuator를 설치하면, 애플리케이션 아래와 같은 내부 작동을 검사할 수 있다.

- Spring 애플리케이션 컨텍스트에서 어떤 bean이 구성되었는지
- Spring Boot의 auto-configuration으로 어떻게 설정되었는지
- 애플리케이션에서 사용할 수 있는 환경 변수, 시스템 속성, configuration properties 및 command-line arguments
- 응용 프로그램의 스레드 및 지원 스레드의 현재 상태
- 애플리케이션에서 처리한 최근 HTTP 요청 추적
- 메모리 사용량, garbage collection, 웹 요청 및 데이터 소스 사용량과 관련된 다양한 메트릭

Actuator의 정보는 웹의 endpoint 또는 shell interface를 통해 접근할 수 있다. 후자의 경우 애플리케이션에 SSH (secure shell)을 열고, 실행 중인 애플리케이션을 검사하는 명령을 실행할 수 있다.

## 4️⃣**THE COMMAND-LINE INTERFACE (CLI)**

CLI를 통해 애플리케이션을 빠르게 작성할 수 있다. starter dependencies와 자동 구성을 활용하여 코드 작성에 집중할 수 있도록 한다. 

```java
@RestController
class HelloController {

		  @RequestMapping("/")
		  def hello() {
				    return "Hello World"
		  }
}
```

```java
$ spring run HelloController.groovy
```

위의 코드를 CLI를 이용해 아래의 명령문으로 바로 실행시킬 수 있다. (Groovy 언어를 사용하였지만 Java도 가능하다.)

코드에 import 구문이 없어도 작동하는 과정은 아래와 같다. 

1) CLI가 유형을 통해 알아서 필요한 starter dependency를 classpath에 추가한다.

2) auto-configuration이 시작된다.

3) DispatcherServlet과 Spring MVC를 활성화시킨다.

4) 컨트롤러가 HTTP 요청에 응답할 수 있게 된다.

장점은 간단히 파일럿 프로젝트를 실행시킬 수 있다?

## 🚫오해

### 1. Spring Boot는 애플리케이션 서버가 아니다.

이러한 오해는 기존 Java 애플리케이션 서버에 애플리케이션을 배포하지 않고도 명령어로 실행할 수 있는 자체 실행 가능한 JAR 파일로 웹 애플리케이션을 생성할 수 있다는 점에서 비롯된다. Spring Boot는 애플리케이션 내에 서블릿 컨테이너 (Tomcat, Jetty 또는 Undertow)를 내장하여 이를 가능하게 한다. 그러나 엄밀히 말하면 이는 Spring Boot 자체가 아니라 애플리케이션 서버 기능을 제공하는 임베디드 서블릿 컨테이너이다. 

### 2. Spring Boot는 JPA 또는 JMS와 같은 엔터프라이즈 Java 사양을 구현하지 않는다.

여러 엔터프라이즈 Java 사양을 지원하지만, Spring Boot는 이러한 기능을 지원하는 Spring에서 빈을 자동으로 구성하는 역할을 한다. 예를 들어, Spring Boot는 JPA를 구현하지 않지만 JPA 구현 (예, Hibernate)에 적합한 빈을 자동 구성하여 JPA를 지원한다.

## 🔖요약

Spring Boot의 핵심은 Spring이다. Spring Boot는 사용자가 직접 수행할 수 있는 것과 동일한 종류의 bean 구성을 Spring에서 수행한다. Spring Boot를 이용해 명시적인 `boilerplate configuraion`을 처리할 필요가 없으며, 애플리케이션 로직에 집중할 수 있게 한다.

# 예상 질문

1. ‘spring’과 ‘spring boot’의 차이점은?
    
    스프링 프레임워크란 자바에서 가장 많이 사용되는 프레임워크이다. **자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크이다.** 의존성 주입 (DI)와 제어 역전 (IoC, Inversion of Control), 관점 지향 프로그래밍 (AOP)이 가장 중요한 요소이다.
    
    스프링부트란 자동 구성, 임베디드 서블릿 컨테이너, 스타터 종속성, Actuator 및 Spring Boot CLI와 같은 다양한 기능을 도입하여 스프링 기반 어플리케이션 개발을 더욱 쉽게 만듭니다.
    
2. dependency 중 ‘spring-boot-starter’는 무엇인가요?
3. 스프링부트의 자동 설정 auto-configuration이란 무엇인가요?
4. spring boot에서 제공하는 어노테이션은 무엇이 있나요?
    
    `@SpringBootApplication`, `@EnableAutoConfiguration`이 있다.
    
    - `@SpringBootApplication`
        
        하나 이상의 @Bean 메서드를 선언하고 `[auto-configuration](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/autoconfigure/EnableAutoConfiguration.html)` 및 `[component scanning](https://docs.spring.io/spring-framework/docs/6.0.8/javadoc-api/org/springframework/context/annotation/ComponentScan.html)`을 트리거하는 구성 클래스를 나타냅니다.
        
        `@Configuration`, `@EnableAutoConfiguration` and `@ComponentScan`을 선언하는 것과 같다.
        
        - `@ComponentScan` : **@component** 어노테이션 **@Service**, **@Repository**, **@Controller** 등의 어노테이션을 스캔하여 Bean으로 등록해주는 어노테이션입니다
        - `@****EnableAutoConfiguration` :**
            
            @EnableAutoConfiguration은 사전에 정의한 라이브러리들을  Bean으로 등록해 주는 어노테이션입니다.
            
            사전에 정의한 라이브러리들 모두가 등록되는 것은 아니고 특정 Condition(조건)이 만족될 경우에 Bean으로 등록합니다.
            
        
        ```java
        @Target(TYPE)
        @Retention(RUNTIME)
        @Documented
        @Inherited
        @SpringBootConfiguration
        @EnableAutoConfiguration
        @ComponentScan(excludeFilters={@Filter(type=CUSTOM,classes=TypeExcludeFilter.class),})
        public @interface SpringBootApplication {}
        ```
        
    - `@EnableAutoConfiguration`