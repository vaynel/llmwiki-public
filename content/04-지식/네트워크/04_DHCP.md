---
publish: true
tags: [지식, 네트워크]
---

좋습니다. 이제 네트워크 기본 중에서도 실무에서 아주 중요한 \*\*DHCP (Dynamic Host Configuration Protocol)\*\*를 상세히 정리해드릴게요. 개념부터 실제 동작 과정, 실무 설정, 문제 상황까지 포함합니다.

---

# 📡 DHCP (Dynamic Host Configuration Protocol) 완전 정리

---

## ✅ DHCP란?

> **IP 주소를 자동으로 할당해주는 프로토콜**입니다.
> 클라이언트가 네트워크에 연결될 때, 필요한 IP 정보(DNS, 게이트웨이 포함)를 DHCP 서버가 자동으로 부여합니다.

* **OSI 7계층 중 Application Layer**
* IP는 물론 **서브넷 마스크, DNS, 게이트웨이, 리스 타임** 등도 함께 설정

---

## 🔧 왜 DHCP가 필요한가?

* 수동 IP 설정은 사람이 관리하기 어렵고 오류가 많음
* 수천 대 장비가 자동으로 IP를 가져와야 하는 대규모 네트워크에서 필수
* 주소 충돌 방지 및 임시 장비 접속에 유리

---

## 🔁 DHCP 동작 과정 (DORA 방식)

```plaintext
1. Discover → 2. Offer → 3. Request → 4. Acknowledge
```

### 📶 1. DHCP Discover

* 클라이언트가 네트워크에 접속할 때 \*\*브로드캐스트(255.255.255.255)\*\*로 DHCP 서버 찾음

### 📬 2. DHCP Offer

* DHCP 서버가 사용 가능한 IP와 기타 설정 정보를 포함해 응답

### 📩 3. DHCP Request

* 클라이언트가 받은 제안 중 하나를 선택하여 **요청**

### ✅ 4. DHCP ACK

* 서버가 요청을 수락하고, 해당 IP를 **리스(lease)** 형태로 부여

---

## 🗂️ DHCP가 할당하는 정보 항목

| 항목               | 설명                     |
| ---------------- | ---------------------- |
| IP Address       | 클라이언트에 할당할 IP 주소       |
| Subnet Mask      | 서브넷 범위 지정              |
| Gateway (Router) | 외부로 나갈 기본 경로           |
| DNS Servers      | 이름 해석용 서버              |
| Lease Time       | IP 주소 임대 시간 (ex: 24시간) |
| Domain Name      | 내부 네트워크 도메인 지정         |
| NTP Server       | 시간 동기화를 위한 서버 지정 (선택)  |

---

## 🏠 실무 예시

### 예: 가정용 공유기

* DHCP 서버: 공유기
* 클라이언트: 노트북, 스마트폰 등
* 연결 즉시 `192.168.0.x` 대의 IP, 게이트웨이, DNS 자동 부여

### 예: 리눅스 서버에서 수동 설정

```bash
sudo dhclient eth0
```

→ eth0 인터페이스로 DHCP Discover 보내고 자동 IP 요청

---

## 🖥️ 리눅스에서 DHCP 서버 구성 예시 (ISC-DHCP)

### `/etc/dhcp/dhcpd.conf` 예시:

```conf
subnet 192.168.100.0 netmask 255.255.255.0 {
  range 192.168.100.10 192.168.100.100;
  option routers 192.168.100.1;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  default-lease-time 600;
  max-lease-time 7200;
}
```

---

## 🔍 DHCP vs Static IP

| 항목       | DHCP       | Static IP             |
| -------- | ---------- | --------------------- |
| 설정 편의성   | 자동         | 수동                    |
| IP 충돌 위험 | 낮음         | 있음                    |
| 관리 대규모   | 쉬움         | 어려움                   |
| 사용 용도    | 대부분의 클라이언트 | 서버, 프린터 등 고정 주소 필요 장비 |

---

## ⚠️ 문제 상황 & 디버깅

### 🛠️ "IP를 못 받는다":

* 서버가 없음 / 브로드캐스트 차단됨 / 중간 스위치가 relay 안 함

### 🛠️ "계속 다른 IP를 받는다":

* lease time 짧거나, IP 범위 내 다른 장비와 충돌

### 🛠️ 로그 확인:

* 서버:

  ```bash
  tail -f /var/log/syslog | grep dhcpd
  ```
* 클라이언트:

  ```bash
  journalctl -u NetworkManager
  ```

---

## 🧠 고급 기능

* **DHCP Reservation**: MAC 주소 기반 고정 IP 할당
* **PXE 부팅**: DHCP + TFTP 조합으로 무디스크 서버 부팅
* **DHCP Relay Agent**: 다른 서브넷의 DHCP 서버와 연결

---

## ✅ 결론

> DHCP는 IP 할당을 자동화함으로써 네트워크 운영의 기본 인프라를 제공합니다.
> 특히 클라우드, VM, 컨테이너 환경에서 **동적 환경을 관리하는 핵심 요소**입니다.
