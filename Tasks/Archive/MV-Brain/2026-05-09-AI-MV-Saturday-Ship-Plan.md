---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Runbooks/Hermes-Obsidian-운영-규칙.md
  - /home/dev/openclaw/config/workspace/vault/Memory/Daily/2026-05-09.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/2026-05-07-AI-MV-빌드로그-초안.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/docs/public/overview.ko.md
  - git:/home/dev/mv-brain status --short --branch
  - git:/home/dev/mv-brain log --oneline -5
created: 2026-05-09
reviewed_at: 2026-05-09
---
# 2026-05-09 AI MV Saturday Ship Plan

> 목표: 코드나 repo 변경 없이, 오늘 사람이 바로 만들 수 있는 작은 공개용 artifact 하나를 정한다.

## 선택한 ship target

**README 상단/포트폴리오에 붙일 60~90초 no-media demo 캡처 패키지 초안**을 만든다.

산출물 형태는 짧은 화면 녹화 또는 GIF 후보이며, 핵심 장면은 다음 3개로 제한한다.

1. `mv-brain demo`로 데모 파일 생성 흐름.
2. 로컬 UI에서 `Demo Project`, `8 demo clips`가 보이는 클립 브라우저.
3. output 패널의 `demo_roughcut.fcpxml`과 “FCPXML로 Final Cut Pro handoff” 메시지.

## 선택 근거

- 최근 daily note는 README/overview의 가치 제안보다 **데모 흐름을 실제로 보여주는 asset**이 다음 병목이라고 반복해서 지적했다.
- repo는 `main...origin/main`, working tree clean 상태이며 최근 커밋은 `fe8852f docs: align Korean overview with public positioning`이다.
- README와 `docs/public/overview.ko.md`는 이미 no-media demo, local-first, editor-in-the-loop, FCPXML handoff 포지션을 설명한다. 오늘 ship은 새 기능보다 이 내용을 시각 증거로 압축하는 것이 가장 작고 효과적이다.

## 3-step 실행 체크리스트

1. **녹화 범위 고정**: 터미널 한 화면 + 로컬 UI 두 화면만 사용한다. 실제 미디어 ingest, Gemini/provider 호출, Qdrant mutation은 하지 않는다.
2. **3장면 캡처/녹화**: `mv-brain demo` 결과, `Demo Project / 8 demo clips`, `demo_roughcut.fcpxml` output 패널을 순서대로 보여준다.
3. **짧은 설명 붙이기**: 마지막 자막/README 문구로 “AI가 완성본을 자동 생성하는 것이 아니라, 편집자가 후보를 찾고 FCPXML로 넘기는 로컬 어시스턴트”를 넣는다.

## Definition of Done

- 60~90초 이하의 화면 녹화 또는 GIF 후보 1개가 로컬에 저장된다.
- 캡처 안에 `Demo Project`, `8 demo clips`, `demo_roughcut.fcpxml` 중 최소 2개가 보인다.
- README 상단에 붙일 1문장 caption이 준비된다.
- 개인 촬영본, API key, provider 로그, Qdrant 데이터, 비밀 경로가 화면에 노출되지 않는다.

## README caption 후보

> Try the no-media demo: MV BRAIN creates sample clip metadata and a FCPXML rough cut locally, so reviewers can see the clip browser → output handoff flow without footage, Qdrant, or provider keys.

## 관련 문서

- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Projects/AI-MV-Agent/2026-05-07-AI-MV-빌드로그-초안|2026-05-07 AI MV 빌드로그 초안]]
- [[Memory/Daily/2026-05-09|2026-05-09 Daily Note]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
