---
type: research
note_status: literature
confidence_level: medium
source_agent: claude-code
generated_via: manual
verified_by: none
sources:
  - vault_quality_eval.py (Ragas 0.4.3)
  - ChromaDB vault_docs collection (734 docs)
created: 2026-03-18
reviewed_at: 2026-03-18
---

# Vault 시맨틱 검색 품질 평가 (Ragas)

## 요약

ChromaDB 기반 vault 검색의 품질을 Ragas 프레임워크로 정량 평가.
5개 테스트 케이스, OpenAI text-embedding-3-small, gpt-4o-mini 기준.

## 전체 평균 점수

| 메트릭 | 점수 | 해석 |
|--------|------|------|
| context_precision | 0.24 | 검색된 문서 중 관련 있는 비율 **낮음** |
| context_recall | 0.20 | 정답에 필요한 정보를 찾은 비율 **매우 낮음** |
| faithfulness | 0.58 | 답변이 컨텍스트에 충실한 정도 **중간** |
| factual_correctness | 0.00 | ground_truth와 사실 일치 **실패** |

## 개별 결과

### Q1: 쿠팡 Vendor ID가 뭐야? (ground_truth: A01410454)
- **결과**: 정보 없음 (실패)
- **원인**: Vendor ID는 CLAUDE.md에만 존재, vault 문서에 미기록
- precision: 0.00 / recall: 0.00

### Q2: 에이전트 간 위임 규칙은?
- **결과**: 부분 성공 (위임-결과전달 규칙 문서 검색됨)
- **원인**: 관련 Runbook 존재, SOUL.md 라우팅 게이트 내용은 누락
- precision: 1.00 / recall: 1.00 / faithfulness: 0.875

### Q3: 네이버 DataLab API 제한은? (ground_truth: 최대 3개 카테고리)
- **결과**: 정보 없음 (실패)
- **원인**: API 제한사항은 CLAUDE.md/MEMORY.md에만 기록, vault 미인덱싱

### Q4: team_bus 메시지 TTL은? (ground_truth: 12시간)
- **결과**: 정보 없음 (실패)
- **원인**: 시스템 설정값이 vault에 분산/미기록

### Q5: 에러 알림 쿨다운 시간은? (ground_truth: 2시간)
- **결과**: 정보 없음 (실패)
- **원인**: 운영 파라미터가 vault에 체계적으로 기록되지 않음

## 근본 원인 분석

1. **운영 상수 미인덱싱**: Vendor ID, API 제한, TTL 등 핵심 운영 값이 CLAUDE.md/MEMORY.md에만 존재하고 vault ChromaDB에 인덱싱되지 않음
2. **snippet 절단 문제**: rag_search.py가 200자 snippet만 반환 -- 전체 문서 사용 시 Q2 성적이 precision 0.75 -> 1.00으로 향상
3. **검색 커버리지 갭**: 734개 문서 중 시스템 설정/운영 파라미터를 담은 전용 문서 부재

## 개선 권고

1. **운영 상수 전용 vault 문서 생성**: `vault/Reference/시스템-운영-상수.md` -- API 키, 제한, TTL 등 한곳에 집약
2. **CLAUDE.md/SOUL.md 인덱싱**: 현재 vault 디렉토리 외부 파일은 ChromaDB 미포함 -- 인덱서에 추가 필요
3. **rag_search.py snippet 확장**: 200자 -> 최소 500자, 또는 질문 유형에 따라 전체 문서 반환 옵션 추가

## Arize Phoenix 셀프호스팅 가능성

### 서버 스펙
- RAM: 7.7GB (가용 3.6GB)
- CPU: 4코어
- 디스크: 131GB 여유
- Docker: v29.2.1 사용 가능

### Phoenix 요구사항
- 최소 RAM: 4GB (권장 8GB+)
- Docker 이미지: `arizephoenix/phoenix:latest`
- 포트: 6006 (기본)
- 의존성: PostgreSQL (선택, SQLite도 가능)

### 판정: **조건부 가능**

- **가능한 이유**: Docker 사용 가능, 디스크 충분, SQLite 모드로 가벼운 운영 가능
- **리스크**: 가용 RAM 3.6GB로 빠듯함. OpenClaw + ChromaDB + Phoenix 동시 운영 시 OOM 가능성
- **권고**: `docker run -m 2g` 메모리 제한 걸고 테스트 후 판단. 상시 운영보다는 평가 시에만 기동하는 패턴 추천
- **대안**: Ragas 단독 스크립트(vault_quality_eval.py)로 정기 평가 충분. Phoenix는 트레이싱/디버깅이 필요할 때만 기동

## 관련 파일

- 평가 스크립트: `workspace/scripts/vault_quality_eval.py`
- 결과 JSON: `vault/Research/2026-03-18-vault-search-quality-eval.json`
- 검색 엔진: `workspace/scripts/rag_search.py`
- ChromaDB: `/home/dev/openclaw/rag-data/chroma.sqlite3`
