## Promotion & Casting

### 형변환

---

하나의 데이터 유형을 다른 데이터 유형으로 변환하는 과정

### Promotion

---

작은 크기의 데이터 유형을 **더 큰 크기의 데이터 유형으로 자동으로 변환**

작은 메모리 크기의 데이터 타입 → 큰 메모리 크기의 데이터 타입

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/51ef3d07-be73-4b61-839e-6a9b006b4f50/Untitled.png)

### 특징

- 데이터 유형 간의 계층 구조에 따라 이루어짐
- 자동으로 실행되며 명시적인 변환 연산자가 필요없음
- **데이터 손실이 없음 ❗**

### Casting

---

큰 크기의 데이터 유형을 **작은 크기의 데이터 유형으로 변환**

```java
double num = 1.54;
int castingNum = (int)num;
// castingNum = 1

byte errorNum = 128;
// 범위 초과
byte castingNum2 = (byte)128;
// 데이터 손실
// castingNum2 = -128
```

### 특징

- 명시적으로 수행해야 하며 변환하려는 데이터 유형을 지정해야 함
- **데이터 손실이 발생할 수 있음**
- 크기가 작은 데이터 유형으로 변환할 때, 데이터 범위를 초과해 데이터 손실 발생

### 예상 질문

1. 프로모션과 캐스팅에 대해서 설명해주세요
2. 형변환 할 때 주의할 점에 대해서 설명해주세요

[백기선 자바 스터디 2주차](https://velog.io/@shkim1199/백기선-자바-스터디-1주차-sbetx07n)
