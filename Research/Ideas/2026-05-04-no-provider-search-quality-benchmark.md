---
type: idea-card
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: idea-discussion
verified_by: agent
sources:
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/mv_brain/tests/test_rag_benchmark.py
  - /home/dev/mv-brain/mv_brain/tests/test_search.py
  - /home/dev/mv-brain/mv_brain/search/query_builder.py
  - /home/dev/mv-brain/mv_brain/demo.py
  - https://github.com/ssrajadh/sentrysearch
  - https://github.com/chn-lee-yumi/MaterialSearch
  - https://github.com/IliasHad/edit-mind
  - https://reddit.com/r/VideoEditing/comments/12qiykp/ive_built_a_massive_video_clip_search_engine/
  - https://reddit.com/r/editors/comments/lkq2ic/i_created_a_natural_language_video_search_engine/
  - https://reddit.com/r/editors/comments/qtbh4c/davinci_resolve_question_how_to_cut_clips_in/
created: 2026-05-04
reviewed_at: 2026-05-04
---
# No-provider search quality benchmark report

## 한 줄 요약

MV BRAIN의 “자연어로 원하는 클립을 찾는다”는 핵심 주장을, provider key·실제 미디어·Qdrant 운영 없이도 보여주는 **검색 품질 벤치마크 리포트/fixture pack**으로 만든다. 목표는 새 기능보다 포트폴리오 신뢰 proof다.

## 왜 지금인가

- README는 MV BRAIN의 핵심 가치를 clip triage, natural-language search, FX recommendation, FCPXML handoff로 설명한다. 최근 카드는 no-media demo, FCPXML, safety, MP4 preview를 다뤘지만 “검색이 얼마나 그럴듯하게 작동하는가”를 수치로 보여주는 artifact는 아직 별도 카드가 없다.
- repo 내부에는 이미 `mv_brain/tests/test_rag_benchmark.py`가 50개 클립 + 20개 쿼리, Recall@3/Recall@5/MRR 구조를 정의한다. Gemini API가 필요한 정확도 모드와 더미 임베딩 구조 테스트가 분리되어 있어, 첫 산출물은 paid/provider 호출 없이 만들 수 있다.
- `mv_brain/tests/test_search.py`와 `mv_brain/search/query_builder.py`는 BPM, mood, shot_type, quality_flags, song_position_bucket, scene_pacing_bucket 같은 필터를 검증한다. 즉 단순 “벡터 검색 됩니다”가 아니라 편집자용 조건 검색을 설명할 근거가 있다.
- `mv_brain/demo.py`는 8개 no-media clip fixture와 example queries를 이미 만든다. 벤치마크 리포트 MVP는 이 demo fixture와 기존 benchmark fixture를 연결해 `docs/search-benchmark.md` 형식의 읽을 수 있는 결과표를 만드는 것으로 충분하다.
- 외부 신호: GitHub 검색 결과 `ssrajadh/sentrysearch`는 “Semantic search over videos using Gemini Embedding 2 or Qwen3-VL”, `chn-lee-yumi/MaterialSearch`는 “Search local photos and videos through natural language”, `IliasHad/edit-mind`는 “Local-first Video Knowledge Base”를 내세운다. 즉 local/semantic video search는 이미 경쟁적으로 보이는 카테고리라, MV BRAIN은 claims보다 eval proof가 필요하다.
- 외부 신호: Reddit에는 “massive video clip search engine”, “natural language video search engine” 같은 게시물이 있고, r/editors에는 선택한 타임라인 클립을 meta tag/keyword로 검색하고 싶다는 질문도 있다. 편집자 pain은 “AI가 멋지다”가 아니라 “나중에 필요한 샷을 다시 찾을 수 있나”에 가깝다.
- 안전 점검: `git status --short --branch`는 `main...origin/main` clean, 최근 커밋은 `3604db7 chore: refresh UI lockfile audit`, `393aee3 chore: initial public-ready release`; `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과. 오늘 cron은 코드/미디어/provider/Qdrant 변경 없이 아이디어 카드만 기록했다.

## 선택 전 후보 검토

- 후보 A: Windows MP4 preview 구현 후속 — 2026-05-03 카드와 중복이라 오늘 신규 아이디어로 부적합.
- 후보 B: Read-only FCPXML MCP inspector 후속 — 2026-05-02 카드와 중복.
- 후보 C: Local API hardening 후속 — 2026-05-01 카드와 중복.
- 후보 D: No-provider search quality benchmark report — 기존 테스트 자산을 제품 proof로 전환하고, 최근 카드와 중복이 가장 낮다.

## 내부 토론

### idea-scout

MV BRAIN의 차별점은 “영상 생성 AI”가 아니라 편집자가 이미 가진 footage를 빨리 찾고 쓸 수 있게 하는 것이다. 외부에는 semantic video search repo와 Reddit 예시가 이미 많으므로, 다음 wedge는 새로운 검색 알고리즘보다 현재 repo가 어떤 쿼리를 어떤 기준으로 통과시키는지 보여주는 작은 benchmark report다.

### builder

MVP는 1~3일 범위로 작게 자른다. `test_rag_benchmark.py`의 50 clip/20 query 정의와 metric helper를 문서화하고, provider 없는 모드에서는 “semantic accuracy가 아니라 fixture/eval contract 검증”이라고 명시한다. `demo.py`의 8개 clip cards를 예시 테이블로 붙이면 실제 미디어 없이도 README에 넣을 수 있는 artifact가 생긴다. 구현으로 넘어가도 `docs/search-benchmark.md` + JSON fixture snapshot + one command 정도다.

### market

포트폴리오 방문자와 기술형 편집자는 “AI 검색”이라는 말보다 “어떤 쿼리 세트를 만들고, Recall/MRR로 어떻게 검증하려 했는지”를 더 신뢰한다. 수익 경로는 local media library/search setup, editor asset taxonomy template, benchmark-driven demo writeup으로 좁게 잡을 수 있다. 과장된 SaaS보다 컨설팅/템플릿 lead magnet에 가깝다.

### critic

가장 큰 실패 이유는 더미 임베딩 결과를 실제 검색 품질처럼 포장하는 것이다. 따라서 첫 리포트는 두 층으로 나눠야 한다: (1) no-provider structural benchmark는 CI/portfolio proof, (2) optional provider benchmark는 사용자가 키를 넣었을 때만 실제 Recall/MRR 측정. 오늘 카드의 승인 조건은 “정확도 수치를 만든다”가 아니라 “평가 contract와 fixture를 보이는 문서 artifact를 만든다”로 제한해야 한다.

### curator

승인. 최근 카드와 중복되지 않고, repo 내부에 이미 벤치마크 코드가 있으며, 외부 시장 신호도 충분하다. fatal flaw는 더미 결과 과장인데, 리포트 명칭과 README 문구에서 `no-provider contract`와 `provider-backed accuracy`를 분리하면 해소된다.

## 점수

| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 5 | 검색 품질을 Recall@K/MRR, fixture, command로 설명하면 개발자 포트폴리오 proof가 선명함 |
| revenue_fit | 4 | local media search setup, taxonomy/eval template, editor workflow 컨설팅으로 좁은 유료화 가능 |
| build_ease | 5 | 기존 benchmark/search/demo fixture를 재사용하고 첫 산출물은 문서/fixture snapshot 중심 |
| proof_speed | 5 | 1~2일 내 docs 리포트 + sample JSON + README snippet 가능 |
| mv_brain_synergy | 5 | MV BRAIN 핵심 가치인 자연어 clip search를 직접 강화 |
| risk_inverse | 4 | provider/미디어/Qdrant mutation 없이 시작 가능. 단, 더미 결과를 과장하면 신뢰 리스크가 있음 |
| **합계** | **28/30** | approved |

## Verdict

`approved` — 총점 28/30, fatal flaw 없음. 단, 첫 결과물은 “no-provider benchmark contract/report”이며 실제 semantic accuracy claim은 provider-backed optional run 전까지 보류한다.

## MVP 계획

1. `docs/search-benchmark.md` 초안: benchmark 목적, 50 clip/20 query 구조, Recall@3/Recall@5/MRR 정의, no-provider vs provider-backed 모드 차이를 명시한다.
2. `mv_brain/tests/test_rag_benchmark.py`의 장르/쿼리 표를 축약해 README에 넣을 수 있는 “검색 품질 리포트 예시”를 만든다.
3. `mv-brain demo`의 8개 clip fixture와 example queries를 연결해, 실제 미디어 없이 “query → expected clip attributes” snapshot을 보여준다.
4. optional provider run은 별도 명령으로만 남기고, cron/기본 demo에서는 절대 Gemini/OpenAI/Anthropic 호출을 하지 않는다.

## 리스크와 제한

- 더미 임베딩으로 나온 점수를 실제 검색 성능처럼 보여주면 안 된다.
- 외부 repo들이 이미 semantic video search를 표방하므로, MV BRAIN은 “검색 앱” 일반론보다 music-video editor workflow와 FCPXML handoff까지 이어지는 점을 강조해야 한다.
- 벤치마크가 실제 사용자 footage를 대표한다고 주장하지 말고, regression/eval harness의 시작점으로 표현한다.

## 다음 1개 액션

`/home/dev/mv-brain/docs/search-benchmark.md` 초안을 만들기 전, `test_rag_benchmark.py`의 20개 쿼리를 표로 정리하고 README에 들어갈 5문장 “검색 품질을 어떻게 검증하는가” 문구를 먼저 작성한다. 코드 변경은 별도 개발 세션에서만 한다.

## 관련 문서

- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Research/Ideas/2026-05-02-readonly-fcpxml-mcp-inspector|Read-only FCPXML MCP inspector]]
- [[Research/Ideas/2026-05-03-windows-mp4-roughcut-preview|Windows MP4 roughcut preview]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
