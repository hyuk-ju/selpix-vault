---
type: idea-card
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: idea-discussion
verified_by: agent
sources:
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/AGENTS.md
  - https://www.reddit.com/r/finalcutpro/comments/1so6gob/
  - https://www.reddit.com/r/finalcutpro/comments/1slyaqh/
  - https://github.com/mfahsold/montage-ai
  - https://github.com/DareDev256/fcpxml-mcp-server
created: 2026-05-10
reviewed_at: 2026-05-10
---
# Shareable Demo Evidence Pack

> `mv-brain demo` 결과를 포트폴리오/영업용으로 바로 보여줄 수 있는 정적 증거 패키지로 묶는 1-7일 웨지.

## 요약

MV BRAIN은 이미 no-footage/no-provider demo, clip browser, output flow, FCPXML rough cut을 README에서 주장한다. 다음 웨지는 새 AI 기능이 아니라 **증거를 공유 가능한 형태로 고정**하는 것이다.

MVP는 `demo` 실행 후 생성되는 샘플 메타데이터와 FCPXML을 기반으로 `docs/demo-evidence/` 또는 `output/demo_evidence/`에 아래를 만드는 작은 산출물이다.

- 샘플 클립 목록 요약 JSON/Markdown
- 생성된 FCPXML 파일 링크와 검증 결과
- UI 캡처 가이드 또는 정적 screenshot placeholder
- 60초 포트폴리오 설명 스크립트
- “provider 호출 없음 / 실제 미디어 없음 / Qdrant 불필요” 실행 조건 명시

코드 변경은 별도 구현 요청이 있을 때만 한다. 이 카드는 다음 개발 액션을 정하는 용도다.

## 근거

### 내부 신호

- `/home/dev/mv-brain/README.md`는 `mv-brain demo`가 `data/demo/clips.json`와 `output/demo_roughcut.fcpxml`을 생성해 UI clip browser/output flow를 즉시 보여준다고 설명한다.
- 같은 README는 현재 제품이 기술 early user용이며, Visual search quality는 실제 Gemini frame analysis 여부에 좌우된다고 제한을 명시한다. 따라서 “실제 AI 품질”보다 “검증 가능한 로컬 데모 흐름”을 먼저 보여주는 편이 안전하다.
- 오늘 안전 체크: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과.
- 최근 아이디어들은 Windows MP4 preview, FCPXML inspector, cutlist clip-pack, search benchmark, provider receipt 등 기능별 proof가 많다. 하지만 이를 한 번에 포트폴리오 증거로 묶는 카드/팩은 최근 중복이 없다.

### 외부 신호

- Reddit `r/finalcutpro`의 Bowdler 글은 “local AI video processing app”이 클라우드 도구/구독 피로에서 출발했고, 사용자 피드백 후 기능을 개선했다는 사례다. 로컬 AI 비디오 도구는 기능보다 “보여지는 개선 증거”가 커뮤니티 설득에 중요하다.  
  URL: https://www.reddit.com/r/finalcutpro/comments/1so6gob/
- Reddit `r/finalcutpro`의 Papercut Pro 글은 인터뷰/다큐 편집 피로에서 출발한 text-based FCP workflow 사례다. FCP 사용자에게는 추상 AI 주장보다 실제 편집 워크플로우와 연결되는 증거가 설득력 있다.  
  URL: https://www.reddit.com/r/finalcutpro/comments/1slyaqh/
- GitHub 검색에서 `mfahsold/montage-ai`는 local-first AI video editor가 transcript-based editing, beat-synced cuts, OTIO/EDL export를 강조한다. 즉 로컬-first 편집 도구 경쟁축은 “실행 가능한 demo + 편집툴 handoff”다.  
  URL: https://github.com/mfahsold/montage-ai
- `DareDev256/fcpxml-mcp-server`는 FCPXML을 자연어/AI 도구 연결면으로 포지셔닝한다. MV BRAIN의 FCPXML handoff를 증거 팩 안에서 강조할 이유가 있다.  
  URL: https://github.com/DareDev256/fcpxml-mcp-server

## 후보 비교

- 후보 A: Shareable Demo Evidence Pack — 승인. 기존 demo/README/FCPXML 결과를 포트폴리오 증거로 묶는 가장 빠른 산출물.
- 후보 B: UI screenshot automation — 관찰. 시각적으로 좋지만 브라우저/fixture 안정화가 먼저 필요할 수 있다.
- 후보 C: Public demo landing README rewrite — 관찰. 카피 수정만으로는 proof가 약하다.
- 후보 D: 더 깊은 FCPXML feature 추가 — 폐기. 최근 FCPXML/EDL 관련 카드와 중복되고 1일 증거성이 낮다.

## 역할별 토론

- idea-scout: 최근 카드가 기능 proof에 치우쳤으므로 오늘은 “보여줄 수 있는 증거 번들”이 다음 병목이다.
- builder: `mv-brain demo` 산출물, README, 기존 no-media 테스트를 재사용하면 작은 스크립트/문서로 끝낼 수 있다. provider, Qdrant, 실제 미디어가 필요 없다.
- market: 포트폴리오/리드마그넷에서는 “설치 후 어떤 화면과 파일이 나오나”가 가장 먼저 궁금하다. 공유 가능한 evidence pack은 GitHub README, 블로그, DM 영업에 바로 쓸 수 있다.
- critic: 실제 UI 캡처 자동화까지 욕심내면 환경 의존성이 생긴다. MVP는 정적 파일/검증 로그/캡처 가이드로 제한해야 한다.
- curator: 승인. 새 AI 기능보다 신뢰와 데모 전환율을 높이는 웨지이며 최근 카드와 직접 중복되지 않는다.

## 점수

| 항목 | 점수 | 이유 |
|---|---:|---|
| portfolio_fit | 5 | 실행 결과와 파일 산출물을 보여줘 개발 역량이 명확하다. |
| revenue_fit | 4 | 템플릿/데모 패키지/컨설팅 리드마그넷으로 연결 가능하다. |
| build_ease | 5 | 기존 demo 산출물과 README를 재사용한다. |
| proof_speed | 5 | 1일 내 Markdown/JSON/검증 로그 산출 가능하다. |
| mv_brain_synergy | 5 | no-provider demo, FCPXML handoff, UI output flow를 직접 강화한다. |
| risk_inverse | 5 | 실제 미디어, provider 호출, Qdrant mutation 없이 가능하다. |
| **합계** | **29/30** | 승인 기준 22점 이상, fatal flaw 없음. |

## MVP 계획

1. `mv-brain demo` 산출물을 기준으로 evidence manifest 초안을 정의한다: clips count, roughcut path, FCPXML validation result, no-provider 조건.
2. `docs/demo-evidence/README.md` 또는 `output/demo_evidence/README.md` 형태의 정적 포트폴리오 페이지를 만든다.
3. README의 “first-run demo” 섹션에서 evidence pack으로 연결한다.

## 리스크와 제한

- 자동 스크린샷은 OS/browser 의존성이 있으므로 MVP에는 넣지 않는다.
- 실제 편집 품질을 과장하지 않는다. “샘플 fixture로 제품 흐름을 보여주는 증거”로 표현한다.
- provider receipt, MP4 preview, FCPXML inspector 등 기존 카드와 결합 가능하지만 첫 구현은 독립된 작은 artifact여야 한다.

## 다음 1개 액션

- 구현 요청 시 첫 작업은 `mv-brain demo` 결과를 읽어 `output/demo_evidence/manifest.json`와 `output/demo_evidence/README.md`를 생성하는 no-provider 명령/스크립트 설계부터 시작한다.

## 관련 문서

- [[MV-BRAIN-프로젝트-브리프]]
- [[2026-05-09-provider-data-receipt]]
- [[2026-05-08-transcript-roughcut-sidecar]]
- [[2026-05-05-agent-readable-cutlist-clip-pack]]
- [[2026-05-03-windows-mp4-roughcut-preview]]
