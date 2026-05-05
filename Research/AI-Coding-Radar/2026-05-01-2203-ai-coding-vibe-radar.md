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
  - https://github.com/anomalyco/opencode/releases/tag/v1.14.31
  - https://github.com/google-gemini/gemini-cli/releases/tag/v0.40.1
  - https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.72
  - https://github.com/Fr-e-d/GAAI-framework
  - https://github.com/benjamin7007/Pawscope
  - https://github.com/wirvii/mneme
  - https://github.com/yvgude/lean-ctx
  - https://github.com/garrytan/gstack
  - https://github.com/Yeachan-Heo/oh-my-claudecode
  - https://github.com/openai/codex-plugin-cc
  - https://github.com/openai/skills
  - https://github.com/multica-ai/multica
  - https://github.com/CoplayDev/unity-mcp
  - https://pu.dev/
  - https://news.ycombinator.com/item?id=47968112
  - https://openai.com/index/harness-engineering/
  - https://ibm.github.io/mcp-context-forge/1.0.0/
  - https://reddit.com/r/ClaudeAI/comments/1sztkml/tdd_and_rules_enforcement_using_hooks/
  - https://reddit.com/r/ClaudeAI/comments/1sy4z7p/built_an_opensource_persistent_memory_layer_for/
created: 2026-05-01
reviewed_at: 2026-05-01
---

# AI 코딩 빠른 레이더 — 2026-05-01 22:03 KST

## 핵심 3줄
1. 이번 신호는 “새 모델”보다 **하네스/스킬/가드레일** 쪽이 강하다. OpenAI Codex, Claude Code, OpenCode, Gemini CLI가 모두 활발하고, 공통 레이어는 Markdown/YAML/bash 기반 워크플로다.
2. 바로 써볼 만한 것은 `GAAI-framework`, `Pu.sh`, `gstack/oh-my-claudecode`, `Pawscope`, `mneme/supamem` 계열이다. 단, 일부는 별이 낮거나 초기 repo라 실험용으로만 본다.
3. 한국/검색 노출에서는 `oh-my-claudecode`, `openai/codex-plugin-cc`, `openai/skills`, `unity-mcp`, `multica`가 잡혔고 GitHub API로 존재/활동을 확인했다.

## 주목할 것

### 1. GAAI-framework — “에이전트 도구 위에 얹는 납품 시스템”
- 링크: https://github.com/Fr-e-d/GAAI-framework
- 출처/신뢰도: GitHub, medium
- GitHub 검증: 135 stars, pushed_at `2026-05-01T12:36:47Z`, maintained yes, license `NOASSERTION`
- 왜 중요: Claude Code/Codex/Gemini/Cursor 위에 `.gaai/` 폴더만 넣어 Discovery→Delivery→criteria pass 흐름을 강제한다. Hermes 크론/스킬도 “계획만 하고 끝나는” 문제를 줄일 수 있다.
- 바로 할 일: 설치하지 말고 README/구조만 읽어서 `mv-brain`에 맞는 `Definition of Done`, `safe check`, `no provider call` 템플릿을 차용.

### 2. Pu.sh — 400줄 shell 기반 coding-agent harness
- 링크: https://pu.dev/ / HN: https://news.ycombinator.com/item?id=47968112
- 출처/신뢰도: HN + public site, medium-high
- 검증: HN 84 points / 21 comments, created `2026-04-30T20:55:12Z`
- 왜 중요: 무거운 SDK보다 작은 shell harness가 다시 주목받는 흐름. Hermes의 “simple, inspectable workflows” 원칙과 맞다.
- 바로 할 일: 구조를 벤치마크해서 cron output, checkpoint, approval boundary를 어떻게 작게 표현하는지 확인.

### 3. gstack / oh-my-claudecode — 역할형 스킬팩과 팀형 오케스트레이션
- 링크: https://github.com/garrytan/gstack / https://github.com/Yeachan-Heo/oh-my-claudecode
- 출처/신뢰도: GitHub + Korean/Naver-visible search, high for repo existence, medium for claims
- GitHub 검증: `garrytan/gstack` 87,607 stars, pushed_at `2026-05-01T06:06:17Z`, MIT. `Yeachan-Heo/oh-my-claudecode` 32,151 stars, pushed_at `2026-04-30T16:58:34Z`, MIT.
- 왜 중요: “CEO/Designer/QA/Release Manager” 같은 역할 스킬이 빠르게 fork/변형되고 있다. `mv-brain`에도 `clip-triage`, `fx-review`, `fcpxml-qa`, `demo-writer` 식으로 역할을 쪼개기 좋다.
- 바로 할 일: 설치 금지. 스킬 이름/입출력 패턴만 추출해 Hermes skill 문서/프롬프트에 반영 후보로 둔다.

### 4. Pawscope — 로컬-only agent session 관찰 대시보드
- 링크: https://github.com/benjamin7007/Pawscope
- 출처/신뢰도: GitHub, medium-low
- GitHub 검증: 2 stars, pushed_at `2026-05-01T12:48:55Z`, MIT, maintained yes but very early
- 왜 중요: Codex/Claude/Copilot 세션을 read-only/local-only로 보는 대시보드 방향은 `mv-brain` 데모에도 잘 맞는다. “에이전트가 뭘 했는지 보여주는 UI”는 포트폴리오 가치가 큼.
- 바로 할 일: 지금 도입하지 말고 UI/정보구조 아이디어만 참고. `mv-brain` build log에 “local-first trace panel” 아이디어로 변환 가능.

### 5. agent memory/checkpoint/context 실험 — lean-ctx, mneme, supamem, SIGNAL
- 링크: https://github.com/yvgude/lean-ctx / https://github.com/wirvii/mneme / https://github.com/dzmitrys-dev/supamem / https://github.com/mattbaconz/signal
- 출처/신뢰도: GitHub + Reddit, medium-low
- GitHub 검증: `lean-ctx` 975 stars, pushed_at `2026-05-01T12:26:40Z`, Claude Code/Cursor/Copilot/Windsurf/Codex/Gemini 대상 context layer. `mneme` 0 stars, pushed_at `2026-05-01T13:01:26Z`, Apache-2.0. `supamem` 4 stars, pushed_at `2026-05-01T13:01:45Z`, MIT. `SIGNAL` 9 stars, pushed_at `2026-05-01T10:17:18Z`, MIT.
- 왜 중요: “메모리” 자체보다 session restore/checkpoint/terse output/context read-mode가 실무 효용이 있다. Hermes는 이미 Obsidian/RAG가 있으므로 새 DB보다 checkpoint/context template가 더 유용하다.
- 바로 할 일: Qdrant/새 메모리 서버 도입 보류. 대신 `context-save`, `handoff`, `ckpt`, `review`, `read-mode` 포맷만 추출.

## 한국/Threads/X 신호
- X 검색(`site:x.com "Claude Code" "MCP" github.com`)은 X-only 홍보성 결과가 많았고, 직접 추천할 GitHub URL은 추출되지 않았다. 신뢰도 low.
- Threads 검색은 공개 검색에서 의미 있는 GitHub URL을 찾지 못했다. 보류.
- 한국/Naver-visible 검색에서 확인한 GitHub 링크:
  - https://github.com/Yeachan-Heo/oh-my-claudecode — 32,151 stars, pushed_at `2026-04-30T16:58:34Z`, “Teams-first Multi-agent orchestration for Claude Code”.
  - https://github.com/openai/codex-plugin-cc — 16,952 stars, pushed_at `2026-04-18T20:42:15Z`, “Use Codex from Claude Code to review code or delegate tasks”.
  - https://github.com/openai/skills — 17,938 stars, pushed_at `2026-05-01T04:58:56Z`, Codex skills catalog.
  - https://github.com/CoplayDev/unity-mcp — 9,078 stars, pushed_at `2026-04-29T20:54:55Z`, Unity Editor MCP bridge.
  - https://github.com/multica-ai/multica — 23,348 stars, pushed_at `2026-05-01T07:49:17Z`, managed agents platform.

## 우리한테 적용
- mv-brain:
  - `GAAI`식 criteria/DoD를 `FCPXML export`, `clip triage`, `FX recommendation` 작업에 붙인다.
  - gstack 패턴으로 `media-qa`, `demo-writer`, `fx-reviewer`, `release-note` 역할을 문서화하면 포트폴리오 설명력이 오른다.
  - Pawscope류 trace UI는 “local-first editor assistant가 안전하게 일하는 모습”을 보여주는 데모 소재.
- Hermes/자동화:
  - 새 autonomous loop보다 `checkpoint`, `guard`, `review`, `dry-run`, `vault-record` 템플릿 강화가 우선.
  - agent memory repo는 아직 낮은 신호. 기존 Obsidian + exact lookup + manual RAG 방침 유지.
- 콘텐츠/홍보:
  - Hook: “요즘 AI 코딩 도구의 핵심은 모델이 아니라 하네스다.”
  - X/Threads 초안: “Claude Code/Codex/Gemini가 다 빨라졌지만, 실제 생산성은 `.gaai/`, gstack, checkpoint 같은 ‘작업 규칙’에서 갈린다. 내 mv-brain에는 agent가 영상을 만드는 게 아니라, rough-cut/export를 검증 가능한 작은 단계로 돕게 만들고 있다.”

## 버릴 것/보류
- X/Threads-only “MCP/Claude Code 대박 repo”류: GitHub 링크/활동 검증 없으면 추천하지 않음.
- star 0~2 신규 memory/checkpoint repo 전체 도입: 아이디어만 참고.
- Cursor rogue DB 삭제류 기사/Reddit: 교훈은 “destructive action guard”지만, 구체 워크플로 증거가 약해 보류.
- Unity/Godot MCP: 영상/게임 demo에는 재미있지만 `mv-brain` 핵심 경로와는 거리가 있어 당장 도입 보류.

## 관련 문서
- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
