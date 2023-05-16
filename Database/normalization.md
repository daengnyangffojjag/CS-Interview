## 정규화(Normalization)

데이터를 구조화하여 중복을 최소화하고 데이터의 일관성, 무결성을 유지하기 위한 과정

<aside>
💡 테이블을 분해하고 관련된 정보를 적절한 관계로 나누는 과정으로 데이터 이상 현상을 방지

</aside>

### 정규화 단계

테이블을 어떻게 분해하는지에 따라 단계가 나뉨

### 1. 제1정규형(1NF)

테이블의 속성이 원자값(하나의 값)을 가져야 한다

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3dc120b8-7946-41a9-851c-5a28f5a97bfb/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/622e5b4c-f90b-4116-8832-9e0b3fa3b819/Untitled.png)

### 2. 제2정규형(2NF)

제1정규형을 만족하며 **완전 함수적 종속**을 만족해야 한다

<aside>
💡 완전 함수적 종속?

</aside>

한 속성의 모든 값이 다른 속성 또는 속성 집합에 대해 유일하게 결정되는 상태

- 기본키가 복합키로 묶여 있을 때, 두키 중 하나의 키만으로 다른 컬럼을 결정지을 수 있으면 안됨
- 기본키의 부분집합 키가 결정자가 되어선 안됨

| 고객ID | 이름 | 전화번호 | 이메일 |
| --- | --- | --- | --- |
| 1 | 김 | 010-1234-5678 | kim@google.com |
| 2 | 박 | 010-2345-6789 | park@naver.com |
| 3 | 이 | 010-9876-5432 | lee@damu.net |
1. 고객ID → 이름
2. 고객ID → 전화번호
3. 고객ID → 이메일

모든 속성이 고객ID에 대해 완전 함수적 종속을 가지기 때문에 고객ID가 주어졌을 때 이름, 전화번호, 이메일이 모두 유일 하게 결정됨

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2c6ac6e8-efe3-46c2-83ea-7f900046e4f0/Untitled.png)

기본키 : 학생번호 , 강좌이름

학생번호, 강좌이름 → 성적

강좌이름 → 강의실

❗기본키의 부분집합 키인 강좌이름이 결정자가 됨

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5d41fbcf-04bc-44a7-bac9-db79099a1f1b/Untitled.png)

### 3. 제3정규형(3NF)

제2정규형을 만족하며 **이행적 종속성**을 제거해야 한다

<aside>
💡 이행적 종속성?

</aside>

A가 B에 종속되고 B가 C에 종속될 때, A가 C에 종속되는 상황

| 이름 | 학과 | 담당교수 |
| --- | --- | --- |
| 김 | 컴과 | 김교수 |
| 박 | 경영학과 | 박교수 |
| 이 | 수학과 | 이교수 |

이름이 학과를 결정 → 학과가 담당교수를 결정

❗김학생이 학과가 수학과로 바꼈을 때 담당교수는 이교수여야 하는데 김교수에서 이교수로 바꾸는 번거러움을 해결하기 위함

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f7279ffb-b9a8-423c-bbfc-1cc1f010bcf2/Untitled.png)

학생번호 → 강좌이름 → 수강료

**이행적 종속성이 있는 테이블**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2654d1ce-056f-4e2b-a8c8-48719cea34fa/Untitled.png)

**이행적 종속성을 제거한 테이블**

### 정규화의 장점

1. 중복 제거
- 데이터의 저장 공간 절약
1. 데이터 일관성 유지
- 정규화된 테이블은 각각의 속성이 독립적으로 관리
- 데이터의 조작이 독립적으로 이루어지므로 일관성 유지
1. 유연성과 확장성
- 정규화된 테이블은 작은 단위로 분리
- 데이터의 확장이나 변경에 특정 테이블만 수정하면 됨

### 정규화의 단점

1. 복잡도 증가
- 정규화된 테이블은 여러 테이블로 분리되어 있음
- 데이터를 검색하거나 조인할 때 복잡성 증가
- 성능의 저하도 일어날 수 있음
1. 추가적인 조인 작업
- 대량의 데이터를 처리할 때 많은 조인 작업이 필요
- 성능 저하 발생
- 이로 인한 역정규화를 할 때도 있음

### 예상질문

1. 이상현상이 무엇인지 어떤것이 있는지 설명해주세요
2. 정규화가 무엇인지 어떤 이유에서 하는지 설명해주세요
3. 정규화의 단계에 대해서 각각 설명해주세요
4. 정규화의 장점과 단점에 대해서 설명해주세요