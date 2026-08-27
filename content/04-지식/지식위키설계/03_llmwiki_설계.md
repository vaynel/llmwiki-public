---
publish: true
tags: [지식, 지식위키설계]
---

# LLM 위키 시스템 설계 문서

> 개인 지식관리 + 선택적 공개 위키 + LLM 자동 정리
> 작성 대상 서버: mini PC / Ubuntu 24.04.2 LTS (RAM 15Gi, Disk 466G)

---

## 1. 목표

- **Obsidian을 파일 작성·관리 도구로** 사용하여 모든 지식을 한곳에 축적한다.
- **이동 중 웹 스크랩** → 지식이 자동으로 위키에 쌓이게 한다.
- **LLM(OpenAI, Claude Code)** 이 스크랩·노트를 자동으로 요약·태깅·연결하여 정리한다.
- **개인 문서(예: 가족 이야기)와 공개 문서를 명확히 구분**하여, 공개할 것만 세상에 노출한다.
- 개발·운영 자원을 최소화한다. git에 공개된 **오픈소스만 조합**하여 구축한다.

---

## 2. 핵심 원칙 — "권한으로 숨기기"가 아니라 "애초에 안 내보내기"

개인 정보 보호의 안전장치를 **접근 권한**이 아니라 **물리적 분리**로 구현한다.

| 구분      | 방식                                          | 안전성                     |
| ------- | ------------------------------------------- | ----------------------- |
| ❌ 나쁜 방식 | 다 공개하고, 개인 것만 "숨김" 표시                       | 실수 한 번 = 개인정보 노출        |
| ✅ 채택 방식 | 기본값 비공개, `publish: true` 표시한 것만 공개 (opt-in) | 표시 안 하면 = 안 나감. 실수해도 안전 |

**두 단계는 완전히 별개다:**

1. **LLM 정리** — vault 전체(공개+개인)에 적용. 전부 서버 내부에서만 실행되고 밖으로 안 나감.
2. **공개 게이트(`publish: true`)** — 오직 "공개 사이트로 내보낼지"만 결정.

> 결과: 본인은 Obsidian에서 정리된 **모든** 노트(개인 포함)를 본다.
> 세상은 `publish: true` 노트만 본다.
> 개인 문서 = "정리는 되지만 공개는 안 됨".

---

## 3. 전체 아키텍처

```
[폰 / 노트북에서 스크랩 · 작성]
   │  Obsidian Web Clipper (이동 중 웹 스크랩) + 직접 작성
   ▼
[Obsidian vault]
   │  Self-hosted LiveSync
   ▼
[CouchDB]  ─────────  전부 비공개. WireGuard 터널 안에서만 존재.
   │                   공인 인터넷·LAN 어디서도 안 보임.
   │
   ├──────────────────────────────┬────────────────────────────────┐
   ▼                              ▼                                ▼
[Claude Code                  [개인 노트는                    [publish:true 노트만]
 + obsidian-self-mcp]          여기서 멈춤 —                   export → git repo
 vault 직접 읽고               절대 밖으로                          │ git push (나가기만)
 요약 / 태깅 / 링크 정리        안 나감]                             ▼
 (공개·개인 전부 정리)                                    [Cloudflare / GitHub Pages]
                                                          Quartz v4 빌드 → 공개 위키
                                                          (전 세계 접근 가능)
```

**핵심:** 공개 사이트 호스팅 측(Cloudflare/GitHub Pages)은 vault 전체를 절대 못 본다.
git 저장소에는 `publish: true`로 걸러진 노트만 올라가고, 미니 PC는 외부 인바운드 연결을 하나도 받지 않는다 (git push만 나감).

---

## 4. 구성 요소 (전부 오픈소스, git)

| 역할 | 도구 | 저장소 / 출처 | 비고 |
|------|------|--------------|------|
| 웹 스크랩 | Obsidian Web Clipper | obsidian.md/clipper (공식) | iOS/Android, 로컬 Markdown 저장 |
| 동기화 백엔드 | CouchDB | apache/couchdb (Docker) | WireGuard 내부에만 bind |
| 동기화 플러그인 | Self-hosted LiveSync | `vrtmrz/obsidian-livesync` | E2E 암호화 지원 |
| VPN | WireGuard | 커널 내장 | 폰·노트북 피어 |
| LLM ↔ vault 브릿지 | obsidian-self-mcp | `suhasvemuri/obsidian-self-mcp` (MIT) | Obsidian 앱 없이 CouchDB 직접 접근 |
| 에이전트 | Claude Code | Anthropic | MCP 도구로 vault 조작 |
| 공개 위키 생성 | Quartz v4 | `jackyzha0/quartz` | ExplicitPublish 필터 |
| 공개 호스팅 | Cloudflare Pages / GitHub Pages | - | git push → 자동 빌드·배포 |

대안(참고): 공개 위키 생성은 Quartz 대신 Digital Garden 플러그인(`oleeskild/obsidian-digital-garden`, `dg-publish: true` 방식)도 동일 철학으로 사용 가능.

---

## 5. 데이터 흐름 (사용자 시나리오)

```
① 이동 중 좋은 글 발견
      │ 폰 Web Clipper로 클립 (+ OpenAI Interpret로 즉석 요약)
      ▼
② vault에 raw 클립 저장 → LiveSync → CouchDB
      │
③ Claude Code(스케줄/수동)가 신규 클립 감지
      │ 요약 정제 · 태그 부여 · 기존 노트와 위키링크 연결 · 중복 정리
      │ "공개 후보"면 사람에게 publish 제안 (자동으로 켜지 않음)
      ▼
④ 본인이 Obsidian에서 정리된 결과 확인 (공개·개인 전부)
      │ 공개할 노트만 publish: true 로 직접 전환
      ▼
⑤ export 잡이 publish:true 노트만 git repo로 내보냄 → push
      ▼
⑥ Cloudflare/GitHub Pages가 Quartz 빌드 → 공개 위키 갱신
```

---

## 6. 공개 / 비공개 구분 메커니즘 (상세)

### 6.1 노트 frontmatter 규약

```yaml
---
publish: false      # 기본값. 템플릿에 false를 넣어 둔다.
tags: [clip, ai]
created: 2026-08-08
---
```

- 공개하려면 `publish: true` 로 직접 변경.
- Obsidian 신규 노트 템플릿에 `publish: false` 를 기본 포함시켜, 실수로라도 공개되지 않게 한다.

### 6.2 다중 안전장치 (defense in depth)

1. **Export 단계 필터** — CouchDB → git으로 내보낼 때 `publish: true`인 노트만 추출. 개인 노트는 git repo에 물리적으로 존재하지 않음.
2. **Quartz ExplicitPublish 필터** — 빌드 시 `publish: true`가 아닌 파일은 렌더링에서 제외 (2차 방어선).
3. **폴더 ignorePatterns** — `private/`, `가족/`, `templates/` 등 폴더는 통째로 제외.
4. **호스팅 격리** — 공개 호스팅은 컴파일된 정적 HTML(`public/`)만 받고, 원본 vault는 절대 접근 불가.

### 6.3 (선택) 대상별 다중 뷰

나중에 "완전 공개 + 지인만 공개"처럼 여러 대상이 필요하면:

- 태그 기반 분기: `tags: [blog]` → 블로그 사이트, `tags: [brain]` → digital garden, 둘 다면 양쪽 발행.
- 하나의 vault에서 Quartz 사이트를 2개 뽑아 각각 다른 도메인에 배포.

> 초기에는 "공개 위키 / 개인 Obsidian" 이분법으로 시작하고, 필요 시 확장 권장.

---

## 7. LLM 역할 분담 (n8n 미사용)

| LLM | 실행 위치 | 역할 |
|-----|-----------|------|
| **OpenAI** | 폰 Web Clipper "Interpret" | 클립하는 순간 즉석 요약·핵심 추출·구조화 |
| **Claude Code** | 서버 (localhost) | 쌓인 노트 심층 정리: 요약 정제, 태깅, 위키링크 연결, 중복 정리, 공개 후보 제안 |

**안전 원칙:** LLM은 `publish: true`를 자동으로 켜지 않는다. LLM은 "공개해도 될 것 같다"고 **제안만** 하고, 최종 공개 스위치는 사람이 직접 넘긴다. (가족 프라이버시 최종 게이트를 사람이 쥔다.)

---

## 8. 서버 배치 및 자원

| 컴포넌트 | 배치 | 예상 부하 |
|----------|------|-----------|
| CouchDB | Docker, wg0에만 bind | 낮음 (개인 vault) |
| WireGuard | 호스트 커널 | 무시 가능 |
| obsidian-self-mcp | Python, 스케줄/수동 | 낮음 (간헐 실행) |
| Claude Code | 수동/스케줄 실행 | 실행 시에만 |
| Quartz 빌드 | **외부 호스팅에서 수행** | 서버 부하 ≈ 0 |

> RAM 15Gi 중 13Gi 여유, Disk 398G 여유 → 매우 넉넉. 병목 없음.

---

## 9. 보안 고려사항

- **네트워크 노출 최소화**: CouchDB·리버스프록시는 `wg0` 인터페이스에만 bind. 공인 IP/LAN 노출 0. 미니 PC는 인바운드 연결을 받지 않음 (git push만 아웃바운드).
- **CouchDB 계정**: admin 계정 강력한 비밀번호, LiveSync 전용 사용자 분리, `_security` 문서로 DB 접근 제한.
- **E2E 암호화**: LiveSync의 종단간 암호화 passphrase 활성화 → CouchDB에 평문 저장 방지.
- **TLS**: iOS Obsidian은 유효 인증서 HTTPS 요구. Let's Encrypt **DNS-01 챌린지**로 발급 (80/443 인터넷 개방 불필요).
- **WireGuard 피어 격리**: 필요 시 nftables로 특정 피어(예: 폰)가 민감 DB에 라우팅되지 않도록 제한.
- **공개 repo 점검**: export된 git repo는 정기적으로 `publish:true` 외 내용이 섞이지 않았는지 자동 검증(CI 린트) 추가 권장.
- **비밀정보 관리**: OpenAI/Anthropic API 키는 환경변수·시크릿 매니저로 관리, vault·git repo에 절대 커밋 금지.

---

## 10. 구축 로드맵 (권장 순서)

- [ ] **1단계** — WireGuard 서버 구성 (wg0, 폰·노트북 피어 conf)
- [ ] **2단계** — CouchDB Docker (wg0 bind, admin, CORS) + LiveSync 폰 동기화 검증
- [ ] **3단계** — Obsidian Web Clipper 폰 연결 + 클립 → CouchDB 흐름 확인
- [ ] **4단계** — obsidian-self-mcp 설치 + Claude Code MCP 등록 (vault 읽기/쓰기 테스트)
- [ ] **5단계** — LLM 정리 워크플로우 정립 (요약/태깅/링크, publish 제안)
- [ ] **6단계** — Quartz v4 fork + ExplicitPublish 필터 + export 잡 구성
- [ ] **7단계** — Cloudflare/GitHub Pages 연결 → 공개 위키 배포
- [ ] **8단계** — (선택) 다중 뷰, 공개 repo 린트 CI

---

## 부록 A. 주요 저장소 / 링크

- Obsidian Web Clipper: https://obsidian.md/clipper
- Self-hosted LiveSync: https://github.com/vrtmrz/obsidian-livesync
- obsidian-self-mcp: https://github.com/suhasvemuri/obsidian-self-mcp (glama.ai 등록)
- Quartz v4: https://github.com/jackyzha0/quartz
- Digital Garden(대안): https://github.com/oleeskild/obsidian-digital-garden