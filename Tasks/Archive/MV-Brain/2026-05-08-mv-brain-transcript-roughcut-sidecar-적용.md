---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-05-08-transcript-roughcut-sidecar.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
created: 2026-05-08
reviewed_at: 2026-05-08
---
# MV BRAIN Transcript Roughcut Sidecar 적용

## 적용 판정
- **결정**: apply
- **대상 프로젝트**: `/home/dev/mv-brain`
- **선택 아이디어**: [[2026-05-08-transcript-roughcut-sidecar|Transcript Roughcut Sidecar]]

## 확인한 상태
- Repo: `main...origin/main`, working tree clean.
- 최근 커밋: `fe8852f docs: align Korean overview with public positioning`.
- 검색 결과: `mv_brain` Python 경로에서 `transcript/srt/vtt/subtitle/caption` 직접 구현은 아직 확인되지 않음.
- 안전 체크: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과.
- 타깃 테스트: `python3 -m pytest mv_brain/tests/test_editing.py mv_brain/tests/test_demo_flow.py -q` → `44 passed in 0.37s`.

## 내부 토론
- `builder`: 기존 demo/FCPXML 흐름이 통과하므로, 첫 적용은 미디어 처리 없이 `demo_transcript.srt → cutlist JSON` 사양과 fixture부터 잡는 것이 안전하다.
- `market`: 대본/가사 기반 러프컷은 편집자가 즉시 이해하는 포트폴리오 데모이며, FCPXML/EDL 템플릿 또는 워크플로 컨설팅으로 설명하기 쉽다.
- `critic`: 실제 Whisper/ASR, 저작권 가사 샘플, 자동 최종 편집으로 확장하면 산만하고 위험하다. 이번 적용은 sidecar-only와 자체 샘플 문장으로 제한해야 한다.
- `curator`: 중복 작업이 아니고 테스트 기반이 살아 있으므로 apply. 단, 코드 구현은 별도 명시 요청 전에는 하지 않는다.

## 1-3단계 적용안
1. `docs/` 또는 README 섹션으로 no-media transcript sidecar 스펙을 먼저 작성한다: 입력 필드(`start`, `end`, `text`, `energy_hint`), 출력 cutlist JSON, 금지 범위(ASR/provider/저작권 가사 없음).
2. 자체 작성 `demo_transcript.srt` fixture와 예상 cutlist 예시를 설계해 기존 `mv-brain demo` 출력 흐름과 연결될 파일명을 정한다.
3. 이후 구현 시 `test_demo_flow.py` 또는 신규 단위 테스트에서 SRT fixture 파싱 → cutlist 생성까지만 먼저 검증하고, FCPXML 연결은 두 번째 단계로 분리한다.

## 완료 기준
- README나 docs에 sidecar-only 데모 명령/예상 출력이 추가된다.
- 자체 샘플 대본만 사용한다.
- `compileall`과 demo/editing 관련 no-media 테스트가 통과한다.
- 실제 미디어 ingest, provider 호출, Qdrant mutation은 발생하지 않는다.

## 관련 문서
- [[2026-05-08-transcript-roughcut-sidecar]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[2026-05-07-fx-recommendation-explainability-gallery]]
- [[2026-05-06-universal-edl-handoff-smoke]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
