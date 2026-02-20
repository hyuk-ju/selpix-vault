---
type: log
status: final
tags: [architecture, pipeline, decision-log, trend-driven]
last_updated: 2026-02-20
---
# 인텔 토론 — 기획안: 트렌드 주도형 소싱 아키텍처 (2026-02-20)

## 📌 안건 (Agenda)
AI 트위터 뉴스 기반 소싱 로직 완전 폐기 및 "트렌드 스카우트 지시서(Sourcing Directive)" 기반의 신규 이벤트 주도형(Event-Driven) 파이프라인 아키텍처 도입.

---

## 🕵️‍♂️ trend-scout (데이터/추출 관점)
- **Veridict:** `APPROVE`
- **의견:** 
  현재 OpenAI 릴리즈 소식 같은 무관한 단어가 소싱 키워드로 들어가는 구조는 확실한 패착입니다. 
  `sourcing_directive.json` 통신 규격을 강제하면 필터링을 쉽게 걸 수 있습니다. 
  다만, 네이버 데이터랩이나 쿠팡 인기 상품 크롤링이 Block 당하는 경우를 대비해 스캐터랩 방식이나 여러 API를 fallback으로 준비해야 합니다.
- **제안 스펙:**
  ```json
  {
    "date": "YYYY-MM-DD",
    "directive_id": "TD-...",
    "keywords": [
      {
        "term": "봄맞이 카디건",
        "category": "fashion",
        "reason": "환절기 급상승",
        "target_margin_min": 25
      }
    ]
  }
  ```

## 💼 biz-writer (비즈니스 임팩트/리스크)
- **Veridict:** `APPROVE`
- **의견:**
  이전 방식은 "개발자 관점의 장난감"에 불과했습니다. AI 뉴스를 이커머스 상품 소싱에 다이렉트로 꽂는 것은 트래픽 전환율이 0에 가깝습니다.
  실제 쿠팡/네이버 트렌드와 시즌성(Seasonality)에 집중하면 임시저장(반려) 스트레스를 줄이고 CVR(전환율)을 높일 수 있습니다.
  **리스크:** 한 달 내내 똑같은 계절 상품만 소싱될 우려가 있으니, 지시서 생성 시 "최근 7일 소싱 이력"을 제외하는 로직이 필요합니다.

## 🛠 ops-dev (시스템 아키텍처/병목)
- **Veridict:** `APPROVE`
- **의견:**
  `pipeline_sourcing.js` 안에 있던 `loadTwitterKeywords` 함수를 통째로 뜯어내고 `candidate_keywords.json`의 의존성을 `sourcing_directive.json`으로 완전히 교체하는 작업 자체는 매우 직관적입니다. 
  문제는 이 지시서 포맷이 어긋났을 때 전체 파이프라인이 붕괴하지 않도록 사전 Payload 검증(`validateDirective`)이 필수적입니다. 지시서가 없으면 그 날 소싱 스킵 로직도 넣어야 합니다.

---

## 🏁 최종 결정 (Decision)
- **결과:** **전면 승인 (Approved by Consensus)**
- **액션 아이템:**
  1. `sourcing_directive.json` 스키마 픽스.
  2. 일일 트렌드 크론 `coupang-daily-trend.js` 또는 `.py` 개발.
  3. `pipeline_sourcing.js` 완전 리소스 교체 및 Twitter 연동 제거.
  4. 7일 소싱 이력 제외 안전장치 마련.
