---
publish: true
tags: [지식, 데이터베이스]
---

# SQL 기본

---

## SQL이란?

> **SQL (Structured Query Language)**은 관계형 데이터베이스 관리 시스템(RDBMS)에서 데이터를 정의, 조작, 제어하기 위해 사용하는 표준화된 언어입니다. 데이터베이스와 상호작용하는 가장 기본적이고 중요한 도구입니다.

---

## SQL의 분류

### 1. DDL (Data Definition Language) - 데이터 정의어
데이터베이스 구조를 정의하는 명령어

* CREATE: 데이터베이스, 테이블, 인덱스 등 생성
* ALTER: 기존 구조 수정
* DROP: 데이터베이스, 테이블 등 삭제
* TRUNCATE: 테이블 데이터 전체 삭제

### 2. DML (Data Manipulation Language) - 데이터 조작어
데이터를 조작하는 명령어

* SELECT: 데이터 조회
* INSERT: 데이터 삽입
* UPDATE: 데이터 수정
* DELETE: 데이터 삭제

### 3. DCL (Data Control Language) - 데이터 제어어
권한을 제어하는 명령어

* GRANT: 권한 부여
* REVOKE: 권한 회수

### 4. TCL (Transaction Control Language) - 트랜잭션 제어어
트랜잭션을 제어하는 명령어

* COMMIT: 트랜잭션 확정
* ROLLBACK: 트랜잭션 취소
* SAVEPOINT: 저장점 설정

---

## DDL - 데이터 정의어

### 데이터베이스 생성 및 삭제
```sql
-- 데이터베이스 생성
CREATE DATABASE mydb;

-- 데이터베이스 사용
USE mydb;  -- MySQL
\c mydb;   -- PostgreSQL

-- 데이터베이스 삭제
DROP DATABASE mydb;
```

### 테이블 생성
```sql
-- 기본 테이블 생성
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- AUTO_INCREMENT (MySQL)
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL
);

-- SERIAL (PostgreSQL)
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL
);
```

### 테이블 수정
```sql
-- 컬럼 추가
ALTER TABLE users ADD COLUMN phone VARCHAR(20);

-- 컬럼 수정
ALTER TABLE users MODIFY COLUMN email VARCHAR(200);  -- MySQL
ALTER TABLE users ALTER COLUMN email TYPE VARCHAR(200);  -- PostgreSQL

-- 컬럼 삭제
ALTER TABLE users DROP COLUMN phone;

-- 컬럼 이름 변경
ALTER TABLE users RENAME COLUMN email TO email_address;
```

### 테이블 삭제
```sql
-- 테이블 삭제
DROP TABLE users;

-- 테이블 데이터만 삭제 (구조 유지)
TRUNCATE TABLE users;
```

### 인덱스 생성
```sql
-- 인덱스 생성
CREATE INDEX idx_email ON users(email);

-- 유니크 인덱스
CREATE UNIQUE INDEX idx_unique_email ON users(email);

-- 복합 인덱스
CREATE INDEX idx_name_email ON users(username, email);

-- 인덱스 삭제
DROP INDEX idx_email ON users;  -- MySQL
DROP INDEX idx_email;  -- PostgreSQL
```

---

## DML - 데이터 조작어

### SELECT - 데이터 조회
```sql
-- 모든 컬럼 조회
SELECT * FROM users;

-- 특정 컬럼 조회
SELECT id, username, email FROM users;

-- 조건부 조회
SELECT * FROM users WHERE age > 18;

-- 여러 조건
SELECT * FROM users 
WHERE age > 18 AND email LIKE '%@gmail.com';

-- 정렬
SELECT * FROM users ORDER BY created_at DESC;

-- 제한
SELECT * FROM users LIMIT 10;  -- MySQL
SELECT * FROM users LIMIT 10 OFFSET 20;  -- MySQL
SELECT * FROM users FETCH FIRST 10 ROWS ONLY;  -- PostgreSQL

-- 그룹화
SELECT department, COUNT(*) 
FROM employees 
GROUP BY department;

-- 집계 함수
SELECT 
    department,
    COUNT(*) as count,
    AVG(salary) as avg_salary,
    MAX(salary) as max_salary,
    MIN(salary) as min_salary
FROM employees
GROUP BY department;
```

### INSERT - 데이터 삽입
```sql
-- 단일 행 삽입
INSERT INTO users (username, email, age) 
VALUES ('john', 'john@example.com', 25);

-- 여러 행 삽입
INSERT INTO users (username, email, age) VALUES
('jane', 'jane@example.com', 30),
('bob', 'bob@example.com', 28);

-- 다른 테이블에서 데이터 삽입
INSERT INTO users (username, email)
SELECT name, email FROM temp_users;

-- 삽입된 데이터 반환 (PostgreSQL)
INSERT INTO users (username, email) 
VALUES ('john', 'john@example.com')
RETURNING *;
```

### UPDATE - 데이터 수정
```sql
-- 단일 행 수정
UPDATE users 
SET email = 'newemail@example.com' 
WHERE id = 1;

-- 여러 컬럼 수정
UPDATE users 
SET email = 'newemail@example.com', age = 26 
WHERE id = 1;

-- 조건부 수정
UPDATE users 
SET age = age + 1 
WHERE created_at < '2023-01-01';

-- 수정된 데이터 반환 (PostgreSQL)
UPDATE users 
SET email = 'newemail@example.com' 
WHERE id = 1
RETURNING *;
```

### DELETE - 데이터 삭제
```sql
-- 조건부 삭제
DELETE FROM users WHERE id = 1;

-- 여러 조건
DELETE FROM users 
WHERE age < 18 AND created_at < '2023-01-01';

-- 모든 데이터 삭제 (주의!)
DELETE FROM users;

-- 삭제된 데이터 반환 (PostgreSQL)
DELETE FROM users WHERE id = 1
RETURNING *;
```

---

## JOIN - 테이블 조인

### INNER JOIN
```sql
-- 내부 조인
SELECT u.username, o.order_id, o.total
FROM users u
INNER JOIN orders o ON u.id = o.user_id;

-- 동일한 결과 (WHERE 사용)
SELECT u.username, o.order_id, o.total
FROM users u, orders o
WHERE u.id = o.user_id;
```

### LEFT JOIN (LEFT OUTER JOIN)
```sql
-- 왼쪽 외부 조인
SELECT u.username, o.order_id, o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

### RIGHT JOIN (RIGHT OUTER JOIN)
```sql
-- 오른쪽 외부 조인
SELECT u.username, o.order_id, o.total
FROM users u
RIGHT JOIN orders o ON u.id = o.user_id;
```

### FULL OUTER JOIN
```sql
-- 완전 외부 조인 (PostgreSQL)
SELECT u.username, o.order_id, o.total
FROM users u
FULL OUTER JOIN orders o ON u.id = o.user_id;
```

### CROSS JOIN
```sql
-- 교차 조인 (카테시안 곱)
SELECT u.username, p.product_name
FROM users u
CROSS JOIN products p;
```

### SELF JOIN
```sql
-- 자기 조인
SELECT e1.name as employee, e2.name as manager
FROM employees e1
LEFT JOIN employees e2 ON e1.manager_id = e2.id;
```

---

## 서브쿼리 (Subquery)

### 스칼라 서브쿼리
```sql
-- 단일 값 반환
SELECT 
    username,
    (SELECT COUNT(*) FROM orders WHERE user_id = users.id) as order_count
FROM users;
```

### 인라인 뷰
```sql
-- FROM 절의 서브쿼리
SELECT * FROM (
    SELECT department, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department
) AS dept_avg
WHERE avg_salary > 50000;
```

### 상관 서브쿼리
```sql
-- 외부 쿼리와 연관된 서브쿼리
SELECT * FROM users u
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.user_id = u.id
);
```

### IN 서브쿼리
```sql
-- IN 연산자 사용
SELECT * FROM users
WHERE id IN (
    SELECT user_id FROM orders 
    WHERE total > 1000
);
```

---

## 집계 함수 및 그룹화

### 집계 함수
```sql
-- COUNT: 행 개수
SELECT COUNT(*) FROM users;
SELECT COUNT(DISTINCT email) FROM users;

-- SUM: 합계
SELECT SUM(total) FROM orders;

-- AVG: 평균
SELECT AVG(age) FROM users;

-- MAX/MIN: 최대/최소
SELECT MAX(created_at), MIN(created_at) FROM users;

-- GROUP_CONCAT (MySQL): 그룹 연결
SELECT department, GROUP_CONCAT(name) 
FROM employees 
GROUP BY department;
```

### GROUP BY
```sql
-- 기본 그룹화
SELECT department, COUNT(*) 
FROM employees 
GROUP BY department;

-- 여러 컬럼 그룹화
SELECT department, position, COUNT(*) 
FROM employees 
GROUP BY department, position;

-- HAVING: 그룹 조건
SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

### 윈도우 함수 (PostgreSQL, MySQL 8.0+)
```sql
-- RANK: 순위
SELECT 
    name,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) as rank
FROM employees;

-- ROW_NUMBER: 행 번호
SELECT 
    name,
    ROW_NUMBER() OVER (ORDER BY created_at) as row_num
FROM users;

-- LAG/LEAD: 이전/다음 행 값
SELECT 
    date,
    sales,
    LAG(sales) OVER (ORDER BY date) as prev_sales,
    LEAD(sales) OVER (ORDER BY date) as next_sales
FROM daily_sales;
```

---

## 제약 조건 (Constraints)

### PRIMARY KEY
```sql
-- 기본키
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50)
);

-- 복합 기본키
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```

### FOREIGN KEY
```sql
-- 외래키
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- 외래키 옵션
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
    ON DELETE CASCADE      -- 부모 삭제 시 자식도 삭제
    ON UPDATE CASCADE      -- 부모 업데이트 시 자식도 업데이트
);
```

### UNIQUE
```sql
-- 유니크 제약
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);
```

### CHECK
```sql
-- 체크 제약
CREATE TABLE products (
    id INT PRIMARY KEY,
    price DECIMAL(10,2) CHECK (price > 0),
    age INT CHECK (age >= 0 AND age <= 120)
);
```

### NOT NULL
```sql
-- NOT NULL 제약
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

### DEFAULT
```sql
-- 기본값
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status VARCHAR(20) DEFAULT 'active'
);
```

---

## 뷰 (View)

### 뷰 생성
```sql
-- 기본 뷰
CREATE VIEW user_orders AS
SELECT 
    u.username,
    o.order_id,
    o.total,
    o.order_date
FROM users u
JOIN orders o ON u.id = o.user_id;

-- 뷰 조회
SELECT * FROM user_orders;

-- 뷰 삭제
DROP VIEW user_orders;
```

### 뷰 수정
```sql
-- 뷰 수정 (PostgreSQL)
CREATE OR REPLACE VIEW user_orders AS
SELECT 
    u.username,
    o.order_id,
    o.total
FROM users u
JOIN orders o ON u.id = o.user_id;
```

---

## 트랜잭션

### 트랜잭션 기본
```sql
-- 트랜잭션 시작
BEGIN;  -- PostgreSQL
START TRANSACTION;  -- MySQL

-- 작업 수행
INSERT INTO orders (user_id, total) VALUES (1, 100);
UPDATE users SET balance = balance - 100 WHERE id = 1;

-- 커밋
COMMIT;

-- 롤백
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

## SQL 함수

### 문자열 함수
```sql
-- CONCAT: 문자열 연결
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM users;  -- MySQL
SELECT first_name || ' ' || last_name AS full_name FROM users;  -- PostgreSQL

-- UPPER/LOWER: 대소문자 변환
SELECT UPPER(username), LOWER(email) FROM users;

-- SUBSTRING: 문자열 자르기
SELECT SUBSTRING(email, 1, 5) FROM users;

-- LENGTH: 문자열 길이
SELECT LENGTH(username) FROM users;

-- TRIM: 공백 제거
SELECT TRIM('  hello  ') FROM dual;
```

### 숫자 함수
```sql
-- ROUND: 반올림
SELECT ROUND(price, 2) FROM products;

-- FLOOR/CEIL: 내림/올림
SELECT FLOOR(price), CEIL(price) FROM products;

-- ABS: 절댓값
SELECT ABS(-10) FROM dual;

-- MOD: 나머지
SELECT MOD(10, 3) FROM dual;
```

### 날짜 함수
```sql
-- 현재 날짜/시간
SELECT NOW(), CURRENT_DATE, CURRENT_TIME;

-- 날짜 계산
SELECT DATE_ADD(created_at, INTERVAL 1 DAY) FROM users;  -- MySQL
SELECT created_at + INTERVAL '1 day' FROM users;  -- PostgreSQL

-- 날짜 추출
SELECT YEAR(created_at), MONTH(created_at) FROM users;  -- MySQL
SELECT EXTRACT(YEAR FROM created_at) FROM users;  -- PostgreSQL

-- 날짜 포맷
SELECT DATE_FORMAT(created_at, '%Y-%m-%d') FROM users;  -- MySQL
SELECT TO_CHAR(created_at, 'YYYY-MM-DD') FROM users;  -- PostgreSQL
```

---

## SQL 모범 사례

### 1. 쿼리 최적화
* SELECT * 사용 지양
* 적절한 WHERE 조건 사용
* 인덱스 활용
* 불필요한 JOIN 제거

### 2. 보안
* SQL 인젝션 방지 (Prepared Statement 사용)
* 최소 권한 원칙
* 입력 데이터 검증

### 3. 가독성
* 명확한 별칭 사용
* 적절한 들여쓰기
* 주석 활용
* 일관된 네이밍

### 4. 성능
* 인덱스 활용
* EXPLAIN으로 실행 계획 확인
* 서브쿼리 최적화
* 배치 작업 활용

---

## 참고

* SQL은 데이터베이스와 상호작용하는 기본 언어입니다
* 표준 SQL을 학습하되, 각 DBMS의 특성을 이해하세요
* 쿼리 최적화를 통해 성능을 향상시키세요
* 보안을 위해 Prepared Statement를 사용하세요
* 정기적으로 쿼리 성능을 모니터링하세요

