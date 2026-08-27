---
publish: true
tags: [지식, 데브옵스]
---

# Ansible 개요

---

## Ansible이란?

> **Ansible**은 Red Hat에서 개발한 오픈소스 자동화 플랫폼으로, 설정 관리, 애플리케이션 배포, 오케스트레이션을 수행하는 도구입니다. 에이전트리스(Agentless) 아키텍처를 사용하여 SSH를 통해 원격 시스템을 관리합니다.

---

## Ansible의 특징

### 1. 에이전트리스 (Agentless)
* 대상 서버에 에이전트 설치 불필요
* SSH만으로 원격 관리
* 간단한 설치 및 유지보수

### 2. 간단한 문법
* YAML 기반 플레이북
* 읽기 쉬운 구성 파일
* 학습 곡선이 낮음

### 1. 멱등성 (Idempotency)
* 여러 번 실행해도 동일한 결과
* 안전한 반복 실행
* 현재 상태 확인 후 변경

### 4. 모듈 기반
* 다양한 내장 모듈
* 커뮤니티 모듈 지원
* 확장 가능한 구조

### 5. 멀티 플랫폼
* Linux, Windows, 네트워크 장비 지원
* 클라우드 플랫폼 통합
* 다양한 환경 지원

---

## Ansible 아키텍처

### 1. Control Node (제어 노드)
* Ansible이 설치된 머신
* 플레이북 실행
* 인벤토리 관리

### 2. Managed Node (관리 노드)
* 관리 대상 서버
* 에이전트 불필요
* SSH 접근 가능해야 함

### 3. Inventory (인벤토리)
* 관리 대상 서버 목록
* 그룹화 가능
* 동적 인벤토리 지원

### 4. Playbook (플레이북)
* 자동화 작업 정의
* YAML 형식
* 재사용 가능

### 5. Module (모듈)
* 실행 가능한 작업 단위
* 내장 모듈 및 커스텀 모듈
* 멱등성 보장

---

## Ansible 핵심 개념

### 1. Inventory (인벤토리)
관리 대상 서버 목록을 정의하는 파일

**정적 인벤토리 예시:**
```ini
[webservers]
web1.example.com
web2.example.com

[databases]
db1.example.com

[webservers:vars]
ansible_user=admin
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

**YAML 형식:**
```yaml
all:
  children:
    webservers:
      hosts:
        web1.example.com:
        web2.example.com:
    databases:
      hosts:
        db1.example.com:
```

### 2. Playbook (플레이북)
자동화 작업을 정의하는 YAML 파일

**기본 플레이북 예시:**
```yaml
---
- name: 웹 서버 설정
  hosts: webservers
  become: yes
  tasks:
    - name: Apache 설치
      yum:
        name: httpd
        state: present
    
    - name: Apache 시작 및 활성화
      systemd:
        name: httpd
        state: started
        enabled: yes
    
    - name: 인덱스 페이지 생성
      copy:
        content: "<h1>Welcome</h1>"
        dest: /var/www/html/index.html
```

### 3. Play (플레이)
* 하나 이상의 태스크 그룹
* 특정 호스트 그룹에 적용
* 변수 및 역할 포함 가능

### 4. Task (태스크)
* 실행할 작업 단위
* 모듈 호출
* 멱등성 보장

### 5. Module (모듈)
* 특정 작업을 수행하는 코드
* 내장 모듈 및 커스텀 모듈
* 멱등성 보장

### 6. Role (역할)
* 재사용 가능한 플레이북 모음
* 구조화된 디렉토리 구조
* 변수, 템플릿, 파일 포함

---

## Ansible 설치

### Linux/macOS
```bash
# pip를 통한 설치
pip install ansible

# 패키지 매니저를 통한 설치 (Ubuntu/Debian)
sudo apt update
sudo apt install ansible

# 패키지 매니저를 통한 설치 (CentOS/RHEL)
sudo yum install ansible
```

### Windows
```bash
# WSL 사용 권장
# 또는 pip install ansible
```

### 설치 확인
```bash
ansible --version
```

---

## Ansible 기본 명령어

### Ad-hoc 명령어
```bash
# 핑 테스트
ansible all -i inventory -m ping

# 패키지 설치
ansible webservers -i inventory -m yum -a "name=httpd state=present" -b

# 파일 복사
ansible webservers -i inventory -m copy -a "src=/path/to/file dest=/tmp/file"

# 서비스 시작
ansible webservers -i inventory -m systemd -a "name=httpd state=started" -b

# 셸 명령 실행
ansible webservers -i inventory -m shell -a "uptime"
```

### 플레이북 실행
```bash
# 플레이북 실행
ansible-playbook playbook.yml

# 특정 인벤토리 사용
ansible-playbook -i inventory playbook.yml

# 특정 태그만 실행
ansible-playbook playbook.yml --tags "install"

# 체크 모드 (실제 변경 없이 테스트)
ansible-playbook playbook.yml --check

# 상세 출력
ansible-playbook playbook.yml -v
ansible-playbook playbook.yml -vvv
```

### 기타 명령어
```bash
# 인벤토리 확인
ansible-inventory -i inventory --list

# 변수 확인
ansible-inventory -i inventory --host web1.example.com

# 모듈 문서 확인
ansible-doc yum
ansible-doc -l  # 모든 모듈 목록
```

---

## Ansible 모듈

### 시스템 모듈
```yaml
# 패키지 관리
- name: 패키지 설치
  yum:  # 또는 apt, dnf
    name: nginx
    state: present

# 서비스 관리
- name: 서비스 시작
  systemd:
    name: nginx
    state: started
    enabled: yes

# 사용자 관리
- name: 사용자 생성
  user:
    name: appuser
    uid: 1001
    groups: wheel
    shell: /bin/bash

# 파일 권한
- name: 파일 권한 설정
  file:
    path: /etc/nginx/nginx.conf
    mode: '0644'
    owner: root
    group: root
```

### 파일 모듈
```yaml
# 파일 복사
- name: 파일 복사
  copy:
    src: /local/path/file.conf
    dest: /remote/path/file.conf
    owner: root
    group: root
    mode: '0644'

# 템플릿 사용
- name: 템플릿 배포
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
    backup: yes

# 디렉토리 생성
- name: 디렉토리 생성
  file:
    path: /var/www/html
    state: directory
    mode: '0755'

# 파일 내용 작성
- name: 파일 내용 작성
  copy:
    content: |
      server {
        listen 80;
        server_name example.com;
      }
    dest: /etc/nginx/conf.d/example.conf
```

### 네트워크 모듈
```yaml
# 방화벽 규칙
- name: 방화벽 포트 열기
  firewalld:
    port: 80/tcp
    permanent: yes
    state: enabled
    immediate: yes

# URL 확인
- name: URL 확인
  uri:
    url: http://example.com
    status_code: 200
```

### 명령 실행 모듈
```yaml
# 셸 명령
- name: 명령 실행
  shell: |
    cd /opt/app
    ./deploy.sh
  args:
    chdir: /opt/app

# 명령 실행 (셸 없이)
- name: 명령 실행
  command: /usr/bin/script.sh arg1 arg2

# 스크립트 실행
- name: 스크립트 실행
  script: /local/path/script.sh
```

---

## Ansible 변수

### 변수 정의
```yaml
---
- name: 변수 사용 예시
  hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  tasks:
    - name: 변수 사용
      debug:
        msg: "포트는 {{ http_port }}입니다"
```

### 인벤토리 변수
```ini
[webservers]
web1.example.com http_port=8080
web2.example.com http_port=8081

[webservers:vars]
max_clients=200
```

### 변수 파일
```yaml
# group_vars/webservers.yml
http_port: 80
max_clients: 200

# host_vars/web1.example.com.yml
http_port: 8080
```

### 팩트 (Facts)
```yaml
- name: 시스템 정보 수집
  setup:

- name: 팩트 사용
  debug:
    msg: "OS는 {{ ansible_os_family }}입니다"
```

---

## Ansible 조건문 및 루프

### 조건문
```yaml
- name: 조건부 작업
  yum:
    name: httpd
    state: present
  when: ansible_os_family == "RedHat"

- name: 다중 조건
  command: /usr/bin/script.sh
  when:
    - ansible_os_family == "RedHat"
    - ansible_distribution_major_version == "8"
```

### 루프
```yaml
- name: 여러 패키지 설치
  yum:
    name: "{{ item }}"
    state: present
  loop:
    - httpd
    - mysql
    - php

- name: 사용자 생성
  user:
    name: "{{ item.name }}"
    uid: "{{ item.uid }}"
  loop:
    - { name: 'user1', uid: 1001 }
    - { name: 'user2', uid: 1002 }
```

---

## Ansible 역할 (Roles)

### 역할 구조
```
roles/
  webserver/
    tasks/
      main.yml
    handlers/
      main.yml
    templates/
      nginx.conf.j2
    files/
      index.html
    vars/
      main.yml
    defaults/
      main.yml
    meta/
      main.yml
```

### 역할 사용
```yaml
---
- name: 웹 서버 설정
  hosts: webservers
  roles:
    - webserver
    - { role: database, vars: { db_name: myapp } }
```

### 역할 생성
```bash
ansible-galaxy init webserver
```

---

## Ansible Vault

### 암호화된 변수 파일
```bash
# 파일 암호화
ansible-vault create secrets.yml

# 파일 편집
ansible-vault edit secrets.yml

# 파일 암호화 해제 (읽기 전용)
ansible-vault view secrets.yml

# 파일 암호화
ansible-vault encrypt file.yml

# 파일 복호화
ansible-vault decrypt file.yml
```

### 플레이북에서 사용
```bash
# 실행 시 비밀번호 입력
ansible-playbook playbook.yml --ask-vault-pass

# 비밀번호 파일 사용
ansible-playbook playbook.yml --vault-password-file vault-pass.txt
```

---

## Ansible 모범 사례

### 1. 플레이북 구조
* 역할을 사용한 모듈화
* 변수 파일 분리
* 명확한 네이밍

### 2. 멱등성 보장
* 상태 확인 모듈 사용
* 명령 대신 모듈 사용
* 체크 모드로 테스트

### 3. 보안
* 비밀 정보는 Vault 사용
* SSH 키 관리
* 최소 권한 원칙

### 4. 문서화
* 주석 작성
* README 파일
* 변수 설명

### 5. 테스트
* 체크 모드 활용
* 스테이징 환경에서 테스트
* 점진적 배포

### 6. 버전 관리
* 플레이북을 Git으로 관리
* 태그 사용
* 코드 리뷰

---

## Ansible Galaxy

### 역할 검색 및 설치
```bash
# 역할 검색
ansible-galaxy search nginx

# 역할 설치
ansible-galaxy install geerlingguy.nginx

# requirements.yml에서 설치
ansible-galaxy install -r requirements.yml
```

### requirements.yml 예시
```yaml
---
roles:
  - name: geerlingguy.nginx
    version: 3.1.0
  - name: geerlingguy.mysql
    version: 3.0.0
```

---

## Ansible의 장점

* **에이전트리스**: 설치 및 유지보수 간단
* **간단한 문법**: YAML 기반으로 읽기 쉬움
* **멱등성**: 안전한 반복 실행
* **광범위한 지원**: 다양한 플랫폼 및 모듈
* **커뮤니티**: 활발한 커뮤니티 및 Galaxy
* **무료**: 오픈소스

---

## Ansible의 단점

* **성능**: 대규모 환경에서 느릴 수 있음
* **Windows 지원**: 제한적 (PowerShell 필요)
* **복잡한 로직**: 복잡한 조건문은 어려울 수 있음

---

## Ansible 사용 사례

### 1. 설정 관리
* 서버 설정 자동화
* 애플리케이션 구성 관리
* 환경 일관성 유지

### 2. 애플리케이션 배포
* 코드 배포 자동화
* 롤백 지원
* 무중단 배포

### 3. 프로비저닝
* 서버 초기 설정
* 패키지 설치
* 사용자 및 권한 관리

### 4. 오케스트레이션
* 다중 서버 작업 조율
* 복잡한 워크플로우 실행
* 의존성 관리

---

## 참고

* Ansible은 설정 관리와 자동화에 강력한 도구입니다
* 작은 작업부터 시작하여 점진적으로 확장하세요
* 역할을 활용하여 코드 재사용성을 높이세요
* 보안을 위해 Vault를 적극 활용하세요
* 정기적으로 체크 모드로 테스트하세요
