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
  - /home/dev/mv-brain/mv_brain/integrations/auto_editor.py
  - /home/dev/mv-brain/mv_brain/tests/test_cli.py
  - https://reddit.com/r/finalcutpro/comments/1slyaqh/papercut_pro_trying_another_textbased_approach_to/
  - https://github.com/jhowtkd/audio-highlights/pull/470
  - https://github.com/WyattBlue/auto-editor/issues/1063
created: 2026-04-29
reviewed_at: 2026-04-29
---
# Auto-cut 후보 사이드카 → 검색 가능한 클립 메모리 브리지

## 한 줄 요약
MV BRAIN의 `auto-editor` 후보 구간 JSON sidecar를 `SummaryCard`/검색 fixture로 변환해, 실제 AI·Qdrant·대용량 미디어 없이도 “자동 컷 후보 → 편집자가 검색/선택하는 클립 메모리” 흐름을 보여주는 1~3일짜리 데모 브리지를 만든다.

## 왜 지금인가
- MV BRAIN README는 “source-first clip triage”, 자연어 클립 검색, FCPXML rough-cut export를 핵심 워크플로로 제시한다.
- repo에는 이미 `mv_brain/integrations/auto_editor.py`가 있고, `write_candidate_sidecar()`가 FCPXML에서 `candidate_regions` JSON을 만든다. `test_cli.py`도 `auto-cut` CLI가 sidecar를 생성한다고 기대한다.
- 오늘 안전 점검에서 `python3 -m compileall -q mv_brain`은 통과했지만, `pytest --collect-only`는 `mv_brain.output` 누락과 선택 의존성(`qdrant_client`, `cv2`) 부재로 실패했다. 즉 “전체 AI 파이프라인”보다 더 가벼운, 의존성 적은 demo seam이 필요하다.
- 외부 신호: r/finalcutpro의 Papercut Pro 글은 interview/documentary 편집에서 텍스트 기반 paper cut 접근을 시도한다고 설명한다. 즉 타임라인/클립 후보를 사람이 읽고 선택하는 텍스트 레이어 수요가 있다.
- 외부 신호: GitHub `audio-highlights` PR은 NLE export(EDL/FCPXML) 지원을 제안하고, `auto-editor` 이슈에는 DaVinci-exported FCPXML import 이상 동작이 보고되어 있다. 자동 컷 후보와 NLE export 사이의 검증 가능한 중간 메타데이터가 중요하다는 신호다.

## 선택 전 후보 검토
- 후보 A: `mv_brain.output` 복구 스프린트 — 중요하지만 2026-04-27 FCPXML 계약 테스트 카드와 강하게 겹친다.
- 후보 B: No-media 검색 fixture 구현 — 2026-04-28 카드와 중복된다.
- 후보 C: Auto-cut 후보 sidecar를 검색 가능한 클립 메모리로 바꾸는 브리지 — 기존 repo 기능(`auto_editor.py`)을 살리면서 앞선 두 카드 사이를 잇는 새 seam이다.
- 후보 D: UI provider settings 개선 — 포트폴리오 설명력은 낮고 수익 wedge가 약하다.

## 내부 토론
### idea-scout
지금 MV BRAIN이 보여줘야 할 것은 “AI가 마법처럼 최종 MV를 만든다”가 아니라, 자동 후보 생성과 편집자 판단 사이에 검색 가능한 메모리 레이어가 있다는 점이다. `auto-editor` 후보 구간은 이미 repo에 있고, 이를 SummaryCard/fixture로 변환하면 source-first 포지션이 더 구체화된다.

### builder
MVP 범위는 작다.
1. `examples/auto-cut-sidecar/`에 실제 영상 없이도 읽을 수 있는 `sample.candidates.json` fixture를 둔다.
2. sidecar의 `candidate_regions`를 `Clip` 또는 `SummaryCard` 형태로 변환하는 순수 함수/문서 스펙을 정의한다.
3. “자동 컷 후보 5개 → 검색/라벨/등급 후보 → roughcut 입력” 예시 출력을 README 또는 demo note에 붙인다.

실제 `auto-editor` 실행, FCP 실행, Qdrant 쓰기, Gemini 호출은 제외한다. 현재 repo의 `auto_editor.py`, `SummaryCard`, `test_cli.py`를 활용하면 1~3일 내 fixture와 테스트 중심 산출물이 가능하다.

### market
타깃은 기술형 영상 편집자와 local-first AI 편집 도구에 관심 있는 개발자다. 수익 경로는 좁게 잡는다.
- “자동 컷 후보 + 검색 가능한 클립 로그” 템플릿
- 로컬 footage triage 설치/커스터마이징 서비스
- Final Cut Pro/auto-editor 기반 rough-cut 워크플로 컨설팅 리드

이 아이디어는 넓은 SaaS가 아니라, README/기술 블로그에서 바로 보여줄 수 있는 개발자 포트폴리오 증거다.

### critic
가장 큰 실패 이유는 자동 컷 후보가 “편집적으로 좋은 컷”이 아니라 단순 음성/모션 기반 구간일 수 있다는 점이다. 따라서 이 브리지는 최종 편집 자동화가 아니라 “후보 생성 → 사람이 검색/선별”이라는 제한된 역할로 써야 한다. 또 `pytest --collect-only`가 현재 실패하므로, 이 작업은 output 모듈 복구와 섞지 말고 sidecar fixture/순수 변환 스펙부터 시작해야 한다.

### curator
승인. 4/27 FCPXML 카드와 4/28 검색 카드의 중복이 아니라, 둘을 연결하는 작은 bridge 아이디어다. repo에 이미 auto-editor sidecar 코드가 있고, 외부에서도 텍스트 기반 paper cut/NLE export 관심이 확인된다. API 비용과 실제 미디어 처리 없이 증거를 만들 수 있다.

## 점수
| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 5 | 자동 컷 후보, JSON sidecar, 검색 메모리, README 예시가 기술적으로 설명 가능 |
| revenue_fit | 4 | local triage 템플릿/셋업/컨설팅으로 좁은 유료화 가능 |
| build_ease | 4 | 기존 `auto_editor.py`와 dataclass 모델 재사용, fixture 중심으로 시작 가능 |
| proof_speed | 4 | 실제 미디어 없이 sample sidecar와 예상 출력으로 1~3일 내 proof 가능 |
| mv_brain_synergy | 5 | source-first triage → search → roughcut 흐름을 직접 강화 |
| risk_inverse | 4 | 자동 후보 품질 과장 리스크는 “candidate, not final cut” 라벨로 관리 가능 |
| **합계** | **26/30** | approved |

## Verdict
`approved` — 총점 26/30, fatal flaw 없음. 단, 자동 컷 후보를 최종 편집 품질 증거로 과장하지 않는다.

## 다음 1개 액션
`/home/dev/mv-brain`에서 `examples/auto-cut-sidecar/sample.candidates.json` 스키마와 “candidate_regions → SummaryCard/Clip 후보” 변환 스펙만 먼저 적는 작업 메모를 만든다. 코드 변경은 별도 사용자 요청 전까지 하지 않는다.

## 관련 문서
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML 계약 테스트 + 데모 팩]]
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
