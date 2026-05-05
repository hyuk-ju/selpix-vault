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
  - https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.1
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.72
  - https://github.com/NahimNasser/pu
  - https://news.ycombinator.com/item?id=47968112
  - https://openai.com/index/harness-engineering/
  - https://cursor.com/blog/typescript-sdk
  - https://github.com/cursor/cookbook
  - https://github.com/Infisical/agent-vault
  - https://github.com/yakkomajuri/agentport
  - https://github.com/letta-ai/letta-code
  - https://github.com/microsoft/playwright-mcp
  - https://github.com/garrytan/gstack
  - https://github.com/affaan-m/everything-claude-code
created: 2026-05-01
reviewed_at: 2026-05-01
---

# AI 코딩 빠른 레이더 — 2026-05-01 15:02 KST

## 핵심 3줄
1. 오늘 강한 신호는 “더 큰 자동화”보다 **작은 agent harness + 체크포인트/복구 + 권한 게이트**다. Pu.sh, OpenAI harness 글, Agent Vault/AgentPort가 같은 방향을 가리킨다.
2. 공식 릴리스는 빠르게 움직임: Claude Code v2.1.126, Codex rust-v0.128.0, Gemini CLI v0.40.1, Playwright MCP v0.0.72가 24~48h 내 갱신됐다.
3. 한국/Threads/X 검색은 GitHub 검증 가능한 새 강한 신호가 거의 없었다. X 검색은 리소스 모음류가 많고 일부 GitHub URL은 검색 스니펫상 잘려 검증 불가라 보류.

## 주목할 것

### 1. Pu.sh — 400줄 shell coding-agent harness
- 링크: https://pu.dev/ / https://github.com/NahimNasser/pu / HN https://news.ycombinator.com/item?id=47968112
- 출처/신뢰도: HN + GitHub, high
- GitHub 검증: 76 stars, pushed_at 2026-04-30T20:47:28Z, archived=false, license=MIT
- 왜 중요: Anthropic/OpenAI 루프, bash/read/write/edit/grep/find/ls, auto-compaction, checkpoint/resume, pipe mode, 90 no-API tests를 아주 작은 표면적으로 구현. Hermes 크론/스킬은 “큰 자율 루프”보다 이런 작고 검증 가능한 구조를 가져오는 게 안전하다.
- 바로 할 일: 설치하지 말고 README/소스만 읽어 `context-save/restore`, `checkpoint/resume`, `pipe mode`, `no-API tests` 패턴을 Hermes runbook 후보로 추출.

### 2. OpenAI “Harness engineering” + Codex 최신 릴리스
- 링크: https://openai.com/index/harness-engineering/ / https://github.com/openai/codex/releases/tag/rust-v0.128.0
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: openai/codex 79,217 stars, pushed_at 2026-05-01T06:03:15Z, Apache-2.0, maintained
- 왜 중요: Codex 자체보다 중요한 건 harness 설계 관점이다. mv-brain에서 “AI가 편집을 끝까지 자동 생성”이 아니라, 검증 가능한 작업 단위·로그·리뷰 지점을 만드는 방향과 맞다.
- 바로 할 일: mv-brain README/데모 문구에 “agent-first harness with inspectable checkpoints” 포지셔닝을 반영할 수 있는지 검토.

### 3. Agent credentials/permissions: Agent Vault + AgentPort
- 링크: https://github.com/Infisical/agent-vault / https://github.com/yakkomajuri/agentport / AgentPort HN 소개 https://news.ycombinator.com/item?id=47950752
- 출처/신뢰도: GitHub/HN, medium-high
- GitHub 검증: Infisical/agent-vault 869 stars, pushed_at 2026-04-30T00:16:27Z, archived=false. yakkomajuri/agentport 13 stars, pushed_at 2026-04-30T17:50:55Z, MIT, archived=false.
- 왜 중요: 에이전트에게 API 키를 직접 주지 않는 credential broker/permission gateway 패턴이 반복 등장. Hermes/Coupang/Domeggook 자동화에서 특히 중요하다.
- 바로 할 일: 당장 도입은 보류. 대신 운영 규칙에 “크론/에이전트는 비밀값 직접 출력 금지 + destructive call은 승인 게이트” 체크리스트를 더 강화.

### 4. Cursor TypeScript SDK + cookbook
- 링크: https://cursor.com/blog/typescript-sdk / https://github.com/cursor/cookbook
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: cursor/cookbook 2,706 stars, pushed_at 2026-04-29T17:43:57Z, maintained
- 왜 중요: Cursor가 IDE 내부 도구를 넘어 programmatic agent 쪽으로 메시지를 내고 있다. 팀/에이전트 워크플로우를 제품화하려는 흐름.
- 바로 할 일: mv-brain은 Cursor 종속 대신 “FCPXML export/checklist/test fixtures를 어떤 agent에서도 실행 가능”으로 차별화.

### 5. Memory-first / long-term context tools: Letta Code + gstack류
- 링크: https://github.com/letta-ai/letta-code / https://github.com/garrytan/gstack / https://github.com/affaan-m/everything-claude-code
- 출처/신뢰도: GitHub, medium-high
- GitHub 검증: letta-ai/letta-code 2,402 stars, pushed_at 2026-05-01T05:28:56Z, Apache-2.0. garrytan/gstack 87,468 stars, pushed_at 2026-05-01T05:54:22Z, MIT. affaan-m/everything-claude-code 171,076 stars, pushed_at 2026-04-30T16:25:17Z, MIT.
- 왜 중요: memory/session restore/skills pack이 계속 뜬다. 다만 별 수가 비정상적으로 큰 repo는 hype/집계 이상 가능성을 염두에 두고 패턴만 추출하는 게 안전하다.
- 바로 할 일: 설치 금지. `guard`, `review`, `investigate`, `qa`, `context-save/restore` 같은 skill naming만 Hermes/mv-brain 작업 카드에 차용.

## 한국/Threads/X 신호
- DuckDuckGo HTML로 `site:x.com "Claude Code" "MCP" github.com`, `site:threads.net "Claude Code" github.com`, `"클로드 코드" "github.com"`, `"커서" "MCP" "github.com"`, Velog/Disquiet 쿼리를 저용량 조회.
- 검증 가능한 한국어/Threads 신규 GitHub 추천 후보는 없음.
- X 결과는 “Claude Code 리소스 모음”류가 보였지만 검색 스니펫의 GitHub URL이 `github.com/affaan-m/every...`처럼 잘려 있었고 원문 접근은 로그인/검색품질 이슈가 있어 추천 제외. 이미 검증된 `affaan-m/everything-claude-code`만 watchlist로 유지.

## 우리한테 적용
- mv-brain: “local-first editor assistant” 포지션을 강화. 자동 완성형 MV보다 `clip triage → search → FX recommendation → FCPXML rough-cut → checkpoint/review` 흐름을 데모로 만들기.
- Hermes/자동화: Pu.sh식 작은 도구 표면 + checkpoint/resume + no-API tests를 크론/스킬 개선 기준으로 삼기. credentials/permission gateway는 당장 설치하지 말고 설계 체크리스트로 반영.
- 콘텐츠/홍보: 빌드로그 훅 — “AI 코딩 에이전트의 다음 경쟁력은 모델이 아니라 체크포인트와 권한 설계다.” 예시로 Pu.sh, Agent Vault, mv-brain의 FCPXML checkpoint를 연결.

## 버릴 것/보류
- X/Threads-only 리소스 모음: GitHub 원본 검증 전 추천 금지.
- 별 0~2개짜리 새 skill repo 다수: `codex-review-skill`, `iago`, `agent-skill-git-checkpoint` 등은 아이디어는 좋지만 adoption 낮아 watch만.
- “완전 자율 coworker/자체 컴퓨터”류: ghostwright/phantom 같은 repo는 활동성은 있으나 권한/보안 리스크가 커 Hermes에는 바로 적용 부적합.
- Cursor rogue/DB deletion류 영상/기사: 경고 사례로만 사용. 검증 가능한 재현/문서가 없으면 추천 항목으로 올리지 않음.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
