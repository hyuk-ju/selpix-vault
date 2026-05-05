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
  - https://claude.com/blog/building-agents-that-reach-production-systems-with-mcp
  - https://github.com/trycua/cua
  - https://github.com/frenchie4111/harness
  - https://github.com/context-labs/HALO
  - https://github.com/tomo-kay/tene
  - https://github.com/z3z1ma/agent-loom
  - https://github.com/agentrq/agentrq
  - https://reddit.com/r/LocalLLaMA/comments/1sz3i73/what_tools_are_you_using_to_give_your_llm_a/
  - https://reddit.com/r/ClaudeAI/comments/1syvmnb/leaked_my_anthropic_key_into_a_public_repo_lost/
created: 2026-04-30
reviewed_at: 2026-04-30
---

# AI 코딩 빠른 레이더 — 2026-04-30 15:02 KST

## 핵심 3줄
1. 이번 24~72시간 신호는 “더 똑똑한 모델”보다 **에이전트 운영 레이어** 쪽이 강함: Cursor SDK, MCP production access, Playwright MCP, Harness/agent worktree, CUA 같은 실행·검증 인프라.
2. Reddit/HN에서는 **세션 손실, 장기 메모리, 병렬 에이전트, 키 유출/권한 통제**가 반복됨. Hermes와 mv-brain은 자동 루프보다 `체크포인트 + 승인 + 재현 가능한 로그`를 먼저 강화하는 쪽이 맞음.
3. 한국/Threads/X 검색은 검증 가능한 GitHub 링크가 거의 없었고, X 검색에서 Claude Code repo 모음 글이 잡혔지만 사회적 큐레이션 수준이라 추천 항목에는 넣지 않음.

## 주목할 것 5개

### 1. Cursor TypeScript SDK — programmatic agent 워크플로우
- 링크: https://cursor.com/blog/typescript-sdk
- 출처/신뢰도: official/HN, high
- GitHub 검증: Cursor 관련 GitHub `cursor/cursor`는 stars 32,798, pushed_at 2026-04-29T07:22:24Z로 활동 확인. SDK 자체는 공식 블로그 기준.
- 왜 중요: IDE 안에서만 쓰던 Cursor 흐름을 스크립트/CI/작업 큐와 연결하려는 방향. mv-brain의 “클립 분석 → rough-cut task → 검토” 같은 단계형 워크플로우를 에이전트 작업 단위로 포장하는 레퍼런스가 됨.
- 바로 할 일: 설치하지 말고, README/블로그에서 task schema와 permission 모델만 읽어서 `mv-brain`의 “agent task contract” 초안에 반영.

### 2. Anthropic: MCP로 production system에 닿는 agent 설계
- 링크: https://claude.com/blog/building-agents-that-reach-production-systems-with-mcp
- 출처/신뢰도: official/HN, high
- GitHub 검증: 관련 MCP 생태계 `microsoft/playwright-mcp` stars 31,811, pushed_at 2026-04-27T20:52:22Z, Apache-2.0, maintained.
- 왜 중요: MCP가 단순 tool 연결을 넘어 production 권한·감사·제한 설계 문제로 이동 중. Hermes cron/commerce ops에서 “조회는 자동, 변경은 승인” 원칙을 문서화하기 좋음.
- 바로 할 일: Hermes runbook에 `read-only collection`, `dry-run`, `mutation requires explicit approval` 3단계 권한 문구를 추가 후보로 둠.

### 3. trycua/cua — desktop/browser computer-use 인프라
- 링크: https://github.com/trycua/cua
- 출처/신뢰도: GitHub/HN, high
- GitHub 검증: stars 15,296, forks 950, pushed_at 2026-04-27T19:21:01Z, MIT, maintained.
- 왜 중요: “AI가 UI를 보고 테스트한다” 흐름이 강해짐. mv-brain은 실제 영상 편집 앱을 자동 조작하기보다, 데모/QA용으로 “UI visible check + screenshot evidence” 패턴만 가져오는 게 안전.
- 바로 할 일: 실사용 설치 보류. 대신 mv-brain 데모 콘텐츠 제목 후보: “AI 편집 에이전트가 UI를 직접 믿지 않고 증거 스냅샷으로 검증하는 법”.

### 4. Harness / worktree 병렬 에이전트 패턴
- 링크: https://github.com/frenchie4111/harness
- 출처/신뢰도: GitHub/HN, medium
- GitHub 검증: stars 14, forks 1, pushed_at 2026-04-30T00:12:13Z, MIT, 신생이지만 최근 활동.
- 왜 중요: 병렬 Claude Code를 git worktree로 분리해 충돌을 줄이는 패턴이 계속 등장. 현재 Hermes에는 broad autonomous loop를 되살릴 필요는 없지만, mv-brain에서 “문서 리뷰 agent / 테스트 agent / UI agent”를 분리하는 디자인 참고가 됨.
- 바로 할 일: 도구 설치 금지. `worktree per task + final human merge` 원칙만 노트로 추출.

### 5. Agent memory/session 복구: z3z1ma/agent-loom + Reddit second brain 신호
- 링크: https://github.com/z3z1ma/agent-loom / https://reddit.com/r/LocalLLaMA/comments/1sz3i73/what_tools_are_you_using_to_give_your_llm_a/
- 출처/신뢰도: GitHub/Reddit/HN, medium
- GitHub 검증: `z3z1ma/agent-loom` stars 10, forks 1, pushed_at 2026-04-30T00:51:45Z, MIT, maintained. Reddit second-brain thread는 score 21/comments 78.
- 왜 중요: 세션을 잃는 문제, long-running agent state, Markdown-native memory가 반복 신호. 이미 Obsidian vault를 쓰는 Hermes와 잘 맞지만, RAG 자동화보다 “작업별 summary/context-save 파일”이 더 단순하고 안전.
- 바로 할 일: mv-brain 작업마다 `context.md`, `decision-log.md`, `next-action.md` 세 파일만 유지하는 lightweight memory 템플릿 검토.

## 한국/Threads/X 신호
- Threads 검색: `site:threads.net "Claude Code" "github.com"`는 공개 검색에서 검증 가능한 결과 없음.
- X 검색: `site:x.com "Claude Code" "MCP" "github.com"`에서 “Best GitHub repos for Claude code...” 큐레이션 글이 잡혔지만, 원문/리스트 검증이 불충분해 추천 제외. 이미 사전 데이터의 `garrytan/gstack`, `ariadoss/superskills` 등 GitHub 검증 항목으로만 관찰.
- 한국어 검색: `"클로드 코드" "github.com"`, `"커서" "MCP" "github.com"`, Velog 쿼리에서 추천할 만한 검증 링크 없음. 현재는 한국어 신호보다 GitHub/HN/Reddit 쪽이 실질적.

## 우리한테 적용
- mv-brain: “완전 자동 편집”보다 `agent task contract`, `UI evidence snapshot`, `rough-cut export 검증`, `작업별 context-save`를 포트폴리오 포인트로 잡는 게 좋음.
- Hermes/자동화: MCP/agent 권한 이슈가 커지는 중이므로 cron은 지금처럼 public read-only, dry-run, vault note 중심이 안전. agent memory는 RAG 자동 인덱싱보다 Markdown 작업 로그로 유지.
- 콘텐츠/홍보: 빌드로그 주제 3개 — ① “AI coding agent가 실패하는 지점은 모델이 아니라 운영 레이어다” ② “mv-brain에 agent task contract를 넣는 이유” ③ “Claude/Codex/Cursor를 섞어도 망가지지 않는 체크포인트 규칙”.

## 버릴 것/보류
- `agentrq/agentrq`: HN Show HN에 등장했지만 GitHub stars 1, license 없음, 설명이 “AgentRQ App” 수준이라 watch만.
- `tomo-kay/tene`: AI-safe secret manager 방향은 좋지만 stars 7, 신생. 키 유출 Reddit 신호와 함께 watch, 바로 도입은 보류.
- X/Threads-only repo 큐레이션: 원문 검증/재현성 약함. GitHub API로 확인된 repos만 후속 검토.
- `gstack` 파생 다수: 패턴은 유용하지만 자동 설치/스킬 대량 도입은 현 Hermes 원칙과 충돌. `guard`, `review`, `context-save` 같은 단일 패턴만 추출.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
