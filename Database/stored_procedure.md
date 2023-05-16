# Stored Procedure?
- 여러 SQL 문을 하나의 SQL 문처럼 정리하여 CALL ✕ ✕라는 명령으로 실행할 수 있게 만든 것을 저장 프로시저(Stored Procedure)라고 합니다.
- 사용자가 정의한 프로시저
- RDBMS에 저장되고 사용되는 프로시저
- 구체적인 하나의 task를 수행한다.
  ![](https://velog.velcdn.com/images/tkdtkd97/post/c0374d15-99a8-41b5-a0ef-454cd27def57/image.png)


### Stored function 예제 1
두 정수의 곱셉 결과를 가져오는 프로시저를 작성하자

delimiter $$ : 저장 프로시저 시작
$ $ : 끝표시
IN : 인풋 파라미터 (안쓰면 IN으로 디폴트)
OUT : 아웃풋 파라미터 (프로시저 결과를 가져오는 역할)

call + 프로시저 이름(1, 2, @result); => 프로시저 실행
@ = 사용자가 정의한 변수
![](https://velog.velcdn.com/images/tkdtkd97/post/296e1877-2001-4dfa-aaa1-99ea4d7a699c/image.png)

### Stored function 예제 2
두 정수를 맞바꾸는 프로시저를 작성하자
INOUT = 값을 파라미터로 전달받을수도 최종적으로 값을 반환할 수도 있다.
![](https://velog.velcdn.com/images/tkdtkd97/post/1593845f-d887-435c-864a-c9d08fe71b3f/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/e2b87714-81fb-41cf-95f4-f7f100e4d903/image.png)

### Stored function 예제 3
각 부서별 평균 연봉을 가져오는 프로시저를 작성하자
![](https://velog.velcdn.com/images/tkdtkd97/post/cd628980-3cb1-4e96-a678-6b99e19d6a9c/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/9aea7d6c-e5f0-4ba0-aedd-0d3e1211680d/image.png)

### Stored function 예제 4
사용자가 프로필 닉네임을 바꾸면 이전 닉네임을 로그에 저장하고 새 닉네임으로 업데이트하는 프로시저를 작성하자
![](https://velog.velcdn.com/images/tkdtkd97/post/2bac5fe1-0b58-4149-b02a-b54d7d5e1cba/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/3b96ca53-2236-46c6-b42b-f50b92fd2982/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/4df300d3-2df9-4c61-bcef-3a1dbe4c3a46/image.png)

### Stored procedure 활용
- 이외에도 조건문을 통해 분기처리를 하거나
- 반복문을 수행하거나
- 에러를 핸들링하거나 에러를 일으키는 등의 다양한 로직을 정의할 수 있다.

### Stored procedure과 Stored function의 차이
* 전제 : mysql, oracle, ms sql server, pstageSQL의 공통적인부분임
  ![](https://velog.velcdn.com/images/tkdtkd97/post/d8dca0fc-9b4d-4d89-9874-e7dc81b5dc01/image.png)
  ![](https://velog.velcdn.com/images/tkdtkd97/post/983a68ea-00a6-4672-b363-a2aaa3b915c9/image.png)
  ![](https://velog.velcdn.com/images/tkdtkd97/post/050ea305-0c0f-41d4-9d2e-b9ab0c52f5d1/image.png)
  트랜잭션 사용 : 여러개의 sql문들을 하나로 묶어서 사용을 하고 싶을때 그 여러 sql문들이 성공을 하면 그 결과는 반영하고 아니라면 아무일없던것처럼 하는것
  ![](https://velog.velcdn.com/images/tkdtkd97/post/5f559665-be5e-422d-9b3e-7c23b3851343/image.png)

이외에도
- 다른 function/procedure를 호출할 수 있는지
- resultset(=table)을 반환할 수 있는지
- precomplied execution plan을 만드는지
- try-catch를 사용할 수 있는지

### 3-tier architecture에서 stored procedure의 의미
오늘날의 IT회사들은 일반적으로 client-server architecture의 한 종류인 tree-tier architecure 모델로 서비스를 개발한다.

![](https://velog.velcdn.com/images/tkdtkd97/post/51f2df16-781f-4f3e-8ee2-c1283250733d/image.png)
로직 티어 : 사용자로부터 받은 요청을 처리함
로직티어에서 비즈니스 로직을 담당하는데 여러개의 웹어플리케이션 서버를 띄우게 된다.
![](https://velog.velcdn.com/images/tkdtkd97/post/efaf4946-68cc-4148-bfb9-90326994245a/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/fcd40da5-82ac-426a-b260-757042196cea/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/2145e6b6-df45-4da5-b800-788969524f02/image.png)
![](https://velog.velcdn.com/images/tkdtkd97/post/eee58a65-2f25-4186-a43a-bede42966040/image.png)
stored procedure를 사용한다는것은 로직티어에도 데이터티어에도 비즈니스 로직이 존재할 수 있다는 것이다.

### Stored procedure의 장점
1. application에 transparent하다

로직티어에 4개의 자바 스프링을 사용하는 웹어플리케이션 서버가 있다. 데이터티어에는 mysql하나로 가동한다고 가정한다.(실무에서는 여러대임)

로직티어에 소스코드로 비즈니스로직을 관리한다면
4개의 인스턴스들은 같은 코드를 기반으로 동작을 하기 때문에 같은 소스코드 기반으로 동작을 한다.
![](https://velog.velcdn.com/images/tkdtkd97/post/f2a618c8-792f-4f0e-b80b-5ac759f64e42/image.png)

수정을 해야한다면 코드 수정하고 배포를 해줘야한다. 컴파일하고 배포파일 만들고 기존의 인스턴스를 내리고 새로운 인스턴스를 올려야한다.

그작업을 하려면 트래픽을 계속해서 받고 있기 때문에 동시에 바꿔주면 안된다. 하나씩 재기동을 해줘야한다.
수정이 있을때마다 하나하나 작업하는데 번거로움이 있을 수 있다.

하지만 그 비즈니스 로직이 프로시저로 관리가 된다면 새로운 로직으로 바꿔줄때 어떻게 해야할까
프로시저로 비즈니스 로직을 관리하면 로직티어에서는 프로시저를 호출하는 형태이다.
![](https://velog.velcdn.com/images/tkdtkd97/post/0d7b22de-11e2-4b33-b40c-013f3bc3b21d/image.png)

프로시저의 이름이나 파라미터 말고 바디부분만 수정이 있다면 그냥 프로시저만 바꿔주면 로직티어에 적용된다.

2. network traffic을 줄여서 응답 속도를 향상시킬 수 있다.

로직티어에서 비즈니스 로직을 담당하고 있다면
select를 하면 네트워크 트래픽이 생긴다.
![](https://velog.velcdn.com/images/tkdtkd97/post/6f8b81db-8e52-4d7a-94c0-4549510fc914/image.png)

비즈니스 로직이 프로시저에 있다면 네트워크 트래픽이 줄어 응답속도가 줄어든다.
![](https://velog.velcdn.com/images/tkdtkd97/post/b9ea94fc-cb7b-41e8-ae6c-3df1016ab5bc/image.png)

3. 여러 서비스에서 재사용이 가능하다.

DB를 사용하는 서버가 3개있다.
![](https://velog.velcdn.com/images/tkdtkd97/post/07416b57-063c-4337-a1b7-bd968e42527b/image.png)
로직티어에서 비즈니스로직을 구현한다면 각각 다른 언어로 똑같이 구현한다.

하지만 프로시저에서 로직을 관리하면 각 서버에서는 호출만 하면 되기 때문에 번거롭게 각자 구현하지 않아도 된다.
![](https://velog.velcdn.com/images/tkdtkd97/post/0b08463a-138a-44aa-a8e4-12a8189778c3/image.png)

4. 민감한 정보에 대한 접근을 제한할 수 있다.

회사에서 개인정보등 민감한 정보는 개발자의 접근에 우선 제한을 둔다.
이때 프로시저를 통해 DB에 접근하도록하여 프로시저를 통해서 로직을 작성하도록한다.
직접적인 접근은 막고 프로시저를 통해 개발을 하게 하는것
![](https://velog.velcdn.com/images/tkdtkd97/post/fa985990-6e56-41d8-9620-47ecf1b509c5/image.png)

### Stored procedure 단점과 실무에서 쓰기 조심스러울 수 있는 주관적 의견
1. 유지 관리 보수 비용이 커진다.
   ![](https://velog.velcdn.com/images/tkdtkd97/post/6f198578-9055-4262-b669-047524556dd1/image.png)
   일부는 소스코드상에 구현되어있고 일부는 프로시저에서 구현되어있다보니까
   버전관리를 둘 다 해줘야한다.
   왔다갔다봐야하니 가독성이 떨어진다.
   프로시저관련 문법을 알아야한다.

2. DB서버를 추가하는 것은 간단한 작업이 아니다.

로직티어에 온 트래픽이 db까지 이어진다. 그러면 RDBMS의 cpu사용량이 웹어플리케이션 서버보다 많아진다.
![](https://velog.velcdn.com/images/tkdtkd97/post/b48bc7b3-ae64-4969-ad2f-c194e5d5fb2c/image.png)
이때 갑자기 트래픽이 늘면 웹어플리케이션에서 프로시저를 호출하고 프로시저가 많이 동작하게된다.
그러면 사용자가 불편을 겪게 된다. (느려진다.)
![](https://velog.velcdn.com/images/tkdtkd97/post/61e4a581-09b6-44aa-a793-70a4e48d6eff/image.png)

DB 서버를 늘리려면 데이터 복제를 해야하기 때문에 쉽사리 늘려지지 않는다.

> 저장 프로시저를 사용하면 유사시에 트래픽이 몰릴때 DB에 메모리사용량이나 cpu사용량이 증가하고 긴급하게 대응을 할 수 없게된다.

![](https://velog.velcdn.com/images/tkdtkd97/post/af4b31ae-c92d-42d4-bcd0-f5df391318b1/image.png)

3. 로직 티어에 어플리케이션 서버 투입은 간단하다.
   ![](https://velog.velcdn.com/images/tkdtkd97/post/d134bf49-de06-4119-b2bc-05b3eec45d8d/image.png)
   DB에 cpu 사용량이 안정적이고 서버가 부하를 받을때 신규서버를 투입하기 쉽다.
   ![](https://velog.velcdn.com/images/tkdtkd97/post/a064b316-d146-4744-9d5f-f9d6ab55dff6/image.png)

4. stored procedure가 언제나 transparent인건 아니다.

프로시저의 로직의 이름이 수정이 되면 서버에서 호출할때 이름이랑 다르기때문에 문제가 생긴다.
![](https://velog.velcdn.com/images/tkdtkd97/post/b1878834-5ad8-4043-b5c2-7420c448d482/image.png)
그래서 그냥 바꿔주면 안되고 기존의 프로시저를 두고 새로운이름의 프로시저를 만든다. 그리고 서버에서 호출하는 로직을 변경해준다.
![](https://velog.velcdn.com/images/tkdtkd97/post/b4e6db12-7524-4051-b335-b76621836c2f/image.png)
그러면 바꿔진 코드로 빌드를하고 하나하나 재시작을 해주고 기존의 프로시저를 지운다. 상황에 따라 오히려 손이 많이 갈 수 있다.

5. transparent하다고 무조건 좋은 것도 아니다.
   ![](https://velog.velcdn.com/images/tkdtkd97/post/b98144d8-70a4-4aff-bac1-568a411b6d02/image.png)
   프로시저의 바디부분을 변경을 했는데 예상치 못한 버그가 있었다.
   이때 이 버그를 롤백할동안에 들어온 트래픽들은 문제가 생기고 사용자의 피해로 돌아간다.

하지만 로직티어에서 관리한다면 하나하나 변경을 하고 모니터링할수있고 예상치 못한 에러를 발견해도 4대의 서버중 1대만 문제가 있는것이기 때문에 장애를 최소화할수 있다.
![](https://velog.velcdn.com/images/tkdtkd97/post/5c65c26f-65f3-4794-938e-bc94ba95eda1/image.png)

6. 재사용이 가능하다는 것도 양날의 검이 될 수도 있다.
   ![](https://velog.velcdn.com/images/tkdtkd97/post/73ece180-3dba-41d9-8e8a-eafcda6d7f14/image.png)
   1개의 서버가 무리한 호출로 DB에 영향을 미치면 그 DB를 사용하는 다른 서버들도 문제가 발생할 수 있다.
   ![](https://velog.velcdn.com/images/tkdtkd97/post/6943185d-12b4-4c66-948a-9d2d1741dd6e/image.png)
   그래서 이렇게 관리를 해야한다. 각각의 서비스들은 프로시저를 직접적으로 호출하는 것이 아닌 api를 사용해서 호출한다.
   이러면 호출이 무리하게 많아졌을때 그 서버의 호출을 중단하고 다른서버는 문제가 생기지 않고 사용될 수 있다.

7. 비즈니스 로직을 소스 코드에 두고도 응답 속도를 향상 시킬 수 있다.
   응답속도가 빠르다는 장점이 있었다.
   ![](https://velog.velcdn.com/images/tkdtkd97/post/0dbff713-2b51-4d04-ac30-787c29907316/image.png)
   동시호출로 응답속도를 향상시킨다.
   순차적인 호출이 요구될때 캐쉬를 사용해서 응답 속도를 향상 시킨다.
   ![](https://velog.velcdn.com/images/tkdtkd97/post/617ace19-0af3-403c-8253-0b7953a1ba0c/image.png)

8. 민감한 정보에 대한 접근을 완벽히 제한할 순 없다.
   개발자가 마음만 먹으면 프로시저를 통해 민감한 정보를 리턴하게 할수도있다.
   DB 혹은 테이블 접근을 막으면 개발 및 CS업무의 신속함이 떨어진다.
   ![](https://velog.velcdn.com/images/tkdtkd97/post/cd984136-b377-40b3-9a6c-2ed9b84e7e7e/image.png)

![](https://velog.velcdn.com/images/tkdtkd97/post/3b0ffb51-5cbc-49c9-9a5a-5c40eb7312ca/image.png)

# 참고
https://velog.io/@donghoim/MySQL-%EC%A0%80%EC%9E%A5-%ED%94%84%EB%A1%9C%EC%8B%9C%EC%A0%80-Stored-Procedure
https://www.youtube.com/watch?v=m2jx18yg8EA
https://www.youtube.com/watch?v=SOLm-GXFzG8

# 면접 질문
1. 저장 프로시저가 무엇인지 설명해주세요

   데이터베이스에 저장된 SQL명령문을 하나의 함수처럼 실행하기 위한 Query(쿼리)의 집합이다. 같은 쿼리를 반복할 필요가 없어 속도가 빠르고 에러 확률을 감소시킨다.


2. 저장 프로시저를 사용하면 어떤 장점이 있나요?
- 반복적인 작업을 피할 수 있다. 하지만 저장 프로시저에 예상치못한 에러가 발생하면 피해를 저장 프로시저를 호출하는 모든 곳에서 받는다는 단점이 있다.
- 개발 언어에 비의존적이다.
- SQL문이 서버에 이미 저장되어 있기 때문에 클라이언트와 서버 간 네트워크 상 트래픽이 감소된다.
- 프로시저 내에서 참조 중인 테이블의 접근을 막을 수 있아 보안성이 향상된다.