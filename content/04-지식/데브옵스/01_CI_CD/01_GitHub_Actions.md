---
publish: true
tags: [지식, 데브옵스]
---

# GitHub Actions

---

## 개요

**GitHub Actions**는 GitHub 저장소에 통합된 CI/CD 자동화 플랫폼으로, YAML 파일로 워크플로우를 정의하여 코드 빌드, 테스트, 배포를 자동화합니다.

### 특징

* **GitHub 통합**: 저장소와 완전히 통합된 CI/CD 환경
* **YAML 기반**: 코드로 워크플로우 관리 (Infrastructure as Code)
* **커뮤니티 Actions**: 수천 개의 재사용 가능한 Actions
* **다양한 러너**: Linux, Windows, macOS 지원
* **무료 플랜**: 퍼블릭 저장소 무제한, 프라이빗 저장소 월 2,000분

---

## GitHub Actions란?

> **GitHub Actions**는 GitHub 저장소에서 바로 CI/CD 파이프라인을 구축할 수 있는 통합 자동화 플랫폼입니다.

### 작동 원리

1. **워크플로우 파일**: `.github/workflows/*.yml` 파일에 작업 정의
2. **이벤트 트리거**: Push, PR, Schedule 등 이벤트 발생 시 자동 실행
3. **러너**: GitHub 호스팅 머신 또는 셀프호스팅 러너에서 작업 실행
4. **결과**: 성공/실패 상태가 저장소에 표시됨

---

## 핵심 요소와 개념

### 1. Workflow (워크플로우)

워크플로우는 자동화된 프로세스를 정의하는 YAML 파일입니다.

**핵심 개념**:
* 하나의 저장소에 여러 워크플로우 파일 가능
* 각 워크플로우는 독립적으로 실행
* 워크플로우는 하나 이상의 Job으로 구성

**작동 방식**:
```yaml
name: Workflow Name  # 워크플로우 식별자
on:                  # 트리거 조건
jobs:                # 실행할 작업들
```

### 2. Events (이벤트/트리거)

워크플로우를 실행시키는 조건입니다.

**주요 이벤트**:

* **push**: 특정 브랜치나 태그에 코드 푸시 시
* **pull_request**: PR 생성, 업데이트, 병합 시
* **workflow_dispatch**: 수동 실행 (GitHub UI 또는 API)
* **schedule**: Cron 표현식 기반 스케줄 실행
* **release**: 릴리즈 생성, 편집, 삭제 시
* **workflow_call**: 다른 워크플로우에서 호출
* **repository_dispatch**: 외부 이벤트 수신

**심화 학습**:
* 이벤트 필터링: paths, paths-ignore로 특정 파일 변경 시만 실행
* 이벤트 페이로드: `github.event` 컨텍스트로 이벤트 정보 접근
* 조건부 실행: `if` 문으로 조건부 실행 가능

### 3. Jobs (작업)

워크플로우의 실행 단위입니다. 여러 Job을 병렬 또는 순차 실행할 수 있습니다.

**핵심 개념**:
* Job은 독립적인 환경에서 실행됨 (새로운 러너 인스턴스)
* 기본적으로 병렬 실행
* `needs` 키워드로 의존성 정의 가능
* `runs-on`으로 실행 환경 지정

**Job 간 데이터 공유**:
* `actions/upload-artifact`, `actions/download-artifact` 사용
* 환경 변수는 Job 간 공유되지 않음

**심화 학습**:
* Job 전략: Matrix, Fail-fast 설정
* Job 간 조건부 실행: `if: needs.previous-job.result == 'success'`
* Job 타임아웃: `timeout-minutes` 설정

### 4. Runners (러너)

워크플로우를 실행하는 머신입니다.

**종류**:
* **GitHub-hosted runners**: GitHub에서 제공하는 무료 러너
  * Ubuntu, Windows, macOS
  * 사전 설치된 도구들 포함
  * 사용량 제한 있음
* **Self-hosted runners**: 자체 인프라에서 실행
  * 무제한 사용
  * 특수한 하드웨어/소프트웨어 요구사항 충족 가능
  * 보안 관리 필요

**러너 선택 전략**:
* 기본: GitHub-hosted runners 사용
* 대용량/보안 요구: Self-hosted runners 고려
* 비용 최적화: Self-hosted runners로 전환

### 5. Steps (단계)

Job 내의 개별 실행 단위입니다.

**핵심 개념**:
* Steps는 순차적으로 실행됨
* 한 Step 실패 시 이후 Steps는 실행되지 않음 (조건부 실행 제외)
* 각 Step은 독립적인 프로세스

**Step 구성 요소**:
* `name`: Step 이름 (선택사항)
* `uses`: Action 사용
* `run`: 셸 명령어 실행
* `with`: Action에 전달할 입력값
* `env`: 환경 변수
* `if`: 조건부 실행
* `continue-on-error`: 실패해도 계속 진행

### 6. Actions (액션)

재사용 가능한 작업 단위입니다.

**Action 종류**:
* **Composite Actions**: 여러 단계를 하나로 묶음
* **JavaScript Actions**: Node.js로 작성
* **Docker Actions**: Docker 컨테이너로 실행

**Action 사용**:
* Marketplace에서 검색: https://github.com/marketplace?type=actions
* 버전 지정: `@v3`, `@main`, `@commit-hash`
* 보안: 액션 버전을 고정하여 안정성 확보

**커스텀 Action 생성**:
* `.github/actions/my-action/action.yml` 파일 생성
* 다른 저장소에서 재사용 가능
* 비즈니스 로직 재사용 시 유용

---

## 주요 기능

* **CI (Continuous Integration)**: 코드 빌드 및 테스트 자동화
* **CD (Continuous Deployment)**: 자동 배포
* **워크플로우 자동화**: 이슈 관리, 코드 리뷰 등
* **다양한 플랫폼 지원**: Linux, Windows, macOS

---

## 기본 구조

### Workflow 파일 위치

```
.github/workflows/
  └── deploy.yml
```

### Workflow 파일 예시

```yaml
name: Deploy Application

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: Install dependencies
      run: npm install
    
    - name: Run tests
      run: npm test
    
    - name: Build
      run: npm run build
```

---

## 주요 개념

### Events (트리거)

* `push`: 코드 푸시 시
* `pull_request`: PR 생성/업데이트 시
* `schedule`: 스케줄 기반 (cron 형식)
* `workflow_dispatch`: 수동 실행
* `release`: 릴리즈 생성 시

### Jobs

* 독립적인 작업 단위
* 병렬 또는 순차 실행 가능
* 다른 머신에서 실행 가능

### Steps

* Job 내의 개별 작업
* 순차적으로 실행
* Actions 또는 명령어 실행

### Actions

* 재사용 가능한 작업 단위
* GitHub Marketplace에서 제공
* 커스텀 Actions 생성 가능

---

## 주요 Actions

| Action | 용도 |
| ------ | ---- |
| `actions/checkout` | 저장소 체크아웃 |
| `actions/setup-node` | Node.js 환경 설정 |
| `actions/setup-python` | Python 환경 설정 |
| `actions/setup-java` | Java 환경 설정 |
| `actions/cache` | 의존성 캐싱 |
| `actions/upload-artifact` | 빌드 아티팩트 업로드 |
| `actions/download-artifact` | 빌드 아티팩트 다운로드 |

---

## Secrets 관리

### Secrets 설정

1. 저장소 → Settings → Secrets and variables → Actions
2. New repository secret 클릭
3. 이름과 값을 입력

### Secrets 사용

```yaml
- name: Deploy
  env:
    API_KEY: ${{ secrets.API_KEY }}
    DATABASE_URL: ${{ secrets.DATABASE_URL }}
  run: |
    echo "Deploying with secret..."
```

---

## 실전 예시

### Node.js 애플리케이션 배포

```yaml
name: Node.js CI/CD

on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm test

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /var/www/app
            git pull
            npm install --production
            pm2 restart app
```

### Docker 이미지 빌드 및 푸시

```yaml
name: Docker Build and Push

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      
      - name: Build and push
        uses: docker/build-push-action@v4
        with:
          context: .
          push: true
          tags: user/app:latest
```

---

## Matrix 전략

여러 버전/OS에서 동시 테스트:

```yaml
strategy:
  matrix:
    node-version: [16, 18, 20]
    os: [ubuntu-latest, windows-latest, macos-latest]
```

---

## Workflow 상태 배지

```markdown
![CI](https://github.com/username/repo/workflows/CI/badge.svg)
```

---

## 가격 정책

* **무료 플랜**: 퍼블릭 저장소 무제한, 프라이빗 저장소 월 2,000분
* **유료 플랜**: 추가 시간 구매 가능

---

---

## 심화 학습 포인트

### 1. Context (컨텍스트)

워크플로우 실행 중 접근 가능한 정보입니다.

**주요 컨텍스트**:
* `github`: 워크플로우 실행 정보, 이벤트 정보
* `env`: 환경 변수
* `job`: 현재 Job 정보
* `steps`: 이전 Steps 출력값
* `runner`: 러너 정보
* `secrets`: 시크릿 값

**사용 예시**:
```yaml
- name: Get branch name
  run: echo "Branch: ${{ github.ref_name }}"
  
- name: Check event
  run: echo "Event: ${{ github.event_name }}"
```

### 2. Matrix 전략

여러 조합에서 동시에 테스트할 때 사용합니다.

**핵심 개념**:
* 하나의 Job을 여러 조합으로 실행
* `strategy.matrix`로 변수 정의
* 자동으로 조합 생성

**고급 활용**:
```yaml
strategy:
  matrix:
    node-version: [16, 18, 20]
    os: [ubuntu-latest, windows-latest]
    include:
      - node-version: 21
        os: ubuntu-latest
        experimental: true
    exclude:
      - node-version: 16
        os: windows-latest
```

### 3. 캐싱 전략

의존성 다운로드 시간을 단축하기 위한 캐싱입니다.

**`actions/cache` 사용**:
```yaml
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

**캐시 키 전략**:
* Lock 파일 해시 기반 키: 정확한 재사용
* Fallback 키: 부분 일치 캐시 활용
* 캐시 만료: GitHub에서 자동 관리

### 4. Artifacts 관리

빌드 결과물을 저장하고 다운로드합니다.

**핵심 개념**:
* 업로드: `actions/upload-artifact`
* 다운로드: `actions/download-artifact`
* 보존 기간: 90일 (조정 가능)
* 압축: 자동으로 압축됨

**사용 시나리오**:
* 빌드 산출물 저장
* Job 간 빌드 결과 공유
* 배포 전 산출물 검증

### 5. 환경 변수와 Secrets

**환경 변수**:
* Workflow, Job, Step 레벨에서 정의 가능
* 상위 레벨에서 하위 레벨로 상속
* 동일 이름 시 하위 레벨이 우선

**Secrets 관리**:
* 저장소 Settings에서 설정
* 조직/저장소 레벨 Secrets
* 환경별 Secrets 분리 가능
* 마스킹: 로그에 표시되지 않음

### 6. 조건부 실행과 흐름 제어

**`if` 조건문**:
```yaml
- name: Deploy
  if: github.ref == 'refs/heads/main' && github.event_name == 'push'
  run: ./deploy.sh
```

**주요 조건들**:
* `success()`: 이전 단계 성공
* `failure()`: 이전 단계 실패
* `always()`: 항상 실행
* `cancelled()`: 취소된 경우

### 7. 워크플로우 재사용

**Reusable Workflows**:
* 여러 저장소에서 공통 워크플로우 사용
* `workflow_call` 이벤트 사용
* 입력값과 시크릿 전달 가능

**예시**:
```yaml
# .github/workflows/reusable.yml
on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string

# 다른 워크플로우에서 사용
jobs:
  call-workflow:
    uses: ./.github/workflows/reusable.yml
    with:
      node-version: '18'
```

### 8. 보안 모범 사례

* **Action 버전 고정**: `@v3` 대신 `@v3.1.2` 사용
* **시크릿 검증**: 민감한 정보는 시크릿으로 관리
* **의존성 검토**: Action의 의존성 확인
* **최소 권한 원칙**: 필요한 권한만 부여
* **코드 스캔**: Dependabot으로 취약점 검사

### 9. 성능 최적화

* **의존성 캐싱**: npm, pip 등 패키지 캐싱
* **병렬 실행**: 독립적인 Job 병렬 처리
* **조건부 실행**: 불필요한 Job 스킵
* **멀티스테이지 빌드**: Docker 이미지 크기 최적화
* **Artifact 업로드 최적화**: 필요한 파일만 업로드

### 10. 디버깅 전략

* **워크플로우 로그**: Actions 탭에서 상세 로그 확인
* **Step 단계 디버깅**: 중간 결과 출력
* **Runner SSH 디버깅**: Self-hosted runner에서 SSH 접속
* **`tmate` Action**: 대화형 디버깅 세션
* **재실행**: 실패한 워크플로우 재실행

---

## 학습 체크리스트

### 초급
- [ ] 기본 워크플로우 작성 및 실행
- [ ] 이벤트 트리거 이해
- [ ] 기본 Actions 사용 (checkout, setup-node 등)
- [ ] 환경 변수와 Secrets 사용
- [ ] Job과 Step 개념 이해

### 중급
- [ ] Matrix 전략 활용
- [ ] 캐싱 구현
- [ ] Artifacts 업로드/다운로드
- [ ] 조건부 실행 구현
- [ ] 여러 Job 간 의존성 관리
- [ ] Self-hosted runner 설정

### 고급
- [ ] 커스텀 Action 작성
- [ ] Reusable Workflows 구성
- [ ] 환경별 배포 전략 구현
- [ ] 보안 모범 사례 적용
- [ ] 워크플로우 성능 최적화
- [ ] 복잡한 CI/CD 파이프라인 설계

---

## 참고

* GitHub Actions 공식 문서: https://docs.github.com/actions
* Marketplace: https://github.com/marketplace?type=actions
* YAML 문법 숙지 필요
* Workflow 로그에서 디버깅 가능
* Context API 참조: https://docs.github.com/en/actions/learn-github-actions/contexts

