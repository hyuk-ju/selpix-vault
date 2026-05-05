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
  - https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.0
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.71
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.29
  - https://github.com/trycua/cua
  - https://github.com/yakkomajuri/agentport
  - https://github.com/Ege-Deniz/brain-operator
  - https://github.com/ndycode/codex-multi-auth
  - https://code.claude.com/docs/en/champion-kit
  - https://news.ycombinator.com/item?id=47945185
created: 2026-04-29
reviewed_at: 2026-04-29
---

# AI 코딩 빠른 레이더 — 2026-04-29 22:03 KST

## 핵심 3줄

1. 이번 신호의 중심은 “더 똑똑한 모델”보다 **권한·세션·작업공간 관리**입니다. Codex, OpenCode, Playwright MCP, CUA가 모두 실제 자동화 안정성 쪽으로 움직입니다.
2. `mv-brain`에는 Playwright MCP의 드래그앤드롭 도구와 CUA의 백그라운드 컴퓨터 유즈가 데모/편집 UI 테스트에 바로 연결됩니다.
3. Hermes에는 `brain-operator`식 Agent Receipts/Safe Apply와 `agentport`식 destructive-op 승인 게이트가 재사용할 만합니다.

## 주목할 것

### 1. Playwright MCP v0.0.71 — browser_drop 추가

- 링크: https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.71
- 출처/신뢰도: GitHub release/official, high
- GitHub 검증: `microsoft/playwright-mcp`, stars 31,766, pushed_at 2026-04-27T20:52:22Z, maintained
- 왜 중요: `browser_drop`가 MCP 도구로 추가되어 드래그앤드롭 UI를 에이전트가 직접 다룰 수 있습니다. 편집 타임라인/클립 배치/파일 업로드 UI 테스트에 유용합니다.
- 바로 할 일: `mv-brain` UI에 “클립을 타임라인으로 드롭”하는 Playwright MCP 데모 시나리오를 1개만 설계합니다. 설치/실행은 별도 승인 후.

### 2. Codex rust-v0.125.0 — app-server/session 쪽 강화

- 링크: https://github.com/openai/codex/releases/tag/rust-v0.125.0
- 출처/신뢰도: GitHub release/official, high
- GitHub 검증: `openai/codex`, stars 78,806, pushed_at 2026-04-29T12:30:44Z, maintained
- 왜 중요: Unix socket transport, resume/fork, remote thread config/store plumbing 등 장기 실행·원격 세션 관리에 가까운 변경이 포함되었습니다.
- 바로 할 일: Hermes cron/agent 작업 로그에서 “thread id, cwd, git status, tool receipts”를 남기는 얇은 세션 영수증 포맷을 먼저 도입할 가치가 있습니다.

### 3. CUA — background computer-use가 HN에서 강한 반응

- 링크: https://github.com/trycua/cua
- 출처/신뢰도: HN + GitHub, high
- GitHub 검증: `trycua/cua`, stars 15,122, pushed_at 2026-04-27T19:21:01Z, MIT, maintained
- 왜 중요: “커서를 훔치지 않고 macOS 앱을 백그라운드에서 조작”하는 방향은 영상 편집/브라우저/데스크톱 자동화 데모에 강합니다. HN 신호도 points 150/comments 34로 큽니다.
- 바로 할 일: 실제 설치는 보류. 콘텐츠 관점에서는 “AI 영상 편집 에이전트가 로컬 앱을 직접 만질 때 필요한 조건: 격리, 체크포인트, 승인” 글감으로 좋습니다.

### 4. Agent permission/security gateway — agentport / Cordon

- 링크: https://github.com/yakkomajuri/agentport , https://github.com/marras0914/cordon
- 출처/신뢰도: HN + GitHub, medium
- GitHub 검증: `yakkomajuri/agentport` stars 5, pushed_at 2026-04-28T22:35:41Z, MIT, maintained. `marras0914/cordon` stars 0, pushed_at 2026-04-28T20:37:04Z, MIT, maintained but tiny.
- 왜 중요: MCP/tool 호출에 사람 승인과 세밀 권한을 붙이는 흐름입니다. 실수로 production 삭제, 키 노출, 외부 서비스 호출하는 사고를 줄이는 방향입니다.
- 바로 할 일: Hermes에는 “읽기 전용/쓰기 전 확인/파괴적 작업 금지”를 별도 체크리스트로 고정하고, mv-brain에는 provider/media mutation 전에 승인 게이트 문구를 README에 반영합니다.

### 5. brain-operator — markdown vault 기반 Agent Receipts / Safe Apply

- 링크: https://github.com/Ege-Deniz/brain-operator
- 출처/신뢰도: GitHub search, medium
- GitHub 검증: `Ege-Deniz/brain-operator`, stars 1, pushed_at 2026-04-29T13:02:12Z, description 확인, 신생 repo
- 왜 중요: OpenClaw/Hermes/Obsidian 구조와 맞는 패턴입니다. 답변 추적, agent receipts, fenced handoffs, safe apply는 우리 운영 노트와 자동화 로그에 맞습니다.
- 바로 할 일: 설치하지 말고 패턴만 차용합니다. 다음 Hermes 보고서부터 `evidence`, `action`, `risk`, `rollback` 4개 필드를 고정해도 효과가 큽니다.

## 한국/Threads 신호

- DuckDuckGo 공개 검색으로 `site:threads.net "클로드 코드" github.com`, `site:velog.io "Claude Code" github.com`, `site:disquiet.io "AI 코딩" github.com`, `"커서" "MCP" "github.com"`를 저용량 확인했습니다.
- 이번 실행에서는 추천할 만한 한국어/Threads 원문 + 검증 가능한 GitHub repo 조합은 발견하지 못했습니다.
- X 공개 검색(`site:x.com "Claude Code" "MCP" github.com`)은 일부 GitHub URL이 잡혔지만 문맥이 약하고 검증 가치가 낮아 추천 제외했습니다.

## 우리한테 적용

- mv-brain: Playwright MCP `browser_drop`를 이용한 “클립 드래그 → 타임라인 배치” 테스트/데모가 가장 실용적입니다. CUA는 장기적으로 로컬 편집 앱 제어 스토리텔링에 사용하되 지금은 설치 보류.
- Hermes/자동화: Agent Receipts, Safe Apply, destructive-op approval gate를 우선 적용합니다. broad autonomous loop보다 각 실행의 증거와 rollback을 남기는 쪽이 안전합니다.
- 콘텐츠/홍보: “AI 코딩 도구의 다음 병목은 모델이 아니라 권한·세션·작업공간이다”라는 빌드로그/스레드 소재가 좋습니다. `mv-brain`과 연결하면 “영상 편집 에이전트도 체크포인트와 승인 없이는 위험하다”로 포지셔닝 가능.

## 버릴 것/보류

- `ndycode/codex-multi-auth`: stars 94, pushed_at 2026-04-29T13:00:54Z로 활동성은 있으나 OAuth/다중 계정/쿼터 회전 성격이라 운영 리스크가 커서 추천 제외.
- Reddit의 “키 유출로 비용 손실”류 글: 보안 교훈은 유효하지만 원문 검증이 약하고 선정적이라 직접 추천 제외. 대신 승인 게이트/secret scan 체크리스트에 반영.
- 신생 `agent memory`, `checkpoint`, `multi-agent chat` repo 다수: stars 0~1이 많고 실사용 검증 부족. 패턴만 관찰.
- X/Threads/Korean social-only 주장: GitHub/docs/release로 확인되지 않으면 보류.

## 콘텐츠 초안

### Threads/X

AI 코딩 도구의 다음 병목은 “모델 성능”보다 권한·세션·작업공간이다.

- Codex는 resume/fork/session 쪽이 강화되는 중
- Playwright MCP는 드래그앤드롭 같은 실제 UI 조작 도구가 늘어남
- CUA는 백그라운드 computer-use로 데스크톱 자동화 가능성을 보여줌
- agentport/Cordon은 MCP 호출에 승인 게이트를 붙이는 흐름

내 결론: 에이전트를 더 오래 돌리기 전에, 먼저 영수증·체크포인트·롤백부터 만들어야 한다.

### LinkedIn

이번 주 AI coding agent 신호를 보면, 경쟁 포인트가 “누가 더 긴 코드를 쓰느냐”에서 “누가 더 안전하게 실제 작업공간을 다루느냐”로 이동하고 있습니다.

Codex는 세션/원격 작업 흐름을 강화하고 있고, Playwright MCP는 드래그앤드롭 같은 브라우저 조작을 도구화하고 있습니다. CUA는 백그라운드 computer-use를 보여줬고, agentport/Cordon은 MCP 호출에 권한과 승인 레이어를 붙이려 합니다.

제가 `mv-brain` 같은 로컬 영상 편집 에이전트를 만들 때도 같은 결론입니다. 자동화보다 먼저 필요한 것은 체크포인트, 승인, 실행 영수증, 롤백입니다.

## 관련 문서

- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
