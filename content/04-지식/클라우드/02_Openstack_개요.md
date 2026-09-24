---
publish: true
tags: [지식, 클라우드]
---

# OpenStack 개요

---

## OpenStack이란?

> **OpenStack**은 IaaS(Infrastructure as a Service) 형태의 프라이빗/퍼블릭 클라우드 구축을 위한 오픈소스 플랫폼입니다.

OpenStack은 가상화된 컴퓨팅, 스토리지, 네트워크 리소스를 중앙에서 관리하고, API를 통해 클라우드 인프라를 제공하는 클라우드 운영 시스템(Cloud Operating System)입니다.

---

## 개요

### 정의

OpenStack은 대규모 컴퓨팅, 스토리지, 네트워크 리소스를 풀(Pool)로 관리하고, 사용자에게 자동화된 방식으로 제공하는 클라우드 플랫폼입니다. AWS나 Azure와 유사한 클라우드 서비스를 자체 인프라에서 구축할 수 있게 해줍니다.

### 특징

* **오픈소스**: Apache 2.0 라이선스, 무료로 사용 가능
* **모듈형 아키텍처**: 필요에 따라 컴포넌트 선택 사용
* **확장성**: 수천 개의 노드까지 확장 가능
* **표준 API**: RESTful API 제공
* **다양한 하이퍼바이저 지원**: KVM, VMware, Hyper-V, Xen 등
* **플러그 가능한 아키텍처**: 다양한 백엔드 기술 통합

### 역사

* **2010년**: NASA와 Rackspace가 공동으로 시작
  * NASA의 Nova (Compute)
  * Rackspace의 Swift (Object Storage)
* **2012년**: OpenStack Foundation 설립
* **현재**: Cloud Native Computing Foundation (CNCF)와 함께 주요 오픈소스 클라우드 프로젝트

### 릴리즈 정책

* **6개월 주기**: 매년 4월과 10월에 새 버전 릴리즈
* **버전 네이밍**: 알파벳 순서로 도시 이름 사용
  * 예: Austin, Bexar, Cactus, Diablo, Essex, Folsom, Grizzly, Havana, Icehouse, Juno, Kilo, Liberty, Mitaka, Newton, Ocata, Pike, Queens, Rocky, Stein, Train, Ussuri, Victoria, Wallaby, Xena, Yoga, Zed

---

## OpenStack의 목적과 가치

### 주요 목적

**1. 클라우드 인프라 제공**
* 가상 머신 (VM) 생성 및 관리
* 블록 스토리지 제공
* 오브젝트 스토리지 제공
* 네트워크 서비스 제공

**2. 리소스 자동화 및 관리**
* 자동화된 프로비저닝
* 자동 스케일링
* 리소스 풀 관리

**3. 멀티 테넌시 지원**
* 여러 조직/프로젝트가 동일 인프라 공유
* 격리 보장
* 할당량(Quota) 관리

### 주요 가치

**비용 효율성**:
* 오픈소스로 라이선스 비용 없음
* 기존 하드웨어 활용 가능
* 공용 클라우드 대비 비용 절감 가능

**유연성**:
* 벤더 종속성 없음
* 커스터마이징 가능
* 다양한 하드웨어/소프트웨어 통합

**제어권**:
* 데이터와 인프라의 완전한 제어
* 보안 정책 직접 설정
* 규정 준수 요구사항 충족

---

## OpenStack vs 다른 클라우드 플랫폼

### AWS, Azure, GCP (퍼블릭 클라우드)

| 특징 | OpenStack | AWS/Azure/GCP |
| ---- | --------- | ------------- |
| **배포** | 자체 인프라 | 클라우드 제공자 |
| **비용** | 인프라 투자 필요 | 사용한 만큼 지불 |
| **제어** | 완전한 제어 | 제한적 |
| **설치/운영** | 복잡함, 전문 인력 필요 | 제공자 관리 |
| **데이터 위치** | 자체 데이터센터 | 제공자 데이터센터 |

### VMware vSphere

| 특징 | OpenStack | VMware vSphere |
| ---- | --------- | -------------- |
| **라이선스** | 오픈소스 (무료) | 상용 (유료) |
| **API** | RESTful API (표준) | vSphere API |
| **확장성** | 대규모 클라우드 환경 | 엔터프라이즈 가상화 |
| **통합** | 다양한 오픈소스 도구 | VMware 생태계 |

### 사용 시나리오

**OpenStack이 적합한 경우**:
* 프라이빗 클라우드 구축 필요
* 데이터 주권(Data Sovereignty) 중요
* 대규모 인프라 운영
* 오픈소스 선호
* 커스터마이징 필요

**퍼블릭 클라우드가 적합한 경우**:
* 빠른 시작 필요
* 운영 인력 부족
* 초기 투자 최소화
* 글로벌 인프라 필요

---

## OpenStack의 구성 요소 (Core Services)

OpenStack은 모듈형 아키텍처로 여러 서비스로 구성됩니다.

### 필수 코어 서비스 (Big 6)

**1. Nova (Compute)**
* 가상 머신 생성 및 관리
* 하이퍼바이저 관리
* 스케일링 및 고가용성

**2. Neutron (Networking)**
* 가상 네트워크 생성
* 서브넷, 라우터 관리
* 보안 그룹 및 방화벽
* 로드 밸런서

**3. Cinder (Block Storage)**
* 블록 스토리지 볼륨 제공
* 스냅샷 및 백업
* 다양한 스토리지 백엔드 지원

**4. Swift (Object Storage)**
* 대용량 오브젝트 스토리지
* 분산 저장
* HTTP API 제공

**5. Glance (Image Service)**
* 가상 머신 이미지 저장 및 관리
* 이미지 버전 관리
* 다양한 이미지 형식 지원

**6. Keystone (Identity Service)**
* 사용자 인증 및 권한 관리
* 토큰 기반 인증
* 서비스 카탈로그

### 추가 서비스 (Optional Services)

**7. Horizon (Dashboard)**
* 웹 기반 사용자 인터페이스
* 그래픽 관리 도구

**8. Heat (Orchestration)**
* 인프라 자동화
* 템플릿 기반 리소스 생성

**9. Ceilometer/Telemetry (Monitoring)**
* 사용량 및 메트릭 수집
* 청구(Billing) 데이터 제공

**10. Barbican (Key Management)**
* 암호화 키 관리
* 보안 강화

**11. Designate (DNS)**
* DNS 서비스 관리

**12. Trove (Database Service)**
* 데이터베이스 서비스 제공
* 관계형 및 NoSQL 지원

---

## OpenStack 아키텍처 개요

### 기본 구조

OpenStack은 **분산 아키텍처**를 따릅니다:

**1. Control Plane (제어 노드)**
* API 서버 실행
* 데이터베이스 및 메시지 큐
* 서비스 관리

**2. Compute Nodes (컴퓨트 노드)**
* 실제 가상 머신 실행
* 하이퍼바이저 설치

**3. Storage Nodes (스토리지 노드)**
* 블록 스토리지 또는 오브젝트 스토리지
* 분산 저장

**4. Network Nodes (네트워크 노드)**
* 네트워크 서비스 제공
* 라우팅 및 NAT

### 통신 구조

**Message Queue (RabbitMQ/AMQP)**:
* 서비스 간 비동기 메시지 전달
* 작업 큐 관리

**Database (MySQL/MariaDB/PostgreSQL)**:
* 서비스 상태 및 메타데이터 저장
* 각 서비스별 데이터베이스

**RESTful API**:
* 모든 서비스가 REST API 제공
* HTTP/HTTPS 통신

---

## OpenStack 배포 모델

### 1. All-in-One (단일 노드)

모든 서비스를 하나의 노드에서 실행합니다.

**용도**:
* 개발 및 테스트
* 학습 및 평가

**장점**:
* 빠른 설정
* 최소 하드웨어 요구사항

**단점**:
* 프로덕션 사용 불가
* 성능 제한

### 2. Multi-Node (다중 노드)

여러 노드에 서비스를 분산 배포합니다.

**구성**:
* 1개 Control Node
* 여러 Compute Nodes
* 선택적 Storage/Network Nodes

**용도**:
* 중소규모 프로덕션
* 개념 증명(PoC)

### 3. High Availability (HA)

모든 서비스에 고가용성 구성합니다.

**구성**:
* 여러 Control Nodes (Active-Active 또는 Active-Passive)
* Load Balancer
* 공유 스토리지

**용도**:
* 대규모 프로덕션 환경
* 엔터프라이즈 배포

### 4. Hyper-Converged

컴퓨트와 스토리지를 같은 노드에서 실행합니다.

**장점**:
* 단순한 아키텍처
* 리소스 효율성

---

## OpenStack 배포 도구

### OpenStack 배포를 위한 도구

**1. OpenStack-Ansible (OSA)**
* Ansible 기반 배포
* 유연한 커스터마이징

**2. Kolla-Ansible**
* Docker 컨테이너 기반
* 각 서비스를 컨테이너로 실행

**3. TripleO (OpenStack on OpenStack)**
* OpenStack으로 OpenStack 배포
* Undercloud/Overcloud 모델

**4. DevStack**
* 개발 환경용
* 단일 노드 빠른 설정

**5. Packstack**
* RDO (Red Hat OpenStack) 배포 도구
* 단순한 설정

**6. Canonical OpenStack (Charmed OpenStack)**
* Ubuntu 기반
* Juju Charms 사용

**7. OpenStack-Helm**
* Kubernetes 위에 OpenStack 배포
* Helm Charts 사용

---

## OpenStack의 장점과 단점

### 장점

**1. 오픈소스**
* 라이선스 비용 없음
* 소스 코드 접근 가능
* 커뮤니티 지원

**2. 벤더 독립성**
* 특정 벤더에 종속되지 않음
* 하드웨어 선택의 자유
* 마이그레이션 용이

**3. 확장성**
* 소규모에서 대규모까지 확장
* 수천 개의 노드 지원

**4. 유연성**
* 필요한 서비스만 선택
* 다양한 통합 가능

**5. 표준 API**
* RESTful API
* 다양한 클라이언트 도구

**6. 커뮤니티**
* 활발한 개발 커뮤니티
* 정기적인 업데이트
* 광범위한 문서

### 단점

**1. 복잡성**
* 설치 및 구성이 복잡
* 많은 컴포넌트 이해 필요

**2. 운영 난이도**
* 전문 인력 필요
* 지속적인 모니터링 및 관리

**3. 초기 투자**
* 하드웨어 투자 필요
* 인력 교육 비용

**4. 문서화**
* 방대한 문서
* 버전별 차이로 혼란 가능

**5. 업그레이드**
* 버전 업그레이드가 복잡
* 다운타임 발생 가능

---

## OpenStack 사용 사례

### 1. 프라이빗 클라우드

기업 내부에서 클라우드 인프라 구축:
* 데이터 보안 및 규정 준수
* 자체 데이터센터 활용
* 예측 가능한 비용

### 2. 하이브리드 클라우드

프라이빗과 퍼블릭 클라우드 통합:
* 데이터는 프라이빗에
* 확장은 퍼블릭으로
* 일관된 API 사용

### 3. 전자통신사업자 (Telco)

5G 및 NFV (Network Functions Virtualization):
* 가상 네트워크 기능
* 엣지 컴퓨팅
* 낮은 지연시간 요구사항

### 4. 과학 연구 기관

대규모 컴퓨팅 자원 필요:
* HPC (High Performance Computing)
* 대용량 데이터 처리
* 연구 프로젝트 격리

### 5. 교육 기관

학생 및 연구자를 위한 클라우드 제공:
* 교육 환경 제공
* 리소스 할당량 관리
* 비용 효율적

### 6. 스타트업 및 중소기업

AWS 대안으로 프라이빗 클라우드:
* 장기 비용 절감
* 데이터 제어
* 커스터마이징

---

## OpenStack 생태계

### 주요 참여 기업

**플래티넘 멤버**:
* Red Hat (IBM)
* Canonical
* SUSE
* HPE
* Huawei
* Intel
* 등

**골드 멤버**:
* VMware
* Oracle
* Dell
* Cisco
* 등

### 주요 배포판

**1. Red Hat OpenStack Platform (RHOSP)**
* 엔터프라이즈 지원
* 상업적 지원 제공

**2. Canonical OpenStack**
* Ubuntu 기반
* Juju 배포 도구

**3. SUSE OpenStack Cloud**
* SUSE Linux 기반

**4. Mirantis OpenStack**
* 전문 서비스 제공

**5. OVH Public Cloud**
* 퍼블릭 클라우드 서비스

---

## OpenStack 학습 경로

### 초급

**기본 개념 이해**:
- [ ] OpenStack이 무엇인지 이해
- [ ] 코어 서비스 이해 (Nova, Neutron, Cinder, Swift, Glance, Keystone)
- [ ] 기본 아키텍처 이해
- [ ] DevStack으로 로컬 환경 구축

**기본 사용**:
- [ ] Horizon 대시보드 사용
- [ ] CLI (OpenStack Client) 사용
- [ ] 가상 머신 생성 및 관리
- [ ] 네트워크 구성

### 중급

**서비스 이해**:
- [ ] 각 서비스의 상세 아키텍처
- [ ] 서비스 간 상호작용
- [ ] API 사용
- [ ] Heat 템플릿 작성

**운영**:
- [ ] 로그 분석
- [ ] 문제 해결
- [ ] 성능 모니터링
- [ ] 백업 및 복구

**배포**:
- [ ] Multi-node 배포
- [ ] 배포 도구 사용 (Kolla, OSA 등)
- [ ] 하드웨어 요구사항 계획

### 고급

**고급 운영**:
- [ ] High Availability 구성
- [ ] 스케일링 및 최적화
- [ ] 보안 강화
- [ ] 다중 리전 구성

**커스터마이징**:
- [ ] 서비스 수정 및 확장
- [ ] 커스텀 플러그인 개발
- [ ] 통합 개발

**아키텍처 설계**:
- [ ] 엔터프라이즈 아키텍처 설계
- [ ] 재해 복구 계획
- [ ] 용량 계획

---

## 참고

* OpenStack 공식 문서: https://docs.openstack.org/
* OpenStack 소스 코드: https://opendev.org/openstack
* OpenStack 커뮤니티: https://www.openstack.org/
* OpenStack 릴리즈 정보: https://releases.openstack.org/
* OpenStack은 지속적으로 발전하고 있으므로 최신 문서를 확인하세요
