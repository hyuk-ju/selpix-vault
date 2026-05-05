---
type: research
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - https://github.com/openai/codex/releases/tag/rust-v0.128.0
  - https://github.com/anthropics/claude-code/releases/tag/v2.1.123
  - https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.1
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.72
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.30
  - https://github.com/cline/cline/releases/tag/v3.81.0
  - https://github.com/nizos/conduct
  - https://github.com/Fr-e-d/GAAI-framework
  - https://github.com/trycua/cua
  - https://github.com/garrytan/gstack
  - https://github.com/agentrq/agentrq
  - https://github.com/context-labs/HALO
  - https://news.ycombinator.com/item?id=47969781
  - https://pu.dev/
  - https://cursor.com/blog/typescript-sdk
  - https://www.reddit.com/r/ClaudeAI/comments/1sztkml/tdd_and_rules_enforcement_using_hooks/
  - https://www.reddit.com/r/ClaudeAI/comments/1syvmnb/leaked_my_anthropic_key_into_a_public_repo_lost/
created: 2026-05-01
reviewed_at: 2026-05-01
---
# AI 코딩 빠른 레이더 — 2026-05-01 09:01 KST

## 핵심 3줄
1. OpenAI Codex `rust-v0.128.0`이 `/goal` 지속 워크플로, pause/resume, 권한 프로필, 플러그인 훅을 넣었다. “세션을 이어가는 터미널 에이전트” 쪽 변화가 가장 실전적이다.
2. Playwright MCP `v0.0.72`는 네트워크 요청 세부 조회와 `browser_run_code_unsafe` 명시화를 추가했다. 브라우저 자동화 MCP는 편해지는 동시에 권한 라벨링이 중요해졌다.
3. Reddit/HN/GitHub 신호는 “더 똑똑한 모델”보다 `TDD/정책 엔진`, `메모리/세션 복구`, `로컬 로그/사용량 관측`, `비밀키 유출 방지` 쪽으로 이동 중이다.

## 주목할 것

### 1. OpenAI Codex `rust-v0.128.0`: 지속 목표 워크플로 + 권한 프로필
- 링크: https://github.com/openai/codex/releases/tag/rust-v0.128.0
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: `openai/codex`, stars=79162, pushed_at=2026-05-01T00:01:34Z, maintained=예, license=Apache-2.0
- 왜 중요: `/goal` 생성·일시정지·재개·삭제, runtime continuation, TUI controls는 크론형 작업/긴 리팩터링/문서화 작업을 끊었다 이어가기 좋다. 권한 프로필과 sandbox profile 선택도 Hermes식 안전 실행과 맞다.
- 바로 할 일: `mv-brain`에는 “오늘의 작은 goal → 검증 명령 → build-log seed” 템플릿을 만들고, Hermes에는 권한 프로필/goal 상태를 최종 보고에 남기는 패턴을 차용.

### 2. Playwright MCP `v0.0.72`: 네트워크 요청 조회 강화 + unsafe 명명
- 링크: https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.72
- 출처/신뢰도: official/GitHub, high
- GitHub 검증: `microsoft/playwright-mcp`, stars=31848, pushed_at=2026-05-01T00:00:58Z, maintained=예, license=Apache-2.0
- 왜 중요: `browser_network_requests`가 번호 목록을 주고 `browser_network_request`로 headers/body를 상세 조회 가능. `browser_run_code`가 `browser_run_code_unsafe`로 바뀐 것은 권한/위험 표시의 좋은 사례다.
- 바로 할 일: commerce 공개 조사나 mv-brain 데모 페이지 점검에서 “네트워크 캡처는 파일로 저장, unsafe tool은 명시 승인 전 금지” 규칙을 Runbook 후보로 둔다.

### 3. Conduct: 코딩 에이전트용 TDD/정책 엔진
- 링크: https://github.com/nizos/conduct
- Reddit: https://www.reddit.com/r/ClaudeAI/comments/1sztkml/tdd_and_rules_enforcement_using_hooks/
- 출처/신뢰도: Reddit + GitHub 검증, medium
- GitHub 검증: `nizos/conduct`, stars=29, pushed_at=2026-04-29T06:41:48Z, maintained=예, license=MIT, description=`Process discipline for AI coding agents`
- 왜 중요: Claude Code/Codex/GitHub Copilot CLI/VS Code Chat 등 여러 도구에 대해 TDD 규칙을 강제하려는 흐름. “에이전트가 빨리 짜는 코드”보다 “테스트를 먼저/계속 통과하게 하는 가드”가 실전 가치가 높다.
- 바로 할 일: 설치는 보류. 대신 `mv-brain` 작업 프롬프트에 `compileall`, UI build, provider/media 금지 조건을 Conduct식 pre/post 체크리스트로 반영.

### 4. GAAI-framework: `.gaai/` 폴더 기반 agent delivery framework
- 링크: https://github.com/Fr-e-d/GAAI-framework
- 출처/신뢰도: GitHub search + API 검증, medium
- GitHub 검증: stars=135, pushed_at=2026-04-30T23:53:08Z, maintained=예, license=NOASSERTION
- 왜 중요: SDK 없이 Markdown/YAML/bash로 Discovery → Delivery → acceptance criteria를 굴리는 방식. Claude Code/Codex/Gemini/Cursor 등 다중 툴을 같은 프로젝트 규약으로 묶는 실험이다.
- 바로 할 일: 그대로 도입하지 말고 `mv-brain/AGENTS.md`와 Obsidian build-log 규칙에 “작업 정의/검증 명령/금지 작업/결과물 경로” 4칸 템플릿만 추출.

### 5. 로컬 관측/메모리/컴퓨터 유즈 신호: StackUnderflow, codex-viz, project-memory-skill, CUA
- 링크:
  - https://github.com/0bserver07/StackUnderflow
  - https://github.com/caua68/codex-viz
  - https://github.com/tasuku-9/project-memory-skill
  - https://github.com/trycua/cua
- 출처/신뢰도: GitHub search/API, medium. CUA는 high에 가까움.
- GitHub 검증:
  - `0bserver07/StackUnderflow`, stars=1, pushed_at=2026-04-30T23:59:51Z, license=MIT
  - `caua68/codex-viz`, stars=1, pushed_at=2026-04-30T23:55:49Z
  - `tasuku-9/project-memory-skill`, stars=2, pushed_at=2026-04-30T23:55:43Z
  - `trycua/cua`, stars=15396, pushed_at=2026-04-30T21:28:38Z, license=MIT
- 왜 중요: 아직 초기 repo가 많지만 방향은 명확하다. CLI 세션 로그를 로컬에서 검색/리플레이/토큰·툴 사용량 분석하고, 컴퓨터 유즈는 sandbox/benchmark 기반으로 가는 중이다.
- 바로 할 일: 새 도구 설치 대신 Hermes 크론 결과에 `evidence used`, `commands run`, `unsafe skipped` 필드를 더 명확히 넣는 쪽이 즉시 효과적.

## 한국/Threads/X 신호
- DuckDuckGo public search로 `site:x.com "Claude Code" "MCP" github.com`에서 GitHub URL 일부가 노출됨: `affaan-m/everything-claude-code`, `hkuds/lightrag`, `czlonkowski/n8n-mcp`.
  - 검증: `affaan-m/everything-claude-code` stars=170956, pushed_at=2026-04-30T16:25:17Z, license=MIT. Claude/Codex/OpenCode/Cursor용 harness/skills/memory/security 묶음으로 강한 신호지만, X 검색 결과 기반이므로 홍보성 가능성 있음.
  - 검증: `HKUDS/LightRAG` stars=34626, pushed_at=2026-04-30T19:40:05Z, license=MIT. RAG 일반 도구로 AI coding 직접성은 낮음.
  - 검증: `czlonkowski/n8n-mcp` stars=18938, pushed_at=2026-04-30T21:22:38Z, license=MIT. Cursor/Windsurf/Claude Code용 n8n workflow MCP. commerce 자동화 아이디어에는 참고 가치 있음.
- Threads/Velog/Tistory/Disquiet/한국어 쿼리는 이번 저용량 검색에서 추천할 만한 신규 GitHub-backed 자료가 확인되지 않음.
- X/Threads 결과는 로그인 없는 검색 스니펫 기반이라 신뢰도 medium 이하. 추천 본문에는 GitHub/API로 검증된 repo만 사용.

## 우리한테 적용
- mv-brain:
  - Codex `/goal`식으로 “작은 목표 + 안전 검증 + 이어가기 가능한 상태”를 build-log 단위로 쪼개기.
  - Conduct/GAAI에서 TDD/acceptance gate 패턴만 가져와 `python3 -m compileall -q mv_brain`, UI build, provider/media 금지 조건을 매 작업의 고정 체크로 둔다.
  - Playwright MCP 변화는 향후 웹 데모/문서 스크린샷 자동 점검에 쓸 수 있으나, unsafe 실행은 금지 라벨을 명확히 한다.
- Hermes/자동화:
  - 크론 보고에 `권한`, `사용한 소스`, `건너뛴 위험 작업`, `검증 명령`을 더 표준화.
  - MCP/브라우저 자동화는 공개 조사에만 제한하고, 네트워크 요청 body 저장은 비밀/토큰 포함 가능성을 먼저 경고.
- 콘텐츠/홍보:
  - 포스트 훅: “AI 코딩 에이전트의 다음 경쟁력은 모델이 아니라 체크포인트/권한/테스트/로그다.”
  - build-in-public 소재: `mv-brain`에 “agent-safe checklist”를 붙이고 실제 실패 방지 사례를 짧게 공유.
  - commerce 운영에도 “AI가 작업했지만 무엇을 읽었고 무엇을 안 했는지 남기는 자동화”를 신뢰 포인트로 포장 가능.

## 버릴 것/보류
- Reddit의 비밀키 유출 비용 주장: 사례 자체는 보안 경고로 유용하지만 금액/상황은 검증 불가. 결론은 “키를 repo에 넣지 말고 로컬/권한/스캔을 강화” 수준으로만 사용.
- HN의 “MCP 서버 100개 스캔, 22개 flagged”: 링크는 있으나 상세 보고서/방법론 확인 전까지 공포 마케팅으로 쓰지 않기.
- 별 0~2개의 gstack 파생/스킬 repo 다수: 아이디어 참고는 가능하지만 운영에 바로 도입하지 않기.
- Cursor/Composer 관련 사회적 논쟁, 모델 비교, 가격 플랜 토론: 이번 주 실행 변화가 작아 보류.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
