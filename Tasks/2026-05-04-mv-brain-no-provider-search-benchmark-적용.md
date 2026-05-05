---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-05-04-no-provider-search-quality-benchmark.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/mv_brain/tests/test_rag_benchmark.py
  - /home/dev/mv-brain/mv_brain/tests/test_search.py
  - /home/dev/mv-brain/mv_brain/demo.py
created: 2026-05-04
reviewed_at: 2026-05-04
---
# MV BRAIN No-provider 검색 품질 벤치마크 적용

## 적용 판정

`apply` — 오늘 승인 아이디어는 코드 변경 없이도 다음 개발 세션에서 바로 실행 가능한 작은 문서/fixture 작업으로 전환할 수 있다.

## 대상 프로젝트

- `/home/dev/mv-brain`
- 핵심 가치: 자연어 clip search를 “말”이 아니라 benchmark contract와 fixture 표로 보여주기

## 현재 근거

- repo 상태: `main...origin/main`, working tree clean.
- 최근 커밋: `3604db7 chore: refresh UI lockfile audit`, `393aee3 chore: initial public-ready release`.
- 안전 점검: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과.
- `mv_brain/tests/test_rag_benchmark.py`에는 5개 장르 × 10개 클립 = 50개 fixture, 20개 쿼리, Recall@3/Recall@5/MRR helper가 이미 있다.
- `mv_brain/demo.py`에는 no-media demo clip 8개와 example queries 3개가 있다.

## 내부 토론

### builder

가장 작은 적용 단위는 `/home/dev/mv-brain/docs/search-benchmark.md` 초안과 README용 5문장 snippet이다. 새 검색 알고리즘이나 provider 호출이 아니라, 기존 테스트 자산을 포트폴리오 proof로 설명하는 작업으로 제한한다.

### market

방문자에게 “AI 검색 가능”보다 “50 clip/20 query, Recall@K/MRR로 검증하려는 구조가 있다”가 더 설득력 있다. 이후 유료화는 local media search setup, editor taxonomy/eval template, 작은 컨설팅 lead magnet으로 연결된다.

### critic

더미 임베딩 결과를 실제 검색 정확도로 포장하면 신뢰가 무너진다. 문서 제목과 표에는 반드시 `no-provider structural benchmark`와 `provider-backed accuracy benchmark`를 분리해야 한다.

### curator

적용 승인. 단, 이번 작업의 완료 기준은 “실제 accuracy 수치 공개”가 아니라 “평가 contract, fixture, 안전한 실행 경계가 이해되는 문서 산출물”이다.

## 1-3 step 적용안

1. `mv_brain/tests/test_rag_benchmark.py`의 `GENRE_CONFIG`/`TEST_QUERIES`를 근거로 `docs/search-benchmark.md` 초안에 장르 5개, 쿼리 20개, Recall@3/Recall@5/MRR 정의를 표로 정리한다.
2. `mv_brain/demo.py`의 8개 demo clip과 example queries 3개를 “no-media demo query → expected clip attributes” 표로 붙이고, 실제 미디어/provider/Qdrant 없이 보여주는 범위를 명시한다.
3. README에 넣을 5문장 snippet을 작성한다: “검색 품질을 어떻게 검증하는가”, “더미/fixture 모드가 증명하는 것”, “provider-backed run 전까지 주장하지 않는 것”을 구분한다.

## 완료 기준

- `/home/dev/mv-brain/docs/search-benchmark.md` 초안이 생긴다.
- README에 바로 붙일 수 있는 5문장 snippet이 문서 하단에 있다.
- 문서에 `no-provider structural benchmark != real semantic accuracy` 경고가 포함된다.
- 개발 세션에서 실행할 안전 체크는 `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server`와 provider 없는 관련 테스트로 제한한다.

## 하지 말 것

- 이 적용 리뷰 크론에서 `/home/dev/mv-brain` 코드를 수정하지 않는다.
- Gemini/OpenAI/Anthropic provider 호출을 하지 않는다.
- 실제 미디어 ingest, Qdrant persistent mutation, FCP import를 하지 않는다.
- 더미 임베딩 점수를 실제 검색 품질처럼 홍보하지 않는다.

## 관련 문서

- [[Research/Ideas/2026-05-04-no-provider-search-quality-benchmark|No-provider search quality benchmark report]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
