# Web MVC 요청 처리 과정(DispatcherServlet을 중심으로)
<aside>
💡 스프링 웹 MVC 프레임워크는 DispatcherServlet을 중심으로 설계되었다.

</aside>

## Front Controller의 등장

<aside>
💡 기존 MVC 패턴이 지니고 있던 단점들을 극복하기 위해 발전된 구조

</aside>

- 모든 클라이언트의 요청이 단일 진입점인 FrontController로 집중
- 컨트롤러에서 중복으로 처리해야하는 공통 기능들을 처리하는 입구 역할을 하는 컨트롤러를 두는 개념

### ➡️ Front Controller 도입 전

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f44ffdc7-d739-4169-b111-6faef5626916/Untitled.png)

### ➡️ Front Controller 도입 후

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0e68ba2b-7e46-44da-b068-d90aecdfc527/Untitled.png)

- 하나의 프론트 컨트롤러 서블릿으로 다수의 클라이언트 요청을 받을 수 있고, 다양한 HTTP 요청에 대해 공통 처리가 가능하여 코드 재사용성과 유연성을 높이며 중복을 줄임
- 중앙 집중식 제어를 통해 보안 강화, 사용자 트랙킹에도 유리

# DispatcherServlet이란?

<aside>
💡 HTTP 요청들을 매핑된 컨트롤러로 배차해주는 역할을 수행하는 중앙 서블릿
웹 어플리케이션에서 Front Controller 패턴을 구현하는 방식 중 하나
Spring MVC의 Front-Controller = DispatcherServlet

</aside>

## DispatcherServlet의 요청 처리 단계

1. 클라이언트로부터 HTTP 요청이 도착하면 서블릿 컨테이너는 DispatcherServlet에 해당 요청 전달
2. DispatcherServlet은 HandlerMapping을 사용하여 요청을 처리할 핸들러(컨트롤러)를 결정
3. DispatcherServlet은 HandlerAdapter를 사용하여 결정된 핸들러를 실행
4. DispatcherServlet은 ViewResolver를 사용하여 처리 결과를 적절한 뷰로 변환
5. DispatcherServlet은 변환된 뷰를 사용하여 클라이언트에게 응답을 생성

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/370112af-2e6b-4cd7-8818-6d6df652ca84/Untitled.png)

## 예상 질문
1. Spring MVC에서 DispatcherServlet 의 역할에 대해 설명해 주세요.
2. DispatcherServlet의 요청 처리 단계를 설명해주세요.
