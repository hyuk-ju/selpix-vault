---
type: adr
status: active
created: 2026-02-20
updated: 2026-02-20
owner: main
tags: [idea, openclaw, adr]
related: [[Research/Ideas/2026-02-20-OpenClaw CRM API 레이어로 영업 자동화]]
---
# ADR-001: OpenClaw CRM API 레이어로 영업 자동화

## 맥락 (Context)
source: data/twitter-intel/raw/20260219.json (OpenClaw 쿼리 캡처)
요약: @openclaw 봇에 전용 CRM 레이어를 붙여 API로 contact/deal/task/note/stage를 직접 조작, 후속조치 자동화 및 구조화된 맥락 조회를 가능하게 함.
전개: ① OpenClaw 플러그인(리드 CRUD) 템플릿 제공 ② 내부 영업팀 없는 1인 크리에이터/판매사 대상으로 연락처·파이프라인 자동화 패키지로 상품화 ③ 월정액 + 설정 대행 옵션 제안

## 결정 (Decision)
**✅ 추천 (주의 필요)**
- 시장: 보류 — DVS는 목표 타깃 적합도/계절성/마진 측면에서 높지만, 현재 시장 수요는 기존 CRM 인식이 하락 추세(CRM 키워드 -59.3%)여서 타이밍 리스크가 큽니다. 또한 가격경쟁력은 낮지만 상위 CRM 강세 및 로켓배송/대형 플랫폼 우위 때문에 초반은 선점형 포지셔닝이 필요합니다.
- 기술: 추천 — API 및 리소스 기반이 기구축되어 있어 기술적 허들이 낮으며, 런칭 속도가 빠르므로 시도해볼 가치가 큼.
- 비즈니스: 추천 — SaaS의 빠른 수익화와 1인 기업 타겟팅이 명확하며, 마진 페이로드가 커 비즈니스 임팩트가 우수함.
- Critic: 보류 — 1인/소규모 사업자 대상 CRM은 니즈 파악(PMF) 입증이 부족함. 시장 수요 둔화 시그널과 유사 도구의 무료 옵션이 있어 시장 검증(Waitlist 등) 결과 이후 재평가 요망.

## 검토한 대안 (Options)
(분석 불가)

## 트레이드오프 (Consequences)
- 실행 시: OpenClaw 봇 연동을 위한 랜딩페이지 및 대기명단(Waitlist) 폼 오픈 → 트위터/유튜브에 '영업 안 놓치는 1인 비서 만들기' 플레이북 배포 → 핵심 API 연동(Contact/Deal) 모듈 템플릿 릴리즈 클로즈 베타 테스트
- 미실행 시: 기회 비용 발생

## 리스크 및 탐지 신호
| 리스크 | 내용 | 탐지 신호 |
|--------|------|-----------|
| 치명적 결함 | 1인 기업이나 크리에이터는 CRM의 필요성을 느끼지 못하거나, Notion 등 무료 도구로 이미 해결하고 있을 가능성이 매우 높음 | 초기 검증 실패 |
| 시장 리스크 | B2B SaaS 피로도 증가, CRM은 이미 포화 시장 | 수요 지표 하락 |
| 실행 리스크 | OpenClaw 봇의 스킬 연동 과정에서 사용자 이탈 빈번 | MVP 일정 초과 |

## 관련 Spec / Runbook
- [[Research/Ideas/2026-02-20-OpenClaw CRM API 레이어로 영업 자동화|아이디어 카드]]
