---
type: research
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - https://github.com/anthropics/claude-code/releases/tag/v2.1.123
  - https://github.com/openai/codex/releases/tag/rust-v0.125.0
  - https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.0
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.71
  - https://github.com/cline/cline/releases/tag/v3.81.0
  - https://github.com/letta-ai/letta-code
  - https://github.com/trycua/cua
  - https://github.com/marras0914/cordon
  - https://github.com/toabctl/aichronicles
  - https://github.com/obra/superpowers
  - https://github.com/hesreallyhim/awesome-claude-code
  - https://github.com/czlonkowski/n8n-mcp
  - https://github.com/anthropics/claude-code/issues/49363
  - https://news.ycombinator.com/
  - https://reddit.com/r/ClaudeAI/
  - https://reddit.com/r/Cursor/
  - https://reddit.com/r/LocalLLaMA/
created: 2026-04-29
reviewed_at: 2026-04-29
---

# AI 코딩 빠른 레이더 — 2026-04-29 15:02 KST

## 핵심 3줄
1. 이번 신호의 중심은 “더 똑똑한 단일 에이전트”보다 **권한/검증/세션복구/메모리**다. Codex, Playwright MCP, Cordon, Letta Code, aiChronicles가 같은 방향을 가리킨다.
2. 공식 릴리스 중 바로 유용한 것은 Codex `rust-v0.125.0`의 permission profile/session plumbing, Playwright MCP `v0.0.71`의 network/body 검사 개선, Claude Code `v2.1.123`의 OAuth 401 루프 수정이다.
3. Reddit/HN에서는 에이전트가 DB를 지우는 류의 공포담과 MCP 보안 게이트가 동시에 뜬다. 우리 자동화는 “쓰기 작업 전 승인/드라이런/체크포인트”를 더 전면에 둬야 한다.

## 주목할 것

### 1. OpenAI Codex `rust-v0.125.0` — permission/session plumbing 강화
- 링크: https://github.com/openai/codex/releases/tag/rust-v0.125.0
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: `openai/codex`, stars 78,684, pushed_at 2026-04-29T05:54:58Z, maintained yes, Apache-2.0
- 왜 중요: permission profiles가 TUI/session/MCP sandbox/shell escalation/app-server API에 걸쳐 round-trip 되고, resume/fork 및 tracing도 강화됐다. 장기 작업과 크론형 agent 실행에서 “어떤 권한으로 무엇을 했는지”를 남기기 쉬워진다.
- 바로 할 일: Hermes 작업 로그 포맷에 `permission_profile`, `dry_run`, `checkpoint_path`, `escalation_reason` 필드를 추가하는 작은 스펙을 만들기.

### 2. Playwright MCP `v0.0.71` — 브라우저 검사/네트워크 evidence 개선
- 링크: https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.71
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: `microsoft/playwright-mcp`, stars 31,732, pushed_at 2026-04-27T20:52:22Z, maintained yes, Apache-2.0
- 왜 중요: `browser_network_requests`에 `responseBody`, `responseHeaders` 옵션이 추가됐다. 공개 웹 리서치나 SPA 조사에서 “보이는 화면”과 “실제 API 응답”을 함께 증거로 남길 수 있다.
- 바로 할 일: mv-brain 데모/문서 검증용으로 “브라우저 MCP로 공개 문서/릴리스 페이지를 열고 evidence JSON 저장” 패턴만 추출. 로그인/스크래핑 자동화는 금지.

### 3. Cordon — MCP tool-call 보안 게이트/HITL 승인
- 링크: https://github.com/marras0914/cordon
- HN 신호: Show HN: Cordon – Security gateway for MCP tool calls with HITL approvals
- 출처/신뢰도: GitHub/HN, medium
- GitHub 검증: `marras0914/cordon`, stars 0, pushed_at 2026-04-28T20:37:04Z, maintained recent, MIT, description 미기재
- 왜 중요: 아직 초기지만 방향은 중요하다. MCP 도구가 파일/브라우저/DB/결제/API에 붙을수록 “도구 호출 전 승인 정책”이 핵심 안전장치가 된다.
- 바로 할 일: 설치하지 말고, Hermes runbook에 `read-only`, `dry-run`, `destructive`, `credential-touching` 4단계 승인 라벨만 먼저 도입.

### 4. Letta Code + aiChronicles — 메모리/세션 회수 쪽 신호
- 링크: https://github.com/letta-ai/letta-code, https://github.com/toabctl/aichronicles
- 출처/신뢰도: GitHub/Reddit trend, medium-high for Letta, low-medium for aiChronicles
- GitHub 검증: `letta-ai/letta-code`, stars 2,394, pushed_at 2026-04-29T05:59:27Z, Apache-2.0. `toabctl/aichronicles`, stars 0, pushed_at 2026-04-29T06:00:16Z, Apache-2.0.
- 왜 중요: “채팅 로그 전체를 다시 먹이기” 대신 세션 요약, 실패 패턴, 작업 상태를 구조화해 다음 실행에 넘기는 흐름이 강해지고 있다.
- 바로 할 일: mv-brain/Hermes에 외부 벡터DB를 붙이기보다, 먼저 `RUN_STATE.md` 또는 SQLite 한 파일로 `goal/current_state/last_failure/next_safe_action`을 저장하는 경량 패턴 검토.

### 5. CUA / computer-use runtime — 화면/앱 조작은 커지지만 안전 범위가 필요
- 링크: https://github.com/trycua/cua
- HN 신호: Show HN: Drive any macOS app in the background without stealing the cursor
- 출처/신뢰도: GitHub/HN, high for existence/adoption, medium for our fit
- GitHub 검증: `trycua/cua`, stars 14,996, pushed_at 2026-04-27T19:21:01Z, maintained yes, MIT
- 왜 중요: desktop/control agent 인프라가 빠르게 성숙 중이다. 다만 우리 범위에서는 실계정/상거래/파일 삭제 자동화가 아니라 데모, 공개 웹 evidence, 로컬 UI 테스트에 한정해야 한다.
- 바로 할 일: mv-brain에서 실제 미디어/계정 조작 없이 “샘플 화면에서 텍스트 찾기/하이라이트/검증” 정도의 포트폴리오 데모 아이디어로만 보류.

## 한국/Threads/X 신호
- Threads 검색: `site:threads.net "Claude Code" github.com`, `site:threads.net "클로드 코드"`에서 추천할 만한 공개 GitHub 링크 신호 없음.
- 한국어 검색: Velog/Tistory에 Claude Code 입문/설치 글은 보였지만, 새 GitHub 프로젝트나 검증 가능한 워크플로 신호는 약함.
  - 예: Velog “Claude Code 완전 정복 가이드”, Tistory “Claude Code 사용방법”류. 튜토리얼 성격이라 추천 목록에는 미포함.
- X 검색: `site:x.com "Claude Code" "MCP" github.com`에서 2026-04-28 글이 Claude Code 관련 repo 목록을 언급.
  - 검증된 링크: `obra/superpowers` stars 171,934, pushed_at 2026-04-28T23:56:47Z, MIT; `hesreallyhim/awesome-claude-code` stars 41,752, pushed_at 2026-04-27T14:40:18Z; `czlonkowski/n8n-mcp` stars 18,865, pushed_at 2026-04-29T05:59:50Z, MIT.
  - 판단: repo 자체는 강하지만 X 글은 큐레이션/홍보성이므로 medium. 새 실행보다 패턴 참고용.

## 우리한테 적용
- mv-brain:
  - Codex/Claude 작업 후 `session_summary.md` + `next_safe_action`을 남기는 세션 회수 패턴을 도입할 만하다.
  - Playwright MCP식 evidence capture는 README/데모 검증 자동화에 좋다. 실제 미디어 분석/프로바이더 호출은 계속 금지.
  - 콘텐츠 각도: “AI 에이전트를 더 오래 돌리는 법”보다 “중단돼도 복구되는 로컬-first 작업 로그”가 포트폴리오에 더 신뢰감 있다.
- Hermes/자동화:
  - 크론 출력에 `read-only/public/no-auth`, `sources`, `mutations: none` 같은 안전 배지를 고정하면 좋다.
  - Domeggook/Coupang 관련 자동화는 write/mutation 전 human approval, dry-run preview, rollback path를 명시.
  - MCP/브라우저 도구를 붙이더라도 공개 리서치와 evidence 수집까지만 허용.
- 콘텐츠/홍보:
  - Threads/X 훅: “요즘 AI 코딩 트렌드는 모델보다 권한/검증/복구 쪽으로 움직인다.”
  - LinkedIn/블로그: Codex permission profile, Playwright MCP evidence, Cordon/HITL을 묶어 “AI coding agent를 안전하게 운영하는 4단계” 글로 전환 가능.

## 버릴 것/보류
- “AI agent가 DB를 지웠다”류 Reddit 재확산: 안전 교훈은 유효하지만 원문/맥락 검증 전에는 사례로 쓰지 말 것.
- stars 0~2짜리 새 harness/checkpoint repo: 아이디어는 볼 수 있지만 도입 금지. 설명/테스트/유지보수 증거 부족.
- X/Threads 큐레이션 글만 보고 도구 설치: 금지. 반드시 GitHub/docs/release 확인 후 패턴만 추출.
- `gstack` 파생 repo 다수: 포트폴리오/운영에는 guard/review/context-save 패턴만 참고하고, 스킬팩 대량 설치는 보류.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
