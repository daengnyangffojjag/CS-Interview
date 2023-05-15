# DB  Key

Key란? : 검색, 정렬시 Tuple을 구분할 수 있는 기준이 되는 Attribute.

<학생> 릴레이션

| 학번 | 주민번호 | 성명 |
| --- | --- | --- |
| 1001 | 000323-3*** | 김상욱 |
| 1002 | 021103-3*** | 임선호 |
| 1003 | 040402-4*** | 이다해 |

<수강> 릴레이션

| 학번 | 과목명 |
| --- | --- |
| 1001 | 영어 |
| 1001 | 정보 |
| 1002 | 수학 |
| 1003 | 국어 |


### 1. Candidate Key (후보키)

Tuple을 유일하게 식별하기 위해 사용하는 속성들의 부분 집합. (기본키로 사용할 수 있는 속성들)

### **특징**

- 하나의 릴레이션내에서는 중복된 튜플들이 있을 수 없으므로 모든 릴레이션에는 반드시 하나 이상의 후보키 존재
- 모든 튜플에 대해 2가지 조건 만족
    - 유일성 : 하나의 Key값으로 하나의 Tuple을 유일하게 식별할 수 있음
    - 최소성 : 꼭 필요한 속성으로만 구성

### **예시**

ex) <학생>릴레이션에서 ‘학번’이나 ‘주민번호’는 다른 레코드를 유일하게 구별가능 >> 후보키

### 2. Primary Key (기본키)

후보키 중 선택한 Main Key

### 특징

- Null 값을 가질 수 없음
- 동일한 값이 중복될 수 없음

### 예시

 - <학생>릴레이션에서는 ‘학번’이나 ‘주민번호’가 기본키

- <수강>릴레이션에서는 ‘학번’+’과목명’으로 조합해야 기본키 가능

### 3. Alternate Key (대체키)

후보키 중 기본키를 제외한 나머지 키 = 보조키

### 예시

- <학생> 릴레이션에서 ‘학번’을 기본키로 하면, ‘주민번호’는 대체키

### 4. Super Key (슈퍼키)

한 릴레이션 내에 있는 속성들의 집합으로 구성된 키

### 특징

- 유일성은 만족하지만, 최소성은 만족하지 못하는 키

### 예시

- <학생>릴레이션에서는 ‘학번’, ‘주민번호’,‘학번+’주민번호’,’주민번호+성명’, ‘학번+주민번호+성명’으로 슈퍼키 구성 가능

### 5. Foreign Key (외래키)

다른 릴레이션의 기본키를 그대로 참조하는 속성의 집합

### 특징

- 외래키로 지정되면 참조 릴레이션의 기본키에 없는 값은 입력 x

### 예시

- <수강>릴레이션이 <학생>릴레이션을 참조하고 있으므로 <학생> 릴레이션의 ‘학번’은 기본키이고, <수강>릴레이션의 ‘학번’은 외래키

# Join

조인이란?

- 2개의 테이블에 대해 연관된 튜플들을 결합해 하나의 새로운 릴레이션을 반환

### Join 종류

- INNER JOIN
- LEFT OUTER JOIN
- RIGHT OUTER JOIN
- FULL OUTER JOIN
- CROSS JOIN
- SELF JOIN

### Inner Join

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e80a42be-8dc5-4d8d-af29-da90a4d0dcae/Untitled.png)

- 교집합
- JOIN 대상 테이블에서 공통 속성을 기준으로 ‘=’ 비교에 의해 같은 값을 가지는 행을 연결해 결과를 생성하는 JOIN 방법
- Natural Join이라고도 함.

```jsx
SELECT 학번, 이름, 학생.학과코드, 학과명
FROM 학생, 학과
WHERE 학생.학과코드 = 학과.학과코드
```

### Left Outer Join

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa606576-4707-4dbb-963a-d47aabdea0c4/Untitled.png)

- Join 조건에 만족하지 않는 튜플도 결과로 출력하기 위함
- 우측 항 릴레이션의 어떤 튜플과도 맞지 않는 좌측 항의 릴레이션에 있는 튜플들에 NULL값 붙여서 INNER JOIN 결과에 추가

```jsx
SELECT
A.NAME, B.AGE
FROM EX_TABLE A
LEFT OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
```

### Right Outer Join

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/24aebe61-3158-46d7-81bd-272a2c82213b/Untitled.png)

- Left Join과 반대

```jsx
SELECT
A.NAME, B.AGE
FROM EX_TABLE A
RIGHT OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
```

### Full Outer Join

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bc78cfd3-1822-4a17-91e2-81a3c02f9917/Untitled.png)

- 합집합

```jsx
SELECT
A.NAME, B.AGE
FROM EX_TABLE A
FULL OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
```

### Cross Join

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f7a5a10-7105-4d71-a008-495e3143fa13/Untitled.png)

- 조인하는 두 테이블에 있는 튜플들의 순서쌍을 결과로 반환

```jsx
SELECT
A.NAME, B.AGE
FROM EX_TABLE A
CROSS JOIN JOIN_TABLE B
```

### Self Join

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8ead93ad-acb2-44e1-9501-07c828287a6a/Untitled.png)

- 자기자신과 자기자신 조인
- 자신이 갖고 있는 칼럼을 다양하게 변형시켜 활용할 때 사용

```jsx
**<학생> 테이블을 Self join해 선배가 있는 학생과 선배의 이름을 표시하는 sql문**

SELECT A.학번, A.이름, B.이름 as 선배
FROM 학생 A JOIN 학생 B
ON A.선배 = B.학번
```

# 예상질문

1. primary key에 대한 특성에 대해 설명해 주세요.
2. join에 대해서 설명해주세요
3. Left Outer Join과 Inner Join 차이점에 대해서 설명해 주세요.
4. A, B 테이블에 같은 컬럼이 존재할 경우 조인(Inner Join/Outer Join) 시 어떤 값을 출력하나요?
