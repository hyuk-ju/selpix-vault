---
type: research
note_status: literature
confidence_level: medium
source_agent: claude-code
generated_via: manual
verified_by: none
sources:
  - https://www.augmentcode.com/blog/how-to-build-your-agent-11-prompting-techniques-for-better-ai-agents
  - https://cookbook.openai.com/examples/gpt-5/gpt-5-2_prompting_guide
  - https://developers.openai.com/cookbook/examples/gpt-5/codex_prompting_guide
  - https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/
  - https://www.statsig.com/perspectives/tool-calling-optimization
  - https://platform.openai.com/docs/guides/function-calling
  - https://docs.anythingllm.com/agent-not-using-tools
  - https://www.atlabs.ai/blog/gpt-5.2-prompting-guide-the-2026-playbook-for-developers-agents
  - https://medium.com/tech-ai-made-easy/why-did-my-ai-agent-ignore-half-my-instructions-fde3aea6e9f5
  - https://www.unite.ai/why-large-language-models-skip-instructions-and-how-to-address-the-issue/
  - https://arxiv.org/html/2507.21504v1
created: 2026-03-18
reviewed_at: 2026-03-18
---

# AI Agent Tool-Calling Optimization Research (2026-03)

OpenClaw 에이전트(GPT-5.3/5.4 Codex) 성능 개선을 위한 실전 팁 조사 결과.

---

## 1. "에이전트가 도구를 안 쓴다" 문제 — 원인과 해결

### 원인

- **도구 정의 과적재**: 도구가 많거나 설명이 길면 모델이 JSON 스키마를 정확히 매칭하지 못함 (AnythingLLM docs 확인)
- **복잡 지시 무시 경향**: LLM은 훈련 데이터에서 단순 지시를 더 많이 봤기 때문에 복잡한 멀티스텝 지시를 건너뜀 (Unite.AI 분석)
- **컨텍스트 윈도우 초과**: 토큰 한도 초과 시 뒷부분 지시가 무시됨
- **JSON 응답 포맷 강제 충돌**: `response_format: json`을 설정하면 tool call 응답 자체가 실패할 수 있음

### 해결책

| 기법 | 상세 | 출처 |
|------|------|------|
| **`strict: true` 설정** | 함수 스키마 준수를 best-effort에서 강제로 변경. 항상 켜둘 것 | OpenAI 공식 docs |
| **`tool_choice` 명시 강제** | 특정 도구 사용을 강제: `tool_choice={"type":"function","function":{"name":"xxx"}}` | OpenAI API |
| **도구 수 줄이기** | 30개 이상이면 Tool Search/라우팅 레이어 도입. 적으면 적을수록 정확도 상승 | Composio 가이드 |
| **스키마 단순화** | 파라미터를 줄이고, 필수/선택 명확히, 이름을 의미적으로 명확하게 | Augment Code |
| **에러 시 복구 경로 제공** | 잘못된 파라미터로 도구 호출 시 tool output으로 에러 설명 반환 → 모델이 자가 수정 | Augment Code (#1 SWE-bench) |
| **모델 업그레이드** | 작은/양자화 모델이 문제 원인일 수 있음 → 더 큰 모델 또는 고품질 양자화 사용 | AnythingLLM |

---

## 2. 시스템 프롬프트 엔지니어링 — 도구 사용 에이전트용

### CTCO 패턴 (GPT-5.2 가이드)

```
Context → Task → Constraints → Output format
```

- **Constraints에 부정 제약 필수**: "Do NOT ...", "NEVER ..."
- XML 스캐폴딩으로 구조화 (특히 에이전트 프롬프트)
- 스코프 제한: 모델이 할 수 있는 것과 하면 안 되는 것을 명시

### Augment Code의 11가지 기법 (SWE-bench #1)

1. **도구 출력에서 놀라움 제거**: 예상과 다른 결과가 나오면 왜 다른지 설명하는 메시지 포함
2. **동적 상태는 system prompt에 넣지 말 것**: 현재 시간 등은 user message로 전달 → 프롬프트 내부 일관성 유지
3. **잘못된 도구 호출에 에러 피드백**: 파라미터 검증 후 교정 기회 제공
4. **목적에 맞는 도구 제공**: `edit_file` + `clipboard` 도구 조합 등
5. **수동 테스트 시나리오 기반 평가**: 자동 eval보다 다양한 상황 테스트가 회귀 방지에 효과적
6. **"재정적 파멸" 등 결과 경고문이 실제로 효과 있음**: 정중한 요청이나 대문자 "소리 지르기"는 효과 없음
7. **프롬프트를 코드베이스처럼 버전 관리, 리뷰, 테스트**

### OpenAI Codex 공식 프롬프팅 가이드

- **도구가 존재하면 도구 사용 강제**: "If a tool exists for an action, prefer the tool over shell commands (e.g., `read_file` over `cat`)"
- **자율성과 지속성 섹션이 가장 중요**: 에이전트가 중간에 멈추지 않도록 "non-interactive mode" 프롬프팅
- **eval 실행 시 자율성 높이기**: 확인 요청 줄이기

### P.A.R.T. 프레임워크 (컨텍스트 엔지니어링)

- **P**rompt: 시스템/사용자 프롬프트
- **A**rchive: 대화 기록, 메모리
- **R**esources: 외부 문서, RAG
- **T**ools: 사용 가능한 도구 정의

네 가지 모두 최적화해야 에이전트 품질 향상.

---

## 3. GPT-5.x Codex 에이전트 최적화

### GPT-5.1 특성
- 지시 준수도 높음
- **장기 에이전트 태스크에서 조기 종료 경향** → 프롬프트로 교정 가능 ("Complete the entire task before stopping")
- `none` reasoning mode: 저지연 상호작용용

### GPT-5.2 특성
- 토큰 효율 개선 (중~복잡 태스크)
- 포맷 깔끔, 불필요한 장황함 감소
- 구조적 추론 + 도구 그라운딩 개선
- **Plan-then-Execute 패턴 권장**: planning block → response block (컨텍스트 압축 시 planning 토큰 폐기 가능)

### GPT-5 Prompt Optimizer
- OpenAI Playground에서 기존 프롬프트를 자동 개선/마이그레이션하는 도구 제공
- 기존 프롬프트 입력 → 최적화된 버전 출력

---

## 4. 도구 호출 최적화 — 성능과 비용

### Statsig 권장 패턴

1. **도구 호출 전 이유 한 줄 출력 강제**: "reason: ... → tool: ..." 형태로 추적성 확보, 불필요한 호출 제거
2. **도구 호출 후 관찰(observation) 한 줄 강제**: 환각 후속 호출 감소
3. **병렬 실행**: 독립 API 호출은 동시에 → 지연 = 가장 긴 단일 호출 시간
4. **도구 단순화**: 파라미터 적게, 네임스페이스 명확하게
5. **모니터링**: 도구별 정확도, 완료율, 지연 추적

### parallel_tool_calls 주의

- `gpt-4.1-nano`: 병렬 호출 시 같은 도구를 중복 호출하는 버그 → `parallel_tool_calls: false` 권장
- 대부분의 GPT-5.x 모델에서는 병렬 호출 정상 작동
- Responses API에서는 `function_call_output`에 `call_id` 매칭 필수

---

## 5. 에이전트 Eval 스코어 개선 전략

### 자동 개선 루프

- **Loop AI**: eval 실행 → 실패 분석 → 프롬프트 개선 자동화
- **Agent Optimizer**: 6개 최적화 알고리즘으로 eval 결과 분석 → 프롬프트/설정 개선 제안
- **CI/CD 통합**: eval 실패 시 머지 차단 — 회귀 방지

### 핵심 평가 지표

| 에이전트 유형 | 적합한 메트릭 |
|--------------|-------------|
| Generator (텍스트 생성) | 텍스트 품질 (coherence, fluency) |
| Autonomous (자율 실행) | 태스크 완료율, 도구 사용 정확도, 추론 품질 |
| Coding agent | SWE-bench, CORE-Bench, AppWorld |
| Web agent | WebArena, BrowserGym |

### 다중 메트릭 동시 추적 필수

새 프롬프트가 한 메트릭을 개선하면 다른 메트릭이 퇴보할 수 있음 → 버전 간 회귀 분석이 핵심.

---

## 6. 아키텍처 레벨 해결책

### Neurosymbolic 접근 (가드레일)

- LLM 추론 + 결정론적 심볼릭 룰 결합
- `BeforeToolCallEvent` 훅으로 도구 호출 전 검증 → 규칙 위반 시 취소
- AWS Strands Agents에서 이 패턴 구현

### Router-Based Flow

- 대형 모델이 라우팅/계획 → 소형 에이전트(smolagents)가 실행
- 비용과 지연 절감 + 도구 선택 품질 향상

---

## OpenClaw 적용 가능 액션 아이템

1. **`strict: true` 확인**: 모든 function calling에 strict mode 적용 여부 점검
2. **도구 설명 감사**: 각 도구의 description이 "계약서"처럼 명확한지 검토
3. **에러 복구 경로**: 잘못된 도구 호출 시 교정 메시지 반환하는지 확인
4. **시스템 프롬프트에 부정 제약 추가**: "NEVER ...", "Do NOT ..." 패턴
5. **Plan-then-Execute 패턴 도입**: 복잡한 태스크에 planning block 추가
6. **도구 호출 전 이유 출력 강제**: 추적성 + 불필요 호출 감소
7. **eval 파이프라인 구축**: 프롬프트 변경 시 회귀 감지
