---
publish: true
tags: [지식, 데이터베이스]
---

# RDBMS (관계형 데이터베이스 관리 시스템) 개요

---

## RDBMS란?

> **RDBMS (Relational Database Management System)**는 관계형 데이터베이스를 관리하는 소프트웨어 시스템입니다. 데이터를 테이블(Table) 형태로 저장하고, 테이블 간의 관계(Relationship)를 통해 데이터를 관리합니다. SQL(Structured Query Language)을 사용하여 데이터를 조작합니다.

---

## RDBMS의 특징

### 1. 테이블 기반 구조
* 데이터를 행(Row)과 열(Column)로 구성된 테이블에 저장
* 각 테이블은 엔티티(Entity)를 나타냄
* 명확한 스키마(Schema) 정의

### 2. 관계 (Relationship)
* 테이블 간의 관계를 외래키(Foreign Key)로 정의
* 일대일(1:1), 일대다(1:N), 다대다(N:M) 관계 지원
* 정규화를 통한 데이터 중복 제거

### 3. ACID 특성
* **Atomicity (원자성)**: 트랜잭션은 모두 실행되거나 모두 롤백
* **Consistency (일관성)**: 데이터의 무결성 유지
* **Isolation (격리성)**: 동시 실행 트랜잭션 간 격리
* **Durability (지속성)**: 커밋된 데이터는 영구 저장

### 4. SQL (Structured Query Language)
* 표준화된 쿼리 언어
* 데이터 정의(DDL), 조작(DML), 제어(DCL) 지원
* 복잡한 쿼리 및 조인 연산

### 5. 데이터 무결성
* 제약 조건(Constraint)을 통한 데이터 검증
* 기본키(Primary Key), 외래키(Foreign Key)
* 체크(Check), 유니크(Unique), NOT NULL 제약

---

## RDBMS의 핵심 개념

### 1. 테이블 (Table)
데이터를 저장하는 기본 단위

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 2. 행 (Row / Record)
테이블의 한 개의 데이터 레코드

```sql
INSERT INTO users (id, name, email) 
VALUES (1, '홍길동', 'hong@example.com');
```

### 3. 열 (Column / Field)
테이블의 속성(Attribute)

```sql
SELECT name, email FROM users;
```

### 4. 기본키 (Primary Key)
각 행을 고유하게 식별하는 키

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100)
);
```

### 5. 외래키 (Foreign Key)
다른 테이블의 기본키를 참조하는 키

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### 6. 인덱스 (Index)
데이터 검색 속도를 향상시키는 구조

```sql
CREATE INDEX idx_email ON users(email);
```

### 7. 뷰 (View)
가상의 테이블로, 쿼리 결과를 저장

```sql
CREATE VIEW user_orders AS
SELECT u.name, o.order_id, o.order_date
FROM users u
JOIN orders o ON u.id = o.user_id;
```

---

## RDBMS의 장점

### 1. 데이터 무결성
* 제약 조건을 통한 데이터 검증
* 참조 무결성 보장
* 일관된 데이터 관리

### 2. 복잡한 쿼리
* JOIN을 통한 관계형 데이터 조회
* 집계 함수 및 서브쿼리
* 복잡한 비즈니스 로직 구현

### 3. 표준화
* SQL 표준 준수
* 다양한 RDBMS 간 이식성
* 광범위한 지원 및 문서

### 4. 트랜잭션 지원
* ACID 특성 보장
* 데이터 일관성 유지
* 동시성 제어

### 5. 보안
* 사용자 권한 관리
* 역할 기반 접근 제어
* 데이터 암호화 지원

---

## RDBMS의 단점

### 1. 확장성 제한
* 수직 확장(Scale-up) 위주
* 수평 확장(Scale-out) 어려움
* 대용량 데이터 처리 시 성능 저하

### 2. 스키마 변경
* 스키마 변경이 복잡하고 비용이 큼
* 마이그레이션 작업 필요
* 다운타임 발생 가능

### 3. 복잡한 관계
* 복잡한 JOIN 연산
* 성능 최적화 어려움
* 쿼리 작성 복잡도 증가

### 4. 비용
* 라이선스 비용 (상용 DB)
* 하드웨어 비용
* 전문 인력 필요

---

## 주요 RDBMS 제품

### 1. MySQL
* 가장 인기 있는 오픈소스 RDBMS
* 웹 애플리케이션에 널리 사용
* 빠른 읽기 성능
* 커뮤니티 에디션 무료

### 2. PostgreSQL
* 고급 기능을 갖춘 오픈소스 RDBMS
* SQL 표준 준수도 높음
* 확장성 및 안정성 우수
* 복잡한 쿼리 처리에 강함

### 3. Oracle Database
* 엔터프라이즈급 상용 RDBMS
* 높은 성능과 안정성
* 다양한 고급 기능
* 높은 라이선스 비용

### 4. Microsoft SQL Server
* 마이크로소프트의 RDBMS
* Windows 환경과 통합
* .NET 애플리케이션과 호환성 우수
* Express 버전 무료 제공

### 5. MariaDB
* MySQL의 포크(Fork)
* 오픈소스
* MySQL과 호환성 높음
* 커뮤니티 중심 개발

### 6. SQLite
* 경량 임베디드 데이터베이스
* 파일 기반
* 설정 불필요
* 모바일 앱에 적합

---

## RDBMS 선택 가이드

### 프로젝트 규모
* **소규모**: SQLite, MySQL
* **중규모**: MySQL, PostgreSQL
* **대규모**: PostgreSQL, Oracle, SQL Server

### 예산
* **무료/오픈소스**: MySQL, PostgreSQL, MariaDB, SQLite
* **상용**: Oracle, SQL Server

### 기능 요구사항
* **기본 기능**: MySQL, SQLite
* **고급 기능**: PostgreSQL, Oracle
* **엔터프라이즈 기능**: Oracle, SQL Server

### 플랫폼
* **Linux**: MySQL, PostgreSQL, MariaDB
* **Windows**: SQL Server, MySQL, PostgreSQL
* **크로스 플랫폼**: MySQL, PostgreSQL, SQLite

---

## 데이터베이스 설계 원칙

### 1. 정규화 (Normalization)
* 데이터 중복 제거
* 1NF, 2NF, 3NF, BCNF
* 데이터 무결성 향상

### 2. 반정규화 (Denormalization)
* 성능 최적화를 위한 선택적 중복
* 읽기 성능 향상
* 트레이드오프 고려

### 3. 인덱싱 전략
* 자주 조회되는 컬럼에 인덱스 생성
* 복합 인덱스 활용
* 인덱스 오버헤드 고려

### 4. 관계 설계
* 적절한 관계 타입 선택
* 외래키 제약 조건
* 참조 무결성 보장

---

## RDBMS 모범 사례

### 1. 스키마 설계
* 명확한 네이밍 컨벤션
* 적절한 데이터 타입 선택
* 제약 조건 활용
* 문서화

### 2. 쿼리 최적화
* 인덱스 활용
* EXPLAIN으로 실행 계획 확인
* 불필요한 JOIN 제거
* 서브쿼리 최적화

### 3. 트랜잭션 관리
* 적절한 트랜잭션 범위
* 데드락 방지
* 롤백 전략 수립

### 4. 백업 및 복구
* 정기적인 백업
* 백업 검증
* 복구 계획 수립
* 재해 복구 전략

### 5. 보안
* 최소 권한 원칙
* SQL 인젝션 방지
* 데이터 암호화
* 정기적인 보안 감사

### 6. 모니터링
* 성능 모니터링
* 리소스 사용량 추적
* 느린 쿼리 로그 분석
* 용량 계획

---

## RDBMS vs NoSQL

| 구분 | RDBMS | NoSQL |
|:---|:---|:---|
| **데이터 모델** | 테이블 (행/열) | 문서, 키-값, 그래프 등 |
| **스키마** | 고정 (엄격) | 유연 (스키마리스) |
| **확장성** | 수직 확장 | 수평 확장 |
| **ACID** | 완전 지원 | 제한적 지원 |
| **쿼리 언어** | SQL | 제품별 고유 언어 |
| **복잡한 관계** | JOIN 지원 | 제한적 |
| **일관성** | 강한 일관성 | 최종 일관성 |
| **사용 사례** | 트랜잭션 시스템, ERP | 빅데이터, 실시간 분석 |

---

## RDBMS 사용 사례

### 1. 트랜잭션 시스템
* 은행 시스템
* 전자상거래
* 주문 관리 시스템

### 2. ERP 시스템
* 재고 관리
* 회계 시스템
* 인사 관리

### 3. 콘텐츠 관리
* 블로그
* CMS
* 위키

### 4. 데이터 웨어하우스
* 비즈니스 인텔리전스
* 리포트 생성
* 데이터 분석

---

## 참고

* RDBMS는 구조화된 데이터 관리에 최적화되어 있습니다
* 프로젝트 요구사항에 맞는 RDBMS를 선택하세요
* 정규화와 성능 사이의 균형을 고려하세요
* 정기적인 백업과 모니터링이 필수입니다
* 쿼리 최적화를 통해 성능을 향상시키세요

