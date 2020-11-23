# Cache

### 캐시

- 정의
  - 데이터나 값을 미리 복사해 놓은 임시 장소
- 캐시의 지역성
  - 시간적 지역성 : 특정 데이터가 한번 접근되었을 경우, 가까운 미래에 또 한번 데이터에 접근할 가능성이 높은 것
  - 공간적 지역성 : 특정 데이터와 가까운 주소가 순서대로 접근되었을 경우



### 캐시서버

- 웹 서비스의 3구조 : WEB-WAS(Web Application Server)-DB

  - 사용자의 수가 늘어나면 트랜잭션의 수가 많아지면서 DB에 부하가 생긴다.
  - DB의 부하를 줄이기 위해 캐시서버를 도입하는 것을 고려해야한다.
    - 캐시서버는 트랜잭션 과정에서 DB에 접근하기 전에 캐시서버에 저장된 값을 참조하여 DB의 부하를 줄여준다는 장점이 있다.
    - 캐시서버에 값이 저장되어 있으면 캐시히트, 없다면 캐시미스
    - 하지만, 캐시서버는 속도 향상을 위하여 주로 메모리를 사용하기 때문에 서버에 장애가 일어나면 메모리에 저장된 데이터가 손실된다는 단점이 있다.

  

### Redis(Remote Dictionary Server)

- Key-Value pair(NoSQL) 기반의 In-memory 데이터 저장소
  - Physical Memory 이상을 사용하면 문제가 발생
  - Maxmemory 설정 : 사용 가능한 최대 메모리? - 실제로는 더 사용할 수도 있음!
  - RSS 값을 모니터링 해야한다.
    - RSS(Resident Set Size) : OS가 Redis instance를 할당하기 위해 사용한 물리적 메모리 양
- Collection 제공!
- Single Thread : 1번에 1개의 명령어만을 수행, 서버 하나에 여러개의 서버를 띄울 수도 있다.
  - 큰 메모리를 사용하는 하나의 instance보다 적은 메모리를 사용하는 instance 여러개가 안전
- 다양한 데이터 구조 : String, List, Set, Sorted Set(리더보드 기능), Hash
  - 리스트, 배열과 같은 데이터를 처리하는데 유리
  - 리스트형 데이터 입력과 삭제가 다른 DB에 비해 월등히 빠르다.(약 10배)
  - 메모리를 활용하면서 영속적인 데이터 보존
- 만료일을 지정하여 만료가 되면 자동으로 데이터를 삭제한다.
- Snapshot 기능 지원 : 특정 시점의 데이터를 디스크에 저장 가능, 주로 장애 상황 복구 시에 사용
- Master - Slave 구조 : 여러 개의 복사본 제작 가능, Read Replica 적용 가능, 데이터 분실 위험을 줄여줌
  - Master는 CUD가 가능하지만 Slave는 R만 가능하다.
- 샤딩(Sharding) 기능 지원
  - 샤딩 : 파티셔닝(Partitioning)과 동일하다. 같은 테이블 스키마를 가진 데이터를 다수의 데이터베이스에 분산하여 저장하는 방법
- Redis Cluster
  - Clinet - Primary(Master) - Secondary(Slave)
  - Primary 서버가 죽으면 Secondary 서버가 Primary 서버가 된다.
  - 장점
    - 자체적인 Primary - Secondary Failover(시스템 대체 작동)
    - Slot 단위의 체계적인 데이터 관리
  - 단점
    - 메모리 사용량이 많음
    - Migration 자체가 관리자가 시점을 결정함
    - Library 구현이 필요함



### Memcached

- Redis와 같은 Key-Value pair In-memory 데이터 저장소, 처리 속도가 빠르다.
- Collection 제공 X
- 디스크를 거치지 않는다. 불의의 경우에는 메모리에 저장된 데이터가 손실된다.
- 만료일을 지정하여 만료가 되면 자동으로 데이터를 삭제한다.
- 저장소 메모리를 재사용한다. (LRU - Least Recently Used 알고리즘 참조)
- Multi Thread 지원
- 단일 데이터 구조 : String



![daangn-redis vs memcahced](C:/Users/multicampus/Downloads/IT 질문/작성중/image/daangn-redis-vs-memcahced.jpeg)
