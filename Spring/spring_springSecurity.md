### Spring Security란?
<hr/>

인증, 인가, 보안기능을 담당하는 스프링 하위 프레임워크이다.
Spring Security는 '인증'과 '권한'에 대한 부분을 Filter 흐름에 따라 처리하고 있다.
ex. 로그인, 권한설정, 권한검사

서블릿 필터로 구현되어있다.
- api를 호출할때 흐름
  ![](https://velog.velcdn.com/images/tkdtkd97/post/8a39c77f-b779-4907-b359-92fa7a927f4d/image.png)
  ![](https://velog.velcdn.com/images/tkdtkd97/post/d1e9eed4-5684-4799-8d49-0ffce3ceebe5/image.png)
  서블릿 컨테이너는 그 사이 필터로 개입을 할 수 있다. 여기에 security filter도 넣어 보안기능을 수행한다.
  <br/><br/>
   하지만 서블릿 컨테이너로 필터를 만들면 이 필터자체가 스프링 빈을 이해하지 못한다. 그래서 스프링에 위임한다.
![](https://velog.velcdn.com/images/tkdtkd97/post/81ddc2db-25fc-4968-8e8c-27215565b7ae/image.png)
  <br/><br/>
   그걸 따라서 스프링은 security filter chain을 만든
![](https://velog.velcdn.com/images/tkdtkd97/post/8db6d8d7-16f7-4c92-bf92-9de53f5f46fe/image.png)

   ![](https://velog.velcdn.com/images/tkdtkd97/post/aab0dd80-b19d-42df-ba9b-0b6e7070886d/image.png)
그래서 여기에는 여러 필터들이 있다.
이 필터들의 묶음을 필터체인이라 부르고 각 필터들은 고유한 기능이 있다.
기본적으로 설정하지 않아도 default로 등록되는 필터가 존재한다.
[공식문서 - 보안필터](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-securityfilterchain)

- security를 구현하는 방법은 다양하다.
  <br/><br/>
필터자체를 이해해서 필터를 구현할 것인지, 함수나 메서드를 오버라이딩해서 일부흐름만 커스터마이징할 것인지 구현방법은 사용자가 결정하기 나름이다.

### WebSecurityConfigurerAdapter vs SecurityFilterChain
<hr/>

- WebSecurityConfigureAdapter를 사용한 방식
```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
 
    @Bean
    public UserDetailsService userDetailsService() {
        return new ShopmeUserDetailsService();
    }
 
    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
     
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests().antMatchers("/login").permitAll()
                .antMatchers("/users/**", "/settings/**").hasAuthority("Admin")
                .hasAnyAuthority("Admin", "Editor", "Salesperson")
                .hasAnyAuthority("Admin", "Editor", "Salesperson", "Shipper")
                .anyRequest().authenticated()
                .and().formLogin()
                .loginPage("/login")
                    .usernameParameter("email")
                    .permitAll()
                .and()
                .rememberMe().key("AbcdEfghIjklmNopQrsTuvXyz_0123456789")
                .and()
                .logout().permitAll();
 
        http.headers().frameOptions().sameOrigin();
    }
     
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers("/images/**", "/js/**", "/webjars/**"); 
    }
}
```

- SecurityFilterChain를 사용한 방식
```java
@Configuration
public class SecurityConfiguration {
 
    @Bean
    public UserDetailsService userDetailsService() {
        return new ShopmeUserDetailsService();
    }
 
    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
 
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
     
        http.authorizeRequests().antMatchers("/login").permitAll()
                .antMatchers("/users/**", "/settings/**").hasAuthority("Admin")
                .hasAnyAuthority("Admin", "Editor", "Salesperson")
                .hasAnyAuthority("Admin", "Editor", "Salesperson", "Shipper")
                .anyRequest().authenticated()
                .and().formLogin()
                .loginPage("/login")
                    .usernameParameter("email")
                    .permitAll()
                .and()
                .rememberMe().key("AbcdEfghIjklmNopQrsTuvXyz_0123456789")
                .and()
                .logout().permitAll();
 
        http.headers().frameOptions().sameOrigin();
 
        return http.build();
    }
 
    @Bean
    public WebSecurityCustomizer webSecurityCustomizer() {
        return (web) -> web.ignoring().antMatchers("/images/**", "/js/**", "/webjars/**");
    }
 
}
```

[WebSecurityConfigurerAdapter가 없는 스프링 보안](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter)
[해당 버전확인](https://codejava.net/frameworks/spring-boot/fix-websecurityconfigureradapter-deprecated)

### HttpSecurity
<hr/>

HttpSecurity는 인증, 인가의 세부적인 기능을 설정할 수 있도록 API를 제공해주는 클래스이다.
[공식문서 HttpSecurity 사용법](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/config/annotation/web/builders/HttpSecurity.html)

#### 댕냥에 사용된 기능
- .httpBasic() : 사용자 인증방법으로는 HTTP Basic Authentication을 사용한다.
  [HTTP Basic Authentication](https://velog.io/@code12/Spring-Security-HTTP-Basic-Authentication)
- .csrf()
  [csrf란?](https://itstory.tk/entry/CSRF-%EA%B3%B5%EA%B2%A9%EC%9D%B4%EB%9E%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-CSRF-%EB%B0%A9%EC%96%B4-%EB%B0%A9%EB%B2%95)
  [JWT를 사용하면 .csrf().disable()해도 될까?](https://hongguri.tistory.com/40)

- .cos()란? CorsFilter사용할 를 추가합니다.
  .authorizeHttpRequests() : 특정 리소스의 접근 허용 또는 특정 권한을 가진 사용자만 접근을 가능하게 할 수 있습니다.
  [AuthorizeRequests vs AuthorizeHttpRequests](https://whatistudy.tistory.com/entry/%EC%B6%94%EA%B0%80-AuthorizeRequests-vs-AuthorizeHttpRequests)
  .permitAll() : 설정한 리소스의 접근을 인증절차 없이 허용한다는 의미 입니다.

- [.exceptionHandling()](https://velog.io/@gwichanlee/Spring-Security-ExceptionHandling)
    - AccessDeniedHandler
        - 인가 예외 처리를 담당
        - 서버에 요청을 할 때 액세스가 가능한지 권한을 체크후 액세스 할 수 없는 요청을 했을시 동작함

    - AuthenticationEntryPoint
        - 인증이 되지않은 유저가 요청을 했을때 동작함

- .sessionManagement() : 세션 관리 기능이 작동함
  .sessionCreationPolicy(SessionCreationPolicy.STATELESS) : 스프링 시큐리티가 생성하지도 않고 존재해도 사용하지 않음
  SessionCreationPolicy.STATELESS 로 세션정책을 설정한다는것은 인증 처리 관점에서 스프링 시큐리티가 더 이상 세션쿠키 방식의 인증 메카니즘으로 인증처리를 하지 않겠다는 의미로 볼 수 있습니다.
  [댕냥은 세션쿠키방식의 인증은 안하는걸까?](https://www.inflearn.com/questions/34886/jsessionid%EC%9D%98-%EC%83%9D%EC%84%B1%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C)

- .addFilterBefore() : 지정된 필터 앞에 커스텀 필터를 추가
  인증을 처리하는 기본필터 UsernamePasswordAuthenticationFilter 대신 별도의 인증 로직을 가진 필터를 생성하고 사용하고 싶을 때 아래와 같이 필터를 등록하고 사용합니다.

- [.formLogin()](https://kimchanjung.github.io/programming/2020/07/02/spring-security-02/) : 로그인 페이지와 기타 로그인 처리 및 성공 실패 처리를 사용하겠다는 의미 입니다.

- 댕냥에 admin 도전과제로 해도 좋을듯

### Spring Security 모듈
<hr/>

[2번 참고](https://mangkyu.tistory.com/76)

### Spring Security 처리 과정
<hr/>

![](https://velog.velcdn.com/images/tkdtkd97/post/e4bc7bf5-f263-4b0d-8e29-302515d4162e/image.png)
[Session 사용](https://mangkyu.tistory.com/77)

[JWT 사용](https://mangkyu.tistory.com/57)


### 참고
<hr/>

https://www.youtube.com/watch?v=ewslpCROKXY
[인프런 정수원 -springSecurity 유료강의 요약본(이건 그냥 첨부)](https://velog.io/@eldehd9898/Spring-Security-%EC%A0%95%EC%88%98%EC%9B%90-8%ED%9A%8C%EC%B0%A8)

### 면접 예상질문
<hr/>

#### 1. Spring Security란 무엇인가요?

- Spring 기반의 애플리케이션의 보안(인증과 권한, 인가 등)을 담당하는 스프링 하위 프레임워크
  <br/><br/>
- '인증'과 '권한'에 대한 부분을 Filter 흐름에 따라 처리
  <br/><br/>
- Spring Security는 보안과 관련해서 체계적으로 많은 옵션을 제공해주기 때문에 개발자 입장에서는 일일이 보안관련 로직을 작성하지 않아도 된다는 장점이 있다.

#### 2. csrf란 무엇인가요?
- 웹 어플리케이션 취약점 중 하나로, 인터넷 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위 (modify, delete, register 등)를 특정한 웹사이트에 request하도록 만드는 공격을 말한다.
  <br/>
<br/>
주로 해커들이 많이 이용하는 것으로, 유저의 권한을 도용해 중요한 기능을 실행하도록 한다.
  <br/><br/>
우리가 실생활에서 CSRF 공격을 볼 수 있는 건, 해커가 사용자의 SNS 계정으로 광고성 글을 올리는 것이다.
<br/><br/>
정확히 말하면, CSRF는 해커가 사용자 컴퓨터를 감염시거나 서버를 해킹해서 공격하는 것이 아니다. CSRF 공격은 아래와 같은 조건이 만족할 때 실행된다.
  <br/><br/>
- 사용자가 해커가 만든 피싱 사이트에 접속한 경우
- 위조 요청을 전송하는 서비스에 사용자가 로그인을 한 상황
  <br/><br/>
보통 자동 로그인을 해둔 경우에 이런 피싱 사이트에 접속하게 되면서 피해를 입는 경우가 많다.
<br/><br/>
- 꼬리질문 : 프로젝트에서 csrf()기능을 꺼놓은 이유가 무엇인가요?

#### 3. Filter와 Security Filter의 차이는 무엇인가요?
- Filter와 Security Filter 모두 Servlet에 요청이 맵핑되기 전에 실행되는 필터(Filter)입니다.
  <br/><br/>
둘다 동일한 Filter이지만 단순한 Filter는 서블릿 컨테이너에 직접 등록해서 사용하는 필터이고 Security Filter는 DelegatingFilterProxy가 서블릿 컨테이너에 Filter로 등록되어서 Filter 작업을 Security FilterChain으로 위임해서 실행되는 필터를 의미합니다.
#### 4. 인증을 위해서 어떻게 Spring Security를 사용하셨나요?
- Spring Security를 사용해서 인증을 구현하기 위해 JWT를 이용한 인증방식을 선택하였고 이를 사용해서 Filter 작업을 수행할 JwtTokenFilter를 만들었습니다.
<br/><br/>
 JWT 인증 필터가 기본 인증 필터인 UsernamePasswordAuthenticationFilter 이전에 수행할 수 있도록 Security Filter Chain 설정을 통해서 인증을 구현할 수 있었습니다.