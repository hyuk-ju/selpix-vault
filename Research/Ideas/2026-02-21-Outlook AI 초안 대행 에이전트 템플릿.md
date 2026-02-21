---
type: idea-card
status: completed
tags: [idea, openclaw]
created: 2026-02-21
---
# Outlook AI 초안 대행 에이전트 템플릿

> 1인 운영자+[Outlook 답장 자동화 필요]+OpenClaw 로컬 HTTPS add-in + 커스텀 프롬프트

## 배경
출처: X @BadBrainCode status 2021374299437744577 (source tag: track A/query built with OpenClaw). 실제로 @BadBrainCode는 Outlook API 제한 때문에 OpenClaw를 로컬 HTTPS add-in으로 우회해 개별 작성 톤 기반 자동 답장 초안을 구성했다. 적용안: Selpix/오픈클로우 고객 대상으로 '브랜드 톤 템플릿 + 수신함 분류 + 답변 승인 플로우' 패키지를 제공해 월 구독형(팀/개인 플랜)으로 출시.

## 스코어카드 요약

| 항목 | 점수 | 상세 |
|------|------|------|

| ICE (기술) | 22/10 | I:8 C:7 E:7 |
| MVP 복잡도 | 18/15 | API:4 데이터:4 UI:3 에러:4 테스트:3 |
| Build vs Buy | OpenClaw 에이전트 스킬 형태로 Build가 유리. Microsoft Graph API(Mail.ReadWrite)로 직접 연동하면 외부 의존성 없이 구현 가능. Outlook add-in manifest는 로컬 HTTPS 서빙으로 우회. | |

| Kill Criteria | ? | 유료 전환율 3% 미만, Microsoft의 소규모 사업자 전용 Copilot Lite 출시, 또는 Graph API 정책 변경으로 add-in 사이드로딩 차단 시 |
| 미검증 가정 | 2/5 | |

---

> ⚡️ **Fast Track**: 내부 시스템 업데이트 아이디어이므로 시장 및 비즈니스 분석(Trend, Biz) 단계는 로직에 의해 생략되었습니다.


## 기술 분석 (ops-dev)
- **실현 가능성:** 높음. Microsoft Graph API 인증(OAuth 2.0 + MSAL)이 표준화되어 있고, OpenClaw 기존 스킬 아키텍처에 플러그인 형태로 추가 가능. @BadBrainCode가 이미 PoC를 공개한 상태.
- **MVP 범위:** 수신함 분류(카테고리 3개), AI 답장 초안 생성(Claude API), 사용자 톤 프로필 설정의 3가지 핵심 기능
- **추가 개발:** Graph API 인증 모듈 + 메일 읽기/쓰기 래퍼 + Claude 프롬프트 템플릿 + Outlook add-in manifest. 예상 개발 기간 3-5일.
- **판정:** 추천 — 기술적 허들이 낮고(Graph API 표준, PoC 존재), OpenClaw 스킬 아키텍처 재활용으로 빠른 MVP 출시(3-5일)가 가능함.


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

- **기술:** 추천 — 기술적 허들이 낮고(Graph API 표준, PoC 존재), OpenClaw 스킬 아키텍처 재활용으로 빠른 MVP 출시(3-5일)가 가능함.

- **Critic:** 보류 — 이메일 데이터 접근에 대한 보안 우려와 Microsoft의 가격 전략 변화 리스크가 있으나, 현재 블루오션 + PoC 존재라는 강점이 리스크를 상쇄. 베타 테스트로 PMF 검증 후 본격 투자 권장.

### 검토한 대안
Outlook 대신 전체 이메일 클라이언트(IMAP/SMTP) 지원으로 확장하거나, Gmail 동시 지원으로 TAM 확대 검토

### 리스크 및 탐지 신호
| 리스크 | 내용 | 대응 |
|--------|------|------|
| 치명적 결함 | 이메일 데이터는 민감 정보의 집합체. 1인 사업자라도 고객 정보가 포함되어 있어, AI가 읽고 답장 초안을 쓴다는 것에 대한 심리적 저항이 예상보다 클 수 있음. | 사전 검증 필요 |
| 시장 리스크 | Microsoft의 Copilot 가격 전략 변화, Google Gemini의 Gmail 통합 확대에 따른 간접 경쟁 심화 | 모니터링 |
| 실행 리스크 | Outlook add-in 승인 프로세스가 느리고(2-4주), 조직 환경에서 사이드로딩 제한 가능성 | MVP 범위 조정 |


---

## 관련 문서
- [[📊 대시보드|📊 대시보드]]
- [[Memory/아이디어-실험실-운영-규칙|💡 아이디어 실험실 운영 규칙]]
- [[Tasks/할일-보드|📋 할일 보드]]
