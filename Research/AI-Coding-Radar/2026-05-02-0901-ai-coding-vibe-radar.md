---
type: research
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - https://github.com/anthropics/claude-code/releases/tag/v2.1.126
  - https://github.com/openai/codex/releases/tag/rust-v0.128.0
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.31
  - https://github.com/cline/cline/releases/tag/v3.82.0
  - https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.1
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73
  - https://github.com/atlassian-labs/mcp-compressor
  - https://github.com/letta-ai/letta-code
  - https://github.com/Tien-Lam/agent-history
  - https://github.com/roodriigoooo/trail
  - https://github.com/Mduffy37/claudeworks
  - https://github.com/juanallo/six-hats-skill
  - https://pu.dev/
  - https://openai.com/index/harness-engineering/
  - https://hn.algolia.com/api/v1/search_by_date
created: 2026-05-02
reviewed_at: 2026-05-02
---

# AI 코딩 빠른 레이더 — 2026-05-02 09:01 KST

> 공개/무인증/읽기 전용으로 수집. GitHub API, GitHub releases, HN Algolia, Reddit 사전 수집 결과, DuckDuckGo HTML 검색 일부를 사용했다. X/Threads/한국어 검색은 검색엔진 노출 결과 기준이라 신뢰도 낮음.

## 핵심 3줄

1. 이번 주 실전 변화는 “에이전트에게 더 많은 권한을 주기”보다 “권한 프로필·상태 복구·세션 히스토리·체크포인트” 쪽이다.
2. Claude Code v2.1.126의 `--dangerously-skip-permissions` 범위 확대와 Codex의 permission profile/goal workflow는 Hermes 같은 크론·작업 에이전트에 바로 영향이 있다.
3. `mcp-compressor`, `letta-code`, `agent-history`, `trail`처럼 컨텍스트 비용·메모리·세션 복구를 다루는 작은 도구들이 mv-brain/Hermes 운영 패턴과 잘 맞는다.

## 주목할 것

### 1. Claude Code v2.1.126 — 권한/상태 삭제 기능 변화

- 링크: https://github.com/anthropics/claude-code/releases/tag/v2.1.126
- 출처/신뢰도: GitHub release, high
- GitHub 검증: `anthropics/claude-code` stars=119721, pushed=2026-05-01T03:11:33Z
- 왜 중요:
  - `/model` picker가 Anthropic-compatible gateway의 `/v1/models`를 읽음.
  - `claude project purge [path]`가 transcripts/tasks/file history/config entry 삭제 지원. `--dry-run`, `--interactive` 있음.
  - `--dangerously-skip-permissions`가 `.claude/`, `.git/`, `.vscode/`, shell config 등 기존 보호 경로 쓰기 프롬프트까지 우회 가능해짐.
- 바로 할 일:
  - Hermes/Claude Code 계열 작업에서 `--dangerously-skip-permissions`를 크론/자동 작업 기본값으로 쓰지 말 것.
  - purge 기능은 프로젝트 초기화 전 `--dry-run`만 먼저 사용하도록 runbook에 패턴 반영 가능.

### 2. OpenAI Codex rust-v0.128.0 — persisted `/goal`, permission profiles

- 링크: https://github.com/openai/codex/releases/tag/rust-v0.128.0
- 관련 글: https://openai.com/index/harness-engineering/
- 출처/신뢰도: GitHub release + official blog, high
- GitHub 검증: `openai/codex` stars=79403, pushed=2026-05-01T23:59:54Z
- 왜 중요:
  - persisted `/goal` workflows: create/pause/resume/clear, app-server APIs, runtime continuation.
  - permission profiles: sandbox CLI profile selection, cwd controls, active-profile metadata.
  - Hermes 작업 방식과 직접 연결된다. “작업 목표 저장 → 중단/재개 → 권한 프로필로 실행 lane 분리”가 가능해지는 방향.
- 바로 할 일:
  - mv-brain에서 “demo-export”, “read-only-analysis”, “docs-only” 같은 작업 권한 프로필 네이밍을 먼저 설계.
  - 크론 프롬프트도 goal/checkpoint 형태로 바꾸면 실패 후 재개 보고가 쉬워짐.

### 3. Atlassian `mcp-compressor` — MCP 토큰 사용량 줄이는 래퍼

- 링크: https://github.com/atlassian-labs/mcp-compressor
- 출처/신뢰도: GitHub, medium-high
- GitHub 검증: stars=43, pushed_at=2026-05-01T23:59:54Z, license=Apache-2.0, maintained=최근 push 있음
- 왜 중요:
  - MCP 도구 호출이 많아질수록 tool result/context가 비용과 품질 병목이 된다.
  - mv-brain의 검색/클립 메타데이터, Hermes의 vault/search 결과처럼 “길고 반복되는 도구 출력”을 압축하는 패턴에 적합.
- 바로 할 일:
  - 설치는 보류. 먼저 기존 Hermes tool output 중 압축 가치가 큰 것(`rag_search`, vault search, product lookup)을 샘플링해서 요약 포맷만 흉내 내기.

### 4. Memory/session restore 계열 — `letta-code`, `agent-history`, `trail`

- 링크:
  - https://github.com/letta-ai/letta-code
  - https://github.com/Tien-Lam/agent-history
  - https://github.com/roodriigoooo/trail
- 출처/신뢰도: GitHub, medium
- GitHub 검증:
  - `letta-ai/letta-code`: stars=2403, pushed_at=2026-05-01T23:57:26Z, license=Apache-2.0, desc=The memory-first coding agent
  - `Tien-Lam/agent-history`: stars=0, pushed_at=2026-05-01T23:56:26Z, license=MIT, desc=Claude/Copilot/Gemini/Codex/OpenCode conversation history TUI
  - `roodriigoooo/trail`: stars=1, pushed_at=2026-05-01T18:27:18Z, license=MIT, desc=session artifacts/checkpoints for fresh-session continuation
- 왜 중요:
  - Reddit/HN에서도 “session history”, “persistent second brain”, “checkpoint”가 반복 신호.
  - 당장 도입보다 패턴이 중요: commands/errors/files/code blocks/prompts/responses를 lane으로 나누고, fresh session에 주입 가능한 checkpoint를 만드는 것.
- 바로 할 일:
  - Hermes 크론 결과 노트에 `Evidence used`, `Blocked/Retry`, `Next checkpoint` 섹션을 표준화.
  - mv-brain build-log에도 “지난 세션에서 이어받을 최소 컨텍스트”를 별도 섹션으로 남기기.

### 5. Playwright MCP v0.0.73 + OpenCode/Cline 안정화 신호

- 링크:
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.31
  - https://github.com/cline/cline/releases/tag/v3.82.0
- 출처/신뢰도: GitHub releases, high
- GitHub 검증:
  - `microsoft/playwright-mcp`: stars=31889, pushed=2026-05-01T22:35:25Z
  - `sst/opencode`: stars=153096, pushed=2026-05-01T23:56:45Z
  - `cline/cline`: stars=61263, pushed=2026-05-01T22:45:55Z
- 왜 중요:
  - Playwright MCP가 official MCP Registry 배포 흐름에 올라감.
  - OpenCode는 task child session에서 parent `external_dir`와 deny permissions 보존 버그 수정.
  - Cline은 terminal support, hook JSON escaping, ripgrep error handling 등 에이전트 실행 안정성 개선.
- 바로 할 일:
  - Hermes 브라우저/웹 리서치 자동화는 “registry MCP 사용 가능성”만 관찰. 설치/연동은 아직 보류.
  - Cline/Roo/OpenCode 비교 콘텐츠를 만든다면 “권한 상속·deny 유지·hook 안정성”을 비교 축으로 잡기.

## 한국/Threads/X 신호

- DuckDuckGo HTML 검색으로 `site:x.com "Claude Code" "MCP" github.com`에서 Claude Code 리소스 모음류 X 게시물 3건이 노출됐다. 로그인 없이 본문 검증이 제한되어 추천에는 미반영.
- `site:threads.net "Claude Code" github.com`, `"클로드 코드" "github.com"`, `"커서" "MCP" "github.com"`, Velog/Disquiet 조합은 이번 실행에서 유의미한 결과가 거의 없었다.
- 따라서 한국/Threads/X 신호는 오늘은 “보류”: GitHub URL이 직접 검증된 새 도구가 없었다.

## HN/Reddit 보조 신호

- HN: `Show HN: Pu.sh – a full coding-agent harness in 400 lines of shell` — points=88, comments=26, 2026-04-30. 링크: https://pu.dev/
- HN: `I built a Claude Code skill for structured decision making` → GitHub 검증 `juanallo/six-hats-skill`, stars=7, pushed_at=2026-05-01T20:22:03Z. 링크: https://github.com/juanallo/six-hats-skill
- HN: `Show HN: Destiny – Claude Code's fortune Teller skill` → GitHub 검증 `xodn348/destiny`, stars=32, pushed_at=2026-05-01T22:08:26Z. 재미/노이즈 성격이 강해 보류.
- Reddit 사전 수집: r/ClaudeAI/r/LocalLLaMA에서 session history, persistent second brain, AGENTS.md 패턴, skills packaging 논의가 반복. 단, 대부분 discussion-only라 GitHub/공식 근거 없는 항목은 추천 제외.

## 우리한테 적용

- mv-brain:
  - `AGENTS.md`/README에 “safe profiles”를 명시: `read-only`, `docs-only`, `demo-export`, `no-provider-calls`.
  - build-log 포맷에 `세션 체크포인트` 추가: 최근 명령, 바뀐 파일, 실패 로그, 다음 실험 1개.
  - UI/검색 demo는 Playwright MCP 자체 도입보다 “브라우저 기반 QA 체크리스트”부터 만들기.

- Hermes/자동화:
  - `--dangerously-skip-permissions`류 플래그를 크론에 넣지 않는 규칙 재확인.
  - 리서치 크론 산출물에 `sources`, `verification method`, `confidence`, `discarded`를 계속 남기는 현재 방식은 올바름.
  - MCP/tool 출력은 원문 전체보다 요약/증거 링크/검증 상태로 압축하는 포맷을 강화.

- 콘텐츠/홍보:
  - 포스트 훅: “요즘 AI 코딩 도구의 핵심은 더 똑똑한 모델보다 권한·체크포인트·세션 복구다.”
  - 짧은 빌드로그 소재: “mv-brain에 provider call 없이 안전하게 이어받는 agent checkpoint 포맷을 붙였다.”
  - 비교글 소재: Claude Code/Codex/OpenCode/Cline을 기능 목록이 아니라 `permission`, `resume`, `history`, `MCP result size` 기준으로 비교.

## 버릴 것/보류

- X/Threads 리소스 모음형 글: 검색 결과는 있으나 로그인 없이 본문·GitHub 링크 검증이 제한되어 오늘 추천 제외.
- `xodn348/destiny`: HN 반응은 있지만 fortune-telling skill이라 mv-brain/Hermes/수익화 fit 낮음.
- 별 0~1개짜리 “Claude Code clone/mini agent” 대량 repo: 최근 push는 많지만 유지보수·채택 신호 부족.
- Tavily MCP load balancer의 multi-key 회전류: request limit 우회 뉘앙스가 있어 Hermes 원칙과 맞지 않음.

## 관련 문서

- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
