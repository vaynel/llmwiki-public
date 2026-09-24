---
publish: true
tags: [지식, 데이터베이스]
---

# MongoDB 개요

---

## MongoDB란?

> **MongoDB**는 가장 인기 있는 오픈소스 문서 지향(Document-Oriented) NoSQL 데이터베이스입니다. JSON과 유사한 BSON 형식으로 데이터를 저장하며, 유연한 스키마와 강력한 쿼리 기능을 제공합니다.

---

## MongoDB의 특징

### 1. 문서 지향
* JSON/BSON 형태의 문서 저장
* 스키마리스 (Schema-less)
* 유연한 데이터 구조

### 2. 높은 성능
* 인덱싱 지원
* 수평 확장 (Sharding)
* 빠른 읽기/쓰기

### 3. 강력한 쿼리
* 풍부한 쿼리 언어
* 집계 파이프라인
* 텍스트 검색

### 4. 확장성
* 자동 샤딩
* 복제 (Replication)
* 고가용성

### 5. 개발자 친화적
* 직관적인 API
* 다양한 드라이버
* 풍부한 문서화

---

## MongoDB 설치

### Linux (Ubuntu/Debian)
```bash
# GPG 키 추가
wget -qO - https://www.mongodb.org/static/pgp/server-7.0.asc | sudo apt-key add -

# 저장소 추가
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

# 패키지 업데이트
sudo apt update

# MongoDB 설치
sudo apt install -y mongodb-org

# MongoDB 시작
sudo systemctl start mongod
sudo systemctl enable mongod
```

### Linux (CentOS/RHEL)
```bash
# 저장소 파일 생성
sudo vi /etc/yum.repos.d/mongodb-org-7.0.repo

# 내용 추가
[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/7.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-7.0.asc

# MongoDB 설치
sudo yum install -y mongodb-org

# MongoDB 시작
sudo systemctl start mongod
sudo systemctl enable mongod
```

### macOS
```bash
# Homebrew를 통한 설치
brew tap mongodb/brew
brew install mongodb-community@7.0

# MongoDB 시작
brew services start mongodb-community@7.0
```

### Docker
```bash
# MongoDB 컨테이너 실행
docker run -d -p 27017:27017 --name mongodb mongo:latest

# MongoDB Shell 접속
docker exec -it mongodb mongosh
```

---

## MongoDB 기본 개념

### 데이터베이스
```javascript
// 데이터베이스 사용
use mydb

// 데이터베이스 목록
show dbs

// 현재 데이터베이스
db
```

### 컬렉션 (Collection)
```javascript
// 컬렉션은 자동 생성됨
// 명시적 생성
db.createCollection("users")

// 컬렉션 목록
show collections

// 컬렉션 삭제
db.users.drop()
```

### 문서 (Document)
```javascript
// 문서는 JSON과 유사한 BSON 형식
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "username": "johndoe",
  "email": "john@example.com",
  "age": 30,
  "tags": ["developer", "mongodb"],
  "address": {
    "city": "Seoul",
    "country": "Korea"
  }
}
```

---

## MongoDB 기본 명령어

### 데이터 삽입 (INSERT)
```javascript
// 단일 문서 삽입
db.users.insertOne({
  username: "johndoe",
  email: "john@example.com",
  age: 30
})

// 여러 문서 삽입
db.users.insertMany([
  { username: "jane", email: "jane@example.com", age: 25 },
  { username: "bob", email: "bob@example.com", age: 28 }
])

// 삽입 결과 확인
db.users.find()
```

### 데이터 조회 (FIND)
```javascript
// 모든 문서 조회
db.users.find()

// 예쁘게 출력
db.users.find().pretty()

// 조건부 조회
db.users.find({ age: 30 })

// 비교 연산자
db.users.find({ age: { $gt: 25 } })  // age > 25
db.users.find({ age: { $gte: 25 } })  // age >= 25
db.users.find({ age: { $lt: 30 } })   // age < 30
db.users.find({ age: { $lte: 30 } })  // age <= 30
db.users.find({ age: { $ne: 30 } })   // age != 30

// 논리 연산자
db.users.find({ $and: [{ age: { $gt: 25 } }, { age: { $lt: 35 } }] })
db.users.find({ $or: [{ age: 25 }, { age: 30 }] })
db.users.find({ age: { $not: { $gt: 30 } } })

// IN 연산자
db.users.find({ age: { $in: [25, 30, 35] } })

// 정규식
db.users.find({ email: /@gmail\.com$/ })

// 프로젝션 (특정 필드만)
db.users.find({}, { username: 1, email: 1 })

// 정렬
db.users.find().sort({ age: 1 })   // 오름차순
db.users.find().sort({ age: -1 })  // 내림차순

// 제한
db.users.find().limit(10)
db.users.find().skip(10).limit(10)  // 페이징

// 개수
db.users.countDocuments({ age: { $gt: 25 } })
```

### 데이터 수정 (UPDATE)
```javascript
// 단일 문서 수정
db.users.updateOne(
  { username: "johndoe" },
  { $set: { age: 31 } }
)

// 여러 문서 수정
db.users.updateMany(
  { age: { $lt: 30 } },
  { $set: { status: "young" } }
)

// 필드 추가
db.users.updateOne(
  { username: "johndoe" },
  { $set: { phone: "010-1234-5678" } }
)

// 필드 제거
db.users.updateOne(
  { username: "johndoe" },
  { $unset: { phone: "" } }
)

// 배열에 추가
db.users.updateOne(
  { username: "johndoe" },
  { $push: { tags: "newtag" } }
)

// 배열에서 제거
db.users.updateOne(
  { username: "johndoe" },
  { $pull: { tags: "newtag" } }
)

// 값 증가
db.users.updateOne(
  { username: "johndoe" },
  { $inc: { age: 1 } }
)

// Replace (문서 전체 교체)
db.users.replaceOne(
  { username: "johndoe" },
  { username: "johndoe", email: "newemail@example.com" }
)
```

### 데이터 삭제 (DELETE)
```javascript
// 단일 문서 삭제
db.users.deleteOne({ username: "johndoe" })

// 여러 문서 삭제
db.users.deleteMany({ age: { $lt: 18 } })

// 모든 문서 삭제 (주의!)
db.users.deleteMany({})
```

---

## MongoDB 인덱싱

### 인덱스 생성
```javascript
// 단일 필드 인덱스
db.users.createIndex({ email: 1 })  // 1: 오름차순, -1: 내림차순

// 복합 인덱스
db.users.createIndex({ username: 1, email: 1 })

// 고유 인덱스
db.users.createIndex({ email: 1 }, { unique: true })

// TTL 인덱스 (자동 삭제)
db.sessions.createIndex(
  { createdAt: 1 },
  { expireAfterSeconds: 3600 }
)

// 텍스트 인덱스
db.articles.createIndex({ title: "text", content: "text" })
```

### 인덱스 관리
```javascript
// 인덱스 목록
db.users.getIndexes()

// 인덱스 삭제
db.users.dropIndex({ email: 1 })

// 모든 인덱스 삭제
db.users.dropIndexes()

// 인덱스 사용 통계
db.users.aggregate([{ $indexStats: {} }])
```

---

## MongoDB 집계 (Aggregation)

### 집계 파이프라인
```javascript
// 기본 집계
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: {
      _id: "$customer_id",
      total: { $sum: "$amount" },
      count: { $sum: 1 }
    }
  },
  { $sort: { total: -1 } },
  { $limit: 10 }
])

// Lookup (JOIN)
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "user_id",
      foreignField: "_id",
      as: "user_info"
    }
  }
])

// Unwind (배열 펼치기)
db.orders.aggregate([
  { $unwind: "$items" },
  { $group: {
      _id: "$items.product_id",
      total: { $sum: "$items.quantity" }
    }
  }
])
```

### 집계 연산자
```javascript
// $sum: 합계
{ $sum: "$amount" }

// $avg: 평균
{ $avg: "$amount" }

// $min/$max: 최소/최대
{ $min: "$amount" }
{ $max: "$amount" }

// $first/$last: 첫 번째/마지막
{ $first: "$amount" }
{ $last: "$amount" }

// $push/$addToSet: 배열에 추가
{ $push: "$items" }
{ $addToSet: "$tags" }
```

---

## MongoDB 관계 모델링

### 임베딩 (Embedding)
```javascript
// 관련 데이터를 같은 문서에 저장
{
  _id: ObjectId("..."),
  username: "johndoe",
  address: {
    street: "123 Main St",
    city: "Seoul",
    zipcode: "12345"
  },
  posts: [
    { title: "Post 1", content: "..." },
    { title: "Post 2", content: "..." }
  ]
}
```

### 참조 (Referencing)
```javascript
// 다른 컬렉션 참조
// users 컬렉션
{
  _id: ObjectId("..."),
  username: "johndoe"
}

// posts 컬렉션
{
  _id: ObjectId("..."),
  title: "Post 1",
  user_id: ObjectId("...")  // users 참조
}
```

---

## MongoDB 복제 (Replication)

### Replica Set 설정
```javascript
// Replica Set 초기화
rs.initiate({
  _id: "myReplicaSet",
  members: [
    { _id: 0, host: "localhost:27017" },
    { _id: 1, host: "localhost:27018" },
    { _id: 2, host: "localhost:27019" }
  ]
})

// Replica Set 상태
rs.status()

// Primary 확인
rs.isMaster()
```

---

## MongoDB 샤딩 (Sharding)

### 샤드 클러스터 구성
```javascript
// Config Server 설정
// Shard Server 설정
// Mongos Router 설정

// 샤드 추가
sh.addShard("shard1/localhost:27017")
sh.addShard("shard2/localhost:27018")

// 데이터베이스 활성화
sh.enableSharding("mydb")

// 컬렉션 샤딩
sh.shardCollection("mydb.users", { user_id: 1 })

// 샤드 상태
sh.status()
```

---

## MongoDB 보안

### 인증 설정
```javascript
// 관리자 사용자 생성
use admin
db.createUser({
  user: "admin",
  pwd: "password",
  roles: ["root"]
})

// 데이터베이스 사용자 생성
use mydb
db.createUser({
  user: "appuser",
  pwd: "password",
  roles: ["readWrite"]
})

// 인증
db.auth("admin", "password")
```

### 역할 관리
```javascript
// 읽기 전용 역할
db.createRole({
  role: "readOnly",
  privileges: [
    { resource: { db: "mydb", collection: "" }, actions: ["find"] }
  ],
  roles: []
})
```

---

## MongoDB 백업 및 복구

### mongodump
```bash
# 전체 데이터베이스 백업
mongodump --out /backup/mongodb

# 특정 데이터베이스 백업
mongodump --db mydb --out /backup/mongodb

# 특정 컬렉션 백업
mongodump --db mydb --collection users --out /backup/mongodb
```

### mongorestore
```bash
# 백업 복구
mongorestore /backup/mongodb

# 특정 데이터베이스 복구
mongorestore --db mydb /backup/mongodb/mydb
```

---

## MongoDB 모범 사례

### 1. 스키마 설계
* 접근 패턴에 맞춘 설계
* 임베딩 vs 참조 선택
* 인덱스 전략 수립

### 2. 인덱싱
* 자주 조회되는 필드에 인덱스
* 복합 인덱스 활용
* 인덱스 오버헤드 고려

### 3. 쿼리 최적화
* 프로젝션 사용
* 적절한 인덱스 활용
* explain()으로 실행 계획 확인

### 4. 성능
* 연결 풀링
* 읽기 우선순위 설정
* 적절한 Write Concern

### 5. 보안
* 인증 활성화
* 역할 기반 접근 제어
* 네트워크 보안

---

## MongoDB의 장점

* **유연한 스키마**: 스키마 변경 용이
* **개발자 친화적**: JSON 형태로 직관적
* **높은 성능**: 인덱싱 및 샤딩 지원
* **확장성**: 수평 확장 용이
* **풍부한 기능**: 집계, 텍스트 검색 등

---

## MongoDB의 단점

* **메모리 사용**: 인덱스와 데이터가 메모리에 로드
* **트랜잭션**: 제한적인 트랜잭션 지원 (4.0+ 지원)
* **JOIN**: 제한적인 JOIN 기능
* **데이터 중복**: 정규화 부족으로 인한 중복 가능

---

## 참고

* MongoDB는 문서 지향 데이터베이스로 유연한 스키마를 제공합니다
* 접근 패턴에 맞춘 스키마 설계가 중요합니다
* 적절한 인덱싱으로 성능을 최적화하세요
* 프로덕션 환경에서는 복제와 샤딩을 고려하세요
* 정기적인 백업과 모니터링이 필수입니다

