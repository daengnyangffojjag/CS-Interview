# 프로그래밍의 오류 종류
1. 컴파일 에러(compile-time error) : 컴파일시에 발생하는 에러
2. 런타임 에러(runtime error) : 실행시에 발생하는 에러
3. 논리적 에러(logical error) : 실행은 되지만 의도와 다르게 동작하는것

### 논리 에러 (Logic Error)

프로그램이 실행하고 작동하는데 아무런 문제가 없는 오류이지만, 결과가 예상과 달라 의도한 작업을 수행하지 못한경우

> 논리적 오류는 컴퓨터 입장에서는 프로그램이 멀쩡히 돌아가는 것이니 에러 메시지를 알려주지 않는다

ex
재고량이 음수가 나왔다.
게임에서 피가 0이 됐는데 죽지 않는다.

### 컴파일 에러 (Compillation Error)

컴파일 단계에서 오류 발견하면 컴파일러가 에러 메시지 출력해주는 것을 말한다.

> 컴파일에러가 일어나면 프로그램 실행 자체가 안된다. 그래서 차후에 일어날 에러를 컴파일러가 미리 멘토링 한다고 생각하며 코드를 수정하면 될 일이다.

ex
문법 구문 오류(syntax error) 맞춤법, 문장부호생략, 선언되지 않은 변수 사용 등

### 런타임 에러 (Runtime Error)

컴파일에는 문제가 없더라도 프로그램 실행 중에 에러가 발생해 잘못된 결과를 얻거나 외부적인 요인으로 프로그램이 비정상적으로 종료되는 것

> 대체로 설계 미숙으로 발생하는 에러, 런타임 에러 발생 시 프로그래머가 역추적해서 원인 확인해야 한다

# 오류(error) 와 예외(exception)
자바 프로그래밍에서는 실행 시(runtime) 발생할 수 있는 오류를 '에러(error)'와 '예외(exception)' 두가지로 구분

1. 에러(error) : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
2. 예외(exception) : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류
> 예외는 에러와 달리 문제가 발생하더라도 이에 대한 대응 코드를 미리 작성해 놓음으로써 어느정도 프로그램의 비정상적인 종료 혹은 동작을 막을 수 있다. = 예외 처리(exception handling)

# 자바의 예외(Exception) 클래스
### Throwable 클래스란?
Throwable 클래스의 역할은 오류나 예외에 대한 메시지를 담는 것
getMessage() 와 printStackTrace() 메서드가 바로 이 클래스에 속해 있음

printStackTrace() : 발생한 Exception의 출처를 메모리상에서 추적하면서 결과를 알려줍니다. 발생한 위치를 정확히 출력해줘서 제일 많이 쓰며 void를 반환합니다.
getMessage() : 한줄로 요약된 메세지를 String으로 반환해줍니다.

![](https://velog.velcdn.com/images/tkdtkd97/post/0d0f4456-fca3-4afc-b6e8-dcc575a19e32/image.png)

![](https://velog.velcdn.com/images/tkdtkd97/post/10789123-b698-4449-9f0e-2b6b4516b58f/image.png)

![](https://velog.velcdn.com/images/tkdtkd97/post/10ff4d98-9623-41b5-a0f0-76555c4c1547/image.png)

### 런타임 예외 클래스 종류
ArrayIndexOutOfBoundsException -> 배열의 범위를 넘어선 인덱스를 참조할 때 발생하는 에러
ArithmeticException -> 정수를 0으로 나눌 때 발생하는 에러
NullPointException -> null 객체에 접근해서 method를 호출하는 경우 발생하는 에러 (객체가 없는 상태에서 객체를 사용하려 했으니)
NumberFormatException -> 정수가 아닌 문자열을 정수로 변환할 때 예외 발생
InputMismatchException -> 의도치 않는 입력 오류 시 발생하는 예외

### 컴파일 예외 클래스 종류
IOException
-> 컴퓨터 프로그램이 실행될 때 언제 어떤 문제가 발생할지 모르는 일이기 때문에, 컴퓨터와 상호소통 하는 I/O(입력과 출력)에 관해서는 발생할 수 있는 예외에 대해서 까다롭게 규정하고 있다.
-> 그래서 입력과 출력을 다루는 메서드에 예외처리(IOException)가 없다면 컴파일 에러가 발생하게 된다.
ex. StringBuffer 뒤에 IOException

FileNotFoundException -> 파일에 접근하려고 하는데 파일을 찾지 못했을 때 발생하는 에러

# Checked Exception / Unchecked Exception

Checked Exception = 컴파일 예외클래스
Unchecked Exception = 런타임 예외클래스

![](https://velog.velcdn.com/images/tkdtkd97/post/51b45a37-be27-496e-95d5-4b64417623b7/image.png)

### 예외는 반드시 예외 처리를 전부 해야 하는가?
#### Checked Exception
Checked Exception은 체크 하는 시점이 컴파일 단계이기 때문에, 별도의 예외 처리를 하지 않는다면 컴파일 자체가 되지 않는다.

따라서 Checked Exception이 발생할 가능성이 있는 메소드라면 반드시 로직을 try - catch로 감싸거나 throws로 던져서 처리해야 한다.

#### Unchecked Exception
반면에 Unchecked Exception의 경우는 명시적인 예외 처리를 하지 않아도 된다.

따라서 에러를 일부러 일으키는 코드가 있더라도 try - catch 처리하지 않더라도 컴파일도 되고 실행까지 가능하다.


# 참고
https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%90%EB%9F%ACError-%EC%99%80-%EC%98%88%EC%99%B8-%ED%81%B4%EB%9E%98%EC%8A%A4Exception-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC

# 면접 예상 질문
> Error와 Exception의 차이를 설명해주세요.

Error는 실행 중 일어날 수 있는 치명적 오류를 말합니다.
컴파일 시점에 체크할 수 없고, 오류가 발생하면 프로그램은 비정상 종료되며 예측 불가능한 UncheckedException에 속합니다.

런타임에서 실행 시 발생되며 전부 예측 불가능한 Unchecked Error에 속한다.
반면, Exception은 Error보다 비교적 경미한 오류이며, try-catch를 이용해 프로그램의 비정상 종료를 막을 수 있습니다.

> 자바에서 예외 처리 방법 두가지 설명해주세요

예외가 발생한 메소드 내에서 직접 처리하는 방법( try - catch - finally)과
예외가 발생한 메소드를 호출 한 곳으로 예외 객체를 넘기는 방법 (throws)이 있습니다.