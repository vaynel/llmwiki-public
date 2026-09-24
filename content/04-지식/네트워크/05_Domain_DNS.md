---
publish: true
tags: [지식, 네트워크]
---


## 🌐 Domain & DNS 작동 과정 상세 정리

### ✅ 도메인이란?

* 사람이 기억하기 쉬운 **문자 기반 주소 체계**
* 예: `www.example.com`
* 실질적으로는 IP 주소(`93.184.216.34`)로 매핑되어야 인터넷 통신이 가능

### ✅ DNS란?

* **Domain Name System**의 약자
* 도메인 → IP 주소로 \*\*해석(Resolve)\*\*해주는 시스템
* OSI 7계층 중 Application Layer에서 작동
* 기본 포트: `53/UDP`, 일부 TCP

---

## 🔍 도메인 구조

`www.blog.example.co.kr` → 루트부터 오른쪽에서 왼쪽으로:

| 계층        | 구성        | 설명                                             |
| --------- | --------- | ---------------------------------------------- |
| Root      | `.`       | 최상위 루트 DNS 서버                                  |
| TLD       | `kr`      | 국가 코드 또는 일반 도메인 (`com`, `org`, `net`, `edu` 등) |
| SLD       | `co`      | 2차 도메인 (국가 도메인에서 계층 구성)                        |
| Subdomain | `example` | 등록한 개별 도메인 이름                                  |
| Host      | `www`     | 특정 서비스 서버 (웹서버 등)                              |

---

## 🔄 도메인 해석(DNS Resolution) 과정

1. **사용자 입력**: 브라우저에 `www.example.com` 입력
2. **로컬 캐시 확인**: OS/브라우저가 로컬에 DNS 결과를 저장한 적 있는지 확인
3. **`/etc/hosts` 확인** (Linux): 수동 등록된 IP 확인
4. **DNS Resolver로 쿼리 전송** (보통 ISP 제공)
5. **Recursive Query 시작**:

   * 로컬 DNS가 루트 서버(`.`)에 쿼리 → `.com` TLD 서버 반환
   * `.com` TLD → `example.com` 권한 서버(NS) 정보 반환
   * `example.com` NS → 최종 A 레코드(IP 주소) 반환
6. **결과 캐시 및 응답 반환**
7. **브라우저가 IP를 사용해 HTTP 요청 전송**

---

## 🧠 DNS 종류와 레코드

| 레코드 타입      | 설명                    |
| ----------- | --------------------- |
| A (Address) | 도메인 → IPv4 주소         |
| AAAA        | 도메인 → IPv6 주소         |
| CNAME       | 별칭 도메인 → 실제 도메인       |
| MX          | 메일 서버 주소 지정           |
| TXT         | SPF, DKIM 등 텍스트 정보 저장 |
| NS          | 권한 있는 네임서버 지정         |
| SOA         | 도메인 영역의 시작 정보         |

---

## 🌐 DNS 실무 예시

### ✅ Linux 도구:

```bash
nslookup www.example.com
dig www.example.com
cat /etc/resolv.conf
```

### ✅ DNS 설정 파일 위치:

* `/etc/hosts` → 로컬 도메인 직접 매핑
* `/etc/resolv.conf` → DNS 서버 지정 (ex: 8.8.8.8)
* BIND 서버 설정: `/etc/named.conf`, `/var/named/*.zone`

---

## 🚧 보안 관련 사항

* DNS 캐시 포이즈닝 공격 주의
* DNS over HTTPS (DoH), DNSSEC 사용 권장
* Public DNS 예: Google(8.8.8.8), Cloudflare(1.1.1.1)

---

> ✅ 정리: DNS는 도메인을 IP로 해석하는 핵심 시스템으로, 계층 구조와 캐시 전략, 레코드 설정 이해가 중요합니다.

---

## 📎 참고

* DNS RFC1034, RFC1035
* Linux `resolv.conf`, systemd-resolved
* 실무 환경에서는 로컬 DNS + 캐시 서버 구성도 고려됨
