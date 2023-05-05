# 해시 (Hash)

> 📎 **Hash Function 
: 임의의 데이터를 고정된 길이의 데이터로 매핑하는 단방향 함수.<br>
📎 Hashing 
: 해시함수를 이용해서 데이터를 해시 테이블에 저장하고 검색하는 기법.**


### Hash function

- 보통 복잡하지 않은 알고리즘으로 구현되기 때문에 상대적으로 시스템 자원 소모가 덜하다.
- **해시값 (해시섬, 체크섬)** : 해시 함수를 적용하여 나온 고정된 길이의 값

### Hashing

- 해시함수를 이용해서 데이터를 해시 테이블에 저장하고 검색하는 기법.
- 해시함수는 데이터가 저장되어 있는 곳 (key) 을 알려주기 때문에 다량의 데이터 중에서 **원하는 데이터를 빠르게 찾을 수 있다.**
- 한정된 자원으로 많은 데이터를 효율적으로 관리하기 위해 사용.
- HashSet, HashMap, Hashtable 등이 해싱을 구현하였다.


## 📌 해시 충돌 (Collision)

- 같은 해시값이 나오는 충돌이 발생하면 index가 동일할 수 있다.
- 데이터 개수가 적을 대는 **오픈 어드레싱(Open Addressing)**, 반대의 경우 **개별 체이닝(Seperate Chaining)**이 효율이 좋다.

<img src="https://user-images.githubusercontent.com/102219847/236395930-6ef22328-491d-43a3-a36d-bb4567894b10.png" width="30%">

### 1️⃣ Open Addressing

- 다음에 위치한 index 중 비어있는 곳에 넣음
- 전체 슬롯의 개수 이상은 저장할 수 없다.
- 모든 원소가 반드시 자신의 해시값과 일치하는 주소에 저장된다는 보장은 없다.
- **Linear Probing (선형 탐사)** : 정해진 고정 폭으로 옮겨 해시값의 중복을 피함
- **Quadratic Probing (제곱 탐사)** : 정해진 고정 폭을 제곱수로 옮겨 해시값의 중복을 피함

<img src="https://user-images.githubusercontent.com/102219847/236395794-024decfd-6353-4390-a2de-670b2a0f8ed9.png" width="50%">

- Double Hashing (이중 해싱) : 다음 프로빙으로 또 다른 해시 함수 사용.

### 2️⃣ Separate  Chaining (Closed Addressing)

- 해시테이블의 기본 방식
- 각각의 index를 **연결리스트**로 만들어서 같은 해시값을 가져도 원하는 데이터에 접근 가능
- JAVA의 HashMap이 사용하는 방법

<img src="https://user-images.githubusercontent.com/102219847/236395863-12e67649-bdf9-4550-83cc-5ce46732c69e.png" width="50%">


## ○ 해시테이블 시간 복잡도

- `O(1)` : key값은 해시함수에 의해 고유 index를 가지게 되어 바로 접근할 수 있다.
- 데이터 충돌이 발생한 경우 chaining에 연결된 리스트도 탐색하므로 O(N)까지 증가할 수 있다.
- 테이블이 꽉 차있는 경우라면 테이블을 확장해주어야 하는데, 이는 심각한 성능 저하를 불러오기 때문에 가급적 확장을 하지 않도록 설계해야 한다.


### 예상 질문
- 해시란 무엇이고, 왜 사용하나요?
- 해시 충돌은 무엇이고, 해결방법은 무엇이 있나요?
