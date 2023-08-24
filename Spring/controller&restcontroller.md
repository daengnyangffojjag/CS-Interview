# @Controller
- 전통적인 Spring MVC의 컨트롤러인 @Controller는 주로 View를 반환하기 위해 사용합니다.

![](https://velog.velcdn.com/images/tkdtkd97/post/194ef34f-8b35-46fc-ad2a-bd047a2b4a59/image.png)
1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. DispatcherServlet이 요청을 처리할 대상을 찾는다.
3. HandlerAdapter을 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후에 ViewName을 반환한다.
5. DispatcherServlet은 ViewResolver를 통해 ViewName에 해당하는 View를 찾아 사용자에게 반환한다.

### Controller로 Data 반환하기 = RestController
-  Spring MVC의 컨트롤러를 사용하면서 Data를 반환해야 하는 경우도 있습니다.
- 컨트롤러에서는 데이터를 반환하기 위해 @ResponseBody 어노테이션을 활용해주어야 합니다. 이를 통해 Controller도 Json 형태로 데이터를 반환할 수 있습니다.
  ![](https://velog.velcdn.com/images/tkdtkd97/post/9e1aca3c-dd96-4ea7-9a05-c38e61f18a1b/image.png)
1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. DispatcherServlet이 요청을 처리할 대상을 찾는다.
3. HandlerAdapter을 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후에 객체를 반환한다.
5. 반환되는 객체는 Json으로 Serialize되어 사용자에게 반환된다.


- 컨트롤러를 통해 객체를 반환할 때에는 일반적으로 ResponseEntity로 감싸서 반환을 합니다.
- 그리고 객체를 반환하기 위해서는 viewResolver 대신에 HttpMessageConverter가 동작합니다.
- HttpMessageConverter에는 여러 Converter가 등록되어 있고, 반환해야 하는 데이터에 따라 사용되는 Converter가 달라집니다.
- 단순 문자열인 경우에는 StringHttpMessageConverter가 사용되고, 객체인 경우에는 MappingJackson2HttpMessageConverter가 사용되며, 데이터 종류에 따라 서로 다른 MessageConverter가 작동하게 됩니다.
- Spring은 클라이언트의 HTTP Accept 헤더와 서버의 컨트롤러 반환 타입 정보 둘을 조합해 적합한 HttpMessageConverter를 선택하여 이를 처리합니다.
- MessageConverter가 동작하는 시점은 HandlerAdapter와 Controller가 요청을 주고 받는 시점이다. - 그림의 4번에서는 메세지를 객체로, 6번에서는 객체를 메세지로 변환하는데 메세지 컨버터가 사용된다.

### Controller 예제
```java
@Controller
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @GetMapping(value = "/users")
    public @ResponseBody ResponseEntity<User> findUser(@RequestParam("userName") String userName){
        return ResponseEntity.ok(userService.findUser(user));
    }
    
    @GetMapping(value = "/users/detailView")
    public String detailView(Model model, @RequestParam("userName") String userName){
        User user = userService.findUser(userName);
        model.addAttribute("user", user);
        return "/users/detailView";
    }
}
```
- 위 예제의 findUser는 User 객체를 ResponseEntity로 감싸서 반환하고 있고, User를 json으로 반환하기 위해 @ResponseBody라는 어노테이션을 붙여주고 있습니다.
- detailView 함수에서는 View를 전달해주고 있기 때문에 String을 반환값으로 설정해주었습니다. (물론 이렇게 데이터를 반환하는 RestController와 View를 반환하는 Controller를 분리하여 작성하는 것이 좋습니다.)

# @RestController
- @RestController는 @Controller에 @ResponseBody가 추가된 것입니다.
- 당연하게도 RestController의 주용도는 Json 형태로 객체 데이터를 반환하는 것입니다.
- 최근에 데이터를 응답으로 제공하는 REST API를 개발할 때 주로 사용하며 객체를 ResponseEntity로 감싸서 반환합니다.
- 이러한 이유로 동작 과정 역시 @Controller에 @ReponseBody를 붙인 것과 완벽히 동일합니다.

![](https://velog.velcdn.com/images/tkdtkd97/post/f863e3fc-9993-4397-9e3b-1d16a52bb287/image.png)
1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. DispatcherServlet이 요청을 처리할 대상을 찾는다.
3. HandlerAdapter을 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후에 객체를 반환한다.
5. 반환되는 객체는 Json으로 Serialize되어 사용자에게 반환된다.

### @RestController 예제 코드
```java
@RestController
@RequestMapping("/user")
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @GetMapping(value = "/users")
    public User findUser(@RequestParam("userName") String userName){
        return userService.findUser(user);
    }

    @GetMapping(value = "/users")
    public ResponseEntity<User> findUserWithResponseEntity(@RequestParam("userName") String userName){
        return ResponseEntity.ok(userService.findUser(user));
    }
}
```

- findUser는 User 객체를 그대로 반환하고 있습니다.
- 이러한 경우의 문제는 클라이언트가 예상하는 HttpStatus를 설정해줄 수 없다는 것입니다.
- 예를 들어 어떤 객체의 생성 요청이라면 201 CREATED를 기대할 것이지만 객체를 그대로 반환하면 HttpStatus를 설정해줄 수 없습니다.
- 그래서 객체를 상황에 맞는 ResponseEntity로 감싸서 반환해주어야 합니다.

# 참고
https://mangkyu.tistory.com/49

# 면접 예상 질문
> @Controller 와 @RestController의 차이에 대해 설명하세요

@Controller는 주로 View를 반환하기 위해 사용합니다. 뷰리졸버를 통해 View를 찾아 랜더링 할 수 있습니다

데이터를 반환하고 싶으면 @ResponseBody 어노테이션을 활용하여 Json 형태로 데이터를 반환할 수 있습니다.
@RestController는 2개의 어노테이션이 합친 것으로, json형태로 객체 데이터를 반환해줍니다.
 