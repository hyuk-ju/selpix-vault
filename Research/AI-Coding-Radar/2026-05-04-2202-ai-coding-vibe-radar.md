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
  - https://github.com/Mibayy/token-savior
  - https://github.com/MinishLab/semble
  - https://github.com/boshu2/agentops
  - https://github.com/noor-malaika/prism
  - https://github.com/huahuadeliaoliao/long-long-run
  - https://github.com/fivetaku/insane-search
  - https://reddit.com/r/ClaudeAI/comments/1t3du61/your_claude_code_agent_is_always_working_from/
  - https://reddit.com/r/ClaudeAI/comments/1t3gdif/prism_mcp_a_tool_to_bridge_claude_code_with_vs/
  - https://reddit.com/r/codex/comments/1t3cavb/i_got_tired_of_telling_codex_continue_so_i_built/
  - https://news.ycombinator.com/item?id=48007425
  - https://github.com/smithy-ai/smithy-ai
created: 2026-05-04
reviewed_at: 2026-05-04
---

# AI 코딩 빠른 레이더 — 2026-05-04 22:02 KST

## 핵심 3줄
1. 대형 에이전트 CLI는 모두 최근 릴리스/푸시가 살아있다: Claude Code v2.1.126, Codex rust-v0.128.0, OpenCode v1.14.33, Cline v3.82.0, Gemini CLI v0.40.1, Playwright MCP v0.0.73.
2. 이번 신호의 실전 축은 “더 똑똑한 모델”보다 `토큰 절약 코드 검색`, `세션/메모리 복원`, `MCP 권한/감사`, `장시간 작업 계속 실행`이다.
3. mv-brain/Hermes에 바로 가져올 패턴은 grep 남발을 줄이는 구조 검색, 크론/에이전트 작업의 로컬 감사 로그, 실패 시 복구 가능한 checkpoint/mainline 기록이다.

## 주목할 것

### 1. Mibayy/token-savior — 구조 코드 탐색 + persistent memory MCP
- 링크: https://github.com/Mibayy/token-savior
- 출처/신뢰도: GitHub + Reddit 방향 신호, medium-high
- GitHub 검증: stars 785, forks 57, pushed_at 2026-05-04T12:25:18Z, MIT, maintained yes
- 왜 중요: “코드베이스를 매번 grep/read로 재소모하지 말고 구조 탐색 + 지속 메모리로 토큰/시간을 줄이자”는 패턴. Reddit의 stale context 문제 제기와도 맞물린다.
- 바로 할 일: 설치는 보류. README/API만 읽고 mv-brain에서 `repo-lens/search-summary` 같은 read-only 구조 요약 명령을 설계할 가치가 있다.

### 2. MinishLab/semble — agent용 저토큰 코드 검색
- 링크: https://github.com/MinishLab/semble
- 출처/신뢰도: GitHub + HN Show HN, high
- GitHub 검증: stars 594, forks 51, pushed_at 2026-05-04T09:12:44Z, MIT, maintained yes
- 왜 중요: HN에 “grep 대비 토큰 98% 절감”으로 소개된 코드 검색 도구. 수치 자체는 벤치 확인 필요지만, 에이전트에게 전체 파일을 읽히기 전 좁은 후보를 주는 방향은 즉시 유용하다.
- 바로 할 일: Hermes 스킬에 “먼저 search_files/read_file로 좁히기” 규칙을 더 엄격히 적용. mv-brain에도 `clip_search`, `fx_recommend` 쪽에서 결과 압축 포맷을 콘텐츠 소재로 만들기.

### 3. boshu2/agentops — 코딩 에이전트 운영 레이어
- 링크: https://github.com/boshu2/agentops
- 출처/신뢰도: GitHub, medium
- GitHub 검증: stars 328, forks 34, pushed_at 2026-05-04T12:18:43Z, license NOASSERTION, maintained yes
- 왜 중요: memory/validation/feedback loop를 세션 간 누적하는 운영 레이어. Hermes cron과 비슷한 문제(작업은 반복되는데 맥락·검증·피드백이 흩어짐)를 겨냥한다.
- 바로 할 일: 외부 도구 도입보다 “작업 시작 맥락 → 실행 증거 → 검증 → 다음 액션” 로그 스키마를 Runbook/cron 출력에 통일하는 쪽이 안전하다.

### 4. Prism MCP — Claude Code와 VS Code LSP 연결
- 링크: https://github.com/noor-malaika/prism
- 출처/신뢰도: Reddit + GitHub 검증, medium
- GitHub 검증: stars 0, forks 0, pushed_at 2026-05-04T09:03:56Z, MIT, maintained yes but very early
- 왜 중요: grep 대신 LSP의 types/references/call graphs를 MCP로 노출하는 방향. TypeScript/React UI나 Python refactor에서 agent hallucination을 줄일 수 있다.
- 바로 할 일: 지금 설치는 보류. mv-brain UI/TS 쪽에서 “참조 찾기 → 변경 범위 제한” 데모 아이디어로만 저장.

### 5. Long Long Run — Codex 장시간 작업 스킬
- 링크: https://github.com/huahuadeliaoliao/long-long-run
- 출처/신뢰도: Reddit + GitHub 검증, medium-low
- GitHub 검증: stars 2, forks 0, pushed_at 2026-05-04T07:00:51Z, Apache-2.0, maintained yes but tiny
- 왜 중요: Codex가 중간에 “continue?”로 멈추는 문제를 skill/hook/stop guard로 다루려는 시도. 과도한 자율 루프는 위험하지만, 긴 테스트/빌드/리서치의 checkpoint 설계는 유용하다.
- 바로 할 일: broad autonomous loop는 만들지 말고, Hermes cron에는 `notify_on_complete`, 명확한 종료 조건, checkpoint 파일만 차용.

## 한국/Threads/X 신호
- DuckDuckGo public 검색으로 `site:x.com "Claude Code" "MCP" github.com`에서 Claude Code 리소스 모음류 X 게시물은 보였지만, 대부분 링크 모음/재포스트 성격이라 추천 근거로 쓰지 않았다.
- `site:threads.net "클로드 코드" github.com`, `site:velog.io "Claude Code" github.com`, `site:disquiet.io "AI 코딩" MCP github.com`, `site:okky.kr "Cursor" "MCP"`는 이번 저볼륨 검색에서 강한 공개 결과가 없었다.
- 한국/Threads에서 검증 가능한 GitHub URL을 새로 확보하지 못했으므로, 한국어 신호는 보류로 분류한다.

## 릴리스/트래킹 상태
- Claude Code: v2.1.126, published 2026-05-01 — https://github.com/anthropics/claude-code/releases/tag/v2.1.126
- OpenAI Codex: rust-v0.128.0, published 2026-04-30 — https://github.com/openai/codex/releases/tag/rust-v0.128.0
- OpenCode: v1.14.33, published 2026-05-02 — https://github.com/anomalyco/opencode/releases/tag/v1.14.33
- Cline: v3.82.0, published 2026-05-01 — https://github.com/cline/cline/releases/tag/v3.82.0
- Gemini CLI: v0.40.1, published 2026-04-30 — https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.1
- Playwright MCP: v0.0.73, published 2026-05-01 — https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.73

## 우리한테 적용
- mv-brain: agent에게 “전체 영상/코드 다 읽기”가 아니라 검색 후보, 근거 파일, call/reference 범위를 먼저 주는 repo/clip lens를 만들면 데모 품질이 좋아진다. 포지셔닝은 `local-first editor assistant with evidence-backed search`.
- Hermes/자동화: cron 출력에 source → action → verification → checkpoint를 고정하고, 에이전트 세션 로그를 읽기 전용 대시보드로 보는 아이디어(Pawscope류)는 보류 관찰.
- 콘텐츠/홍보: “AI 코딩에서 진짜 병목은 모델이 아니라 stale context와 grep 남발이었다”를 Threads/X 빌드로그 훅으로 사용 가능.

## 버릴 것/보류
- X 링크 모음류: GitHub 검증 없는 리소스 성격이라 추천 제외.
- Cursor/Claude 계정 우회·프록시류: raine/claude-code-proxy는 GitHub stars 71, pushed_at 2026-05-03으로 살아있지만 구독/약관 리스크가 있어 운영 적용 보류.
- smithy-ai/smithy-ai: HN Show HN, stars 7, AGPL-3.0, issue tracker orchestration은 흥미롭지만 Hermes에 바로 넣기엔 과도한 자율 루프.
- AxonFlow Claude plugin: 권한/감사 방향은 좋지만 stars 3의 초기 repo라 watch만.
- gstack 파생 repo 다수: 패턴(context-save/restore, guard, review, qa)은 유용하지만 설치/도입은 보류.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
