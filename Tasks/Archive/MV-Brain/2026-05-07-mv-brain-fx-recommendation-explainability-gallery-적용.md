---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-05-07-fx-recommendation-explainability-gallery.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/mv_brain/effects/recommender.py
  - /home/dev/mv-brain/mv_brain/effects/fx_catalog.py
  - /home/dev/mv-brain/mv_brain/tests/test_effects.py
created: 2026-05-07
reviewed_at: 2026-05-07
---
# MV BRAIN FX recommendation explainability gallery 적용

## 적용 판정

`apply` — 오늘 아이디어는 MV BRAIN의 기존 FX 추천 구조를 포트폴리오용 설명 갤러리로 바꾸는 작은 문서/fixture 작업으로 적용한다. 코드 구현은 이 크론에서 하지 않는다.

## 확인한 repo 상태

- 브랜치: `main...origin/main`
- working tree: clean
- 최근 커밋: `fe8852f docs: align Korean overview with public positioning`
- 안전 점검: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과
- 좁은 테스트: `python3 -m pytest mv_brain/tests/test_effects.py -q`는 `ModuleNotFoundError: No module named 'cv2'`로 collection 단계 중단. 적용 작업은 먼저 문서/카드 포맷으로 제한한다.

## 내부 적용 토론

### builder

첫 구현은 `docs/fx-recommendation-gallery.md` 초안이다. `Recommendation.reasoning`, `confidence`, `based_on_patterns`와 `FXEntry`의 `ffmpeg_filter`, `style_tags`, `energy`, `best_for`, `duration_sec`, `requires_gl`를 카드 필드로 매핑하면 된다.

### market

이 산출물은 “AI가 알아서 편집”이 아니라 “편집자가 검토 가능한 FX 후보와 이유”를 보여준다. 포트폴리오에서는 추천 시스템/도메인 모델링/실행 handoff를 보여주고, 수익 실험으로는 transition preset guide 또는 editor workflow template로 이어질 수 있다.

### critic

현재 로컬 환경에서 `cv2`가 없어 `test_effects.py`를 바로 증명하지 못했다. 따라서 첫 작업에서 실제 추천 결과를 과장하면 안 된다. gallery는 `sample card draft`로 시작하고, “실제 테스트 fixture 생성은 OpenCV 의존성 설치/검증 후”로 분리해야 한다.

### curator

적용 승인. 단, 오늘 작업 단위는 코드가 아니라 설명 갤러리 설계와 카드 5개 초안이다. 테스트 실패를 고치려는 의존성 설치/코드 변경은 별도 구현 요청 때 처리한다.

## 1-3단계 적용안

1. `docs/fx-recommendation-gallery.md`의 카드 포맷을 먼저 만든다: `detected cue`, `suggested FX`, `why`, `confidence`, `render notes`, `when not to use`.
2. `fx_catalog.py`에서 provider 없이 설명 가능한 built-in FX 5개를 고른다: `cross_dissolve`, `fade_black`, `slide_left`, `wipe_left`, `zoom_in`.
3. README에는 한 문장만 연결한다: “FX recommendation is editor-in-the-loop; it provides candidates, reasons, and limitations, not a final creative answer.”

## 완료 기준

- `docs/fx-recommendation-gallery.md`에 최소 5개 카드가 있다.
- 각 카드에 `why`와 `when not to use`가 모두 있다.
- GL 필요 FX와 built-in xfade FX가 섞이지 않는다.
- 코드 변경 후에는 최소 `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server`를 다시 통과시킨다.
- `cv2` 의존성이 준비되기 전까지 `test_effects.py` 통과를 완료 기준으로 주장하지 않는다.

## 관련 문서

- [[Research/Ideas/2026-05-07-fx-recommendation-explainability-gallery|FX recommendation explainability gallery]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-05-04-no-provider-search-quality-benchmark|No-provider search quality benchmark]]
- [[Research/Ideas/2026-05-06-universal-edl-handoff-smoke|Universal EDL handoff smoke]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
