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
  - /home/dev/mv-brain-hermes/README.md
  - /home/dev/mv-brain-hermes/mv_brain_hermes/core.py
  - /home/dev/mv-brain-hermes/exports/chorus/cutlist.csv
  - https://reddit.com/r/finalcutpro/comments/1ry48b9/final_cut_pro_x_commandpost_mcp_almost_instantly/
  - https://reddit.com/r/finalcutpro/comments/1slaa0o/update_on_doza_assist_free_local_ai_editors/
  - https://github.com/IliasHad/edit-mind
  - https://github.com/vulture-s/arkiv
created: 2026-05-05
reviewed_at: 2026-05-05
---
# Agent-readable cutlist clip pack

## 한 줄 요약

MV BRAIN 본체의 FCPXML 중심 데모와 별도로, `mv-brain-hermes` 어댑터가 이미 만드는 **JSON/CSV cutlist + README + preview_manifest**를 “에이전트가 읽고 편집자가 바로 넘겨받는 clip pack” 데모로 포장한다. 핵심은 새 영상 처리 기능이 아니라, Final Cut Pro가 없어도 확인 가능한 범용 handoff proof다.

## 왜 지금인가

- MV BRAIN README는 clip triage, 자연어 검색, FX 추천, FCPXML handoff를 핵심 가치로 설명한다. 최근 아이디어는 no-media demo, FCPXML preflight/MCP, local safety, MP4 preview, search benchmark를 다뤘다. 오늘은 중복을 피해서 “검색 결과가 편집자/에이전트에게 어떤 전달물로 남는가”에 집중한다.
- `/home/dev/mv-brain-hermes`는 이미 MV BRAIN과 별도인 Hermes/MCP adapter로 존재한다. README는 no-provider demo 명령 `demo → search → export-cutlist`를 제시하고, 결과물로 `cutlist.json`, `cutlist.csv`, `README.md`를 만든다고 설명한다.
- 실제 확인한 샘플 `/home/dev/mv-brain-hermes/exports/chorus/cutlist.csv`에는 `demo_clip_01`, `demo_edit_01`, `demo_clip_06` 같은 선택 클립과 source, start/end, duration, labels, description, score가 들어 있다. 즉 “AI가 고른 후보를 사람이 검토 가능한 표/JSON으로 남긴다”는 포트폴리오 artifact가 이미 작동한다.
- `mv_brain_hermes/core.py`는 `write_cutlist()`, `export_cutlist()`, `export_clips()`, `export_preview_manifest()`를 제공한다. MP4 렌더링은 `--render`일 때만 실행하고, 기본은 metadata-only라 cron/데모 안전성과 맞다.
- 외부 신호: r/finalcutpro의 CommandPost MCP 글은 prompt로 scene cut, silence removal, timeline info CSV export 같은 자동화를 언급한다. 영상 편집 도구와 MCP/CSV handoff가 실제 관심 주제라는 신호다.
- 외부 신호: r/finalcutpro의 Doza Assist 글은 local-first editor assistant가 finished video에서 편집 스타일을 배우고, 긴 인터뷰 footage에서 중요한 3분을 찾는 pain을 설명한다. MV BRAIN의 local-first clip pack은 이 방향과 맞지만, 범위를 뮤직비디오 clip triage와 범용 전달물로 좁힌다.
- 외부 신호: GitHub의 `IliasHad/edit-mind`는 local-first video knowledge base, `vulture-s/arkiv`는 local-first media asset manager with AI semantic search를 내세운다. local/semantic search 자체는 경쟁이 있으므로 MV BRAIN은 “검색 후 편집 전달물”을 보여주는 차별 proof가 필요하다.
- 안전 점검: `/home/dev/mv-brain`는 `main...origin/main` clean, 최근 커밋은 `3604db7 chore: refresh UI lockfile audit`; `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과. `/home/dev/mv-brain-hermes`도 `main...origin/main` clean, `python3 -m compileall -q mv_brain_hermes && python3 -m pytest -q` 통과했다. 오늘 cron은 코드·미디어·provider·Qdrant 변경 없이 아이디어 카드만 기록했다.

## 선택 전 후보 검토

- 후보 A: Provider/API key privacy receipt — 2026-05-01 local action safety gate와 겹치고, 구현이 보안/설정 쪽으로 넓어진다.
- 후보 B: 실제 MP4 preview 구현 후속 — 2026-05-03 Windows MP4 roughcut preview와 중복이다.
- 후보 C: Agent-readable cutlist clip pack — 기존 어댑터 산출물을 포트폴리오 proof로 묶고, FCPXML이 없어도 확인 가능한 handoff artifact라 중복이 낮다.
- 후보 D: 실제 CommandPost/FCP 자동화 연동 — 외부 신호는 강하지만 FCP/macOS와 timeline mutation이 필요해 daily scout 범위에서 위험하다.

## 내부 토론

### idea-scout

지금 MV BRAIN의 약점은 “AI가 클립을 찾았다”와 “편집자가 실제로 다음 단계에서 쓴다” 사이가 아직 관찰자에게 추상적이라는 점이다. adapter에는 이미 cutlist CSV/JSON과 preview manifest가 있다. 이것을 clip pack으로 이름 붙이면 FCPXML보다 더 범용적이고, Hermes/MCP agent가 읽을 수 있으며, 포트폴리오 방문자가 파일 하나만 봐도 워크플로를 이해한다.

### builder

MVP는 1~2일짜리 문서/fixture 작업이다. 코드부터 바꾸지 않는다. `exports/chorus/`를 “golden clip pack” 예시로 정리하고, README 또는 `docs/clip-pack.md` 초안에 포함할 구조를 잡는다: `cutlist.csv`는 편집자용, `cutlist.json`은 agent/tool용, `preview_manifest.json`은 rough preview용, `README.md`는 human handoff용. 이미 adapter 테스트가 통과하므로 첫 proof는 명령 transcript와 파일 tree/readback이면 충분하다.

### market

타깃은 Final Cut Pro만 쓰는 사람이 아니라 Premiere, DaVinci, CapCut, 모바일 편집자, 그리고 local AI 편집 workflow를 평가하는 개발자다. 수익 경로는 좁게 보면 “AI clip search SaaS”가 아니라 music-video/editor clip pack 템플릿, local setup, agent-readable metadata handoff 컨설팅이다. GitHub 포트폴리오에서도 CSV/JSON/README 산출물은 채용자·사용자 모두가 빠르게 이해한다.

### critic

가장 큰 실패 이유는 너무 얇아 보이는 것이다. 단순 CSV export는 흔하다. 따라서 메시지는 “CSV를 만들었다”가 아니라 “자연어 검색 결과를 editor-readable + agent-readable + optional render-safe pack으로 묶는다”여야 한다. 또 adapter가 본체 분석 품질을 대체한다고 말하면 안 된다. 본체는 풍부한 metadata/FCPXML, adapter는 범용 handoff layer로 분리해야 한다.

### curator

승인. 최근 카드들과 직접 중복되지 않고, 이미 동작하는 adapter 산출물과 외부 편집/MCP/local-first 신호가 있다. fatal flaw인 “너무 얇음”은 golden fixture, command transcript, file contract, README positioning으로 해소 가능하다.

## 점수

| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 5 | CSV/JSON/README/manifest 파일 tree와 명령 transcript가 개발자·편집자 모두에게 보이는 artifact임 |
| revenue_fit | 4 | clip pack template, local editor setup, agent handoff workflow 컨설팅으로 좁은 유료화 가능 |
| build_ease | 5 | adapter에 이미 구현과 샘플 export가 있고 첫 단계는 문서/fixture 정리 중심 |
| proof_speed | 5 | 1일 내 golden clip pack README, 파일 tree, 명령 예시, before/after 설명 가능 |
| mv_brain_synergy | 5 | MV BRAIN의 검색/러프컷 결과를 FCPXML 외 범용 전달물로 확장 |
| risk_inverse | 5 | provider, 실제 media ingest, Qdrant mutation, FCP 실행 없이 metadata-only로 시작 가능 |
| **합계** | **29/30** | approved |

## Verdict

`approved` — 총점 29/30, fatal flaw 없음. 단, 첫 산출물은 “golden clip pack demo/contract”이며 실제 FCP/CommandPost 자동화나 MP4 렌더링은 opt-in 후속으로만 둔다.

## MVP 계획

1. `/home/dev/mv-brain-hermes/exports/chorus/`를 기준으로 `golden clip pack` 구조를 문서화한다: `cutlist.csv`, `cutlist.json`, `README.md`, 선택적으로 `preview_manifest.json`.
2. README에 넣을 6줄 demo transcript를 만든다: 자연어 쿼리 → 검색 결과 → cutlist export → 편집자가 CSV 확인 → agent가 JSON 재사용.
3. MV BRAIN 본체 README/overview에 “FCPXML handoff + universal clip pack handoff”를 분리해 설명하는 문구 초안을 만든다.
4. MP4 렌더링은 `--render` opt-in으로만 설명하고, no-provider/no-media 기본 데모에서는 metadata-only임을 유지한다.

## 리스크와 제한

- 단순 CSV export처럼 보이지 않게, agent-readable JSON과 human-readable README, optional preview manifest까지 하나의 contract로 묶어야 한다.
- `mv-brain-hermes`가 본체의 분석/임베딩 품질을 대체한다고 주장하면 안 된다. adapter는 handoff layer다.
- 실제 local footage 렌더링은 codec/path/저작권 리스크가 있으므로 기본 proof에서는 실행하지 않는다.

## 다음 1개 액션

`/home/dev/mv-brain-hermes/exports/chorus/README.md`를 기준으로, 코드 변경 전에 `golden clip pack` 설명 초안을 만든다. 포함할 문장은 “CSV는 편집자용, JSON은 에이전트용, preview manifest는 렌더 전 검토용, MP4 rendering은 opt-in” 네 가지로 제한한다.

## 관련 문서

- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-05-02-readonly-fcpxml-mcp-inspector|Read-only FCPXML MCP inspector]]
- [[Research/Ideas/2026-05-03-windows-mp4-roughcut-preview|Windows MP4 roughcut preview]]
- [[Research/Ideas/2026-05-04-no-provider-search-quality-benchmark|No-provider search quality benchmark]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
