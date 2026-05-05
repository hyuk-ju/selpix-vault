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
  - /home/dev/mv-brain/mv_brain/tests/test_search.py
  - /home/dev/mv-brain/mv_brain/tests/test_rag_benchmark.py
  - https://github.com/ClipABit/.github
  - https://reddit.com/r/editors/comments/1qi2fnd/i_couldnt_afford_an_enterprise_mam_so_i_built_one/
  - https://reddit.com/r/finalcutpro/comments/1rrhy7p/my_fcp_keyword_system_completely_broke_down_after/
created: 2026-04-28
reviewed_at: 2026-04-28
---
# No-media 클립 검색 데모 팩

## 한 줄 요약
MV BRAIN의 자연어 클립 검색을 실제 영상·Gemini·Qdrant 장기 저장소 없이도 보여줄 수 있게, 작은 가상 클립 fixture와 검색 품질 골든 쿼리, README 데모 명령을 묶은 1~3일짜리 포트폴리오 증거로 만든다.

## 왜 지금인가
- MV BRAIN README는 `natural-language clip search`, 3-channel embedding(`visual`, `audio`, `style`), Qdrant 기반 검색을 핵심 약속으로 제시한다.
- repo에는 이미 `mv_brain/tests/test_search.py`와 `test_rag_benchmark.py`가 있어 더미 임베딩, 50개 클립/20개 쿼리, Recall@K/MRR 같은 검색 증거를 만들 기반이 있다.
- 외부 신호: ClipABit은 “video editors search through their footage using natural language”라는 문제를 정면으로 다루는 GitHub 프로젝트다. 즉 “편집자가 footage를 자연어로 찾고 싶다”는 개발자/에디터 니즈가 확인된다.
- 외부 신호: r/editors에는 “enterprise MAM이 비싸서 100% local semantic search MAM을 만들었다”는 글이 있고, r/finalcutpro에는 장기 사용 시 FCP keyword system이 무너졌다는 글이 있다. MV BRAIN의 local-first, 검색/메타데이터 보조 포지션과 직접 맞닿는다.
- 전날 아이디어가 FCPXML export 증거였다면, 오늘 아이디어는 그 앞단인 “어떤 클립을 왜 골랐는지”를 API 비용 없이 보여주는 증거다.

## 선택 전 후보 검토
- 후보 A: No-media 클립 검색 데모 팩 — 검색/메타데이터 고통과 repo 테스트 기반이 맞아 1~3일 proof가 빠르다.
- 후보 B: 실제 Gemini 검색 품질 벤치마크 — 증거는 강하지만 API 비용과 미디어 준비가 필요해 daily idea로는 무겁다.
- 후보 C: FCPXML import 검증 후속 — 어제 카드와 중복되어 오늘 새 아이디어로는 제외한다.
- 후보 D: Hermes/Obsidian idea QA — 운영 개선이지만 MV BRAIN 시너지가 낮다.

## 내부 토론
### idea-scout
MV BRAIN의 가장 이해하기 쉬운 문제는 “촬영본이 많을 때 원하는 장면을 빨리 찾기 어렵다”이다. FCPXML은 결과물이고, 그 전에 검색이 설득돼야 한다. 외부 커뮤니티도 footage organization, semantic MAM, keyword system 붕괴 문제를 반복적으로 말한다.

### builder
MVP 범위는 코드 변경 없이도 PRD로 시작 가능하고, 구현해도 작다.
1. `examples/search-demo/`에 10~20개 가상 클립 JSON fixture를 둔다.
2. “후렴 고에너지 댄스 클로즈업”, “브릿지용 느린 감성 컷” 같은 5~8개 골든 쿼리와 기대 결과를 정의한다.
3. 기존 `test_search.py`/`test_rag_benchmark.py` 패턴을 재사용해 deterministic dummy embedding 또는 간단한 lexical fallback으로 smoke demo를 만든다.
4. README에는 “영상·API 키 없이 검색 제품 스토리 확인” 섹션과 예상 출력 스니펫만 추가한다.

### market
타깃은 기술형 영상 편집자, Final Cut Pro 사용자, local-first AI tool에 관심 있는 개발자다. 수익 경로는 당장 SaaS가 아니라:
- GitHub/README에서 검색 품질을 보여주는 포트폴리오 산출물
- local footage search 템플릿/설치 대행
- 개인/소규모 팀용 로컬 MAM-lite 컨설팅 리드
로 좁게 잡을 수 있다.

### critic
가장 큰 리스크는 dummy fixture가 실제 검색 품질을 과장한다는 점이다. 따라서 “검색 품질 입증”이 아니라 “제품 데모와 회귀 테스트 골든셋”으로 포지셔닝해야 한다. 실제 Gemini/Qdrant 품질은 별도 slow benchmark로 분리하고, README에 dummy 결과는 production 품질 증거가 아니라고 명시해야 한다.

### curator
승인. 어제 FCPXML demo pack과 중복되지 않고, MV BRAIN의 앞단 가치인 clip triage/search를 안전하게 보여준다. 외부 수요 신호가 있고, 현재 repo 테스트 구조를 재사용하므로 1~3일 내 보이는 산출물이 가능하다.

## 점수
| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 5 | 검색 fixture, benchmark, README 출력으로 개발 역량이 바로 보임 |
| revenue_fit | 4 | local MAM-lite/검색 템플릿/설치 대행으로 좁은 유료화 가능 |
| build_ease | 4 | 기존 테스트와 더미 임베딩 재사용, API/실제 미디어 불필요 |
| proof_speed | 5 | JSON fixture와 README 스니펫만으로 1~3일 내 증거 가능 |
| mv_brain_synergy | 5 | README 핵심인 자연어 클립 검색과 3-channel 구조에 직접 연결 |
| risk_inverse | 4 | dummy demo 과장 리스크는 라벨링과 slow benchmark 분리로 관리 가능 |
| **합계** | **27/30** | approved |

## Verdict
`approved` — 총점 27/30, fatal flaw 없음. 단, dummy fixture 결과를 실제 검색 품질 보장으로 주장하지 않는다.

## 다음 1개 액션
`/home/dev/mv-brain`에서 `examples/search-demo/` 기준으로 가상 클립 12개, 골든 쿼리 6개, README 예상 출력 1개를 설계하는 작업 메모를 만든다. 코드 변경은 별도 사용자 요청 전까지 하지 않는다.

## 관련 문서
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML 계약 테스트 + 데모 팩]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
