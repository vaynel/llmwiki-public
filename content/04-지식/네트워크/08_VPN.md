---
publish: true
tags: [지식, 네트워크]
---

## 🔒 VPN (Virtual Private Network) 상세 정리

### ✅ VPN이란?

> VPN은 **공용 인터넷을 통해 사설 네트워크처럼 안전하게 연결**하는 기술입니다.

* 외부 네트워크에서도 내부망처럼 통신 가능 (예: 회사 내부망 접속)
* 암호화된 터널을 통해 **데이터 보안**, **위치 우회**, **접근 제어** 가능
* OSI 계층: 주로 **3계층(Network Layer)** 또는 **7계층(Application Layer)**

---

## 📦 VPN의 주요 구성 요소

| 구성 요소           | 설명                                            |
| --------------- | --------------------------------------------- |
| VPN Client      | 접속 요청을 보내는 사용자 측 장치 (ex: laptop, phone)       |
| VPN Server      | 클라이언트의 요청을 받아 인증 및 라우팅 처리                     |
| Tunnel Protocol | 데이터를 암호화하여 안전하게 터널링 (IPsec, WireGuard, SSL 등) |
| Authentication  | 사용자 인증 (예: ID/PW, 인증서, MFA 등)                 |

---

## 🔐 주요 VPN 프로토콜 비교

| 프로토콜       | 계층    | 특징                            |
| ---------- | ----- | ----------------------------- |
| PPTP       | L2    | 속도 빠름, 보안 취약 (권장 X)           |
| L2TP/IPsec | L2+L3 | 강력한 보안, 설정 복잡                 |
| IPsec      | L3    | 네트워크 계층 보호, 고성능, 기업용 자주 사용    |
| OpenVPN    | L4+L7 | TLS 기반, 유연성 좋음, 널리 사용됨        |
| WireGuard  | L3    | 최신 프로토콜, 설정 간단, 빠르고 안전        |
| SSL VPN    | L7    | 웹 기반 접근, 포트 제한 없음, 브라우저 통합 가능 |

---

## 🔄 VPN 작동 과정 (예: WireGuard 기준)

1. **Client가 VPN Server에 연결 요청** (UDP 51820)
2. **인증서/키 교환**으로 피어 신뢰 확인
3. **암호화된 터널 생성 (interface: `wg0`)**
4. **라우팅 설정 적용 (예: 0.0.0.0/0 via wg0)**
5. **모든 패킷이 VPN을 통해 전송되거나 특정 대역만 통과 (split/full tunnel)**

---

## 🧪 실무 예시

### ✅ WireGuard 설정 예 (`/etc/wireguard/wg0.conf`):

```ini
[Interface]
PrivateKey = ...
Address = 10.8.0.1/24
ListenPort = 51820

[Peer]
PublicKey = ...
AllowedIPs = 10.8.0.2/32
Endpoint = vpn.example.com:51820
PersistentKeepalive = 25
```

### ✅ OpenVPN 예시

```bash
sudo openvpn --config client.ovpn
```

### ✅ VPN 인터페이스 확인

```bash
ip a | grep wg0
ip route
```

---

## ⚠️ 보안 및 운영 주의사항

* Split Tunnel vs Full Tunnel 설정 고려
* DNS 누수 방지 설정 필요
* 인증서 및 키 관리 철저히 (권한 위임 주의)
* 포트 포워딩 및 방화벽 허용 필요 (예: UDP 51820, TCP 443)

---

## 📎 참고

* WireGuard 공식 문서: [https://www.wireguard.com](https://www.wireguard.com)
* OpenVPN, IPsec, Cloud VPN 등 비교 자료
* `iptables`, `ufw`로 VPN 경로 보호 및 NAT 설정
