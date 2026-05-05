---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-04-28-no-media-clip-search-demo-pack.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/mv_brain/tests/test_search.py
  - /home/dev/mv-brain/mv_brain/tests/test_rag_benchmark.py
created: 2026-04-28
reviewed_at: 2026-04-28
---
# MV BRAIN No-media 검색 데모팩 적용 작업

## 적용 판정
`apply`

오늘 승인된 아이디어 `No-media 클립 검색 데모 팩`은 MV BRAIN의 핵심 가치인 “많은 촬영본에서 원하는 장면을 자연어로 빠르게 찾기”를 실제 영상, Gemini 호출, Qdrant 장기 저장소 없이도 보여주는 포트폴리오 증거로 전환할 수 있다.

## 확인한 현재 상태
- repo: `/home/dev/mv-brain`
- git 상태: `main...origin/main`, 미추적 `AGENTS.md` 존재
- 최근 커밋: `044628c Build source-first MV Brain MVP`
- 안전 점검: `python3 -m compileall -q mv_brain` 통과
- README는 자연어 검색, 3-channel embedding(`visual`, `audio`, `style`), Qdrant 기반 clip memory를 핵심 워크플로로 이미 설명한다.
- `mv_brain/tests/test_search.py`에는 Qdrant local mode, `QueryBuilder`, dummy embedding, metadata filter 검색 테스트가 있다.
- `mv_brain/tests/test_rag_benchmark.py`에는 50개 가상 클립/20개 쿼리, Recall@K/MRR, Gemini optional + dummy fallback 구조가 있다.
- 아직 `examples/search-demo/` 같은 “README에서 그대로 볼 수 있는 no-media demo fixture”는 확인되지 않았다.

## 내부 적용 토론
### builder
1일 적용 범위는 새 모델 품질 개선이 아니라 “제품 스토리 + 회귀용 골든셋”을 만드는 것이다. 기존 테스트의 더미 임베딩/벤치마크 구조를 재사용하고, 외부 API 없이 fixture JSON과 골든 쿼리, 예상 출력 스니펫을 먼저 고정한다.

### market
이 작업은 GitHub/README 방문자가 “MV BRAIN이 어떤 문제를 푸는지”를 빠르게 이해하게 만든다. 수익화는 당장 SaaS보다 local MAM-lite 설치/템플릿, 편집자용 검색 워크플로 컨설팅 리드로 좁게 연결된다.

### critic
가장 큰 리스크는 dummy fixture가 실제 의미 검색 품질처럼 과장되는 것이다. 따라서 README와 작업 정의에서 `production search quality proof`가 아니라 `no-media product demo + regression golden set`이라고 명확히 라벨링해야 한다. 실제 Gemini/Qdrant 품질 벤치마크는 별도 slow benchmark로 분리한다.

### curator
`apply`로 전환한다. 코드 변경은 이 크론에서 하지 않고, 다음 개발 세션에서 1-3단계로 처리한다.

## 1-3 step 적용안
1. **검색 데모 fixture 설계**
   - 후보 경로: `examples/search-demo/clips.json`, `examples/search-demo/golden_queries.json`.
   - 범위: 가상 클립 12개, 골든 쿼리 6개.
   - 필드: `clip_id`, `source`, `description`, `channel_tags`, `bpm`, `energy`, `shot_type`, `grade`, `expected_use`.

2. **no-media smoke demo 명령/테스트 정의**
   - 기존 `test_search.py`/`test_rag_benchmark.py`의 dummy embedding·filter 패턴을 재사용한다.
   - 목표: “후렴 고에너지 댄스 클로즈업”, “브릿지용 느린 감성 컷”, “A컷 후보” 같은 쿼리가 fixture의 기대 clip_id를 반환하는지 확인한다.
   - 실제 Gemini, 실제 미디어 ingest, Qdrant 영구 저장소 쓰기는 제외한다.

3. **README 데모 섹션 추가 기준 정리**
   - 섹션명 후보: `No-media clip search demo`.
   - 포함: fixture 기반 실행 명령, 예상 출력 1개, dummy demo의 한계 문구.
   - 문구 제한: “실제 검색 품질 보장”이 아니라 “제품 흐름과 회귀 테스트를 API 비용 없이 확인”으로 설명한다.

## 완료 기준
- `python3 -m compileall -q mv_brain` 통과 유지
- no-media fixture/golden query가 repo에 추가되고, 실제 실행 가능한 smoke 명령이 README와 일치
- README에 dummy demo 한계와 real Gemini/Qdrant benchmark 분리 기준이 명시됨
- 실제 미디어 파일, provider API, 대용량 Qdrant 쓰기 없이 검증 가능

## 관련 문서
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML 계약 테스트 + 데모 팩]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
