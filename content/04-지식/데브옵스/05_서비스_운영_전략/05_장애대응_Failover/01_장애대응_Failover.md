---
publish: true
tags:
  - devops
  - failover
  - 장애대응
  - 운영전략
---

# 장애대응 및 Failover

> [!abstract] 개요
> 장애 발생 시 자동으로 정상 노드로 트래픽을 전환(Failover)하여 서비스 중단 시간을 최소화하고, 신속하게 시스템을 복구하는 핵심 운영 전략입니다.

## 1. 핵심 지표

| 지표 | 의미 | 목표 |
|------|------|------|
| **RTO** (Recovery Time Objective) | 장애 발생 후 복구까지 허용 시간 | 짧을수록 좋음 |
| **RPO** (Recovery Point Objective) | 복구 시 허용하는 데이터 손실 시간 | 짧을수록 좋음 |
| **MTTR** (Mean Time To Repair) | 평균 복구 시간 | 줄이는 것이 목표 |
| **MTBF** (Mean Time Between Failures) | 평균 장애 간격 | 늘리는 것이 목표 |

```text
장애 발생        복구 완료
   │                │
   ├── RTO ─────────┤  (이 시간 안에 복구해야 함)
   │
   └── RPO ──┤      (이 시점 이전 데이터는 허용)
          최근 백업
```

## 2. 네트워크 계층 관점 (OSI 7 Layer)

- **L3 (네트워크 계층):** BGP 기반의 라우팅이나 글로벌 DNS 설정을 조작하여, 리전(Region) 또는 데이터센터 단위 장애 시 다른 지역으로 트래픽 전체를 전환합니다.
- **L4 (전송 계층):** 로드밸런서가 백엔드 서버의 TCP/UDP 포트 연결 상태를 지속적으로 확인(Health Check)하고, 연결 실패 시 해당 노드를 라우팅 대상에서 제외합니다.
- **L7 (응용 계층):** HTTP 응답 코드(예: 500 에러)나 애플리케이션 지연 시간 등을 감지하여, [[01_로드밸런싱]] 규칙에 따라 정상 서비스나 대기 페이지로 트래픽을 우회합니다.

## 3. 구현 도구 및 서비스

| 구분 | 오픈소스 (직접 구축) | 상용 / 클라우드 서비스 |
| :--- | :--- | :--- |
| **클러스터 관리 / HA** | Keepalived, Pacemaker, Corosync | AWS Auto Scaling, Azure VM Scale Sets |
| **DNS 레벨 Failover** | CoreDNS, BIND | AWS Route 53, Cloudflare DNS |
| **서비스 메시 (차단)** | Envoy, Istio (Circuit Breaker 내장) | AWS App Mesh |

## 4. 핵심 장애 대응 패턴

### 4.1. 헬스체크 및 트래픽 격리 (Isolation)

> [!warning] 얕은 헬스체크 vs 깊은 헬스체크
> - **Shallow Check:** 단순 `/health` 엔드포인트의 200 응답만 확인. 속도는 빠르나 DB 연동 오류 등은 감지 불가.
> - **Deep Check:** DB, 캐시, 외부 API 등 의존성 연결까지 모두 확인. 서비스의 실제 가용성을 판단하지만 오버헤드가 발생할 수 있음.

### 4.2. DB Failover (Primary-Replica)

평상시에는 쓰기 트래픽을 Primary로, 읽기 트래픽을 Replica로 분산합니다.
Primary 노드에 장애가 감지되면, 복제본(Replica) 중 하나를 새로운 Primary로 자동 승격(Promotion)시키고 애플리케이션의 연결 설정을 동적으로 변경합니다.

### 4.3. Circuit Breaker (서킷 브레이커) 패턴

외부 시스템 장애가 내부 전체로 전파되는 것을 막는 패턴입니다.

- **CLOSED:** 정상 상태 (트래픽 통과)
- **OPEN:** 장애 감지 (즉시 에러 반환, 타임아웃 방지)
- **HALF-OPEN:** 일정 시간 후 일부 트래픽만 시도하여 복구 여부 확인

## 5. 실제 적용 예시

> [!example] 글로벌 스트리밍 서비스의 리전 단위 장애 대응
> 1. 서울 리전(Active)에 메인 서버, 도쿄 리전(Passive)에 백업 서버를 [[01_이중화]] 구성합니다.
> 2. AWS Route 53(DNS)이 1분마다 서울 리전의 상태를 체크합니다.
> 3. 데이터센터 전원 차단으로 서울 리전 응답이 없으면, DNS가 즉시 도메인의 IP를 도쿄 리전으로 변경(Failover)합니다.
> 4. 사용자는 일시적인 끊김 후, 자동으로 복구된 백업 서버를 통해 서비스를 계속 이용할 수 있습니다.
