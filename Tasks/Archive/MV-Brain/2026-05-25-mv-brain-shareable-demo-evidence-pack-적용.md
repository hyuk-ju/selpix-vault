---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-05-10-shareable-demo-evidence-pack.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프.md
created: 2026-05-25
reviewed_at: 2026-05-25
---
# MV BRAIN Shareable Demo Evidence Pack 적용 검토

## 적용 판정

- 판정: **apply**
- 대상 프로젝트: `/home/dev/mv-brain`
- 기준 아이디어: [[2026-05-10-shareable-demo-evidence-pack]]
- 참고: 오늘(2026-05-25) 승인 아이디어가 없어서 최근 승인 카드 중 가장 최신인 `Shareable Demo Evidence Pack`을 검토했다.

## 현재 상태 근거

- Repo 상태: `git -C /home/dev/mv-brain status --short --branch` → `## main...origin/main`, 작업트리 변경 없음.
- 최근 커밋: `fe8852f docs: align Korean overview with public positioning`, `3604db7 chore: refresh UI lockfile audit`, `393aee3 chore: initial public-ready release`.
- 안전 정적 체크: `python3 -m compileall -q mv_brain` → 통과(exit 0).
- 테스트 수집 체크: `python3 -m pytest --collect-only -q mv_brain/tests` → 실패(exit 2). 원인은 현재 환경에 `cv2`, `qdrant_client`가 없어 일부 테스트 모듈 import가 막힘. 아이디어 자체는 no-provider/no-media 정적 evidence pack이므로 구현 전 필수 해결 조건은 아니지만 README/검증 로그에는 이 제약을 명시해야 한다.
- 중복 확인: `Tasks/`에 동일한 `shareable demo evidence pack` 적용 노트는 없음. 다만 2026-04-28 no-media 검색 데모팩, 2026-05-02 no-media demo README, 2026-05-05 cutlist clip-pack과 연결되는 후속 증거 번들이다.

## 역할별 적용 토론

- `builder`: 새 AI 기능보다 `mv-brain demo` 산출물, README, FCPXML 검증 결과를 묶는 작은 문서/스크립트가 적합하다. 자동 스크린샷은 제외하고 정적 manifest와 README부터 시작하면 1-3일 범위다.
- `market`: 포트폴리오 방문자와 초기 유료 상담 후보는 “설치하면 무엇이 나오나”를 먼저 본다. evidence pack은 GitHub README, DM, 블로그에 바로 붙일 수 있는 증거가 된다.
- `critic`: 기존 no-media demo 카드와 겹칠 수 있다. 그래서 범위를 “새 데모 기능”이 아니라 “이미 있는 데모 결과를 공유 가능한 증거 패키지로 고정”하는 것으로 제한해야 한다.
- `curator`: 적용. compile 체크가 통과했고, 누락 dependency는 환경 제약으로 기록 가능하다. 코드 변경은 별도 구현 요청 전에는 하지 않는다.

## 1-3 step 적용안

1. **Evidence manifest 계약 정의**
   - 입력: `data/demo/clips.json`, demo FCPXML 산출물, 안전 체크 로그.
   - 출력 초안: `output/demo_evidence/manifest.json` 또는 `docs/demo-evidence/manifest.example.json`.
   - 필드: clip_count, sample_queries, roughcut_path, fcpxml_validation, provider_calls=false, real_media=false, qdrant_required=false, generated_at.

2. **정적 README 증거 페이지 설계**
   - 위치 후보: `docs/demo-evidence/README.md`.
   - 포함: 60초 설명 스크립트, 생성 파일 목록, no-provider/no-media 조건, FCPXML handoff 설명, 테스트 수집 제약(`cv2`, `qdrant_client` missing 가능) 문구.

3. **README 연결 기준 정리**
   - public README의 first-run demo 섹션에서 evidence pack 링크를 추가한다.
   - 단, 구현 시 실제 파일 생성 명령과 샘플 산출물이 존재할 때만 링크한다.

## 완료 기준

- `python3 -m compileall -q mv_brain` 통과 유지.
- evidence pack 문서가 실제 존재하는 fixture/산출물 경로만 가리킴.
- provider 호출, 실제 미디어 ingest, Qdrant mutation 없이 검증 가능하다고 명시.
- README 또는 public overview에서 “AI가 최종 MV 자동 완성”으로 오해될 표현이 없음.

## 관련 문서

- [[2026-05-10-shareable-demo-evidence-pack]]
- [[MV-BRAIN-프로젝트-브리프]]
- [[2026-05-02-MV-BRAIN-Saturday-Ship-No-media-demo-README]]
- [[2026-04-28-MV-BRAIN-No-media-검색데모팩-적용]]
- [[2026-05-05-mv-brain-agent-readable-cutlist-clip-pack-적용]]
