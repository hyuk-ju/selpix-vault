---
type: research
note_status: permanent
confidence_level: high
source_agent: claude-code
generated_via: manual
verified_by: human
sources:
  - "Twitter/X: Dan Malone (@danmalone), ZeroFutureTech, agent swarm practitioners"
  - "YouTube: n8n tutorials, CrewAI courses, Anthropic SDK docs, alert fatigue guides"
  - "Korean community: 도매꾹 OpenAPI, 나노바나나 n8n, 쿠파일럿, 판다랭크"
  - "Technical: Unwind AI, DEV.to, IBM alert fatigue, InfoQ observability, HITL/HOTL spectrum"
created: 2026-02-25
reviewed_at: 2026-02-25
---

# OpenClaw 운영자 경험(Operator UX) 개선 레퍼런스

> 34개 크론잡 + 8개 에이전트를 혼자 운영하는 오퍼레이터의 피로도를 줄이고, 시스템 자율성을 높이기 위한 조사 결과.

---

## 1. Alert Fatigue 해결 — 3-Tier + Calm Mode

### 문제
- 현재 34개 크론잡 중 22개가 Telegram `announce` 모드로 메시지 전송
- 모든 메시지가 동일 우선순위 → 중요한 것도 묻힘

### 레퍼런스
- **IBM Alert Fatigue Research**: 70%+ 알림이 noise → 운영자가 "전부 무시" 습관 형성
- **Dan Malone (Twitter)**: 동일 아키텍처(Telegram+multi-agent)에서 "exception-only notification" 패턴 적용
- **n8n community**: "digest node" 패턴 — 개별 알림 → 일정 시간 누적 후 요약 발송

### 적용 방안
```
CRITICAL (즉시 전송): 품절, 등록 실패, 시스템 장애
WARNING  (조용히 기록): 가격 변동, 트렌드 변화
INFO     (다이제스트): 일상 보고, 통계, 건강 체크 OK
```

### Calm Mode (야간 모드)
- 23:00~08:00 KST: CRITICAL만 전송, 나머지 아침 다이제스트에 합산
- ZeroFutureTech 사례: "night silent mode" → 오퍼레이터 수면 품질 개선 보고

---

## 2. 자율성 수준 (Autonomy Spectrum)

### 레퍼런스
- **HITL/HOTL 스펙트럼** (InfoQ, academic papers):
  - HITL (Human-in-the-loop): 매 결정마다 승인 필요
  - HOTL (Human-on-the-loop): 예외만 보고, 나머지 자율
  - Full Auto: 모든 결정 자율

### 현재 OpenClaw 상태
- 대부분 HITL 수준 — 크론 결과를 매번 사람이 확인
- 목표: 일상 작업은 HOTL, 비용 발생/외부 API 호출만 HITL

---

## 3. Agent Confidence Signal + Escalation

### 레퍼런스
- **Anthropic SDK Docs**: tool_use 결과에 confidence score 포함 가능
- **DEV.to agent failure cases**: "에이전트가 확신 없이 행동 → 데이터 오염" 사례 다수
- **CrewAI (YouTube)**: agent role에 "불확실하면 멈추고 보고" 규칙 삽입

### 적용 방안
- 프롬프트에 신뢰도 표시 규칙 추가: `🟢 확신 / 🟡 불확실 / 🔴 위험`
- 🔴 발생 시 자동 에스컬레이션 (실행 보류 + 텔레그램 알림)

---

## 4. Observability — Langfuse / AgentOps

### 레퍼런스
- **Langfuse**: self-hosted 가능, 세션별 비용/토큰/latency 추적
- **AgentOps**: SaaS, 더 쉬운 셋업이지만 데이터 외부 전송
- **InfoQ 3-phase observability**: Log → Metric → Trace 순차 도입 권장

### OpenClaw 적용 우선순위
- Phase 1: 크론 실행 결과를 structured JSON으로 기록 (이미 `state` 필드 부분 존재)
- Phase 2: 일별/주별 집계 대시보드
- Phase 3: Langfuse 연동 (비용 추적)

---

## 5. 도매꾹 Official API 전환

### 레퍼런스
- **도매꾹 OpenAPI**: `domeggook.com/main/OpenApi/OpenApiIntro.asp`
- **한국 커뮤니티**: 스크래핑 차단 강화 추세, API 전환 권장
- 현재 pipeline_sourcing.js가 웹 스크래핑 → 불안정

### 적용 방안
- API 키 발급 → pipeline_sourcing.js 리팩토링
- 카테고리 검색, 상품 상세, 가격 조회 엔드포인트 활용

---

## 6. Per-Agent KPI Dashboard

### 레퍼런스
- **The Unwind AI**: 24/7 agent team → 에이전트별 성공률/비용/실행시간 추적
- **weekly-kpi-review** 크론잡이 이미 존재하나 구조화된 메트릭 부족

### 적용 방안
- 각 에이전트별: 실행 횟수, 성공률, 평균 실행시간, 토큰 소비
- 주간 자동 리포트 → Telegram topic 29 (비즈 보고)

---

## 우선순위 정리

| # | 항목 | 난이도 | 영향도 | 비고 |
|---|------|--------|--------|------|
| 1 | Alert 3-tier + Calm Mode | 낮음 | 매우 높음 | 프롬프트+다이제스트 스크립트 |
| 2 | n8n self-hosting | 중간 | 높음 | 시각적 모니터링 |
| 3 | Confidence Signal | 낮음 | 높음 | 프롬프트만 수정 |
| 4 | Langfuse/AgentOps | 중간 | 중간 | 비용 추적 |
| 5 | 도매꾹 API 전환 | 중간 | 중간 | 안정성 |
| 6 | Per-agent KPI | 높음 | 중간 | 데이터 수집 체계 필요 |

---

## 관련 문서
- [[📊 대시보드]]
- [[Memory/변경로그]]
