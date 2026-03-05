---
type: adr
status: accepted
created: 2026-03-05
source_agent: claude-code
confidence_level: high
tags: [architecture, ops-dev, claude, multi-model]
---
# ADR-034: Claude Bridge — ops-dev GPT-5.3 Codex 약점 보완

## 배경

ops-dev(GPT-5.3 Codex) 운영 이력 분석 결과 반복 실패 패턴 5가지 확인:

1. **"접수했습니다" 루프** — directive 수신 → 접수 응답만 → 실행 0건 (RG-01 6일 방치)
2. **1차 수정 항상 실패** — 쿠팡 속성 에러 2회, agent_eval 4회 반복
3. **부작용 체크 누락** — SOUL.md 통합 시 크론 6개 참조경로 미업데이트
4. **에러 피상적 해석** — "유효하지 않은 구매 옵션" → 단위 구조(basicUnit) 2주 미발견
5. **긴 컨텍스트 실패** — 멀티파일 지시 시 context window exceeded

GPT-5.3 Codex 알려진 단점: 모호한 지시 무비판 수용, 자발적 도구 탐색 안 함, 반복 세션 버그 재도입, 엣지케이스 무시.

## 결정

`claude_bridge.js` 구현 — ops-dev가 Claude Code CLI(`claude -p`)를 호출하여 5가지 약점을 보완.

### 모드 5가지

| 모드 | 트리거 | 모델 | 역할 |
|------|--------|------|------|
| `plan` | 3+ 파일 변경 | sonnet | 구현 계획 + 영향 범위 |
| `diagnose` | 에러 2회+ 반복 | opus | 근본 원인 분석 |
| `review` | 코드 작성 후 | sonnet | 품질 게이트 (부작용/엣지케이스) |
| `impact` | 설정/구조 변경 | sonnet | 참조 파일 전수 스캔 |
| `clarify` | 모호한 지시 | haiku | 해석 분기 정리 |

### 기술 결정

- **비용**: 구독 크레딧 사용 (API 키 불필요)
- **호출**: `claude -p --model {model} --output-format json --allowedTools "Read,Grep,Glob"`
- **안전장치**: `--max-budget-usd 0.50` 호출당 상한
- **통합**: team_bus directive handler 또는 ops-dev 스크립트에서 직접 호출
- **제약**: 고빈도 자동화 부적합 (구독 속도제한), 비정기 고품질 위임용

## 성공 기준

| 지표 | 현재 | 목표 |
|------|------|------|
| 1차 수정 성공률 | ~40% | 80%+ |
| directive "접수만" 비율 | ~60% | 20% |
| 부작용 누락 | 월 2-3건 | 0건 |
| 에러 근본원인 도달 | 2일~2주 | 1일 이내 |

## 참고

- [Claude Code Headless 모드](https://code.claude.com/docs/en/headless)
- [GPT-5.3 vs Claude Opus 비교](https://www.nxcode.io/resources/news/gpt-5-3-codex-vs-claude-opus-4-6-ai-coding-comparison-2026)
- vault 변경로그 2026-03-05 ops-dev 실패 사례 분석
