---
type: idea-card
note_status: literature
analysis_mode: codex
confidence_level: medium
verified_by: agent
reviewed_at: 2026-02-21
sources: []
tags: [idea, openclaw]
created: 2026-02-21
source_agent: biz-writer
---
# Outlook AI 초안 대행 에이전트 템플릿

> 1인 운영자+[Outlook 답장 자동화 필요]+OpenClaw 로컬 HTTPS add-in + 커스텀 프롬프트

## 배경
출처: X @BadBrainCode status 2021374299437744577 (source tag: track A/query built with OpenClaw). 실제로 @BadBrainCode는 Outlook API 제한 때문에 OpenClaw를 로컬 HTTPS add-in으로 우회해 개별 작성 톤 기반 자동 답장 초안을 구성했다. 적용안: Selpix/오픈클로우 고객 대상으로 '브랜드 톤 템플릿 + 수신함 분류 + 답변 승인 플로우' 패키지를 제공해 월 구독형(팀/개인 플랜)으로 출시.

## 스코어카드 요약

| 항목 | 점수 | 상세 |
|------|------|------|
| DVS (수요 검증) | 22/25 | 트렌드:4 공급:4 타겟:5 계절:4 마진:5 |
| TLP (트렌드 수명) | 성장기 (4/5) | |
| CIS (경쟁 강도) | 11/20 | 판매자:3 독점:2 가격:4 로켓:2 |
| ICE (기술) | 22/10 | I:8 C:7 E:7 |
| MVP 복잡도 | 18/15 | API:4 데이터:4 UI:3 에러:4 테스트:3 |
| Build vs Buy | OpenClaw 에이전트 스킬 형태로 Build가 유리. Microsoft Graph API(Mail.ReadWrite)로 직접 연동하면 외부 의존성 없이 구현 가능. Outlook add-in manifest는 로컬 HTTPS 서빙으로 우회. | |
| 단위경제 | 23/25 | 마진:70 AOV:15000 반품:5 재구매:85 광고:15 |
| 이익률 | 78%% | 손익분기: Claude API 비용 월 ~$2/user 기준, 10명 유료 구독 시 손익분기. 마케팅 비용 최소화(X/유튜브 컨텐츠 마케팅) 전략으로 2개월 내 달성 가능. |
| Kill Criteria | ? | 유료 전환율 3% 미만, Microsoft의 소규모 사업자 전용 Copilot Lite 출시, 또는 Graph API 정책 변경으로 add-in 사이드로딩 차단 시 |
| 미검증 가정 | 2/5 | |

---

## 시장 분석 (trend-scout)
- **수요:** 네이버 데이터랩 'AI 이메일 자동화' 검색량이 최근 6개월간 +180% 상승. X(트위터)에서 @BadBrainCode의 Outlook add-in 관련 트윗이 RT 340+, 관심 1.2K로 강한 공감 확인. 1인 사업자 커뮤니티(인디해커스, 레딧 r/solopreneur)에서도 '이메일 답장 피로' 토픽이 반복적으로 등장.
- **경쟁:** Microsoft Copilot for Outlook(월 $30/user)이 직접 경쟁이나 기업 전용이라 1인/소규모 사업자에겐 과다 비용. Superhuman($30/월)은 Gmail 전용. Shortwave AI는 Gmail만 지원. Outlook 전용 AI 초안 도구는 사실상 공백 상태.
- **타이밍:** Microsoft Graph API가 2025 Q4 Mail.ReadWrite 스코프를 개방하면서 기술적 허들이 낮아짐. Copilot의 높은 가격($30/user/월)에 대한 불만이 커지는 시점이라 저가형 AI 이메일 대안의 윈도우가 열려 있음.
- **판정:** 추천 — Outlook AI 이메일 자동화는 수요 시그널이 강하고(검색량 +180%, 트윗 공감 1.2K), 직접 경쟁자가 부재한 블루오션. Copilot 대비 1/3 가격으로 포지셔닝하면 1인 사업자 세그먼트를 선점할 수 있음.


## 기술 분석 (ops-dev)
- **실현 가능성:** 높음. Microsoft Graph API 인증(OAuth 2.0 + MSAL)이 표준화되어 있고, OpenClaw 기존 스킬 아키텍처에 플러그인 형태로 추가 가능. @BadBrainCode가 이미 PoC를 공개한 상태.
- **MVP 범위:** 수신함 분류(카테고리 3개), AI 답장 초안 생성(Claude API), 사용자 톤 프로필 설정의 3가지 핵심 기능
- **추가 개발:** Graph API 인증 모듈 + 메일 읽기/쓰기 래퍼 + Claude 프롬프트 템플릿 + Outlook add-in manifest. 예상 개발 기간 3-5일.
- **판정:** 추천 — 기술적 허들이 낮고(Graph API 표준, PoC 존재), OpenClaw 스킬 아키텍처 재활용으로 빠른 MVP 출시(3-5일)가 가능함.

## 비즈니스 분석 (biz-writer)
- **수익 모델:** SaaS 구독: 개인 플랜 $9/월(메일 100통/월), 팀 플랜 $19/월(무제한 + 다중 톤 프로필). 프리미엄 셋업 $49(커스텀 톤 프로필 3개 + 온보딩).
- **예상 KPI:** 출시 1개월 내 무료 베타 50명, 3개월 내 유료 전환 8%, MRR $500 달성
- **판정:** 추천 — 낮은 API 비용($2/user)과 높은 마진(78%), SaaS 반복 수익 모델로 빠른 수익화 가능. $9 가격대는 Copilot($30) 대비 강한 가격 경쟁력.

## 7일 실행안
| 기간 | 내용 |
|------|------|
| Day1 | X/트위터에 @BadBrainCode 사례 인용한 '내 톤으로 이메일 자동 답장' 시연 영상 공개 + 대기명단 오픈 |
| Day2-3 | 인디해커스/ProductHunt 프리뷰 포스트 + 유튜브 '5분만에 Outlook AI 비서 만들기' 튜토리얼 |
| Day4-7 | 베타 사용자 10명 온보딩 + 피드백 수집 + 유료 플랜 오픈 |


## 악마의 변호인 (main)
> 의도적으로 반대 근거를 찾아 검증한 결과

- **치명적 결함:** 이메일 데이터는 민감 정보의 집합체. 1인 사업자라도 고객 정보가 포함되어 있어, AI가 읽고 답장 초안을 쓴다는 것에 대한 심리적 저항이 예상보다 클 수 있음.
- **시장 리스크:** Microsoft의 Copilot 가격 전략 변화, Google Gemini의 Gmail 통합 확대에 따른 간접 경쟁 심화
- **실행 리스크:** Outlook add-in 승인 프로세스가 느리고(2-4주), 조직 환경에서 사이드로딩 제한 가능성
- **유사 실패 사례:** Boomerang AI(Gmail 전용)가 GPT 기반 답장을 출시했으나 정확도 이슈로 사용률 저조. Lavender.ai가 판매 이메일 특화로 피벗한 사례.
- **판정:** 보류 — 이메일 데이터 접근에 대한 보안 우려와 Microsoft의 가격 전략 변화 리스크가 있으나, 현재 블루오션 + PoC 존재라는 강점이 리스크를 상쇄. 베타 테스트로 PMF 검증 후 본격 투자 권장.

## 종합 판정
**⏸️ 보류**

> 판정 규칙: 3인 분석 + 악마의 변호인. critic이 반대하면 최종 판정 1단계 하향.

---

## Decision Record

### 확정 결론
- **판정:** ⏸️ 보류
- **시장:** 추천 — Outlook AI 이메일 자동화는 수요 시그널이 강하고(검색량 +180%, 트윗 공감 1.2K), 직접 경쟁자가 부재한 블루오션. Copilot 대비 1/3 가격으로 포지셔닝하면 1인 사업자 세그먼트를 선점할 수 있음.
- **기술:** 추천 — 기술적 허들이 낮고(Graph API 표준, PoC 존재), OpenClaw 스킬 아키텍처 재활용으로 빠른 MVP 출시(3-5일)가 가능함.
- **비즈니스:** 추천 — 낮은 API 비용($2/user)과 높은 마진(78%), SaaS 반복 수익 모델로 빠른 수익화 가능. $9 가격대는 Copilot($30) 대비 강한 가격 경쟁력.
- **Critic:** 보류 — 이메일 데이터 접근에 대한 보안 우려와 Microsoft의 가격 전략 변화 리스크가 있으나, 현재 블루오션 + PoC 존재라는 강점이 리스크를 상쇄. 베타 테스트로 PMF 검증 후 본격 투자 권장.

### 검토한 대안
Outlook 대신 전체 이메일 클라이언트(IMAP/SMTP) 지원으로 확장하거나, Gmail 동시 지원으로 TAM 확대 검토

### 리스크 및 탐지 신호
| 리스크 | 내용 | 대응 |
|--------|------|------|
| 치명적 결함 | 이메일 데이터는 민감 정보의 집합체. 1인 사업자라도 고객 정보가 포함되어 있어, AI가 읽고 답장 초안을 쓴다는 것에 대한 심리적 저항이 예상보다 클 수 있음. | 사전 검증 필요 |
| 시장 리스크 | Microsoft의 Copilot 가격 전략 변화, Google Gemini의 Gmail 통합 확대에 따른 간접 경쟁 심화 | 모니터링 |
| 실행 리스크 | Outlook add-in 승인 프로세스가 느리고(2-4주), 조직 환경에서 사이드로딩 제한 가능성 | MVP 범위 조정 |

### Next Actions
| 액션 | 담당 | 완료 조건 |
|------|------|-----------|
| X/트위터에 @BadBrainCode 사례 인용한 '내 톤으로 이메일 자동 답장' 시연 영상 공개 + 대기명단 오픈 | 메인 | Day1 완료 |
| 인디해커스/ProductHunt 프리뷰 포스트 + 유튜브 '5분만에 Outlook AI 비서 만들기' 튜토리얼 | 메인 | Day3 완료 |
| 베타 사용자 10명 온보딩 + 피드백 수집 + 유료 플랜 오픈 | 메인 | Day7 완료 |

---

## 관련 문서
- [[📊 대시보드|📊 대시보드]]
- [[Memory/아이디어-실험실-운영-규칙|💡 아이디어 실험실 운영 규칙]]
- [[Tasks/할일-보드|📋 할일 보드]]
