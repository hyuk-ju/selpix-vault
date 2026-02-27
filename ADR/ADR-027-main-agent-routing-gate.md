---
type: adr
note_status: permanent
confidence_level: high
source_agent: claude-code
generated_via: manual
verified_by: human
sources: []
created: 2026-02-27
reviewed_at: 2026-02-27
---
# ADR-027: Main Agent 위임 우회 방지 — Routing Gate

## 상태
**적용 완료** (2026-02-27)

## 문제

Main agent가 "유튜브 서치해줘" 같은 요청을 받으면, trend-scout에 `sessions_spawn`으로 위임해야 하는데 대신:

1. 로컬 파일(`sourcing_directive.json`, `keyword_history.json` 등)을 직접 읽고
2. `pipeline_sourcing.js` 등을 직접 실행하고
3. 결과를 "리서치했습니다"라고 보고

**근본 원인**:
- SOUL.md 위임 규칙이 300줄 문서의 중간(§9.2)에 묻혀 있어 LLM 주의도가 낮음
- `tools.allow: ["*"]`로 기술적으로 모든 도구 접근 가능 → LLM이 "내가 더 빠르게 하겠다"고 판단
- 구체적 위반 사례 없이 추상적 규칙만 있어 패턴 매칭 실패

## 결정

### 1. SOUL.md — §0 ROUTING GATE 최상단 배치

기존 §0(Reporting Protocol)을 §0.5로 밀고, **새로운 §0을 Identity(§1) 바로 앞에 배치**.

포함 내용:
- **키워드 매칭 → 강제 위임 테이블** (유튜브→trend-scout, 코드→ops-dev 등)
- **자가 검증 (Self-Check)**: "내가 파일을 열어서 직접 분석하려는 거 아닌가?" → YES면 중단+위임
- **라우팅 선언 필수 출력**: 모든 응답 시작에 `📍 직접실행/위임/대화` 1줄 선언
- **Anti-Pattern 3가지**: 구체적 위반 행위 명시 (로컬 파일 읽기 = 환각과 동급)
- **위반 시 결과**: 신뢰도 🔴 위험으로 간주 (기존 Confidence Signal과 연결)

### 2. SOUL.md §9.2 — Anti-Pattern 사례 추가

기존 위임 테이블 아래에 구체적 위반 패턴 2건:
- **패턴 1 "가짜 리서치"**: pipeline_sourcing.js 코드 읽기 + register_queue.json 조회로 리서치 흉내
- **패턴 2 "스킬 없이 대체"**: main에는 duckduckgo/youtube-watcher/bird 스킬이 없음. 로컬 파일 읽기 ≠ 리서치

### 3. AGENTS.md — 위임 자가검증 규칙

Delegation 섹션 아래에 3가지 자가 질문:
1. 외부 데이터 필요? → trend-scout/biz-writer 위임
2. 코드 수정 필요? → ops-dev 위임
3. 내 스킬만으로 완료 가능? → NO면 위임

+ main이 해도/안 되는 것 명시

## 변경하지 않은 것

- `openclaw.json`의 `tools.allow: ["*"]` 유지 — §9.1 스크립트 실행에 필요
- 스킬 할당 변경 없음 — 현재 구조가 올바름
- 기존 §9.1, §9.2 테이블 삭제 안 함 — 보강만

## 핵심 전략

| 기법 | 목적 |
|------|------|
| 위치 변경 (§0 최상단) | LLM이 문서 앞쪽 지시를 더 강하게 따름 |
| 라우팅 선언 강제 (📍 출력) | "생각을 말하게" 해서 무의식적 우회 방지 |
| Anti-Pattern 명시 | 구체적 위반 사례를 보여줘서 패턴 매칭 |
| 위반 = 🔴 위험 | 기존 Confidence Signal과 연결해서 페널티 부여 |
| 자가검증 질문 | 행동 전 체크리스트 강제 |

## 검증

- `prompt_hygiene_check.js`: FAIL 0, WARN 5 (기존 budget 초과 — 배포 차단 없음)
- 텔레그램 테스트 대기 중: "유튜브 트렌드 조사해줘" → `📍 위임 → trend-scout` 확인 예정

## 롤백

```bash
cd /home/dev/openclaw/config && git revert HEAD
```
