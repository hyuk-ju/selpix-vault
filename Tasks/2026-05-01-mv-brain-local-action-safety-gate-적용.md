---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-05-01-local-action-safety-gate.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프.md
created: 2026-05-01
reviewed_at: 2026-05-01
---
# MV BRAIN Local Action Safety Gate 적용안

## 적용 판정
`apply` — 오늘 승인 아이디어인 [[Research/Ideas/2026-05-01-local-action-safety-gate|Local action safety gate]]는 MV BRAIN의 `local-first`, `BYO API key`, `FCPXML output` 포지션을 신뢰 가능한 포트폴리오 근거로 바꾸는 작은 작업이다.

## 현재 확인
- 대상 프로젝트: `/home/dev/mv-brain`
- repo 상태: `main...origin/main`, working tree clean
- 최근 커밋: `3604db7 chore: refresh UI lockfile audit`, `393aee3 chore: initial public-ready release`
- 안전 점검: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과
- 범위 제한: 이 태스크는 문서/테스트/작은 안전 규칙 설계용이며, 이 크론에서는 코드 수정·GitHub issue·branch·PR을 만들지 않는다.

## 내부 토론
### builder
1~3일짜리 적용으로 충분하다. 먼저 `docs/security.md` 또는 README security 섹션 초안을 만들고, 그 다음 코드 변경이 필요하면 `output_name` slug/허용 확장자/path traversal/CORS 기본값 테스트를 작은 단위로 추가하면 된다. 실제 미디어 ingest, provider call, Qdrant mutation은 필요 없다.

### market
포트폴리오에서 “로컬 AI 편집툴”은 기능보다 신뢰가 먼저다. API 키와 로컬 파일을 다루는 앱이 어떤 action을 허용하고 어떤 경로를 막는지 문서화하면 technical editor, 협업자, 잠재 고객에게 설치 장벽을 낮춘다.

### critic
보안 작업이 과해지면 데모 완성보다 느려질 수 있다. OAuth, 계정, 복잡한 인증 시스템은 금지하고, “localhost 전제 + CORS 기본값 + 상태 변경 action 목록 + output 파일명/경로 제한”까지만 다룬다. hostile network에서도 안전하다는 과장 표현은 쓰지 않는다.

### curator
`apply`로 전환한다. 오늘 아이디어는 최근 demo/FCPXML 아이디어와 중복되지 않고, 작은 문서/테스트 산출물로 바로 증명 가능하다.

## 1-3 step 적용안
1. `docs/security.md` 초안 작성: local-only server assumption, allowed CORS origins, state-changing actions(`/api/settings`, `/api/output/roughcut`, 향후 ingest/open/import), output path/filename rules만 1페이지로 정리한다.
2. 테스트 후보를 좁힌다: path traversal 차단, unsafe output name slug/거부, 허용 확장자, CORS 기본 origin을 no-media pytest 또는 API unit test로 확인할 수 있는지 점검한다.
3. README에는 “secure product”가 아니라 “local-first guardrails” 섹션으로 연결하고, demo flow에서 사용자가 어떤 파일이 생성되는지 명시한다.

## 완료 기준
- `docs/security.md` 또는 README security 섹션 초안이 생긴다.
- 상태 변경 action 목록과 output path/filename 규칙이 문서에 명시된다.
- 최소 1개 이상의 no-media 테스트 후보가 구체적 파일/함수 수준으로 정리된다.
- 작업 후 `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server`가 계속 통과한다.

## 관련 문서
- [[Research/Ideas/2026-05-01-local-action-safety-gate|Local action safety gate]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Research/Ideas/2026-04-30-fcpxml-relink-preflight-manifest|FCPXML relink preflight manifest]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
