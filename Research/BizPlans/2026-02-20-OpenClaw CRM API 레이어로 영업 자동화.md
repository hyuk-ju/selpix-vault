---
type: idea-card
status: generated
tags: [idea, openclaw, bizplan]
created: 2026-02-20
confidence_level: medium
source_agent: biz-writer
verified_by: human
reviewed_at: 2026-03-01
---
# 사업계획서: OpenClaw CRM API 레이어로 영업 자동화

> 대상: 1인/소규모 사업자, 문제: 리드·딜 파이프라인을 사람이 수동으로 관리해 후속 누락, 도구: OpenClaw CRM(19개 핵심 엔드포인트/40+ 리소스) + 기존 OpenClaw 봇

## 1. 시장 분석

| 항목 | 내용 |
|------|------|
| TAM (전체 시장) | 글로벌 CRM 시장 $89.6B (2024), CAGR 13.9% |
| SAM (접근 가능) | 소규모/1인 기업 CRM 시장 약 $5.2B |
| SOM (획득 가능) | 한국 1인 기업 AI CRM 시장 약 $12M (초기 진입) |
| 연간 성장률 | 13.9% (글로벌), 한국 소기업 SaaS 20%+ |

### 주요 트렌드
1. AI 에이전트 기반 자동화 CRM 급성장 (Salesforce Einstein, HubSpot AI)
2. 노코드/로코드 CRM 도구 부상 (Notion, Airtable 기반)
3. 1인 기업/프리랜서 전용 경량 CRM 수요 증가

### 출처
- https://www.grandviewresearch.com/industry-analysis/crm-market
- https://www.fortunebusinessinsights.com/crm-market-103418
- https://www.statista.com/outlook/tmo/software/enterprise-software/crm-software/worldwide

## 2. 경쟁사 분석

| 서비스 | URL | 가격 | 강점 | 약점 | 점유율 |
|--------|-----|------|------|------|--------|
| HubSpot CRM | https://www.hubspot.com/products/crm | 무료 기본, Starter $20/월, Professional $890/월 | 무료 티어 강력, 마케팅 자동화 통합, 대규모 생태계 | 1인 기업에겐 과도한 기능, API 연동 복잡, 비싼 상위 플랜 | 약 35% |
| Pipedrive | https://www.pipedrive.com | Essential $14.90/월, Advanced $27.90/월 | 직관적 파이프라인 UI, 영업 특화, 가격 합리적 | 마케팅 기능 부족, AI 자동화 제한적, 커스텀 어려움 | 약 8% |
| Folk CRM | https://www.folk.app | 무료 기본, Standard $20/월 | 1인/소규모 팀 특화, 깔끔한 UI, 관계 중심 | 자동화 약함, API 제한적, 한국어 미지원 | 약 1% |
| Notion CRM 템플릿 | https://www.notion.so | 무료~$10/월 (Notion 구독) | 유연성 최고, 무료, 한국 사용자 많음 | CRM 전용 기능 없음, 자동화 수동, 알림 약함 | 비공식 (CRM 대용 사용자 다수) |

## 3. 타겟 고객

- **페르소나:** 월 매출 500만~5000만 1인 기업/프리랜서. 영업 리드는 있지만 후속 관리를 놓침. Notion/스프레드시트로 임시 관리 중.
- **핵심 문제:**
  1. 리드 후속 조치를 잊어서 매출 기회 누락
  2. 고객 히스토리가 머릿속에만 있어서 체계적 관리 불가
  3. 기존 CRM은 너무 무겁고 세팅에 시간이 많이 걸림
- **지불 의향:** 월 1.5만~3만원 (기존 도구 대비 시간 절약 가치 기준)

## 4. 제품 전략

- **핵심 가치:** 텔레그램/슬랙에서 대화하듯 영업 파이프라인을 관리하는 AI CRM 비서
- **차별화:** 별도 앱 설치 없이 텔레그램에서 바로 사용. AI가 후속 조치를 알아서 제안. 5분 만에 세팅 완료.
- **기술 스택:** Node.js + SQLite(경량 DB) + Telegram Bot API + OpenClaw 에이전트 연동
- **MVP 기능:**
  1. 연락처 CRUD (이름, 회사, 연락처, 메모)
  2. 딜 파이프라인 관리 (스테이지 이동, 금액 추적)
  3. 후속 조치 자동 리마인더 (D+1, D+3, D+7)
  4. 텔레그램 봇 인터페이스 (슬래시 커맨드)
  5. 일일/주간 영업 요약 리포트

## 5. 수익 모델

- **모델:** 프리미엄 구독 (Freemium → Paid)
- **단위 경제:** CAC 약 3만원 (콘텐츠 마케팅), LTV 약 24만원 (평균 12개월 유지), LTV/CAC = 8x
- **손익분기:** 유료 전환 50명 (월 약 100만원) 시 운영비 커버, 3개월 내 달성 목표

### 가격 구간
| 등급 | 가격 | 포함 기능 |
|------|------|-----------|
| Free | 0원 | 연락처 50개, 딜 10개, 기본 리마인더 |
| Pro | 19,900원/월 | 무제한 연락처/딜, AI 후속 제안, 주간 리포트 |
| Team | 49,900원/월 | 팀 5명, 공유 파이프라인, API 연동, 커스텀 필드 |

## 6. 실행 로드맵

### 1주차
- [ ] SQLite 스키마 설계 (contacts, deals, activities, reminders)
- [ ] Telegram Bot 기본 구조 + 슬래시 커맨드 등록
- [ ] 연락처 CRUD API 구현

### 2-4주차
- [ ] 딜 파이프라인 CRUD + 스테이지 관리
- [ ] 리마인더 시스템 (크론 기반)
- [ ] 일일 영업 요약 리포트
- [ ] 랜딩페이지 + 대기명단 오픈

### 2-3개월
- [ ] AI 후속 조치 제안 (GPT 연동)
- [ ] 주간 리포트 + 인사이트
- [ ] 유료 결제 시스템 연동
- [ ] 베타 테스터 피드백 반영

### 마일스톤
1. Week 1: MVP 데모 (연락처 + 봇 기본 동작)
2. Week 4: 클로즈 베타 시작 (20명)
3. Month 2: 퍼블릭 베타 + 대기명단 100명
4. Month 3: 유료 전환 시작

## 7. 리스크 & 대응

| 유형 | 설명 | 대응 | 확률 |
|------|------|------|------|
| 시장 리스크 | 1인 기업이 CRM 필요성을 느끼지 못할 수 있음 | CRM이 아닌 '영업 비서'로 포지셔닝, 구체적 사용 시나리오 마케팅 | high |
| 경쟁 리스크 | HubSpot 무료 티어, Notion 무료 대안이 충분할 수 있음 | 텔레그램 네이티브 + AI 자동화로 차별화, 세팅 시간 5분 강조 | medium |
| 기술 리스크 | 텔레그램 봇 UX 한계 (복잡한 데이터 입력 어려움) | 핵심 기능만 봇에서, 상세 입력은 웹 대시보드로 보완 | medium |

## 8. 코딩 스펙 (bkit PDCA)

- **프로젝트명:** openclaw-crm
- **레벨:** dynamic

### 구현 기능
1. 텔레그램 봇 기본 구조 (명령어 라우팅)
2. 연락처 CRUD (/contact add, list, view, edit, delete)
3. 딜 파이프라인 CRUD (/deal add, list, move, close)
4. 활동 로그 (/log 연락처명 내용)
5. 리마인더 시스템 (자동 D+1, D+3, D+7)
6. 일일 영업 요약 (/report today)
7. SQLite DB 관리 (마이그레이션, 백업)

### API 엔드포인트
| Method | Path | 설명 |
|--------|------|------|
| POST | /api/contacts | 연락처 생성 |
| GET | /api/contacts | 연락처 목록 (필터/검색) |
| GET | /api/contacts/:id | 연락처 상세 |
| PUT | /api/contacts/:id | 연락처 수정 |
| DELETE | /api/contacts/:id | 연락처 삭제 |
| POST | /api/deals | 딜 생성 |
| GET | /api/deals | 딜 목록 (파이프라인 뷰) |
| PUT | /api/deals/:id/stage | 딜 스테이지 이동 |
| POST | /api/activities | 활동 기록 |
| GET | /api/reminders/pending | 대기 중 리마인더 |
| GET | /api/reports/daily | 일일 영업 요약 |

### 데이터 모델
#### contacts
| 필드 | 타입 | 필수 |
|------|------|------|
| id | INTEGER PRIMARY KEY | Y |
| name | TEXT | Y |
| company | TEXT | N |
| phone | TEXT | N |
| email | TEXT | N |
| memo | TEXT | N |
| created_at | DATETIME | Y |
| updated_at | DATETIME | Y |

#### deals
| 필드 | 타입 | 필수 |
|------|------|------|
| id | INTEGER PRIMARY KEY | Y |
| contact_id | INTEGER FK | Y |
| title | TEXT | Y |
| amount | INTEGER | N |
| stage | TEXT | Y |
| expected_close | DATE | N |
| created_at | DATETIME | Y |
| updated_at | DATETIME | Y |

#### activities
| 필드 | 타입 | 필수 |
|------|------|------|
| id | INTEGER PRIMARY KEY | Y |
| contact_id | INTEGER FK | Y |
| deal_id | INTEGER FK | N |
| type | TEXT | Y |
| content | TEXT | Y |
| created_at | DATETIME | Y |

#### reminders
| 필드 | 타입 | 필수 |
|------|------|------|
| id | INTEGER PRIMARY KEY | Y |
| contact_id | INTEGER FK | Y |
| deal_id | INTEGER FK | N |
| remind_at | DATETIME | Y |
| message | TEXT | Y |
| status | TEXT | Y |
| created_at | DATETIME | Y |

---

## 관련 문서
- [[Research/Ideas/2026-02-20-OpenClaw CRM API 레이어로 영업 자동화|아이디어 카드]]
