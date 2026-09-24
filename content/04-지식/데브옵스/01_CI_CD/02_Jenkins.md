---
publish: true
tags: [지식, 데브옵스]
---

# Jenkins

---

## 개요

**Jenkins**는 Java로 작성된 오픈소스 자동화 서버로, CI/CD 파이프라인을 구축하고 실행하는 데 사용됩니다. 플러그인 기반 아키텍처로 다양한 도구와 통합이 가능합니다.

### 특징

* **오픈소스**: 무료로 사용 가능
* **플러그인 생태계**: 1,800개 이상의 플러그인
* **분산 빌드**: Master-Agent 구조로 확장 가능
* **Pipeline as Code**: Jenkinsfile로 파이프라인 버전 관리
* **자체 호스팅**: 완전한 제어 가능

### Jenkins vs 다른 도구

* **GitHub Actions**: GitHub 통합 필요 시 적합
* **GitLab CI**: GitLab 사용 시 통합 환경 제공
* **Jenkins**: 온프레미스, 다양한 소스 제어 시스템, 복잡한 커스터마이징 필요 시

---

## Jenkins란?

> **Jenkins**는 Java 기반의 오픈소스 자동화 서버로, CI/CD 파이프라인을 구축하고 관리하는 데 사용됩니다.

### 작동 원리

1. **Master Node**: 워크플로우 스케줄링, 파이프라인 관리
2. **Agent/Node**: 실제 빌드 작업 수행
3. **Pipeline**: 빌드 단계를 정의한 워크플로우
4. **Plugins**: 기능 확장 및 도구 통합
5. **Artifacts**: 빌드 결과물 저장 및 관리

---

## 핵심 요소와 개념

### 1. Jenkins 아키텍처

**Master-Node 구조**:

* **Jenkins Master (Controller)**:
  * 웹 UI 제공
  * 파이프라인 스케줄링 및 관리
  * 빌드 큐 관리
  * 플러그인 관리
  * 설정 저장 (JENKINS_HOME)

* **Jenkins Agent/Node**:
  * 실제 빌드 작업 실행
  * 여러 노드에 작업 분산 가능
  * SSH, JNLP 등으로 연결
  * Windows, Linux, macOS 지원

**심화 학습**:
* Master 부하 분산: Master가 많아지면 성능 저하
* Agent 종류: Permanent Agent, Cloud Agent (Spot instances)
* 노드 라벨링: 특정 작업을 특정 노드에서 실행

### 2. Pipeline (파이프라인)

파이프라인은 빌드 프로세스를 정의하는 코드입니다.

**Pipeline 종류**:

* **Declarative Pipeline** (권장):
  * 간단하고 구조화된 문법
  * YAML과 유사한 가독성
  * 오류 검증 내장

* **Scripted Pipeline**:
  * Groovy 스크립트 기반
  * 더 유연하지만 복잡함
  * 복잡한 로직 구현 가능

**Pipeline 구조**:
```groovy
pipeline {
    agent any           // 실행할 노드
    options { }         // 파이프라인 옵션
    environment { }     // 환경 변수
    triggers { }        // 트리거
    stages { }          // 실행 단계
    post { }            // 후처리
}
```

**심화 학습**:
* Parallel 실행: 여러 Stage 병렬 실행
* Matrix: 여러 조합에서 동시 빌드
* Shared Libraries: 재사용 가능한 Pipeline 코드

### 3. Stages와 Steps

**Stage**: 파이프라인의 논리적 구분 (예: Build, Test, Deploy)

**Step**: 실제 실행되는 명령어나 작업

**핵심 개념**:
* Stage는 순차적으로 실행 (병렬 설정 시 제외)
* Step 실패 시 해당 Stage 실패
* `post` 섹션에서 실패 처리

### 4. Jenkinsfile

Jenkinsfile은 파이프라인을 코드로 정의한 파일입니다.

**위치**:
* 저장소 루트: `Jenkinsfile`
* 다른 경로: `Jenkinsfile.prod` 등

**버전 관리**:
* Git과 함께 버전 관리
* 코드 리뷰 가능
* 변경 이력 추적

**심화 학습**:
* 환경별 Jenkinsfile: `Jenkinsfile.dev`, `Jenkinsfile.prod`
* Shared Libraries 활용
* 파라미터화된 파이프라인

### 5. Plugins (플러그인)

Jenkins의 핵심 기능은 플러그인으로 확장됩니다.

**플러그인 설치**:
* UI: Manage Jenkins → Manage Plugins
* CLI: Jenkins CLI 사용
* 파일 시스템: `.jpi` 파일 직접 설치

**핵심 플러그인**:
* **Pipeline Plugin**: Pipeline 기능 제공
* **Git Plugin**: Git 통합
* **Docker Pipeline**: Docker 지원
* **Credentials Binding**: 시크릿 관리
* **Blue Ocean**: 모던 UI (deprecated 되었지만 여전히 사용)

**플러그인 관리**:
* 버전 고정: 특정 버전 사용 권장
* 업데이트 주기: 정기적으로 업데이트 확인
* 의존성 관리: 플러그인 간 의존성 주의

### 6. Credentials (자격 증명)

민감한 정보를 안전하게 관리합니다.

**Credentials 종류**:
* Username with password
* SSH Username with private key
* Secret text
* Secret file
* Certificate

**Credentials 스코프**:
* Global: 모든 Job에서 사용
* System: Jenkins 시스템 레벨
* User: 특정 사용자만 사용

**심화 학습**:
* Credentials Binding Plugin: 파이프라인에서 안전하게 사용
* HashiCorp Vault 통합: 외부 시크릿 관리
* AWS Secrets Manager 통합

### 7. Artifacts (빌드 결과물)

빌드에서 생성된 파일을 저장하고 관리합니다.

**Artifacts 저장**:
* `archiveArtifacts` Step 사용
* 빌드 이력에 저장
* 다운로드 가능

**심화 학습**:
* Artifactory/Nexus 통합: 중앙 저장소 사용
* Artifact 버전 관리
* 빌드 간 Artifact 공유

### 8. Triggers (트리거)

파이프라인을 자동으로 실행시키는 조건입니다.

**트리거 종류**:
* **Poll SCM**: 주기적으로 저장소 확인
* **GitHub/GitLab Webhook**: Push/PR 이벤트
* **Timer**: Cron 표현식
* **Upstream**: 다른 Job 완료 시

**Webhook 설정**:
* GitHub: 저장소 Settings → Webhooks
* Jenkins: Jenkins URL + `/github-webhook/`

### 9. Shared Libraries

여러 파이프라인에서 재사용 가능한 코드입니다.

**구조**:
```
vars/          # 전역 변수/함수
src/           # 클래스
resources/     # 리소스 파일
```

**사용**:
```groovy
@Library('my-shared-library') _
pipeline {
    ...
}
```

**심화 학습**:
* 라이브러리 버전 관리
* 테스트 가능한 라이브러리 작성
* 문서화

---

## 주요 기능

* **Continuous Integration**: 코드 빌드, 테스트 자동화
* **Continuous Deployment**: 자동 배포
* **플러그인 시스템**: 다양한 도구와 통합
* **분산 빌드**: 여러 노드에서 병렬 처리
* **파이프라인 as Code**: Jenkinsfile로 파이프라인 정의

---

## Jenkins 설치

### Java 설치 (필수)

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install openjdk-11-jdk

# CentOS/RHEL
sudo yum install java-11-openjdk-devel
```

### Jenkins 설치

```bash
# Ubuntu/Debian
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

# Jenkins 시작
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

### 초기 설정

1. 브라우저에서 `http://your-server:8080` 접속
2. 초기 비밀번호 확인: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
3. 플러그인 설치 (권장 플러그인 또는 직접 선택)
4. 관리자 계정 생성

---

## Jenkins 구조

### 주요 컴포넌트

| 컴포넌트 | 설명 |
| -------- | ---- |
| **Master** | Jenkins 서버 중앙 관리자 |
| **Agent/Node** | 실제 빌드를 수행하는 워커 노드 |
| **Job** | 빌드/배포 작업 단위 |
| **Pipeline** | 여러 단계를 연결한 워크플로우 |
| **Plugin** | 기능 확장을 위한 플러그인 |

---

## Job 생성 방법

### 1. Freestyle Project

* 웹 UI에서 간단하게 설정
* 간단한 빌드에 적합

### 2. Pipeline (Jenkinsfile)

* 코드로 파이프라인 정의
* 버전 관리 가능
* 추천 방식

### Jenkinsfile 예시

```groovy
pipeline {
    agent any
    
    environment {
        NODE_VERSION = '18'
        APP_NAME = 'my-app'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh 'npm run deploy'
            }
        }
    }
    
    post {
        always {
            junit 'test-results.xml'
        }
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
```

---

## 주요 플러그인

| 플러그인 | 용도 |
| -------- | ---- |
| **Git Plugin** | Git 저장소 연동 |
| **Docker Pipeline** | Docker 통합 |
| **Kubernetes Plugin** | Kubernetes 배포 |
| **SSH Plugin** | 원격 서버 접속 및 배포 |
| **Email Extension** | 이메일 알림 |
| **Blue Ocean** | 모던한 UI |
| **Pipeline Plugin** | Pipeline 지원 |
| **Credentials Binding** | 비밀번호 관리 |

---

## Credentials 관리

### Credentials 종류

* Username with password
* SSH Username with private key
* Secret text
* Secret file
* Certificate

### Credentials 설정

1. Jenkins → Manage Jenkins → Manage Credentials
2. Add Credentials
3. 종류 선택 후 정보 입력
4. 파이프라인에서 `credentials('credential-id')` 사용

---

## 배포 예시

### SSH를 통한 배포

```groovy
stage('Deploy') {
    steps {
        sshagent(['ssh-credentials-id']) {
            sh '''
                ssh user@server << EOF
                    cd /var/www/app
                    git pull
                    npm install --production
                    pm2 restart app
                EOF
            '''
        }
    }
}
```

### Docker 이미지 빌드 및 배포

```groovy
stage('Docker Build') {
    steps {
        script {
            def image = docker.build("myapp:${env.BUILD_NUMBER}")
            image.push()
        }
    }
}

stage('Docker Deploy') {
    steps {
        sh 'docker-compose down'
        sh 'docker-compose up -d'
    }
}
```

### Kubernetes 배포

```groovy
stage('Deploy to Kubernetes') {
    steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
        sh 'kubectl rollout status deployment/myapp'
    }
}
```

---

## Blue Ocean

모던한 UI를 제공하는 플러그인:

```bash
# 설치
Manage Jenkins → Manage Plugins → Available
Blue Ocean 검색 후 설치
```

* 시각적 파이프라인 편집기
* 실시간 로그 뷰어
* 브랜치/PR 자동 인식

---

## Jenkins vs GitHub Actions

| 기능 | Jenkins | GitHub Actions |
| ---- | ------- | -------------- |
| **설치** | 서버 설치 필요 | GitHub 통합 |
| **비용** | 무료 (서버 비용 별도) | 무료 플랜 제공 |
| **확장성** | 플러그인으로 확장 | Actions로 확장 |
| **학습 곡선** | 높음 | 낮음 |
| **자체 서버** | 필요 | 불필요 |

---

## Jenkins 관리

### 로그 확인

```bash
# Jenkins 로그
sudo tail -f /var/log/jenkins/jenkins.log

# 특정 Job 로그
/var/lib/jenkins/jobs/{job-name}/builds/{build-number}/log
```

### 백업

```bash
# Jenkins 홈 디렉토리 백업
sudo tar -czf jenkins-backup.tar.gz /var/lib/jenkins
```

### 플러그인 업데이트

1. Manage Jenkins → Manage Plugins
2. Updates 탭에서 업데이트 가능한 플러그인 확인
3. 설치 후 Jenkins 재시작

---

---

## 심화 학습 포인트

### 1. Jenkins 성능 최적화

**Master 최적화**:
* JVM 힙 메모리 조정: `-Xmx` 옵션
* 빌드 큐 최적화: Executor 수 조정
* 디스크 I/O 최적화: SSD 사용 권장
* 플러그인 정리: 불필요한 플러그인 제거

**Agent 최적화**:
* 적절한 Agent 수 유지
* Cloud Agents로 동적 확장
* Agent 라벨링으로 작업 분산

### 2. 보안 모범 사례

* **RBAC (Role-Based Access Control)**:
  * Role Strategy Plugin 사용
  * 최소 권한 원칙 적용

* **Credentials 관리**:
  * 민감한 정보는 Credentials로 관리
  * Credentials 스코프 최소화

* **Jenkins 보안**:
  * 최신 버전 유지
  * HTTPS 사용
  * 네트워크 격리

### 3. Pipeline 고급 기법

**Parallel 실행**:
```groovy
stage('Tests') {
    parallel {
        stage('Unit Tests') { }
        stage('Integration Tests') { }
    }
}
```

**Matrix 빌드**:
```groovy
matrix {
    axes {
        axis {
            name 'NODE_VERSION'
            values '14', '16', '18'
        }
    }
    stages {
        stage('Test') { }
    }
}
```

**조건부 실행**:
```groovy
when {
    anyOf {
        branch 'main'
        branch 'develop'
    }
    not { branch 'feature/*' }
}
```

### 4. Jenkins와 Docker 통합

**Docker Agent**:
```groovy
agent {
    docker {
        image 'node:18'
        args '-v /tmp:/tmp'
    }
}
```

**Docker Compose**:
* Docker Compose Plugin 사용
* 멀티 컨테이너 환경 테스트

**Docker 이미지 빌드 및 푸시**:
* Docker Pipeline Plugin
* Registry 인증 관리

### 5. Jenkins와 Kubernetes 통합

**Kubernetes Plugin**:
* Pod Template로 Agent 동적 생성
* 리소스 효율적 활용
* 스케일링 자동화

**설정 예시**:
```groovy
agent {
    kubernetes {
        yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: node
    image: node:18
"""
    }
}
```

### 6. 모니터링 및 알림

**모니터링**:
* Prometheus Plugin: 메트릭 수집
* Grafana 대시보드 연동
* 빌드 성공/실패율 추적

**알림**:
* Email Extension Plugin
* Slack/Discord 통합
* 빌드 상태에 따른 알림

### 7. 백업 및 복구

**JENKINS_HOME 백업**:
* 전체 디렉토리 백업
* 설정, 플러그인, 빌드 이력 포함
* 정기적 백업 스크립트

**복구 전략**:
* 백업에서 복구
* 설정 마이그레이션
* 플러그인 재설치

### 8. 다중 마스터 구성

**Jenkins High Availability**:
* 여러 Master 구성 (복잡함)
* Active-Passive 또는 Active-Active
* 공유 스토리지 필요

**대안**:
* Agent 분산으로 충분한 경우가 많음
* Kubernetes에서 동적 스케일링

### 9. Jenkins X (Cloud Native Jenkins)

**Jenkins X 특징**:
* Kubernetes 네이티브
* GitOps 방식
* 자동 CI/CD 설정

**사용 시나리오**:
* Kubernetes 환경에서의 현대적 CI/CD
* GitOps 워크플로우 선호 시

---

## 학습 체크리스트

### 초급
- [ ] Jenkins 설치 및 기본 설정
- [ ] Freestyle Project 생성
- [ ] 기본 빌드 및 테스트 실행
- [ ] Git 저장소 연동
- [ ] Jenkinsfile 기본 작성

### 중급
- [ ] Declarative Pipeline 작성
- [ ] 여러 Stage 구성
- [ ] Credentials 사용
- [ ] Artifacts 저장 및 관리
- [ ] Agent/Node 추가 및 관리
- [ ] Webhook 설정

### 고급
- [ ] Scripted Pipeline 작성
- [ ] Shared Libraries 생성 및 사용
- [ ] Parallel 및 Matrix 빌드
- [ ] Docker/Kubernetes 통합
- [ ] 보안 설정 (RBAC)
- [ ] 모니터링 및 알림 구성
- [ ] 성능 최적화
- [ ] 백업 및 복구 전략

---

## 참고

* Jenkins 공식 문서: https://www.jenkins.io/doc/
* Jenkins 플러그인: https://plugins.jenkins.io/
* Jenkinsfile 문법: Declarative Pipeline 또는 Scripted Pipeline
* 자체 서버가 필요한 경우 Jenkins, GitHub 통합이 필요한 경우 GitHub Actions 고려
* Pipeline 문법 참조: https://www.jenkins.io/doc/book/pipeline/syntax/

