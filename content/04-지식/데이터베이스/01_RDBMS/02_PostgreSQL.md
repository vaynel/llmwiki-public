---
publish: true
tags: [지식, 데이터베이스]
---

# PostgreSQL 개요

---

## PostgreSQL이란?

> **PostgreSQL**은 세계에서 가장 고급 기능을 갖춘 오픈소스 관계형 데이터베이스 관리 시스템입니다. SQL 표준을 엄격하게 준수하며, 복잡한 쿼리와 대용량 데이터 처리에 뛰어난 성능을 제공합니다.

---

## PostgreSQL의 특징

### 1. 오픈소스
* PostgreSQL 라이선스 (BSD 스타일)
* 완전 무료
* 상업적 사용 가능

### 2. SQL 표준 준수
* SQL 표준을 가장 잘 준수하는 데이터베이스
* 다양한 SQL 기능 지원
* 복잡한 쿼리 처리

### 3. 고급 기능
* JSON/JSONB 지원
* 배열 및 복합 타입
* 사용자 정의 함수 및 연산자
* 풀텍스트 검색

### 4. 확장성
* 다양한 확장(Extension) 지원
* 사용자 정의 데이터 타입
* 외래 데이터 래퍼(FDW)

### 5. 안정성 및 성능
* ACID 완전 지원
* MVCC (Multi-Version Concurrency Control)
* 고급 인덱싱 (GIN, GiST, BRIN)

---

## PostgreSQL 설치

### Linux (Ubuntu/Debian)
```bash
# 저장소 추가
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

# 패키지 업데이트
sudo apt update

# PostgreSQL 설치
sudo apt install postgresql postgresql-contrib

# PostgreSQL 시작
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

### Linux (CentOS/RHEL)
```bash
# PostgreSQL 저장소 추가
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

# PostgreSQL 설치
sudo yum install -y postgresql14-server

# 데이터베이스 초기화
sudo /usr/pgsql-14/bin/postgresql-14-setup initdb

# PostgreSQL 시작
sudo systemctl start postgresql-14
sudo systemctl enable postgresql-14
```

### macOS
```bash
# Homebrew를 통한 설치
brew install postgresql@14

# PostgreSQL 시작
brew services start postgresql@14
```

### Windows
* PostgreSQL 공식 웹사이트에서 설치 프로그램 다운로드
* 설치 마법사 따라 설치

---

## PostgreSQL 기본 명령어

### 데이터베이스 관리
```sql
-- 데이터베이스 생성
CREATE DATABASE mydb;

-- 데이터베이스 선택
\c mydb

-- 데이터베이스 목록 확인
\l

-- 데이터베이스 삭제
DROP DATABASE mydb;
```

### 테이블 관리
```sql
-- 테이블 생성
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 테이블 목록 확인
\dt

-- 테이블 구조 확인
\d users
\d+ users

-- 테이블 수정
ALTER TABLE users ADD COLUMN age INT;

-- 테이블 삭제
DROP TABLE users;
```

### 데이터 조작 (DML)
```sql
-- 데이터 삽입
INSERT INTO users (username, email) 
VALUES ('john', 'john@example.com');

-- 여러 행 삽입
INSERT INTO users (username, email) VALUES
('jane', 'jane@example.com'),
('bob', 'bob@example.com')
RETURNING *;

-- 데이터 조회
SELECT * FROM users;
SELECT username, email FROM users WHERE id = 1;

-- 데이터 수정
UPDATE users SET email = 'newemail@example.com' WHERE id = 1
RETURNING *;

-- 데이터 삭제
DELETE FROM users WHERE id = 1
RETURNING *;
```

### 인덱스 관리
```sql
-- 인덱스 생성
CREATE INDEX idx_email ON users(email);

-- 복합 인덱스
CREATE INDEX idx_username_email ON users(username, email);

-- 고유 인덱스
CREATE UNIQUE INDEX idx_unique_email ON users(email);

-- 부분 인덱스
CREATE INDEX idx_active_users ON users(email) WHERE active = true;

-- 인덱스 확인
\di

-- 인덱스 삭제
DROP INDEX idx_email;
```

---

## PostgreSQL 고급 데이터 타입

### JSON/JSONB
```sql
-- JSON 컬럼 생성
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    attributes JSONB
);

-- JSON 데이터 삽입
INSERT INTO products (name, attributes) VALUES
('Laptop', '{"brand": "Dell", "ram": "16GB", "storage": "512GB"}'::jsonb);

-- JSON 쿼리
SELECT * FROM products WHERE attributes->>'brand' = 'Dell';
SELECT attributes->'ram' FROM products;

-- JSONB 인덱스
CREATE INDEX idx_attributes ON products USING GIN (attributes);
```

### 배열
```sql
-- 배열 컬럼 생성
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    title VARCHAR(100),
    tags TEXT[]
);

-- 배열 데이터 삽입
INSERT INTO posts (title, tags) VALUES
('PostgreSQL Guide', ARRAY['database', 'sql', 'postgresql']);

-- 배열 쿼리
SELECT * FROM posts WHERE 'database' = ANY(tags);
SELECT * FROM posts WHERE tags @> ARRAY['sql'];
```

### 복합 타입
```sql
-- 복합 타입 생성
CREATE TYPE address AS (
    street VARCHAR(100),
    city VARCHAR(50),
    zipcode VARCHAR(10)
);

-- 복합 타입 사용
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    address address
);

-- 데이터 삽입
INSERT INTO customers (name, address) VALUES
('John Doe', ROW('123 Main St', 'Seoul', '12345'));

-- 쿼리
SELECT name, (address).city FROM customers;
```

---

## PostgreSQL 확장 (Extensions)

### 인기 있는 확장
```sql
-- 확장 목록 확인
\dx

-- 확장 설치
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "pg_trgm";  -- 트라이그램 검색
CREATE EXTENSION IF NOT EXISTS "postgis";  -- 지리 공간 데이터

-- UUID 사용
CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id INT,
    total DECIMAL(10,2)
);
```

### 주요 확장 목록
* **uuid-ossp**: UUID 생성
* **pg_trgm**: 트라이그램 기반 텍스트 검색
* **PostGIS**: 지리 공간 데이터 처리
* **pg_stat_statements**: 쿼리 성능 통계
* **hstore**: 키-값 저장

---

## PostgreSQL 인덱스 타입

### B-tree (기본)
```sql
-- 기본 인덱스
CREATE INDEX idx_email ON users(email);
```

### GIN (Generalized Inverted Index)
```sql
-- JSONB, 배열, 풀텍스트 검색에 사용
CREATE INDEX idx_attributes_gin ON products USING GIN (attributes);
```

### GiST (Generalized Search Tree)
```sql
-- 지리 공간 데이터, 범위 검색에 사용
CREATE INDEX idx_location_gist ON places USING GIST (location);
```

### BRIN (Block Range Index)
```sql
-- 대용량 테이블의 순서가 있는 데이터에 사용
CREATE INDEX idx_created_at_brin ON logs USING BRIN (created_at);
```

### Hash
```sql
-- 등호 검색에 최적화
CREATE INDEX idx_email_hash ON users USING HASH (email);
```

---

## PostgreSQL 사용자 및 권한 관리

### 사용자 생성
```sql
-- 사용자 생성
CREATE USER newuser WITH PASSWORD 'password';

-- 역할 생성 (사용자와 동일)
CREATE ROLE newrole WITH LOGIN PASSWORD 'password';
```

### 권한 부여
```sql
-- 모든 권한 부여
GRANT ALL PRIVILEGES ON DATABASE mydb TO newuser;

-- 테이블 권한 부여
GRANT SELECT, INSERT, UPDATE ON TABLE users TO newuser;

-- 스키마 권한 부여
GRANT USAGE ON SCHEMA public TO newuser;
GRANT ALL ON ALL TABLES IN SCHEMA public TO newuser;

-- 권한 확인
\du
```

### 권한 회수
```sql
-- 권한 회수
REVOKE ALL PRIVILEGES ON TABLE users FROM newuser;
```

### 사용자 삭제
```sql
-- 사용자 삭제
DROP USER newuser;
```

---

## PostgreSQL 백업 및 복구

### pg_dump를 사용한 백업
```bash
# 전체 데이터베이스 백업
pg_dump -U postgres mydb > backup.sql

# 특정 테이블만 백업
pg_dump -U postgres -t users mydb > users_backup.sql

# 구조만 백업 (데이터 제외)
pg_dump -U postgres --schema-only mydb > schema.sql

# 커스텀 형식 백업 (압축)
pg_dump -U postgres -Fc mydb > backup.dump
```

### pg_restore를 사용한 복구
```bash
# SQL 파일로 복구
psql -U postgres mydb < backup.sql

# 커스텀 형식 복구
pg_restore -U postgres -d mydb backup.dump
```

### 전체 클러스터 백업
```bash
# pg_dumpall 사용
pg_dumpall -U postgres > all_databases.sql

# 복구
psql -U postgres < all_databases.sql
```

---

## PostgreSQL 성능 최적화

### 쿼리 최적화
```sql
-- EXPLAIN으로 실행 계획 확인
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';

-- 인덱스 활용
CREATE INDEX idx_email ON users(email);

-- 통계 정보 업데이트
ANALYZE users;

-- 느린 쿼리 로그 설정
-- postgresql.conf
log_min_duration_statement = 1000  # 1초 이상 쿼리 로그
```

### 설정 최적화
```ini
# postgresql.conf 최적화 예시
shared_buffers = 4GB              # 메모리의 25%
effective_cache_size = 12GB       # 메모리의 75%
maintenance_work_mem = 1GB
checkpoint_completion_target = 0.9
wal_buffers = 16MB
default_statistics_target = 100
random_page_cost = 1.1            # SSD인 경우
effective_io_concurrency = 200     # SSD인 경우
```

### 모니터링
```sql
-- 현재 연결 확인
SELECT * FROM pg_stat_activity;

-- 테이블 통계
SELECT * FROM pg_stat_user_tables;

-- 인덱스 사용 통계
SELECT * FROM pg_stat_user_indexes;

-- 느린 쿼리 확인
SELECT * FROM pg_stat_statements 
ORDER BY total_time DESC 
LIMIT 10;
```

---

## PostgreSQL 주요 함수

### 문자열 함수
```sql
-- 문자열 연결
SELECT first_name || ' ' || last_name AS full_name FROM users;

-- 대소문자 변환
SELECT UPPER(username), LOWER(email) FROM users;

-- 문자열 자르기
SELECT SUBSTRING(email, 1, 5) FROM users;

-- 문자열 길이
SELECT LENGTH(username) FROM users;

-- 정규식
SELECT * FROM users WHERE email ~ '^[a-z]+@';
```

### 날짜 함수
```sql
-- 현재 날짜/시간
SELECT NOW(), CURRENT_DATE, CURRENT_TIME;

-- 날짜 계산
SELECT created_at + INTERVAL '1 day' FROM users;
SELECT NOW() - created_at AS age FROM users;

-- 날짜 추출
SELECT EXTRACT(YEAR FROM created_at) FROM users;
SELECT DATE_PART('month', created_at) FROM users;
```

### 집계 함수
```sql
-- 집계 함수
SELECT COUNT(*) FROM users;
SELECT AVG(age) FROM users;
SELECT SUM(price) FROM orders;
SELECT MAX(created_at), MIN(created_at) FROM users;

-- 그룹화
SELECT department, COUNT(*) 
FROM employees 
GROUP BY department;

-- 윈도우 함수
SELECT 
    name,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) as rank
FROM employees;
```

---

## PostgreSQL 트랜잭션

### 트랜잭션 사용
```sql
-- 트랜잭션 시작
BEGIN;

-- 작업 수행
INSERT INTO orders (user_id, total) VALUES (1, 100);
UPDATE users SET balance = balance - 100 WHERE id = 1;

-- 커밋 또는 롤백
COMMIT;
-- 또는
ROLLBACK;
```

### 저장점 (Savepoint)
```sql
BEGIN;
INSERT INTO orders (user_id, total) VALUES (1, 100);
SAVEPOINT sp1;
UPDATE users SET balance = balance - 100 WHERE id = 1;
-- 문제 발생 시
ROLLBACK TO SAVEPOINT sp1;
COMMIT;
```

---

## PostgreSQL 모범 사례

### 1. 인덱싱
* 자주 조회되는 컬럼에 인덱스 생성
* 적절한 인덱스 타입 선택 (B-tree, GIN, GiST 등)
* 부분 인덱스 활용
* 불필요한 인덱스 제거

### 2. 쿼리 최적화
* EXPLAIN ANALYZE 활용
* 적절한 JOIN 사용
* 서브쿼리 최적화
* 윈도우 함수 활용

### 3. JSON/JSONB 활용
* JSONB 사용 권장 (인덱싱 가능)
* GIN 인덱스 활용
* JSON 쿼리 최적화

### 4. 백업
* 정기적인 백업
* pg_dumpall로 전체 백업
* WAL 아카이빙 설정
* 복구 테스트

### 5. 보안
* 강력한 비밀번호
* 최소 권한 원칙
* SSL 연결 사용
* 정기적인 업데이트

---

## PostgreSQL vs MySQL

| 구분 | PostgreSQL | MySQL |
|:---|:---|:---|
| **SQL 표준 준수** | 높음 | 중간 |
| **복잡한 쿼리** | 우수 | 보통 |
| **JSON 지원** | JSONB (인덱싱 가능) | JSON (제한적) |
| **트랜잭션** | 완전 지원 | InnoDB만 지원 |
| **확장성** | 다양한 확장 | 제한적 |
| **성능** | 복잡한 쿼리 우수 | 단순 쿼리 빠름 |
| **사용 사례** | 복잡한 데이터, 분석 | 웹 애플리케이션 |

---

## PostgreSQL의 장점

* **SQL 표준 준수**: 가장 표준에 가까운 구현
* **고급 기능**: JSON, 배열, 복합 타입 등
* **확장성**: 다양한 확장 지원
* **안정성**: ACID 완전 지원
* **성능**: 복잡한 쿼리 처리 우수

---

## PostgreSQL의 단점

* **메모리 사용**: MySQL보다 메모리 사용량 많음
* **단순 쿼리**: 단순 쿼리는 MySQL보다 느릴 수 있음
* **학습 곡선**: 고급 기능 학습 필요

---

## 참고

* PostgreSQL은 복잡한 데이터와 쿼리에 최적화된 데이터베이스입니다
* JSONB를 활용하여 NoSQL과 RDBMS의 장점을 결합하세요
* 적절한 인덱스 타입을 선택하여 성능을 최적화하세요
* 정기적인 VACUUM과 ANALYZE를 실행하세요
* 확장 기능을 활용하여 기능을 확장하세요

