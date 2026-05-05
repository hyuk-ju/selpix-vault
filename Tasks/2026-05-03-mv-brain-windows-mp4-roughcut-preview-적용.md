---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-05-03-windows-mp4-roughcut-preview.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프.md
created: 2026-05-03
reviewed_at: 2026-05-03
---
# MV BRAIN Windows MP4 roughcut preview 적용안

## 적용 판정
`apply` — 오늘 승인 아이디어인 [[Research/Ideas/2026-05-03-windows-mp4-roughcut-preview|Windows MP4 roughcut preview]]는 MV BRAIN의 no-media demo와 FCPXML handoff 사이에 “Final Cut Pro 없이도 확인 가능한 재생 산출물”을 붙이는 작은 포트폴리오 작업이다.

## 현재 확인
- 대상 프로젝트: `/home/dev/mv-brain`
- repo 상태: `main...origin/main`, working tree clean
- 최근 커밋: `3604db7 chore: refresh UI lockfile audit`, `393aee3 chore: initial public-ready release`
- 안전 점검: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과
- 범위 제한: 이 태스크는 preview contract/문서/fixture smoke 설계용이며, 이 크론에서는 코드 수정·GitHub issue·branch·PR을 만들지 않는다.

## 내부 토론
### builder
1~3일 적용으로 충분하다. 첫 단계는 실제 렌더러가 아니라 `demo_preview_plan.json` contract와 ffmpeg dry-run transcript를 문서로 고정하는 것이다. 그 다음 generated fixture 또는 매우 짧은 test sample만 대상으로 smoke MP4를 붙이면, 실제 사용자 영상·Qdrant·provider 없이도 proof가 된다.

### market
FCPXML은 기술적으로 중요하지만 Final Cut Pro가 없는 채용 리뷰어·Windows 사용자에게는 즉시 체감이 어렵다. 짧은 MP4 preview와 README 스니펫은 “클립 선택 → 러프컷 결과”를 눈으로 보여줘 포트폴리오 전환율과 설치 대행/오프라인 roughcut workflow 상담 가능성을 높인다.

### critic
실제 영상 렌더링으로 바로 확장하면 ffmpeg codec, OS 차이, 저작권, 성능 문제가 커진다. 따라서 첫 적용은 “production renderer”가 아니라 “preview proof”로 제한해야 한다. README에도 FCPXML이 canonical editor handoff이고 MP4는 빠른 리뷰용 preview라고 분리한다.

### curator
`apply`로 전환한다. 최근 FCPXML inspector, local action safety, no-media demo와 중복되지 않고, 현재 repo README와 output router의 `.mp4` 표면을 활용해 작게 증명할 수 있다.

## 1-3 step 적용안
1. `docs/demo-preview.md` 또는 README 하위 섹션 초안으로 `demo_preview_plan.json` 예시를 작성한다: clip id, source path, in/out, timeline offset, duration, warning 필드를 포함한다.
2. ffmpeg 실행은 먼저 dry-run transcript로만 설계한다: concat/filter command 예시, required ffmpeg check, 실패 시 skip/warning 규칙을 문서화한다.
3. 다음 코드 작업 후보는 fixture 전용 smoke로 제한한다: generated sample 또는 test fixture만 사용해 `output/demo_preview.mp4`를 만들고, 실제 사용자 영상 preview는 opt-in 후속으로 둔다.

## 완료 기준
- `docs/demo-preview.md` 또는 README demo preview 섹션 초안이 생긴다.
- `demo_preview_plan.json` 예시와 ffmpeg dry-run transcript가 문서에 있다.
- “FCPXML = 편집기 handoff, MP4 = 빠른 리뷰/포트폴리오 preview” 경계가 명확하다.
- 작업 후 `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server`가 계속 통과한다.

## 관련 문서
- [[Research/Ideas/2026-05-03-windows-mp4-roughcut-preview|Windows MP4 roughcut preview]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Research/Ideas/2026-04-30-fcpxml-relink-preflight-manifest|FCPXML relink preflight manifest]]
- [[Research/Ideas/2026-05-02-readonly-fcpxml-mcp-inspector|Read-only FCPXML MCP inspector]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
