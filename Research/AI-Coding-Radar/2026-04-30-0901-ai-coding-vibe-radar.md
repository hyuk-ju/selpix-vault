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
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.30
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.71
  - https://cursor.com/blog/typescript-sdk
  - https://github.com/jazzyalex/agent-sessions
  - https://github.com/letta-ai/letta-code
  - https://github.com/trycua/cua
  - https://github.com/yakkomajuri/agentport
  - https://github.com/frenchie4111/harness
  - https://github.com/garrytan/gstack
  - https://github.com/affaan-m/everything-claude-code
  - https://github.com/kevintseng/gstack-industrial
  - https://news.ycombinator.com/item?id=47945185
  - https://news.ycombinator.com/item?id=47954962
  - https://www.reddit.com/r/ClaudeAI/comments/1symv0c/your_claude_code_project_dashboard_is_now_on_the/
  - https://www.reddit.com/r/LocalLLaMA/comments/1sz3i73/what_tools_are_you_using_to_give_your_llm_a/
created: 2026-04-30
reviewed_at: 2026-04-30
---

# AI 코딩 빠른 레이더 — 2026-04-30 09:01 KST

- 수집 방식: 사전 수집 GitHub/Reddit/HN 컨텍스트 + 공개 GitHub API 검증 + Jina Reader로 공식/공개 문서 확인. 로그인/쿠키/비공개 API 미사용.
- Vault 기록: `/home/dev/openclaw/config/workspace/vault/Research/AI-Coding-Radar/2026-04-30-0901-ai-coding-vibe-radar.md`

## 핵심 3줄

1. **Cursor SDK 공개 베타**가 가장 실무적 신호다. 에디터 안의 Agent를 TypeScript 런타임/클라우드 VM/CI 자동화로 빼내는 방향이라, Hermes 크론·리뷰 파이프라인 아이디어에 바로 연결된다.
2. **세션/메모리/체크포인트 계열**이 빠르게 늘고 있다. `agent-sessions`, `letta-code`, `gstack`, `everything-claude-code`는 “대화 저장”이 아니라 “중단 복구·검색·비용 추적·가드”를 제품화하는 흐름이다.
3. **권한/보안 게이트웨이**가 다음 병목이다. AgentPort, Cordon, Playwright MCP, Cursor/Reddit 삭제 사고 논의는 “자동 승인”보다 “읽기 전용·HITL·스코프 제한”을 포트폴리오 메시지로 써야 함을 보여준다.

## 주목할 것 5개

### 1. Cursor SDK public beta — 프로그램형 코딩 에이전트

- 링크: https://cursor.com/blog/typescript-sdk
- 출처/신뢰도: official, high
- GitHub 검증: 공식 블로그/패키지 중심. GitHub repo 별도 검증 대상 아님.
- 근거: Jina Reader로 공식 블로그 확인. 2026-04-29 공개. `npm install @cursor/sdk`, `Agent.create`, 로컬/클라우드 VM 실행, CI/CD·제품 내 임베딩 사용 사례를 명시.
- 왜 중요: “에디터에서 대화”를 넘어 빌드/리뷰/테스트/릴리즈 워크플로에 Agent를 붙이는 방향. mv-brain의 FCPXML export 검증, README/릴리즈 노트 초안, Hermes 일일 점검 자동화에 적용 가능.
- 바로 할 일: 설치는 보류. 대신 `mv-brain`에 “SDK형 agent runner가 필요할 때의 인터페이스” 메모만 작성하고, 현재는 Codex/Hermes 도구 호출 규칙을 더 단순화.

### 2. agent-sessions — 여러 CLI 에이전트 세션 검색/재개/한도 추적

- 링크: https://github.com/jazzyalex/agent-sessions
- 출처/신뢰도: GitHub + Reddit, medium-high
- GitHub 검증: 515 stars, 30 forks, pushed_at `2026-04-29T23:55:11Z`, MIT, 설명상 Codex CLI/Claude Code/OpenCode/Gemini CLI/OpenClaw 세션 브라우저·Agent Cockpit·Analytics·Limits tracker.
- Reddit 신호: r/ClaudeAI “Claude Code project dashboard”류 관심. 단, Reddit 주장은 보조 신호.
- 왜 중요: 우리 Hermes/Codex 작업도 “지난 세션에서 왜 그렇게 결정했는지”가 비용이다. 세션 인덱스/검색/재개 UX는 mv-brain 데모보다 Hermes 운영 생산성에 더 직접적.
- 바로 할 일: 설치하지 말고 README/데이터 포맷만 추적. Hermes 크론 보고서에는 `session_id`, `source_urls`, `vault_note_path`, `next_action` 같은 최소 메타데이터를 계속 남기는 방향.

### 3. gstack / everything-claude-code / gstack-industrial — 스킬 라우팅과 가드 패턴

- 링크:
  - https://github.com/garrytan/gstack
  - https://github.com/affaan-m/everything-claude-code
  - https://github.com/kevintseng/gstack-industrial
- 출처/신뢰도: GitHub, high for existence/activity; medium for 품질/효과
- GitHub 검증:
  - `garrytan/gstack`: 86,780 stars, pushed_at `2026-04-29T23:04:12Z`, MIT, 23개 역할형 Claude Code 도구/스킬.
  - `affaan-m/everything-claude-code`: 170,182 stars, pushed_at `2026-04-29T23:52:11Z`, MIT, skills/instincts/memory/security/research-first harness.
  - `kevintseng/gstack-industrial`: 8 stars, pushed_at `2026-04-29T18:59:52Z`, MIT, git state/dev phase 기반 skill router.
- 왜 중요: 핵심은 에이전트 수가 아니라 **상황별 프롬프트/권한/검증 라우팅**이다. mv-brain에는 `investigate → implement → review → demo-note`, Hermes에는 `research → verify → write-vault → concise-report` 라우터가 맞다.
- 바로 할 일: 설치 금지. 패턴만 추출해서 Hermes 스킬 문서에 “자동 실행 전 검증 질문/권한 라벨”로 재사용.

### 4. AgentPort / Cordon / Playwright MCP — MCP 권한 게이트웨이와 브라우저 검증

- 링크:
  - https://agentport.sh/
  - https://github.com/yakkomajuri/agentport
  - https://github.com/marras0914/cordon
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.71
- 출처/신뢰도: GitHub/HN/official, medium-high
- GitHub 검증:
  - `yakkomajuri/agentport`: 9 stars, 2 forks, pushed_at `2026-04-29T21:21:39Z`, MIT, “granular permissions” agent service gateway.
  - `marras0914/cordon`: 0 stars, pushed_at `2026-04-28T20:37:04Z`, MIT, HN “Security gateway for MCP tool calls with HITL approvals”로 발견. 아직 초기.
  - `microsoft/playwright-mcp`: 31,795 stars, pushed_at `2026-04-27T20:52:22Z`, latest `v0.0.71` published `2026-04-27T20:52:23Z`.
- 왜 중요: UI/브라우저 자동화가 늘수록 “무엇을 클릭/삭제/전송할 수 있는가”가 차별점. mv-brain은 실제 미디어 변형보다 먼저 **read-only UI review / screenshot QA** 컨셉이 안전하다.
- 바로 할 일: Hermes에서는 MCP/브라우저 도구를 붙일 때 기본값을 `read-only`, `no credentials`, `human approval for mutation`으로 문서화.

### 5. letta-code / CUA / Harness — 메모리·컴퓨터유즈·병렬 worktree는 관찰 대상

- 링크:
  - https://github.com/letta-ai/letta-code
  - https://github.com/trycua/cua
  - https://github.com/frenchie4111/harness
- 출처/신뢰도: GitHub/HN/Reddit, medium-high
- GitHub 검증:
  - `letta-ai/letta-code`: 2,397 stars, 248 forks, pushed_at `2026-04-29T23:58:55Z`, Apache-2.0, “memory-first coding agent”.
  - `trycua/cua`: 15,240 stars, 948 forks, pushed_at `2026-04-27T19:21:01Z`, MIT, desktop computer-use infra/sandboxes/SDK/benchmarks.
  - `frenchie4111/harness`: 13 stars, pushed_at `2026-04-29T23:21:07Z`, MIT, “Run a team of agents”.
- 왜 중요: 장기적으로는 mv-brain의 로컬 앱 조작/프리뷰 QA와 맞지만, 지금 당장 도입하면 범위가 커진다. 현재는 “local-first, sandbox, checkpoint” 메시지만 차용.
- 바로 할 일: `mv-brain` 데모 문구에 “agent does not control your editor blindly; it proposes, previews, exports”를 넣는 방향.

## 한국/Threads 신호

- DuckDuckGo 공개 검색으로 `site:threads.net "클로드 코드" github.com`, `"클로드 코드" "github.com"`, `site:velog.io "Claude Code" "github.com"`, `site:disquiet.io "AI 코딩" "github.com"`, `"커서" "MCP" "github.com"`를 소량 확인했다.
- 이번 창에서는 **추천할 만한 한국어/Threads 공개 자료 + 검증 가능한 GitHub 링크 조합은 발견하지 못했다.**
- X 검색(`site:x.com "Claude Code" "MCP" github.com`)은 리소스 모음/홍보성 링크가 많고 GitHub URL이 검색결과 HTML에서 잘려 나와 검증 불가 항목이 섞였다. 따라서 추천에서 제외.

## 우리한테 적용

- mv-brain:
  - 포지셔닝: “완전자동 MV 생성기”보다 **로컬 퍼스트 편집 보조 + 검색/추천/프리뷰/내보내기**가 안전하고 설득력 있다.
  - 다음 데모 아이디어: `clip triage → FX suggestion → FCPXML rough-cut export → human review` 흐름을 60초 GIF/짧은 영상으로 보여주기.
  - 보안 메시지: 컴퓨터유즈/브라우저 자동화 유행에 편승하되, 실제 mutation 없이 read-only 분석/제안 중심으로 말하기.

- Hermes/자동화:
  - 크론 보고서마다 `sources`, `verified_by`, `vault_note_path`, `discarded_items`를 남기는 지금 방식은 agent-sessions/메모리 트렌드와 잘 맞다.
  - MCP/브라우저 자동화는 기본 권한을 읽기 전용으로 두고, 계정/결제/삭제/등록 액션은 명시 승인 없이는 금지.
  - gstack류에서 가져올 패턴은 다중 에이전트가 아니라 `guard`, `review`, `investigate`, `qa`, `context-save/restore`다.

- 콘텐츠/홍보:
  - Threads/X 훅 1: “요즘 AI 코딩 툴의 진짜 변화는 모델이 아니라 ‘세션 복구·권한 게이트·체크포인트’ 쪽이다.”
  - Threads/X 훅 2: “mv-brain은 자동으로 영상을 완성하려는 도구가 아니라, 편집자가 버리는 시간을 줄이는 로컬 AI 조수로 가야 한다.”
  - LinkedIn/블로그 각도: `Why I avoid fully autonomous coding/video agents: logs, permissions, checkpoints first`.

## 버릴 것/보류

- X/Threads 단독 홍보글: GitHub/API 검증이 안 되면 추천 제외.
- `codex-yolo`류 자동 승인 도구: 실험용으로는 흥미롭지만 Hermes 운영 원칙과 충돌. 권한 자동 승인/네트워크/파일 수정 자동화는 보류.
- 별 0~3개 checkpoint/memory repo 다수: `ReliOptic/campsite`, `aptratcn/session-checkpoint`, `Dicklesworthstone/eidetic_engine_cli`는 키워드는 맞지만 아직 채택 신호가 약해 watchlist.
- Cursor Camp류 밈/게임형 콘텐츠: HN 반응은 크지만 당장 mv-brain/Hermes 운영 개선과 직접 연결은 약함.

## 관련 문서

- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
