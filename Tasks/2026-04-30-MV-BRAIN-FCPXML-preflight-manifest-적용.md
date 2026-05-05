---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-04-30-fcpxml-relink-preflight-manifest.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/mv_brain/output/fcpxml_generator.py
  - /home/dev/mv-brain/mv_brain/demo.py
  - /home/dev/mv-brain/mv_brain/tests/test_demo_flow.py
created: 2026-04-30
reviewed_at: 2026-04-30
---
# MV BRAIN FCPXML preflight manifest 적용 작업

## 적용 판정
`apply`

오늘 승인된 아이디어 `FCPXML relink preflight manifest`는 MV BRAIN의 FCPXML-first handoff를 더 믿을 수 있는 포트폴리오 증거로 바꿀 수 있다. 단, 실제 Final Cut Pro import 성공을 보장하는 기능이 아니라, import 전에 media reference/relink 위험을 드러내는 sidecar report로만 적용한다.

## 확인한 현재 상태
- repo: `/home/dev/mv-brain`
- git 상태: `main...origin/main`, 현재 status 출력에는 추가 변경 목록 없음
- 최근 커밋: `3604db7 chore: refresh UI lockfile audit`, `393aee3 chore: initial public-ready release`
- 안전 점검: `python3 -m compileall -q mv_brain` 통과
- README는 `mv-brain demo`가 `data/demo/clips.json`과 `output/demo_roughcut.fcpxml`을 만든다고 설명한다.
- `mv_brain/demo.py`는 no-media clip fixture 8개로 `TimelineItem`을 만들고 `generate_fcpxml()`을 호출한다.
- `mv_brain/output/fcpxml_generator.py`는 `TimelineItem.source_path`, asset id, asset name, start/duration, timeline offset을 이미 계산하므로 preflight JSON/Markdown을 만들 입력 데이터가 있다.
- 기존 `test_demo_flow.py`는 demo fallback API를 검증하지만, FCPXML 옆 sidecar manifest 존재/내용은 아직 검증하지 않는다.

## 내부 적용 토론
### builder
1~3일 작업으로 충분하다. 새 provider/API/Qdrant 작업이 아니라 `fcpxml_generator.py` 주변에 순수 함수 `build_fcpxml_preflight_manifest(...)`와 Markdown renderer를 붙이고, `create_demo_project()`가 demo FCPXML 옆에 `.preflight.json`/`.preflight.md`를 쓰게 만들면 된다. 테스트는 tmp_path 기반 fixture로 가능하다.

### market
이 적용은 포트폴리오에서 “AI 검색/러프컷이 실제 편집툴로 넘어갈 때 어떤 media를 참조하는지 확인한다”는 실무 감각을 보여준다. 수익화는 좁게 FCPXML handoff 체크리스트, local-first roughcut setup, 편집 자동화 컨설팅/템플릿으로 연결한다.

### critic
주의점은 기존 4/27 FCPXML 계약 테스트 아이디어와 중복될 수 있다는 점이다. 따라서 XML well-formed validation이나 FCP import 보장으로 확장하지 않는다. 완료 기준도 “FCP에서 열린다”가 아니라 “demo roughcut의 참조 media, timeline item, missing/relink warning이 사람이 읽히는 report로 나온다”로 제한한다.

### curator
`apply`로 전환한다. 코드 변경은 이 크론에서 하지 않고, 다음 개발 세션에서 demo/FCPXML export의 작은 sidecar 산출물로 구현한다. 현재 repo smoke check가 통과했으므로 작업 시작 조건은 충분하다.

## 1-3 step 적용안
1. **preflight manifest 스키마 고정**
   - 출력 후보: `output/demo_roughcut.preflight.json`, `output/demo_roughcut.preflight.md`.
   - 필수 필드: `fcpxml_path`, `generated_at`, `project_name`, `timeline_duration`, `fps`, `assets[]`, `timeline_items[]`, `warnings[]`.
   - `assets[]`: `asset_id`, `source_path`, `asset_name`, `file_url`, `media_kind`, `exists_on_disk`, `relink_risk`.

2. **demo flow에 sidecar 생성 연결**
   - `create_demo_project()`가 `generate_fcpxml()` 호출 직후 같은 timeline data로 preflight sidecar를 생성하게 한다.
   - `demo://` 경로는 실제 파일 없음 오류가 아니라 `demo_fixture` 또는 `non_file_scheme` warning으로 분류한다.
   - 실제 영상 ingest, Gemini/provider 호출, Qdrant mutation, Final Cut Pro 실행은 제외한다.

3. **fixture test + README 문구 제한**
   - tmp_path demo 실행 후 `.preflight.md`에 source media table, timeline clip table, relink checklist 3개가 있는지 확인한다.
   - README 문구는 “import validator”가 아니라 “preflight/relink risk report”로 제한한다.

## 완료 기준
- `python3 -m compileall -q mv_brain` 통과 유지
- `mv-brain demo` 또는 `create_demo_project(tmp_path...)`가 FCPXML과 preflight JSON/Markdown sidecar를 함께 생성
- sidecar에 demo clip source, timeline offset, duration, relink warning/checklist가 포함됨
- 실제 media ingest, provider API, Qdrant mutation, Final Cut Pro 실행 없이 테스트 가능
- README/문서가 “FCP import 성공 보장”을 주장하지 않음

## 관련 문서
- [[Research/Ideas/2026-04-30-fcpxml-relink-preflight-manifest|FCPXML relink preflight manifest]]
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML 계약 테스트 + 데모 팩]]
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Research/Ideas/2026-04-29-auto-cut-sidecar-memory-bridge|Auto-cut 후보 사이드카 → 검색 가능한 클립 메모리 브리지]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
