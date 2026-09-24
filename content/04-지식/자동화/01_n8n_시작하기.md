---
publish: true
tags: [지식, 자동화]
---

# n8n 시작하기 (Docker)

---

## n8n이란?

> **n8n**은 오픈소스 워크플로우 자동화 도구입니다. 코드 없이 시각적으로 워크플로우를 구축할 수 있으며, 다양한 서비스와 API를 연결하여 자동화 작업을 수행할 수 있습니다. Zapier, Make(Integromat)와 유사하지만 자체 호스팅이 가능한 도구입니다.

---

## 목차

1. [Docker로 n8n 실행하기](#docker로-n8n-실행하기)
2. [Docker Compose 사용하기](#docker-compose-사용하기)
3. [환경 변수 설정](#환경-변수-설정)
4. [데이터 영속성 설정](#데이터-영속성-설정)
5. [포트 및 네트워크 설정](#포트-및-네트워크-설정)
6. [고급 설정](#고급-설정)
7. [문제 해결](#문제-해결)

---

## Docker로 n8n 실행하기

### 기본 실행 명령어

가장 간단한 방법으로 n8n을 실행합니다:

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  n8nio/n8n
```

**설명**:
- `-it`: 인터랙티브 모드로 실행
- `--rm`: 컨테이너 종료 시 자동 삭제
- `--name n8n`: 컨테이너 이름 지정
- `-p 5678:5678`: 호스트의 5678 포트를 컨테이너의 5678 포트에 매핑
- `n8nio/n8n`: 공식 n8n Docker 이미지

### 백그라운드 실행

```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  n8nio/n8n
```

**설명**:
- `-d`: 백그라운드(데몬) 모드로 실행

### 데이터 영속성을 위한 볼륨 마운트

워크플로우와 설정을 영구적으로 저장하려면 볼륨을 마운트합니다:

```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

**설명**:
- `-v ~/.n8n:/home/node/.n8n`: 호스트의 `~/.n8n` 디렉토리를 컨테이너의 n8n 데이터 디렉토리에 마운트

### 환경 변수 설정

```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  -e N8N_BASIC_AUTH_ACTIVE=true \
  -e N8N_BASIC_AUTH_USER=admin \
  -e N8N_BASIC_AUTH_PASSWORD=yourpassword \
  n8nio/n8n
```

---

## Docker Compose 사용하기

### 기본 docker-compose.yml

```yaml
version: '3.8'

services:
  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: unless-stopped
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=yourpassword
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
    volumes:
      - ~/.n8n:/home/node/.n8n
    networks:
      - n8n-network

networks:
  n8n-network:
    driver: bridge
```

### 실행 방법

```bash
# docker-compose.yml 파일이 있는 디렉토리에서 실행
docker-compose up -d

# 로그 확인
docker-compose logs -f n8n

# 중지
docker-compose down

# 중지 및 볼륨 삭제
docker-compose down -v
```

### 왜 데이터베이스를 함께 사용하나요?

n8n은 기본적으로 **SQLite**를 사용합니다. 하지만 프로덕션 환경이나 특정 상황에서는 **PostgreSQL** 또는 **MySQL** 같은 외부 데이터베이스를 사용하는 것이 권장됩니다.

#### SQLite vs 외부 데이터베이스 (PostgreSQL/MySQL)

| 구분 | SQLite (기본) | PostgreSQL/MySQL |
|:---|:---|:---|
| **사용 시기** | 개발, 테스트, 소규모 프로젝트 | 프로덕션, 대규모 프로젝트 |
| **동시 접근** | 제한적 (단일 사용자 권장) | 다중 사용자 동시 접근 가능 |
| **성능** | 소규모 데이터에 적합 | 대용량 데이터 처리에 우수 |
| **확장성** | 제한적 | 높은 확장성 |
| **백업** | 파일 복사 | 표준 DB 백업 도구 사용 가능 |
| **복제** | 불가능 | 마스터-슬레이브 복제 가능 |
| **트랜잭션** | 기본 지원 | 고급 트랜잭션 기능 |
| **설정 복잡도** | 간단 (설정 불필요) | 초기 설정 필요 |

#### 외부 데이터베이스를 사용해야 하는 경우

1. **다중 사용자 환경**: 여러 사용자가 동시에 워크플로우를 실행할 때
2. **대용량 데이터**: 많은 워크플로우와 실행 기록을 저장할 때
3. **고가용성**: 서버 장애 시에도 데이터 손실을 방지해야 할 때
4. **백업 및 복구**: 표준 데이터베이스 백업 도구를 사용하고 싶을 때
5. **확장성**: 향후 확장 계획이 있을 때
6. **기존 인프라 활용**: 이미 PostgreSQL/MySQL 인프라가 있을 때

#### SQLite로 충분한 경우

1. **개인 사용**: 혼자 사용하는 개인 프로젝트
2. **소규모 팀**: 소수의 사용자만 사용
3. **간단한 워크플로우**: 복잡하지 않은 자동화 작업
4. **빠른 시작**: 최소한의 설정으로 시작하고 싶을 때

### PostgreSQL과 함께 사용하기

n8n을 PostgreSQL 데이터베이스와 함께 사용하는 예제:

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:15
    container_name: n8n-postgres
    restart: unless-stopped
    environment:
      - POSTGRES_DB=n8n
      - POSTGRES_USER=n8n
      - POSTGRES_PASSWORD=n8n
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - n8n-network

  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: unless-stopped
    ports:
      - "5678:5678"
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=n8n
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=yourpassword
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
    volumes:
      - ~/.n8n:/home/node/.n8n
    depends_on:
      - postgres
    networks:
      - n8n-network

volumes:
  postgres_data:

networks:
  n8n-network:
    driver: bridge
```

### MySQL과 함께 사용하기

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: n8n-mysql
    restart: unless-stopped
    environment:
      - MYSQL_DATABASE=n8n
      - MYSQL_USER=n8n
      - MYSQL_PASSWORD=n8n
      - MYSQL_ROOT_PASSWORD=rootpassword
    volumes:
      - mysql_data:/var/lib/mysql
    networks:
      - n8n-network

  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: unless-stopped
    ports:
      - "5678:5678"
    environment:
      - DB_TYPE=mysqldb
      - DB_MYSQLDB_HOST=mysql
      - DB_MYSQLDB_PORT=3306
      - DB_MYSQLDB_DATABASE=n8n
      - DB_MYSQLDB_USER=n8n
      - DB_MYSQLDB_PASSWORD=n8n
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=yourpassword
    volumes:
      - ~/.n8n:/home/node/.n8n
    depends_on:
      - mysql
    networks:
      - n8n-network

volumes:
  mysql_data:

networks:
  n8n-network:
    driver: bridge
```

---

## 환경 변수 설정

### 인증 관련

```bash
# 기본 인증 활성화
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=yourpassword

# JWT 시크릿 (프로덕션 환경)
N8N_SECURE_COOKIE=false
N8N_SESSION_COOKIE_NAME=n8n-session
```

### 데이터베이스 설정

```bash
# SQLite (기본값)
# 별도 설정 불필요

# PostgreSQL
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=localhost
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=n8n
DB_POSTGRESDB_USER=n8n
DB_POSTGRESDB_PASSWORD=yourpassword

# MySQL
DB_TYPE=mysqldb
DB_MYSQLDB_HOST=localhost
DB_MYSQLDB_PORT=3306
DB_MYSQLDB_DATABASE=n8n
DB_MYSQLDB_USER=n8n
DB_MYSQLDB_PASSWORD=yourpassword
```

### 호스트 및 포트 설정

```bash
# 호스트 설정
N8N_HOST=localhost
N8N_PORT=5678
N8N_PROTOCOL=http

# HTTPS 사용 시
N8N_PROTOCOL=https
N8N_PORT=443
```

### 기타 설정

```bash
# 타임존 설정
TZ=Asia/Seoul

# 로그 레벨
N8N_LOG_LEVEL=info

# 웹훅 URL (외부 접근 시)
WEBHOOK_URL=https://yourdomain.com/

# 실행할 워크플로우 수 제한
EXECUTIONS_PROCESS=main
EXECUTIONS_MODE=regular
```

---

## 데이터 영속성 설정

### 볼륨 마운트

```bash
# Docker 명령어
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n

# 또는 절대 경로 사용
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v /path/to/n8n/data:/home/node/.n8n \
  n8nio/n8n
```

### Docker Compose에서 볼륨 사용

```yaml
services:
  n8n:
    image: n8nio/n8n
    volumes:
      # 이름 있는 볼륨
      - n8n_data:/home/node/.n8n
      
      # 또는 호스트 경로
      - ~/.n8n:/home/node/.n8n

volumes:
  n8n_data:
```

### 백업 및 복원

```bash
# 백업
docker cp n8n:/home/node/.n8n ~/n8n-backup

# 복원
docker cp ~/n8n-backup n8n:/home/node/.n8n
docker restart n8n
```

---

## 포트 및 네트워크 설정

### 포트 변경

```bash
# 다른 포트로 실행 (예: 8080)
docker run -d \
  --name n8n \
  -p 8080:5678 \
  n8nio/n8n
```

### Docker Compose에서 포트 설정

```yaml
services:
  n8n:
    ports:
      - "8080:5678"  # 호스트:컨테이너
```

### 네트워크 설정

```bash
# 사용자 정의 네트워크 생성
docker network create n8n-network

# 네트워크에 연결하여 실행
docker run -d \
  --name n8n \
  --network n8n-network \
  -p 5678:5678 \
  n8nio/n8n
```

---

## 고급 설정

### 리버스 프록시와 함께 사용 (Nginx)

```nginx
# /etc/nginx/sites-available/n8n
server {
    listen 80;
    server_name n8n.yourdomain.com;

    location / {
        proxy_pass http://localhost:5678;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### 환경 변수 파일 사용

`.env` 파일 생성:

```bash
# .env
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=yourpassword
N8N_HOST=localhost
N8N_PORT=5678
TZ=Asia/Seoul
```

Docker Compose에서 사용:

```yaml
services:
  n8n:
    image: n8nio/n8n
    env_file:
      - .env
```

### 특정 버전 사용

```bash
# 특정 버전 태그 사용
docker run -d \
  --name n8n \
  -p 5678:5678 \
  n8nio/n8n:1.0.0

# 또는 docker-compose.yml
services:
  n8n:
    image: n8nio/n8n:1.0.0
```

### 리소스 제한 설정

```yaml
services:
  n8n:
    image: n8nio/n8n
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 2G
        reservations:
          cpus: '1'
          memory: 1G
```

---

## 문제 해결

### 컨테이너 로그 확인

```bash
# 실시간 로그 확인
docker logs -f n8n

# 최근 100줄 로그
docker logs --tail 100 n8n
```

### 컨테이너 상태 확인

```bash
# 실행 중인 컨테이너 확인
docker ps

# 모든 컨테이너 확인 (중지된 것 포함)
docker ps -a

# 컨테이너 상세 정보
docker inspect n8n
```

### 포트 충돌 해결

```bash
# 포트 사용 중인 프로세스 확인 (Linux/Mac)
lsof -i :5678

# Windows
netstat -ano | findstr :5678

# 다른 포트 사용
docker run -d --name n8n -p 8080:5678 n8nio/n8n
```

### 권한 문제 해결 (EACCES: permission denied)

이 오류는 n8n 컨테이너가 볼륨 마운트된 디렉토리에 쓰기 권한이 없을 때 발생합니다.

#### 해결 방법 1: 디렉토리 권한 변경 (Linux/Mac)

```bash
# n8n 컨테이너는 node 사용자(UID 1000)로 실행됩니다
# 볼륨 디렉토리의 소유권을 변경합니다

# 디렉토리 생성 (없는 경우)
mkdir -p ~/.n8n

# 권한 변경
sudo chown -R 1000:1000 ~/.n8n

# 또는 현재 사용자로 실행하려면
sudo chown -R $(id -u):$(id -g) ~/.n8n
```

#### 해결 방법 2: Docker Compose에서 사용자 지정

```yaml
services:
  n8n:
    image: n8nio/n8n
    user: "1000:1000"  # node 사용자로 실행
    volumes:
      - ~/.n8n:/home/node/.n8n
```

#### 해결 방법 3: Docker 명령어에서 사용자 지정

```bash
# Linux/Mac
docker run -d \
  --name n8n \
  --user 1000:1000 \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n

# Windows (PowerShell)
docker run -d `
  --name n8n `
  --user 1000:1000 `
  -p 5678:5678 `
  -v ${env:USERPROFILE}\.n8n:/home/node/.n8n `
  n8nio/n8n
```

#### 해결 방법 4: 볼륨 대신 이름 있는 볼륨 사용 (권장)

권한 문제를 피하는 가장 좋은 방법은 이름 있는 볼륨을 사용하는 것입니다:

```bash
# Docker 명령어
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  n8nio/n8n
```

```yaml
# docker-compose.yml
services:
  n8n:
    image: n8nio/n8n
    volumes:
      - n8n_data:/home/node/.n8n  # 이름 있는 볼륨 사용

volumes:
  n8n_data:  # Docker가 자동으로 관리
```

#### 해결 방법 5: Windows에서 권한 문제 해결

Windows에서는 다음과 같이 해결할 수 있습니다:

```powershell
# 방법 1: Docker Desktop의 파일 공유 설정 확인
# Docker Desktop > Settings > Resources > File Sharing
# C:\Users 폴더가 공유되어 있는지 확인

# 방법 2: 절대 경로 사용
docker run -d `
  --name n8n `
  -p 5678:5678 `
  -v C:\n8n-data:/home/node/.n8n `
  n8nio/n8n

# 방법 3: 이름 있는 볼륨 사용 (가장 권장)
docker run -d `
  --name n8n `
  -p 5678:5678 `
  -v n8n_data:/home/node/.n8n `
  n8nio/n8n
```

#### 해결 방법 6: 컨테이너 내부에서 권한 확인

```bash
# 컨테이너 내부 접속
docker exec -it n8n sh

# 사용자 확인
whoami
id

# 디렉토리 권한 확인
ls -la /home/node/.n8n

# 권한이 없다면 수동으로 변경 (임시 해결책)
chmod -R 777 /home/node/.n8n
```

#### 해결 방법 7: 기존 컨테이너 제거 후 재생성

```bash
# 기존 컨테이너 중지 및 제거
docker stop n8n
docker rm n8n

# 권한 설정 후 재생성
sudo chown -R 1000:1000 ~/.n8n  # Linux/Mac
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

#### 권한 문제 예방 (베스트 프랙티스)

1. **이름 있는 볼륨 사용** (가장 권장)
   ```yaml
   volumes:
     - n8n_data:/home/node/.n8n
   ```

2. **디렉토리 미리 생성 및 권한 설정**
   ```bash
   mkdir -p ~/.n8n
   sudo chown -R 1000:1000 ~/.n8n
   ```

3. **Docker Compose 사용**
   - Docker Compose가 자동으로 볼륨을 관리합니다

4. **절대 경로 사용**
   - 상대 경로 대신 절대 경로 사용

### 데이터베이스 연결 문제

```bash
# PostgreSQL 연결 테스트
docker exec -it n8n-postgres psql -U n8n -d n8n

# MySQL 연결 테스트
docker exec -it n8n-mysql mysql -u n8n -p n8n
```

### 컨테이너 재시작

```bash
# 컨테이너 재시작
docker restart n8n

# 컨테이너 중지 후 시작
docker stop n8n
docker start n8n

# 컨테이너 완전히 제거 후 재생성
docker stop n8n
docker rm n8n
docker run -d --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n
```

### 웹 인터페이스 접근 불가

1. **포트 확인**: `docker ps`로 포트 매핑 확인
2. **방화벽 확인**: 호스트 방화벽에서 포트 열기
3. **호스트 설정 확인**: `N8N_HOST` 환경 변수 확인
4. **프로토콜 확인**: `N8N_PROTOCOL` 환경 변수 확인

### Python Task Runner 경고 메시지

다음과 같은 메시지가 나타날 수 있습니다:

```
Failed to start Python task runner in internal mode. because Python 3 is missing from this system.
```

#### 이것은 오류가 아닙니다!

이 메시지는 **경고**이며, n8n은 정상적으로 실행되고 있습니다. JavaScript Task Runner로 모든 워크플로우를 실행할 수 있습니다.

#### Python Task Runner가 필요한 경우

Python Task Runner는 다음 경우에만 필요합니다:

1. **Python 코드 실행**: n8n에서 Python 스크립트를 실행하는 노드 사용
2. **특정 Python 라이브러리**: Python 전용 라이브러리를 사용하는 워크플로우
3. **고급 Python 통합**: Python 기반 API나 도구와의 통합

**대부분의 경우 Python Task Runner는 필요하지 않습니다!**

#### Python Task Runner 활성화하기 (선택사항)

Python 작업이 필요한 경우에만 다음 방법을 사용하세요:

##### 방법 1: Python이 포함된 커스텀 이미지 사용

```dockerfile
# Dockerfile
FROM n8nio/n8n

# Python 3 설치
RUN apk add --no-cache python3 py3-pip
```

```bash
# 이미지 빌드
docker build -t n8n-with-python .

# 실행
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  n8n-with-python
```

##### 방법 2: Docker Compose에서 Python 설치

```yaml
version: '3.8'

services:
  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: unless-stopped
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
    # Python 3 설치 (Alpine Linux 기반)
    command: sh -c "apk add --no-cache python3 py3-pip && n8n start"
```

##### 방법 3: 외부 Python Task Runner 사용 (프로덕션 권장)

프로덕션 환경에서는 외부 모드로 Python Task Runner를 실행하는 것이 권장됩니다:

```bash
# Python Task Runner를 별도 컨테이너로 실행
docker run -d \
  --name n8n-python-runner \
  -p 5679:5679 \
  n8nio/n8n:latest \
  n8n worker --concurrency=1 --queues=python
```

```yaml
# docker-compose.yml
version: '3.8'

services:
  n8n:
    image: n8nio/n8n
    environment:
      - EXECUTIONS_PROCESS=main
      - QUEUE_BULL_REDIS_HOST=redis
    # ... 기타 설정

  n8n-python-worker:
    image: n8nio/n8n
    command: n8n worker --concurrency=1 --queues=python
    environment:
      - QUEUE_BULL_REDIS_HOST=redis
    # ... 기타 설정

  redis:
    image: redis:alpine
```

#### 확인 방법

n8n이 정상적으로 실행 중인지 확인:

```bash
# 로그 확인
docker logs n8n

# 다음 메시지가 보이면 정상입니다:
# "Registered runner 'JS Task Runner'"
# "Version: X.X.X"
```

브라우저에서 `http://localhost:5678`에 접속할 수 있으면 정상적으로 실행 중입니다.

#### 요약

- ✅ **Python Task Runner 경고는 무시해도 됩니다**
- ✅ **대부분의 워크플로우는 JavaScript Task Runner로 충분합니다**
- ✅ **Python 코드를 실행해야 하는 경우에만 Python Task Runner가 필요합니다**
- ✅ **프로덕션 환경에서는 외부 모드로 Python Task Runner를 실행하는 것이 권장됩니다**

---

## 빠른 시작 가이드

### 1단계: Docker 설치 확인

```bash
docker --version
docker-compose --version
```

### 2단계: n8n 실행

```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

### 3단계: 웹 인터페이스 접근

브라우저에서 `http://localhost:5678` 접속

### 4단계: 초기 설정

1. 첫 접속 시 계정 생성 화면 표시
2. 이메일과 비밀번호 입력
3. 워크플로우 생성 시작

---

## 유용한 명령어 모음

```bash
# n8n 컨테이너 시작
docker start n8n

# n8n 컨테이너 중지
docker stop n8n

# n8n 컨테이너 재시작
docker restart n8n

# n8n 컨테이너 로그 확인
docker logs -f n8n

# n8n 컨테이너 내부 접속
docker exec -it n8n sh

# n8n 컨테이너 제거
docker rm n8n

# n8n 이미지 업데이트
docker pull n8nio/n8n
docker stop n8n
docker rm n8n
docker run -d --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n

# Docker Compose로 업데이트
docker-compose pull
docker-compose up -d
```

---

## 참고 자료

* [n8n 공식 문서](https://docs.n8n.io/)
* [n8n Docker Hub](https://hub.docker.com/r/n8nio/n8n)
* [n8n GitHub](https://github.com/n8n-io/n8n)
* [n8n 커뮤니티](https://community.n8n.io/)

---

## 참고

* n8n은 기본적으로 SQLite를 사용하지만, 프로덕션 환경에서는 PostgreSQL 또는 MySQL 사용을 권장합니다
* 데이터 영속성을 위해 반드시 볼륨을 마운트하세요
* 프로덕션 환경에서는 기본 인증을 활성화하고 강력한 비밀번호를 사용하세요
* 정기적으로 데이터를 백업하세요
* Docker Compose를 사용하면 설정 관리가 더 쉬워집니다
