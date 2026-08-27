---
publish: true
tags: [지식, 데브옵스]
---

# Docker를 활용한 배포

---

## 개요

**Docker**는 애플리케이션과 그 의존성을 컨테이너로 패키징하여 어떤 환경에서도 동일하게 실행할 수 있게 해주는 플랫폼입니다. OS 레벨 가상화를 사용하여 가상머신보다 가볍고 빠릅니다.

### 특징

* **컨테이너 기반**: 애플리케이션을 격리된 환경에서 실행
* **이식성**: 개발, 테스트, 프로덕션 환경 동일
* **효율성**: 가상머신보다 빠르고 경량
* **확장성**: 쉽게 복제 및 배포
* **에코시스템**: Docker Hub, Docker Compose 등 다양한 도구

### Docker의 역사

* 2013년: Docker 오픈소스 공개
* 컨테이너 기술의 대중화
* Kubernetes 등 오케스트레이션 도구와 함께 발전

---

## Docker란?

> **Docker**는 애플리케이션을 컨테이너로 패키징하여 어떤 환경에서도 동일하게 실행할 수 있게 해주는 플랫폼입니다.

### 작동 원리

1. **Dockerfile**: 이미지를 빌드하기 위한 명령어 작성
2. **Image 빌드**: Dockerfile을 읽어 이미지 생성
3. **Container 실행**: 이미지를 실행하여 컨테이너 생성
4. **격리 실행**: 호스트 OS와 격리된 환경에서 실행

### 컨테이너 vs 가상머신

| 특징 | 컨테이너 | 가상머신 |
| ---- | -------- | -------- |
| **격리 수준** | 프로세스 격리 | 하드웨어 격리 |
| **OS** | 호스트 OS 공유 | 게스트 OS 필요 |
| **시작 시간** | 초 단위 | 분 단위 |
| **리소스 사용** | 적음 | 많음 |
| **이식성** | 높음 | 중간 |

---

## 핵심 요소와 개념

### 1. Image (이미지)

이미지는 애플리케이션과 실행 환경을 패키징한 읽기 전용 템플릿입니다.

**이미지 계층 구조**:
* **Base Layer**: 운영체제 레이어
* **Intermediate Layers**: 각 Dockerfile 명령어마다 레이어 생성
* **Top Layer**: 읽기/쓰기 가능한 레이어

**레이어 캐싱**:
* 변경되지 않은 레이어는 재사용
* 빌드 시간 단축
* 저장 공간 절약

**심화 학습**:
* Multi-stage builds: 빌드 레이어 최적화
* 이미지 크기 최적화: Alpine Linux 사용
* 이미지 스캔: 보안 취약점 검사

### 2. Container (컨테이너)

컨테이너는 이미지를 실행한 인스턴스입니다.

**컨테이너 생명주기**:
1. 생성 (Created)
2. 시작 (Running)
3. 일시 정지 (Paused)
4. 중지 (Stopped)
5. 삭제 (Removed)

**컨테이너 격리**:
* **네임스페이스**: 프로세스, 네트워크, 파일시스템 격리
* **컨트롤 그룹 (cgroups)**: 리소스 제한
* **Union File System**: 레이어 기반 파일시스템

**심화 학습**:
* 컨테이너 내부 구조 이해
* 리소스 제한 설정
* 컨테이너 간 통신

### 3. Dockerfile

Dockerfile은 이미지를 빌드하기 위한 명령어 집합입니다.

**주요 명령어**:

* **FROM**: 베이스 이미지 지정
* **WORKDIR**: 작업 디렉토리 설정
* **COPY/ADD**: 파일 복사
* **RUN**: 명령어 실행 (이미지 빌드 시)
* **CMD/ENTRYPOINT**: 컨테이너 시작 시 실행 명령
* **ENV**: 환경 변수 설정
* **EXPOSE**: 포트 노출 선언
* **VOLUME**: 볼륨 마운트 지점 선언

**명령어 순서 최적화**:
* 자주 변경되는 명령어는 아래에 배치
* 캐시 활용 최대화

**심화 학습**:
* Multi-stage builds
* Build arguments
* Health check 설정

### 4. Docker Registry

레지스트리는 Docker 이미지를 저장하고 배포하는 저장소입니다.

**공개 레지스트리**:
* **Docker Hub**: 공식 레지스트리
* **GitHub Container Registry**: GitHub 통합
* **Quay.io**: Red Hat 제공

**프라이빗 레지스트리**:
* **AWS ECR**: Amazon 제공
* **Google Container Registry**: GCP 제공
* **Azure Container Registry**: Azure 제공
* **Docker Registry**: 자체 호스팅

**이미지 태깅 전략**:
* Semantic versioning: `v1.0.0`
* Git commit hash: `abc123`
* Latest 태그: 프로덕션에서 피하기

### 5. Volumes (볼륨)

볼륨은 컨테이너와 호스트 간 데이터를 공유하는 메커니즘입니다.

**볼륨 유형**:
* **Named Volumes**: Docker가 관리하는 볼륨
* **Bind Mounts**: 호스트 경로 직접 마운트
* **tmpfs Mounts**: 메모리 기반 임시 파일시스템

**사용 사례**:
* 데이터베이스 데이터 영구 저장
* 로그 파일 저장
* 설정 파일 공유

**심화 학습**:
* 볼륨 드라이버
* 볼륨 백업 및 복구
* 볼륨 권한 관리

### 6. Networks (네트워크)

Docker 네트워크는 컨테이너 간 통신을 위한 가상 네트워크입니다.

**네트워크 유형**:
* **Bridge**: 기본 네트워크, 단일 호스트 내 통신
* **Host**: 호스트 네트워크 직접 사용
* **Overlay**: 여러 호스트 간 통신 (Swarm 모드)
* **Macvlan**: 컨테이너에 MAC 주소 할당

**네트워크 격리**:
* 기본적으로 컨테이너는 격리됨
* 네트워크를 통해서만 통신 가능
* 포트 매핑으로 외부 접근

**심화 학습**:
* 사용자 정의 브리지 네트워크
* DNS 기반 서비스 디스커버리
* 네트워크 보안 설정

### 7. Docker Compose

여러 컨테이너를 하나의 파일로 관리합니다.

**Compose 파일 구조**:
```yaml
version: '3.8'
services:    # 서비스 정의
volumes:     # 볼륨 정의
networks:    # 네트워크 정의
```

**핵심 기능**:
* 서비스 정의 및 의존성 관리
* 환경 변수 관리
* 볼륨 및 네트워크 정의
* 스케일링 지원

**심화 학습**:
* Compose 파일 확장 (extends)
* 환경별 Compose 파일 분리
* 프로덕션 배포 전략

---

## Docker의 장점

* **일관성**: 개발, 테스트, 프로덕션 환경 동일
* **격리**: 애플리케이션 간 독립적 실행
* **이식성**: 어느 환경에서나 동일하게 동작
* **효율성**: 가상머신보다 가볍고 빠름
* **확장성**: 쉽게 복제 및 배포 가능

---

## Docker 기본 개념

### 주요 용어

| 용어 | 설명 |
| ---- | ---- |
| **Image** | 애플리케이션과 실행 환경을 패키징한 읽기 전용 템플릿 |
| **Container** | 이미지를 실행한 인스턴스 (실행 중인 프로세스) |
| **Dockerfile** | 이미지를 빌드하기 위한 명령어 집합 |
| **Registry** | 이미지를 저장/공유하는 저장소 (Docker Hub 등) |
| **Volume** | 컨테이너와 호스트 간 데이터 공유 |

---

## Dockerfile 작성

### 기본 예시

```dockerfile
# Base 이미지
FROM node:18-alpine

# 작업 디렉토리 설정
WORKDIR /app

# 의존성 파일 복사
COPY package*.json ./

# 의존성 설치
RUN npm ci --only=production

# 애플리케이션 코드 복사
COPY . .

# 포트 노출
EXPOSE 3000

# 실행 명령
CMD ["node", "index.js"]
```

### Python 예시

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "app.py"]
```

---

## Docker 빌드 및 실행

### 이미지 빌드

```bash
# 기본 빌드
docker build -t my-app:latest .

# 태그 지정
docker build -t my-app:v1.0.0 .
```

### 컨테이너 실행

```bash
# 기본 실행
docker run my-app:latest

# 포트 매핑
docker run -p 3000:3000 my-app:latest

# 환경 변수 전달
docker run -e NODE_ENV=production my-app:latest

# 볼륨 마운트
docker run -v /host/data:/container/data my-app:latest

# 백그라운드 실행
docker run -d --name my-container my-app:latest
```

---

## 이미지 배포

### Docker Hub에 푸시

```bash
# 로그인
docker login

# 태그 지정
docker tag my-app:latest username/my-app:latest

# 푸시
docker push username/my-app:latest
```

### 다른 Registry 사용

```bash
# AWS ECR
aws ecr get-login-password | docker login --username AWS --password-stdin {account-id}.dkr.ecr.{region}.amazonaws.com
docker push {account-id}.dkr.ecr.{region}.amazonaws.com/my-app:latest

# Google Container Registry
docker tag my-app:latest gcr.io/{project-id}/my-app:latest
docker push gcr.io/{project-id}/my-app:latest
```

---

## Docker Compose

여러 컨테이너를 하나의 파일로 관리:

### docker-compose.yml 예시

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://user:pass@db:5432/mydb
    depends_on:
      - db
    volumes:
      - ./data:/app/data

  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### 실행 명령

```bash
# 서비스 시작
docker-compose up -d

# 서비스 중지
docker-compose down

# 로그 확인
docker-compose logs -f

# 재빌드
docker-compose up --build
```

---

## 프로덕션 배포 전략

### 1. 멀티스테이지 빌드

```dockerfile
# Build stage
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY --from=builder /app/dist ./dist
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

### 2. .dockerignore 사용

```
node_modules
npm-debug.log
.git
.gitignore
.env
*.md
```

### 3. 보안 모범 사례

* 비 root 사용자로 실행
* 최신 베이스 이미지 사용
* 불필요한 패키지 설치 금지
* 시크릿 정보 환경 변수로 관리

---

## Docker 명령어 정리

### 이미지 관리

```bash
docker images              # 이미지 목록
docker rmi <image-id>      # 이미지 삭제
docker pull <image>        # 이미지 다운로드
docker push <image>        # 이미지 업로드
```

### 컨테이너 관리

```bash
docker ps                  # 실행 중인 컨테이너
docker ps -a               # 모든 컨테이너
docker stop <container>    # 컨테이너 중지
docker start <container>   # 컨테이너 시작
docker rm <container>      # 컨테이너 삭제
docker logs <container>    # 로그 확인
docker exec -it <container> /bin/bash  # 컨테이너 접속
```

### 시스템 관리

```bash
docker system df           # 디스크 사용량
docker system prune        # 사용하지 않는 리소스 정리
docker volume ls           # 볼륨 목록
docker network ls          # 네트워크 목록
```

---

## CI/CD 통합

### GitHub Actions 예시

```yaml
- name: Build Docker image
  run: docker build -t my-app:${{ github.sha }} .

- name: Push to Docker Hub
  run: |
    docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
    docker push my-app:${{ github.sha }}
```

### Jenkins 예시

```groovy
stage('Docker Build') {
    steps {
        sh 'docker build -t my-app:${BUILD_NUMBER} .'
    }
}

stage('Docker Push') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
            sh 'docker login -u $USER -p $PASS'
            sh 'docker push my-app:${BUILD_NUMBER}'
        }
    }
}
```

---

---

## 심화 학습 포인트

### 1. 멀티스테이지 빌드 (Multi-stage Builds)

빌드 도구는 필요하지만 실행에는 필요 없는 경우 최적화합니다.

**장점**:
* 최종 이미지 크기 감소
* 빌드 도구가 최종 이미지에 포함되지 않음
* 보안 향상 (공격 표면 감소)

**예시**:
```dockerfile
# Build stage
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY --from=builder /app/dist ./dist
CMD ["node", "dist/index.js"]
```

### 2. 이미지 최적화 전략

**레이어 최소화**:
* 여러 RUN 명령어를 하나로 결합
* 불필요한 파일 제거

**베이스 이미지 선택**:
* Alpine Linux: 가장 작은 크기
* Distroless: 최소한의 런타임만 포함
* Official images: 검증된 이미지 사용

**.dockerignore 사용**:
* 빌드 컨텍스트에 불필요한 파일 제외
* 빌드 시간 단축

### 3. 보안 모범 사례

**비 root 사용자**:
```dockerfile
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001
USER nextjs
```

**시크릿 관리**:
* 환경 변수로 민감한 정보 전달
* Docker Secrets 사용 (Swarm 모드)
* 외부 시크릿 관리 도구 통합

**이미지 스캔**:
* `docker scout` 또는 `trivy`로 취약점 검사
* 정기적으로 베이스 이미지 업데이트

### 4. 컨테이너 오케스트레이션과의 관계

**Docker Swarm**:
* Docker 네이티브 오케스트레이션
* 간단한 설정
* 소규모 환경에 적합

**Kubernetes**:
* 가장 인기 있는 오케스트레이션 플랫폼
* 복잡하지만 강력한 기능
* 대규모 환경에 적합

**선택 기준**:
* 단순성 vs 기능
* 규모 및 복잡도
* 운영 팀의 경험

### 5. 성능 최적화

**리소스 제한**:
```bash
docker run --memory="512m" --cpus="1.0" my-app
```

**컨테이너 프로파일링**:
* `docker stats`로 리소스 사용량 모니터링
* 성능 병목 지점 파악

**캐싱 전략**:
* 레이어 캐싱 활용
* 빌드 캐시 유지

### 6. 컨테이너 간 통신

**Docker Compose 네트워크**:
* 자동으로 네트워크 생성
* 서비스 이름으로 DNS 해석

**로드 밸런싱**:
* 여러 컨테이너 인스턴스 실행
* Docker Compose scale 명령어

### 7. 로깅 및 모니터링

**로깅 드라이버**:
* JSON File (기본)
* Syslog
* Fluentd
* AWS CloudWatch Logs

**로그 관리**:
* 로그 회전 설정
* 로그 레벨 조정
* 중앙 집중식 로그 수집

### 8. 데이터 관리

**볼륨 백업**:
* 볼륨 데이터 정기 백업
* 스냅샷 활용

**데이터 마이그레이션**:
* 컨테이너 간 데이터 이동
* 호스트와 컨테이너 간 동기화

### 9. CI/CD 통합

**빌드 최적화**:
* Docker BuildKit 사용
* 빌드 캐시 활용
* 병렬 빌드

**이미지 배포**:
* 레지스트리에 푸시
* 태그 관리 전략
* 롤백 전략

### 10. 트러블슈팅

**컨테이너 디버깅**:
* `docker exec`로 컨테이너 내부 접근
* `docker logs`로 로그 확인
* `docker inspect`로 상세 정보 확인

**일반적인 문제**:
* 포트 충돌
* 볼륨 권한 문제
* 네트워크 연결 문제
* 리소스 부족

---

## 학습 체크리스트

### 초급
- [ ] Docker 설치 및 기본 명령어 사용
- [ ] Dockerfile 작성 및 이미지 빌드
- [ ] 컨테이너 실행 및 관리
- [ ] 포트 매핑 및 볼륨 마운트
- [ ] Docker Hub 이미지 사용

### 중급
- [ ] 멀티스테이지 빌드 구현
- [ ] Docker Compose로 멀티 컨테이너 관리
- [ ] 네트워크 구성 및 컨테이너 간 통신
- [ ] 이미지 최적화 (크기, 레이어)
- [ ] 환경 변수 및 시크릿 관리
- [ ] Dockerfile 모범 사례 적용

### 고급
- [ ] Docker Registry 구축 및 운영
- [ ] 보안 모범 사례 적용
- [ ] 성능 최적화 및 리소스 관리
- [ ] 로깅 및 모니터링 설정
- [ ] CI/CD 파이프라인 통합
- [ ] Docker Swarm 또는 Kubernetes와 통합
- [ ] 프로덕션 배포 전략 수립

---

## 참고

* Docker 공식 문서: https://docs.docker.com/
* Docker Hub: https://hub.docker.com/
* Best practices: https://docs.docker.com/develop/dev-best-practices/
* 컨테이너는 일시적(ephemeral)이므로 중요한 데이터는 볼륨으로 관리
* 프로덕션에서는 이미지 태그를 명시적으로 관리 (latest 피하기)
* Docker 명령어 참조: https://docs.docker.com/reference/

