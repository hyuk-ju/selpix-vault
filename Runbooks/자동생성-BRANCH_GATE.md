---
type: runbook
note_status: permanent
confidence_level: medium
source_agent: error-pattern-learner
generated_via: cron
created: 2026-03-01
reviewed_at: 2026-03-01
tags: [runbook, auto-generated, branch_gate, error-pattern]
---

# BRANCH_GATE 에러 대응 Runbook

> 이 Runbook은 error_pattern_learner.js가 자동 생성했습니다.

## 패턴 요약

- 카테고리: BRANCH_GATE
- 총 발생 횟수: 35회
- 최초 발견: 2026-02-24T05:00:05.341Z
- 최근 발견: 2026-02-24T20:15:12.118Z
- 에스컬레이션: root_cause_needed

## 근본 증상

```
- 사유: uncommitted changes 경고로 strict 차단 (selpix {N}건, config {N}건)
```

```
- 사유: selpix 저장소 uncommitted changes {N}건으로 strict 차단
```

```
- 사유: config 저장소 uncommitted changes {N}건으로 strict 차단
```

## 대응 액션 (효과율 순)

- (기록된 액션 없음)

## 근본 수정 필요

- workspace_hygiene_guardrails

## 관련 인시던트

- inc_281e40c613c36fa2
- inc_f66bf43e814ce155
- inc_ebb30094ffaa938c
- inc_70e027207faf00bd
- inc_628c39293bcc68a1
- inc_4db4e00ef70df5ae
- inc_17daa7e0add759d8
- inc_c27e5319b63ddd37
