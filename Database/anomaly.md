## 이상(Anomaly)

DB에서 데이터를 조작할 때 릴레이션에서 데이터의 중복, 비일관성, 불일치가 발생하는 현상

---

<aside>
💡 대부분의 이상현상이 발생하는 이유가 데이터의 중복성인데 이를 해결하는 과정이 **정규화**

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b13da8ff-d426-4e1c-90b2-1809425eebe4/Untitled.png)

### 삭제 이상(Deletion Anomaly)

특정 정보를 포함한 행을 삭제할 때 특정 정보와 관련된 다른 정보까지 삭제되는 현상

- 장미란이라는 학생의 정보를 지울 경우 강의실 체육관 103도 같이 사라지게 되어 다른 튜플들이 강의실 103을 사용하지 못하는 경우

### 삽입 이상(Insertion Anomaly)

어떤 정보를 삽입하기 위해 필요한 다른 정보가 없어 NULL을 넣어야 하는 현상

- (403, 백승근, 컴퓨터과, NULL, 알고리즘, NULL) 삽입할 때

### 수정 이상(Update Anomaly)

동일한 정보가 여러 행에 저장되어 있을 때, 일부만 수정되어 데이터의 불일치가 일어나는 현상

- 박지성 학생의 DB수업의 강의실을 공학관 112로 바꿨을 때 김연아 학생의 데이터베이스 수업의 강의실은 그대로 공학관 110으로 강의실이 달라짐