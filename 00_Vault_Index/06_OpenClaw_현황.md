---
type: index
status: active
tags: [agents, openclaw, system-status, operations]
last_updated: 2026-02-20
---
# OpenClaw 운영 현황

> 이 문서는 대화에서 공유된 OpenClaw 에이전트 시스템 현황을 기반으로 작성됨
> 마지막 업데이트: 2026-02-20

---

## 에이전트 구성

| 에이전트 | 모델 | 역할 | 실행방식 |
|---------|------|------|---------|
| **main (나)** | claude-opus-4-6 → codex | 오케스트레이션 + 사용자 대화 | Telegram/Discord 직접 |
| **장 (ops-dev)** | gpt-5.3-codex | 코드 작성/수정, 스크립트, API 픽스 | sessions_spawn 위임 |
| **탐 (trend-scout)** | claude-sonnet-4-5 → codex | 리서치, 웹서치, YouTube, Twitter | 크론 isolated + spawn |
| **기 (biz-writer)** | claude-sonnet-4-5 → codex | 문서 작성, 전략 기획 | sessions_spawn 위임 |
| **달 (worker)** | gpt-5.3-codex | 배치 작업, 쿠팡 API 반복 처리 | sessions_spawn 위임 |

---

## 최근 7일 작업 TOP 5

1. **쿠팡 파이프라인 디버깅** — 가격 버그(minOrder), updateProduct export 누락, MOQ API 경로 오류 반복 수정
2. **크론 구성/재구성** — 7개 크론 설계·수정·통합 (twitter-intel → daily-briefing 등)
3. **에이전트 설정 변경** — 모델 교체, workspace 경량화, streamMode, notifyOnExit 설정
4. **쿠팡 상품 상태 관리** — 임시저장 복구, 반려 처리, 가격 일괄 업데이트, SEO 태그 확장
5. **인프라 구축** — Obsidian Vault + Syncthing 셋업, GitHub 백업 크론

---

## 반복 실패/에러 패턴

### ① 쿠팡 API 스펙 불일치
- NUMBER 속성 단위 접미사 누락 → 반려
- groupNumber 중복 전송 → 반려
- MOQ 업데이트 API 경로 불일치 (**현재 18건 미해결**)
- 원인: 쿠팡 API 문서가 불완전, 실제 동작과 상이

### ② 컨텍스트 과부하 → 서브에이전트 타임아웃
- 장(ops-dev)에 과도한 지시 → `LLM request rejected: context window exceeded`
- 해결책: task 파일 패턴 (current_task.md 경유) — 가끔 잊고 직접 전달

### ③ 크론 trigger 타임아웃
- 게이트웨이가 이전 작업 처리 중 연속 호출 시 60초 타임아웃
- evening + health-check 등록 실패 사례 있음

---

## 병목 지점 (우선순위)

| 순위 | 병목 | 증상 |
|------|------|------|
| 🥇 | 쿠팡 API 불안정 | 반려 이유 불명확, 임시저장 고착, MOQ 경로 오류 |
| 🥈 | ops-dev 컨텍스트 한계 | codex 모델 window 작음, 긴 지시 즉시 실패 |
| 🥉 | 트위터 인증 만료 | bird.env CT0 토큰 주기적 만료 → twitter-intel 중단 |
| 4 | 메모리 검색 불가 | OpenAI embedding API 없어 memory_search 작동 안 함 |
| 5 | 게이트웨이 동시성 | 크론 다수 동시 trigger 시 타임아웃 |

---

## 효율화 제안 (우선순위)

1. **쿠팡 API 오류 패턴 문서화** → 반려 사유별 자동 대응 로직 (장에게 위임)
2. **ops-dev task 파일 패턴 강제화** → AGENTS.md에 "장 위임 시 무조건 current_task.md 경유" 규칙 추가
3. **bird.env 자동갱신 알림 크론** → 토큰 만료 전 선제 알림
4. **memory_search 대안** → `scripts/memory_search.sh` 활용 (이미 존재, 습관화 필요)

---

## 현재 단계 평가

> 🟡 실험 → 운영 전환기 (75% 운영)

| 항목 | 평가 |
|------|------|
| 파이프라인 자동화 | ✅ 운영 수준 — 크론 7개, 자동 동기화/복구/모니터링 |
| 에러 복구 | 🟡 반자동 — 반려 상품 수동 판단 필요 |
| 수익 구조 | 🟡 초기 — 등록 50건, 판매 11건, 매출 미검증 |
| 모니터링 | 🟡 부분 — 보고는 되지만 자동 대응 미흡 |
| 확장성 | ✅ 있음 — 에이전트 구조 분리, 스케일 가능 |

**핵심**: MOQ 오류 18건 해결 시 완전 운영 단계 진입 가능

---

## 관련 문서
- [[00_HOME|🏠 홈 (자료 맵)]]
- [[03_기술개발_인덱스|🛠 기술개발 인덱스]]
- [[Projects/셀픽스-쿠팡-파이프라인|🛒 쿠팡 파이프라인]]
- [[Memory/운영-규칙|⚙️ 운영 규칙]]
- [[📊 대시보드|📊 대시보드]]
