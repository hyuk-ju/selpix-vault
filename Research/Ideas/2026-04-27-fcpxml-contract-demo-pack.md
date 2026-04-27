---
type: idea-card
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: idea-discussion
verified_by: agent
sources:
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/mv_brain/tests/test_fcp_bridge.py
  - https://developer.apple.com/documentation/professional-video-applications/fcpxml-reference
  - https://github.com/DareDev256/fcpxml-mcp-server
  - https://github.com/itsakeyfut/avio/issues/435
  - https://reddit.com/r/finalcutpro/comments/1rf77wy/i_built_a_small_utility_to_export_scene_cuts_as/
created: 2026-04-27
reviewed_at: 2026-04-27
---
# FCPXML 계약 테스트 + 데모 팩

## 한 줄 요약
MV BRAIN의 FCPXML export를 “말로만 되는 기능”이 아니라, 샘플 XML·검증 명령·README 데모·스크린샷/녹화 준비물까지 포함한 1일짜리 포트폴리오 증거로 만든다.

## 왜 지금인가
- MV BRAIN README는 이미 Final Cut Pro용 FCPXML export를 핵심 워크플로로 제시한다.
- repo 내부 테스트에는 `mv_brain.output.fcp_bridge`와 `sample.fcpxml` 기반 검증 의도가 보이지만, 현재 파일 구조 검색에서는 해당 output 모듈/샘플 경로가 바로 확인되지 않았다. 즉 “수정 가능한 작은 신뢰성 격차”가 있다.
- Apple은 FCPXML Reference를 공식 문서로 제공하므로, MV BRAIN의 차별점인 FCPXML-first 포지션을 문서/테스트 기준으로 고정하기 좋다.
- GitHub에는 `fcpxml-mcp-server` 같은 FCPXML 기반 AI/자동화 프로젝트가 존재하고, `avio` 이슈에는 “FCPXML export가 Final Cut Pro에서 에러 없이 열리는 통합 테스트” 필요가 명시되어 있다. FCPXML import 가능성은 시장/개발자 모두에게 검증 포인트다.
- r/finalcutpro에는 FCPXML helper, scene cuts export, FCPXML issue 관련 글이 있어 작은 유틸리티와 import 안정성에 대한 커뮤니티 관심이 확인된다.

## 선택 전 후보 검토
- 후보 A: 3-channel search benchmark 카드 — 좋지만 실제 영상/임베딩 품질 증거가 필요해 1일 proof가 느리다.
- 후보 B: FCPXML 계약 테스트 + 데모 팩 — 현재 README 포지션과 가장 직접 연결되고, fixture 기반으로 안전하게 검증 가능하다.
- 후보 C: UI settings/provider key UX 개선 — 포트폴리오 효과는 있으나 FCPXML-first 차별점보다 약하다.
- 후보 D: Hermes/Obsidian daily idea QA 자동화 — 운영 효율은 좋지만 MV BRAIN 시너지가 낮다.

## 내부 토론
### idea-scout
MV BRAIN은 “local-first editor assistant”를 주장한다. 하지만 외부인이 이해할 수 있는 첫 증거는 모델 성능보다 FCPXML이 실제 편집 타임라인으로 이어진다는 점이다. README의 FCPXML-first 메시지를 테스트 가능한 데모로 바꾸는 것이 가장 빠른 신뢰 확보다.

### builder
MVP 범위는 작게 잡는다.
1. `examples/` 또는 `docs/demo/`에 최소 FCPXML fixture와 expected summary 추가
2. CLI에서 `validate-fcpxml` 또는 동등 명령이 fixture를 통과하는지 smoke test 정리
3. README에 “sample 없이도 확인 가능한 FCPXML demo” 섹션 추가
4. 가능하면 CI/pytest에 XML well-formed + basic timeline contract test 추가

실제 Final Cut Pro 실행, 대용량 media ingest, Gemini/Qdrant 쓰기는 제외한다. 기존 테스트 의도(`test_fcp_bridge.py`)를 살리면 1~3일 안에 데모 가능한 범위다.

### market
타깃은 Final Cut Pro 사용자 중 자동화/AI 편집에 관심 있는 기술형 에디터와 개발자다. 수익 경로는 당장 SaaS가 아니라:
- GitHub/README 포트폴리오 증거
- FCPXML 템플릿/validator 작은 유틸리티
- “내 footage를 로컬에서 분석해 FCPXML rough cut로 넘기는 워크플로” 컨설팅/셋업 리드
으로 좁게 시작할 수 있다.

### critic
치명적 리스크는 “Final Cut Pro 없이 만든 XML이 실제 import에서 깨질 수 있다”는 점이다. 그래서 아이디어 이름을 “완전 import 보장”이 아니라 “계약 테스트 + 데모 팩”으로 제한해야 한다. macOS/FCP 실제 검증은 후속 액션으로 분리한다. 또 FCPXML 자동화는 경쟁 프로젝트가 있으므로, MV BRAIN은 XML 툴 자체보다 “클립 검색→러프컷→FCPXML” 연결성으로 차별화해야 한다.

### curator
승인. 넓은 AI 영상 SaaS가 아니라 MV BRAIN의 핵심 약속을 검증 가능한 작은 산출물로 바꾸는 작업이다. 외부 신호도 FCPXML 자동화/검증 수요를 뒷받침하며, 리스크는 scope 제한으로 관리 가능하다.

## 점수
| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 5 | 테스트, XML, CLI, 편집 워크플로가 바로 보이는 산출물 |
| revenue_fit | 4 | validator/template/셋업 서비스로 좁은 유료화 가능 |
| build_ease | 4 | fixture·문서·smoke test 중심, API/대용량 미디어 불필요 |
| proof_speed | 5 | README diff, 테스트 결과, 샘플 XML로 1~3일 내 증명 가능 |
| mv_brain_synergy | 5 | README의 FCPXML-first 핵심 포지션과 직접 연결 |
| risk_inverse | 4 | 실제 FCP import 검증 부재 리스크는 후속 단계로 격리 가능 |
| **합계** | **27/30** | approved |

## Verdict
`approved` — 총점 27/30, fatal flaw 없음. 단, “Final Cut Pro 실제 import 보장”은 후속 macOS 검증 전까지 주장하지 않는다.

## 다음 1개 액션
`/home/dev/mv-brain`에서 output/FCPXML 관련 실제 파일 구조를 정리하고, “샘플 FCPXML fixture → validate 명령 → README demo snippet”만 포함한 최소 PRD/작업 메모를 작성한다. 코드 변경은 별도 사용자 요청 전까지 하지 않는다.

## 관련 문서
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
