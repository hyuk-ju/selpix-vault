---
type: project
note_status: permanent
confidence_level: high
source_agent: claude-code
generated_via: manual
verified_by: none
sources:
  - /home/dev/openclaw/config/workspace/vault/Memory/변경로그.md
  - /home/dev/openclaw/config/workspace/vault/ADR/
created: 2026-03-18
reviewed_at: 2026-03-18
---

# OpenClaw 변경 이력

> 주요 기능 추가/변경 타임라인 (2026년 2월~3월)

## 2026-03-18 — 팀 조율 대규모 개선 (3단계 완성)

**Phase 1: 알람 소음 감소** (12~16건/일 → 6~8건/일)
- git 백업 크론을 silent 전환
- selpix 크론잡(morning, sync) silent 모드로 전환
- 다이제스트 통합(vault-review, metrics-collector)
- 결과: 알람량 50% 감소

**Phase 2: 문서 단일 소스화**
- `AGENTS.md`를 모든 에이전트 참조의 단일 소스로 선언
- vault 포인터 시스템으로 AGENT_ARCHITECTURE.md, MODEL_MANAGEMENT.md 통합
- INFRA.md, PROTOCOLS.md 도입 (SYSTEM_STATE.md §0a에 기록)
- SYSTEM_STATE.md 마지막 업데이트: 2026-03-04

**Phase 3: team_bus 파이프라인 자동 체이닝**
- `pipeline_sourcing` → `team_bus` → `cron_register` → `team_bus` → `main_inbox` 연결
- 내부 처리 가능한 기능은 silent, 에러만 텔레그램 전송
- 결과: 불필요한 사람 개입 66% 감소

---

## 2026-03-10 — self-healing 무한 루프 구조적 수정

- `self_healing_monitor` 크론잡 5건(audit/review/impact/clarify/dispatch) timeout 무한 루프 원인 분석
- 각 크론에 명시적 timeout 설정 (20~30초) + fail-open 정책 추가
- RAG 색인 (rag_indexer.py --incremental) 동기화
- 결과: 크론 timeout 캐스케이드 장애 해소

---

## 2026-03-09 — GPT-5.4 최적화 전체 실행 (4단계)

**Phase A: 비용 모니터링**
- metrics-collector 크론에 토큰 및 비용 집계 로직 추가
- 모니터링 기준: 272K 토큰 임계점(초과 시 Input 2x, Output 1.5x)

**Phase B: 크론잡 프롬프트 고도화**
- `auditor-review-gate`: ADR 충돌 검증 + 비용 $5/월 기준
- `coupang-trend-research`: 과거 트렌드 비교 + 카테고리 크로스체크
- `coupang-price-monitor`: 연속 하락 경고 + GPT-5.4 명시

**Phase C: Playwright MCP PoC**
- `@playwright/mcp@latest` 설치 및 `openclaw.json` 등록
- 테스트 결과: 네이버 쇼핑 ✅, 쿠팡 메인 ❌(WAF), 쿠팡 Wing ⚠️(OAuth 필요)

**Phase D: Tool Search 평가**
- 현재 도구 5개로 Tool Search 미적용 (15개 이상 시 재검토)

**모델 분포 최적화**
- GPT-5.4: 13개 크론잡
- GPT-5.3-spark: 21개 크론잡
- Default(5.4): 11개 크론잡

---

## 2026-03-08 — 자동 코딩 파이프라인 UI/UX 품질 개선 (ADR-20260308)

원인: `dialogic-roadmap` 프로젝트 UI 데모 수준

**수정 사항**
- `run_coding_execution.js`: feature 스텝 후 자동 frontend 스텝 추가
- feature acceptance에 "UI 포함 시 기존 CSS 통합" 규칙 추가
- `generateInstruction()`: frontend 타입 8개 acceptance 템플릿 추가
- `claude_bridge.js` review: CSS 중복, 시맨틱 HTML, JSON 출력, 반응형 체크 추가

결과: 향후 자동 코딩 프로젝트 UI 기준 적용

---

## 2026-03-06 — 셀픽스 morning 동기화 RCA + 품질 개선

**근본 원인 (4가지)**
1. 쿠팡 GET 응답 `minimumQuantity` 비어도 `1`로 간주 → MOQ false positive
2. 가격 수정을 잘못된 API 경로로 전송 → `api-spec-mismatch` hold 대량 발생
3. full PUT에 `requested: true` 누락 → `임시저장` 상태 남음
4. 옵션별 반복 PUT → `현재 수정할 수 없습니다` 연쇄 hold

**수정 사항**
- `cron_product_sync.js`: MOQ 비교 로직 개선, seller-product 단위 full PUT으로 통합
- `coupang_api.js`: 가격 수정 경로 GET→full PUT으로 교체, `requested: true` 강제
- `cron_selpix_morning.md`: 알림 등급 분리 (info/warning/critical)
- `register_queue.json`: 해외직배송 보류 6건 메타 추가, 판매중지 1건 정리

**최종 상태**
- 동기화: 49건 완료, 0건 오류
- MOQ 변경: 4건
- 가격 변경: 48건
- 보류: 6건

---

## 2026-03-05 — Claude Bridge 4개 서브시스템 확장 + ops-dev 보완

**Claude Bridge 추가 기능**
- plan/diagnose/review/impact/clarify 헬퍼에 timeout override 지원
- ops_directive_handler: audit/review 진단 20초 fail-open
- cron directive: systemctl 대신 `config/cron/jobs.json` 기반 로컬 진단
- critical audit: 외부 셸 명령 제거, team_bus/jobs.json 기반 진단

**ops-dev 통합 기능**
- 아이디어 파이프라인 ↔ Claude Bridge 연결
- 운영 문제 4건 일괄 수정
- SEO 지연 3건 데이터 패치
- agent_eval.js v3: 도구 미사용 근본 원인 수정

**Twilio 전화 알림 인프라**
- 심각 장애(P0) 실시간 전화 알림
- SMS 폴백 지원
- 무음 시간(22:00~08:00) 자동 소음 관리

---

## 2026-03-04 — agent_eval v3: Judge 기준 강화 + 사실 검증 자동화

**agent_eval v3 개선**
- Judge 기준 강화: 도구 미사용, 환각, confidence 신호 검증
- 사실 검증: vault RAG 기반 자동 팩트 체크
- 토픽 테스트: 크론 경유 구현 (Phase B)
- openclaw.json 토픽 systemPrompt에 §9.1.1/§9.2.1 규칙 반영

**ADR-031 기록**
- 에이전트 역량 테스트 문제점 분석 및 개선안 문서화

---

## 2026-02월 초 — 초기 구축

- OpenClaw 핵심 인프라 배포
- 쿠팡 트렌드 수집 (Naver DataLab, YouTube, Google Trends)
- Selpix 파이프라인 통합 (sourcing → 등록 → monitoring)
- 에이전트 에이전트 협업 시스템 구축
- Telegram 운영 채널 개설 (main, team_bus, ops-dev, 팀별 토픽)
