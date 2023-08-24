## ViewResolver

---

클라이언트 요청에 대한 응답으로 생성되는 뷰(View)를 결정하는 역할

클라이언트에게 보여질 화면을 생성하기 위해 Controller가 처리한 결과를

어떤 뷰 템플릿과 결합하여 HTML, XML, JSON 등의 형태로 출력할 지 결정

### ViewResolver의 동작 방식

1. Controller 처리
- 클라이언트의 요청이 컨트롤러로 전달
- Controller는 데이터를 가공하여 Model 객체에 전달
1. Model 객체 준비
- Controller가 가공한 데이터는 Model 객체에 Key-Value 형태로 저장
1. View 이름 반환
- Controller가 처리 한 결과를 바탕으로 어떤 뷰를 사용할지 결정
- **이 때 ViewResolver에 의해 실제 사용될 뷰의 이름을 반환**
1. View 검색
- ViewResolver는 반환된 뷰 이름을 기반으로 실제 뷰를 찾음
1. View 반환
- ViewResolver는 해당 뷰를 반환하여 Controller의 처리 결과와 Model 객체를 뷰에 전달
1. View 렌더링
- 뷰는 전달받은 Model 객체와 컨트롤러의 처리 결과를 활용하여 클라이언트에게 보여질 결과 생성

### Spring MVC의 처리 과정

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/06615a11-46ad-4b0a-9561-7d22c7e23576/Untitled.png)

1. Dispatcher Servlet이 클라이언트에게 HTTP요청을 받음
2. Dispatcher Servlet은 HandlerMapping을 이용해 클라이언트 요청을 어떤 컨트롤러가 처리할지 결정
- 핸들러 어댑터 목록에서 조회
1. Controller를 실행하고 데이터를 가공하여 Model 객체에 담음
2. Controller가 처리한 결과를 바탕으로 어떤 뷰를 사용할지 결정하기 위해 **ViewResolver** 이용
3. ViewResolver는 컨트롤러의 처리 결과와 Model 객체를 기반으로 최종적으로 보여질 View를 찾음
4. View를 렌더링한 후 클라이언트에게 전송

### 면접질문

1. ViewResolver가 어떤건지 설명해주세요

ViewResolver는 Spring MVC에서 클라이언트 요청에 대한 응답으로 생성되는 뷰(View)를 결정하는 역할을 합니다. 컨트롤러에서 처리한 결과와 Model 객체를 기반으로 실제 View를 찾아 View를 렌더링하여 클라이언트에게 전달합니다.

[](https://ittrue.tistory.com/237)

[어노테이션 기반 MVC 프레임워크 만들기](https://hudi.blog/implementing-annotation-based-mvc-framework/)
