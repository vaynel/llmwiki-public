---
publish: true
tags: [지식, 데이터베이스]
---

# MySQL 개요

---

## MySQL이란?

> **MySQL**은 세계에서 가장 널리 사용되는 오픈소스 관계형 데이터베이스 관리 시스템(RDBMS)입니다. Oracle Corporation이 개발 및 지원하며, 웹 애플리케이션 개발에 가장 인기 있는 데이터베이스입니다.

---

## MySQL의 특징

### 1. 오픈소스
* GPL 라이선스
* 커뮤니티 에디션 무료
* 상업용 라이선스도 제공

### 2. 높은 성능
* 빠른 읽기 성능
* 효율적인 스토리지 엔진
* 최적화된 쿼리 처리

### 3. 안정성
* 검증된 안정성
* 대규모 프로덕션 환경에서 사용
* 강력한 트랜잭션 지원

### 4. 확장성
* 수직 확장 지원
* 복제(Replication) 기능
* 샤딩(Sharding) 지원

### 5. 호환성
* 다양한 운영체제 지원
* 다양한 프로그래밍 언어 지원
* 표준 SQL 준수

---

## MySQL 설치

### Linux (Ubuntu/Debian)
```bash
# 패키지 업데이트
sudo apt update

# MySQL 서버 설치
sudo apt install mysql-server

# MySQL 보안 설정
sudo mysql_secure_installation

# MySQL 시작
sudo systemctl start mysql
sudo systemctl enable mysql
```

### Linux (CentOS/RHEL)
```bash
# MySQL 저장소 추가
sudo yum install mysql-server

# MySQL 시작
sudo systemctl start mysqld
sudo systemctl enable mysqld
```

### macOS
```bash
# Homebrew를 통한 설치
brew install mysql

# MySQL 시작
brew services start mysql
```

### Windows
* MySQL 공식 웹사이트에서 설치 프로그램 다운로드
* 설치 마법사 따라 설치

---

## MySQL 기본 명령어

### 데이터베이스 관리
```sql
-- 데이터베이스 생성
CREATE DATABASE mydb;

-- 데이터베이스 선택
USE mydb;

-- 데이터베이스 목록 확인
SHOW DATABASES;

-- 데이터베이스 삭제
DROP DATABASE mydb;
```

### 테이블 관리
```sql
-- 테이블 생성
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 테이블 목록 확인
SHOW TABLES;

-- 테이블 구조 확인
DESCRIBE users;
SHOW CREATE TABLE users;

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
('bob', 'bob@example.com');

-- 데이터 조회
SELECT * FROM users;
SELECT username, email FROM users WHERE id = 1;

-- 데이터 수정
UPDATE users SET email = 'newemail@example.com' WHERE id = 1;

-- 데이터 삭제
DELETE FROM users WHERE id = 1;
```

### 인덱스 관리
```sql
-- 인덱스 생성
CREATE INDEX idx_email ON users(email);

-- 복합 인덱스
CREATE INDEX idx_username_email ON users(username, email);

-- 인덱스 확인
SHOW INDEX FROM users;

-- 인덱스 삭제
DROP INDEX idx_email ON users;
```

---

## MySQL 스토리지 엔진

### InnoDB (기본)
* 트랜잭션 지원
* 외래키 제약 조건
* 행 단위 잠금
* ACID 준수

```sql
-- InnoDB 테이블 생성
CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100)
) ENGINE=InnoDB;
```

### MyISAM
* 빠른 읽기 성능
* 트랜잭션 미지원
* 테이블 단위 잠금
* 풀텍스트 검색 지원

```sql
-- MyISAM 테이블 생성
CREATE TABLE logs (
    id INT PRIMARY KEY,
    message TEXT
) ENGINE=MyISAM;
```

### 기타 엔진
* **MEMORY**: 메모리 기반 임시 테이블
* **CSV**: CSV 파일 형태로 저장
* **Archive**: 압축 저장

---

## MySQL 사용자 및 권한 관리

### 사용자 생성
```sql
-- 사용자 생성
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';

-- 원격 접속 허용
CREATE USER 'newuser'@'%' IDENTIFIED BY 'password';
```

### 권한 부여
```sql
-- 모든 권한 부여
GRANT ALL PRIVILEGES ON mydb.* TO 'newuser'@'localhost';

-- 특정 권한만 부여
GRANT SELECT, INSERT, UPDATE ON mydb.* TO 'newuser'@'localhost';

-- 권한 적용
FLUSH PRIVILEGES;
```

### 권한 확인
```sql
-- 사용자 권한 확인
SHOW GRANTS FOR 'newuser'@'localhost';

-- 현재 사용자 권한
SHOW GRANTS;
```

### 권한 회수
```sql
-- 권한 회수
REVOKE ALL PRIVILEGES ON mydb.* FROM 'newuser'@'localhost';

-- 권한 적용
FLUSH PRIVILEGES;
```

### 사용자 삭제
```sql
-- 사용자 삭제
DROP USER 'newuser'@'localhost';
```

---

## MySQL 복제 (Replication)

### 마스터-슬레이브 복제
* 마스터: 쓰기 전용
* 슬레이브: 읽기 전용
* 데이터 백업 및 부하 분산

### 설정 예시
```sql
-- 마스터 서버 설정
-- my.cnf
[mysqld]
server-id = 1
log-bin = mysql-bin
binlog-format = ROW

-- 슬레이브 서버 설정
-- my.cnf
[mysqld]
server-id = 2
relay-log = mysql-relay-bin
read-only = 1
```

---

## MySQL 백업 및 복구

### mysqldump를 사용한 백업
```bash
# 전체 데이터베이스 백업
mysqldump -u root -p --all-databases > backup.sql

# 특정 데이터베이스 백업
mysqldump -u root -p mydb > mydb_backup.sql

# 특정 테이블만 백업
mysqldump -u root -p mydb users > users_backup.sql

# 구조만 백업 (데이터 제외)
mysqldump -u root -p --no-data mydb > mydb_structure.sql
```

### 복구
```bash
# 백업 파일로 복구
mysql -u root -p mydb < mydb_backup.sql

# 전체 데이터베이스 복구
mysql -u root -p < backup.sql
```

### 자동 백업 스크립트
```bash
#!/bin/bash
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/backup/mysql"
DB_NAME="mydb"

mysqldump -u root -p$MYSQL_PASSWORD $DB_NAME > $BACKUP_DIR/${DB_NAME}_${DATE}.sql

# 7일 이상 된 백업 삭제
find $BACKUP_DIR -name "*.sql" -mtime +7 -delete
```

---

## MySQL 성능 최적화

### 쿼리 최적화
```sql
-- EXPLAIN으로 실행 계획 확인
EXPLAIN SELECT * FROM users WHERE email = 'test@example.com';

-- 인덱스 활용
CREATE INDEX idx_email ON users(email);

-- 느린 쿼리 로그 활성화
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 2;
```

### 설정 최적화
```ini
# my.cnf 최적화 예시
[mysqld]
# 버퍼 풀 크기 (메모리의 70-80%)
innodb_buffer_pool_size = 4G

# 연결 수
max_connections = 200

# 쿼리 캐시
query_cache_size = 256M
query_cache_type = 1
```

### 모니터링
```sql
-- 현재 연결 확인
SHOW PROCESSLIST;

-- 테이블 상태 확인
SHOW TABLE STATUS;

-- 인덱스 사용 통계
SHOW INDEX FROM users;

-- 변수 확인
SHOW VARIABLES LIKE 'innodb_buffer_pool_size';
```

---

## MySQL 주요 함수

### 문자열 함수
```sql
-- 문자열 연결
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM users;

-- 대소문자 변환
SELECT UPPER(username), LOWER(email) FROM users;

-- 문자열 자르기
SELECT SUBSTRING(email, 1, 5) FROM users;

-- 문자열 길이
SELECT LENGTH(username) FROM users;
```

### 날짜 함수
```sql
-- 현재 날짜/시간
SELECT NOW(), CURDATE(), CURTIME();

-- 날짜 계산
SELECT DATE_ADD(created_at, INTERVAL 1 DAY) FROM users;
SELECT DATEDIFF(NOW(), created_at) AS days_ago FROM users;

-- 날짜 포맷
SELECT DATE_FORMAT(created_at, '%Y-%m-%d') FROM users;
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
```

---

## MySQL 트랜잭션

### 트랜잭션 사용
```sql
-- 트랜잭션 시작
START TRANSACTION;

-- 작업 수행
INSERT INTO orders (user_id, total) VALUES (1, 100);
UPDATE users SET balance = balance - 100 WHERE id = 1;

-- 커밋 또는 롤백
COMMIT;
-- 또는
ROLLBACK;
```

### 자동 커밋 제어
```sql
-- 자동 커밋 비활성화
SET autocommit = 0;

-- 작업 수행
INSERT INTO orders (user_id, total) VALUES (1, 100);

-- 수동 커밋
COMMIT;

-- 자동 커밋 재활성화
SET autocommit = 1;
```

---

## MySQL 제약 조건

### 기본키 (Primary Key)
```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50)
);
```

### 외래키 (Foreign Key)
```sql
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### 유니크 (Unique)
```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);
```

### 체크 (Check)
```sql
CREATE TABLE products (
    id INT PRIMARY KEY,
    price DECIMAL(10,2) CHECK (price > 0)
);
```

### NOT NULL
```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50) NOT NULL
);
```

---

## MySQL 모범 사례

### 1. 인덱싱
* 자주 조회되는 컬럼에 인덱스 생성
* 외래키에 인덱스 생성
* 복합 인덱스 활용
* 불필요한 인덱스 제거

### 2. 쿼리 최적화
* SELECT * 사용 지양
* 적절한 WHERE 조건 사용
* JOIN 최적화
* 서브쿼리 최적화

### 3. 트랜잭션 관리
* 적절한 트랜잭션 범위
* 데드락 방지
* 롤백 전략 수립

### 4. 백업
* 정기적인 백업
* 백업 검증
* 복구 테스트

### 5. 보안
* 강력한 비밀번호
* 최소 권한 원칙
* SQL 인젝션 방지
* 정기적인 업데이트

---

## MySQL 버전

### 주요 버전
* **MySQL 5.7**: 안정적인 버전 (EOL)
* **MySQL 8.0**: 현재 LTS 버전
* **MySQL 8.1+**: 최신 버전

### 버전 확인
```sql
SELECT VERSION();
```

---

## MySQL의 장점

* **오픈소스**: 무료 사용 가능
* **높은 성능**: 빠른 읽기 성능
* **안정성**: 검증된 안정성
* **커뮤니티**: 활발한 커뮤니티
* **호환성**: 다양한 플랫폼 지원

---

## MySQL의 단점

* **복잡한 JOIN**: 복잡한 JOIN 성능 저하
* **수평 확장**: 수평 확장 어려움
* **트랜잭션**: MyISAM 엔진은 트랜잭션 미지원

---

## 참고

* MySQL은 웹 애플리케이션에 가장 널리 사용되는 데이터베이스입니다
* InnoDB 엔진을 기본으로 사용하세요
* 정기적인 백업과 모니터링이 필수입니다
* 쿼리 최적화를 통해 성능을 향상시키세요
* 보안을 위해 최신 버전을 유지하세요

