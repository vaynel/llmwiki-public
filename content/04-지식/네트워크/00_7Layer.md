---
publish: true
tags: [지식, 네트워크]
---

## 1️⃣ 물리 계층 (Physical Layer)

> 비트(bit)를 전기적 신호로 변환해 실제 전송하는 계층

* 전압, 케이블, 커넥터, 핀배열 등 물리적 요소 담당
* 데이터 단위: 비트 (bit)
* 대표 장비: 허브, 리피터
* 예시 프로토콜 및 기술:

  * RS-232
  * RJ-45
  * IEEE 802.3 (이더넷 물리 규격)
  * 광케이블, 동축 케이블

---

## 2️⃣ 데이터 링크 계층 (Data Link Layer)

> 물리 계층에서 발생할 수 있는 오류를 감지하고, MAC 주소 기반으로 노드 간 데이터 전송 제어

* 데이터 단위: 프레임 (Frame)
* 오류 검출, 흐름 제어, MAC 주소 사용
* 대표 장비: 스위치, 브리지
* 예시 프로토콜:

  * Ethernet
  * PPP (Point-to-Point Protocol)
  * ARP (Address Resolution Protocol)
  * VLAN (802.1Q)

---

## 3️⃣ 네트워크 계층 (Network Layer)

> 목적지까지 최적 경로로 데이터 전송 (라우팅)

* 데이터 단위: 패킷 (Packet)
* 논리적 주소 (IP 주소)를 기반으로 전송
* 대표 장비: 라우터
* 예시 프로토콜:

  * IP (IPv4, IPv6)
  * ICMP (핑, 트레이스루트)
  * ARP/RARP
  * OSPF, BGP (라우팅 프로토콜)

---

## 4️⃣ 전송 계층 (Transport Layer)

> 종단 간 통신 제공, 신뢰성 및 흐름 제어

* 데이터 단위: 세그먼트 (TCP), 데이터그램 (UDP)
* 포트 번호 기반 통신
* 예시 프로토콜:

  * TCP (Transmission Control Protocol)
  * UDP (User Datagram Protocol)
  * SCTP (Stream Control Transmission Protocol)

---

## 5️⃣ 세션 계층 (Session Layer)

> 세션 수립, 유지, 종료를 담당 (응용 간 연결)

* 애플리케이션 간의 세션 관리
* 예시 프로토콜:

  * NetBIOS
  * RPC (Remote Procedure Call)
  * PPTP (Point-to-Point Tunneling Protocol)

---

## 6️⃣ 표현 계층 (Presentation Layer)

> 데이터 포맷, 인코딩/디코딩, 암호화/복호화

* 서로 다른 시스템 간 데이터 형식 호환
* 예시:

  * JPEG, MPEG (포맷)
  * ASCII, EBCDIC (문자 인코딩)
  * TLS/SSL (암호화 계층)

---

## 7️⃣ 응용 계층 (Application Layer)

> 사용자와 가장 가까운 계층, 실질적인 서비스 제공

* 사용자와의 인터페이스 제공
* 응용 프로그램의 네트워크 접근 창구 역할
* 예시 프로토콜:

  * HTTP / HTTPS (웹 접속)
  * DNS (도메인 해석)
  * FTP / SFTP (파일 전송)
  * SMTP / IMAP / POP3 (메일 서비스)
  * SSH / Telnet (원격 접속)
  * SNMP (네트워크 관리)
  * RDP (원격 데스크탑)

