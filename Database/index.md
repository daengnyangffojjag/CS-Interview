# Index


### Index란?
Index는 RDBMS에서 검색 속도를 높이기 위한 기술이다.

TABLE의 컬럼을 색인화(따로 파일로 저장)하여 검색시 해당 TABLE의 레코드를 Full Scan 하는게 아니라 색인화 되어있는 INDEX 파일을 검색하여 검색속도를 빠르게 한다.

RDBMS에서 사용하는 INDEX는 B-Tree 에서 파생된 B+ Tree 를 사용해서 색인화한다.

보통 SELECT 쿼리의 WHERE절이나 JOIN 예약어를 사용했을때 인덱스가 사용되며 SELECT 쿼리의 검색 속도를 빠르게 하는데 목적을 두고 있다.

### 인덱스가 있고 없고 성능 차이

![](https://velog.velcdn.com/images/tkdtkd97/post/fa44bdfe-a2ae-4be0-becc-b8136b566f98/image.png)
first_name에 index가 걸려있지 않다면?
> full scan(=table scan)으로 찾아야한다.
O(N)의 시간복잡도

first_name에 index가 걸려있다면?
> full scan보다 더 빨리 찾을 수 있다.
O(log N) (B-tree based index)

쿼리문이 더빠르게 처리되고 성능이 향상된다.

### 인덱스를 쓰는 이유
1. 조건을 만족하는 튜플(들)을 빠르게 조회하기 위해
2. 빠르게 정렬(Order by)하거나 그룹핑(Group by)하기 위해

![](https://velog.velcdn.com/images/tkdtkd97/post/539b3867-93ec-45b4-8384-f385c158893b/image.png)

Where절에서 특정 조건에 맞는 데이터를 찾기위한 것과 Join하기 위한 조건을 찾기 위해 사용되는 것이 인덱스이다.

### 인덱스를 거는 문법
네가지 속성들이 있음
- 테이블이 생성되어있는경우 index를 거는 방법
  ![](https://velog.velcdn.com/images/tkdtkd97/post/12684a05-5166-4e8c-851f-696c3b0b072a/image.png)
  name같은 경우는 동명이인이 있을 수 있기 때문에 중복을 허용해준다.
  team_id와 backnumber을 같이 사용하기 때문에 name과 다르게 UNIQUE하게 식별할 수 있다.

- 처음테이블을 생성할때 index를 거는 방법
  ![](https://velog.velcdn.com/images/tkdtkd97/post/2e7f47b9-6f3d-4c88-8a11-5114ad5cbd3a/image.png)
  인덱스 이름은 생략가능하다.
  두개이상의 속성으로 이루어진 인덱스를 Multicolumn index 또는 composite index라고 한다.
  대부분의 RDBMS에서는 PrimaryKey를 생성할때 자동으로 index를 생성해준다.

### 테이블에 걸려있는 인덱스 파악하기
![](https://velog.velcdn.com/images/tkdtkd97/post/263abf66-5a26-4726-8913-0f19327caa25/image.png)
> 원래 더 많이 보여주는데 중요한거만 표시한것들

### B-tree기반 인덱스가 동작하는 방식
![](https://velog.velcdn.com/images/tkdtkd97/post/e080dad8-c765-4995-933b-a29f6d8cd17a/image.png)
> Members 테이블에 a 속성의 인덱스(B-tree기반)
특징 : 오름차순 정렬, ptr(포인터로 실제 Members 테이블의 a 속성에 어디있는지 나타냄)

- 예를들어 a = 9인경우를 찾아보자
  ![](https://velog.velcdn.com/images/tkdtkd97/post/0803a559-3ec8-4402-a4a5-04c77f57538f/image.png)
> 쿼리문에 Where a = 9; 라고 적혀서 실행되면
<br/>
a 인덱스에서 binary search로 9를 찾는다.
a 인덱스에 9가 또 있는지 탐색하고 없으면 포인터로 a 속성에 데이터를 연결한다.

- a = 7이고 b = 95인 경우
  ![](https://velog.velcdn.com/images/tkdtkd97/post/4e878539-aa25-4dfe-b30d-e5ed483d97f8/image.png)
  ![](https://velog.velcdn.com/images/tkdtkd97/post/15df0ad5-3c04-4fcc-8938-79f274ffd5a1/image.png)
> a = 7인 경우를 binary search로 찾고 a 속성의 데이터에 접근한다.
b의 값이 95인경우를 찾는다.
a = 7인 다른경우도 확인한다.

![](https://velog.velcdn.com/images/tkdtkd97/post/7ba975ee-0d1e-4486-bcb6-b179e6a1beeb/image.png)
이런식으로 탐색이 되는데 지금은 같은 경우가 3가지라 괜찮아 보이지만 같은게 많아진다면 full scan을 하는것과 다르지 않다.

> 그래서 a, b를 묶은 인덱스가 필요하다.

- (a, b)인덱스를 생성하고 a = 7, b = 95인 경우를 탐색
  ![](https://velog.velcdn.com/images/tkdtkd97/post/efb75866-4fbd-48c1-91c1-66b9e7aca6e0/image.png)
> a는 전체가 오름차순으로 되어있다. b는 a가 같은경우에만 오름차순으로 정렬되있다.
그래서 동일하게 a를 binary search로 찾고 b도 binary search로 찾으면 된다.

- b=95를 탐색하는경우
  ![](https://velog.velcdn.com/images/tkdtkd97/post/beee9199-935f-4076-8e31-e61461d86c9b/image.png)
  (a, b) 인덱스를 사용하는것은 비효율적이다. 이 경우도 full scan하는것과 흡사하다.

> B-tree기반 인덱스가 동작하는 방식 정리
1. B-tree 자료구조가 갖는 장점인 시간복잡도가 언제나 O(log N)이 나온다는 것을 이용하는것이다.
2. 속성마다 인덱스를 만들어야하는데 Pk가 있으면 자동적으로 만들어준다.
3. 한개의 속성 조건만 있으면 한개짜리 인덱스를 사용하면된다.
4. 두개 이상의 속성을 비교해야 봐야한다면 그 두개의 속성을 활용한 인덱스를 만들어서 탐색해야하 O(log N)의 시간복잡도 이점을 가져갈 수 있다. 즉, 효율성이 높아진다.

예시
![](https://velog.velcdn.com/images/tkdtkd97/post/894f8b6e-d16f-4562-b16e-1614610aaf11/image.png)

![](https://velog.velcdn.com/images/tkdtkd97/post/6afcde21-6d47-4b7c-8755-0d25566b124b/image.png)

### 특정 인덱스를 사용하도록 명시
내가 쿼리문 날리면 어떤 인덱스를 사용하나?
![](https://velog.velcdn.com/images/tkdtkd97/post/7868fac7-70e4-425f-bc88-54a0b9dadee8/image.png)

위의 사진은 DB의 Optimizer가 알아서 적절한 인덱스를 골라서 해줬다.

하지만 Optimizer가 제대로 못 고를때도 있다.
그래서 원하는 인덱스로 탐색하도록 정할 수 있다.
![](https://velog.velcdn.com/images/tkdtkd97/post/30e8ba07-b781-4c98-b85f-bf3f683abf4b/image.png)

USE INDEX는 권장사항 느낌
FORCE INDEX는 웬만하면 써라하는 느낌인데 Optimizer가 주문받은 인덱스로 못찾을 거 같으면 그냥 full scan 한다고 한다.
IGNORE INDEX = 특정 인덱스는 쓰지마라

### 인덱스 막 만들어도 괜찮을까?
인덱스에는 속성마다 들어있는 데이터들이 존재한다.
이때 속성 데이터의 생성, 수정, 삭제가 있을때 인덱스도 똑같이 변경이 발생하고 B-tree구조이기때문에 순서변경도 있을 것임

![](https://velog.velcdn.com/images/tkdtkd97/post/a5c96871-0768-44e0-95e0-cd3ca670fe0b/image.png)
예를 들어 team_id는 team_id+backnumber 이 두개를 묶은 인덱스를 사용하면 되지 굳이 team_id만의 인덱스를 만들필요가 없다.

> 불필요한 index를 만들지 말자
저장공간 차지와 데이터 변경에 따른 작업이 필요하기 때문

### Covering Index
조회하는 속성들의 데이터를 인덱스가 모두 커버할때 테이블에 접근하지 않고 인덱스에서만 동작을 수행하는 방식
조회 성능이 더 빨라진다.
종종 의도적으로 이런 구조를 만들어 조회 성능을 빠르게 할 수 있다고 한다.
![](https://velog.velcdn.com/images/tkdtkd97/post/2663a788-3c9f-44b4-b677-359e5eff4796/image.png)

### Hash index
![](https://velog.velcdn.com/images/tkdtkd97/post/28094a8d-860b-4641-b645-de29edaa317b/image.png)

### Full scan이 더 좋은 경우
![](https://velog.velcdn.com/images/tkdtkd97/post/0f8a39c6-59c6-4b74-a310-05de2e750f3b/image.png)
예시) 통신사 SKT를 사용하는 유저를 조회할때
읽어야 할 건수가 전체 테이블 레코드의 20~25%를 넘어서면 인덱스를 이용하지 않는 것이 효율적이다.
이런 경우 옵티마이저는 인덱스를 이용하지 않고 테이블 전체를 읽어서 처리한다.

### 그 외
![](https://velog.velcdn.com/images/tkdtkd97/post/5c131d55-bd48-4687-a518-e40427d23aa5/image.png)
mysql은 fk도 인덱스 자동으로 생성해줌
이미 데이터가 몇 백만건 이상 있는 데이블에 인덱스를 생성하는건 트래픽이 적은 시간때에 하는것이 좋다.

- 참고
  ![](https://velog.velcdn.com/images/tkdtkd97/post/cd3bf81b-087b-4219-8404-d5f454a3d0ca/image.png)


# 참고
https://mangkyu.tistory.com/286
https://www.youtube.com/watch?v=IMDH4iAQ6zM

# 면접 질문
> 1. index가 뭔지 설명하고 특징을 말해보세요

Index란 테이블을 처음부터 끝까지 검색하는 방법인 Full Scan과는 달리 인덱스를 검색하여 해당 자료의 테이블을 엑세스 하는 방법입니다.

인덱스는 항상 정렬된 상태를 유지하기 때문에 원하는 값을 검색하는데 빠르지만, INSERT, UPDATE시 Index를 수정하므로 추가적인 I/O가 발생하며 너무 많은 인덱스는 성능 저하가 생길 수 있다.

즉, 인덱스는 데이터의 저장 성능을 희생하고 그대신 데이터의 검색 속도를 높이는 기능이라 할 수 있습니다.

> 2. index를 사용할때보다 full scan을 하면 효율적인 경우는?

  읽어야 할 건수가 전체 테이블 레코드의 20~25%를 넘어서면 인덱스를 이용하지 않는 것이 효율적이다.

table에 데이터가 몇백건 정도일 경우 full scan이 index를 활용하는것보다 효율적이다.

> 3. 인덱스를 사용하면 좋은 경우

1. 규모가 작지 않은 테이블
2. INSERT, UPDATE, DELETE가 자주 발생하지 않는 컬럼
3. JOIN이나 WHERE 또는 ORDER BY에 자주 사용되는 컬럼
4. 데이터의 중복도가 낮은 컬럼 (고유한 값 위주로)

> 3.1 추가질문 : INSERT, UPDATE, DELETE가 자주 발생하는 컬럼에 index를 사용하면 어떤 문제가 생기는가?

이럴경우 인덱스의 크기가 비대해져서 성능이 오히려 저하되는 역효과가 발생할 수 있다.

그 이유는 데이터의 추가가 있을때 테이블에는 입력 순서대로 저장되지만, 인덱스는 테이블에는 정렬해서 저장해야되고

삭제의 경우 테이블에서만 삭제되고 인덱스 테이블에는 삭제하지 않고 사용하지않음 처리하여 남아있기 때문에 데이터가 비대해지고(삭제하면 binary search를 위해 정렬해야하기때문에 사용하지 않음 처리)

수정하는 경우 인덱스에는 UPDATE가 없기 때문에 DELETE, INSERT 두 작업 수행하게되어 성능이 저하됩니다.

> 예를들어 만약 어떤 테이블에 UPDATE와 DELETE가 빈번하게 발생된다면 실제 데이터는 10만건이지만, 인덱스는 100만건이 넘어가게 되어, SQL문 처리시 비대해진 인덱스에의해 오히려 성능이 떨어지게 될 것이다.
