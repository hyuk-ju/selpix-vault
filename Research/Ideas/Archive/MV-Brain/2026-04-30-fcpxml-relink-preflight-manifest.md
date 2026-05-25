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
  - /home/dev/mv-brain/mv_brain/output/fcpxml_generator.py
  - https://reddit.com/r/finalcutpro/comments/1su28dk/fcpxml_issues_any_miracle_workers/
  - https://github.com/latenitefilms/BRAWToolbox/issues/101
  - https://github.com/donwellsav/ResolveToNuendoBridge/pull/12
created: 2026-04-30
reviewed_at: 2026-04-30
---
# FCPXML relink preflight manifest

## 한 줄 요약
MV BRAIN의 FCPXML export 옆에 `*.preflight.json`/`*.preflight.md`를 함께 생성해, Final Cut Pro import 전에 “어떤 원본 파일을 참조하는지, relink 위험이 무엇인지, demo roughcut이 어떤 클립으로 구성됐는지”를 사람이 바로 확인하는 1~3일짜리 포트폴리오 증거로 만든다.

## 왜 지금인가
- MV BRAIN README는 `mv-brain demo`가 샘플 클립 메타데이터와 `output/demo_roughcut.fcpxml`을 만든다고 설명하고, 제품 포지션을 FCPXML-first handoff로 잡고 있다.
- 현재 `mv_brain/output/fcpxml_generator.py`는 `asset src`를 `file://` URL로 쓰고, asset 이름·duration·timeline offset을 생성한다. 즉 export 직전에 relink/preflight manifest를 만들 데이터가 이미 있다.
- 외부 신호: r/finalcutpro의 “FCPXML issues - any miracle workers?” 글은 DaVinci에서 받은 FCPXML이 Final Cut Pro에서 import는 되지만 video relink가 실패하는 문제를 설명한다. FCPXML은 “XML이 well-formed”인 것만으로 충분하지 않고, media path/relink 맥락이 중요하다.
- 외부 신호: BRAWToolbox 이슈에는 FCP 이벤트에서 online clip이 offline이 되는 relink 문제 개선 요청이 있고, ResolveToNuendoBridge PR은 missing-media detection/reconciliation을 delivery blocking signal로 표면화한다고 설명한다. 즉 NLE handoff 도구는 “내보내기”뿐 아니라 “가져오기 전 위험 설명”이 신뢰 포인트다.

## 선택 전 후보 검토
- 후보 A: UI screenshot 자동 캡처 스크립트 — 포트폴리오에는 좋지만 현재 README/demo 흐름과 중복되고, 브라우저/빌드 의존성이 생긴다.
- 후보 B: FCPXML relink preflight manifest — 기존 FCPXML export와 demo mode를 강화하면서 실제 editor pain point와 연결된다.
- 후보 C: 실제 Final Cut Pro import 검증 — 강한 proof지만 macOS/FCP가 필요해 cron daily idea 범위에는 무겁다.
- 후보 D: provider settings hardening — 중요하지만 사용자에게 보이는 포트폴리오 proof가 약하다.

## 내부 토론
### idea-scout
MV BRAIN이 지금 보여줘야 할 다음 증거는 “FCPXML 파일을 만들었다”보다 한 단계 더 실무적인 “이 FCPXML이 어떤 원본을 참조하고, import/relink 전에 무엇을 확인해야 하는가”이다. README에 이미 no-provider demo가 생겼고, recent worktree에도 demo 관련 파일이 있으므로 preflight manifest는 그 demo를 더 믿을 수 있게 만든다.

### builder
MVP는 작게 자를 수 있다.
1. `generate_fcpxml()` 또는 demo flow 옆에서 timeline item, source path, asset id, duration, start/end, missing/local path 여부를 모아 JSON manifest를 만든다.
2. 같은 내용을 사람이 읽기 쉬운 Markdown checklist로 렌더링한다: source files, expected duration, relink warnings, “FCP import 전 확인” 3줄.
3. `mv-brain demo` 결과 예시에 `output/demo_roughcut.preflight.md`를 추가하고, no-media fixture만 사용한다.

실제 Final Cut Pro 실행, provider call, Qdrant mutation, real media ingest는 필요 없다. 기존 `fcpxml_generator.py`와 `demo.py` 주변에 순수 함수/fixture test로 시작할 수 있어 1~3일 범위다.

### market
타깃은 Final Cut Pro로 넘어가는 technical editor와, FCPXML 자동화가 실제 workflow에 붙을지 평가하는 개발자다. 수익 경로는 좁게 잡는다.
- FCPXML handoff checklist/template
- local-first roughcut export setup 서비스
- “AI 검색 → roughcut → preflight report → FCP handoff” 포트폴리오 데모/기술 글

이 아이디어는 큰 SaaS가 아니라, MV BRAIN의 FCPXML-first 차별점을 신뢰 가능한 작은 artifact로 만드는 작업이다.

### critic
가장 큰 실패 이유는 preflight가 실제 Final Cut Pro import 성공을 보장하지 못한다는 점이다. Reddit 사례처럼 relink 실패는 codec, wrapped media, NLE 간 metadata mismatch 등 로컬 환경 요소가 많다. 따라서 이름과 README 문구를 “import validator”가 아니라 “preflight/relink risk report”로 제한해야 한다. 또 4/27의 FCPXML 계약 테스트 카드와 겹칠 수 있으므로, XML 구조 검증이 아니라 media reference checklist에만 집중해야 한다.

### curator
승인. 기존 FCPXML 계약 테스트와 중복되지 않게 “relink/media-reference preflight”로 범위를 좁히면, 현재 demo mode의 다음 포트폴리오 proof로 적합하다. 외부 커뮤니티에서도 FCPXML relink/missing-media 문제가 확인되고, repo 내부에는 manifest 생성에 필요한 source path/timeline 데이터가 이미 있다.

## 점수
| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 5 | FCPXML, file URL, manifest, relink checklist가 개발자/에디터에게 바로 설명됨 |
| revenue_fit | 4 | FCPXML handoff template, local setup, workflow consulting으로 작은 유료화 가능 |
| build_ease | 4 | 기존 generator/demo data에서 sidecar를 만들면 되고 API·미디어 처리 불필요 |
| proof_speed | 4 | JSON/Markdown sample + fixture test + README snippet으로 1~3일 proof 가능 |
| mv_brain_synergy | 5 | FCPXML-first handoff와 no-provider demo를 직접 강화 |
| risk_inverse | 4 | import 보장 과장만 피하면 안전하고 로컬-only로 검증 가능 |
| **합계** | **26/30** | approved |

## Verdict
`approved` — 총점 26/30, fatal flaw 없음. 단, “Final Cut Pro import 보장”이 아니라 “relink 위험을 import 전에 드러내는 preflight report”로만 주장한다.

## 다음 1개 액션
`/home/dev/mv-brain`에서 `mv-brain demo`가 만든 `demo_roughcut.fcpxml` 옆에 붙일 `demo_roughcut.preflight.md` 샘플 포맷을 먼저 설계한다: 참조 media 목록, timeline clip table, relink warning, FCP import 전 체크리스트 3개만 포함한다.

## 관련 문서
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML 계약 테스트 + 데모 팩]]
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Research/Ideas/2026-04-29-auto-cut-sidecar-memory-bridge|Auto-cut 후보 사이드카 → 검색 가능한 클립 메모리 브리지]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
