---
type: research
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - https://github.com/anthropics/claude-code/releases/tag/v2.1.128
  - https://github.com/openai/codex/releases/tag/rust-v0.128.0
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.35
  - https://cursor.com/blog/continually-improving-agent-harness
  - https://github.com/MinishLab/semble
  - https://github.com/Gentleman-Programming/engram
  - https://github.com/ActiveMemory/ctx
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73
  - https://github.com/trycua/cua
  - https://reddit.com/r/ClaudeAI/comments/1t45c2b/dangerously_skip_permissions/
  - https://reddit.com/r/vibecoding/comments/1t44vjz/i_made_a_small_claudecodex_handoff_skill_for/
created: 2026-05-05
reviewed_at: 2026-05-05
---

# AI 코딩 빠른 레이더 — 2026-05-05 15:01 KST

- 수집 방식: 사전 GitHub/API/HN 결과 + 저용량 공개 GitHub API 검증 + Reddit public `search.json`/post `.json` fallback + DuckDuckGo/Bing RSS 기반 X/Threads/한국어 검색 확인. 로그인/쿠키/비공개 API 없음.

## 핵심 3줄

1. 메이저 터미널/IDE 에이전트는 모두 최근 1주 내 활발: Claude Code v2.1.128, Codex `rust-v0.128.0`, OpenCode v1.14.35, Cline v3.82.0, Gemini CLI v0.40.1, Playwright MCP v0.0.73.
2. 이번 주 실사용 신호는 “더 큰 모델”보다 **agent harness / 로컬 메모리 / 저토큰 코드검색 / 권한 가드** 쪽이 강함.
3. `mv-brain`과 Hermes에는 새 에이전트를 설치하기보다 `handoff`, `memory`, `grep 대체 코드검색`, `permission review` 패턴을 작게 흡수하는 게 효율적.

## 주목할 것

### 1. Cursor — “Continually improving our agent harness”
- 링크: https://cursor.com/blog/continually-improving-agent-harness
- 출처/신뢰도: official/HN, high
- GitHub 검증: 블로그라 repo 검증 대상 아님. HN fresh signal: 2026-05-05 03:00Z, points=3.
- 왜 중요: 에이전트 품질 개선의 초점이 모델 교체가 아니라 harness, context, tool-call, evaluation loop로 이동 중이라는 공식 신호.
- 바로 할 일: Hermes 스킬/cron 결과에도 `plan → act → verify → evidence` 로그 구조를 더 명확히 유지. `mv-brain` 작업에는 “실행한 검사/실행하지 않은 검사”를 PR/빌드로그에 분리.

### 2. Semble — agent용 저토큰 코드 검색
- 링크: https://github.com/MinishLab/semble
- 출처/신뢰도: GitHub/HN, high
- GitHub 검증: stars=631, pushed_at=2026-05-05T05:53:36Z, MIT, desc=`Fast and Accurate Code Search for Agents. Uses ~98% fewer tokens than grep+read`, maintained=recent push.
- 왜 중요: 큰 repo에서 `grep/read` 반복으로 토큰을 태우는 문제를 직접 겨냥. `mv-brain`처럼 Python+UI+docs가 섞인 repo에서 에이전트 탐색 비용을 줄일 수 있음.
- 바로 할 일: 지금은 설치 보류. 다음 `mv-brain` 리팩터링 때 `rg` baseline 대비 검색 결과 품질/토큰 절감만 작은 실험으로 비교.

### 3. Engram / Ctx — 로컬-first agent memory 경쟁
- 링크: https://github.com/Gentleman-Programming/engram / https://github.com/ActiveMemory/ctx
- 출처/신뢰도: GitHub/HN, high~medium
- GitHub 검증: Engram stars=3181, pushed_at=2026-05-05T05:51:21Z, MIT, SQLite+FTS5+MCP/HTTP/CLI/TUI. Ctx stars=50, pushed_at=2026-05-04T10:56:18Z, local-first convergent memory, license=NOASSERTION.
- 왜 중요: “AGENTS.md + Obsidian + handoff note”를 외부 기억장치로 쓰는 방식이 점점 표준화되고 있음. 단, 자동 기억은 오염 위험이 있어 evidence gate가 필요.
- 바로 할 일: Hermes에는 당장 MCP memory를 붙이지 말고, `Memory/변경로그.md`, daily note, runbook frontmatter를 memory schema처럼 유지. `mv-brain`은 `decision / verified_by / evidence` 템플릿을 먼저 개선.

### 4. Playwright MCP + CUA — browser/computer-use는 데모 자동화 재료
- 링크: https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73 / https://github.com/trycua/cua
- 출처/신뢰도: GitHub/official, high
- GitHub 검증: Playwright MCP stars=32012, pushed_at=2026-05-05T04:52:32Z, Apache-2.0. CUA stars=15626, pushed_at=2026-05-05T05:11:58Z, MIT, maintained=recent push.
- 왜 중요: 브라우저/데스크톱 제어 에이전트는 `mv-brain` UI 데모 캡처, Coupang 공개 페이지 리서치, 포트폴리오 영상 제작 보조에 직접 연결됨.
- 바로 할 일: 계정/쿠키 없이 public demo capture만 대상으로 한 `browser-demo checklist`를 Runbook으로 만들 후보. 실제 설치/자동화는 별도 승인 후.

### 5. Claude/Codex handoff + permission signal
- 링크: https://reddit.com/r/vibecoding/comments/1t44vjz/i_made_a_small_claudecodex_handoff_skill_for/ / https://github.com/francisN21/baton-pass / https://reddit.com/r/ClaudeAI/comments/1t45c2b/dangerously_skip_permissions/
- 출처/신뢰도: Reddit+GitHub, medium
- GitHub 검증: `francisN21/baton-pass` stars=0, pushed_at=2026-05-02T22:25:27Z, desc=low-token handoff workflow for multi-agent repos, maintained=recent but tiny.
- 왜 중요: 실제 사용자 불편은 “모델이 똑똑한가”보다 context handoff, stale assumptions, permission bypass 불안에 있음.
- 바로 할 일: 외부 plugin 설치는 보류. Hermes/Codex 작업에서 `handoff delta`, `verified commands`, `open risks` 3줄을 작업 종료 템플릿으로 차용.

## 한국/Threads 신호

- DuckDuckGo/Bing RSS로 `클로드 코드 github.com`, `코덱스 CLI github.com`, `커서 MCP github.com`, `AI 코딩 에이전트 github.com`, `site:threads.net 클로드 코드 github.com`, Velog/Tistory/OKKY 계열을 저용량 검색.
- 이번 실행에서는 추천할 만한 한국어/Threads 신규 GitHub 링크가 확인되지 않음. 검색 결과는 Claude 공식 다운로드/일반 블로그/무관한 Codex 게임 용어/중국어 자료가 섞여 노이즈가 높았음.
- X 검색은 `Claude Code MCP github.com`에서 “resource bible / best repos”류 게시물만 잡혔고, repo 원천 검증 없이 추천하기엔 낮은 신뢰도라 보류.

## 우리한테 적용

- mv-brain:
  - `Problem → Change → Verified → Demo asset needed → Next experiment` 빌드로그 템플릿 강화.
  - 큰 코드 탐색에는 Semble류를 실험 후보로 두되, 기본은 `rg + read_file + compileall` 유지.
  - UI 데모 캡처/공개 페이지 캡처는 Playwright MCP 패턴을 참고하되 실제 브라우저 자동화는 별도 승인 후.

- Hermes/자동화:
  - cron 보고서마다 `source method / confidence / limitation`을 계속 표기.
  - memory MCP 도입 전, Obsidian frontmatter와 `Memory/변경로그.md`를 evidence-gated memory로 운용.
  - permission 관련 작업은 “dry-run, preview, reversible changes”를 보고서에 명시.

- 콘텐츠/홍보:
  - 훅 후보: “AI 코딩 생산성은 모델보다 harness에서 갈린다.”
  - 짧은 포스트 소재: `Semble: grep/read 반복을 줄이는 agent 검색`, `Claude↔Codex handoff 3줄 규칙`, `권한 skip이 위험한 이유`.
  - `mv-brain` 포트폴리오에는 “에이전트가 실제로 검증한 명령과 못 한 일을 공개하는 빌드로그”가 차별점.

## 버릴 것/보류

- X/Threads only “best repos/resource bible”류: GitHub 원천 확인 전까지 low confidence.
- stars=0~1의 agent framework/checkpoint repo 다수: 최근 push는 있어도 adoption/문서/차별점 부족.
- Reddit의 cheap model/proxy류: 계정/약관/보안 리스크가 커서 Hermes 업무에는 부적합.
- 한국어 broad `바이브코딩` 검색: 노이즈가 높아 다음에도 제품명+GitHub+후기 조합으로만 제한 권장.

## 관련 문서

- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
