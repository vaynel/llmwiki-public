---
publish: true
tags: [지식, 클라우드]
---

# OpenStack 서비스 아키텍처

---

## 개요

OpenStack은 **분산 마이크로서비스 아키텍처**를 따릅니다. 각 서비스는 독립적으로 동작하며, 공통 컴포넌트(인증, 메시징, 데이터베이스)를 공유합니다.

### 아키텍처 원칙

**1. 모듈화 (Modularity)**
* 각 서비스는 독립적인 프로젝트
* 필요에 따라 서비스 선택 사용

**2. 확장성 (Scalability)**
* 수평 확장 (Horizontal Scaling)
* 서비스 인스턴스 추가로 용량 증가

**3. 가용성 (Availability)**
* Active-Active 또는 Active-Passive 구성
* 장애 시 자동 전환

**4. API 중심 (API-First)**
* 모든 기능이 REST API로 제공
* CLI와 Dashboard는 API 래퍼

---

## 공통 아키텍처 패턴

모든 OpenStack 서비스는 다음과 같은 공통 패턴을 따릅니다:

### 1. API Layer (API 계층)

**역할**:
* RESTful API 엔드포인트 제공
* 인증 및 권한 검증
* 요청 라우팅

**구성 요소**:
* **API Server**: HTTP 요청 처리
* **WSGI Application**: Python WSGI 서버 (uWSGI, Gunicorn)
* **Paste Configuration**: API 라우팅 설정

**예시 (Nova)**:
```
HTTP Request → Nova API → Nova Service
```

### 2. Service Layer (서비스 계층)

**역할**:
* 비즈니스 로직 처리
* 작업 스케줄링
* 리소스 관리

**구성 요소**:
* **Service Manager**: 서비스 인스턴스 관리
* **Scheduler**: 리소스 할당 결정
* **Worker**: 실제 작업 수행

### 3. Driver Layer (드라이버 계층)

**역할**:
* 백엔드 기술 추상화
* 다양한 하드웨어/소프트웨어 통합

**예시**:
* Nova: KVM, VMware, Hyper-V 드라이버
* Cinder: Ceph, iSCSI, NFS 드라이버
* Neutron: OVS, Linux Bridge, SDN 드라이버

### 4. Database Layer (데이터베이스 계층)

**역할**:
* 서비스 상태 저장
* 메타데이터 관리
* 트랜잭션 관리

**데이터베이스**:
* 각 서비스별 독립 데이터베이스
* MySQL/MariaDB/PostgreSQL 사용

### 5. Message Queue (메시지 큐)

**역할**:
* 서비스 간 비동기 통신
* 작업 큐 관리
* 이벤트 전달

**메시지 큐**:
* RabbitMQ (기본)
* ZeroMQ
* Apache Qpid

---

## 코어 서비스 아키텍처

### 1. Keystone (Identity Service)

**역할**: 인증, 권한 관리, 서비스 카탈로그

**주요 컴포넌트**:

**Identity (인증)**:
* **User**: 실제 사용자
* **Group**: 사용자 그룹
* **Project (Tenant)**: 리소스 격리 단위
* **Domain**: 프로젝트 그룹핑

**Authentication (인증 방법)**:
* **Password**: 사용자명/비밀번호
* **Token**: 임시 토큰 (기본)
* **Federated**: 외부 IdP 통합 (SAML, OAuth)

**Authorization (권한)**:
* **Role**: 권한 정의
* **Policy**: 정책 기반 접근 제어 (JSON 규칙)

**Service Catalog**:
* 등록된 서비스 목록 및 엔드포인트

**아키텍처**:
```
Client → Keystone API → Backend (SQL/LDAP)
                ↓
         Token 발급
                ↓
    서비스 인증 및 권한 확인
```

**주요 엔드포인트**:
* `/v3/auth/tokens`: 토큰 발급
* `/v3/projects`: 프로젝트 목록
* `/v3/users`: 사용자 관리

### 2. Nova (Compute Service)

**역할**: 가상 머신 생명주기 관리

**주요 컴포넌트**:

**nova-api**:
* REST API 제공
* 인증 및 권한 검증
* 요청 검증 및 라우팅

**nova-conductor**:
* 데이터베이스 접근 중재
* 장기 작업 관리
* Compute 노드와 DB 간 격리

**nova-scheduler**:
* VM 배치 결정
* 필터 및 웨이더 알고리즘
* 리소스 가용성 확인

**nova-compute**:
* Compute 노드에서 실행
* 하이퍼바이저와 통신
* VM 생성/삭제/관리

**nova-consoleauth**:
* 콘솔 접근 인증

**nova-novncproxy**:
* VNC 프록시

**nova-spicehtml5proxy**:
* SPICE 프록시

**아키텍처 흐름 (VM 생성)**:
```
1. Client → nova-api (VM 생성 요청)
2. nova-api → Keystone (인증)
3. nova-api → nova-conductor (스케줄링 요청)
4. nova-conductor → nova-scheduler (노드 선택)
5. nova-scheduler → nova-conductor (선택된 노드)
6. nova-conductor → nova-compute (VM 생성 명령)
7. nova-compute → Hypervisor (VM 생성)
8. nova-compute → Message Queue (상태 업데이트)
```

**하이퍼바이저 드라이버**:
* **libvirt/KVM**: Linux 기반, 가장 일반적
* **VMware vSphere**: vCenter 통합
* **Hyper-V**: Windows 환경
* **Xen**: Xen 하이퍼바이저
* **Bare Metal (Ironic)**: 물리적 서버

### 3. Neutron (Networking Service)

**역할**: 네트워크 가상화 및 관리

**주요 컴포넌트**:

**neutron-server**:
* REST API 제공
* 플러그인 관리
* 네트워크 정책 관리

**Neutron Plugins**:
* **ML2 (Modular Layer 2)**: 네트워크 타입 추상화
  * Type Drivers: local, flat, vlan, vxlan, gre
  * Mechanism Drivers: linuxbridge, openvswitch, SR-IOV

**L3 Agent (neutron-l3-agent)**:
* 라우터 관리
* NAT (SNAT, DNAT)
* Floating IP 관리

**DHCP Agent (neutron-dhcp-agent)**:
* 가상 네트워크에 DHCP 서비스 제공
* IP 주소 할당

**Metadata Agent (neutron-metadata-agent)**:
* VM 메타데이터 서비스 제공
* cloud-init과 통신

**Open vSwitch (OVS)**:
* 가상 스위치
* Flow 규칙 관리
* SDN 지원

**네트워크 아키텍처**:

**Provider Networks (간단한 구조)**:
```
VM → OVS/Linux Bridge → Physical Network
```

**Self-Service Networks (고급 구조)**:
```
VM → Tenant Network (VXLAN/GRE) → Router → External Network
```

**네트워크 구성 요소**:
* **Network**: 논리적 네트워크 격리 단위
* **Subnet**: IP 주소 범위 정의
* **Router**: 네트워크 간 라우팅
* **Port**: 네트워크 인터페이스
* **Security Group**: 방화벽 규칙

**Service Plugins**:
* **FWaaS (Firewall as a Service)**: 방화벽 규칙
* **LBaaS (Load Balancer as a Service)**: 로드 밸런서
* **VPNaaS (VPN as a Service)**: VPN 연결

### 4. Cinder (Block Storage Service)

**역할**: 블록 스토리지 볼륨 제공

**주요 컴포넌트**:

**cinder-api**:
* REST API 제공
* 요청 검증 및 라우팅

**cinder-scheduler**:
* 스토리지 백엔드 선택
* 용량 및 성능 기반 선택

**cinder-volume**:
* 볼륨 관리
* 백엔드 스토리지와 통신

**cinder-backup**:
* 볼륨 백업 관리

**스토리지 아키텍처**:
```
VM → Nova → Cinder API → Cinder Volume → Storage Backend
```

**스토리지 백엔드 드라이버**:
* **LVM**: 로컬 Logical Volume Manager
* **Ceph/RBD**: 분산 스토리지
* **NFS**: 네트워크 파일 시스템
* **iSCSI**: IP 기반 SAN
* **Fibre Channel**: FC SAN
* **GlusterFS**: 분산 파일 시스템
* **NetApp, EMC**: 상용 스토리지

**볼륨 타입 (Volume Types)**:
* 성능 특성별 분류
* QoS 설정
* 백엔드 매핑

### 5. Glance (Image Service)

**역할**: 가상 머신 이미지 저장 및 관리

**주요 컴포넌트**:

**glance-api**:
* REST API 제공
* 이미지 메타데이터 관리

**glance-registry**:
* 이미지 메타데이터 데이터베이스 관리 (deprecated)
* v2 API에서는 glance-api가 직접 처리

**Image Store**:
* **File**: 로컬 파일 시스템
* **Swift**: Object Storage
* **Ceph/RBD**: Ceph 스토리지
* **S3**: AWS S3 호환
* **HTTP**: 외부 HTTP 서버

**이미지 아키텍처**:
```
Nova → Glance API → Image Store (Swift/File/Ceph)
```

**이미지 형식**:
* **RAW**: 원시 디스크 이미지
* **QCOW2**: QEMU Copy-On-Write (권장)
* **VMDK**: VMware
* **VHD**: Hyper-V
* **ISO**: CD/DVD 이미지

**이미지 서비스**:
* 이미지 업로드/다운로드
* 이미지 스냅샷
* 이미지 공유 (멀티 테넌트)

### 6. Swift (Object Storage Service)

**역할**: 대용량 오브젝트 스토리지 제공

**주요 컴포넌트**:

**Proxy Server (swift-proxy-server)**:
* API 요청 처리
* 요청 라우팅
* 인증 처리

**Object Server (swift-object-server)**:
* 실제 데이터 저장
* 오브젝트 읽기/쓰기

**Container Server (swift-container-server)**:
* 컨테이너 메타데이터 관리
* 오브젝트 목록 관리

**Account Server (swift-account-server)**:
* 계정 메타데이터 관리
* 컨테이너 목록 관리

**Swift 아키텍처**:

**Ring (분산 해시 테이블)**:
* 오브젝트 위치 결정
* 복제본 배치
* 파티션 관리

**Replication**:
* 자동 복제 (기본 3복제본)
* 데이터 내구성 보장

**Consistency**:
* 최종 일관성 (Eventually Consistent)
* 자동 복구

**스토리지 구조**:
```
Account → Container → Object
```

**주요 특징**:
* **RESTful API**: HTTP/HTTPS
* **무제한 확장**: 수평 확장
* **자동 복제**: 장애 복구
* **비용 효율적**: 범용 하드웨어

---

## 서비스 간 상호작용

### VM 생성 흐름 (종합)

```
1. 사용자 → Horizon/nova CLI
   ↓
2. nova-api → Keystone (인증 토큰 검증)
   ↓
3. nova-api → Glance (이미지 정보 조회)
   ↓
4. nova-api → Neutron (네트워크 정보 조회)
   ↓
5. nova-api → Cinder (스토리지 정보 조회, 볼륨 생성)
   ↓
6. nova-api → nova-conductor (스케줄링 요청)
   ↓
7. nova-conductor → nova-scheduler (노드 선택)
   ↓
8. nova-scheduler → nova-conductor (선택된 노드)
   ↓
9. nova-conductor → nova-compute (VM 생성 명령)
   ↓
10. nova-compute → Hypervisor (VM 생성)
    ↓
11. nova-compute → Neutron (포트 생성)
    ↓
12. Neutron → DHCP Agent (IP 할당)
    ↓
13. VM 부팅 완료
```

### 인증 흐름

```
1. Client → Keystone (/v3/auth/tokens)
   ↓
2. Keystone → Identity Backend (사용자 인증)
   ↓
3. Keystone → Token 발급
   ↓
4. Client → 서비스 API (X-Auth-Token 헤더)
   ↓
5. 서비스 → Keystone (토큰 검증)
   ↓
6. Keystone → 서비스 (사용자 정보 반환)
   ↓
7. 서비스 → Policy Engine (권한 확인)
   ↓
8. 서비스 → 작업 수행
```

---

## 메시지 큐 아키텍처

### RabbitMQ 구조

**Exchange**:
* 메시지 라우팅
* **Topic Exchange**: 패턴 기반 라우팅 (기본)

**Queue**:
* 작업 큐
* 각 서비스별 큐

**RPC (Remote Procedure Call)**:
* **Call**: 요청
* **Cast**: 비동기 요청
* **Fanout**: 브로드캐스트

**예시 (Nova)**:
```
nova-api → RPC Call → nova-scheduler (스케줄링 요청)
nova-scheduler → RPC Cast → nova-compute (비동기 명령)
nova-compute → RPC Cast → nova-conductor (상태 업데이트)
```

---

## 데이터베이스 아키텍처

### 서비스별 데이터베이스

각 서비스는 독립적인 데이터베이스를 사용:

* **keystone**: 인증 정보
* **nova**: VM 메타데이터
* **neutron**: 네트워크 구성
* **cinder**: 볼륨 메타데이터
* **glance**: 이미지 메타데이터
* **swift**: 메타데이터 (별도)

### 데이터베이스 고가용성

**Active-Passive (Master-Slave)**:
* Galera Cluster (MySQL)
* PostgreSQL Streaming Replication

**Connection Pooling**:
* SQLAlchemy 사용
* 연결 풀 관리

---

## 고가용성 (HA) 아키텍처

### Active-Active 구성

**로드 밸런서**:
* HAProxy 또는 Keepalived
* VIP (Virtual IP) 제공

**서비스 인스턴스**:
* 여러 API 서버 실행
* 로드 밸런서로 분산

**데이터베이스**:
* Galera Cluster (Multi-Master)
* 또는 Master-Slave

**메시지 큐**:
* RabbitMQ Cluster
* Mirrored Queues

### 예시 구성

```
Internet
   ↓
Load Balancer (HAProxy)
   ↓
┌──────────┬──────────┬──────────┐
│ nova-api │ nova-api │ nova-api │
│ (Node 1) │ (Node 2) │ (Node 3) │
└──────────┴──────────┴──────────┘
   ↓              ↓              ↓
   └──────────┬───┴──────────────┘
              ↓
    Message Queue (RabbitMQ Cluster)
              ↓
    Database (Galera Cluster)
```

---

## 서비스 배포 모델

### 1. 모든 서비스 통합 노드 (All-in-One)

**구성**:
* 모든 서비스를 하나의 노드에 설치
* 개발/테스트용

**장점**:
* 빠른 설정
* 최소 리소스

**단점**:
* 프로덕션 부적합
* 성능 제한

### 2. 3-Node 구성 (최소 프로덕션)

**구성**:
* 1개 Controller Node
* 2개 Compute Nodes

**용도**:
* 소규모 프로덕션
* 개념 증명

### 3. 분리된 서비스 노드

**구성**:
* 3개 Controller Nodes (HA)
* 여러 Compute Nodes
* Storage Nodes (Cinder/Glance)
* Network Nodes (Neutron)

**용도**:
* 중대규모 프로덕션
* 엔터프라이즈 환경

---

## 보안 아키텍처

### 인증 및 권한

**Multi-Factor Authentication (MFA)**:
* Keystone에서 지원
* TOTP, SMS 등

**Role-Based Access Control (RBAC)**:
* Policy.json 파일로 정책 정의
* 서비스별 권한 관리

**Service-to-Service 인증**:
* Service Token 사용
* 서비스 간 신뢰

### 네트워크 보안

**Security Groups**:
* VM 레벨 방화벽
* iptables 기반

**Network Isolation**:
* VXLAN/GRE 터널링
* VLAN 분리

**SSL/TLS**:
* API 엔드포인트 암호화
* 인증서 관리

---

## 모니터링 아키텍처

### Telemetry 서비스 (Ceilometer/Gnocchi)

**Ceilometer**:
* 메트릭 수집
* 이벤트 수집

**Gnocchi**:
* 시계열 데이터 저장
* 메트릭 집계

**Aodh**:
* 알람 서비스
* 임계값 기반 알림

**데이터 흐름**:
```
서비스 → Notification → Ceilometer → Gnocchi → Aodh
```

---

## 학습 체크리스트

### 초급
- [ ] 각 코어 서비스의 역할 이해
- [ ] 서비스 간 기본 상호작용 이해
- [ ] VM 생성 흐름 이해
- [ ] 기본 아키텍처 구성 요소 파악

### 중급
- [ ] 각 서비스의 상세 컴포넌트 이해
- [ ] 메시지 큐 아키텍처 이해
- [ ] 데이터베이스 구조 이해
- [ ] 네트워크 아키텍처 이해 (Neutron)
- [ ] 스토리지 아키텍처 이해 (Cinder, Swift)

### 고급
- [ ] 고가용성 아키텍처 설계
- [ ] 성능 최적화
- [ ] 커스텀 드라이버 개발
- [ ] 서비스 확장 및 배포
- [ ] 트러블슈팅 및 디버깅

---

## 참고

* OpenStack 아키텍처 가이드: https://docs.openstack.org/arch-design/
* 각 서비스별 상세 아키텍처 문서 참조
* 실제 배포 시 요구사항에 맞게 아키텍처 조정 필요
* 성능과 가용성을 고려한 설계 중요


