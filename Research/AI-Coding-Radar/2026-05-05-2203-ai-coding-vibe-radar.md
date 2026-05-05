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
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.39
  - https://github.com/MinishLab/semble
  - https://github.com/first-fluke/oh-my-agent
  - https://github.com/vstorm-co/pydantic-deepagents
  - https://github.com/ActiveMemory/ctx
  - https://github.com/rohitg00/agentmemory
  - https://github.com/microsoft/playwright-mcp
  - https://github.com/fivetaku/insane-search
  - https://cursor.com/blog/continually-improving-agent-harness
  - https://reddit.com/r/ClaudeAI/comments/1t4eq55/i_built_a_diff_viewer_for_claude_code_where_you/
  - https://reddit.com/r/cursor/comments/1t4b6px/a_tool_that_turns_repeated_file_reads_into/
  - https://reddit.com/r/LocalLLaMA/comments/1t3w4xc/benching_local_qwen_as_a_codex_validator_coagent/
  - https://github.com/Innestic/claude-relay
created: 2026-05-05
reviewed_at: 2026-05-05
---

# AI 코딩 빠른 레이더 — 2026-05-05 22:03 KST

- 수집 방식: 사전 GitHub/Reddit/HN 공개 수집 결과 + 저용량 DuckDuckGo HTML 검색 + GitHub API 검증.
- 제한: X/Threads/한국어 검색은 공개 검색 결과만 사용. 로그인, 쿠키, CAPTCHA 우회 없음.

## 핵심 3줄

1. 메이저 터미널/IDE 에이전트는 계속 빠르게 갱신 중: Claude Code `v2.1.128`, OpenAI Codex `rust-v0.128.0`, OpenCode `v1.14.39`, Cline `v3.82.0`, Gemini CLI `v0.40.1`이 최근 1주 내 릴리즈/푸시 신호를 보임.
2. 오늘 실무에 바로 쓸 신호는 “더 큰 에이전트”보다 `검색 토큰 절감`, `세션/메모리 복원`, `체크포인트`, `브라우저 MCP` 쪽임.
3. mv-brain/Hermes에는 Semble류 코드검색, pydantic-deepagents류 체크포인트 패턴, Playwright MCP/insane-search식 단계형 웹 리서치가 가장 적용성이 큼.

## 주목할 것

### 1. Semble — agent용 저토큰 코드 검색

- 링크: https://github.com/MinishLab/semble
- 출처/신뢰도: GitHub + HN, high
- GitHub 검증: stars 645, forks 54, `pushed_at=2026-05-05T10:33:20Z`, MIT, maintained로 판단.
- 왜 중요: HN에는 “grep 대비 98% fewer tokens”로 노출. 실제 수치는 검증 필요지만, 에이전트가 큰 코드베이스에서 같은 파일을 반복 읽는 문제를 줄이는 방향은 mv-brain과 Hermes 모두에 바로 유효.
- 바로 할 일: 설치 전 README만 검토하고, mv-brain에서 `README/AGENTS/pyproject/mv_brain` 대상 “검색 전후 토큰/파일읽기 횟수 비교” 미니 실험 후보로 둔다.

### 2. pydantic-deepagents — Python 기반 deep agent/checkpoint 패턴

- 링크: https://github.com/vstorm-co/pydantic-deepagents
- 출처/신뢰도: GitHub, high
- GitHub 검증: stars 763, forks 80, `pushed_at=2026-05-05T12:01:29Z`, MIT, maintained.
- 왜 중요: Claude Code 스타일의 tool-calling, sandboxed execution, multi-agent teams, skills, checkpoints를 Python/Pydantic AI로 구현한다는 포지션. 넓은 자동화 루프를 부활시키기보다 “작은 검증 가능한 하위 에이전트 + 체크포인트” 설계 참고에 적합.
- 바로 할 일: Hermes cron에는 설치하지 말고, 체크포인트/검증/복구 흐름만 Runbook 패턴으로 추출한다.

### 3. oh-my-agent — `.agents` 기반 휴대형 멀티에이전트 harness

- 링크: https://github.com/first-fluke/oh-my-agent
- 출처/신뢰도: GitHub, medium-high
- GitHub 검증: stars 892, forks 102, `pushed_at=2026-05-05T12:59:03Z`, MIT, maintained.
- 왜 중요: Claude Code, Codex, Cursor, OpenCode 등 여러 도구 사이에서 skills/workflows/standards를 이식하려는 흐름. gstack 계열의 “역할별 skill”을 특정 툴에 종속시키지 않는 방향.
- 바로 할 일: 설치 보류. Hermes skill 문서에서 재사용 가능한 패턴만 추출: `investigate → plan → guard → review → summarize`.

### 4. ActiveMemory/ctx + agentmemory — 로컬 우선 agent memory 경쟁

- 링크: https://github.com/ActiveMemory/ctx / https://github.com/rohitg00/agentmemory
- 출처/신뢰도: GitHub + HN/search, medium-high
- GitHub 검증: `ActiveMemory/ctx` stars 52, `pushed_at=2026-05-04T10:56:18Z`; `rohitg00/agentmemory` stars 2197, `pushed_at=2026-04-29T16:26:26Z`, Apache-2.0.
- 왜 중요: 세션 compaction, `/clear`, 툴 간 이동에서 기억 손실을 줄이는 수요가 커짐. 다만 메모리 자동 주입은 오염/비밀 노출 리스크가 있어 evidence-gated 방식 필요.
- 바로 할 일: mv-brain/Hermes에 바로 붙이지 말고 “기억 후보 제안 → 사람이 승인 → Obsidian/AGENTS에 반영” 게이트만 참고.

### 5. Playwright MCP + insane-search — 공개 웹/브라우저 리서치의 표준화

- 링크: https://github.com/microsoft/playwright-mcp / https://github.com/fivetaku/insane-search
- 출처/신뢰도: GitHub/official, high for Playwright MCP, medium for insane-search
- GitHub 검증: Playwright MCP stars 32029, `pushed_at=2026-05-05T04:52:32Z`, Apache-2.0; insane-search stars 606, `pushed_at=2026-05-03T15:36:14Z`, MIT.
- 왜 중요: X/Threads/Reddit/한국 사이트처럼 기본 fetch가 막히는 영역에서 “공개 API → Jina/모바일/구조화 → 브라우저 렌더 → 중단”의 안전한 단계화가 필요함.
- 바로 할 일: Coupang/Domeggook 계정 작업에는 쓰지 말고, 공개 트렌드/콘텐츠 리서치 runbook에만 반영.

## 한국/Threads/X 신호

- DuckDuckGo 공개 검색으로 `site:threads.net "클로드 코드" github.com`, `"클로드 코드" "github.com"`, `"커서" "MCP" "github.com"`를 확인했지만 추천할 만한 한국어/Threads GitHub 링크는 발견하지 못함.
- `site:x.com "Claude Code" "MCP" github.com`는 “Claude Code resource bible”류 결과가 보였으나 검색 결과 내 GitHub URL이 잘려 있어 검증 가능한 신규 repo 추천에는 사용하지 않음.
- 결론: 오늘 한국/Threads/X는 strong signal 없음. GitHub/HN/Reddit 공개 신호 중심으로 보는 것이 안전.

## Reddit/HN 관찰

- Reddit r/ClaudeAI: Claude Code diff viewer, 세션 재사용, production agent loop logging 같은 “관찰/검토 UI” 수요가 보임. 단, 점수/댓글이 낮아 추천보다는 watch.
- Reddit r/Cursor: 반복 file read를 13-token reference로 바꾼다는 토큰 절감 도구 언급이 있음. GitHub 링크 미확인이라 보류하지만 Semble과 같은 방향.
- Reddit r/LocalLLaMA: 로컬 Qwen을 Codex validator/co-agent/challenger로 쓰는 흐름은 비용 절감보다 “검증 에이전트” 포지션이 유효.
- HN: Cursor의 agent harness 개선 글, Semble, ctx, Claude Relay가 모두 “대형 모델보다 harness/검색/기억/세션 연결” 쪽으로 수렴.

## 우리한테 적용

- mv-brain:
  - 코드 검색/컨텍스트 축소 실험: Semble류를 후보로 두고, 현재는 `AGENTS.md + README + compileall` 중심 baseline을 유지.
  - 영상 분석 자체보다 “agent가 왜 이 클립/FX를 추천했는지 근거를 재검색 가능한 형태로 남기는 기능”을 포트폴리오 각도로 잡기.
  - build log 소재: “AI 영상 편집 에이전트에서 진짜 어려운 건 생성이 아니라 컨텍스트/검색/검증이다.”

- Hermes/자동화:
  - cron은 넓은 자율 루프 금지 유지. 대신 각 run에 `sources`, `검증된 GitHub URL`, `보류 사유`, `Vault path`를 남기는 evidence-first 패턴 강화.
  - 메모리 도구는 바로 도입하지 말고 Obsidian 승인형 메모리만 유지.
  - 웹 리서치는 insane-search ladder를 runbook화하되, 로그인/쿠키/계정 자동화와 분리.

- 콘텐츠/홍보:
  - Threads/X 훅: “요즘 AI 코딩의 승부는 모델보다 grep을 얼마나 덜 하게 만드느냐에 가까워졌다.”
  - LinkedIn/블로그 주제: “Claude Code/Codex 시대에 포트폴리오 프로젝트가 보여줘야 할 것: 에이전트 UI보다 검증 가능한 작업 로그.”
  - mv-brain 데모 포인트: 코드/클립 검색 → 추천 근거 → FCPXML rough-cut export까지 ‘증거 있는 편집 보조’로 포지셔닝.

## 버릴 것/보류

- X/Threads-only “10x Claude Code setup” claims: GitHub URL 검증 불가 또는 너무 포괄적이라 보류.
- 계정 전환/사용량 회피성 Codex CLI 도구: 운영 리스크가 크고 현재 workflow fit 낮음.
- stars 0~1의 MCP 서버 대량 생성물: vertical demo 아이디어로는 참고 가능하지만 도입 추천은 아님.
- fully autonomous computer-use OS/desktop agent류: mv-brain/Hermes 현재 안전 기준과 맞지 않음.
- gstack 파생 marketing/game/PPT stacks: 패턴은 참고 가능하지만 지금 설치/도입할 이유는 약함.

## 관련 문서

- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
