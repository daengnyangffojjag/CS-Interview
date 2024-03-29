### 오토 박싱 & 오토 언박싱

---

**프리미티브 타입**과 그에 대응하는 **참조 타입인 래퍼 클래스** 간의 **자동 변환**

- 주로 제너릭과 함께 사용
- 기본적으로 자동으로 수행 (JDK 1.5 부터)

| 원시 타입(Primitivie Type) | 래퍼 클래스(Wrapper Class) |
| --- | --- |
| boolean | Boolean |
| byte | Byte |
| short | Short |
| int | Integer |
| float | Float |
| long | Long |
| double | Double |
| char | Character |
1. 오토 박싱
- 프리미티브 값을 해당하는 래퍼 클래스의 객체로 자동으로 변환
1. 오토 언박싱
- 래퍼 클래스의 객체를 프리미티브 값으로 자동으로 변환

```java
// JDK 1.5부터는 자바 컴파일러가 박싱과 언박싱이 필요한 상황에 자동으로 처리를 해준다

Integer num = 10;
// int 값을 Integer 객체로 오토 박싱

int num1 = num;
// Integer 객체를 int 값으로 오토 언박싱
```

### 제너릭과 함께 사용

```java
List<Integer> numbers = new ArrayList<>();

numbers.add(20); 
// int 값을 자동으로 Integer 객체로 오토박싱
int num1 = numbers.get(0); 
// Integer 객체를 자동으로 int 값으로 언박싱
```

### 오토 박싱 언박싱의 장단점

1. 장점
- 개발자가 명시적으로 처리를 안해줘도 되기 때문에 코드가 간결해짐
- 프리미티브 타입과 래퍼 클래스 간의 변환을 간편하게 처리
- 제너릭과 함께 사용해서 컬렉션에서 프리미티브 타입을 다룰 수 있음
1. 단점
- **자동 변환이 될 때 객체 생성 및 메모리 할당이 필요하므로 오버헤드가 발생할 수 있음**
- null 값이 들어있는 래퍼 클래스 객체를 언박싱 하면 NPE가 발생할 수 있음

### 예상질문

1. 오토 박싱과 언박싱을 사용하는 이유를 설명해주세요

오토 박싱과 언박싱은 개발자가 명시적으로 변환 처리를 안해줘도 되기 떄문에 코드가 간결해지고 제너릭과 함께 사용해서 컬렉션에서 프리미티브 타입을 다룰 수 있기 때문에 사용합니다

1. 오토 박싱과 언박싱은 주로 어떤 상황에서 사용하나요?

오토 박싱과 언박싱은 주로 제너릭과 함께 사용하는데 제너릭은 참조 타입만을 허용하기 때문에 프리미티브 타입을 다루기 위해선 래퍼 클래스를 사용해야 합니다. 따라서 오토박싱과 언박싱을 사용하면 두 타입간의 변환을 간단하게 처리 할 수 있기 때문에 사용합니다

[[Java] 래퍼 클래스와 박싱, 언박싱에 대해 알아보자](https://cbw1030.tistory.com/285)
