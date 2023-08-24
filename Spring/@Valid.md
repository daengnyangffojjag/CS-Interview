## @Valid

---

Spring에서 데이터 전송 객체(DTO)의 검증을 수행하기 위해 사용

- API 요청에 들어있는 데이터를 DTO 객체로 바인딩할 때, 해당 데이터가 유효한지 검증
- JSR-303(Bean Validation)에서 정의된 검증 규칙에 따라 검증
- 클라이언트 검증은 보안에 취약하기 때문에 **서버에서 잘못된 요청을 검증**하기 위해 사용

### @Valid 검증 시점

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1fc5d247-c16c-4103-9d86-86d122ba844f/Untitled.png)

1. 클라이언트가 HTTP 요청을 보냄
2. DispatcherServlet 동작
3. DispatcherServlet에 의해 HandlerAdapter 선택
4. RequestMappingHandlerAdapter가 @RequestMapping이 적용된 메소드를 처리하기 위해 사용
5. RequestMappingHandlerAdapter가 요청을 처리할 Controller의 메소드를 호출
6. 메소드의 파라미터를 해석하기 위해 ArgumentResolver 사용
7. 메소드의 파라미터중 @RequestBody가 적용된 파라미터에 **@Valid**가 함께 사용되어 있다면, ArgumentResolver가 해당 파라미터의 검증을 수행
8. 검증이 성공한 경우엔 메소드가 정상적으로 실행되고, 해당 과정에 예외가 발생하면 **MethodArgumentNotValidException** 발생

### Annotation 종류

- `@AssertFalse` : false 값 통과
- `@AssertTrue` : true 값 통과
- `@DecimalMax(value=, inclusive=)` : 지정된 값 이하의 실수 통과
- `@DecimalMin(value=, inclusive=)` : 지정된 값 이상의 실수 통과
- `@Digits(integer=, fraction=)` : 대상 수가 지정된 정수와 소수 자릿수보다 적을 경우 통과
- `@Email` : 이메일 형식 통과
- `@Future` : 대상 날짜가 현재보다 미래일 경우만 통과
- `@Past` : 대상 날짜가 현재보다 과거일 경우만 통과
- `@Max(value=)` : 지정된 값보다 아래일 경우만 통과
- `@Min(value)` : 지정된 값보다 이상일 경우만 통과
- `@NotNull` : null 값이 아닐 경우만 통과
- `@NotEmpty` : null, "" 이 아닌 경우 통과
- `@NotBlank` : null, "", " " 이 아닌 경우 통과
- `@Null` : null일 겨우만 통과
- `@Pattern(regex=, flag=, message=)` : 해당 정규식을 만족할 경우만 통과
- `@Size(min=, max=)` : 문자열 또는 배열이 지정된 값 사이일 경우 통과

### 예외 메시지 담기

---

message 옵션 값에 메시지 설정

```java
public class BookDTO {
    @NotBlank(message = "이름은 빈칸일 수 없습니다")
    @Size(max = 30, message = "이름은 30자 이하로 입력해주세요")
    private String name;
   
}
```

### 면접질문

1. @Valid 어노테이션은 무엇이고 언제 사용하나요?

@Valid는 Spring에서 DTO의 검증을 위해 사용합니다. Controller의 메소드 파라미터에 적용하여 클라이언트의 요청 데이터가 정상인지 확인하고, 예외가 발생할 경우 에러 처리를 할 수 있습니다.

1. @Valid와 @RequestParam, @PathVariable, @RequestBody등의 차이는 무엇인가요?

@Valid 어노테이션은 요청 데이터에 대한 유효성을 검증하는데 사용하고 나머지 어노테이션은 요청 데이터를 컨트롤러 메소드의 파라미터로 변환하여 활용하는데 사용합니다.

[[Spring] Validation Annotation이란? + DTO에서는 어디까지 검증해야할까?](https://seongwon.dev/Spring-MVC/20220622-Valid란/)

[](https://github.com/devSquad-study/2023-CS-Study/blob/main/Spring/spring_@valid.md)
