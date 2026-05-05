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
  - https://github.com/aattaran/deepclaude
  - https://github.com/MinishLab/semble
  - https://github.com/smithy-ai/smithy-ai
  - https://github.com/garrytan/gstack
  - https://github.com/fivetaku/insane-search
  - https://news.ycombinator.com/item?id=48002447
  - https://reddit.com/r/ClaudeAI/comments/1t38e7c/claudely_launch_claude_code_against_local_llm/
  - https://reddit.com/r/cursor/comments/1t2gjtl/cursor_silently_switched_models_while_i_was_deep/
created: 2026-05-04
reviewed_at: 2026-05-04
---
# AI 코딩 빠른 레이더 — 2026-05-04 15:02 KST

## 핵심 3줄
1. 오늘 강한 신호는 “새 모델”보다 **에이전트 실행비용·검색토큰·체크포인트/오케스트레이션**이다.
2. HN에서 `deepclaude`와 `semble`이 실사용형으로 부상했다. 둘 다 GitHub 검증 완료, 최근 push가 있다.
3. 한국/Threads/X 검색은 공개 검색 기준으로 추천할 만한 검증 GitHub 링크가 약했다. 소셜 단독 주장은 보류한다.

## 주목할 것

### 1. DeepClaude — Claude Code 루프를 Anthropic-compatible backend로 돌리는 비용 절감 레이어
- 링크: https://github.com/aattaran/deepclaude
- 출처/신뢰도: HN + GitHub, medium-high
- GitHub 검증: stars 301, forks 16, pushed_at 2026-05-04T03:13:02Z, MIT, maintained로 보임
- 왜 중요: Claude Code식 agent loop를 유지하면서 DeepSeek V4 Pro/OpenRouter/Anthropic-compatible backend로 비용을 낮추려는 흐름. 장기 작업·리팩터링·문서화 크론 비용 압박을 줄이는 방향성이다.
- 바로 할 일: 설치하지 말고 README/동작방식만 읽어 Hermes 크론에서 “저비용 모델로 가능한 작업 / Pro OAuth가 필요한 작업” 분리 아이디어로 기록.

### 2. Semble — grep/read 대비 토큰을 줄이는 에이전트용 코드 검색
- 링크: https://github.com/MinishLab/semble
- 출처/신뢰도: HN + GitHub, high
- GitHub 검증: stars 569, forks 50, pushed_at 2026-05-03T18:35:28Z, MIT, maintained로 보임
- 왜 중요: mv-brain처럼 코드와 문서가 같이 커지는 repo에서 에이전트가 매번 `grep → read_file`로 과도한 컨텍스트를 먹는 문제를 줄일 수 있다.
- 바로 할 일: mv-brain에 바로 도입하지 말고, README/벤치 방식을 확인해 “safe read-only code search 비교” 콘텐츠 또는 내부 실험 후보로 둔다.

### 3. smithy-ai — 이슈 트래커에서 Dockerized Claude Code 세션 오케스트레이션
- 링크: https://github.com/smithy-ai/smithy-ai
- 출처/신뢰도: HN Show HN + GitHub, medium
- GitHub 검증: stars 7, forks 1, pushed_at 2026-05-03T17:45:34Z, AGPL-3.0, 초기 repo
- 왜 중요: “이슈 → 격리된 에이전트 세션 → 결과 회수” 패턴은 Hermes cron과 mv-brain 작업 큐에 맞다. 단, AGPL/초기성 때문에 제품 의존은 위험.
- 바로 할 일: 코드 도입보다 패턴만 차용: `issue/task spec`, `sandbox`, `checkpoint`, `human review`를 Hermes runbook에 반영 후보.

### 4. gstack / everything-claude-code — 스킬·역할·검토 루틴의 대중화
- 링크: https://github.com/garrytan/gstack / https://github.com/affaan-m/everything-claude-code
- 출처/신뢰도: GitHub, medium-high
- GitHub 검증: gstack stars 88,773, pushed_at 2026-05-04T03:54:06Z, MIT. everything-claude-code stars 172,813, pushed_at 2026-05-03T04:53:48Z, MIT.
- 왜 중요: CEO/Designer/QA 같은 역할 자체보다 `guard`, `review`, `investigate`, `context-save/restore` 같은 반복 가능한 작업 단위가 가치 있다.
- 바로 할 일: 설치하지 말고 Hermes 스킬에 이미 있는 실행 규율과 비교해 “작업 전 조사 → 변경 → 검증 → 기록” 체크리스트를 더 짧게 다듬을 후보.

### 5. Playwright MCP / insane-search — 브라우저·차단 페이지 조사 워크플로우
- 링크: https://github.com/microsoft/playwright-mcp / https://github.com/fivetaku/insane-search
- 출처/신뢰도: GitHub/official, high for Playwright MCP, medium for insane-search
- GitHub 검증: Playwright MCP stars 31,961, pushed_at 2026-05-01T22:35:25Z, Apache-2.0. insane-search stars 603, pushed_at 2026-05-03T15:36:14Z, MIT.
- 왜 중요: X/Threads/쿠팡/네이버처럼 막히는 공개 페이지 조사에서 “API/Jina/mobile/browser render 순서”가 운영 안전성을 높인다.
- 바로 할 일: commerce 리서치에는 대량 scraping 금지. 공개 조사용 fallback runbook으로만 유지.

## 한국/Threads 신호
- DuckDuckGo 공개 HTML 검색으로 `site:threads.net "Claude Code" github.com`, `site:x.com "Claude Code" "MCP" github.com`, `site:velog.io "Claude Code" "github.com"`, `"클로드 코드" "github.com"`, `"커서" "MCP" "github.com"`를 저용량 확인했다.
- 추천할 만큼 명확히 추출·검증 가능한 `github.com/{owner}/{repo}` 링크는 발견하지 못했다.
- X 검색 결과 제목 일부는 Claude Code 리소스/레포 모음으로 보였지만, 공개 검색 결과만으로 repo URL 검증이 안 되어 추천 제외.

## Reddit/HN 관찰
- r/ClaudeAI: Claude Code를 local LLM provider와 연결하려는 `claudely`류 니즈가 보임. 단, Reddit-only라 낮은 신뢰.
- r/Cursor: 모델이 조용히 바뀌어 리뷰/수정 작업을 망쳤다는 불만 신호. 운영적으로는 “모델·설정·diff checkpoint 기록”이 중요.
- HN: `The agent harness belongs outside the sandbox` 논의가 강함. 에이전트 실행부와 제어/감사 harness를 분리하라는 방향은 Hermes와 잘 맞는다.

## 우리한테 적용
- mv-brain: `Semble`류 코드 검색 최적화와 `issue → sandbox → checkpoint → review` 패턴을 포트폴리오 글감으로 쓰기 좋다. 실제 도입 전에는 compile/test baseline과 read-only 비교부터.
- Hermes/자동화: 비용 절감 backend보다 먼저 작업 분리 기준을 세운다. 예: 검색/요약은 저비용, 파일 수정/검증은 현재 Codex/Hermes, 민감 작업은 human review.
- 콘텐츠/홍보: “AI 코딩 도구를 많이 쓰는 법”보다 “토큰 낭비 줄이기, 체크포인트 남기기, 모델 변경 사고 막기”가 실무형 콘텐츠로 강하다.

## 버릴 것/보류
- `terminalize`, `OpenRelay`, `DevMemoryOS`, `eidetic_engine_cli`: 설명은 흥미롭지만 stars 0~9 수준 또는 초기 repo라 watch만.
- X/Threads/Korean social-only 리소스 모음: GitHub URL 검증 실패로 보류.
- account pooling/auto rotate 류 도구: 계정 정책·운영 리스크가 커서 추천 제외.
- “완전 자율 multi-agent swarm”류: 현재 Hermes 원칙과 충돌. 작은 검증 루프만 차용.

## 콘텐츠 변환 씨앗

### Threads/X 초안
AI 코딩에서 이번 주 실무 신호는 “더 똑똑한 모델”보다 “덜 망가지는 작업 방식”에 가깝다.
- 코드 검색 토큰 줄이기: Semble
- 에이전트 비용 분리: DeepClaude류
- 이슈 기반 sandbox 실행: smithy-ai 패턴
- 모델 변경 사고 방지: checkpoint/log
- 스킬팩은 설치보다 guard/review 루틴만 차용

핵심: 에이전트에게 자유를 주기 전에, 복구 지점을 먼저 만든다.

### LinkedIn 초안
이번 AI coding radar에서 가장 실용적인 흐름은 agent autonomy가 아니라 운영 안전장치였습니다. 코드 검색 비용을 줄이는 도구, sandboxed issue runner, human checkpoint, model setting 기록 같은 요소가 반복적으로 등장했습니다. mv-brain/Hermes 같은 개인 프로젝트에도 바로 적용할 수 있는 교훈은 단순합니다: 자동화를 늘리기 전에 diff, checkpoint, model setting, evidence log를 남겨야 합니다.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
