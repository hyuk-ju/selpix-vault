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
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.30
  - https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.0
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.71
  - https://cursor.com/blog/typescript-sdk
  - https://openai.com/index/harness-engineering/
  - https://lanes.sh/blog/linear-to-lanes
  - https://agentport.sh/
  - https://github.com/yakkomajuri/agentport
  - https://github.com/frenchie4111/harness
  - https://github.com/context-labs/HALO
  - https://github.com/nizos/conduct
  - https://github.com/nizos/tdd-guard
  - https://github.com/benvenker/agent-session-search
  - https://github.com/punparin/obsidian-mcp
  - https://github.com/warpdotdev/warp
  - https://reddit.com/r/ClaudeAI/comments/1szn9b0/how_to_be_better_than_99_of_claude_code_users/
  - https://reddit.com/r/ClaudeAI/comments/1sztkml/tdd_and_rules_enforcement_using_hooks/
  - https://reddit.com/r/LocalLLaMA/comments/1sz3i73/what_tools_are_you_using_to_give_your_llm_a/
  - https://news.ycombinator.com/
created: 2026-04-30
reviewed_at: 2026-04-30
---

# AI 코딩 빠른 레이더 — 2026-04-30 22:03 KST

## 핵심 3줄
1. 이번 신호의 중심은 “새 모델”보다 **하네스/환경 설계**입니다. Cursor SDK, OpenAI Codex harness 글, Linear→Lanes MCP 흐름이 모두 에이전트를 제품/운영 파이프라인 안에 넣는 방향입니다.
2. 단기 적용 우선순위는 **작업 격리(git worktree) + 수용 기준 + 테스트/훅 가드 + 세션 검색/메모리**입니다. mv-brain과 Hermes cron에 바로 맞습니다.
3. Reddit 쪽에서는 비용 폭주/루프/키 유출 사례가 반복됩니다. 자동화 확대보다 **권한·예산·파괴적 작업 게이트**를 먼저 넣어야 합니다.

## 주목할 것

### 1. Cursor SDK public beta — programmatic coding agents
- 링크: https://cursor.com/blog/typescript-sdk
- 출처/신뢰도: official + HN, high
- GitHub 검증: 패키지/공식 블로그 중심. repo 검증 대상 아님.
- 왜 중요: Cursor가 IDE 기능을 넘어 “몇 줄 TypeScript로 로컬/클라우드 VM에서 에이전트를 실행”하는 SDK를 공개했습니다. 작업 큐, PR 생성, 반복 빌드/리뷰 자동화의 기준선이 올라갑니다.
- 바로 할 일: mv-brain에 도입하지 말고 패턴만 복제: `acceptance criteria -> isolated worktree -> test/build -> human review` 형태의 작은 runner 설계.

### 2. OpenAI Codex Harness Engineering
- 링크: https://openai.com/index/harness-engineering/
- 출처/신뢰도: official + HN, high
- GitHub 검증: 공식 글. repo 검증 대상 아님.
- 왜 중요: 핵심 메시지가 “에이전트 성능은 모델보다 환경/도구/피드백 루프가 좌우한다”입니다. worktree별 부팅, DevTools/DOM snapshot, 자체 리뷰, acceptance criteria가 반복적으로 나옵니다.
- 바로 할 일: mv-brain에 `demo task template`을 추가할 때 “성공 기준, 금지 작업, 검증 명령, 산출물 경로”를 필수 필드로 만들기.

### 3. Linear → Lanes 두 MCP 워크플로우
- 링크: https://lanes.sh/blog/linear-to-lanes
- 출처/신뢰도: official/HN, medium-high
- GitHub 검증: 블로그 기반. repo 확인은 별도 필요.
- 왜 중요: Linear 이슈를 로컬 보드로 가져오고 fresh git worktree에서 세션을 시작한 뒤 결과를 다시 동기화하는 구조입니다. Hermes cron/Obsidian task와도 구조가 유사합니다.
- 바로 할 일: Hermes에는 Linear 대신 Obsidian `Tasks/` 또는 GitHub issue를 입력으로 쓰는 “1 세션 = 1 산출물” 체크리스트만 먼저 적용.

### 4. AgentPort — 에이전트용 보안 게이트웨이
- 링크: https://agentport.sh/ / https://github.com/yakkomajuri/agentport
- 출처/신뢰도: GitHub + HN, medium
- GitHub 검증: `yakkomajuri/agentport`, stars=10, forks=2, pushed_at=2026-04-29T21:21:39Z, license=MIT, maintained=recent.
- 왜 중요: 외부 서비스 연결/파괴적 작업에 granular permission과 승인 흐름을 두는 방향입니다. Reddit의 비용 폭주/키 유출 신호와 정면으로 맞물립니다.
- 바로 할 일: Hermes live credential 사용 전 확인, dry-run 기본값, destructive command denylist를 문서화. 구현은 자체 작은 가드로 충분.

### 5. Conduct / TDD-Guard — 코딩 에이전트 훅 기반 규칙 집행
- 링크: https://github.com/nizos/conduct / https://github.com/nizos/tdd-guard / https://reddit.com/r/ClaudeAI/comments/1sztkml/tdd_and_rules_enforcement_using_hooks/
- 출처/신뢰도: GitHub + Reddit, medium
- GitHub 검증: `nizos/conduct` stars=23, pushed_at=2026-04-29T06:41:48Z, MIT. `nizos/tdd-guard` stars=2041, pushed_at=2026-04-26T13:34:16Z, MIT.
- 왜 중요: “에이전트에게 TDD 하라고 말하기”보다 훅/정책 엔진으로 못 건너뛰게 하는 접근입니다. 작은 로컬 프로젝트에서 효과가 큽니다.
- 바로 할 일: mv-brain 안전 기준은 `python3 -m compileall -q mv_brain` + UI build 조건을 에이전트 작업 종료 전 체크리스트로 강제.

## 보조 신호
- `frenchie4111/harness`: https://github.com/frenchie4111/harness — stars=14, pushed_at=2026-04-30T00:12:13Z, MIT. parallel Claude Code + worktree 관리. 아직 작지만 패턴 참고 가치 있음.
- `context-labs/HALO`: https://github.com/context-labs/HALO — stars=210, pushed_at=2026-04-29T23:28:06Z. agent loop optimizer. 실사용 검증 전까지 관찰.
- `benvenker/agent-session-search`: https://github.com/benvenker/agent-session-search — stars=0, pushed_at=2026-04-30T13:02:46Z, MIT. 코딩 에이전트 세션 검색 MCP. 아이디어는 Hermes memory 검색에 맞지만 repo 신호는 아직 약함.
- `punparin/obsidian-mcp`: https://github.com/punparin/obsidian-mcp — stars=0, pushed_at=2026-04-30T13:02:28Z, MIT. Obsidian read/write MCP. Hermes에는 권한 범위가 넓어 바로 도입 비추천, 스키마 검증/auto-link 아이디어만 참고.
- `warpdotdev/warp`: https://github.com/warpdotdev/warp — stars=47517, pushed_at=2026-04-30T11:30:16Z, AGPL-3.0. terminal-born agentic dev environment. 큰 흐름 관찰용.

## Reddit/HN 요약
- Reddit r/ClaudeAI “How to be better…”: 성공 기준과 subagent를 의도적으로 쓰고, 반복 패턴이 생긴 뒤 skill/.md 문서화하라는 실전 조언. 신뢰도 medium.
- Reddit r/ClaudeAI “TDD and Rules Enforcement using Hooks”: Conduct/TDD-Guard로 hooks 기반 규칙 집행. GitHub 검증 완료.
- Reddit r/LocalLLaMA “persistent second brain”: LLM memory 도구 리스트와 토론. `fsaint/bestOfSecondBrainLLM` stars=4, pushed_at=2026-04-29T16:35:37Z라 큐레이션 신호는 약하지만 주제 수요는 강함.
- Reddit r/cursor 비용 폭주 사례: 사회적 증거는 낮지만, 예산/루프 제한 필요성은 높음.
- HN: Cursor SDK, OpenAI Codex harness, AgentPort, Linear→Lanes가 모두 최근 24h 내 노출.

## 한국/Threads 신호
- DuckDuckGo 공개 검색으로 `site:threads.net "클로드 코드" github.com`, `site:velog.io "Claude Code" "github.com"`, `site:disquiet.io "AI 코딩" "github.com"`를 확인했지만 추천할 만한 한국어/Threads 공개 결과는 거의 없었습니다.
- X 검색 결과에는 “Claude Code Resource Bible”류 링크가 보였으나 특정 GitHub repo를 검증할 수 있는 강한 원문 신호가 부족해 추천 제외.
- 일본어 gstack 파생 `uzumaki-inc/uzustack`: https://github.com/uzumaki-inc/uzustack — stars=0, pushed_at=2026-04-30T10:53:40Z. 한국 신호는 아니지만 지역화된 skill pack 흐름 참고용.

## 우리한테 적용
- mv-brain: 작업 템플릿을 `문제 → 성공 기준 → 허용/금지 작업 → 검증 명령 → 데모 산출물`로 고정. 실제 미디어 ingestion/Gemini 호출 없이 README/demo fixture 중심의 작은 PR 단위로 운영.
- Hermes/자동화: cron 작업도 “read-only 수집 → evidence links → vault 기록 → final report”처럼 하네스화. live credential/외부 mutation은 별도 승인 플래그 없으면 금지.
- 콘텐츠/홍보: “AI 코딩 잘하는 법 = 프롬프트 비법이 아니라 작업 환경 설계”를 주제로 Threads/X 글감 가능. mv-brain은 `local-first video editing agent harness` 포지션이 좋음.

## 버릴 것/보류
- Claude Code 소스 유출/디컴파일 계열 repo: 법적/윤리적 리스크와 실용성 낮음.
- stars=0의 무작위 MCP/agent memory repo 다수: 이름만 그럴듯하고 신호 약함.
- X/Threads-only “바이블/모음집” 주장: GitHub/docs 검증 전까지 low confidence.
- Obsidian MCP full read/write 도입: 현재 vault write policy와 충돌 가능성이 커서 보류.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
