---
type: adr
status: active
created: 2026-02-21
updated: 2026-02-21
owner: main
tags: [idea, openclaw, adr]
related: [[Research/Ideas/2026-02-21-Outlook AI 초안 대행 에이전트 템플릿]]
---
# ADR-012: Outlook AI 초안 대행 에이전트 템플릿

## 맥락 (Context)
출처: X @BadBrainCode status 2021374299437744577 (source tag: track A/query built with OpenClaw). 실제로 @BadBrainCode는 Outlook API 제한 때문에 OpenClaw를 로컬 HTTPS add-in으로 우회해 개별 작성 톤 기반 자동 답장 초안을 구성했다. 적용안: Selpix/오픈클로우 고객 대상으로 '브랜드 톤 템플릿 + 수신함 분류 + 답변 승인 플로우' 패키지를 제공해 월 구독형(팀/개인 플랜)으로 출시.

## 결정 (Decision)
**⏸️ 보류**
- 시장: 추천 — Outlook AI 이메일 자동화는 수요 시그널이 강하고(검색량 +180%, 트윗 공감 1.2K), 직접 경쟁자가 부재한 블루오션. Copilot 대비 1/3 가격으로 포지셔닝하면 1인 사업자 세그먼트를 선점할 수 있음.
- 기술: 추천 — 기술적 허들이 낮고(Graph API 표준, PoC 존재), OpenClaw 스킬 아키텍처 재활용으로 빠른 MVP 출시(3-5일)가 가능함.
- 비즈니스: 추천 — 낮은 API 비용($2/user)과 높은 마진(78%), SaaS 반복 수익 모델로 빠른 수익화 가능. $9 가격대는 Copilot($30) 대비 강한 가격 경쟁력.
- Critic: 보류 — 이메일 데이터 접근에 대한 보안 우려와 Microsoft의 가격 전략 변화 리스크가 있으나, 현재 블루오션 + PoC 존재라는 강점이 리스크를 상쇄. 베타 테스트로 PMF 검증 후 본격 투자 권장.

## 검토한 대안 (Options)
Outlook 대신 전체 이메일 클라이언트(IMAP/SMTP) 지원으로 확장하거나, Gmail 동시 지원으로 TAM 확대 검토

## 트레이드오프 (Consequences)
- 실행 시: X/트위터에 @BadBrainCode 사례 인용한 '내 톤으로 이메일 자동 답장' 시연 영상 공개 + 대기명단 오픈 → 인디해커스/ProductHunt 프리뷰 포스트 + 유튜브 '5분만에 Outlook AI 비서 만들기' 튜토리얼 → 베타 사용자 10명 온보딩 + 피드백 수집 + 유료 플랜 오픈
- 미실행 시: 기회 비용 발생

## 리스크 및 탐지 신호
| 리스크 | 내용 | 탐지 신호 |
|--------|------|-----------|
| 치명적 결함 | 이메일 데이터는 민감 정보의 집합체. 1인 사업자라도 고객 정보가 포함되어 있어, AI가 읽고 답장 초안을 쓴다는 것에 대한 심리적 저항이 예상보다 클 수 있음. | 초기 검증 실패 |
| 시장 리스크 | Microsoft의 Copilot 가격 전략 변화, Google Gemini의 Gmail 통합 확대에 따른 간접 경쟁 심화 | 수요 지표 하락 |
| 실행 리스크 | Outlook add-in 승인 프로세스가 느리고(2-4주), 조직 환경에서 사이드로딩 제한 가능성 | MVP 일정 초과 |

## 관련 Spec / Runbook
- [[Research/Ideas/2026-02-21-Outlook AI 초안 대행 에이전트 템플릿|아이디어 카드]]
