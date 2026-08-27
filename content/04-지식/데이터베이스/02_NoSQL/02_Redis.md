---
publish: true
tags: [지식, 데이터베이스]
---

# Redis 개요

---

## Redis란?

> **Redis (Remote Dictionary Server)**는 오픈소스 인메모리 데이터 구조 저장소로, Key-Value 형태의 NoSQL 데이터베이스입니다. 메모리에 데이터를 저장하여 매우 빠른 성능을 제공하며, 캐싱, 세션 관리, 메시지 브로커 등 다양한 용도로 사용됩니다.

---

## Redis의 특징

### 1. 인메모리 저장
* 데이터를 RAM에 저장
* 디스크 I/O 없이 매우 빠른 성능
* 마이크로초 단위 응답 시간

### 2. 다양한 데이터 타입
* String, List, Set, Sorted Set, Hash
* Bitmaps, HyperLogLog, Streams
* 다양한 데이터 구조 지원

### 3. 영속성 옵션
* RDB (스냅샷)
* AOF (Append Only File)
* 영속성과 성능 사이 선택 가능

### 4. 고가용성
* Redis Sentinel (고가용성)
* Redis Cluster (분산)
* 복제(Replication) 지원

### 5. 원자성 연산
* 모든 명령어가 원자적
* 트랜잭션 지원
* Lua 스크립트 실행

---

## Redis 설치

### Linux (Ubuntu/Debian)
```bash
# 패키지 업데이트
sudo apt update

# Redis 설치
sudo apt install redis-server

# Redis 시작
sudo systemctl start redis-server
sudo systemctl enable redis-server

# Redis 상태 확인
redis-cli ping
```

### Linux (CentOS/RHEL)
```bash
# EPEL 저장소 추가
sudo yum install epel-release

# Redis 설치
sudo yum install redis

# Redis 시작
sudo systemctl start redis
sudo systemctl enable redis
```

### macOS
```bash
# Homebrew를 통한 설치
brew install redis

# Redis 시작
brew services start redis
```

### Docker
```bash
# Redis 컨테이너 실행
docker run -d -p 6379:6379 --name redis redis:latest

# Redis CLI 접속
docker exec -it redis redis-cli
```

---

## Redis 기본 명령어

### 연결 및 기본 명령
```bash
# Redis CLI 접속
redis-cli

# 서버 연결 테스트
PING
# 응답: PONG

# 서버 정보
INFO

# 모든 키 목록 (주의: 프로덕션에서 사용 금지)
KEYS *

# 키 개수
DBSIZE

# 키 존재 확인
EXISTS key

# 키 삭제
DEL key

# 키 만료 시간 설정
EXPIRE key 60  # 60초 후 만료
TTL key        # 남은 시간 확인
```

### String (문자열)
```bash
# 값 설정
SET mykey "Hello Redis"

# 값 가져오기
GET mykey

# 값이 없을 때만 설정
SETNX mykey "value"

# 값 증가/감소
INCR counter
DECR counter
INCRBY counter 10

# 여러 값 설정/가져오기
MSET key1 "value1" key2 "value2"
MGET key1 key2

# 문자열 일부 가져오기
GETRANGE mykey 0 4
```

### List (리스트)
```bash
# 왼쪽에 추가
LPUSH mylist "item1"
LPUSH mylist "item2"

# 오른쪽에 추가
RPUSH mylist "item3"

# 왼쪽에서 제거
LPOP mylist

# 오른쪽에서 제거
RPOP mylist

# 리스트 조회
LRANGE mylist 0 -1  # 전체 조회

# 리스트 길이
LLEN mylist

# 인덱스로 값 가져오기
LINDEX mylist 0
```

### Set (집합)
```bash
# 값 추가
SADD myset "member1"
SADD myset "member2"

# 모든 멤버 조회
SMEMBERS myset

# 멤버 존재 확인
SISMEMBER myset "member1"

# 멤버 제거
SREM myset "member1"

# 집합 크기
SCARD myset

# 교집합
SINTER set1 set2

# 합집합
SUNION set1 set2

# 차집합
SDIFF set1 set2
```

### Sorted Set (정렬된 집합)
```bash
# 점수와 함께 추가
ZADD leaderboard 100 "player1"
ZADD leaderboard 200 "player2"

# 점수 범위로 조회
ZRANGE leaderboard 0 -1 WITHSCORES

# 역순 조회
ZREVRANGE leaderboard 0 -1 WITHSCORES

# 점수 조회
ZSCORE leaderboard "player1"

# 순위 조회
ZRANK leaderboard "player1"
ZREVRANK leaderboard "player1"

# 점수 범위로 개수 조회
ZCOUNT leaderboard 100 200
```

### Hash (해시)
```bash
# 필드 설정
HSET user:1001 name "John" email "john@example.com"

# 필드 가져오기
HGET user:1001 name

# 모든 필드 가져오기
HGETALL user:1001

# 여러 필드 설정
HMSET user:1001 name "John" age 30

# 필드 존재 확인
HEXISTS user:1001 name

# 필드 삭제
HDEL user:1001 email

# 모든 필드 이름
HKEYS user:1001

# 모든 값
HVALS user:1001
```

---

## Redis 고급 기능

### Pub/Sub (발행/구독)
```bash
# 채널 구독
SUBSCRIBE news

# 다른 터미널에서 메시지 발행
PUBLISH news "Hello World"

# 패턴 구독
PSUBSCRIBE news.*

# 구독 취소
UNSUBSCRIBE news
```

### 트랜잭션
```bash
# 트랜잭션 시작
MULTI

# 명령어 추가
SET key1 "value1"
SET key2 "value2"
INCR counter

# 트랜잭션 실행
EXEC

# 트랜잭션 취소
DISCARD
```

### Lua 스크립트
```bash
# Lua 스크립트 실행
EVAL "return redis.call('GET', KEYS[1])" 1 mykey

# 스크립트 파일 실행
redis-cli --eval script.lua key1 key2 , arg1 arg2
```

### 파이프라인
```bash
# 여러 명령어를 한 번에 실행
(echo -en "PING\r\nPING\r\nPING\r\n"; sleep 1) | nc localhost 6379
```

---

## Redis 영속성

### RDB (스냅샷)
```bash
# redis.conf 설정
save 900 1      # 900초 동안 1개 이상 변경 시 저장
save 300 10     # 300초 동안 10개 이상 변경 시 저장
save 60 10000   # 60초 동안 10000개 이상 변경 시 저장

# 수동 스냅샷
BGSAVE
SAVE
```

### AOF (Append Only File)
```bash
# redis.conf 설정
appendonly yes
appendfsync everysec  # 매초마다 동기화
# appendfsync always  # 항상 동기화 (느림)
# appendfsync no      # OS에 맡김 (빠름)
```

---

## Redis 복제 (Replication)

### 마스터-슬레이브 설정
```bash
# 슬레이브 서버에서
SLAVEOF 192.168.1.100 6379

# 또는 redis.conf
replicaof 192.168.1.100 6379

# 복제 정보 확인
INFO replication
```

---

## Redis Sentinel (고가용성)

### Sentinel 설정
```bash
# sentinel.conf
sentinel monitor mymaster 127.0.0.1 6379 2
sentinel down-after-milliseconds mymaster 5000
sentinel failover-timeout mymaster 10000
```

### Sentinel 실행
```bash
redis-sentinel sentinel.conf
```

---

## Redis Cluster (분산)

### 클러스터 설정
```bash
# 클러스터 노드 생성
redis-cli --cluster create \
  127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 \
  127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 \
  --cluster-replicas 1

# 클러스터 정보
redis-cli -c -p 7000 CLUSTER INFO

# 노드 정보
redis-cli -c -p 7000 CLUSTER NODES
```

---

## Redis 사용 사례

### 1. 캐싱
```python
# Python 예시
import redis

r = redis.Redis(host='localhost', port=6379, db=0)

# 캐시 설정
r.setex('user:1001', 3600, 'user_data')  # 1시간 만료

# 캐시 가져오기
user_data = r.get('user:1001')
```

### 2. 세션 관리
```python
# 세션 저장
r.setex('session:abc123', 1800, session_data)  # 30분 만료

# 세션 조회
session = r.get('session:abc123')
```

### 3. 카운터
```bash
# 페이지 조회수
INCR page:views:article:1001

# 일일 사용자 수
PFADD daily:users:2024-01-01 user:1001
PFCOUNT daily:users:2024-01-01
```

### 4. 실시간 랭킹
```bash
# 점수 추가
ZADD leaderboard 100 "player1"
ZADD leaderboard 200 "player2"

# 상위 10명 조회
ZREVRANGE leaderboard 0 9 WITHSCORES
```

### 5. 메시지 큐
```bash
# 메시지 추가
LPUSH queue:emails "email_data"

# 메시지 처리
RPOP queue:emails
```

---

## Redis 성능 최적화

### 1. 메모리 최적화
```bash
# 메모리 사용량 확인
INFO memory

# 메모리 정리
MEMORY PURGE

# 큰 키 찾기
redis-cli --bigkeys
```

### 2. 연결 풀링
```python
# Python 연결 풀 사용
import redis
pool = redis.ConnectionPool(host='localhost', port=6379, db=0)
r = redis.Redis(connection_pool=pool)
```

### 3. 파이프라인 사용
```python
# 여러 명령어를 한 번에 실행
pipe = r.pipeline()
pipe.set('key1', 'value1')
pipe.set('key2', 'value2')
pipe.execute()
```

### 4. 적절한 데이터 타입 선택
* String: 단순 값
* Hash: 객체 표현
* List: 큐, 스택
* Set: 고유 값 집합
* Sorted Set: 순위, 범위 쿼리

---

## Redis 모니터링

### 명령어
```bash
# 실시간 통계
MONITOR

# 서버 정보
INFO

# 특정 섹션 정보
INFO memory
INFO stats
INFO clients

# 느린 쿼리 로그
SLOWLOG GET 10
```

### 메트릭
* 메모리 사용량
* 연결 수
* 명령어 실행 횟수
* 키 개수
* 히트율 (Hit Rate)

---

## Redis 보안

### 비밀번호 설정
```bash
# redis.conf
requirepass your_password

# 연결 시
redis-cli -a your_password
```

### 네트워크 보안
```bash
# 특정 IP만 허용
bind 127.0.0.1

# 방화벽 설정
# 포트 6379 접근 제한
```

### 명령어 비활성화
```bash
# 위험한 명령어 비활성화
rename-command FLUSHDB ""
rename-command FLUSHALL ""
rename-command CONFIG ""
```

---

## Redis 모범 사례

### 1. 키 네이밍
* 명확하고 일관된 네이밍
* 콜론(:)으로 계층 구조 표현
* 예: `user:1001:profile`, `session:abc123`

### 2. 만료 시간 설정
* 모든 키에 적절한 TTL 설정
* 메모리 누수 방지
* `SETEX`, `EXPIRE` 활용

### 3. 메모리 관리
* 최대 메모리 설정
* LRU 정책 사용
* 큰 키 모니터링

### 4. 연결 관리
* 연결 풀 사용
* 적절한 타임아웃 설정
* 연결 수 모니터링

### 5. 백업
* RDB 스냅샷 정기적 생성
* AOF 활성화
* 백업 파일 검증

---

## Redis의 장점

* **매우 빠른 성능**: 인메모리 저장으로 마이크로초 단위 응답
* **다양한 데이터 타입**: 다양한 데이터 구조 지원
* **원자성 연산**: 모든 명령어가 원자적
* **고가용성**: Sentinel, Cluster 지원
* **풍부한 기능**: Pub/Sub, Lua 스크립트 등

---

## Redis의 단점

* **메모리 제한**: RAM 크기에 제한
* **데이터 손실 위험**: 메모리 기반으로 인한 위험
* **비용**: 대용량 메모리 필요
* **복잡한 쿼리**: 단순한 Key-Value 쿼리만 지원

---

## 참고

* Redis는 캐싱과 세션 관리에 최적화된 도구입니다
* 메모리 관리를 신중하게 계획하세요
* 적절한 만료 시간을 설정하여 메모리 누수를 방지하세요
* 프로덕션 환경에서는 고가용성 구성을 고려하세요
* 정기적인 모니터링과 백업이 필수입니다

