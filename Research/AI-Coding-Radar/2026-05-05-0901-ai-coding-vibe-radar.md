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
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.34
  - https://github.com/cline/cline/releases/tag/v3.82.0
  - https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.1
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73
  - https://github.com/MinishLab/semble
  - https://github.com/thedotmack/claude-mem
  - https://github.com/fivetaku/insane-search
  - https://github.com/garrytan/gstack
  - https://github.com/affaan-m/everything-claude-code
  - https://github.com/trycua/cua
  - https://www.reddit.com/r/ClaudeAI/comments/1t3osat/built_a_plugin_so_my_parallel_claude_code/
  - https://www.reddit.com/r/cursor/comments/1t2gjtl/cursor_silently_switched_models_while_i_was_deep/
  - https://hn.algolia.com/?query=Claude%20Code&sort=byDate&type=story
created: 2026-05-05
reviewed_at: 2026-05-05
---

# AI 코딩 빠른 레이더 — 2026-05-05 09:01 KST

## 핵심 3줄
1. 메이저 코딩 에이전트는 여전히 매일 갱신 중이다: Claude Code `v2.1.128`, OpenCode `v1.14.34`, Playwright MCP `v0.0.73`, Codex `rust-v0.128.0`가 최근 릴리스로 확인됐다.
2. 이번 주 실전 신호는 “더 큰 에이전트”보다 **세션/메모리/검색/검증 레이어**다. Semble, claude-mem, gstack류 스킬, Reddit의 병렬 세션 Relay 패턴이 반복적으로 보인다.
3. Cursor/Claude Code 사용자 사례에서 “모델 전환·삭제·권한 실수” 불만이 계속 나온다. 우리 자동화는 체크포인트, dry-run, diff preview를 기본값으로 유지해야 한다.

## 주목할 것

### 1. Semble — agent용 저토큰 코드 검색
- 링크: https://github.com/MinishLab/semble
- 출처/신뢰도: GitHub + HN, high
- GitHub 검증: stars=620, pushed_at=2026-05-04T18:06:18Z, MIT, maintained=최근 push 확인
- 왜 중요: “grep+read 대비 토큰 98% 절감”을 표방하는 agent용 코드 검색. `mv-brain`처럼 Python+UI+문서가 섞인 repo에서 에이전트가 불필요하게 큰 파일을 읽는 문제를 줄일 수 있다.
- 바로 할 일: 설치 전 README만 검토하고, `mv-brain`에 적용 가능하면 read-only 벤치(동일 질문에서 read_file 호출 수/토큰 비교)를 한 번만 해본다.

### 2. Claude Code / Codex / OpenCode / Gemini CLI 릴리스 추적
- 링크: https://github.com/anthropics/claude-code/releases/tag/v2.1.128, https://github.com/openai/codex/releases/tag/rust-v0.128.0, https://github.com/anomalyco/opencode/releases/tag/v1.14.34, https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.1
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: Claude Code stars=120374 pushed_at=2026-05-04T23:01:47Z, Codex stars=79946 pushed_at=2026-05-05T00:01:40Z, OpenCode stars=154670 pushed_at=2026-05-04T23:32:15Z, Gemini CLI stars=103134 pushed_at=2026-05-05T00:00:33Z
- 왜 중요: 툴 자체보다 “어떤 CLI를 기준 runner로 삼을지”가 빠르게 바뀐다. Hermes는 현재 Codex/Hermes 도구 규칙에 맞게 안전한 read/search/patch 루틴을 유지하는 게 중요하다.
- 바로 할 일: 자동 업데이트 금지. 주 1회만 릴리스 노트에서 permission, sandbox, MCP, session restore 관련 변경만 확인한다.

### 3. claude-mem + Logseq Brain류: 세션 재사용/메모리 레이어
- 링크: https://github.com/thedotmack/claude-mem, Reddit Logseq Brain: https://www.reddit.com/r/ClaudeAI/comments/1t3xulk/logseq_brain_v060_claudes_persistent_memory/
- 출처/신뢰도: GitHub high + Reddit medium
- GitHub 검증: `thedotmack/claude-mem` stars=72003, pushed_at=2026-05-04T23:05:48Z, description=Claude Code session capture/compression/context injection
- 왜 중요: OpenClaw/Hermes도 결국 “지난 작업 맥락을 어떻게 안전하게 재주입하느냐”가 품질을 좌우한다. 단, 자동 메모리 주입은 프롬프트 오염/비밀 노출 리스크가 있으므로 그대로 도입하면 안 된다.
- 바로 할 일: 구현보다 패턴만 차용. `mv-brain`에는 `AGENTS.md + build-log + 최근 git diff` 정도의 좁은 컨텍스트 번들을 수동 생성하는 방식이 안전하다.

### 4. Playwright MCP + insane-search: public web research / 화면 기반 검증
- 링크: https://github.com/microsoft/playwright-mcp, https://github.com/fivetaku/insane-search
- 출처/신뢰도: GitHub/official high
- GitHub 검증: Playwright MCP stars=32002 pushed_at=2026-05-01T22:35:25Z, insane-search stars=606 pushed_at=2026-05-03T15:36:14Z
- 왜 중요: Coupang/Naver/X/Reddit/YouTube류 public research에서 일반 fetch가 막히는 경우가 많다. 단, 로그인/쿠키/CAPTCHA 우회가 아니라 “공개 페이지의 낮은 볼륨 확인”에만 써야 한다.
- 바로 할 일: Hermes runbook에 이미 있는 fetch ladder를 유지. 커머스 상품/주문/계정 데이터는 scraping이 아니라 공식 API/기존 데이터로만 판단한다.

### 5. gstack / everything-claude-code: 스킬 팩·역할 기반 agent workflow
- 링크: https://github.com/garrytan/gstack, https://github.com/affaan-m/everything-claude-code
- 출처/신뢰도: GitHub high
- GitHub 검증: gstack stars=89160 pushed_at=2026-05-04T16:29:48Z, everything-claude-code stars=173286 pushed_at=2026-05-03T04:53:48Z
- 왜 중요: CEO/Designer/QA/Release Manager 식 역할 분리는 `mv-brain`의 포트폴리오 포장에도 유용하다. 다만 broad autonomous loop를 되살리기보다 `investigate → plan → patch → verify → content` 같은 작은 skill checklist만 차용하는 게 맞다.
- 바로 할 일: 설치하지 말고, `mv-brain`용 “demo-review”, “fcpxml-export-check”, “README-polish” 체크리스트로 분해한다.

## 한국/Threads 신호
- 공개 Bing RSS 방식으로 `site:threads.net`, `site:x.com`, `클로드 코드 github.com`, `커서 MCP github.com`, Velog/Disquiet/OKKY 조합을 낮은 볼륨으로 확인했다.
- Threads/X/Korean 검색에서 추천할 만한 **한국어 원문 + GitHub 검증 완료** 조합은 강하게 나오지 않았다. 검색 결과가 GitHub/중국어 가이드/일반 Claude 안내로 흘러가 신뢰 신호가 낮았다.
- 확인된 GitHub 링크 중 한국어/Threads 직접 신호로 추천할 만한 것은 없음. `claude-code-chinese/claude-code-guide`는 stars=346, pushed_at=2026-04-01로 참고만 가능하고 우리 업무 적합도는 낮다.

## Reddit/HN 실전 신호
- Reddit r/ClaudeAI: 병렬 Claude Code 세션끼리 메시지를 주고받는 Relay 패턴이 score=71/comments=18로 반응이 컸다. 링크: https://www.reddit.com/r/ClaudeAI/comments/1t3osat/built_a_plugin_so_my_parallel_claude_code/
- Reddit r/Cursor: 모델이 조용히 바뀌어 코드리뷰 맥락을 잃었다는 불만이 score=16/comments=18. 링크: https://www.reddit.com/r/cursor/comments/1t2gjtl/cursor_silently_switched_models_while_i_was_deep/
- HN: SprintiQ, Semble, smithy-ai, offload-mcp 등은 모두 낮은 HN 포인트지만 “agent가 일감을 쪼개고 검색하고 검증한다”는 방향성은 일관된다.

## 우리한테 적용
- mv-brain: Semble류 코드 검색/컨텍스트 압축을 도입 후보로 두되, 먼저 read-only 벤치. 포트폴리오 콘텐츠는 “AI가 영상을 자동 완성”보다 “로컬 편집 assistant가 검색·추천·FCPXML rough-cut까지 돕는다”로 계속 좁힌다.
- Hermes/자동화: 메모리 자동주입보다 `AGENTS.md`, 최근 git 상태, vault note, dry-run 결과를 명시적으로 묶는 context bundle이 안전하다. 크론은 로그인/쿠키/광범위 scraping 없이 public API/RSS/HTML fetch 위주로 유지.
- 콘텐츠/홍보: “내가 쓰는 AI 코딩 워크플로는 agent보다 checkpoint가 핵심”이라는 Threads/X 글감이 좋다. 예: 문제 → diff preview → compile/test → vault 기록 → 다음 action.
- Commerce promotion: Playwright MCP/insane-search 패턴은 상품 페이지/경쟁 소재를 public research로 관찰하는 데만 사용. 쿠팡 운영 판단은 Coupang API/등록 큐/도매꾹 데이터 기준.

## 버릴 것/보류
- 0-star fresh agent harness 다수(`tejacques/agent-harness`, `benvenker/agent-session-search`, `kirilligum/codex-langfuse-tracer`)는 아이디어는 좋지만 도입 보류. README와 실제 사용자가 쌓이면 재검토.
- `claude-code-proxy`류 구독/프록시 우회 도구는 정책·계정 리스크가 있어 Hermes 업무에는 비추천.
- X/Threads-only 주장, “몇 배 생산성”류 문구, 스타 수 대비 설명이 과한 repo는 데모/코드 검증 전 추천하지 않는다.
- Computer-use agent(`trycua/cua`)는 stars=15612로 신호는 강하지만 현재 `mv-brain`/커머스 운영에 즉시 필요한 1주일 액션은 아니다. 화면 조작 자동화보다 export/check/report가 우선.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
