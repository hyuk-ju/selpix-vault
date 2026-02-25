---
type: project
note_status: permanent
confidence_level: high
source_agent: claude-code
generated_via: manual
verified_by: none
sources: []
created: 2026-02-25
reviewed_at: 2026-02-25
---

# OpenClaw 운영자 경험 개선 — 상세 구현 계획

> 6개 우선순위 항목의 세부 구현 계획. 1번부터 순차 진행.

---

## Priority 1: Alert 3-Tier + Grouping + Calm Mode

### 목표
- 34개 크론잡 알림을 3단계로 분류하여 알림 피로도 80% 감소
- 야간(23:00~08:00 KST) Calm Mode로 수면 방해 제거
- 일 3회 다이제스트로 INFO급 알림 일괄 전달

### 1-1. 알림 분류 체계 정의

```
CRITICAL (즉시 전송 — 어떤 시간이든):
  - 품절 감지 (도매꾹 품절 모니터링)
  - 등록/동기화 실패 (selpix-morning, selpix-evening, selpix-register-lunch)
  - 시스템 장애 (self-healing-monitor → 에스컬레이션)
  - 가격 급변 (coupang-price-monitor → 임계치 초과 시)

WARNING (조용히 기록, 다음 다이제스트에 포함):
  - 가격 소폭 변동 (coupang-price-monitor → 정상 범위)
  - 트렌드 변화 (coupang-daily-trend, coupang-trend-research)
  - 시스템 감사 이상 (system-audit-periodic, system-audit-deep)

INFO (다이제스트 전용 — 즉시 전송 안 함):
  - 일일 브리핑 완료 (daily-briefing → vault 저장만, TG는 다이제스트)
  - 건강 체크 OK (cron-health-check, vault-health-check)
  - 통계/동기 (seller-stats-daily, todo-board-daily-sync, dashboard-weekly-sync)
  - git 백업 완료 (vault-git-backup, config-git-backup)
```

### 1-2. 구현 파일 목록

| 파일 | 변경 내용 |
|------|----------|
| `workspace/data/alert_config.json` | **신규** — 크론잡별 알림 등급, Calm Mode 설정 |
| `workspace/scripts/digest_accumulator.js` | **신규** — 크론 결과를 다이제스트 파일에 축적 |
| `workspace/data/daily_digest.json` | **신규** — 다이제스트 축적 데이터 |
| `cron/jobs.json` | INFO급 잡의 delivery.mode를 조건부로 변경하는 방식 대신, 프롬프트에 다이제스트 규칙 삽입 |
| `workspace/prompts/cron_*.md` | 각 프롬프트에 알림 등급 + 다이제스트 저장 규칙 추가 |
| 다이제스트 발송 크론잡 | **신규** — 09:00/14:00/20:00 KST 다이제스트 요약 발송 |

### 1-3. alert_config.json 스키마

```json
{
  "version": 1,
  "calm_mode": {
    "enabled": true,
    "start_hour": 23,
    "end_hour": 8,
    "timezone": "Asia/Seoul",
    "allow_levels": ["critical"]
  },
  "digest_schedule": {
    "times_kst": ["09:00", "14:00", "20:00"],
    "topic": "-1003620634996:topic:29"
  },
  "jobs": {
    "selpix-morning": { "level": "critical", "escalate_on": ["error", "registration_fail"] },
    "selpix-evening": { "level": "critical", "escalate_on": ["error"] },
    "selpix-register-lunch": { "level": "critical", "escalate_on": ["error"] },
    "selpix-sourcing-evening": { "level": "critical", "escalate_on": ["error"] },
    "도매꾹 품절 모니터링": { "level": "critical", "escalate_on": ["stockout"] },
    "coupang-price-monitor": { "level": "warning", "escalate_on": ["price_change_gt_20pct"] },
    "self-healing-monitor": { "level": "critical", "escalate_on": ["failure"] },
    "system-audit-periodic": { "level": "warning" },
    "system-audit-deep": { "level": "warning" },
    "cron-health-check": { "level": "info" },
    "daily-briefing": { "level": "info" },
    "weekly-kpi-review": { "level": "info" },
    "seller-stats-daily": { "level": "info" },
    "coupang-daily-trend": { "level": "info" },
    "coupang-trend-research": { "level": "info" },
    "intel-discussion-loop": { "level": "info" },
    "todo-board-daily-sync": { "level": "info" },
    "dashboard-weekly-sync": { "level": "info" },
    "vault-health-check": { "level": "info" },
    "idea-lab-loop": { "level": "info" },
    "coding-execution-loop": { "level": "info" }
  }
}
```

### 1-4. 다이제스트 축적 스크립트 (`digest_accumulator.js`)

- 크론잡 프롬프트에서 실행 결과를 `daily_digest.json`에 append
- 형식: `{ timestamp, job_name, level, summary, details }`
- 다이제스트 발송 크론이 읽고 → 요약 → Telegram 전송 → 파일 초기화

### 1-5. 다이제스트 발송 크론잡 (신규 3개)

```
digest-morning:   09:00 KST → 전날 밤 + 아침 축적분 요약
digest-afternoon: 14:00 KST → 오전 축적분 요약
digest-evening:   20:00 KST → 오후 축적분 요약
```

발송 프롬프트: `daily_digest.json`을 읽고, level별 그룹핑 후 요약 메시지 작성, topic:29로 전송

### 1-6. 프롬프트 수정 (INFO급 잡)

기존 프롬프트 하단에 추가:
```
## 알림 등급: INFO
실행 결과를 Telegram에 직접 보내지 말고, 아래 방식으로 다이제스트에 축적:
1. 결과를 JSON으로 정리
2. /home/dev/openclaw/config/workspace/data/daily_digest.json 에 append
3. 형식: {"timestamp":"ISO","job":"job-name","level":"info","summary":"한줄요약"}
4. Telegram 전송은 다이제스트 크론이 일괄 처리함
```

### 1-7. Calm Mode 구현

- OpenClaw 에이전트의 delivery 로직은 gateway 레벨 → 직접 수정 불가
- **대안**: INFO/WARNING급 크론잡의 스케줄 자체를 08:00~23:00 범위로 제한
- CRITICAL급만 24시간 실행 유지
- 이미 대부분의 크론잡이 주간 시간대에 실행 중 → 추가 조정 최소화

---

## Priority 2: n8n Self-Hosting (시각적 모니터링)

### 구현 계획
1. Docker Compose로 n8n 배포 (포트 5678)
2. webhook으로 크론 실행 결과 수신
3. 대시보드 workflow 구성
4. **보류 사유**: Priority 1의 다이제스트 체계가 먼저 안정화되어야 함

---

## Priority 3: Confidence Signal + Escalation

### 구현 계획
1. SOUL.md 또는 에이전트 시스템 프롬프트에 신뢰도 규칙 추가
2. 모든 크론 프롬프트에 3단계 신뢰도 표시 의무화
3. 🔴 발생 시 실행 보류 + 에스컬레이션 메시지

### 수정 대상
- `config/openclaw.json` — 에이전트 systemPrompt에 규칙 추가
- `workspace/prompts/cron_*.md` — 각 프롬프트에 신뢰도 섹션

---

## Priority 4: Langfuse/AgentOps 연동

### 구현 계획
1. Phase 1: 크론 실행 메트릭을 JSON 파일로 수집 (jobs.json state 확장)
2. Phase 2: 주간 집계 스크립트 작성
3. Phase 3: Langfuse Docker 배포 + API 연동

---

## Priority 5: 도매꾹 Official API 전환

### 구현 계획
1. 도매꾹 OpenAPI 키 발급 (사용자 액션 필요)
2. `pipeline_sourcing.js`의 스크래핑 로직을 API 호출로 교체
3. 기존 데이터 형식 호환 유지

---

## Priority 6: Per-Agent KPI Dashboard

### 구현 계획
1. `weekly-kpi-review` 크론잡의 프롬프트 개선
2. jobs.json의 state 필드에서 메트릭 추출
3. 주간 성적표 포맷 정의 + Telegram 발송

---

## 관련 문서
- [[Research/OpenClaw-운영자경험-개선-레퍼런스]]
- [[📊 대시보드]]
- [[Memory/변경로그]]
