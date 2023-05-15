# Redis 발표 - 예지

> **RE**mote **DI**ctionary **S**erver
‘키-값’ 구조의 데이터를 저장하고 관리하기 위한 오픈 소스 기반의 **in-momory data structure store**
> 

- In-Memory Data Strucrue store
- Open Source (BSD 3 License)
- Support data structures
    - Strings, set, sorted-set, hashes, list
    - Hyperloglog, bitmap, geospatial index
    - Stream

## 📌 특징

**○ In-memory data structure**

문자열, hash, list, set, sorted set, stream 등의 자료형을 지원하는 “data structure server”

**○ Programmability**

Lua를 사용한 server-side scripting과 Redis Functions를 이용한  server-side 저장 과정

**○ Persistence**

빠른 액세스를 위해 데이터 세트를 메모리에 보관하지만, 재부팅 및 시스템 오류가 발생해도 영구 스토리지에 대한 모든 쓰기를 유지할 수도 있다.

**○ Clusturing**

해시 기반 샤딩을 통한 수평적 확장성, 클러스터 성장 시 자동 재파티션을 통해 수백만 개의 노드로 확장.

**○ High availability**

독립 실행형 및 클러스터된 배포 모두에 대해 자동 장애 조치가 포함된 replication.

<img src="https://github.com/daengnyangffojjag/CS-Interview/assets/102219847/35858207-73b5-46dc-aea6-fd1f499ed1b7" width="80%">


### ✅ Collection이 필요한 이유

#### 1. **개발 난이도가 낮아진다.**

멤캐시드는 collection을 제공하지 않는다. Redis는 제공한다.
라이브러리로 미리 구현해놓으면 훨씬 편하게 개발하는 것처럼 멤캐시드는 구현해야할 것이 많으나 **Redis는 구현해놓은 것이 많아서 쓰기만 하면 된다.** 
예를 들어, 랭킹 서버에서 DB에 넣고 정렬을 하면 갯수가 많아질수록 느려진다. 
하지만, Redis의 sorted-set을 이용하면 개발 난이도가 낮아지게 된다.

#### 2. **문제를 줄여줄 수 있다.**

Race condition : transaction이 두개가 동시에 진행될 때, ACID하지 않을 수 있다.
Redis는 자료구조가 Atomic하기 때문에 Race Condition을 피할 수 있다. 
(그래도 잘못짜면 발생 가능 ex. 따닥)
→ 외부의 Collection을 잘 이용하는 것으로 여러가지 개발 시간을 단축시키고, 문제를 줄여줄 수 있기 때문에 Collection이 중요하다.


### ✅ Redis의 사용

- Remote Data Store
    - A서버, B서버, C서버에서 데이터를 공유하고 싶을 때
- 한 대에서만 필요하다면, 전역 변수를 쓰면 되지 않을까?
    - Redis 자체가 single thread라 Atomic을 보장해준다. -> 동시의 하나의 명령만 처리할 수 있다.
- 주로 많이 쓰는 곳들
    - 인증 토큰 등을 저장 (String 또는 hash)
    - Ranking 보드로 사용 (Sorted set)
    - 유저 API Limit
    - 좌표 (list)

## 📌 Redis Collections

- **Strings**
    - key-value
    - key를 어떻게 설정하느냐에 따라 분산이 바뀔 수 있다.
- **List**
    - 데이터 추가 : Lpush, Rpush / 데이터 꺼내기 : LPOP, RPOP
        
        `Lpush <key> <data>`
        
    - 좌표 저장에 많이 사용
- **Set**
    - `SADD <key> <value>` : value가 이미 key에 있으면 추가되지 않는다.
    - `SMEMBERS <key>` : 모든 value를 돌려줌
    - `SISMEMBER <key> <value>` : value가 존재하면 1, 없으면 0
    - 특정 유저를 follow하는 목록을 저장
- **Sorted-set**
    - `ZADD <key> <score> <value>` : score 값으로 정렬된다. value가 이미 key에 있으면 해당 score로 변경된다.
    - `ZRANGE <key> <startindex> <endindex>` : 해당 index 범위 값을 모두 돌려준다.
    - Zrange testkey 0 -1 : 모든 범위를 가져온다.
    - 유저 랭킹 보드로 사용할 수 있다.
    - **⭐️ score 값은 double 타입이기 때문에 값이 정확하지 않을 수 있다.**
- **Hash**
    - key-value 밑에 sub key가 존재한다.
    - `Hmset <key> <subkey1> <value1> <subkey2> <value2>`
    - `Hgetall <key>` : 해당 key의 모든 subkey와 value를 가져온다.
    - `Hget <key> <subkey>`
    - `Hmget <key> <subkey1> <subkey2> .. <subkeyN>`

📎 **Messaging**

- List
    - 자체적인 Blocking 기능을 이용해 event queue로 사용 가능하다.
    - 불필요한 polling process를 막을 수 있다.
    - 트위터에서 타임라인을 구성하기 위해 `RPUSHX`를 사용한다.
        
        → 자주 접속하는 유저는 캐싱을 하고, 사용하지 않으면 삭제되어 key값이 없어진다. `RPUSHX`를 사용하면 키가 있을 때만 데이터 저장이 가능하다.
        
- Streams
    - 로그를 저장하기 가장 적절한 자료구조
    - append-only 방식
    - 시간 범위로 검색 / 신규 추가 데이터 수신 / 소비자별 다른 데이터 수신 (소비자 그룹)
    - kafka를 간단히 대체할 수 있다고 함

## 📌 Redis Single Thread
Redis 4.0 부터는 기본적으로 4개의 쓰레드로 동작하지만 일반 명령어를 처리하는 메인쓰레드 1개와 별도의 시스템 명령들을 사용하는 전용 sub trhead 3개 로써, 실제로 사용자가 사용하는 명령어들을 싱글쓰레드로 동작한다고 생각하면 된다.
Redis는 데이터를 atomic 하게 유지하는 목적으로 싱글스레드 - Eventloop
레디스는 **Event Loop(이벤트루프)**를 이용하여 요청을 수행한다.

즉, 실제 명령에 대한 작업(Task)는 커널 I/O 레벨에서 Multiplexing(멀티플렉싱)을 통해 처리하여 동시성을 보장한다.

따라서, 유저 레벨에서는 싱글스레드로 동작하지만, 커널 I/O 레벨에서는 스레드풀을 이용한다.

Redis가 싱글스레드인 4가지 이유
1. CPU는 병목현상의 원인이 아님
  레디스의 병목현상의 대부분은 CPU가 아닌 시스템 메모리/네트워크 대역폭에서 발생
2. 동시성을 보장 (병렬성과는 다름)
  이벤트루프 패턴을 통해 동시성을 구현하였고 Context-Switch(문맥교환)가 없어 자원을 절약
3. 쉬운 구현 (프로그래밍 용이성)
4. 쉬운 배포
  최소 한 개의 Core(코어)만 있어도 사용할 수 있기 때문에 배포/사용이 쉽다.

- atomic: 여러 개의 스레드가 하나의 공유 자원에 접근하여 sync가 안되는 상황에 결과값이 달라지는 것. 순차 처리 보장X
- deadlock: 여러개의 스레드가 lock에 대해서 계속 대기 상태
- *Thread safe
  변수,객체 등 여러 스레드에서 동시 접근 이루어져도 프로그램 실행에 문제 없는 것


## 📌 데이터 분산

- 데이터의 특성에 따라서 선택할 수 있는 방법이 달라진다

### ○ Appliation 레벨

1️⃣ **Consistent Hashing**

- twemproxy를 사용하는 방법으로 쉽게 사용 가능하다
- 서버가 추가되거나 빠지더라도 해당 서버에 있는 데이터들만 리밸런싱이 일어난다.

2️⃣ **Sharding**

- 상황마다 샤딩 방법이 달라진다
- Range
    - 특정 range를 정의하고 해당 range에 속하면 거기에 저장한다.
    - 확장은 편하지만, 당시 서버 상황에 따라 놀고 있는 서버와 열일하는 서버가 나뉜다.
- Modular
    - 균등하게 분배되지만, 서버 한대가 추가될 때 재분배 양이 많아진다.
- Indexed
    - 해당 key가 저장되어야할 관리 서버가 따로 존재한다.
    - 서버가 2배로 늘어나면 반 나눠서 옮긴다.
    - 인덱스 서버가 죽으면 분배가 안 된다.

### ○ Redis cluster

Hash 기반으로 slot 16384로 구분한다.

- hash 알고리즘은 CRC16을 사용한다.
- slot = crc16(key) % 16384
    
    실제 클러스터가 16384개를 넘을 수 없다.
    
- key가 `key{hashkey}` 패턴이면 실제 crc16에 hashkey가 사용된다.
    
    `key{hashkey}` : 원하는 서버로 보낼 수 있다.
    
- 특정 redis 서버는 이 `slot range`를 가지고 있고, 데이터 migration은 이 slot 단위의 데이터를 다른 서버로 전달하게 된다. (migrateCommand 이용)
  
  <img src="https://github.com/daengnyangffojjag/CS-Interview/assets/102219847/43a06639-8bde-4157-96e4-7a664bc359ec" width="30%">

- primary 서버가 죽으면 seconary가 primary가 된다
    - primary와 secondary는 replication으로 연결이 되어 있다.
- primary마다 slot range가 정해져있다.
    
    slot이 잘못오면 `-MOVED <primary>` 응답을 하고 이에 따라서 클라이언트가 요청을 다시 맞는 primary에 보낸다 
    
    → 이러한 처리는 보통 라이브러리에 구현이 되어있고, 직접 쓰려고 하면 구현을 해줘야 한다.
    
    → 레디스 클러스터는 라이브러리 의존적이다.
    

**✅ 장점**

- 자체적으로 헬스체크를 하고 primary, secondary failover를 한다.
- slot 단위의 데이터처리를 할 수 있다.

**✅ 단점**

- 슬롯 관리 등으로 메모리 사용량이 더 많따.
- migration 자체는 관리자가 시점을 결정해야 한다.
- Library 구현이 필요하다.



## 📌 영구 저장 (Redis persistence)

redis를 캐시 이외의 상황에 적용한다면 적절한 백업이 필요하다. 

**AOF (Append Only File)**

: 데이터 변경하는 커맨드를 모두 저장

- 파일이 점점 커지기 때문에 압축해서 다시 저장해야 한다.
- 자동 : redis.conf 파일에서 auto-aof-rewrite-percentage 옵션 (크기 기준)
- 수동 : `BGREWRITEAOF` 커맨드를 이용해 cli 창에서 수동으로 AOF 파일 생성

**RDB (snapshot)**

: 저장 당시 파일을 저장한다.

- 자동 : redis.conf 파일에서 SAVE 옵션 (시간 기준)
- 수동 : `BGSAVE` 커맨드를 이용해 cli 창에서 수동으로 RDB 파일 저장 가능
    - SAVE 커맨드는 절대 사용 X

### ✅ 선택 기준

- 백업은 필요하지만 어느 정도의 데이터 손실이 발생해도 괜찮은 경우
    - RDB 단독 사용
    - redis.conf 파일에서 SAVE 옵션을 적절히 사용
- 장애 상황 직전까지의 모든 데이터가 보장되어야할 경우
    - AOF 사용 (appendonly yes)
    - APPENDFSYNC 옵션이 everysec인 경우 최대 1초 사이의 데이터 유실 가능 (기본 설정)
- 제일 강력한 내구성이 필요한 경우
    - RDB & AOF 동시 사용

## 📌 아키텍쳐 선택

<img src="https://github.com/daengnyangffojjag/CS-Interview/assets/102219847/9194f825-21fe-4d46-960f-4bf8de0933e6" width="80%">

### 1️⃣ Replication 구성

- `replicaof` 커맨드를 이용해서 간단하게 복제 연결
- 비동기식 복제
- HA 기능이 없으므로 장애 상황 시 수동 복구
    - `replicaof no one`으로 수동으로 연결을 끊는다.
    - 애플리케이션에서 연결 정보 변경

### 2️⃣ Sentinel

- 자동 failover 가능한 HA 구성 (High Availability)
- sentinel 노드가 다른 노드를 감시한다.
- 마스터가 비정상 상태일 때 자동으로 failover → replica가 마스터가 된다.
- 연결 정보 변경 필요 없음 - sentinel 정보만 알면 마스터로 바로 연결시켜 준다.
- sentinel 노드는 항상 3대 이상의 홀수로 존재해야 한다.
    - 과반수 이상의 sentinel이 동의해야 failover 진행 가능하다.
    - 마스터와 replica 노드에 sentinel을 같이 띄울 수 있다.

### 3️⃣ Cluster

- 스케일 아웃과 HA 구성 (High Availability)
- 키를 여러 노드에 자동으로 분할해서 저장 (sharidng)
- 모든 노드가 서로를 감시하며, 마스터가 비정상일 때 failover
- 최소 3대의 마스터 노드가 필요하다.

참고 :

[https://redis.io/](https://redis.io/)

[https://www.youtube.com/watch?v=mPB2CZiAkKM](https://www.youtube.com/watch?v=mPB2CZiAkKM)

[https://www.youtube.com/watch?v=jgpVdJB2sKQ](https://www.youtube.com/watch?v=jgpVdJB2sKQ)
     
### 질문
- Redis는 특징은 무엇이고, 언제 사용하는가?
     - 키-값 구조의 인메모리 데이터베이스로 데이터에 접근 속도가 매우 빠릅니다.
     - string, set, sorted set, hash, list 등의 자료구조를 제공한다.
     - 자체적으로 자료구조를 제공하고 빠르게 접근할 수 있기 때문에 로그 저장, 랭킹 보드, JWT 등 토큰 저장에 사용할 수 있다.
     
- Redis는 영속성을 어떻게 보장하는가?
  - 레디스에서는 RDB & AOF를 지원한다. RDB는 snapshot으로 당시 파일들을 모두 저장하는 방식이며, AOF는 이전의 커맨드들을 저장하여  방식이다.
