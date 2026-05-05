---
type: research
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - https://github.com/anthropics/claude-code/releases/tag/v2.1.122
  - https://github.com/openai/codex/releases/tag/rust-v0.125.0
  - https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.0
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.71
  - https://github.com/opslane/opslane
  - https://github.com/aurite-ai/agent-verifier
  - https://github.com/codebreaker77/Fullerenes
  - https://github.com/trycua/cua
  - https://github.com/waybarrios/opencode-power-pack
  - https://github.com/yakkomajuri/agentport
  - https://github.com/z3z1ma/agent-loom
  - https://news.ycombinator.com/
  - https://www.reddit.com/r/ClaudeAI/comments/1syca49.json
  - https://www.reddit.com/r/micro_saas/comments/1syg8o5.json
  - https://www.reddit.com/r/ClaudeAI/comments/1sy4z7p.json
  - https://www.reddit.com/r/LocalLLaMA/comments/1swf37n.json
created: 2026-04-29
reviewed_at: 2026-04-29
---

# AI 코딩 빠른 레이더 — 2026-04-29 09:01 KST

## 수집 조건

- 공개/읽기 전용만 사용. 로그인, 쿠키, CAPTCHA 우회, 사설 API 없음.
- 사전 수집 데이터: GitHub repo/release 추적, Reddit 공개 JSON, HN Algolia fresh signals.
- 추가 검증: GitHub REST API로 HN/Reddit에서 나온 repo를 확인.
- X/Threads/한국어 검색은 public search/Jina/Bing 경로를 소량 시도했으나, 추천할 만한 GitHub URL 검증 결과는 발견하지 못함. 이 부분은 `부분 실패/낮은 신호`로 기록.

## 핵심 3줄

1. 오늘 강한 신호는 “새 모델/에이전트 hype”보다 **검증·권한·브라우저 실행 확인** 쪽이다. Opslane `/verify`, Agent Verifier, AgentPort/Cordon류가 같은 문제를 가리킨다.
2. Claude Code/OpenAI Codex/Gemini CLI/OpenCode는 모두 24h 내 push/release가 활발하다. 다만 우리가 바로 베낄 패턴은 “대형 agent loop”가 아니라 **작은 skill + evidence report + rollback/approval guard**다.
3. Reddit/HN에서 production deletion, 클릭 검증 피로, persistent memory가 반복된다. Hermes와 mv-brain에는 “작업 전 spec → 실행 → 검증 스크린샷/로그 → 변경 요약” 흐름을 작게 붙이는 게 가장 현실적이다.

## 주목할 것

### 1. Opslane `/verify` — Playwright MCP 기반 실행 검증 레이어

- 링크: https://github.com/opslane/opslane
- 출처/신뢰도: Reddit + GitHub 검증, medium
- GitHub 검증: stars=109, forks=3, pushed_at=2026-04-28T19:54:39Z, MIT, maintained=true, desc=`Verification Layer for Claude Code`
- 왜 중요: “코드 리뷰”가 아니라 실제 브라우저에서 계획서 기준으로 체크리스트를 돌리고 screenshots/report/verdicts를 남기는 방향. mv-brain UI나 commerce admin 모니터에 바로 맞는 패턴.
- 바로 할 일: 설치하지 말고 README/구조만 참고해서 mv-brain에 `plan.md -> playwright smoke -> report.html` 미니 데모를 설계.

### 2. Agent Verifier — agent output에 대한 정책/보안/루프 검증 skill

- 링크: https://github.com/aurite-ai/agent-verifier
- 출처/신뢰도: Reddit + GitHub 검증, medium
- GitHub 검증: stars=10, forks=0, pushed_at=2026-04-28T07:57:50Z, MIT, maintained=true, desc=`Agent Verifier is a coding agent skill... Works with Claude Code, Cursor, Windsurf, and 30+ agents.`
- 왜 중요: hardcoded secrets, hallucinated tools, infinite loop, 큰 system prompt 같은 agent 실패를 체크하는 방향. Hermes cron/scripts에는 “변경 전 dry-run, secrets 노출 방지, live credential 사용 금지” 규칙을 자동 점검하는 체크리스트로 재사용 가능.
- 바로 할 일: repo는 watch. 우리 쪽은 `Runbooks/agent-change-review` 형태의 수동 체크리스트부터 만들면 충분.

### 3. Fullerenes — local-first codebase knowledge graph / MCP memory

- 링크: https://github.com/codebreaker77/Fullerenes / npm `fullerenes`
- 출처/신뢰도: Reddit + npm/GitHub 검증, medium
- GitHub 검증: stars=15, forks=2, pushed_at=2026-04-28T12:50:38Z, MIT, maintained=true, desc=`Persistent memory for AI coding agents.` npm latest=0.1.4, modified=2026-04-27T14:31:05Z
- 왜 중요: 매 세션 같은 파일을 다시 읽는 비용을 줄이려는 흐름. 하지만 아직 작고 검증이 약하다.
- 바로 할 일: 설치 보류. mv-brain/Hermes에서는 이미 Obsidian + exact file lookup + manual RAG가 있으므로, “어떤 파일을 읽었고 왜 읽었는지”를 작업 로그에 남기는 쪽이 우선.

### 4. CUA / computer-use infra — 데스크톱/브라우저 agent sandbox

- 링크: https://github.com/trycua/cua
- 출처/신뢰도: HN + GitHub 검증, high
- GitHub 검증: stars=14900, forks=932, pushed_at=2026-04-27T19:21:01Z, MIT, maintained=true, desc=`Open-source infrastructure for Computer-Use Agents...`
- 왜 중요: “브라우저/데스크톱을 실제로 조작하는 agent” 실험이 계속 강해짐. mv-brain의 장기 데모(로컬 에디터 조작, FCPXML/export 확인)에는 참고 가치가 큼.
- 바로 할 일: 실사용/설치 금지. 데모 아이디어만 추출: “agent가 편집 결과를 눈으로 확인하고 실패 컷만 리포트한다.”

### 5. OpenCode skill portability — Claude Code skill을 OpenCode로 이식

- 링크: https://github.com/waybarrios/opencode-power-pack
- 출처/신뢰도: Reddit + GitHub 검증, medium
- GitHub 검증: stars=92, forks=11, pushed_at=2026-04-27T01:16:46Z, MIT, maintained=true, desc=`Eleven Claude Code skills ported to OpenCode...`
- 왜 중요: agent별 플러그인보다 Markdown+YAML skill 포맷이 더 이식성 있다는 신호. Hermes skills도 특정 agent에 잠그기보다 “작은 절차 문서 + 검증 기준”으로 두는 게 낫다.
- 바로 할 일: Hermes commerce/mv-brain skill 작성 시 `inputs`, `allowed tools`, `stop conditions`, `evidence format`를 명시하는 템플릿으로 정리.

## 공식/릴리스 watch

- Claude Code: https://github.com/anthropics/claude-code/releases/tag/v2.1.122 — 2026-04-28 release, repo pushed 2026-04-28T22:05:15Z.
- OpenAI Codex: https://github.com/openai/codex/releases/tag/rust-v0.125.0 — latest release 2026-04-24, repo pushed 2026-04-29T00:01:23Z.
- Gemini CLI: https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.0 — 2026-04-28 release, repo pushed 2026-04-28T23:58:47Z.
- OpenCode: https://github.com/anomalyco/opencode — repo pushed 2026-04-28T23:54:26Z, 매우 활발.
- Playwright MCP: https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.71 — 2026-04-27 release. `/verify`류의 실제 브라우저 검증 기반.

## 한국/Threads/X 신호

- 소량 public search 시도: `site:threads.net "클로드 코드" "github.com"`, `site:velog.io "Claude Code" "github.com"`, `site:x.com "Claude Code" "MCP" "github.com"`, `"커서" "MCP" "github.com"`.
- 결과: Bing/Jina/HTML 검색에서 검증 가능한 신규 GitHub URL을 추천 수준으로 찾지 못함.
- 결론: 오늘 보고서에는 한국/Threads/X-only 항목을 추천하지 않음. 신뢰도 낮은 social-only claim은 보류.

## 우리한테 적용

- mv-brain:
  - UI/검색/FX 추천 기능은 “agent가 고쳤다”에서 끝내지 말고 `plan.md -> smoke test -> screenshot/report -> verdicts.json` 흐름을 작은 데모로 만들기.
  - FCPXML export도 샘플 입력/출력 diff와 실패 케이스 리포트를 남기면 포트폴리오 신뢰도가 올라감.
- Hermes/자동화:
  - cron 결과에 “수집 방법, 신뢰도, 실패한 소스, GitHub 검증 여부”를 계속 남기는 현재 패턴 유지.
  - live credential/commerce mutation 전에는 Agent Verifier식 체크리스트: secrets, destructive command, broad loop, approval 필요 여부.
- 콘텐츠/홍보:
  - 훅: “AI 코딩의 다음 병목은 생성이 아니라 검증이다.”
  - 빌드로그 소재: “Claude/Codex에게 맡기기 전에 내가 추가한 5가지 guardrail: dry-run, source links, screenshots, verdicts.json, rollback note.”

## 버릴 것/보류

- X/Threads/Korean-search-only claim: 오늘은 검증된 repo 링크가 없어 추천 제외.
- production DB deletion Reddit 사례: 경고 신호로는 유용하지만 원문 사실관계/맥락이 제한적이라 단독 근거로 사용하지 않음.
- Cordon/AgentPort: 방향은 좋지만 stars와 사용 사례가 아직 작음. watch만.
  - https://github.com/marras0914/cordon — stars=0, pushed_at=2026-04-28T20:37:04Z
  - https://github.com/yakkomajuri/agentport — stars=2, pushed_at=2026-04-28T22:35:41Z
- neural-ledger-system: HN에 떴지만 patent-pending/작은 repo/범용 memory 주장이라 보류.
  - https://github.com/umbecanessa/neural-ledger-system — stars=2, pushed_at=2026-04-28T12:17:04Z

## 콘텐츠 초안

### Threads/X

AI 코딩의 병목이 “생성”에서 “검증”으로 넘어가는 중.

- Claude Code/Codex/Gemini CLI는 계속 빨라짐
- 그런데 실제 실패는 DB 삭제, 잘못된 클릭, hallucinated tool, 무한 루프에서 남
- 오늘 눈에 띈 건 `/verify`, Agent Verifier, Playwright MCP 같은 검증 레이어
- 다음 좋은 포트폴리오 데모는 “AI가 만들었다”가 아니라 “AI가 만든 걸 이렇게 증명했다”일 가능성이 큼

내 쪽도 mv-brain에 plan → smoke test → screenshot/report 흐름을 붙이는 쪽으로 볼 예정.

### LinkedIn

최근 AI coding agent 흐름을 보면, 더 큰 agent loop보다 작은 verification layer가 더 실용적으로 보입니다. Opslane `/verify`는 plan을 기준으로 브라우저 검증 리포트를 만들고, Agent Verifier는 보안/품질/무한루프 같은 agent output 리스크를 확인합니다.

제가 가져갈 교훈은 단순합니다. AI가 코드를 생성한 뒤 “작동한다”고 말하게 두지 않고, 재현 가능한 evidence를 남기는 것: smoke test, screenshot, verdicts.json, rollback note. 포트폴리오 프로젝트도 이 증거가 있어야 신뢰를 얻습니다.

## 관련 문서

- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
