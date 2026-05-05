---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Runbooks/Hermes-Obsidian-운영-규칙.md
  - /home/dev/openclaw/config/workspace/vault/Memory/Daily/2026-05-02.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/2026-04-30-AI-MV-빌드로그-초안.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - git:/home/dev/mv-brain status --short --branch
  - git:/home/dev/mv-brain log --oneline -5
created: 2026-05-02
reviewed_at: 2026-05-02
---
# 2026-05-02 MV BRAIN Saturday Ship Plan — No-media first-run demo README block

## 오늘 ship 목표
`docs/public/overview.ko.md`, `docs/public/overview.en.md`, 또는 README 하단에 바로 붙일 수 있는 **“no-media first-run demo” 문서 블록 초안**을 완성한다.

이번 ship은 코드 구현이 아니라 포트폴리오 공개용 문서 산출물이다. 실제 영상, Qdrant 서버, Gemini/provider API key 없이도 `mv-brain demo → mv-brain serve → clip browser/output flow → demo_roughcut.fcpxml` 가치가 60초 안에 이해되도록 만드는 것이 목표다.

## 근거 스냅샷
- repo: `/home/dev/mv-brain`
- git 상태: `main...origin/main`, working tree clean
- 최근 커밋:
  - `3604db7 chore: refresh UI lockfile audit`
  - `393aee3 chore: initial public-ready release`
- README 현재 메시지: MV BRAIN은 자동 MV 완성기가 아니라 local-first editor assistant이며, 클립 탐색/FX 추천/FCPXML handoff가 핵심이다.
- 최근 vault 흐름: 2026-05-01~02 Daily note 모두 no-media demo를 README/포트폴리오 설명으로 바꾸는 작업을 다음 행동으로 잡고 있다.
- 안전 범위: 이 크론에서는 repo 코드 수정, 실제 media ingest, Gemini/provider 호출, Qdrant mutation, Final Cut Pro 실행을 하지 않는다.

## 3-step execution checklist
1. **문서 블록 5문장 고정**
   - 순서: `mv-brain demo` → `mv-brain serve` → sample clip browser 확인 → `output/demo_roughcut.fcpxml` 확인 → Final Cut Pro handoff 가치.
   - 톤: “AI가 알아서 완성”이 아니라 “편집자가 쓸 컷을 빠르게 찾고 타임라인 파일로 넘기는 assistant”.

2. **README/overview 삽입 위치 결정**
   - 1순위: `docs/public/overview.ko.md` / `docs/public/overview.en.md`의 데모 섹션.
   - 2순위: README `For a first-run demo...` 단락 바로 아래에 짧은 “What you can verify in 60 seconds” 블록.
   - 코드 변경 없이 문안만 준비하고, 실제 적용은 별도 개발 세션에서 diff로 처리한다.

3. **포트폴리오 증거 3개 체크리스트 작성**
   - 터미널 명령 2개: `mv-brain demo`, `mv-brain serve`.
   - UI 캡처 후보 1개: clip browser 또는 output page.
   - 파일 증거 1개: `output/demo_roughcut.fcpxml` 존재와 “Final Cut Pro로 넘길 산출물” 문구.

## Definition of Done
- 한국어/영어 각각 5~7문장짜리 no-media demo 문안이 준비됨.
- 문안이 provider/API/Qdrant/실제 영상 없이 재현 가능한 흐름만 설명함.
- “Final Cut Pro import 성공 보장” 같은 과장 표현이 없음.
- 다음 개발 세션에서 README 또는 `docs/public/overview.*.md`에 그대로 붙일 수 있을 만큼 명령어·산출물·가치 설명이 명확함.
- 이번 Saturday ship 산출물은 **문서 초안 1개**로 제한한다.

## 바로 사용할 문안 초안

### 한국어
실제 영상 파일이나 API 키가 없어도 MV BRAIN의 첫 흐름은 확인할 수 있습니다. `mv-brain demo`는 샘플 클립 메타데이터와 `output/demo_roughcut.fcpxml`을 생성하고, `mv-brain serve`로 로컬 UI를 열면 clip browser와 output 흐름을 볼 수 있습니다. 이 데모의 목적은 “AI가 최종 뮤직비디오를 자동 완성한다”가 아니라, 편집자가 자연어로 쓸만한 컷을 찾고 러프컷 산출물을 Final Cut Pro로 넘기는 과정을 보여주는 것입니다. 샘플 데이터만으로도 클립 탐색, 후보 선택, FCPXML handoff라는 제품의 핵심 가치를 60초 안에 확인할 수 있습니다. 실제 프로젝트에서는 사용자가 선택한 provider key와 로컬 영상/메타데이터를 연결해 같은 흐름을 확장합니다.

### English
You can verify MV BRAIN’s first-run flow without real footage or an API key. `mv-brain demo` creates sample clip metadata and `output/demo_roughcut.fcpxml`, then `mv-brain serve` opens the local UI so you can inspect the clip browser and output flow. The demo is not claiming that AI finishes a complete music video by itself. It shows the practical assistant loop: find usable shots with natural language, select candidates, and hand off a rough-cut FCPXML file to Final Cut Pro. With only sample data, a reviewer can understand the core value—clip triage to timeline handoff—in about 60 seconds. Real projects can extend the same flow with the editor’s own footage, local project data, and optional BYO provider keys.

## 관련 문서
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Projects/AI-MV-Agent/2026-04-30-AI-MV-빌드로그-초안|2026-04-30 AI MV 빌드로그 초안]]
- [[Memory/Daily/2026-05-02|2026-05-02 Daily Note]]
- [[Tasks/2026-04-30-MV-BRAIN-FCPXML-preflight-manifest-적용|MV BRAIN FCPXML preflight manifest 적용 작업]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
