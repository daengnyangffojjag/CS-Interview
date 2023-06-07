# File System
### File System 이란?
파일 시스템은 컴퓨터에서 파일이나 자료를 쉽게 발견 및 접근할 수 있도록 보관 또는 조직하는 체제를 가리키는 말이다.
### File System의 구성
![](https://velog.velcdn.com/images/tkdtkd97/post/1238d800-a03b-4b50-8b06-41e994606b1e/image.png)

> 파일(File)

컴퓨터에서 데이터를 저장하는 데 사용되는 단위이다.

> 파티션(Partition)

연속된 저장 공간을 하나 이상의 연속되고 독립적인 영역으로 나누어 사용할 수 있도록 정의한 규약이다. 하나의 물리적 디스크 안에 여러 파티션을 두는 게 일반적이지만, 여러 물리적 디스크를 하나의 파티션으로 구성하기도 한다.
ex. c드라이브 d드라이브

> 디렉토리(Directory)

파일들을 분류, 보관하기 위한 개념이며 폴더라고 한다.


### 파일 접근 방법
시스템이 파일에 접근하여 데이터를 읽는 방법에는 3가지가 있다.

> 1. 순차 접근(Sequential Access)

![](https://velog.velcdn.com/images/tkdtkd97/post/9d570e0b-eb50-4b5c-9a96-4bfe7d53b66b/image.png)

가장 단순한 방법으로 파일의 정보가 레코드 순서대로 처리된다. 카세트테이프를 사용하는 방식과 동일하다.
현재 위치에서 읽거나 쓰면 offset이 자동으로 증가하고, 뒤로 돌아가기 위해선 되감기가 필요하다.

> 2. 직접 접근(Random Access)

파일의 레코드를 임의의 순서로 접근할 수 있다. LP 판을 사용하는 방식과 동일하다.

> 3. 색인 접근(Index Access)

![](https://velog.velcdn.com/images/tkdtkd97/post/c4dd567d-f7ec-4dbc-9379-790992680672/image.png)

파일에서 레코드를 찾기 위해 색인을 먼저 찾고 대응되는 포인터를 얻는다. 이를 통해 파일에 직접 접근하여 원하는 데이터를 얻을 수 있다. 따라서 크기가 큰 파일에서 유용하다.

### 디렉터리
> 1단계 디렉터리(Single-Level Directory)

![](https://velog.velcdn.com/images/tkdtkd97/post/959f21d5-d9de-44b9-af78-d9b0640f00a1/image.png)

파일 시스템 내에 하나의 디렉토리만 존재한다.

문제점
폴더가 하나라면 파일이름이 중복됐을때 충돌이 발생한다.
파일이 많아지거나 다수의 사용자가 사용하는 시스템에서는 심각한 제약을 갖는다.

> 2단계 디렉터리(Two-Level Directory)

![](https://velog.velcdn.com/images/tkdtkd97/post/b92bd623-add1-48a4-a8f2-172d7174055b/image.png)

사용자 마다 하나의 디렉토리를 배정한다.
1단계의 파일 이름이 충돌하는 문제가 사라진다.
UFD : 자신만의 사용자 파일 디렉터리
MFD : 사용자의 이름과 계정 번호로 색인되어 있는 디렉터리. 각 엔트리는 사용자의 UFD를 가리킨다.

문제점
서브 디렉토리 생성 불가능하다.
사용자간 파일을 공유시 보안이슈가 발생한다.

> 트리 구조 디렉터리(Tree-Structured Directory)

![](https://velog.velcdn.com/images/tkdtkd97/post/c12654ff-09cc-4f23-bf19-265b3bd107cb/image.png)

Tree 형태의 계층적 디렉토리 사용 가능
사용자가 하부 디렉토리 생성/관리 가능
대부분의 OS가 사용

> 비순환 그래프 디렉터리(Acyclic-Graph Directory)

![](https://velog.velcdn.com/images/tkdtkd97/post/24e4226c-b65f-4132-9d79-76d9f8a87722/image.png)

사이클이 없는 그래프는 디렉터리들이 서브디렉터리와 파일들을 공유할 수 있도록 허용한다.
Hierarchical directory structure 확장
Link의 개념 사용 ex. 바로가기
파일을 무작정 삭제하게 되면 현재 파일을 가리키는 포인터는 대상이 사라지게 되기 때문에 파일을 삭제할 때 대상이 없는 포인터(dandling pointer) 를 남긴다. (참조 계수가 0이면 포이터가 존재하지 않아 삭제 가능)

> 일반 그래프 디렉터리(General Graph Directory)

![](https://velog.velcdn.com/images/tkdtkd97/post/5d8ec6af-8803-44af-8637-2d27987fcaff/image.png)

비순환 그래프 디렉터리에서 사이클을 허용한 구조

문제점
파일 탐색시 무한루프를 고려해서 하위 디렉터리가 아닌 파일에 대한 링크만 허용하는 등 조치가 필요하다.




# 참고
https://noep.github.io/2016/02/23/10th-filesystem/
https://www.youtube.com/watch?v=bRwjKvmeyZQ
https://www.youtube.com/watch?v=3VOqyi-wbJU
# 면접 예상 질문
> File System 이 무엇인지 설명해주세요

파일 시스템은 컴퓨터에서 파일이나 자료를 쉽게 발견 및 접근할 수 있도록 보관 또는 조직하는 체제를 가리키는 말입니다.

> 비순환 그래프 디렉터리의 특징을 말해보세요

비순환 그래프 디렉터리는 트리 구조 디렉터리를 확장한 것으로 디렉터리들이 서브디렉터리와 파일들을 공유할 수 있도록 허용합니다.

이름 그대로 순환을 허용하지 않고 파일 삭제할때 해당 파일을 가리키는 포인터는 대상이 사라지게 되기 때문에 삭제하려는 파일을 가리키는 포인터가 없는지 확인을 해야합니다.
