---
type: idea-card
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: idea-discussion
verified_by: agent
sources:
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/mv_brain/effects/recommender.py
  - /home/dev/mv-brain/mv_brain/effects/fx_catalog.py
  - /home/dev/mv-brain/mv_brain/tests/test_effects.py
  - https://github.com/scriptituk/xfade-easing
  - https://github.com/qq2225936589/xfade-ffmpeg-script
  - https://reddit.com/r/editors/comments/1dkyy00/best_professional_heavy_sign_transition_packs_for/
  - https://reddit.com/r/VideoEditing/comments/atruzm/what_is_the_best_effectstransition_pack_out_there/
created: 2026-05-07
reviewed_at: 2026-05-07
---
# FX recommendation explainability gallery

## 한 줄 요약

MV BRAIN의 FX 추천 기능을 “추천합니다” 수준에서 끝내지 말고, provider 없이 재현 가능한 **FX 추천 설명 갤러리**로 만든다. 각 예시는 `전환 유형 → 추천 FX → 이유 → confidence → ffmpeg/FCPXML handoff 가능성`을 한 장 카드처럼 보여준다.

## 왜 지금인가

- README는 MV BRAIN의 핵심 기능 중 하나로 FX recommendation을 내세운다. 최근 아이디어는 no-provider demo, FCPXML/EDL handoff, MP4 preview, 검색 벤치마크, clip pack에 집중했지만 “왜 이 FX를 추천했는지”를 보여주는 포트폴리오 artifact는 아직 없다.
- `/home/dev/mv-brain/mv_brain/effects/recommender.py`의 `Recommendation`에는 이미 `suggested_effects`, `suggested_tags`, `confidence`, `based_on_patterns`, `reasoning` 필드가 있다. 즉 새 AI 기능보다 현재 구조를 읽기 쉬운 proof로 포장하는 작업이 우선이다.
- `/home/dev/mv-brain/mv_brain/effects/fx_catalog.py`는 FFmpeg xfade/filter, style_tags, energy, best_for, duration, GL 필요 여부를 담고 있다. 이 카탈로그를 표/카드로 보여주면 “편집 의사결정 + 실행 가능한 transition mapping”이 함께 보인다.
- `/home/dev/mv-brain/mv_brain/tests/test_effects.py`는 API 키 없는 fallback labeler, transition detector, recommender 테스트 fixture를 포함한다. 첫 MVP는 실제 미디어·provider·Qdrant 없이 synthetic frame fixture로 만들 수 있다.
- 외부 신호: GitHub `scriptituk/xfade-easing`은 FFmpeg xfade easing/GLSL transition port를 제공하고, `qq2225936589/xfade-ffmpeg-script`는 xfade로 여러 영상을 연결하는 스크립트다. 즉 FFmpeg 기반 transition handoff는 작은 도구 생태계가 있다.
- 외부 신호: Reddit r/editors와 r/VideoEditing에는 transition/effect pack 추천 질문이 반복된다. 사용자는 “AI 영상 생성”보다 구체적인 transition 선택, 과하지 않은 효과, 도구별 적용 가능성을 궁금해한다.
- 안전 점검: `/home/dev/mv-brain`는 `main...origin/main` clean이고 최근 커밋은 `fe8852f docs: align Korean overview with public positioning`; `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과. 오늘은 코드·미디어·provider·Qdrant 변경 없이 아이디어 카드만 기록했다.

## 선택 전 후보 검토

- 후보 A: Universal EDL handoff 후속 — 2026-05-06 카드와 직접 중복.
- 후보 B: Agent-readable cutlist pack 후속 — 2026-05-05 카드와 중복.
- 후보 C: FX recommendation explainability gallery — README의 미노출 핵심 기능을 눈에 보이는 artifact로 바꾸며, existing effects 모듈과 테스트 fixture를 재사용한다.
- 후보 D: 실제 transition render demo — MP4 preview/렌더링 쪽으로 커져서 daily scout 범위에서는 리스크가 높다.

## 내부 토론

### idea-scout

MV BRAIN이 “검색 → 추천 → 러프컷” 흐름을 말하려면 FX 추천도 설명 가능해야 한다. 편집자는 블랙박스 추천보다 “왜 slide_left인지, 어떤 에너지/씬에 맞는지, 안전한 fallback인지”를 원한다. 오늘 wedge는 모델 성능이 아니라 추천 의사결정의 투명성이다.

### builder

MVP는 1~3일 안에 작다. `test_effects.py`의 synthetic frame fixture와 fallback labeler를 사용해 5~8개 샘플 recommendation을 만들고, `fx_catalog.py`의 metadata를 붙여 `docs/fx-recommendation-gallery.md` 또는 README 섹션으로 정리한다. 첫 단계는 코드 구현이 아니라 “어떤 카드 포맷이 데모에 설득력 있는가”를 확정하는 것이다.

### market

포트폴리오 관점에서는 단순 CRUD보다 “영상 편집 도메인 모델링”을 보여준다. 수익 경로는 transition preset guide, editor workflow template, local setup/consulting으로 좁게 잡을 수 있다. 특히 Final Cut Pro 사용자뿐 아니라 FFmpeg/CapCut/Premiere로 넘어갈 수 있는 범용 설명 카드가 좋다.

### critic

가장 큰 실패 이유는 “FX 추천”이 취향 영역이라 정답처럼 보이면 신뢰를 잃는다는 점이다. 따라서 gallery는 `recommendation`, `confidence`, `why`, `when not to use`를 함께 보여줘야 한다. 또 실제 렌더링 가능 여부를 과장하지 말고 `requires_gl`과 built-in xfade를 분리해야 한다.

### curator

승인. 최근 카드와 중복이 낮고, repo 내부에 이미 추천 결과 필드·FX 카탈로그·fallback 테스트가 있다. fatal flaw는 과장된 취향 주장인데, explainability와 limitation을 카드 포맷에 포함하면 해소된다.

## 점수

| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 5 | 추천 시스템, 도메인 모델링, explainability, 테스트 fixture를 한 번에 보여줌 |
| revenue_fit | 4 | transition guide/template, editor workflow setup, demo consulting으로 좁은 수익 실험 가능 |
| build_ease | 5 | 기존 `Recommendation`, `FXEntry`, fallback tests를 재사용하고 첫 산출물은 문서/fixture 중심 |
| proof_speed | 5 | 1~2일 내 카드형 gallery와 README snippet 가능 |
| mv_brain_synergy | 5 | README 핵심 기능인 FX recommendation을 직접 강화 |
| risk_inverse | 4 | provider/미디어/Qdrant 없이 시작 가능. 단, 취향 추천 과장 리스크는 있음 |
| **합계** | **28/30** | approved |

## Verdict

`approved` — 총점 28/30, fatal flaw 없음. 단, 첫 산출물은 실제 렌더 결과가 아니라 no-provider FX recommendation explainability gallery다.

## MVP 계획

1. `docs/fx-recommendation-gallery.md` 초안 포맷을 정한다: transition type, detected cue, suggested FX, tags, confidence, reasoning, best_for, duration, render notes, when-not-to-use.
2. `test_effects.py`의 synthetic frame fixture를 기준으로 5~8개 샘플 케이스를 고른다. 실제 미디어, provider, Qdrant는 사용하지 않는다.
3. `fx_catalog.py`의 built-in xfade와 `requires_gl` 항목을 분리해 “바로 가능한 FX”와 “옵션 빌드 필요 FX”를 명확히 표시한다.
4. README에는 “FX 추천은 editor-in-the-loop이며 정답이 아니라 후보와 이유를 제시한다”는 문구를 넣는다.

## 리스크와 제한

- 취향 기반 추천을 정답처럼 포장하면 안 된다.
- 실제 렌더링, GL transition, FCPXML 적용은 후속 proof가 필요하다.
- `based_on_patterns`가 0인 fallback 예시는 “학습된 취향”이 아니라 “rule/fallback suggestion”으로 표시해야 한다.

## 다음 1개 액션

코드 변경 전에 `docs/fx-recommendation-gallery.md`에 들어갈 카드 5개를 종이에 쓰듯 정리한다. 각 카드는 “추천 FX + 이유 + 쓰지 말아야 할 경우”를 반드시 포함한다.

## 관련 문서

- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-05-03-windows-mp4-roughcut-preview|Windows MP4 roughcut preview]]
- [[Research/Ideas/2026-05-04-no-provider-search-quality-benchmark|No-provider search quality benchmark]]
- [[Research/Ideas/2026-05-05-agent-readable-cutlist-clip-pack|Agent-readable cutlist clip pack]]
- [[Research/Ideas/2026-05-06-universal-edl-handoff-smoke|Universal EDL handoff smoke]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
