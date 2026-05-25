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
  - /home/dev/mv-brain/mv_brain/api/routers/output.py
  - /home/dev/mv-brain/mv_brain/api/agent/tools.py
  - /home/dev/mv-brain/mv_brain/tests/test_demo_flow.py
  - /home/dev/mv-brain/mv_brain/tests/test_editing.py
  - https://reddit.com/r/finalcutpro/comments/1qu9is9/why_is_final_cut_pros_export_process_so_slow/
  - https://reddit.com/r/finalcutpro/comments/1djo46r/export_with_custom_ffmpeg_command/
  - https://reddit.com/r/finalcutpro/comments/1r49yz6/transcriptbased_rough_cutting_for_final_cut/
  - https://github.com/znyupup/ai-video-editing-skill
  - https://github.com/6missedcalls/video-editing-skill
created: 2026-05-03
reviewed_at: 2026-05-03
---
# Windows MP4 roughcut preview

## 한 줄 요약

MV BRAIN의 FCPXML/EDL 러프컷 산출물 옆에 **짧은 MP4 preview export**를 붙여, Final Cut Pro가 없는 Windows/리눅스/채용 리뷰 환경에서도 “클립 선택 → 러프컷 결과”를 눈으로 확인하게 만드는 2~5일짜리 포트폴리오 wedge다.

## 왜 지금인가

- README는 현재 `mv-brain demo`가 `data/demo/clips.json`과 `output/demo_roughcut.fcpxml`을 만든다고 설명한다. 즉 no-provider demo proof는 생겼지만, FCPXML은 Final Cut Pro가 없으면 비개발자·리뷰어가 바로 체감하기 어렵다.
- `mv_brain/api/routers/output.py`는 이미 `.mp4`를 output listing/download 대상에 포함하고, download docstring도 “미리보기 MP4”를 언급한다. 반면 `_build_roughcut()`의 `output_format`은 현재 `final_cut`/`premiere`만 허용하고 `.fcpxml` 또는 `.edl`만 생성한다. 제품 표면에는 MP4 preview 자리가 열려 있지만 생성 경로는 아직 비어 있다.
- `mv_brain/tests/test_demo_flow.py`는 Qdrant/provider 없이 demo clips fallback을 검증하고, `test_editing.py`는 fake clip 기반 roughcut/timeline/FCPXML 흐름을 테스트한다. 즉 첫 MVP는 실제 미디어 ingest 없이도 “preview plan JSON + ffmpeg command transcript + placeholder fixture”로 시작할 수 있다.
- 외부 신호: r/finalcutpro에는 “Final Cut Pro export is slow, ffmpeg with same codec is much faster”라는 글과 “Final Cut에서 원하는 h265 옵션을 바로 못 써서 uncompressed export 후 ffmpeg를 돌린다”는 글이 있다. 편집자에게 빠른 확인용/커스텀 ffmpeg 출력은 실제 pain point다.
- 외부 신호: r/finalcutpro의 transcript-based rough cutting 글은 보안 요구 때문에 cloud tool 없이 offline workflow를 만든 사례를 설명한다. MV BRAIN의 local-first roughcut preview와 맞는다.
- 외부 신호: GitHub에는 `znyupup/ai-video-editing-skill`, `6missedcalls/video-editing-skill`처럼 ffmpeg 기반 AI/video editing skill들이 보인다. 영상 agent의 신뢰 proof는 “XML을 만들었다”보다 “재생 가능한 preview artifact”가 더 직관적이다.
- 안전 점검: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과. 오늘 cron은 코드 변경·미디어 처리 없이 아이디어만 기록했다.

## 선택 전 후보 검토

- 후보 A: Read-only FCPXML MCP inspector 후속 — 2026-05-02 카드와 중복이다.
- 후보 B: Local action safety gate 후속 — 2026-05-01 카드와 중복이다.
- 후보 C: Windows MP4 roughcut preview — 기존 output router의 `.mp4` 표면과 README demo gap을 연결하고, 최근 카드와 중복이 적다.
- 후보 D: 실제 FCP import 검증 — proof는 강하지만 macOS/FCP가 필요해 daily scout 범위에서 무겁다.

## 내부 토론

### idea-scout

MV BRAIN의 다음 설득 병목은 “FCPXML 파일이 생겼다”에서 끝나면 보는 사람이 결과를 상상해야 한다는 점이다. MP4 preview는 Final Cut Pro, Qdrant, provider key 없이도 포트폴리오 방문자가 제품 가치를 바로 볼 수 있게 한다. 특히 README가 Windows/로컬 설치 가능성을 열어둔 상황에서, 재생 가능한 결과물은 가장 빠른 trust artifact다.

### builder

MVP는 실제 렌더러부터 만들 필요 없다. 1단계는 `preview_plan.json`과 ffmpeg concat/filter command transcript를 생성하는 dry-run contract다. 2단계는 테스트 fixture나 아주 짧은 generated color/audio sample만 대상으로 `preview.mp4` smoke를 붙인다. 3단계에서만 실제 selected clip source paths를 잘라 concat하는 opt-in command로 확장한다. 기존 roughcut builder, output router, demo flow 테스트를 재사용하므로 2~5일 안에 보이는 산출물이 가능하다.

### market

타깃은 Final Cut Pro를 쓰는 기술형 편집자뿐 아니라 GitHub/포트폴리오를 보는 개발자·채용자다. 수익 경로는 “MP4 렌더링 SaaS”가 아니라 local editor assistant 설치 대행, FCPXML+preview template, offline roughcut workflow 컨설팅으로 좁게 잡는다. 무료 공개 repo에서도 demo GIF/MP4가 있으면 README 전환율이 오른다.

### critic

가장 큰 실패 이유는 실제 영상 파일을 만지는 순간 ffmpeg codec, 경로, 저작권, OS 차이, 성능 문제가 커지는 것이다. 따라서 첫 산출물은 “production renderer”가 아니라 “roughcut preview proof”로 제한해야 한다. 실제 사용자 영상 ingest/provider/Qdrant 없이, fixture/dry-run/짧은 generated sample 중심으로 시작하고 README에는 FCPXML이 canonical handoff, MP4는 preview라고 명확히 적어야 한다.

### curator

승인. 최근 no-media demo/FCPXML/security/MCP 카드와 중복되지 않고, 현재 repo의 `.mp4` output 표면과 README demo gap을 직접 메운다. 외부 커뮤니티에서도 offline/ffmpeg/export pain point가 확인된다. 범위를 preview artifact로 제한하면 fatal flaw가 없다.

## 점수

| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 5 | 재생 가능한 MP4/GIF, ffmpeg command, output API, README demo가 개발자·비개발자 모두에게 즉시 보임 |
| revenue_fit | 4 | local roughcut preview/FCPXML handoff 템플릿, 설치 대행, offline editor workflow 컨설팅으로 좁은 유료화 가능 |
| build_ease | 4 | 기존 roughcut/output/demo 테스트를 재사용하고 첫 단계는 dry-run plan/fixture로 제한 가능 |
| proof_speed | 5 | 1~3일 내 preview plan JSON + sample command + README 스니펫, 2~5일 내 fixture MP4 proof 가능 |
| mv_brain_synergy | 5 | clip triage → roughcut → FCPXML handoff 사이의 체감 demo gap을 직접 줄임 |
| risk_inverse | 4 | 실제 미디어 렌더링을 opt-in/fixture로 제한하면 provider·Qdrant·FCP 리스크 낮음. ffmpeg/codec OS 차이는 남음 |
| **합계** | **27/30** | approved |

## Verdict

`approved` — 총점 27/30, fatal flaw 없음. 단, 첫 버전은 “Windows도 볼 수 있는 preview proof”이며, 최종 납품 렌더러나 FCP 대체 export로 과장하지 않는다.

## MVP 계획

1. `mv-brain demo` 산출물 옆에 둘 `demo_preview_plan.json` 스키마를 정의한다: source clip id, source path, in/out, timeline offset, duration, warning.
2. `output_format=preview_mp4`는 바로 구현하지 말고 먼저 문서/테스트 이름으로 contract를 잡는다: `build_roughcut_preview_plan()` 또는 CLI `mv-brain roughcut-preview --dry-run`.
3. fixture 단계에서는 실제 사용자 영상 대신 generated sample 또는 test fixture만 사용해 `output/demo_preview.mp4` smoke artifact를 만든다.
4. README에는 “FCPXML is the editor handoff; MP4 preview is for quick review / portfolio demo”라고 분리해서 적는다.

## 리스크와 제한

- 실제 촬영본 처리로 바로 확대하면 codec/path/성능/저작권 리스크가 커진다.
- MP4 preview가 Final Cut Pro timeline fidelity를 보장한다고 주장하면 안 된다.
- ffmpeg 의존성은 설치 확인/스킵 테스트로 다뤄야 하며, cron이나 기본 demo에서 무거운 렌더링을 자동 실행하지 않는다.

## 다음 1개 액션

`/home/dev/mv-brain`에 코드 변경을 하기 전, `docs/demo-preview.md` 또는 README 섹션 초안으로 `demo_preview_plan.json` 예시와 `ffmpeg concat` dry-run transcript를 설계한다. 첫 proof는 실제 미디어 처리 없이 fixture/dry-run으로 제한한다.

## 관련 문서

- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-28-no-media-clip-search-demo-pack|No-media 클립 검색 데모 팩]]
- [[Research/Ideas/2026-04-30-fcpxml-relink-preflight-manifest|FCPXML relink preflight manifest]]
- [[Research/Ideas/2026-05-01-local-action-safety-gate|Local action safety gate]]
- [[Research/Ideas/2026-05-02-readonly-fcpxml-mcp-inspector|Read-only FCPXML MCP inspector]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
