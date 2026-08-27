---
publish: true
tags: [지식, 지식위키설계]
---

# 🛠️ 개인 지식 위키 — Karakeep 하이브리드 구축계획서

> **채택안:** [설계서 v2](05_개인지식위키_LLM_Wiki_설계서_v2.md) §11 "Karakeep 하이브리드"
> **핵심 제약 반영:** ❌ **로컬 LLM(Ollama) 미사용** — 미니PC 사양 한계로, LLM 연산은 **외부 API(Gemini 무료 티어)**에 오프로딩하여 미니PC 부하를 0으로 만든다.
> **목표:** 1일(반나절 실작업) 내 MVP 구동.

---

## 0. 확정 아키텍처 (Ollama → 외부 API 버전)

```text
   [웹/모바일]
        │  브라우저 확장 · iOS/Android 앱 · 공유시트
        ▼
┌──────────── Karakeep (미니PC, Docker) ────────────┐
│  수집 · 아카이브 · 검색                             │
│  AI 태그/요약 → ☁️ Google Gemini API (외부, 무료)   │◀── 미니PC 연산 부하 0
└───────────────────────┬────────────────────────────┘
                         │  Karakeep REST API (tag=to-wiki)
                         ▼
        [bridge.py] (cron, 미니PC)   ★유일한 신규 개발
          Karakeep 항목 → 마크다운 → 위키 레포 00_Inbox/
                         │
        (선택) Obsidian에서 "✍️ 내 생각" 작성 (F2)
                         │
                 git commit & push
                         │
        GitHub Webhook → `quartz build` → https://wiki.example.com
```

**LLM 배치의 변화:** 미니PC엔 **모델을 올리지 않는다.** Karakeep이 태깅/요약이 필요할 때만 Gemini API를 호출하고, 결과 텍스트만 받는다. → CPU/RAM 부담 없음, GPU 불필요.

---

## 1. 사전 준비물 (Prerequisites)

| 항목 | 준비 내용 | 비고 |
| --- | --- | --- |
| **미니PC** | Docker + Docker Compose 설치 (`myserver`) | 기존 서버 재사용 |
| **도메인** | `karakeep.example.com`, `wiki.example.com` 서브도메인 | 보유 도메인 |
| **Cloudflare** | 위 두 서브도메인 A/CNAME 레코드, SSL **Full(Strict)** | 기존 설정 재사용 |
| **Nginx** | 443 → 내부 포트 리버스 프록시 | `80_blog/01` 방식 재사용 |
| **GitHub** | 위키용 신규 레포 `knowledge-wiki` | Webhook 연동 |
| **☁️ Gemini API 키** | Google AI Studio에서 **무료** 발급 | Ollama 대체 (§4) |
| **Node/Quartz** | 기존 블로그 환경 재사용 | `80_blog` 참고 |

> **사양 체크(중요):** `free -h`, `docker stats`로 여유 RAM 확인. Karakeep은 헤드리스 Chrome + Meilisearch가 메모리를 쓰므로, **최소 2GB(권장 4GB) 여유**를 확보한다. 부족하면 §3.3 경량화 옵션 적용.

---

## 2. 기술 스택 확정표

| 계층 | 도구 | 미니PC 부하 |
| --- | --- | --- |
| 수집 | Karakeep (Docker) | 중 (Chrome 크롤러) |
| **AI 정리** | **Google Gemini `gemini-2.5-flash` (외부 API)** | **0 (외부)** |
| 검색 | Meilisearch (Karakeep 내장) | 소~중 |
| 저장/형상관리 | Markdown + Git | 소 |
| 발행/학습웹 | Quartz (정적) | 소 |
| 연결 | `bridge.py` (cron) | 소 |

---

## 3. Phase 1 — Karakeep 설치 (미니PC)

### 3.1. 디렉토리 & compose

```bash
mkdir -p /root/karakeep && cd /root/karakeep
# 공식 compose 내려받기 (최신본은 docs.karakeep.app 확인)
curl -o docker-compose.yml https://raw.githubusercontent.com/karakeep-app/karakeep/main/docker/docker-compose.yml
```

### 3.2. `.env` 설정 (⚠️ Ollama 대신 Gemini)

```bash
# /root/karakeep/.env  — git에 커밋 금지
NEXTAUTH_SECRET=$(openssl rand -base64 36)
MEILI_MASTER_KEY=$(openssl rand -base64 36)
NEXTAUTH_URL=https://karakeep.example.com
DATA_DIR=/data
DISABLE_SIGNUPS=true            # 최초 계정 생성 후 true 권장

# ── AI: 로컬(Ollama) 대신 외부 OpenAI 호환 엔드포인트 = Gemini 무료 ──
OPENAI_API_KEY=<Google_AI_Studio에서_발급한_키>
OPENAI_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai/
INFERENCE_TEXT_MODEL=gemini-2.5-flash
INFERENCE_IMAGE_MODEL=gemini-2.5-flash
INFERENCE_LANG=korean           # 태그/요약 한국어
```

> **핵심:** `OLLAMA_BASE_URL`은 **설정하지 않는다.** `OPENAI_API_KEY`+`OPENAI_BASE_URL`만 있으면 Karakeep은 해당 외부 엔드포인트로 태깅/요약을 보낸다. (env 이름은 버전마다 다를 수 있으니 [환경변수 문서](https://docs.karakeep.app/configuration/environment-variables/) 확인)

### 3.3. 🪶 경량화 옵션 (미니PC 사양 대응)

`.env`에 추가하여 부하를 낮춘다:

```bash
CRAWLER_NUM_WORKERS=1           # 동시 크롤 1개로 제한
CRAWLER_DOWNLOAD_VIDEO=false    # 영상 다운로드 비활성(기본값)
CRAWLER_FULL_PAGE_ARCHIVE=false # 전체페이지 스냅샷 비활성(용량↓)
CRAWLER_FULL_PAGE_SCREENSHOT=false
```

> 나중에 여유가 되면 하나씩 켠다. 우선은 **텍스트+요약 위주 최소 구성**으로 뜨는 게 목표.

### 3.4. 기동 & 확인

```bash
docker compose up -d
docker compose ps
docker stats --no-stream        # RAM 사용량 점검 (여유 없으면 §3.3 강화)
```

---

## 4. Phase 2 — Gemini API 연결 & 검증

1. **키 발급:** Google AI Studio 접속 → API Key 생성(무료). → `.env`의 `OPENAI_API_KEY`에 입력 후 `docker compose up -d` 재적용.
2. **검증:** Karakeep 웹 UI에서 링크 하나 저장 → 잠시 후 **AI 태그 + 요약이 자동 생성**되는지 확인.
3. **무료 한도 인지:** 무료 티어는 분당 요청(RPM)·일일 한도가 있다. 개인 수집량이면 충분하나, 대량 일괄 저장 시 `429`가 날 수 있음 → 천천히 저장하거나 §9 대응.

> **프라이버시 주의:** 저장 콘텐츠 본문이 요약을 위해 **외부(Google)로 전송**된다. 영어/IT/독서/성경 자료는 일반적으로 문제없지만, **민감 정보는 스크랩 대상에서 제외**한다.

---

## 5. Phase 3 — 외부 접속 (도메인 · 프록시 · 클라이언트)

### 5.1. Nginx 리버스 프록시 (`karakeep.example.com` → `localhost:3000`)

```nginx
server {
    server_name karakeep.example.com;
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    error_page 497 301 =307 https://$host$request_uri;  # Cloudflare Full(Strict)
}
```

- Cloudflare에 `karakeep` 서브도메인 레코드 추가, SSL **Full(Strict)** 유지(기존 `80_blog/01` 트러블슈팅 참고).

### 5.2. 클라이언트 설치

- **브라우저 확장:** Chrome/Firefox/Safari 확장 설치 → 서버 주소 `https://karakeep.example.com` + API 키 연결.
- **모바일 앱:** iOS/Android 앱 설치 → 동일 서버 로그인 → **공유시트로 어디서나 저장**(F3 크로스플랫폼 충족).
- **분류 규칙:** 저장 시 위키로 보낼 항목엔 `to-wiki` 태그(또는 전용 리스트) 지정 → §7 Bridge가 이 태그만 가져감.

---

## 6. Phase 4 — 위키 발행 파이프라인 (Quartz)

> `80_blog/01`(Webhook 구축) · `80_blog/02`(새 프로젝트 연결) 가이드를 **그대로 재사용**한다.

1. GitHub에 `knowledge-wiki` 레포 생성, 카테고리 폴더 구성:
   ```text
   knowledge-wiki/
   ├── 00_Inbox/     10_English/   20_IT/
   ├── 30_Bible/     40_Reading/   90_Templates/
   └── quartz.config.ts
   ```
2. 미니PC `/root/blog/content`에 `git clone` → `webhook_server.py`의 `PROJECTS`에 `knowledge-wiki` 항목 추가(시크릿·경로·`npx quartz build`).
3. GitHub Webhook 등록(Payload `https://blog.example.com/webhook`, Push 이벤트) → Ping `200 OK` 확인.
4. Cloudflare에 `wiki.example.com` 레코드 + Nginx로 Quartz 결과물 서빙.
5. `systemctl restart quartz-webhook` 후 `journalctl -u quartz-webhook -f`로 빌드 모니터링.

---

## 7. Phase 5 — Bridge 스크립트 (Karakeep → 위키 레포)

미니PC에서 cron으로 주기 실행. `to-wiki` 태그 신규 항목을 마크다운으로 떨구고 자동 커밋한다.

```python
# /root/karakeep/bridge.py   (단방향: Karakeep → vault)
# 1) Karakeep REST API: tag=to-wiki AND NOT exported 항목 조회
#    GET https://karakeep.example.com/api/v1/bookmarks?...   (Authorization: Bearer <API키>)
# 2) 각 항목 → 프론트매터(title/source/tags/summary/clipped) + 본문(요약+링크) 마크다운
# 3) 태그/리스트 → 폴더 라우팅: 10_English / 20_IT / 30_Bible / 40_Reading (없으면 00_Inbox)
# 4) knowledge-wiki 레포에 파일 저장
# 5) git add/commit/push  → 기존 Webhook이 Quartz 빌드 트리거
# 6) 해당 Karakeep 항목에 'exported' 태그 부여 (재임포트 방지)
```

```yaml
# 프론트매터 출력 예시
---
title: "{Karakeep title}"
source: "{원본 URL}"
clipped: "{저장일}"
category: "{매핑된 카테고리}"
tags: [{Karakeep AI 태그들}]
status: raw          # 이후 Obsidian에서 내 문서 작성 시 mynote
---
> **AI 요약:** {Gemini가 만든 요약}

## ✍️ 내 생각 / 정리
(여기를 채우면 나만의 문서가 된다)
```

```bash
# cron 등록: 15분마다
crontab -e
*/15 * * * * /usr/bin/python3 /root/karakeep/bridge.py >> /root/karakeep/bridge.log 2>&1
```

> API 키는 `.env`/환경변수로 주입, **레포에 커밋 금지**. Bridge는 미니PC 내부에서만 호출.

---

## 8. Phase 6 — E2E 테스트 & 검증 체크리스트

**E2E 시나리오:** 모바일 앱에서 아티클 저장(`to-wiki` 태그) → Karakeep이 Gemini로 태그·요약 생성 → 15분 내 `bridge.py`가 `00_Inbox/`에 마크다운 커밋 → Webhook → `wiki.example.com`에 페이지 반영 → Obsidian에서 "내 생각" 작성 → 재발행.

- [ ] Karakeep 웹/앱 로그인 및 링크 저장 정상
- [ ] Gemini AI 태그·요약 자동 생성 확인
- [ ] `karakeep.example.com` HTTPS 접속(Cloudflare Full Strict) 정상
- [ ] `bridge.py` 수동 실행 시 마크다운 생성 + 커밋 성공
- [ ] cron 자동 실행 로그 확인(`bridge.log`)
- [ ] `wiki.example.com`에 페이지 발행 + 그래프/검색 동작
- [ ] `exported` 태그로 중복 임포트 없음
- [ ] `docker stats` RAM 여유 확인(장시간 안정성)

---

## 9. 1일 구축 시간표 (One-Day Plan)

| 시간 | Phase | 작업 |
| --- | --- | --- |
| 09:00–09:30 | 0 | 사양 점검, DNS(karakeep/wiki) 레코드, Gemini 키 발급 |
| 09:30–10:30 | 1 | Karakeep compose + `.env`(경량화 포함) 기동 |
| 10:30–11:00 | 2 | Gemini 연결 + AI 태깅 검증 |
| 11:00–12:00 | 3 | Nginx 프록시 + 브라우저 확장 + 모바일 앱 |
| 13:00–15:00 | 4 | knowledge-wiki 레포 + Quartz + Webhook + `wiki.example.com` |
| 15:00–17:30 | 5 | `bridge.py` 작성 + cron 등록 |
| 17:30–18:00 | 6 | E2E + 체크리스트 |

> 막히면 우선순위: **Karakeep 수집 → Bridge → Quartz 발행** 최소 경로부터 살리고, AI 태깅 튜닝·경량화는 후순위.

---

## 10. 리스크 & 대응 (Risks)

| 리스크 | 영향 | 대응 |
| --- | --- | --- |
| **미니PC RAM 부족** | Karakeep 불안정 | §3.3 경량화, Chrome 크롤 워커 1개, 전체보관 off |
| **Gemini 무료 한도(429)** | 대량 저장 시 태깅 실패 | 저장 속도 조절, 실패분 재시도, 필요 시 OpenRouter 무료 모델로 `OPENAI_BASE_URL` 교체 |
| **외부 전송 프라이버시** | 민감정보 노출 | 민감 자료 스크랩 제외, 개인용 한정 |
| **이중 저장소 동기화** | 중복/누락 | Bridge 단방향 + `exported` 태그, vault=영구 원천 원칙 |
| **Karakeep 외부 노출** | 무단 접근 | `DISABLE_SIGNUPS=true`, 강한 비밀번호, (선택) Cloudflare Access |
| **API 키 유출** | 비용/오남용 | `.env`만 사용, git 제외, 키 회전 |

---

## 11. LLM 제공자 대안 (Ollama 불가 시 무료 옵션)

| 제공자 | `OPENAI_BASE_URL` | 모델 예시 | 특징 |
| --- | --- | --- | --- |
| **Google Gemini** ✅ | `https://generativelanguage.googleapis.com/v1beta/openai/` | `gemini-2.5-flash` | 무료 한도 넉넉, 한국어 양호 |
| OpenRouter | `https://openrouter.ai/api/v1` | 무료 티어 모델들 | 여러 모델 스위칭 |
| Groq | `https://api.groq.com/openai/v1` | Llama 계열 | 매우 빠름, 무료 |

> 셋 다 OpenAI 호환이라 **`.env`의 base_url·모델명·키만 교체**하면 즉시 전환. 미니PC엔 아무 것도 설치하지 않는다.

---

**한 줄 평:** > "무거운 계산은 남의 GPU에게, 내 미니PC는 수집과 발행만 — 사양이 낮아도 위키는 돈다."
