---
title: "RPKI·BGP 용어정리"
publish: true
created: 2026-10-27
updated: 2026-10-27
tags: [네트워크, BGP, RPKI, 용어정리]
---

# RPKI·BGP 용어정리

[인프라 지도](<04-지식/개발/인프라/README.md>) · [RPKI 학습자료](<04-지식/개발/인프라/09_RPKI_기초부터_운영까지.md>) · [BGP 하이재킹](<04-지식/개발/인프라/11_BGP_하이재킹.md>)

> 용어를 외우기 전에 **경로 정보 → 권한 검증 → 정책과 전달**의 어느 부분에 속하는지 구분한다.

![경로 교환·권한 검증·패킷 전달로 나눈 용어 지도](<04-지식/개발/인프라/images/rpki/RPKI_03_용어지도.png>)

*그림 1. 각 열은 역할을 묶은 것이다. BGP 업데이트와 실제 사용자 트래픽은 서로 다른 정보 흐름이다.*

## 1. 경로를 표현하는 용어

| 용어 | 뜻 | 예시·주의점 |
|---|---|---|
| AS | Autonomous System. 하나의 명확한 라우팅 정책으로 운영되는 네트워크 집합 | 회사 하나와 항상 일대일은 아니다 |
| ASN | AS를 식별하는 번호 | 예: 문서용 AS64496 |
| Prefix | 주소 범위의 공통 앞부분과 길이 | 192.0.2.0/24 |
| CIDR | 주소를 프리픽스 길이로 표현·집계하는 방식 | /24보다 /25가 더 작은 대역 |
| BGP | AS 간 경로 정보를 교환하는 프로토콜 | 패킷 본문을 전달하는 터널이 아니다 |
| eBGP / iBGP | 서로 다른 AS / 같은 AS 내부의 BGP 관계 | 내부 공유에도 BGP가 사용됨 |
| NLRI | 도달 가능한 대상 대역을 나타내는 정보 | 일반적인 유니캐스트에서는 IP 프리픽스 |
| AS_PATH | 경로가 통과해 온 AS 정보를 담는 속성 | 보이는 경로가 암호학적으로 증명됐다는 뜻은 아님 |
| Origin ASN | 경로를 처음 기원한 AS 번호 | 일반적인 AS_SEQUENCE에서는 오른쪽 끝 ASN |
| BGP ORIGIN | 경로 정보의 기원을 분류하는 별도 BGP 속성 | IGP·EGP·INCOMPLETE. Origin ASN과 다름 |

위 정의의 BGP 속성·경로 처리 기반은 [RFC 4271](https://www.rfc-editor.org/rfc/rfc4271)을 따른다. AS_SET·컨페더레이션·빈 경로 등의 origin 판정은 단순한 “오른쪽 끝” 설명만으로 처리하지 말고 [RFC 6811](https://www.rfc-editor.org/rfc/rfc6811)의 규칙을 확인한다.

## 2. 권한을 검증하는 용어

| 용어 | 뜻 | 헷갈리기 쉬운 점 |
|---|---|---|
| RPKI | Resource Public Key Infrastructure | TLS 웹 인증서와 용도가 다르며 인터넷 번호 자원을 다룸 |
| Resource Certificate | 자원 범위를 담은 인증서 | 웹사이트 콘텐츠의 신뢰성을 보증하지 않음 |
| TA | Trust Anchor, 신뢰 앵커. 인증서 검증의 출발점으로 신뢰하는 기준 | RPKI에서는 자체 서명 CA 인증서로 표현. 자체 서명만으로 신뢰가 생기지는 않음 |
| TAL | Trust Anchor Locator. TA 인증서 위치 URI와 공개키를 담은 파일 | 인증서 자체가 아니며, 신뢰할 수 있는 경로로 설치·관리해야 함 |
| CA | Certification Authority, 인증기관. 인증서를 발급하고 관리하는 역할 | 하위 CA·EE 인증서를 발급. 웹 TLS 인증기관만을 뜻하지 않음 |
| CA Certificate | CA의 공개키·자원 범위 등을 담은 인증서 | CA라는 발급 주체와 인증서 객체를 구분 |
| EE Certificate | End-Entity 인증서. ROA 등 서명 객체의 검증에 쓰는 공개키를 인증 | ROA의 경우 대응하는 개인키로 ROA에 서명하며, EE는 하위 인증서를 발급하지 않음 |
| Certificate Chain | 신뢰 앵커에서 하위 인증서로 이어지는 인증 경로 | BGP의 AS_PATH와 전혀 다른 관계 |
| Issuer / Subject | 인증서를 발급한 주체 / 그 인증서가 나타내는 대상 | RPKI 인증서는 조직의 실명보다 자원 사용 권한을 다룸 |
| ROA | Route Origin Authorization. origin 권한을 담은 서명 객체 | 한 객체에 여러 프리픽스가 들어갈 수 있음 |
| maxLength | 허용되는 가장 긴 프리픽스 길이 | 주소 개수·AS_PATH 길이가 아님 |
| Validator / RP | Relying Party. RPKI 객체를 검증하는 소프트웨어 | 라우터의 경로 비교와 구분 |
| VRP | Validated ROA Payload. 검증된 prefix·길이·ASN 정보 | ROA의 서명된 원본 자체가 아님 |
| RTR | RPKI-to-Router 프로토콜 | 검증 캐시에서 라우터로 정보 전달 |
| ROV | Route Origin Validation | 전체 경로 검증과 다름 |
| Manifest / CRL | 저장소 객체 목록 / 인증서 폐기 목록 | Validator가 최신·유효한 자료를 판단할 때 사용하는 구성요소 |

구조는 [RFC 6480](https://www.rfc-editor.org/rfc/rfc6480), ROA는 [RFC 9582](https://www.rfc-editor.org/rfc/rfc9582), RTR은 [RFC 8210](https://www.rfc-editor.org/rfc/rfc8210)을 참고한다.

### TA·CA·EE는 어떻게 연결될까?

![TAL로 확인한 TA에서 하위 CA와 EE를 거쳐 ROA를 검증하는 관계](<04-지식/개발/인프라/images/rpki/RPKI_05_신뢰체인.png>)

*그림 2. 아래쪽은 인증서 발급 관계와 ROA 서명 검증 관계를 단순화했다. 하위 CA는 여러 단계일 수 있으며 인증서 자체가 서명하는 것은 아니다.*

**TA는 신뢰의 출발점이라는 지위이고, CA는 인증서를 발급하는 역할이다.** RPKI의 TA 인증서는 자체 서명 CA 인증서다. 하위 CA는 상위 CA가 발급한 인증서를 통해 검증 경로에 연결된다. 따라서 TA와 CA를 서로 무관한 두 종류의 기관으로 외우면 안 된다.

Validator는 신뢰할 수 있게 전달받은 **TAL**의 URI에서 TA 인증서를 가져오고, 인증서의 공개키가 TAL의 공개키와 일치하는지 확인한다. 아무 TAL이나 받아 설치하는 것은 신뢰의 시작점을 임의로 바꾸는 일이다. TAL은 위치만 알려 주는 주소록도, 인증서 원본도 아니다. [TAL과 TA: RFC 8630](https://www.rfc-editor.org/rfc/rfc8630)

ROA 검증에서는 **EE 인증서**와 ROA 서명을 함께 본다. CA는 EE 인증서를 발급하고, 그 EE 인증서의 공개키에 대응하는 개인키로 ROA에 서명한다. 검증자는 인증서 체인·유효성·폐기 상태·자원 범위와 ROA 서명을 확인한다. “CA가 인증서를 발급한다”와 “ROA를 서명한다”를 하나의 동작으로 합치지 않는다. [자원 인증서: RFC 6487](https://www.rfc-editor.org/rfc/rfc6487) · [ROA 검증: RFC 9582](https://www.rfc-editor.org/rfc/rfc9582)

| 질문 | 기억할 답 |
|---|---|
| 무엇을 출발점으로 신뢰하는가? | TA |
| 그 TA를 어디서 가져오고 어떤 키와 대조하는가? | TAL |
| 누가 하위 인증서를 발급하는가? | CA |
| ROA 서명 검증에 사용하는 공개키는 어디에 있는가? | 해당 EE 인증서 |
| 인증서를 실제로 따라가며 검증하는 주체는? | Validator / RP |

## 3. 검증 상태와 실제 동작

- **Valid:** 조건을 만족하는 VRP가 하나 이상 있다.
- **Invalid:** 포함하는 VRP는 있지만 조건을 만족하는 것은 없다.
- **NotFound:** 포함하는 VRP가 없다.

이 상태는 로컬 검증 정보와의 비교 결과다. 경로 허용·거부는 별도 정책이다. 상세 예제는 [RPKI의 판정 표](<04-지식/개발/인프라/09_RPKI_기초부터_운영까지.md#4-검증-상태를-직접-계산하기>)를 본다. [RFC 6811](https://www.rfc-editor.org/rfc/rfc6811) · [RFC 8481](https://www.rfc-editor.org/rfc/rfc8481)

## 4. 선택과 전달을 구분하는 용어

| 용어 | 역할 | 구분할 내용 |
|---|---|---|
| Import / Export Policy | 수신·재광고에 적용하는 규칙 | 어떤 경로를 받고 누구에게 알릴지 결정 |
| LOCAL_PREF | 같은 AS 내부에서 경로 선호도를 표현 | 동일 목적 대역 후보의 선택 과정에 사용 |
| RIB | Routing Information Base, 라우팅 정보 | 수신 후보·선택 결과 등 구현별 구조 구분 |
| FIB | Forwarding Information Base, 패킷 전달 정보 | 실제 전달에 사용되는 엔트리 |
| LPM | Longest Prefix Match, 가장 긴 일치 프리픽스 선택 | 서로 다른 길이의 설치된 경로 중 패킷 목적지와 가장 구체적으로 일치하는 것 선택 |

**BGP가 같은 프리픽스의 후보를 고르는 과정과, FIB에서 패킷의 목적지에 맞는 가장 긴 프리픽스를 찾는 과정은 다르다.** “AS_PATH가 짧은 경로가 모든 경우에 이긴다”는 설명으로 둘을 합치지 않는다. [RFC 4271 §9](https://www.rfc-editor.org/rfc/rfc4271#section-9)

## 5. 보안 사건 용어

| 용어 | 핵심 의미 | ROV만으로 해결되는가? |
|---|---|---|
| Prefix Hijack | 권한 없는 대역 광고로 경로를 잘못 유도 | VRP와 불일치하면 탐지 가능, 정책·배포 범위에 의존 |
| Subprefix Hijack | 더 구체적인 대역을 무단 광고 | 수용·설치되면 일부 목적지 트래픽이 해당 경로로 향할 수 있음 |
| Forged-origin Hijack | 허용된 origin ASN을 경로에 넣어 위조 | origin 일치만으로는 잡히지 않을 수 있음 |
| Route Leak | 의도한 정책 범위를 넘어 경로 재광고 | origin이 유지되면 Valid일 수 있음 |
| Blackhole | 트래픽이 폐기되거나 목적지에 도달하지 못하는 상태 | 공격·장애·의도된 방어를 맥락에 따라 구분 |

하이재킹의 예제는 [별도 문서](<04-지식/개발/인프라/11_BGP_하이재킹.md>)에서 설명한다. 경로 유출은 [RFC 7908](https://www.rfc-editor.org/rfc/rfc7908), origin 위조와 maxLength 관계는 [RFC 9319](https://www.rfc-editor.org/rfc/rfc9319)를 참고한다.

## 6. 한 문장으로 복습하기

**ROA를 Validator가 검증해 VRP를 만들고, 라우터가 RTR로 받아 ROV 상태를 판정하며, 운영 정책과 경로 선택을 거쳐 패킷 전달 정보에 반영한다.**

이 문장을 설명할 수 있으면 개별 제품 이름이나 설정 명령을 배우기 전에 필요한 개념 연결이 된 것이다.

*자료 확인: 2026-10-27. 도식은 학습용이며 특정 장비 내부 구현을 그대로 표현하지 않는다.*
