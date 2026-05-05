---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-04-29-auto-cut-sidecar-memory-bridge.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/mv_brain/integrations/auto_editor.py
  - /home/dev/mv-brain/mv_brain/core/models.py
  - /home/dev/mv-brain/mv_brain/demo.py
created: 2026-04-29
reviewed_at: 2026-04-29
---
# MV BRAIN Auto-cut sidecar 메모리 브리지 적용 작업

## 적용 판정
`apply`

오늘 승인된 아이디어 `Auto-cut 후보 사이드카 → 검색 가능한 클립 메모리 브리지`는 MV BRAIN의 현재 방향과 맞다. 자동 편집 결과를 최종본으로 과장하지 않고, `auto-editor`가 만든 후보 구간을 편집자가 검색/선별할 수 있는 클립 메모리로 바꾸는 작은 연결 작업이다.

## 확인한 현재 상태
- repo: `/home/dev/mv-brain`
- git 상태: `main...origin/main`, 다수의 미커밋 변경 존재
- 최근 커밋: `274f98c [verified] Prepare mv-brain for open source release`
- 안전 점검: `python3 -m compileall -q mv_brain` 통과
- README에는 이미 `mv-brain demo`와 no-media first-run flow가 설명되어 있다.
- `mv_brain/integrations/auto_editor.py`에는 `AutoEditorCandidateRegion`, `parse_candidate_regions()`, `write_candidate_sidecar()`가 있어 FCPXML asset clip을 `candidate_regions` JSON으로 바꾸는 기반이 있다.
- `mv_brain/core/models.py`에는 `SummaryCard`, `Clip`, `TimelineItem` 모델이 있어 sidecar 후보를 검색/러프컷 입력 형태로 매핑할 수 있다.
- `mv_brain/demo.py`는 이미 no-media demo clip metadata와 sample FCPXML rough cut을 생성한다. 따라서 이번 적용은 새 대형 파이프라인이 아니라 기존 demo flow에 auto-cut sidecar seam을 붙이는 작업이어야 한다.

## 내부 적용 토론
### builder
1~3일 작업으로 충분하다. 먼저 실제 `auto-editor` 실행 없이 fixture sidecar와 변환 스펙을 고정하고, 그 다음 순수 함수 테스트로 `candidate_regions`가 `SummaryCard` 또는 demo clip dict로 바뀌는지 검증한다. 기존 `demo.py`와 충돌하지 않게 `examples/auto-cut-sidecar/` 또는 별도 helper부터 시작한다.

### market
이 작업은 포트폴리오에서 “자동 컷 후보 → 사람이 검토 가능한 클립 메모리 → 검색/러프컷”이라는 제품 스토리를 보여준다. 유료화 각도는 넓은 SaaS보다 local-first 편집 워크플로 템플릿, FCPXML/auto-editor 연동 컨설팅, 데모 가능한 설치 지원으로 좁히는 편이 좋다.

### critic
이미 no-media demo가 repo에 들어간 상태라, 또 다른 demo fixture를 무리하게 늘리면 산만해질 수 있다. 따라서 이번 작업은 “새 제품 기능”이 아니라 `auto_editor.py`의 sidecar를 demo/search 메타데이터로 연결하는 최소 seam으로 제한해야 한다. 실제 컷 품질, Final Cut Pro import 성공, Gemini/Qdrant 검색 품질은 이 작업의 완료 기준에 넣지 않는다.

### curator
`apply`로 전환한다. 현재 repo에는 미커밋 변경이 많으므로, 이 크론은 작업 노트만 만들고 코드는 수정하지 않는다. 다음 개발 세션에서 기존 변경과 충돌하지 않는 작은 순수 변환/fixture 단위로 처리한다.

## 1-3 step 적용안
1. **sidecar fixture와 매핑 스펙 고정**
   - 후보 경로: `examples/auto-cut-sidecar/sample.candidates.json`.
   - 필수 필드: `source_tool`, `source_path`, `fcpxml_path`, `mode`, `candidate_count`, `candidate_regions[]`.
   - 각 region은 `source_path`, `start_time`, `end_time`, `duration`, `timeline_offset`, `candidate_reason`을 포함한다.

2. **`candidate_regions` → 검색 가능한 clip card 변환 정의**
   - 변환 대상: `SummaryCard` 또는 `demo.py`의 clip dict와 호환되는 최소 dict.
   - 기본값: `grade=B`, `score=0.5~0.7`, `labels=["auto-cut", mode, candidate_reason]`, `description="Auto-editor candidate region ..."`.
   - 목표: 실제 영상/Gemini/Qdrant 없이도 후보 5개가 clip browser/search fixture로 보이게 한다.

3. **README/demo 문구를 “candidate, not final cut”로 제한**
   - 포함: fixture 입력, 변환 후 clip card 예시 1개, roughcut 입력으로 이어지는 설명.
   - 제외: 실제 컷 품질 보장, provider API 호출, Qdrant 쓰기, Final Cut Pro 실제 import 보장.

## 완료 기준
- `python3 -m compileall -q mv_brain` 통과 유지
- 실제 미디어 ingest, Gemini/provider 호출, Qdrant mutation 없이 fixture/순수 변환 검증 가능
- README 또는 examples 문서가 “auto-cut 후보는 최종 편집본이 아니라 편집자 선별 후보”라고 명확히 표기
- 기존 `mv-brain demo` 흐름과 중복되지 않고, auto-editor sidecar가 검색/러프컷 입력으로 이어지는 seam만 보여줌

## 관련 문서
- [[Research/Ideas/2026-04-29-auto-cut-sidecar-memory-bridge|Auto-cut 후보 사이드카 → 검색 가능한 클립 메모리 브리지]]
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML 계약 테스트 + 데모 팩]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
