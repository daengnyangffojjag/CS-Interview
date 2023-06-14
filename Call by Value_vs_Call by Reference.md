### Call by Value

> 값에 의한 호출
> 
- 함수가 호출될 때, 메모리 공간 안에서는 함수를 위한 별도의 임시공간이 생성됨 (종료 시 해당 공간 사라짐)
- call by value 호출 방식은 함수 호출 시 전달되는 변수 값을 복사해서 함수 인자로 전달함
    - 이때 복사된 인자는 함수 안에서 지역적으로 사용되기 때문에 local value 속성을 가짐

`따라서, 함수 안에서 인자 값이 변경되더라도, 외부 변수 값은 변경안됨`

예시

```jsx
void func(int n) {
    n = 20;
}

void main() {
    int n = 10;
    func(n);
    printf("%d", n);   //결과 : 10
}
```

### Call by reference

> 참조에 의한 호출
> 
- 함수 호출 시 인자로 전달되는 변수의 레퍼런스를 전달함
- 따라서 함수 안에서 인자 값이 변경되면, 아규먼트로 전달된 객체의 값도 변경됨

예시

```jsx
void func(int *n) {
    *n = 20;
}

void main() {
    int n = 10;
    func(&n);
    printf("%d", n);  //결과 : 20
}
```

### Java 함수 호출 방식

> **자바의 경우, 항상 call by value로 값 넘김**
> 
- C/c++ 같이 변수 주소값 자체를 가져올 방법 x
- reference type을 넘길 시에는 해당 객체의 주소값을 **복사하여** 사용
    - 원본 객체의 속성까지는 접근 가능 하나, 원본 객체 자체를 변경 x

예시

```java
User a = new User("daeng");   // 1

foo(a);

public void foo(User b){        // 2
    b = new User("nyang");    // 3
}

/*
==========================================

// 1 : a에 User 객체 생성 및 할당(새로 생성된 객체의 주소값을 가지고 있음)
 
 a   -----> User Object [name = "daeng"]
 
==========================================

// 2 : b라는 파라미터에 a가 가진 주소값을 복사하여 가짐

 a   -----> User Object [name = "nyang"]
               ↑     
 b   -----------
 
==========================================

// 3 : 새로운 객체를 생성하고 새로 생성된 주소값을 b가 가지며 a는 그대로 원본 객체를 가리킴
 
 a   -----> User Object [name = "daeng"]
                   
 b   -----> User Object [name = "nyang"]
 
*/
```

### 예상질문

Call by Value vs Call by Reference에 대해 설명해주세요.

> 값에 의한 호출(Call by Value) : 값을 복사해서 새로운 함수로 넘기는 호출 방식. 원본 값 변경X
> 
> 
> 참조에 의한 호출(Call by Reference) : 주소 값을 인자로 전달하는 호출 방식. 원본 값 변경O
>
