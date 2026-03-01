---
type: runbook
note_status: permanent
confidence_level: medium
source_agent: error-pattern-learner
generated_via: cron
created: 2026-03-01
reviewed_at: 2026-03-01
tags: [runbook, auto-generated, timeout_relay, error-pattern]
---

# TIMEOUT_RELAY 에러 대응 Runbook

> 이 Runbook은 error_pattern_learner.js가 자동 생성했습니다.

## 패턴 요약

- 카테고리: TIMEOUT_RELAY
- 총 발생 횟수: 81회
- 최초 발견: 2026-02-28T13:36:50.636Z
- 최근 발견: 2026-02-28T18:03:05.108Z
- 에스컬레이션: root_cause_needed

## 근본 증상

```
lane duration 147896ms exceeds delay threshold 120000ms
```

## 대응 액션 (효과율 순)

- ~~gateway_channel_separation (0%, 226회 적용)~~ (비효과적)

## 근본 수정 필요

- gateway_channel_separation

## 관련 인시던트

- inc_574aa835a242e433
