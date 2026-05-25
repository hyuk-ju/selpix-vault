---
type: project-review
note_status: draft
confidence_level: high
source_agent: codex-ops
generated_via: hermes-telegram
verified_by: codex-ops
sources:
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/pyproject.toml
  - /home/dev/mv-brain/mv_brain/cli.py
  - /home/dev/mv-brain/mv_brain/api/agent/orchestrator.py
  - /home/dev/mv-brain/mv_brain/api/agent/tools.py
  - /home/dev/mv-brain/mv_brain/core/qdrant_manager.py
created: 2026-04-28
reviewed_at: 2026-04-28
---

# mv-brain Gstack-style 평가 — 2026-04-28

## 요약

Gstack에서 흡수할 만한 관점(`review`, `investigate`, `qa`, `guard`, `context-save/restore`)으로 `/home/dev/mv-brain`을 점검했다.

- Python syntax baseline: `python3 -m compileall -q mv_brain` 통과.
- Python pytest collection: 실패. 원인은 `qdrant_client`, `cv2` 미설치와 `mv_brain.output` 패키지 누락.
- UI baseline: `npm ci && npm run build` 통과. 단, Vite chunk-size warning 있음.
- 최종 git 상태: `main...origin/main`, 기존 untracked `AGENTS.md`만 확인.

## 점수

| 항목 | 점수 | 메모 |
|---|---:|---|
| 포지셔닝 명확도 | 8.5/10 | local-first MV 편집 브레인 방향이 좋음 |
| 포트폴리오 임팩트 | 8/10 | CLI/FastAPI/React/Qdrant/Gemini/FCPXML 스토리가 강함 |
| 데모 가능성 | 6/10 | UI는 빌드되지만 output 모듈 누락으로 E2E 신뢰도 낮음 |
| 수익화 가능성 | 6.5/10 | editor-in-the-loop 유료화 가능성은 있으나 ICP/패키징이 아직 약함 |
| 기술 완성도 | 5.5/10 | 구조는 넓지만 핵심 import 경로가 깨짐 |

## 가장 강한 포지셔닝

> 로컬-first AI clip triage + FCPXML rough-cut assistant for music-video editors.

AI가 완성본을 자동 생성한다는 과장보다, 편집자가 실제 NLE에서 쓸 클립을 빠르게 찾고 러프컷으로 넘기는 보조 도구로 잡는 것이 가장 강하다.

## 핵심 리스크

1. `mv_brain.output` 패키지 누락
   - README/CLAUDE/test/CLI는 `mv_brain.output.fcpxml_generator`, `edl_generator`, `fcp_bridge`를 전제로 하지만 실제 디렉터리가 없다.
   - FCPXML-first라는 핵심 약속과 직접 충돌한다.

2. 테스트 수집 실패
   - `qdrant_client`, `cv2` 미설치.
   - 외부 의존 테스트를 marker/importorskip로 분리해야 한다.

3. 데모 데이터 부족
   - Gemini/Qdrant/실제 미디어 없이도 포트폴리오 데모를 보여줄 fixture/demo mode가 필요하다.

4. API 안전 경계
   - CORS 전체 허용, settings 저장, ingest/roughcut 같은 로컬 파일 접근 기능은 local-only/token/confirmation 경계가 필요하다.
   - output download에는 path traversal 방어가 있으나, roughcut `output_name` slug 검증이 필요하다.

## 추천 우선순위

### P0 — 데모 신뢰도 복구
- `mv_brain/output/` 복구 또는 최소 구현.
  - `fcpxml_generator.py`
  - `edl_generator.py`
  - `fcp_bridge.py`
  - `__init__.py`
- 우선 `test_cli.py`, `test_editing.py`, `test_fcp_bridge.py` 중 최소 핵심 경로를 통과시킨다.

### P1 — No API key demo
- `demo/fixtures/clips.json` 또는 pre-indexed fake clip payload 추가.
- `MV_BRAIN_DEMO_MODE=1 mv-brain serve`로 Qdrant/Gemini 없이 UI 데모 가능하게 만든다.

### P2 — README/포트폴리오 정리
- 3분 데모 시나리오 추가.
- architecture diagram/GIF/screenshot 추가.
- `ui/README.md`가 generic template라면 mv-brain용으로 교체.

### P3 — Guard 적용
- `output_name` slug 처리.
- settings/ingest/roughcut endpoint에 local-only/token/confirmation 추가.
- CORS를 localhost origin으로 축소.

## 흡수한 Gstack식 판단

- `review`: README가 주장하는 기능과 실제 import 가능한 모듈을 대조.
- `investigate`: pytest 실패를 단순 dependency 문제가 아니라 `mv_brain.output` 누락으로 분리.
- `qa`: UI build와 Python compile/test baseline을 따로 확인.
- `guard`: 로컬 파일 접근/출력 파일명/CORS 경계 확인.
- `context-save`: 다음 세션에서 바로 이어갈 수 있도록 이 평가를 Vault에 저장.

## 관련 문서

- [[Projects/AI-MV-Agent]]
- [[Projects/AI-MV-Agent/2026-04-28-mv-brain-gstack-style-review]]
- [[Research/AI-Coding-Radar/2026-04-29-0234-ai-coding-vibe-radar]]
