---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-05-02-readonly-fcpxml-mcp-inspector.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프.md
created: 2026-05-02
reviewed_at: 2026-05-02
---
# MV BRAIN Read-only FCPXML MCP Inspector 적용안

## 적용 판정
`apply` — 오늘 승인 아이디어인 [[Research/Ideas/2026-05-02-readonly-fcpxml-mcp-inspector|Read-only FCPXML MCP inspector]]는 MV BRAIN의 FCPXML handoff를 “AI가 안전하게 읽고 설명할 수 있는 산출물”로 보여주는 작은 포트폴리오 작업이다.

## 현재 확인
- 대상 프로젝트: `/home/dev/mv-brain`
- repo 상태: `main...origin/main`, working tree clean
- 최근 커밋: `3604db7 chore: refresh UI lockfile audit`, `393aee3 chore: initial public-ready release`
- 안전 점검: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과
- 범위 제한: 이 태스크는 read-only contract와 sample transcript 준비용이며, 이 크론에서는 코드 수정·GitHub issue·branch·PR을 만들지 않는다.

## 내부 토론
### builder
1~3일 적용으로 충분하다. 먼저 `tools/fcpxml-mcp-server/README.md`에 “upstream 서버가 아니라 public smoke/read-only inspector compatibility layer”라는 경계와 tool contract를 쓴다. 그 다음 `sample.fcpxml` 기준 expected JSON/Markdown transcript를 준비하면 구현 전에도 포트폴리오 proof가 된다.

### market
MCP 자체보다 중요한 가치는 “로컬 편집 산출물을 안전한 read-only tool interface로 읽는다”는 신뢰다. 기술형 편집자와 개발자에게 FCPXML, 로컬 파일 경계, agent tool contract를 동시에 보여줄 수 있어 README/데모 글감으로 좋다.

### critic
MCP buzzword로 과장되면 역효과다. 실제 Final Cut Pro 제어, timeline mutation, 자동 편집 서버라고 말하지 않는다. 첫 산출물은 `inspect_fcpxml`, `list_timeline_clips`, `report_media_refs` 같은 read-only contract와 fixture transcript까지만 둔다.

### curator
`apply`로 전환한다. 오늘 아이디어는 기존 no-media demo·preflight·safety gate와 겹치지 않고, 현재 repo의 parser/test 자산을 살려 작게 증명할 수 있다.

## 1-3 step 적용안
1. `tools/fcpxml-mcp-server/README.md` 초안을 만든다: 이 폴더의 역할, read-only 범위, non-goals(FCP 제어/파일 변경/자동 편집 없음), smoke check 명령을 명시한다.
2. tool contract 3개만 정의한다: `inspect_fcpxml(path)`, `list_timeline_clips(path)`, `report_media_refs(path)` — 입력/출력 필드와 안전 제한을 Markdown 표로 쓴다.
3. `sample.fcpxml` 기준 expected transcript를 README나 `examples/` 문서 블록으로 준비한다: timeline name, clip count, connected clips, media refs, warnings를 보여준다.

## 완료 기준
- `tools/fcpxml-mcp-server/README.md` 또는 동등한 문서 초안이 생긴다.
- “read-only inspector”와 “실제 MCP/FCP 제어 아님” 경계가 명시된다.
- 3개 tool contract와 sample transcript가 있어 README/포트폴리오에 붙일 수 있다.
- 작업 후 `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server`가 계속 통과한다.

## 관련 문서
- [[Research/Ideas/2026-05-02-readonly-fcpxml-mcp-inspector|Read-only FCPXML MCP inspector]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML 계약 테스트 + 데모 팩]]
- [[Research/Ideas/2026-04-30-fcpxml-relink-preflight-manifest|FCPXML relink preflight manifest]]
- [[Research/Ideas/2026-05-01-local-action-safety-gate|Local action safety gate]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
