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
  - https://github.com/wkdomains/macos-app
  - https://github.com/OWASP/Agent-Security-Regression-Harness
  - https://github.com/Yambr/open-computer-use
  - https://github.com/garrytan/gstack
  - https://github.com/roodriigoooo/trail
  - https://github.com/boshu2/agentops
  - https://www.mendral.com/blog/agent-harness-belongs-outside-sandbox
  - https://pu.dev/
created: 2026-05-03
reviewed_at: 2026-05-03
---

# AI 코딩 빠른 레이더 — 2026-05-03 09:01 KST

## 핵심 3줄
1. 이번 주 실전 변화는 “권한/세션/복구” 쪽이다. Claude Code는 위험 권한 우회 범위를 넓혔고, Codex는 `/goal` 지속 워크플로와 권한 프로필을 강화했다.
2. Playwright MCP의 공식 MCP Registry 배포와 wkdomains류 “브라우저를 에이전트 API로 노출” 패턴은 mv-brain 데모/검증 자동화에 바로 참고할 만하다.
3. Reddit/HN 신호는 멀티 에이전트 수동 관리 피로, 로컬 모델로 Plan 실행, agent harness를 sandbox 밖에 두자는 논쟁으로 모인다. 단, X/Threads/한국어 검색은 오늘 검증 가능한 GitHub 링크가 거의 없거나 검색이 막혔다.

## 주목할 것

### 1. Claude Code v2.1.126 — 권한 우회 범위 확대 + 프로젝트 상태 purge
- 링크: https://github.com/anthropics/claude-code/releases/tag/v2.1.126
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: `anthropics/claude-code`, stars=119934, pushed_at=2026-05-01T03:11:33Z, maintained=yes
- 왜 중요: `--dangerously-skip-permissions`가 `.claude/`, `.git/`, `.vscode/`, shell config 등 이전 보호 경로 쓰기 프롬프트까지 우회한다. 자동화/크론/실험 세션에서 실수 반경이 커졌다. 반대로 `claude project purge --dry-run`은 세션/상태 정리에 유용하다.
- 바로 할 일: Hermes와 mv-brain 작업 지침에 “권한 우회 금지 또는 명시 승인” 체크를 유지하고, Claude Code 상태 꼬임이 생길 때는 먼저 `purge --dry-run` 패턴만 참고한다.

### 2. OpenAI Codex rust-v0.128.0 — 지속 `/goal`, 권한 프로필, 외부 세션 import
- 링크: https://github.com/openai/codex/releases/tag/rust-v0.128.0
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: `openai/codex`, stars=79575, pushed_at=2026-05-02T23:59:28Z, maintained=yes
- 왜 중요: 장기 작업을 `/goal`로 만들고 pause/resume/clear 하는 방향이 명확해졌다. permission profile, sandbox cwd/profile metadata, external agent session import는 Hermes cron과 대화형 Codex 작업의 경계 설계에 직접 연결된다.
- 바로 할 일: mv-brain에서 “demo rough-cut export” 같은 작은 목표를 `/goal` 단위로 기록하는 운영 메모를 실험 후보로 둔다. 단, 설치/설정 변경은 하지 않는다.

### 3. Playwright MCP v0.0.73 + 브라우저/devtool MCP 패턴
- 링크: https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: `microsoft/playwright-mcp`, stars=31917, pushed_at=2026-05-01T22:35:25Z, maintained=yes
- 왜 중요: Playwright MCP가 release마다 official MCP Registry에 배포된다. 브라우저 제어/스크린샷/대시보드 검증을 “표준 MCP 도구”로 붙이는 쪽이 안정화되는 신호다.
- 바로 할 일: mv-brain UI가 커지면 무거운 E2E 전에 “로컬 페이지 열기 → 스크린샷 → 핵심 버튼 확인” 체크리스트를 MCP/Playwright 패턴으로 설계한다.

### 4. wkdomains/macos-app — 사람이 보는 브라우저를 Codex/Claude Code가 읽는 로컬 API로 노출
- 링크: https://github.com/wkdomains/macos-app
- 출처/신뢰도: GitHub/HN, medium
- GitHub 검증: stars=0, pushed_at=2026-05-03T00:00:03Z, license=MIT, maintained=very fresh but unproven
- 왜 중요: “인증 브라우저 컨텍스트를 재사용”한다는 설명은 위험하지만, agent가 현재 페이지/screenshot/XHR shape을 읽는 로컬 dev-browser 아이디어는 유용하다. 로그인/쿠키 우회 목적이 아니라, 사람이 띄운 로컬 앱 디버깅 보조로 쓰는 패턴만 참고 가치가 있다.
- 바로 할 일: 설치 보류. mv-brain에는 “현재 편집 화면의 DOM/스크린샷을 에이전트가 읽어 UX 피드백” 데모 아이디어로만 보관.

### 5. OWASP Agent Security Regression Harness — MCP/agent 보안 회귀 테스트
- 링크: https://github.com/OWASP/Agent-Security-Regression-Harness
- 출처/신뢰도: GitHub/HN, medium-high
- GitHub 검증: stars=8, pushed_at=2026-05-02T17:43:04Z, license=Apache-2.0, maintained=fresh
- 왜 중요: agentic app/MCP 통합의 보안 회귀 테스트를 실행 가능한 harness로 만든다는 방향은 Hermes의 “도구 권한/시크릿 노출/쓰기 제한” 검증과 잘 맞는다.
- 바로 할 일: 설치하지 말고 테스트 케이스 이름/구조만 읽어 Hermes skill guardrail 체크리스트로 추출할 후보.

## 한국/Threads 신호
- DuckDuckGo public search에서 `site:x.com "Claude Code" "MCP" github.com`는 “Claude Code Resource Bible”류 X 결과를 찾았지만, 결과 스니펫만으로 검증 가능한 GitHub repo가 없어서 추천 제외.
- Threads 검색(`site:threads.net "Claude Code" github.com`, `site:threads.net "AI 코딩" "github.com"`)은 공개 검색에서 결과 없음/검색 차단 성격이라 유효 신호 없음.
- 한국어 검색(`"클로드 코드" "github.com"`, `"커서" "MCP" "github.com"`, Velog/Disquiet 제한 검색)은 DuckDuckGo/Jina/Google/Bing 경로에서 429·차단·무결과가 섞여 검증 가능한 GitHub URL을 확보하지 못했다.
- 결론: 오늘 한국/Threads 쪽은 “추천 가능한 repo 신호 없음”. social-only 결과는 보류.

## 우리한테 적용
- mv-brain: Codex `/goal`식 목표 단위(예: clip triage demo, FCPXML rough-cut export)를 build-log에 남기고, Playwright MCP 패턴으로 UI 스모크 체크를 설계한다. wkdomains류는 “사람이 보는 로컬 UI를 에이전트가 리뷰”하는 데모 각도만 참고.
- Hermes/자동화: Claude Code 권한 우회 확대는 위험 신호다. 크론/자동화에는 `--dangerously-skip-permissions`류 우회 금지, dry-run 우선, writable folder 제한을 계속 유지한다. OWASP harness는 향후 skill/도구 보안 체크리스트 추출 후보.
- 콘텐츠/홍보: “AI 코딩 툴 뉴스”보다 “권한·세션·브라우저 검증이 실제 자동화 품질을 좌우한다”는 build-in-public 포스트가 좋다.

## 콘텐츠 초안
### Threads/X
AI 코딩 툴이 빨라지는 지점은 모델보다 “세션/권한/검증” 쪽이다.

- Claude Code: 권한 우회 범위가 넓어져 자동화에서는 더 조심해야 함
- Codex: `/goal` 지속 워크플로 + 권한 프로필 강화
- Playwright MCP: 브라우저 검증이 표준 MCP 배포 흐름으로 이동
- OWASP: agent/MCP 보안 회귀 테스트가 repo 단위로 등장

내 결론: 에이전트 자동화는 “더 자율적으로”보다 “더 복구 가능하고 검증 가능하게” 설계해야 한다.

### LinkedIn
이번 주 AI coding agent 흐름에서 가장 실무적인 변화는 모델 성능이 아니라 운영 계층입니다. Codex는 지속 가능한 `/goal` workflow와 권한 profile을 강화했고, Claude Code는 permission bypass 범위를 넓혔습니다. Playwright MCP는 official MCP Registry 배포 흐름을 탔고, OWASP 쪽에서는 agent/MCP 보안 회귀 테스트 harness가 등장했습니다. 제가 mv-brain/Hermes에 적용할 방향은 단순합니다: 목표 단위 로그, dry-run 우선, UI 스모크 체크, 권한 우회 금지.

## 버릴 것/보류
- X “Resource Bible/10x repo”류 목록 글: 링크 스니펫만 있고 repo 검증이 안 되어 보류.
- gstack 파생 repo 다수: `garrytan/gstack` 자체는 신호가 크지만, stars 0~3 파생 skill marketplace/게임/한국 SMB repo들은 아직 채택 신호 부족.
- `open-computer-use`: stars=64, fresh지만 Docker computer-use/MCP는 권한·네트워크·격리 위험이 커서 설치 보류. 아이디어 참고만.
- Reddit의 “local LLM으로 Plan.md 실행”, “멀티 에이전트 세션 관리 피로”는 방향성은 맞지만, 오늘은 공식/검증 repo보다 낮은 신뢰도로 watch.

## 조사 방법/한계
- 사용: GitHub REST API, GitHub releases, pre-run Reddit public JSON/Algolia HN context, DuckDuckGo HTML, Jina Reader 경유 Google/Bing 시도.
- 제한: Google/Jina 검색 일부는 429/CAPTCHA 페이지 반환. Threads와 한국어 공개 검색은 검증 가능한 GitHub URL 확보 실패.
- 안전: no login, no cookies, no credentials, no mutation.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
