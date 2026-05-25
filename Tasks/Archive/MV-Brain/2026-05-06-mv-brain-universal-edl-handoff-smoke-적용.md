---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-05-06-universal-edl-handoff-smoke.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/mv_brain/tests/test_editing.py
created: 2026-05-06
reviewed_at: 2026-05-06
---
# MV BRAIN Universal EDL handoff smoke 적용

## 적용 판정

`apply` — 오늘 아이디어는 기존 EDL generator와 테스트가 이미 있어 코드 수정 없이도 작은 문서/샘플 산출물로 전환 가능하다.

## 대상 프로젝트

- `/home/dev/mv-brain`
- 브랜치 상태: `main...origin/main`, working tree clean
- 최근 커밋: `fe8852f docs: align Korean overview with public positioning`

## 안전 점검

- `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과
- `python3 -m pytest mv_brain/tests/test_editing.py -q` → `41 passed in 0.06s`
- 실제 미디어 ingest, provider 호출, Qdrant mutation, FCP/Premiere/Resolve import는 실행하지 않음

## 내부 토론

### builder

`mv_brain/tests/test_editing.py`의 `TestEDLGenerator._make_simple_timeline()`과 `test_build_and_export_premiere_edl()`을 근거로 fixture 기반 EDL 샘플 생성 절차를 문서화하면 된다. 첫 구현은 새 기능보다 `docs/samples/demo_roughcut.edl`, `docs/edl-handoff.md`, README 문구 2~3줄이 적절하다.

### market

FCPXML 중심 프로젝트가 Premiere/Resolve 사용자에게도 “러프컷 교환 파일을 이해한다”는 신호를 줄 수 있다. 포트폴리오에서는 local-first AI editor가 실제 NLE handoff를 어떻게 검증하는지 보여주는 작고 선명한 증거가 된다.

### critic

EDL은 오래된 컷 리스트 포맷이라 FX, multicam, speed ramp, compound clip 보존을 주장하면 위험하다. 문구는 반드시 “minimal EDL smoke / review artifact”로 제한하고, 실제 Premiere/Resolve import 호환은 수동 검증 전까지 주장하지 않는다.

### curator

적용 승인. 범위가 작고 현재 repo의 통과 테스트로 뒷받침되며, 최근 FCPXML/clip-pack 아이디어와 겹치지 않고 handoff 신뢰를 보강한다.

## 1-3 step 적용안

1. `mv_brain/tests/test_editing.py`의 `TestEDLGenerator._make_simple_timeline()` 구조를 참고해, 실제 미디어 없이 재현 가능한 golden EDL 샘플 생성 절차를 `docs/edl-handoff.md` 초안에 적는다.
2. `docs/samples/demo_roughcut.edl` 샘플을 만들 경우, `validate_edl_file()` 결과와 `python3 -m pytest mv_brain/tests/test_editing.py -q` 통과 로그를 문서에 함께 남긴다.
3. README에는 “FCPXML is primary; EDL is a minimal Premiere/Resolve review smoke artifact” 수준으로만 추가하고, 완전한 Premiere/Resolve export나 효과 보존은 주장하지 않는다.

## 완료 기준

- no-provider / no-media / no-Qdrant 조건에서 샘플 또는 생성 절차가 재현된다.
- `validate_edl_file()`와 targeted pytest가 통과한다.
- 문서가 EDL의 한계와 주장 범위를 명시한다.

## 관련 문서

- [[Research/Ideas/2026-05-06-universal-edl-handoff-smoke|Universal EDL handoff smoke]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-05-05-agent-readable-cutlist-clip-pack|Agent-readable cutlist clip pack]]
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML contract demo pack]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
