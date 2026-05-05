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
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73
  - https://github.com/0xhimanshu/governor
  - https://news.ycombinator.com/item?id=47982718
  - https://github.com/atlassian-labs/mcp-compressor
  - https://www.reddit.com/r/ClaudeAI/comments/1t1e5u0/loading_every_mcp_server_on_every_prompt_was/
  - https://github.com/dezgit2025/auto-memory
  - https://github.com/daniel-aranda/Agents-todo
  - https://www.reddit.com/r/selfhosted/comments/1t1ff28/cxq_a_repolocal_sqlite_queue_for_coding_agents/
  - https://github.com/garrytan/gstack
  - https://hn.algolia.com/api/v1/search_by_date
created: 2026-05-02
reviewed_at: 2026-05-02
---

# AI 코딩 빠른 레이더 — 2026-05-02 15:01 KST

## 핵심 3줄
1. 이번 신호는 “더 똑똑한 모델”보다 **토큰/컨텍스트 절약, 권한 프로필, 세션 복구, 체크포인트** 쪽이 실무 영향이 큼.
2. Codex CLI는 persisted `/goal`와 권한 프로필을 추가했고, Claude Code는 project purge와 `--dangerously-skip-permissions` 범위 변경이 있어 자동화 스크립트에서 특히 주의 필요.
3. MCP는 많이 붙이는 것보다 **라우팅/압축/선택 로딩**이 중요해지는 흐름. Hermes와 mv-brain에는 “항상 켜는 도구”보다 “작업별 필요한 도구만 켜기”가 맞다.

## 주목할 것

### 1. OpenAI Codex CLI v0.128.0 — persisted `/goal`, 권한 프로필, TUI 상태 개선
- 링크: https://github.com/openai/codex/releases/tag/rust-v0.128.0
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: openai/codex, stars 79,443, pushed_at 2026-05-02T06:00:33Z, maintained yes
- 왜 중요: 장기 작업을 goal 단위로 만들고 pause/resume/clear할 수 있어 cron/작업 큐와 맞물리기 좋다. 권한 프로필도 자동화 안전장치로 중요.
- 바로 할 일: Hermes 작업 로그에 “goal → evidence → result” 필드를 추가할지 검토. mv-brain에서는 FCPXML export 같은 긴 작업을 goal 단위 체크리스트로 쪼개는 데 활용.

### 2. Claude Code v2.1.126 — project purge와 위험 권한 범위 변경
- 링크: https://github.com/anthropics/claude-code/releases/tag/v2.1.126
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: anthropics/claude-code, stars 119,777, pushed_at 2026-05-01T03:11:33Z, maintained yes
- 왜 중요: `claude project purge [path]`는 상태 정리에 유용하지만, 자동 실행하면 transcript/task/file history를 지울 수 있음. release note 기준 `--dangerously-skip-permissions`가 `.claude/`, `.git/`, `.vscode/`, shell config 등까지 bypass한다고 되어 있어 위험 권한 사용을 더 엄격히 막아야 함.
- 바로 할 일: Hermes/cron에서 Claude Code를 쓴다면 purge는 항상 `--dry-run` 먼저. 위험 권한은 운영 repo에서 금지.

### 3. MCP 토큰 절약 흐름 — Governor + mcp-compressor + Reddit 실사용 불만
- 링크: https://github.com/0xhimanshu/governor, https://github.com/atlassian-labs/mcp-compressor, https://www.reddit.com/r/ClaudeAI/comments/1t1e5u0/loading_every_mcp_server_on_every_prompt_was/
- 출처/신뢰도: GitHub/HN/Reddit, medium-high
- GitHub 검증: governor stars 23, pushed_at 2026-05-02T02:16:14Z, MIT; atlassian-labs/mcp-compressor stars 43, pushed_at 2026-05-02T06:00:20Z, Apache-2.0
- 왜 중요: MCP 서버를 많이 붙이면 매 prompt마다 토큰/지연이 늘어난다는 실사용 문제가 반복됨. Governor는 Claude Code 출력/컨텍스트 slimming/telemetry/drift guardrails, mcp-compressor는 MCP tool 결과 토큰 축소에 초점.
- 바로 할 일: 설치보다 패턴만 차용. Hermes skill에는 “도구 사용 전 필요성 판단 → 출력 1차 요약 → 원본 링크 보존” 규칙을 강화.

### 4. Playwright MCP v0.0.73 — 공식 MCP Registry 배포
- 링크: https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: microsoft/playwright-mcp, stars 31,896, pushed_at 2026-05-01T22:35:25Z, maintained yes
- 왜 중요: browser/devtool MCP는 mv-brain 데모 UI 확인, 공개 웹 리서치, 콘텐츠 캡처에 유용. 공식 registry 배포로 설치/발견성이 올라감.
- 바로 할 일: 실제 브라우저 자동화는 아직 설치/상시 연결하지 말고, mv-brain UI smoke-test나 공개 페이지 캡처가 필요할 때만 일회성으로 검토.

### 5. 세션 기억/작업 큐: auto-memory와 repo-local SQLite queue
- 링크: https://github.com/dezgit2025/auto-memory, https://github.com/daniel-aranda/Agents-todo, https://www.reddit.com/r/selfhosted/comments/1t1ff28/cxq_a_repolocal_sqlite_queue_for_coding_agents/
- 출처/신뢰도: GitHub/Reddit, medium
- GitHub 검증: auto-memory stars 314, pushed_at 2026-05-02T06:00:36Z; daniel-aranda/Agents-todo stars 2, pushed_at 2026-05-02T05:45:05Z
- 왜 중요: 장기 세션 recall과 repo-local queue는 mv-brain 같은 포트폴리오 프로젝트에서 “다음 작업을 잊지 않는” 데 직접적. 다만 새 repo/초기 프로젝트는 과대평가 금지.
- 바로 할 일: 외부 도구 설치보다 Obsidian daily + git status + 작은 SQLite/Markdown queue 패턴을 우선 적용.

## 한국/Threads/X 신호
- DuckDuckGo 기반 공개 검색에서 `site:threads.net "Claude Code" github.com`, `site:velog.io "Claude Code" github.com`, `"클로드 코드" "github.com"`, `"커서" "MCP" "github.com"`는 추천 가능한 검증 링크를 찾지 못함.
- Google/Jina 검색은 429/CAPTCHA 경고가 떠서 중단. 로그인/캡차 우회는 하지 않음.
- X `site:x.com "Claude Code" "MCP" github.com` 검색은 listicle성 글이 보였지만 원문 접근/검증이 약하고 일부 snippet repo만 확인 가능하여 추천 제외. 소셜-only claim은 보류.

## 우리한테 적용
- mv-brain: agent가 긴 작업을 직접 계속 들고 가기보다 `goal/checkpoint/evidence` 단위로 쪼개기. UI smoke-test나 docs capture는 Playwright MCP 패턴을 후보로만 둠.
- Hermes/자동화: MCP/도구는 상시 전체 로딩 금지. 작업별 routing, 결과 압축, 원본 링크 보존, 위험 권한 금지를 기본값으로 둠.
- 콘텐츠/홍보: “AI 코딩은 모델보다 컨텍스트/권한/체크포인트 관리가 성패를 가른다”를 build-log 주제로 쓰기 좋음.

## 버릴 것/보류
- 0 star 대량 생성형 “agent harness/Claude Code skill” repo는 대부분 보류. 설명만 좋고 사용 신호가 약함.
- X/Threads listicle은 GitHub/API 검증 전 추천 금지.
- `--dangerously-skip-permissions`를 편의상 켜는 워크플로는 운영/상거래/비밀 파일이 있는 repo에서는 금지.
- Computer-use agent 범용 데모는 hype가 많고 mv-brain에 바로 필요한 건 browser smoke-test 정도라 우선순위 낮음.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
