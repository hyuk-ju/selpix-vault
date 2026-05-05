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
  - /home/dev/mv-brain/tools/fcpxml-mcp-server/fcpxml/parser.py
  - /home/dev/mv-brain/mv_brain/tests/test_fcp_bridge.py
  - https://raw.githubusercontent.com/modelcontextprotocol/modelcontextprotocol/main/README.md
  - https://github.com/DareDev256/fcpxml-mcp-server
  - https://github.com/elliotttate/SpliceKit
  - https://github.com/kevinten-ai/mcp-ffmpeg
created: 2026-05-02
reviewed_at: 2026-05-02
---
# Read-only FCPXML MCP inspector

## 한 줄 요약
MV BRAIN의 기존 `tools/fcpxml-mcp-server` 호환 parser를 출발점으로, 실제 Final Cut Pro 제어나 파일 변경 없이 **FCPXML을 읽고 timeline/clips/warnings를 설명하는 read-only MCP inspector 데모**를 만드는 1~3일짜리 포트폴리오 wedge다.

## 왜 지금인가
- README는 MV BRAIN의 핵심 handoff를 `FCPXML rough-cut export → Final Cut Pro`로 설명한다. 최근 아이디어들은 no-media demo, FCPXML preflight, local action safety에 집중했다. 오늘은 “AI 도구가 편집 산출물을 어떻게 안전하게 이해하는가”를 보여주는 read-only 인터페이스가 중복을 피한다.
- repo 내부에는 이미 `/home/dev/mv-brain/tools/fcpxml-mcp-server/fcpxml/parser.py`가 있다. 주석상 “upstream fcpxml-mcp-server가 아니라 public smoke tests가 private checkout에 의존하지 않도록 둔 tiny parser”이며, asset path, timeline, connected clip, transition을 파싱한다.
- `mv_brain/tests/test_fcp_bridge.py`는 이 tool 경로를 `sys.path`에 넣고 `sample.fcpxml`을 parse/analyze/validate/import-no-open 흐름으로 테스트한다. 즉 read-only inspector에 필요한 최소 parsing proof는 이미 repo에 있다.
- 외부 신호: Model Context Protocol 공식 repo는 MCP specification, protocol schema, official documentation을 제공한다고 설명한다. “AI 클라이언트가 도구를 호출해 로컬 자료를 읽는 표준 인터페이스”를 포트폴리오에 보여줄 명분이 있다.
- 외부 신호: GitHub 검색 결과 `DareDev256/fcpxml-mcp-server`는 “AI-powered MCP server for Final Cut Pro XML”, `elliotttate/SpliceKit`은 “Final Cut Pro command palette, MCP server, plugin framework”, `kevinten-ai/mcp-ffmpeg`는 “FFmpeg video/audio editing tools via MCP”를 표방한다. 영상 편집 도구와 MCP를 붙이는 시도가 이미 생기고 있으나, MV BRAIN은 더 안전하게 read-only FCPXML inspector로 좁힐 수 있다.
- 안전 점검: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과. 실제 media ingest, provider call, Qdrant mutation, FCP 실행 없이 진행 가능하다.

## 선택 전 후보 검토
- 후보 A: README no-media demo 문안 적용 — 오늘 Task가 이미 존재해 중복이다.
- 후보 B: Local action safety gate 구현 — 2026-05-01 카드와 중복이다.
- 후보 C: Read-only FCPXML MCP inspector — 기존 parser/test 자산을 살리면서 최근 카드와 다른 “agent tool interface” proof를 만든다.
- 후보 D: MP4 rough preview export — 사용자에게 보이는 artifact는 강하지만 ffmpeg/미디어 처리 경로가 넓어지고 오늘은 외부 신호 대비 구현 범위가 더 크다.

## 내부 토론
### idea-scout
MCP 기반 편집 도구 repos가 이미 보이고, MV BRAIN repo에도 `fcpxml-mcp-server` 호환 parser가 남아 있다. 지금 좋은 wedge는 “FCP를 조종하는 AI”가 아니라 “AI가 FCPXML 산출물을 read-only로 검사하고 설명하는 도구”다. 이는 local-first와 editor-in-the-loop 포지션을 해치지 않는다.

### builder
MVP는 작게 자른다.
1. MCP server 전체 구현보다 먼저 `tools/fcpxml-mcp-server/README.md` 또는 `docs/public`에 read-only tool contract를 쓴다: `inspect_fcpxml(path)`, `list_clips(path)`, `timeline_warnings(path)`.
2. `examples/sample.fcpxml` 기준 expected JSON/Markdown transcript를 만든다: timeline name, clip count, media refs, connected clips, duration warnings.
3. 기존 parser와 `test_fcp_bridge.py`를 근거로 no-provider/no-FCP smoke test 명령을 문서화한다.

새 provider, 실제 media ingest, Qdrant, FCP import, live FCP 제어는 제외한다. 구현으로 넘어가도 parser wrapper + fixture output + README transcript라 1~3일 범위다.

### market
타깃은 AI coding/tooling에 관심 있는 개발자와 FCPXML workflow를 평가하는 기술형 편집자다. 포트폴리오 효과는 “MCP를 유행어로 붙였다”가 아니라 “로컬 편집 산출물을 안전한 read-only tool interface로 노출했다”는 점이다. 수익 경로는 FCPXML handoff/setup 템플릿, local editor-agent integration 컨설팅, demo writeup/lead magnet으로 좁게 잡는다.

### critic
가장 큰 실패 이유는 MCP가 buzzword로 보이거나, 이미 있는 외부 FCPXML MCP 서버와 차별성이 약해지는 것이다. 따라서 “Final Cut Pro control”, “AI edits your timeline”은 금지하고, MV BRAIN의 차별점인 no-provider demo + generated FCPXML + read-only validation transcript에만 집중해야 한다. 또한 현재 repo의 `tools/fcpxml-mcp-server`는 실제 upstream server가 아니라 compatibility parser이므로, 아이디어 제목/문서에서 이 사실을 숨기면 안 된다.

### curator
승인. 최근 아이디어와 중복이 적고, repo 내부 자산이 있으며, 외부 MCP/FCPXML 편집 도구 신호가 충분하다. 단, 첫 산출물은 MCP 서버 구현이 아니라 read-only inspector contract와 sample transcript로 제한한다.

## 점수
| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 5 | MCP, FCPXML parsing, fixture transcript, local-first tool boundary가 개발자에게 선명함 |
| revenue_fit | 4 | FCPXML handoff/setup, editor-agent integration 컨설팅/템플릿으로 좁은 유료화 가능 |
| build_ease | 4 | 기존 parser/test/sample.fcpxml 재사용, 새 미디어/Provider/FCP 불필요 |
| proof_speed | 4 | README contract + sample JSON/Markdown transcript + compile/test 명령으로 1~3일 proof 가능 |
| mv_brain_synergy | 5 | FCPXML handoff와 editor-in-the-loop 포지션을 직접 강화 |
| risk_inverse | 5 | read-only 범위로 제한하면 파일 변경/외부 API/저작권/보안 리스크 낮음 |
| **합계** | **27/30** | approved |

## Verdict
`approved` — 총점 27/30, fatal flaw 없음. 단, 첫 버전은 “read-only inspector”로만 주장하고, 실제 FCP 제어·timeline mutation·자동 편집 MCP는 범위 밖으로 둔다.

## MVP 계획
1. `tools/fcpxml-mcp-server/README.md` 초안: 이 폴더는 upstream server가 아니라 MV BRAIN public smoke/read-only inspector compatibility layer라고 명확히 적는다.
2. `examples/sample.inspect.json` 또는 문서 블록: `sample.fcpxml`에서 timeline, clips, connected clips, media refs, warnings를 보여준다.
3. README/overview에 4문장 demo transcript: “AI client asks → local read-only FCPXML inspector answers → editor checks before FCP handoff”.

## 리스크와 제한
- MCP buzzword 과장 금지: 실제 서버 구현 전에는 “MCP-ready/read-only inspector contract”로 표현한다.
- FCP import 성공 보장 금지: XML 구조와 media reference 요약만 다룬다.
- 파일 변경 금지: inspector는 읽기 전용이며 path traversal/output mutation 범위와 섞지 않는다.

## 다음 1개 액션
`/home/dev/mv-brain/tools/fcpxml-mcp-server/README.md`에 붙일 read-only inspector contract 초안을 만든다. 포함할 tool은 3개만 둔다: `inspect_fcpxml`, `list_timeline_clips`, `report_media_refs`. 코드 변경은 별도 개발 세션에서만 한다.

## 관련 문서
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML 계약 테스트 + 데모 팩]]
- [[Research/Ideas/2026-04-30-fcpxml-relink-preflight-manifest|FCPXML relink preflight manifest]]
- [[Research/Ideas/2026-05-01-local-action-safety-gate|Local action safety gate]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
