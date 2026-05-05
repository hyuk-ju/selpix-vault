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
  - https://github.com/0xhimanshu/governor
  - https://github.com/lahfir/agent-desktop
  - https://github.com/dzmitrys-dev/supamem
  - https://github.com/gautamvarmadatla/mcpsafetywarden
  - https://github.com/BambooGap/skills-orchestrator
  - https://github.com/jjjames38/gstack-skills-marketplace
  - https://github.com/garrytan/gstack
  - https://github.com/affaan-m/everything-claude-code
  - https://reddit.com/r/ClaudeAI/comments/1t1e5u0/loading_every_mcp_server_on_every_prompt_was/
  - https://reddit.com/r/cursor/comments/1t1g6xz/are_you_all_still_managing_multiple_agent/
  - https://reddit.com/r/LocalLLaMA/comments/1t1jtc2/create_planmd_with_claude_code_opus_execute/
  - https://news.ycombinator.com/item?id=47984343
  - https://github.com/0xhimanshu/governor
  - https://github.com/lahfir/agent-desktop
created: 2026-05-02
reviewed_at: 2026-05-02
---

# AI 코딩 빠른 레이더 — 2026-05-02 22:02 KST

- 수집 범위: 공개 GitHub/release/search, Reddit 공개 결과, HN Algolia 결과, DuckDuckGo 기반 X/Threads/한국어 공개 검색 일부.
- 제한: 로그인/쿠키/비공개 API/CAPTCHA 우회 없음. X/Threads/한국어 검색은 검색 결과 스니펫 수준이라 추천 근거로는 낮게 취급.

## 핵심 3줄

1. 큰 흐름은 “더 똑똑한 모델”보다 **토큰/컨텍스트 낭비 제어, MCP 도구 게이팅, 세션/메모리 복원** 쪽으로 이동 중이다.
2. HN/Reddit에서 반복 신호가 있다: MCP를 전부 켜두면 예산이 새고, 여러 에이전트 세션을 사람이 수동 관리하는 피로가 커지고 있다.
3. `mv-brain`에는 완전 자동 편집보다 **로컬-first 에이전트 작업대 + 안전한 도구 권한 + 검색/메모리/체크포인트** 메시지가 더 팔릴 만하다.

## 주목할 것

### 1. Governor — Claude Code 토큰/컨텍스트 가드

- 링크: https://github.com/0xhimanshu/governor
- 출처/신뢰도: HN + GitHub, high
- GitHub 검증: stars 43, pushed_at `2026-05-02T02:16:14Z`, MIT, archived false, maintained yes
- 왜 중요: Claude Code 출력 압축, context slimming, tool-output filtering, telemetry, drift guardrails를 표방한다. r/ClaudeAI의 “MCP를 전부 로딩하면 토큰 예산이 무너진다” 신호와 직접 맞물린다.
- 바로 할 일: 설치하지 말고 패턴만 복사한다. Hermes skill/cron에 `tool-output 요약`, `불필요 MCP 비활성`, `비용 로그` 체크리스트를 추가 후보로 둔다.

### 2. Agent Desktop — OS 접근성 트리 기반 데스크톱 자동화 CLI

- 링크: https://github.com/lahfir/agent-desktop
- 출처/신뢰도: HN Show HN + GitHub, high
- GitHub 검증: stars 348, forks 16, pushed_at `2026-05-01T01:04:57Z`, Apache-2.0, archived false, maintained yes
- 왜 중요: Playwright/browser만이 아니라 네이티브 앱을 JSON 구조로 조작하는 방향. `mv-brain`의 FCPXML/편집 워크플로와 맞지만, 실제 Final Cut/미디어 앱 제어는 위험하므로 데모는 읽기/검색/내보내기 중심이 안전하다.
- 바로 할 일: `mv-brain` 홍보 문구에 “blind browser automation이 아니라 local editor assistant”를 강조. 실제 도입은 보류하고, README에 “desktop automation is optional/future, FCPXML export first”로 정리.

### 3. Supamem — Claude Code/Cursor/OpenCode용 dual-memory MCP CLI

- 링크: https://github.com/dzmitrys-dev/supamem
- 출처/신뢰도: GitHub fresh search, medium
- GitHub 검증: stars 4, forks 2, pushed_at `2026-05-02T13:01:27Z`, MIT, archived false, maintained yes but very early
- 왜 중요: “프로젝트 무관 메모리 + 구조적 memory hooks + Qdrant hybrid retrieval”은 Hermes/Obsidian/RAG와 동일한 문제를 겨냥한다. 다만 초신생 repo라 바로 채택은 위험하다.
- 바로 할 일: 코드 도입 금지. 대신 `mv-brain`에 `session summary -> project memory -> next run restore` 형태의 작고 읽기 쉬운 메모리 스펙을 설계할 때 참고.

### 4. MCP Safety Warden — MCP 도구 행동 프로파일/차단

- 링크: https://github.com/gautamvarmadatla/mcpsafetywarden
- 출처/신뢰도: GitHub fresh search, medium
- GitHub 검증: stars 6, forks 1, pushed_at `2026-05-02T13:00:58Z`, archived false, license NOASSERTION
- 왜 중요: MCP 서버가 실제 런타임에서 무엇을 하는지 불투명하다는 문제를 정면으로 다룬다. Coupang/Domeggook/Hermes 쪽에서는 “도구 호출 권한과 파괴적 작업 차단”이 핵심이다.
- 바로 할 일: 설치 보류. Hermes의 기존 원칙(읽기→초안→승인→실행)에 `MCP/tool allowlist`, `destructive call dry-run` 문구를 강화할지 검토.

### 5. Gstack/skills 파생 흐름 — 한국 SMB/커머스 skill pack 조짐

- 링크: https://github.com/garrytan/gstack, https://github.com/jjjames38/gstack-skills-marketplace
- 출처/신뢰도: GitHub watchlist + GitHub fresh search, medium
- GitHub 검증: `garrytan/gstack` stars 87989, pushed_at `2026-05-02T03:15:17Z`; `jjjames38/gstack-skills-marketplace` stars 0, pushed_at `2026-05-02T10:43:54Z`, MIT, archived false
- 왜 중요: gstack 본체는 이미 강한 adoption. 한국 SMB/라이브커머스/스마트스토어 skill pack은 아직 별 신호가 없지만, Hermes commerce skill을 “작고 검증 가능한 운영 skill”로 포장하는 콘텐츠 아이디어가 된다.
- 바로 할 일: 설치하지 말고 `context-save/restore`, `guard`, `review`, `qa`, `plan-eng-review` 패턴만 추출. 쿠팡/도매꾹에는 대규모 autonomous loop보다 체크리스트/검증 스킬이 맞다.

## 한국/Threads/X 신호

- DuckDuckGo 공개 검색에서 X 결과는 “Claude Code Resource Bible”, “Best GitHub repos for Claude Code”류의 큐레이션/리스트 글이 주로 잡혔다. 반복 노출은 있으나 스니펫 기반이고 원문/로그인 장벽 가능성이 있어 low confidence.
- 검색 결과 내 GitHub 언급으로 `affaan-m/everything-claude-code`, `hkuds/lightrag`, `czlonkowski/n8n-mcp` 계열이 보였으나 추천은 GitHub/공식 검증된 `affaan-m/everything-claude-code`만 watchlist로 유지한다.
- Threads/한국어 공개 검색(`클로드 코드`, `커서 MCP`, Velog/Tistory/Disquiet 계열)은 이번 저볼륨 검색에서는 추천할 만한 새 GitHub 검증 항목이 나오지 않았다.
- 한국 SMB 전용 gstack 파생 repo(`jjjames38/gstack-skills-marketplace`)는 발견됐지만 stars 0의 초기 repo라 “콘텐츠 아이디어” 수준으로만 보류.

## 우리한테 적용

- mv-brain:
  - “AI가 최종 MV를 자동 완성”보다 “로컬 편집자를 돕는 안전한 assistant” 포지셔닝을 유지.
  - 다음 build-log 소재: `clip search -> rough cut FCPXML -> human review checkpoint`와 `session memory/restore`.
  - desktop automation은 데모 욕심을 줄이고 FCPXML/파일 기반 워크플로를 먼저 보여준다.

- Hermes/자동화:
  - MCP/도구는 전부 켜두지 말고 작업별 allowlist로 줄이는 방향.
  - cron 리포트에는 `근거 링크`, `검증 상태`, `보류 이유`를 계속 남긴다.
  - agent memory는 broad RAG 자동화보다 `작업 종료 요약`, `다음 실행 입력`, `검증된 파일 링크` 중심이 더 안전하다.

- 콘텐츠/홍보:
  - Threads/X 훅 후보: “요즘 AI 코딩 툴에서 제일 중요한 건 모델 교체보다 MCP/토큰/체크포인트 관리다.”
  - LinkedIn/블로그 소재: “AI coding agent를 제품에 붙일 때 자동화보다 먼저 설계해야 하는 3가지: 권한, 메모리, 복구.”
  - `mv-brain` README 메시지: “local-first music-video editing assistant: search, triage, rough-cut export, human checkpoint.”

## 버릴 것/보류

- X/Threads 큐레이션 글만 근거인 “필수 Claude Code repo 50개”류: 중복/하이프 가능성 큼.
- stars 0~1의 agent harness/skills repo: 아이디어 참고는 가능하지만 바로 설치/도입 금지.
- native desktop/computer-use 자동화: demo value는 높지만 권한/오작동 리스크가 커서 `mv-brain` 핵심 경로로 삼지 말 것.
- “local model이 모든 agent 비용을 해결” 주장: Reddit 논의는 많지만 실제 병목(disk/RAM/tool-calling 품질)이 반복적으로 언급되어 보류.

## 관련 문서

- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
