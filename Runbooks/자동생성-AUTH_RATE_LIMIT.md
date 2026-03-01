---
type: runbook
note_status: permanent
confidence_level: medium
source_agent: error-pattern-learner
generated_via: cron
created: 2026-03-01
reviewed_at: 2026-03-01
tags: [runbook, auto-generated, auth_rate_limit, error-pattern]
---

# AUTH_RATE_LIMIT 에러 대응 Runbook

> 이 Runbook은 error_pattern_learner.js가 자동 생성했습니다.

## 패턴 요약

- 카테고리: AUTH_RATE_LIMIT
- 총 발생 횟수: 11회
- 최초 발견: 2026-02-24T17:43:53.541Z
- 최근 발견: 2026-02-24T20:38:29.223Z
- 에스컬레이션: root_cause_needed

## 근본 증상

```
⚠️ incident 감지(AUTH_RATE_LIMIT): Error: All models failed ({N}): openai-codex/gpt-{N}.{N}-codex-spark: No available auth profile for openai-codex (all in cooldown or unavailable). (rate_limit) | openai-codex/gpt-{N}.{N}-codex: No available auth profile for openai-codex (all in cooldown or unavailable). (rate_limit) (incident=inc_e0bdb4744639b848)
```

## 대응 액션 (효과율 순)

- ~~provider_profile_scaling (0%, 34회 적용)~~ (비효과적)

## 관련 인시던트

- inc_e0bdb4744639b848
