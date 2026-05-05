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
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.33
  - https://github.com/cline/cline/releases/tag/v3.82.0
  - https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.1
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73
  - https://github.com/microsoft/playwright-mcp
  - https://github.com/agentskillexchange/skills
  - https://github.com/jyao97/xylocopa
  - https://github.com/star-ga/mind-mem
  - https://github.com/OWASP/Agent-Security-Regression-Harness
  - https://github.com/ahmedbutt2015/spine
  - https://github.com/garrytan/gstack
  - https://github.com/affaan-m/everything-claude-code
  - https://github.com/bawbel/bawbel-scanner
  - https://github.com/the-open-agent/openagent
  - https://www.mendral.com/blog/agent-harness-belongs-outside-sandbox
  - https://reddit.com/r/ClaudeAI/comments/1t2bfkg/monitoring_claude_code_on_mobile/
  - https://reddit.com/r/cursor/comments/1t1g6xz/are_you_all_still_managing_multiple_agent/
  - https://reddit.com/r/LocalLLaMA/comments/1t1jtc2/create_planmd_with_claude_code_opus_execute/
created: 2026-05-03
reviewed_at: 2026-05-03
---

# AI 코딩 빠른 레이더 — 2026-05-03 15:02 KST

## 핵심 3줄

1. 큰 에이전트들은 계속 빠르게 릴리스 중이다: Claude Code v2.1.126, Codex rust-v0.128.0, OpenCode v1.14.33, Cline v3.82.0, Gemini CLI v0.40.1, Playwright MCP v0.0.73가 4/30~5/2 사이 갱신됐다.
2. 이번 주 실무 신호는 “새 모델”보다 **에이전트 운영 레이어**다: 스킬 카탈로그, 모바일/다중 세션 모니터링, persistent memory, security regression harness, browser/devtool MCP.
3. `mv-brain`과 Hermes에는 자동 자율 루프보다 **검증 가능한 작은 도구**가 맞다: repo onboarding spine, 안전한 Playwright MCP 탐색, agent skill scanner, memory/checkpoint runbook을 우선 적용.

## 주목할 것

### 1. Playwright MCP v0.0.73 — 브라우저/웹앱 검증 MCP의 기본 옵션

- 링크: https://github.com/microsoft/playwright-mcp / https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73
- 출처/신뢰도: GitHub/official, high
- GitHub 검증: stars 31,922, pushed_at 2026-05-01T22:35:25Z, archived=false, license Apache-2.0
- 왜 중요: Claude/Codex/OpenCode류 에이전트가 UI를 직접 “본다/검증한다”는 흐름이 강해졌다. `mv-brain` UI 데모, FCPXML export preview, commerce 운영 대시보드 smoke test에 바로 연결 가능.
- 바로 할 일: `mv-brain`에는 provider/media 호출 없이 UI 빌드 후 screenshot/checklist 기반 read-only smoke test runbook만 먼저 만든다.

### 2. agentskillexchange/skills — 스킬 포맷/카탈로그 경쟁 시작

- 링크: https://github.com/agentskillexchange/skills
- 출처/신뢰도: GitHub search, medium
- GitHub 검증: stars 6, pushed_at 2026-05-03T06:00:28Z, archived=false, license MIT
- 왜 중요: “Claude Code/Cursor/Codex용 skill을 어떻게 포장·스캔·버전 고정할 것인가”가 반복 신호로 등장. Reddit에서도 Skill Forge, skills packaging 논의가 보인다.
- 바로 할 일: Hermes skill 작성 규칙을 `source ladder`, `evidence rule`, `dry-run`, `no credential` 템플릿으로 정리하면 포트폴리오 콘텐츠가 된다. 설치는 보류.

### 3. jyao97/xylocopa — 에이전트 세션을 휴대폰/저주의 레이어로 관리하려는 신호

- 링크: https://github.com/jyao97/xylocopa
- 출처/신뢰도: GitHub + Reddit 유사 니즈, medium
- GitHub 검증: stars 28, pushed_at 2026-05-03T06:00:09Z, archived=false, license Apache-2.0
- 관련 Reddit: https://reddit.com/r/ClaudeAI/comments/1t2bfkg/monitoring_claude_code_on_mobile/ , https://reddit.com/r/cursor/comments/1t1g6xz/are_you_all_still_managing_multiple_agent/
- 왜 중요: 사용자는 여러 CLI/IDE 에이전트 세션을 수동으로 감시하는 데 피로를 느낀다. “모바일에서 상태만 보고 승인/중단”하는 UX가 뜨고 있다.
- 바로 할 일: Hermes cron 결과에 `CURRENT/NEXT/RISKS` 구조를 강제하고, long-running 작업은 notify + 요약만 보내는 식으로 축소 적용.

### 4. mind-mem / persistent memory MCP — 로컬·감사 가능 메모리 경쟁

- 링크: https://github.com/star-ga/mind-mem
- 출처/신뢰도: GitHub, medium
- GitHub 검증: stars 4, pushed_at 2026-05-03T05:51:14Z, archived=false, license Apache-2.0
- 왜 중요: 설명 자체가 “auditable, contradiction-safe, rollback, MCP tools, hybrid BM25+vector”를 강조한다. 과장 가능성은 있지만, 방향은 Hermes/Obsidian 메모리 운영과 일치한다.
- 바로 할 일: 새 memory 서버를 붙이지 말고, 먼저 Obsidian note frontmatter + related docs + changelog 원칙을 “에이전트 기억의 최소 구현”으로 콘텐츠화한다.

### 5. OWASP Agent Security Regression Harness + bawbel-scanner — agentic 보안 회귀 테스트가 니치에서 표준으로 이동

- 링크: https://github.com/OWASP/Agent-Security-Regression-Harness / https://github.com/bawbel/bawbel-scanner
- 출처/신뢰도: GitHub/HN, medium-high for OWASP repo, medium for bawbel
- GitHub 검증: OWASP stars 10, pushed_at 2026-05-03T04:17:57Z, Apache-2.0; bawbel stars 2, pushed_at 2026-05-03T05:59:22Z, archived=false
- 왜 중요: HN에도 “agent harness belongs outside sandbox” 논의가 올라왔다. 에이전트가 DB/cluster/secrets를 건드리는 사고성 뉴스와 맞물려 permission guard, dry-run, secret scanner가 실무 필수로 가는 흐름.
- 바로 할 일: Hermes 작업 전 체크리스트에 `read-only? credentials? mutation? rollback?` 4문장을 고정하고, `mv-brain`에는 provider/media jobs 금지 가드를 README/AGENTS에 유지한다.

## 한국/Threads/X 신호

- public web search만 시도했다. Google 경유 Jina 검색은 429/CAPTCHA로 막혀 중단했고, DuckDuckGo HTML/Jina는 저용량으로 확인했다.
- Threads `site:threads.net "Claude Code" "github.com"`, Velog `site:velog.io "Claude Code" "github.com"`, 한국어 `"클로드 코드" "github.com"`에서는 이번 실행 기준 추천할 만한 검증된 GitHub 링크를 못 찾았다.
- X `site:x.com "Claude Code" "MCP" "github.com"`는 DuckDuckGo에 X 게시물 결과가 보였지만 본문 GitHub URL을 추출·검증할 수 없어 추천 항목에는 넣지 않았다. 신뢰도 low, 관찰만.

## Reddit/HN 보조 신호

- Reddit r/ClaudeAI: “Monitoring Claude Code on Mobile?” — 모바일/원격 세션 모니터링 니즈. https://reddit.com/r/ClaudeAI/comments/1t2bfkg/monitoring_claude_code_on_mobile/
- Reddit r/cursor: “Are you all still managing multiple agent sessions manually?” — 다중 에이전트 운영 피로. https://reddit.com/r/cursor/comments/1t1g6xz/are_you_all_still_managing_multiple_agent/
- Reddit r/LocalLLaMA: “Create Plan.md with Claude Code Opus, Execute Plan.md locally in Open Code using Qwen …” — 고급 모델로 계획, 로컬/저비용 모델로 실행 패턴. https://reddit.com/r/LocalLLaMA/comments/1t1jtc2/create_planmd_with_claude_code_opus_execute/
- HN: “The agent harness belongs outside the sandbox” — 에이전트 하네스/샌드박스 경계 논의. https://www.mendral.com/blog/agent-harness-belongs-outside-sandbox
- HN: Spine — codebase onboarding용 architecture spine. https://github.com/ahmedbutt2015/spine

## 우리한테 적용

- mv-brain:
  - Playwright MCP 패턴을 참고해 UI smoke test / screenshot checklist를 만든다. 실제 미디어 ingestion, Gemini/Qdrant mutation은 금지.
  - `spine`류 “verified architecture tour”를 README 또는 `Projects/AI-MV-Agent` 빌드로그로 변환하면 포트폴리오 진입장벽을 낮춘다.
  - 콘텐츠 훅: “자동 MV 생성이 아니라, 로컬 편집자가 검증 가능한 rough-cut assistant를 만드는 이유.”

- Hermes/자동화:
  - cron 결과를 `CURRENT/NEXT/RISKS` + Vault path + evidence link로 표준화한다.
  - 새 memory MCP 도입 대신 Obsidian frontmatter/related docs/changelog를 더 엄격히 지키는 것이 현재 비용 대비 최선.
  - tool mutation 전 `read-only / credentials / rollback / verification` 가드를 체크리스트로 고정한다.

- 콘텐츠/홍보:
  - Threads/X 초안: “이번 주 AI coding 툴 트렌드는 모델이 아니라 운영 레이어: skills, memory, browser MCP, security harness.”
  - LinkedIn/블로그 초안: “에이전트가 코드를 짜는 시대에 더 중요한 것은 permission, memory, review, and evidence log.”
  - commerce ops 연결: Coupang/Domeggook 운영 자동화도 broad autonomous loop보다 검증 가능한 가격/재고/마진 체크 스크립트가 더 안전하다는 메시지.

## 버릴 것/보류

- stars 0~1의 “full autonomy”, “agent OS overnight”, “account pooling”, “computer control”류는 과장·리스크가 커서 추천 제외.
- X/Threads-only claim은 본문/Repo 검증이 안 되면 low confidence로만 기록.
- gstack 파생 repo는 큰 방향은 유용하지만 설치/도입은 보류. 현재는 `guard`, `review`, `investigate`, `qa`, `context-save/restore` 패턴만 추출.
- local LLM 실행 최적화 글은 흥미롭지만 `mv-brain` 핵심 1주 액션과 거리가 있어 watchlist.

## 관련 문서

- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
