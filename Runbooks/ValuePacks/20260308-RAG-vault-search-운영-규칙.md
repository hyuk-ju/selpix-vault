---
title: RAG·vault-search 운영 규칙
type: value-pack
source_agent: openclaw-main
confidence_level: high
status: active
note_status: permanent
created: 2026-03-08
tags: [value-pack, rag, vault-search, search, knowledge-base]
last_verified: 2026-03-08
rule_scope: rag
---

# RAG·vault-search 운영 규칙

summary: vault-search는 ChromaDB + OpenAI/Azure OpenAI text-embedding-3-small + BM25 + RRF 융합 구조이며, 기존 vault 파일을 바꾸지 않는 별도 레이어로 유지한다.
decision: RAG는 기존 vault를 건드리지 않고 옆 레이어로 유지하며, 검색 score는 순수 벡터값이 아닌 최종 관련도 점수로 해석한다.
evidence_paths:
  - Projects/20260228-OpenClaw-RAG-구현-지시서.md
  - docs/20260308-벨류팩-도입-가이드.md
  - Templates/벨류팩-템플릿.md
exceptions:
  - memory_search는 현재 별도 메모리 검색 경로라 vault-search와 동일한 엔진으로 취급하지 않는다.
  - 문서 설명과 실제 코드가 다르면 구현 코드를 우선 근거로 본다.
action_rule:
  - 관련 문서 탐색 시 vault-search를 먼저 사용하고, 상위 결과 파일을 추가로 읽는다.
  - 검색 결과의 score는 관련도, confidence_level은 문서 신뢰도 메타데이터로 구분한다.
  - 기존 문서를 일괄 개조하지 말고 신규 판단 문서부터 벨류팩 포맷을 적용한다.

## evidence
- RAG 구현 지시서에 기존 Vault 파일 구조를 변경하지 않고 별도 레이어로 추가하라는 절대 원칙이 명시돼 있다.
- 현재 구현은 ChromaDB 벡터 검색과 BM25 키워드 검색을 RRF로 합쳐 score를 계산한다.
- 벨류팩 도입 가이드는 기존 파서 호환 키를 유지하면서 점진 도입하는 방식을 권장한다.

## notes
- score는 검색엔진 결과값이라 문서 자체 신뢰도와 동일하지 않다.
- 새 메타데이터 필드 추가 전에는 기존 파서가 읽는 키(type, source_agent, confidence_level 등)를 유지하는 것이 안전하다.

## related
- [[Projects/20260228-OpenClaw-RAG-구현-지시서]]
- [[docs/20260308-벨류팩-도입-가이드]]
- [[Templates/벨류팩-템플릿]]
