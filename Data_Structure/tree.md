# 🎄 트리

### 📎 그래프 (Graph)

💡 **그래프 : 노드(node)와 노드 사이에 연결된 간선(edge)의 정보를 가지고 있는 자료구조**


![[https://laboputer.github.io/ps/2017/09/29/graph/](https://laboputer.github.io/ps/2017/09/29/graph/)](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4a975d09-5a1e-4e2e-b47b-2fc6b0f75eb1/Untitled.png)

[https://laboputer.github.io/ps/2017/09/29/graph/](https://laboputer.github.io/ps/2017/09/29/graph/)

- ‘서로 다른 개체가 연결되어 있다 → 그래프 알고리즘 고려
- ex) 여러개의 도시가 연결되어 있다.

### 📎 트리 (Tree)

- 트리는 그래프 중에서도 특수한 케이스에 해당하는 자료 구조이다.

<aside>
💡 **트리 : 두 개의 노드 사이에 반드시 1개의 경로만을 가지며 사이클이 존재하지 않는 방향 그래프이다.**

</aside>

![참고:[https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/83d51f01-229d-4440-ab83-ecd3db3b5a32/Untitled.png)

참고:[https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)

- 방향 그래프
- 계층적 구조 (부모-자식 관계)
- 사이클, 자체 간선 (self-loop)가 불가능
- 모든 자식 노드는 하나의 부모 노드를 가지며, 따라서 루트 노드는 하나만 존재

  → 노드가 N개인 트리는 항상 N-1개의 간선을 가짐

- 임의의 두 노드 간의 경로는 유일

## 📌 트리 종류

### ○ 편향 트리 (skew tree)

- 모든 노드들이 자식을 하나만 가진 트리
- 왼쪽 방향으로 자식을 하나만 가지면 left skew tree, 오른쪽으로 가지면 right skew tree라고 한다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3da52a42-cbc4-4cf5-b4cf-c201cce73df0/Untitled.png)


### ○ 이진 트리 (Binary Tree)

- 각 노드의 차수 (자식 노드)가 2 이하인 트리

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dbd91568-314e-4b2f-af77-4a1ea27661ef/Untitled.png)


### ○ 이진 탐색 트리 (Binary Search Tree, BST)

- 순서화된 이진 트리 : 자식 노드가 2개 이하
- **탐색에 최적화**된 이진 트리, 하지만 재구성 할 때 트리 구조를 만들고 유지할 때 오버헤드가 크다.
- 노드의 왼쪽 자식은 부모의 값보다 작은 값을 가지며, 오른쪽 자식은 부모보다 큰 값을 가진다. (**왼쪽 자식 노드 < 부모 노드 < 오른쪽 자식 노드**)
- 중위 순회로 정렬된 순서를 읽을 수 있다.

![https://blog.penjee.com/wp-content/uploads/2015/11/binary-search-tree-sorted-array-animation.gif](https://blog.penjee.com/wp-content/uploads/2015/11/binary-search-tree-sorted-array-animation.gif)

### ○ m원 탐색 트리 (M-way Search Tree)

- 최대 m개의 서브 트리를 갖는 탐색 트리
- 이진 탐색 트리의 확장된 형태로 높이를 줄이기 위해 사용

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a6d3dba1-e6be-4ceb-ab1d-825015fc68a0/Untitled.png)

- 다원 탐색 트리는 스스로 균형을 유지하기 못하기 때문에 불균형이 발생할 수 있으며, 이 경우 검색 성능이 떨어지게 된다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04f8c13d-c105-44b4-b531-72df378884a6/Untitled.png)

- 이를 보완하기 위해 스스로 균형을 유지하는 `Balanced Tree`가 있다.

### ○ 균형 트리 (B-Tree, Balanced Tree)

- m원 탐색 트리에서 높이 균형을 유지하는 트리
- 노드의 key의 수가 k개라면, 자식 node의 수는 k+1개이다.
- 루트와 잎 노드를 제외한 트리의 각 노드는 최소 m/2보다 크거나 같은 정수의 서브트리를 갖는다.
- 트리의 루트는 최소 2개의 서브트리를 갖는다.
- 트리의 모든 잎 노드는 같은 레벨에 있다.
- B+ tree, B* tree도 있다. ([참고](https://ssocoit.tistory.com/217))

![[https://hayden-archive.tistory.com/392](https://hayden-archive.tistory.com/392)](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44b89e88-c005-449b-887c-601a7994327e/Untitled.png)

[https://hayden-archive.tistory.com/392](https://hayden-archive.tistory.com/392)

## 📌 트리의 적용

**1️⃣ Spanning trees (신장 트리)**

- 그래프 내의 모든 정점을 포함하는 트리 구조
- 최소의 간선을 이용하여 모든 노드를 연결하고자 하는 경우

  → N개의 노드를 (N-1)개의 간선으로 연결

- 예) 통신 네트워크, 라우터에서 사용되는 최단 경로

**2️⃣ 이진 탐색 트리**

- 데이터를 정렬된 상태로 유지하는데 도움이 된다.

**3️⃣ 계층적 구조 데이터**

- 트리는 데이터를 계층 구조로 저장한다.
- 예) 폴더와 파일

**4️⃣ Syntax tree**

- 구문 트리는 컴파일러에서 사용되는 프로그램 소스 코드의 구조를 나타낸다.
- 컴파일러는 계층 구조로 된 정보를 추출하여 Syntax Tree로 만든다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6760035e-1eea-48b1-a533-6eb2987e82d5/Untitled.png)


**5️⃣ Trie**

- 문자열을 빠르게 탐색할 수 있는 자료구조이다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/36853d9e-4c86-4258-916c-91ffeb71d8d9/Untitled.png)


**6️⃣ Heap**

- priority queue을 구현하는데 사용될 수 있다.
- 배열 형태로 표현할 수 있는 트리 데이터 구조이다.
- 예) 최소 힙은 항상 부모 노드가 자식 노드보다 크기가 작은 자료구조로 집합에서 가장 작은 수만을 꺼내올 때 유용한 자료구조이다.

**7️⃣ DB indexing**

- 데이터 베이스 인덱싱을 구현하는데 트리를 사용한다.
- 예) B-Tree, B+ Tree, AVL Tree

## 📌 트리 구조 장점

- 효율적인 삽입, 삭제, 검색이 가능
- 계층적인 구조를 가진다.
- List, LinkedList보다 적은 메모리 공간을 사용한다.

## 📌 트리 구조 단점

- 포인터를 위한 추가 메모리가 필요하다.
- 다른 데이터 구조에 비해 확장성이 제한된다.
- 대용량 데이터에는 적합하지 않다.
- 불균형이 심해지면 성능이 저하되고, 효율성이 떨어진다.

## 📌 트리의 순회

트리의 모든 노드들을 방문하는 과정을 트리 순회 (Treee Traversal)이라고 한다.

전위 순회 (Pre-order), 중위 순회 (In-order), 후위 순회 (Post-order) 세 가지가 있으며, 보통 재귀로 구현된다.

### ○ 전위 순회 (Pre-order)

전위 순회는 깊이 우선 순회 (Depth-First Traversal)이라고도 한다.

트리를 복사하거나, 전위 표기법을 구하는데 주로 사용된다. 트리를 복사할 때 전위 순회를 사용하는 이유는 트리를 생성할 때 자식 노드보다 부모 노드가 먼저 생성되어야 하기 때문이다.

1. root 노드를 방문한다.
2. 왼쪽 서브 트리를 전위 순회한다.
3. 오른쪽 서브 트리를 전위 순회 한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/121c0c25-4c5c-4b80-92b0-9cf444ec7ae2/Untitled.png)

`A → B → D → E → C → F → G`

### ○ 중위 순회 (In-order)

중위 순회는 왼쪽 오른쪽 대칭으로 순회하기 때문에 대칭 순회 (Symmetric traversal)이라고도 한다.

이진 탐색 트리에서 오른차순 또는 내림차순으로 값을 가져올 때 사용한다.

1. 왼쪽 서브 트리를 중위 순회한다.
2. root 노드를 방문한다.
3. 오른쪽 서브 트리를 중위 순회한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/243678d6-27b8-454b-aec7-f3f0dc21407d/Untitled.png)

`D → B → E → A → F → C → G`

### ○ 후위 순회 (Post-order)

후위 순회는 주로 트리를 삭제하는데 사용된다.

이유는 부모노드를 삭제하기 전에 자식 노드를 삭제하는 순으로 노드를 삭제해야 하기 때문이다.

1. 왼쪽 서브트리를 후위 순회한다.
2. 오른쪽 서브트리를 후위 순회한다.
3. root노드를 방문한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ec303309-42e8-49b0-a2ba-71f08afbd1ea/Untitled.png)

`D → E → B → F → G → C → A`

### 참고
[https://www.geeksforgeeks.org/introduction-to-tree-data-structure-and-algorithm-tutorials/](https://www.geeksforgeeks.org/introduction-to-tree-data-structure-and-algorithm-tutorials/)
[https://yoongrammer.tistory.com/68](https://yoongrammer.tistory.com/68)