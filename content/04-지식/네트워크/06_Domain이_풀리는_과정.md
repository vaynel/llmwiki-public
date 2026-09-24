---
publish: true
tags: [지식, 네트워크]
---

## 도메인이 풀리는 과정 (기술 상세 흐름)

### 예시: 사용자가 브라우저에 `www.example.com` 입력

---

### 브라우저 캐시 확인

* 브라우저 자체에 **최근 방문한 도메인과 IP 매핑이 있는지** 확인
* 있으면 DNS 질의 생략하고 바로 접속

---

### 운영체제 캐시 확인

* OS 수준에서 `nscd`, `systemd-resolved`, `dnsmasq` 등의 캐시 확인
* 존재 시 바로 반환됨

---

### `/etc/hosts` 파일 확인

* 리눅스 시스템에서는 이 파일에 수동으로 지정된 IP가 우선됨
* 예: `127.0.0.1   localhost`

---

### DNS Resolver에게 질의 시작

* `/etc/resolv.conf` 또는 시스템 설정을 통해 지정된 **Recursive DNS 서버**에게 요청 전송
* 일반적으로 ISP, 사내 DNS, 또는 Google(8.8.8.8), Cloudflare(1.1.1.1)

---

### Recursive DNS 서버가 실제 질의 수행

#### Root Name Server

* `.com`에 대한 정보 요청
* root는 `.com`을 관리하는 TLD DNS 서버 주소 반환

#### TLD Name Server (`.com`)

* `example.com`에 대한 NS(Name Server) 레코드 반환

#### Authoritative Name Server

* `example.com` 도메인을 실제 관리하는 DNS 서버
* 이 서버가 `www.example.com`의 **A 레코드(IP 주소)** 반환

---

### 응답 캐싱 & 반환

* Recursive Resolver는 이 IP 정보를 클라이언트에게 반환
* 동시에 일정 시간(TTL)에 따라 캐시 저장

---

### 브라우저가 IP로 접속

* 이제 웹브라우저는 해당 IP에 TCP 연결을 맺고(예: 포트 80 또는 443), HTTP 또는 HTTPS 요청 전송

---

## 캐시 계층 요약

```plaintext
[브라우저 캐시]
    ↓
[OS DNS 캐시]
    ↓
[/etc/hosts]
    ↓
[Recursive DNS]
    ↓
[Root → TLD → Authoritative]
```


