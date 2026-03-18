---
type: project
note_status: permanent
confidence_level: high
source_agent: claude-code
generated_via: manual
verified_by: none
sources: []
created: 2026-03-18
reviewed_at: 2026-03-18
---

# OpenClaw 기능 총람

> **소규모 AI 에이전트 팀** — 이커머스 자동화 + 트렌드 리서치 + 비즈니스 인텔리전스 + 시스템 감시

OpenClaw는 8명의 전담 에이전트와 50개 이상의 자동화 크론잡으로 쿠팡 소싱·등록·모니터링과 트렌드 분석을 24/7 운영하는 **AI 에이전트 플랫폼**입니다.

---

## 🎯 핵심 미션

1. **자동화 파이프라인**: 도매꾹 → 쿠팡 상품 소싱, 등록, 가격 최적화, 품질 모니터링
2. **트렌드 인텔리전스**: 네이버 DataLab, YouTube, Twitter 수집 → AI 분석 → 자동 적용
3. **비즈니스 지능**: 일일 브리핑, 주간 KPI 리뷰, 아이디어 실험실, 시장 분석
4. **시스템 신뢰성**: 자동 감사, 자가치유, 알람 3-Tier, 중앙 모니터링

---

## 👥 팀 구성 (8명)

| 에이전트 | 역할 | 모델 | 핵심 능력 |
|----------|------|------|-----------|
| **main** 🕴️ | CEO/오케스트레이터 | GPT-5.3 Codex | 팀 지휘, 사용자 소통, 최종 승인 |
| **ops-dev** 🛠️ | CTO/아키텍트 | GPT-5.3 Codex | 시스템 설계, 코드 구현, 아키텍처 |
| **biz-writer** 📝 | 수석 PM/기획자 | GPT-5.3 Codex | 전략 수립, 브리핑, 아이디어 리드 |
| **trend-scout** 📡 | 리서처 | GPT-5.3 Codex | 시장 데이터 수집, 트렌드 분석 |
| **worker** 🔧 | 자동화 실행자 | GPT-5.3 Codex | 소싱, 등록, SEO 최적화 실행 |
| **tester** 🧪 | QA/검증자 | GPT-5.3 Codex | 교차 검증, 품질 루브릭 평가 |
| **auditor** 🔍 | 시스템 감사관 | GPT-5.3 Spark | 모니터링, 에러 감지, 자동 복구 |
| **ollama-worker** 🧠 | 경량 LLM | 로컬 (Ollama) | Windows PC 로컬 AI 요청 처리 |

→ 상세: `AGENTS.md`

---

## 🚀 핵심 기능 (5가지)

### 1️⃣ 쿠팡 자동화 파이프라인

**도매꾹 → 수집 → 분석 → 등록 → 모니터링 → 가격 최적화**

- **소싱**: 트렌드 키워드 기반 도매꾹 상품 검색 (매일 3회 배치)
- **세트 관리**: 최저가 공급처 자동 선택, 마진율 계산
- **SEO 최적화**: 쿠팡 노출 키워드 자동 생성 (로컬 LLM 활용)
- **상품 등록**: Coupang Open API 자동 송신 (매 30분)
- **품질 모니터링**: 재고 부족 감지 → 자동 판매 중지 → 복구
- **가격 모니터**: 쿠팡 경쟁사 가격 추적 → 제안가 자동 조정 (매일 10시)
- **리뷰 분석**: 상위 경쟁사 리뷰 → 불만점 추출 → 상품 개선 피드백

→ 상세: `AUTOMATION_DESIGN.md`, `SYSTEM_STATE.md §7` (호스트 크론)

---

### 2️⃣ 트렌드 리서치 + 인텔 루프

**네이버 DataLab · YouTube · Twitter → 분석 → 자동 적용**

**수집 소스**:
- **Naver DataLab**: 5개 쇼핑 카테고리 일일 추이 (매 09:00, 21:00 UTC)
- **YouTube**: 15개 채널 트랜스크립트 + 인사이트 (매일 10:30 KST)
- **Twitter/X**: 30개 watchlist 계정 → 필터링 → actionable items (매일 09:00)
- **Google Trends**: 키워드 검색량 추이 (불안정성 있으나 보정 적용)

**분석 프로세스**:
1. 트렌드 수집 → `intel_briefing.json` 통합
2. 3명 에이전트 토론 (trend-scout, biz-writer, tester 의견 수렴)
3. 합의된 항목만 자동 적용 (매일 11:00 KST)
   - 키워드 추가 (watchlist.json)
   - 검색 태그 추가 (trend_aggregator.py)
   - 제외 목록 (pipeline_sourcing.js)
   - 배수 조정 (3/3 표결 필수)

→ 상세: `SYSTEM_STATE.md §9` (Intel Discussion Loop)

---

### 3️⃣ 아이디어 실험실 (4단계 깊이 분석)

**아이디어 제안 → 타당성 검토 → 사업계획서 → 코딩 착수**

**4단계 분석 깊이**:
- **스캔 (scan)**: 기술 가능성만 검토
- **검증 (validate)**: 시장 + 기술 + 비판적 검토 (3단계)
- **심층 (deep)**: 시장 + 기술 + 비즈니스 + 비판 (4단계 완전 분석)
- **자율 (auto)**: 자동 분석 → 반대 시 패스, 추천 시 사업계획서, 보류 시 큐 진행

**자동 처리**:
- discussing 아이디어 감지 → depth별 분석 → finalize → 버튼 제시
- 사업계획서 pending 감지 → 웹서치 + AI 생성 → finalize
- 자율 모드(autonomy=auto)는 상반된 의견 없으면 자동 진행

**결과 추적**:
- 프로젝트별 디렉토리 생성 (`projects/{name}/`)
- 분석 단계별 JSON 기록 (`idea_{ideaId}_*.json`)
- vault에 최종 사업계획서 저장

→ 상세: `SYSTEM_STATE.md §10`, `IDEA_LAB_PROTOCOL.md`

---

### 4️⃣ 일일 운영 (3가지 다이제스트 + 할일 관리)

**매일 09:00 / 14:00 / 20:00 KST 다이제스트 발송**

**콘텐츠**:
1. **오전 브리핑** (09:00 KST)
   - 일일 트렌드 요약 (Twitter 핵심 5개 + YouTube 인사이트 3개)
   - 오늘의 쿠팡 가격 변화
   - 어제 등록 완료 상품 현황

2. **오후 다이제스트** (14:00 KST)
   - 실시간 알람 (warning/critical 에스컬레이션)
   - 매출 추적 (쿠팡 일일 판매량)
   - 아이디어 진행 상황

3. **저녁 다이제스트** (20:00 KST)
   - 오늘의 주요 이슈 정리
   - 내일 일정 안내
   - 자동화 크론 상태 요약

**할일 관리**:
- 우선순위 + 마감일 지정 가능
- Google Calendar 자동 동기화 (마감일 있는 항목 → 종일 이벤트)
- 텔레그램 할일 보드 (자동 업데이트)
- 마감 리마인더 (매일 09:14, 18:14 KST)

→ 상세: `SYSTEM_STATE.md §10` (Dashboard & Tasks)

---

### 5️⃣ 시스템 감시 + 자동 복구

**6개 서브시스템 × 38개 체크 → 자동 복구 또는 에스컬레이션**

**감시 대상 (매 4시간 + 매일 심층 감사)**:
| 서브시스템 | 체크 수 | 자동 복구 |
|-----------|---------|----------|
| 쿠팡 파이프라인 | 12 | 에러 리셋, seoTimedOut, .bak 복구 |
| 크론잡 | 7 | 보고 전용 |
| 아이디어 실험실 | 6 | 강제완료, 최신만 유지 |
| 코딩 실행 | 4 | 보고 전용 |
| 인텔 루프 | 4 | .bak 정리 |
| 인프라 | 5 | 로그 정리, vault 커밋 |

**자동 에스컬레이션**:
- 같은 크론잡 연속 에러 ≥2회 → info → warning
- 연속 에러 ≥4회 → critical (야간에도 즉시 전송)
- Calm Mode (23:00~08:00 KST)에서 critical은 우회하여 전송

**자가치유 (Self-Healing)**:
- 매 20분 크론 에러 자동 재실행
- 세션 캐시 자동 회전
- vault git 자동 커밋 + 푸시

→ 상세: `SYSTEM_STATE.md §11`, `SYSTEM_STATE.md §14` (Alert 3-Tier)

---

## ⚙️ 활성 자동화 (50개 크론잡)

### 📅 카테고리별 크론

**Main 에이전트 (15개)**:
- `vault-auto-sync` — vault 자동 동기화 (매 30분)
- `memory-graph-index` — 메모리 그래프 인덱싱 (heartbeat)
- `idea-lab-loop` — 아이디어 4단계 토론 + 처리 (매 3시간)
- `team-dispatch` — team_bus 메시지 라우팅 (매 20분)
- `inbox-batch-handler` — inbox 배치 처리 (매 30분)
- `intel-discussion-loop` — 인텔 토론 + 자동 적용 (매일 11:00 KST)
- `coupang-price-monitor` — 쿠팡 가격 추적 (매일 10:00 KST)
- `weekly-cost-report` — 주간 비용 리포트 (월요일 00:30 UTC)
- git backup: `vault-git-backup`, `config-git-backup` (매일 03:00/03:30 KST)

**Biz-writer 에이전트 (8개)**:
- `daily-briefing` — 일일 브리핑 + 아이디어 추출 (매일 09:02 KST)
- `todo-board-daily-sync` — 할일 보드 + 대시보드 동기화 (매일 09:08 KST)
- `task-reminder-check` — 할일 마감 리마인더 (매일 09:14, 18:14 KST)
- `digest-morning/afternoon/evening` — 다이제스트 발송 (09:00 / 14:00 / 20:00 KST)
- `weekly-kpi-review` — 주간 KPI 리뷰 (월요일 09:00 KST)

**Trend-scout 에이전트 (3개)**:
- `intel-collect-youtube` — YouTube 인사이트 수집 (매일 10:30 KST)
- `coupang-daily-trend` — 쿠팡 일일 트렌드 (매일 20:00 KST)
- `coupang-trend-research` — 주간 트렌드 조사 (월요일 10:00 KST)

**Ops-dev 에이전트 (3개)**:
- `ops-directive-handler` — team_bus 디렉티브 수신 (매 20분)
- `cron-health-check` — 크론 상태 점검 (매일 06:00 KST)
- `도매꾹 품절 모니터링` — 품절 감지 (매 6시간)

**Worker 에이전트 (4개)**:
- `selpix-morning/evening` — 오전/저녁 소싱 파이프라인 (07:30 / 20:03 KST)
- `selpix-register-lunch` — 점심 등록 배치 (13:00 KST)
- `selpix-sourcing-evening` — 저녁 소싱 (21:00 KST)

**Auditor 에이전트 (6개)**:
- `system-audit-periodic` — 주기적 감사 (매 4시간)
- `system-audit-deep` — 심층 감사 + vault 기록 (매일 05:00 KST)
- `vault-health-check` — vault frontmatter 검증 (월 1일 06:00 KST)
- `self-healing-*` (3개) — 자가치유 + 세션 캐시 회전 + 워치독

**인프라 + 기타 (8개)**:
- `windows-llm-request-handler` — Windows LLM 요청 처리 (매 20분)
- `metrics-collector-hourly` — 시스템 메트릭 수집 (매시)
- `vault-rag-incremental-index` — vault RAG 인덱스 갱신 (매 6시간)
- `advanced-automation-preflight` — 자동화 사전 점검 (매일 08:15 KST)
- `coding-execution-loop` — 자동 코딩 스텝 실행 (매 2시간)
- 기타: `claude-code-tips-collector`, `instinct-extraction-daily`, `kstartup-daily-monitor`

**팀 inbox 핸들러 (5개)**:
- `main-inbox-handler`, `biz-inbox-handler`, `worker-inbox-handler`, `tester-inbox-handler`, `trend-inbox-handler`
- 모두 team_bus 메시지 수신 처리 (매 15분)

→ 전체 목록: `config/cron/jobs.json`, `SYSTEM_STATE.md §7`

---

## 🔗 외부 연동

| 서비스 | 용도 | 상태 |
|--------|------|------|
| **Coupang Open API** | 상품 등록, 가격 변경, 재고 조회 | ✅ 정상 |
| **Naver DataLab API** | 카테고리별 쇼핑 트렌드 수집 | ✅ 정상 |
| **YouTube (yt-dlp)** | 채널 트랜스크립트 수집 | ✅ 정상 |
| **Twitter/X (bird)** | 트윗 수집 (DDG 폴백) | ⚠️ 폴백 중 |
| **Google Trends** | 검색량 추이 | ⚠️ 불안정 |
| **Telegram Bot API** | 알람, 다이제스트, 파일 전송 | ✅ 정상 |
| **Google Calendar API** | 할일 마감일 동기화 | ✅ 정상 |
| **Resend (Email)** | 이메일 발송 | ✅ 정상 |
| **Vault/ChromaDB** | 의사결정 기록 + RAG 검색 | ✅ 정상 |
| **Ollama (로컬)** | Windows PC 로컬 LLM 요청 | ✅ 선택사항 |

---

## 🏗️ 기술 스택

| 계층 | 기술 |
|------|------|
| **LLM 모델** | OpenAI GPT-5.3 Codex (main agent) / GPT-5.3 Spark (auditor) |
| **런타임** | Node.js (OpenClaw systemd service) |
| **스크립트** | Python 3.11 (trend_aggregator.py) |
| **브라우저** | Puppeteer (headless) + Browser Relay (Windows PC) |
| **데이터베이스** | JSON 파일 (config) + ChromaDB (vault RAG) |
| **인프라** | 3대 장비: 서버(Ubuntu) + Windows PC + MacBook |
| **자동화** | OpenClaw 크론 + 호스트 crontab |
| **통신** | team_bus (비동기 메시지 큐) + sessions_spawn (동기 위임) |

---

## 📊 최근 주요 변경 (2026-03)

### Phase 1: 알람 소음 50% 감소
- 3-Tier 알림 시스템 도입 (info/warning/critical)
- Calm Mode 적용 (야간 조용함)
- 자동 에스컬레이션 임계값 튜닝

### Phase 2: 문서 단일 소스화
- SYSTEM_STATE.md 추가 (환각 방지)
- INFRA.md, PROTOCOLS.md 분리
- AGENTS.md 역할/권한 표준화

### Phase 3: team_bus 파이프라인 자동 체이닝
- 에이전트 간 비동기 메시지 큐 구현
- ollama-worker를 통한 공유 LLM 서비스 (Windows PC)
- 자동 에스컬레이션 (auditor → main)

---

## 🎓 참고 문서

| 문서 | 내용 |
|------|------|
| `AGENTS.md` | 에이전트 역할, 권한, 도구, 통신 그래프 |
| `SOUL.md` | main 에이전트 행동 규칙 + 라우팅 가이드 |
| `SYSTEM_STATE.md` | 현황, 크론 목록, 환경변수, 상태 점검 |
| `AUTOMATION_DESIGN.md` | 쿠팡 파이프라인 5단계 설계 |
| `INFRA.md` | 3대 장비, Browser Relay, Ollama, 보안 |
| `PROTOCOLS.md` | Confidence Signal, Vault Search, 파일 전송 |
| `IDEA_LAB_PROTOCOL.md` | 아이디어 실험실 4단계 분석 프로토콜 |
| `SKILLS_GUIDE.md` | 설치된 스킬 목록 + 사용법 |

---

## ⚡ 빠른 시작

### 아이디어 제안
```
💡 사용자: "AI 요약 도구 만들어 보면 어떨까?"
📍 main → biz-writer (세션 생성)
→ 4단계 분석 → 타당성 평가 → 버튼 제시
→ 승인 시 사업계획서 자동 생성 + 코딩 팀 배정
```

### 트렌드 조사
```
🔍 사용자: "요즘 핫한 제품이 뭐야?"
📍 main → trend-scout (세션 생성)
→ Twitter, YouTube, 네이버 DataLab 수집
→ 분석 리포트 + 추천 상품 3개 제시
```

### 상품 소싱
```
🛒 자동 매일 자동 실행:
07:30 KST - worker가 도매꾹 검색 → 상품 스펙 추출
10:00 KST - 쿠팡 경쟁사 가격 추적
13:00 KST - 등록 대기 상품 일괄 등록
20:00 KST - 저녁 소싱 배치
→ 모든 과정 자동화, 에러 발생 시 자동 복구
```

### 시스템 상태 확인
```bash
# vault RAG로 최근 변경 검색
node /home/dev/openclaw/config/workspace/scripts/lib/vault_search.js "크론잡 추가" 5

# 크론 상태 조회
jq '.jobs[] | select(.enabled==true) | {name, lastStatus, lastDurationMs}' cron/jobs.json

# 최신 감사 결과
cat workspace-auditor/data/audit_findings.json | jq '.findings[] | {code, severity, finding}'
```

---

**업데이트**: 2026-03-18 · **문서 버전**: 1.0 · **에이전트**: claude-code
