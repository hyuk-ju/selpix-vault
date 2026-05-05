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
  - https://github.com/Gentleman-Programming/engram
  - https://github.com/boshu2/agentops
  - https://github.com/OWASP/Agent-Security-Regression-Harness
  - https://github.com/CelestoAI/SmolVM/
  - https://www.mendral.com/blog/agent-harness-belongs-outside-sandbox
  - https://www.reddit.com/r/LocalLLaMA/comments/1t2j8uo/upskill_skill_registry_your_agent_consults_before/
  - https://www.reddit.com/r/cursor/comments/1t2gjtl/cursor_silently_switched_models_while_i_was_deep/
  - https://www.reddit.com/r/ClaudeAI/comments/1t25br1/be_super_careful_we_might_destroy_your_computer/
  - https://hn.algolia.com/
created: 2026-05-03
reviewed_at: 2026-05-03
---

# AI 코딩 빠른 레이더 — 2026-05-03 22:02 KST

## 핵심 3줄
1. 이번 레이더의 강한 신호는 “더 똑똑한 모델”보다 **메모리/검증/샌드박스/권한 게이트**다. 긴 세션과 자동 실행이 늘면서 실패 복구와 보안 회귀 테스트가 중요해졌다.
2. Claude Code, Codex, OpenCode, Cline, Gemini CLI, Playwright MCP는 모두 최근 릴리스/푸시가 있었고, 실사용 레이어는 MCP·브라우저·세션 메모리 쪽으로 이동 중이다.
3. 한국/Threads/X 공개 검색은 이번 실행에서 추천할 만한 GitHub 검증 링크를 새로 확보하지 못했다. 소셜 단독 주장은 보류한다.

## 주목할 것

### 1. Engram — 코딩 에이전트용 로컬 지속 메모리
- 링크: https://github.com/Gentleman-Programming/engram
- 출처/신뢰도: GitHub, high
- GitHub 검증: stars 3,123 / forks 344 / pushed_at 2026-05-03T12:25:15Z / MIT / maintained yes
- 왜 중요: SQLite+FTS5, MCP, HTTP API, CLI/TUI 조합은 Hermes cron 결과, mv-brain 의사결정, 에이전트 세션 복구에 바로 적용 가능한 패턴이다.
- 바로 할 일: 설치는 보류. 먼저 `Hermes 작업 메모리 최소 스키마`를 문서화하고, Chroma/RAG와 중복되지 않는 “작업 로그 FTS” 용도로만 검토.

### 2. OWASP Agent Security Regression Harness — 에이전트/MCP 보안 회귀 테스트
- 링크: https://github.com/OWASP/Agent-Security-Regression-Harness
- 출처/신뢰도: GitHub/HN, high
- GitHub 검증: stars 11 / forks 5 / pushed_at 2026-05-03T10:52:38Z / Apache-2.0 / maintained yes
- 왜 중요: Reddit에서도 MCP가 로컬 파일/앱을 건드리는 위험 경고가 반복된다. mv-brain이나 Hermes에 MCP/browser/computer-use를 붙일수록 “실행 전 보안 테스트”가 포트폴리오 신뢰 포인트가 된다.
- 바로 할 일: mv-brain README/Runbook에 `provider call 금지`, `real media ingestion 금지`, `Qdrant mutation 금지` 같은 안전 케이스를 테스트 체크리스트로 분리.

### 3. Agent harness outside sandbox + SmolVM — 에이전트 실행 경계 재설계
- 링크: https://www.mendral.com/blog/agent-harness-belongs-outside-sandbox, https://github.com/CelestoAI/SmolVM/
- 출처/신뢰도: HN/GitHub, medium-high
- GitHub 검증: CelestoAI/SmolVM stars 493 / pushed_at 2026-05-02T21:34:38Z / Apache-2.0 / maintained yes
- 왜 중요: HN에서 119점/86댓글로 논쟁이 붙었다. 핵심은 “에이전트 하네스가 샌드박스 안에 있으면 관찰·중단·정책 집행이 약해진다”는 설계 주장. SmolVM은 코드 실행/브라우저/AI 에이전트용 샌드박스 인프라로 연결된다.
- 바로 할 일: Hermes에서는 광범위 자율 루프를 만들지 말고, 각 작업에 `read-only`, `dry-run`, `write`, `external-call` 게이트를 명시. mv-brain은 ingest/analyze/export 경계를 UI와 CLI에서 분리.

### 4. Playwright MCP + insane-search 패턴 — 브라우저 리서치 도구화
- 링크: https://github.com/microsoft/playwright-mcp, https://github.com/fivetaku/insane-search
- 출처/신뢰도: GitHub/official, high for Playwright MCP, medium for insane-search
- GitHub 검증: Playwright MCP stars 31,931 / pushed_at 2026-05-01T22:35:25Z / Apache-2.0. insane-search stars 601 / pushed 2026-04-22T08:44:25Z.
- 왜 중요: Coupang/Naver/X/Threads처럼 일반 fetch가 막히는 공개 리서치에서 “Phase 0 API → Jina/mobile → browser render” 절차가 실전적이다. 단, 로그인·쿠키·CAPTCHA 우회는 금지.
- 바로 할 일: 현재 레이더처럼 소셜 검색은 web search/Jina 실패 시 `검색 실패/검증 불가`로 명시하고, 추천 후보는 반드시 GitHub API로 확인.

### 5. Codex/Claude/OpenCode/Gemini CLI 릴리스 추세 — 터미널 에이전트 경쟁은 계속 빠름
- 링크: https://github.com/anthropics/claude-code, https://github.com/openai/codex, https://github.com/anomalyco/opencode, https://github.com/google-gemini/gemini-cli
- 출처/신뢰도: GitHub/official, high
- GitHub 검증: Claude Code stars 120,036 / pushed 2026-05-01, Codex stars 79,694 / pushed 2026-05-03, OpenCode stars 153,827 / pushed 2026-05-03, Gemini CLI stars 103,011 / pushed 2026-05-02.
- 왜 중요: 도구 자체는 계속 빠르게 변하지만, 우리에게 중요한 차별점은 특정 CLI 사용법보다 `검증 가능한 산출물`, `세션 복구`, `권한 제한`, `build log`다.
- 바로 할 일: mv-brain 포트폴리오 문구를 “AI가 MV를 자동 완성”이 아니라 “로컬-first 편집 보조 + 검색/FX/FCPXML export + 안전한 에이전트 워크플로”로 고정.

## 한국/Threads/X 신호
- Google 공개 검색을 저용량으로 시도한 쿼리: `site:x.com "Claude Code" "MCP"`, `site:threads.net "클로드 코드" "github.com"`, `site:velog.io "Claude Code" "github.com"`, `"커서" "MCP" "github.com"`.
- 결과: 검색 HTML은 반환됐지만 추천 가능한 X/Threads/한국어 공개 글 또는 검증 가능한 GitHub 링크를 파싱하지 못했다. 이번 회차에서는 한국/Threads 신호를 **보류**한다.
- 신뢰도: low. 로그인/쿠키/CAPTCHA 우회 없음.

## Reddit/HN 보조 신호
- Reddit r/LocalLLaMA: “Upskill: skill registry your agent consults before it starts” — 점수 9, 댓글 22. 스킬 레지스트리/플레이북 재사용 수요는 있지만 GitHub 링크 검증이 없어 추천은 보류.
- Reddit r/cursor: 모델이 조용히 바뀌며 코드리뷰 손실을 겪었다는 사례 — 점수 낮음이지만 “모델/세션/체크포인트 표시” UX 필요성을 보여줌.
- Reddit r/ClaudeAI: MCP가 로컬 환경을 위험하게 건드릴 수 있다는 경고 사례 — 점수 낮음이지만 OWASP harness와 결합하면 실무 체크리스트 소재.
- HN: “The agent harness belongs outside the sandbox”는 높은 토론량으로 설계 관점에서 추적 가치 있음.

## 우리한테 적용
- mv-brain: `analysis provider`, `media ingestion`, `Qdrant write`, `FCPXML export`를 각각 권한 단계로 나누고, README에 안전 실행 예시/금지 예시를 넣으면 포트폴리오 신뢰도가 올라간다.
- Hermes/자동화: cron 리포트는 Engram류 메모리 도입보다 먼저 `작업 단위 체크포인트`, `sources`, `verified_by`, `dry-run 여부`를 표준화하는 게 더 싸고 안전하다.
- 콘텐츠/홍보: “바이브코딩으로 다 만든다” 대신 `AI 코딩 에이전트가 망가지는 지점: 메모리, 권한, 샌드박스, 증거`라는 빌드로그/Threads 소재가 좋다.

## 버릴 것/보류
- X/Threads/한국어 소셜 단독 주장: 이번 회차에서 GitHub 검증 링크 없음.
- “무료 모델 프록시로 Claude Code 사용”류: 법적/약관/보안 리스크 확인 전 보류.
- 0~1 star의 gstack 파생/스킬 마켓플레이스: 아이디어 관찰은 가능하지만 즉시 도입 가치 낮음.
- “Agent OS가 밤새 도구를 만들었다”류 자율 루프: 현재 Hermes 원칙과 충돌. 승인 없는 장시간 자동 실행 금지.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
