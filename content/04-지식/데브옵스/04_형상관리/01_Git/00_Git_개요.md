---
publish: true
tags: [지식, 데브옵스]
---

# Git 개요

---

## Git이란?

> **Git**은 분산 버전 관리 시스템(Distributed Version Control System, DVCS)으로, 소프트웨어 개발 프로젝트의 소스 코드 변경사항을 추적하고 관리하는 도구입니다. 2005년 리누스 토르발스(Linus Torvalds)가 리눅스 커널 개발을 위해 만들었습니다.

---

## Git의 특징

### 1. 분산 버전 관리
* 중앙 서버 없이도 작업 가능
* 각 개발자가 전체 히스토리 보유
* 오프라인 작업 가능

### 2. 빠른 성능
* 로컬 저장소에서 작업
* 네트워크 의존성 최소화
* 효율적인 데이터 구조

### 3. 브랜치 및 병합
* 가벼운 브랜치 생성
* 빠른 병합 작업
* 비선형 개발 지원

### 4. 무결성 보장
* SHA-1 해시를 통한 데이터 무결성
* 변경사항 추적 불가능
* 데이터 손실 방지

### 5. 오픈소스
* 무료 사용 가능
* 활발한 커뮤니티
* 다양한 플랫폼 지원

---

## Git의 기본 개념

### 1. 저장소 (Repository)
**로컬 저장소 (Local Repository)**
* 개발자의 컴퓨터에 저장된 저장소
* 전체 히스토리 포함

**원격 저장소 (Remote Repository)**
* GitHub, GitLab, Bitbucket 등
* 팀원들과 공유하는 저장소

### 2. 커밋 (Commit)
* 변경사항을 저장하는 단위
* 고유한 해시 값 부여
* 메시지와 함께 저장

### 3. 브랜치 (Branch)
* 독립적인 개발 라인
* 기능별, 버전별 분기
* 병합을 통한 통합

### 4. 병합 (Merge)
* 브랜치를 통합하는 작업
* 자동 병합 또는 충돌 해결

### 5. 작업 디렉토리 (Working Directory)
* 현재 작업 중인 파일들
* 수정된 파일의 상태

### 6. 스테이징 영역 (Staging Area)
* 커밋 전 임시 저장 공간
* `git add`로 추가
* 선택적 커밋 가능

---

## Git의 3가지 영역

### 1. Working Directory (작업 디렉토리)
* 실제 파일이 있는 곳
* 수정 중인 파일들

### 2. Staging Area (스테이징 영역)
* 커밋할 파일들을 준비하는 곳
* `git add`로 추가
* Index라고도 불림

### 3. Git Repository (저장소)
* 커밋된 파일들이 저장되는 곳
* `.git` 디렉토리
* 히스토리 보관

---

## Git 기본 명령어

### 저장소 초기화 및 클론
```bash
# 새 저장소 초기화
git init

# 원격 저장소 클론
git clone <repository-url>

# 원격 저장소 URL 확인
git remote -v
```

### 상태 확인
```bash
# 현재 상태 확인
git status

# 변경사항 확인
git diff

# 스테이징된 변경사항 확인
git diff --staged
```

### 파일 추가 및 커밋
```bash
# 파일을 스테이징 영역에 추가
git add <file>
git add .  # 모든 파일 추가

# 커밋 생성
git commit -m "커밋 메시지"

# 파일 추가와 커밋을 한 번에
git commit -am "커밋 메시지"
```

### 브랜치 관리
```bash
# 브랜치 목록 확인
git branch

# 새 브랜치 생성
git branch <branch-name>

# 브랜치 전환
git checkout <branch-name>
git switch <branch-name>  # Git 2.23+

# 브랜치 생성 및 전환
git checkout -b <branch-name>
git switch -c <branch-name>

# 브랜치 삭제
git branch -d <branch-name>  # 안전한 삭제
git branch -D <branch-name>  # 강제 삭제
```

### 병합
```bash
# 현재 브랜치에 다른 브랜치 병합
git merge <branch-name>

# 병합 충돌 확인
git status

# 충돌 해결 후
git add <resolved-files>
git commit
```

### 원격 저장소 작업
```bash
# 원격 저장소 추가
git remote add <name> <url>

# 원격 저장소에서 가져오기
git fetch <remote>

# 원격 저장소에서 가져오고 병합
git pull <remote> <branch>

# 원격 저장소에 푸시
git push <remote> <branch>

# 원격 브랜치 추적 설정
git push -u <remote> <branch>
```

### 히스토리 확인
```bash
# 커밋 히스토리 확인
git log

# 간단한 로그
git log --oneline

# 그래프 형태로 확인
git log --graph --oneline --all

# 특정 파일의 히스토리
git log <file>
```

### 되돌리기
```bash
# 마지막 커밋 수정 (메시지만 변경)
git commit --amend

# 파일을 이전 커밋 상태로 되돌리기
git checkout -- <file>

# 스테이징 영역에서 제거
git reset HEAD <file>

# 커밋 되돌리기 (히스토리 유지)
git revert <commit-hash>

# 커밋 되돌리기 (히스토리 변경)
git reset --soft HEAD~1  # 커밋만 취소
git reset --mixed HEAD~1  # 커밋과 스테이징 취소
git reset --hard HEAD~1  # 모든 변경사항 취소
```

---

## Git 워크플로우

### 1. 기본 워크플로우
```
1. git pull (최신 코드 가져오기)
2. 파일 수정
3. git add (변경사항 스테이징)
4. git commit (커밋 생성)
5. git push (원격 저장소에 업로드)
```

### 2. 브랜치 워크플로우
```
1. git checkout -b feature/new-feature
2. 파일 수정 및 커밋
3. git push -u origin feature/new-feature
4. Pull Request 생성
5. 코드 리뷰 후 병합
```

### 3. Git Flow
```
- main: 프로덕션 코드
- develop: 개발 브랜치
- feature/*: 기능 개발
- release/*: 릴리스 준비
- hotfix/*: 긴급 수정
```

---

## Git 고급 기능

### 1. Stash (임시 저장)
```bash
# 현재 변경사항 임시 저장
git stash

# 스테이시 목록 확인
git stash list

# 스테이시 적용
git stash apply
git stash pop  # 적용 후 삭제

# 스테이시 삭제
git stash drop
```

### 2. Rebase (재배치)
```bash
# 현재 브랜치를 다른 브랜치 위에 재배치
git rebase <branch>

# 인터랙티브 리베이스
git rebase -i HEAD~3
```

### 3. Cherry-pick
```bash
# 특정 커밋만 가져오기
git cherry-pick <commit-hash>
```

### 4. Tag (태그)
```bash
# 태그 생성
git tag <tag-name>
git tag -a <tag-name> -m "태그 메시지"

# 태그 목록
git tag

# 태그 푸시
git push origin <tag-name>
git push --tags  # 모든 태그
```

---

## .gitignore 파일

### 자주 사용되는 패턴
```
# 의존성
node_modules/
vendor/

# 빌드 결과물
dist/
build/
*.class

# 환경 변수
.env
.env.local

# IDE 설정
.idea/
.vscode/
*.swp

# OS 파일
.DS_Store
Thumbs.db

# 로그 파일
*.log
logs/
```

---

## Git 모범 사례

### 1. 커밋 메시지
* 명확하고 의미 있는 메시지 작성
* 첫 줄은 50자 이내
* 본문은 72자마다 줄바꿈
* 현재형 동사 사용 ("Add", "Fix", "Update")

### 2. 커밋 단위
* 논리적으로 관련된 변경사항만 함께 커밋
* 작고 자주 커밋
* 하나의 커밋에 하나의 목적

### 3. 브랜치 전략
* 명확한 브랜치 네이밍
* 기능별 브랜치 사용
* 정기적인 병합

### 4. 코드 리뷰
* Pull Request 활용
* 팀원과 코드 공유
* 피드백 수용

### 5. 백업
* 정기적으로 원격 저장소에 푸시
* 중요한 브랜치 보호
* 태그를 사용한 릴리스 관리

---

## Git 호스팅 서비스

### GitHub
* 가장 인기 있는 Git 호스팅 서비스
* 무료 공개 저장소
* Pull Request, Issues, Actions 등 기능

### GitLab
* 오픈소스 기반
* 자체 호스팅 가능
* CI/CD 통합

### Bitbucket
* Atlassian 제품군과 통합
* 무료 비공개 저장소
* Jira 연동

### Azure DevOps
* 마이크로소프트의 개발 도구
* Git 저장소 및 CI/CD
* 프로젝트 관리 기능

---

## Git의 장점

* **분산 시스템**: 중앙 서버 장애에 강함
* **빠른 성능**: 로컬 작업으로 빠른 속도
* **브랜치 지원**: 쉬운 브랜치 생성 및 병합
* **무결성**: 데이터 손실 방지
* **오픈소스**: 무료 사용 가능
* **광범위한 지원**: 대부분의 IDE와 통합

---

## Git의 단점

* **학습 곡선**: 초보자에게 복잡할 수 있음
* **대용량 파일**: 바이너리 파일 관리에 부적합
* **충돌 해결**: 병합 충돌 해결이 복잡할 수 있음

---

## 참고

* Git은 현대 소프트웨어 개발의 필수 도구입니다
* 정기적인 커밋과 푸시를 습관화하세요
* 의미 있는 커밋 메시지를 작성하세요
* 브랜치 전략을 팀과 함께 수립하세요
* Git의 고급 기능은 필요에 따라 학습하세요
