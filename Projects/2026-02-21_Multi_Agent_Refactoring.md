# 멀티 에이전트 아키텍처 리팩토링 기획 (v2 - 종합 검토 반영)

## 1. 배경 및 진단 (왜 고쳐야 하는가?)
사용자 피드백과 외부 리서치(NeurIPS 2025, Anthropic/OpenAI 가이드라인) 및 유튜브 인사이트를 종합하여 현재 OpenClaw 시스템의 안티패턴을 진단함.

1. **PM 에이전트의 부재 및 `main`의 병목**:
   - `main` 에이전트가 오케스트레이션(CEO)에 더해 인텔 토론 중재 및 아이디어 실험실까지 관여하여 과부하 발생.
   - `biz-writer`는 기획자(PM)가 아니라 단순 문서 작성/크론 실행기(daily-briefing, todo, dashboard)로 사용 중.
2. **과도한 에이전트 수 (Coordination Tax)**:
   - 9개의 에이전트 체제는 조정 비용(Coordination Tax)을 급증시킴 (MAST Taxonomy 논문 참조: 4개 이상부터 실패율 증가).
   - 최근 21시간 전에 추가된 `monitor`, `analyst`, `tester`의 독립적 효용성을 검증하여 합리적으로 축소해야 함.
3. **페르소나와 구조적 평가(Rubric)의 부재**:
   - "까칠함" 같은 어조(Tone) 부여만으로는 팩트 정확도나 성능이 개선되지 않음 (arXiv, "A Helpful Assistant" 연구).
   - `tester` 등 검증 에이전트에게 명확한 '구조화된 평가 루브릭(JSON)'이 없어 비효율적인 Maker-Checker 루프 발생.
4. **결정권의 파편화 (`jobs.json` 하드코딩)**:
   - 현재 가장 시급한 문제. `daily-briefing` 크론에만 800자 이상의 프롬프트가 `jobs.json`에 하드코딩되어 있어, `SOUL.md`의 정체성보다 절차적 크론 스크립트가 에이전트의 행동을 강하게 지배함.

## 2. 핵심 개편 방향 (Anthropic/OpenAI 패턴 적용)

### 원칙 1: 구조적 단순화 (Start Simple) - 에이전트 9개 → 7개 통폐합
- **`monitor` 삭제**: 단순 알람 역할이므로 `auditor`에 기능 흡수.
- **`analyst` 삭제 (또는 도구화)**: 독립 에이전트 대신, 수석 PM(`biz-writer`)이 도구(Tool)를 활용해 수행하는 방식으로 흡수.
- **`tester` 유지 및 고도화**: 격리된 컨텍스트와 읽기 전용 권한을 가진 QA 전담으로 필수 유지 (Maker-Checker 루프 완성).

### 원칙 2: `biz-writer` 를 "수석 PM(Product Manager)"으로 격상
- **정체성**: 아이디어 검증(daily-briefing 리서치 연계), 스펙(PRD) 정의, 엔지니어 위임을 총괄.
- **권한 위임**: `main`이 하던 '인텔 토론 루프'와 '아이디어 실험실 중재' 역할을 `biz-writer`에게 이관.
- **크론 이관**: PM 역할과 무관한 단순 유지보수성 작업(`todo-board-daily-sync`, `dashboard-weekly-sync`)은 `worker`나 `auditor`로 이관.

### 원칙 3: `tester` (QA)의 구조화된 평가 (Maker-Checker Loop)
- **정체성**: 스스로 코드를 수정하지 않고(반려만 수행), 명확한 "구조화된 평가 루브릭(JSON)"을 기반으로 검증하는 체크 에이전트.
- **평가 루브릭 예시**:
  - `code_review_rubric`: security, reliability, maintainability 등을 체크리스트화.
  - `idea_review_rubric`: market timing, feasibility, risk 등을 체크리스트화.
- **기대 효과**: 막연히 "빈틈을 찾아내라" 대신 "이 10개 체크리스트로 검증하고 pass/fail 및 지적사항(structured data)을 반환하라"로 재편. 최대 3~5회 반복 후 에스컬레이션(보고).

### 원칙 4: `jobs.json` 하드코딩 탈피 및 크로스 리뷰 확립 (우선 과제)
- `jobs.json`에 하드코딩된 복잡한 지시문(SUBBRAIN_POLICY 등)을 제거.
- 에이전트의 `SOUL.md` 내지는 별도의 프롬프트 지시서 파일로 분리.
- 유튜브 인사이트에서 강조한 **"구체적 보고서 작성 (발견/수정/이유)"** 지침과 **"AI 간 크로스 리뷰"** 룰을 `AGENTS.md` 루프 파일에 명시.

## 3. 에이전트 역할 (SOUL) 매트릭스 (행동 규칙 중심)

*단순 형용사(성격)보다 `Do & Don't`의 구체적 룰스펙(Behavior)이 핵심임.*

| 에이전트 | 직책 | 핵심 행동 규칙 (Do & Don't / 평가 루브릭) |
|---|---|---|
| **main** | CEO (Orchestrator) | Do: 라우팅, 사용자 승인 중재. / Don't: 실무 개입 금지, 인텔 토론/아이디어 토론 중재 금지. |
| **biz-writer** | 수석 PM (기획자) | Do: PRD 작성, 리서치 기반 스펙 정의, 인텔 토론/아이디어 토론 리드. / Don't: 직접 코딩 금지, 단순 동기화(`todo-board` 등) 금지. |
| **ops-dev** | CTO (엔지니어) | Do: 시스템 아키텍처 설계, 코드 구현 (수석 PM의 PRD 기반). / Don't: 비즈니스 기획 관여 금지. |
| **tester** | 무자비한 로직 QA | Do: 격리 상태에서 JSON 루브릭을 통한 구조적 검증(Maker-checker), 검토 리포트 작성. / Don't: 스스로 코드 직접 수정 금지. |
| **trend-scout**| 리서치 파트장 | Do: 데이터와 명확한 출처 기반의 시장 인사이트 수집 (`daily-briefing` 기여). / Don't: 근거 없는 추측성 정보 전달 금지. |
| **worker** | 자동화 로봇 (행동대장)| Do: 시킨 명령 최단시간 완수, `todo-board`/`dashboard` 등 단순/반복 동기화 전담. / Don't: 판단하거나 주관적 의견 내기 금지. |
| **auditor** | 보안/컴플라이언스 | Do: 시스템 상태, 룰 위반 감시, `monitor` 역할 흡수. / Don't: 유저 허락 없는 대규모 설정 변경 금지. |

## 4. 구체적 실행 액션 플랜 (Phase 1)

**[1. 에이전트 통폐합 및 역할 전환]**
   - 1a. `monitor`를 `auditor`에 흡수. `analyst`를 `biz-writer`의 데이터 분석 도구로 전환 (9 -> 7 체제).
   - 1b. `biz-writer` SOUL.md를 '수석 PM' 기능 및 행동 지침 중심으로 전면 재작성 (크론 이관 포함).
   - 1c. `tester` SOUL.md에 '구조화된 평가 루브릭(JSON)' 및 '반려 룰' 추가.

**[2. 핵심 부채 해소 (`jobs.json` 오버라이드)]**
   - `jobs.json` 내 하드코딩 프롬프트를 별도 시스템 프롬프트 파일이나 `.claude/skills`로 분리하여 `SOUL.md` 개편 효과 극대화. (단일 프롬프트 관리가 아닌 참조형 구조).

**[3. 실행 루프(Orchestration) 재정의]**
   - `main` SOUL.md에서 인텔토론/아이디어 중재 규칙을 `PM(biz-writer)`에게 위임하는 규칙 추가.
   - 유튜브/리서치 팁의 "크로스 리뷰" 및 "구체적인 완료/진단 보고서 의무 작성" 룰을 각 에이전트 워크플로우(`AGENTS.md`)에 적용.

**[4. 행동 룰/구문 최적화 적용]**
   - 남은 에이전트(4개: `ops-dev`, `worker`, `auditor`, `trend-scout`)의 SOUL.md를 위 매트릭스 행동 규칙(Do/Don't) 위주로 명문화.
