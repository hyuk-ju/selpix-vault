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
  - /home/dev/mv-brain/mv_brain/api/main.py
  - /home/dev/mv-brain/mv_brain/api/routers/output.py
  - /home/dev/mv-brain/ui/src/pages/ChatPanel.tsx
  - https://owasp.org/API-Security/editions/2023/en/0xa8-security-misconfiguration/
  - https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/
  - https://portswigger.net/web-security/cors
created: 2026-05-01
reviewed_at: 2026-05-01
---
# Local action safety gate

## 한 줄 요약
MV BRAIN의 로컬 웹 UI가 `settings 저장`, `roughcut 생성`, `output 다운로드` 같은 실제 동작을 수행할 때, “로컬 전용 + 의도 확인 + 파일 경로 제한”을 작은 safety gate로 묶어 포트폴리오에서 신뢰를 보여주는 1~3일짜리 보안/제품 개선 아이디어다.

## 왜 지금인가
- README는 MV BRAIN을 `local-first`, `BYO model key`, `editor-in-the-loop`, `FCPXML-first` 제품으로 설명한다. 로컬 앱이라도 API 키 저장, 출력 파일 생성, 로컬 파일 다운로드가 있으면 사용자는 “내 컴퓨터에서 무엇을 건드리는가”를 먼저 확인한다.
- 현재 코드 확인 결과 `mv_brain/api/main.py`는 CORS origin을 기본적으로 `127.0.0.1/localhost`로 제한하고 있어 좋은 출발점이다. 다만 `POST /api/settings`는 body를 받아 settings를 저장하고, `ChatPanel.tsx`는 선택 클립으로 `POST /api/output/roughcut`을 호출한다. 즉 destructive까지는 아니어도 “상태 변경 action”이 이미 있다.
- `mv_brain/api/routers/output.py`의 download endpoint는 absolute path와 `..`를 차단한다. 이 장점을 확장해 roughcut output name slug, 허용 확장자, action confirmation, demo-safe mode를 한 묶음으로 문서화/테스트하면 실무 신뢰가 올라간다.
- 외부 신호: OWASP API Security 2023은 Security Misconfiguration과 Broken Function Level Authorization을 별도 리스크로 다룬다. 로컬 앱도 HTTP API를 띄우는 순간 CORS, action 권한, 불필요한 method 노출을 설명해야 한다.
- 외부 신호: PortSwigger Web Security Academy는 CORS를 cross-origin resource sharing 보안 주제로 다룬다. MV BRAIN이 localhost UI/API 구조를 갖기 때문에 “허용 origin이 왜 좁은지”를 README/테스트로 보여주는 것이 포트폴리오 설득 포인트가 된다.
- 안전 점검: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과, `ui`에서 `npm run build` 통과. 따라서 아이디어는 깨진 빌드 복구가 아니라 작고 검증 가능한 product hardening으로 잡을 수 있다.

## 선택 전 후보 검토
- 후보 A: 60초 demo story bundle — 오늘 daily note와 맞지만 2026-04-28 no-media demo, 2026-04-30 preflight 카드와 겹친다.
- 후보 B: Output page metadata polish — preflight manifest 아이디어와 중복될 위험이 크다.
- 후보 C: Local action safety gate — 최근 아이디어들과 다르게 “보안/신뢰” 축을 강화하고, README의 local-first 주장과 직접 연결된다.
- 후보 D: 실제 FCP import 검증 — proof는 강하지만 macOS/FCP가 필요해 daily idea 범위에 무겁다.

## 내부 토론
### idea-scout
MV BRAIN은 이제 `demo`, FCPXML, search, UI 흐름이 어느 정도 설명된다. 다음으로 외부인이 믿고 실행할 수 있게 만드는 문제는 “로컬 서버가 내 파일/API 키/출력물을 안전하게 다루는가”이다. 특히 로컬 웹 UI가 AI provider 설정과 roughcut 생성 버튼을 갖고 있으므로, 작은 safety gate는 제품 신뢰와 개발자 포트폴리오 둘 다에 맞는다.

### builder
MVP는 1~3일로 충분히 좁힐 수 있다.
1. 상태 변경 API를 `safe action` 목록으로 분류한다: `/api/settings`, `/api/output/roughcut`, 향후 ingest/open/import 계열.
2. roughcut `output_name`을 slug/타임스탬프 whitelist로 고정하고, output listing/downloading은 허용 확장자와 output dir 상대경로만 유지한다.
3. README 또는 `docs/security.md`에 “local-first safety model”을 1페이지로 추가하고, pytest로 path traversal/unsafe output name/CORS origin 기본값을 확인한다.

실제 미디어 ingest, provider call, Qdrant mutation, Final Cut Pro 실행은 필요 없다. 기존 `output.py`, `main.py`, `ChatPanel.tsx`, 테스트 fixture만 보면 된다.

### market
타깃은 local AI editor tool을 설치해볼 기술형 편집자와 개발자 채용/협업 관찰자다. 수익 경로는 “보안 기능 자체 판매”가 아니라:
- local-first AI tool setup 컨설팅에서 신뢰 증거
- GitHub README의 security posture 섹션
- BYO API key 제품의 도입 장벽 감소
- FCPXML/로컬 파일 처리 워크플로를 설명하는 technical writeup
으로 좁게 잡는다.

### critic
가장 큰 실패 이유는 보안 작업이 데모 영상처럼 화려하지 않고, 과하면 제품 개발 속도를 늦춘다는 점이다. 따라서 OAuth/계정/복잡한 auth 시스템을 넣지 않는다. “로컬 action safety gate”의 범위를 slug output names, local-only CORS defaults, confirmation copy, path tests, security doc으로 제한해야 한다. 또 로컬 앱에 완전한 웹 보안을 약속하면 안 된다. README에는 “local-first default와 guardrails”라고 말해야지 “secure by default for hostile networks”라고 말하면 과장이다.

### curator
승인. 최근 아이디어가 demo/search/FCPXML proof에 집중되어 있었기 때문에 오늘은 중복을 피하려면 trust/hardening 축이 맞다. 내부 코드에 실제 상태 변경 endpoint와 CORS 설정이 있고, 외부 보안 기준도 근거가 된다. 범위를 작은 테스트/문서/slug gate로 제한하면 fatal flaw가 없다.

## 점수
| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 4 | FastAPI, CORS, path traversal test, local-first security model이 개발자에게 잘 보임 |
| revenue_fit | 3 | 직접 과금 기능은 아니지만 BYO-key/local setup 신뢰를 높여 컨설팅·유료 설치 리드에 도움 |
| build_ease | 4 | 기존 API/UI 주변의 문서·테스트·slug 제한 중심, 새 서비스 불필요 |
| proof_speed | 4 | pytest 결과, README security 섹션, before/after API behavior로 1~3일 proof 가능 |
| mv_brain_synergy | 5 | local-first/BYO key/FCPXML output 신뢰를 직접 강화 |
| risk_inverse | 5 | provider, media ingest, Qdrant mutation 없이 로컬 코드와 테스트로 검증 가능 |
| **합계** | **25/30** | approved |

## Verdict
`approved` — 총점 25/30, fatal flaw 없음. 단, 복잡한 인증 시스템으로 키우지 말고 “로컬 action safety gate + path/CORS/slug tests + README security model”로 제한한다.

## 다음 1개 액션
`/home/dev/mv-brain`에서 `docs/security.md` 초안을 먼저 만든다는 작업 메모를 준비한다. 포함 범위는 4개만 둔다: local-only server assumption, CORS allowed origins, state-changing actions list, output path/filename rules. 코드 변경은 별도 요청 전까지 하지 않는다.

## 관련 문서
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Research/Ideas/2026-04-30-fcpxml-relink-preflight-manifest|FCPXML relink preflight manifest]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
