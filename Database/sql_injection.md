# SQL injection

SQL injection은 사용자의 입력값이 서버측에서 코드로 실행되는 ‘코드 인젝션’ 공격 기법 중 하나이다.  


> 📌 **SQL injection :** 
웹 애플리케이션에서 DB Query에 사용될 사용자 입력값을 유효성 검증을 하지 않아, 개발자가 의도하지 않은 동적 쿼리 (Dynamic Query)를 생성하여 DB 정보를 열람하거나 조작할 수 있는 공격 기법이다.



2017년 “여기어때”의 고개 정보 및 고객 투숙 정보 노출, 2015년 “뽐뿌”의 개인정보 노출 사고가 SQL injection의 원인이었다.

### Classic SQLi

로그인에서 사용자의 ID와 password를 확인하기 위해 아래와 같은 쿼리를 사용하는 경우가 있다.

```sql
SELECT * 
FROM USERTABLE 
WHERE username='your_user_input' AND password='your_password_input'
```

여기에 `1=1 -—`을 삽입하여 SQLi 공격을 시도하면 아래와 같이 된다.

```sql
SELECT * 
FROM USERTABLE 
WHERE username='random_input' OR 1=1 -- 'AND password='random_password'
```

위의 예시 외에도 아래와 같은 방법이 있다.

- Union-based SQLi : UNION 명령어를 이용하여 DB 종류, 컬럼 수, 취약 영역을 찾을 수 있다.
- Error-based SQLi : 데이터베이스 구조를 얻기 위해 서버에서 반환되는 에러에 의존한다.

| 구분 | 예시 |
| --- | --- |
| Classic SQLi | Union-based SQLi, Error-based SQLi |
| Blind SQLi | Boolean-based SQLi, Time-based SQLi |
| Out-of-band SQLi | - |

## 대응방안

### 1️⃣ Prepared Statement

매개변수화된 구문을 사용해서 동일한 구문을 실행할 때 SQLi 공격을 방지하고 효율성 높은 쿼리문을 실행할 수 있다.

바인딩된 변수는 쿼리와 별도로 서버로 전송되므로 사용자가 입력한 구문이 쿼리로 실행될 수가 없다. 서버는 쿼리 템플릿이 모두 분석된 이후 실행 시점에 사용자 입력값을 사용한다. 바인딩된 매개변수는 쿼리 문자열로 삽입되지 않으므로 이스케이프할 필요가 없다.

- Native — Java : [java.sql](https://docs.oracle.com/en/java/javase/12/docs/api/java.sql/java/sql/package-summary.html)’s [PreparedStatement](https://docs.oracle.com/en/java/javase/12/docs/api/java.sql/java/sql/PreparedStatement.html)

PreparedStatement의 `setString()`을 통해서 검증을 함으로써 파라메터 바인딩하고 자동으로 이스케이프할 수 있다.

⌨️ setString() 코드의 일부

<img src="https://github.com/daengnyangffojjag/CS-Interview/assets/102219847/a0ad2ae1-d42e-42da-a5b5-c1a1dbc680cd" width="60%">

- 예시
    
    **⌨️ 취약한 코드**
    
    ```java
    try{
      String uId = props.getProperty(“jdbc.uId”);
     
      String query = “SELECT * FROM tb_user WHERE uId=” + uId;
      stmt = conn.prepareStatement(query);
      ResultSet rs = stmt.executeQuery();
      while(rs.next()){
        .. …
      }
    }catch(SQLException se){
      .. …
    }finally{
      .. …
    }
    ```
    
    **⌨️ 안전한 코드**
    
    ```java
    try{
      String uId = props.getProperty(“jdbc.uId”);
     
      String query = “SELECT * FROM tb_user WHERE uId = ?”
      stmt = conn.prepareStatement(query);
      stmt.setString(1, uId);
    
      ResultSet rs = stmt.executeQuery();
      while(rs.next()){
        .. …
      }
    }catch(SQLException se){
      .. …
    }finally{
      .. …
    }
    ```
    

### 2️⃣ 블랙리스트 & 화이트 리스트 기반 필터링

블랙리스트는 SQL 쿼리의 구조를 변경하는 특수문자, SQL 예약어 등을 미리 등록해놓고 입력을 제한하는 방식이다.

해당 문자가 입력되었을 때 에러 메시지를 반환하거나, 미리 준비한 문자열로 치환하는 방법이 있다.

```sql
(특수문자) ' , " , = , & , | , ! , ( , ) , { , } , $ , % , @ , #, -- 등
(예 약 어) UNION, GROUP BY, IF, COLUMN, END, INSTANCE 등
(함 수 명) DATABASE(), CONCAT(), COUNT(), LOWER() 등
```

화이트 리스트 방식은 허용하는 문자열을 미리 등록하고 리스트 이외의 문자열이 입력되었을 때 차단한다. 블랙리스트보다 강력하지만 이용자 편의성이 떨어진다. 사이트 기능에 따라 하나로 정의된 사전 목록을 사용할 수 없기 때문에 패턴화, 정규화한 문자열을 사용하는 것을 권장한다.

### 3️⃣ 오류 메세지 출력 제한

DB 오류 메세지의 노출은 공격자에게 2차 공격을 할 수 있는 정보를 제공하므로 오류 메시지의 출력을 제한한다.

DB 버전정보, DB 종류 등을 포함하여 로그인 시 ID, Password 오류 여부도 불필요한 경우 노출하지 않는다.

### 4️⃣ DB 보안 적용

관리자 DB 계정과 웹애플리케이션 운영 계정의 권한을 분리하여 운영한다. DDL, DML 등 사용하는 구문 별 계정을 구분하여 운영한다.

이외에도 웹 방화벽 사용, 주기적 로그 점검, 취약점 점검 등을 적용할 수 있다.

## JPA에서 SQL injection

Spring Data JPA에서 기본적으로 제공하는 API는 내부적으로 이미 Parameter Binding을 이용하고 있어서 SQL injection에 대응한다. 하지만 쿼리 스트링을 직접 짜서 사용하는 경우에는 주의를 해야한다.

아래의 코드는 직접 입력값을 이용하기 때문에 안전하지 않다.

```java
public List<AccountDTO> unsafeJpaFindAccountsByCustomerId(String customerId) {    
    String jql = "from Account where customerId = '" + customerId + "'";        
    TypedQuery<Account> q = em.createQuery(jql, Account.class);        
    return q.getResultList()
      .stream()
      .map(this::toAccountDTO)
      .collect(Collectors.toList());        
}
```

`@Query()`를 사용 또는 쿼리에 `:parameter`가 들어간다면 Parameter binding을 하게된다.

```java
String jql = "from Account where customerId = :customerId";
TypedQuery<Account> q = em.createQuery(jql, Account.class)
  .setParameter("customerId", customerId);
// Execute query and return mapped results (omitted)
```

```java
@Query(value="SELECT mdate FROM tablexyz WHERE instanceName =:instanceName",nativeQuery=true)
List<String> findAllByInstanceName(@Param("instanceName") String instanceName);
```

### 질문
- SQL injection이란?

    웹애플리케이션에서 DB Query에 사용될 사용자 입력값을 유효성 검증을 하지 않아, 개발자가 의도하지 않은 동적 쿼리 (Dynamic Query)를 생성하여 DB 정보를 열람하거나 조작할 수 있는 공격 방법이다. 
    - JPA에서 SQL injection 방어는?
    
        JPA는 기본 메서드에서 PreparedStatement를 사용하여 SQL injection을 방어한다. 하지만 JPA query를 만들어서 사용할 때도 입력값을 바로 쿼리에 넣게되면 SQL injection에 취약하게 되므로, 이때도 파라미터 바인딩를 사용하도록 한다.


### 참고

[https://gomguk.tistory.com/118](https://gomguk.tistory.com/118)
[https://www.baeldung.com/sql-injection](https://www.baeldung.com/sql-injection)
