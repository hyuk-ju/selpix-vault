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
  - /home/dev/mv-brain/mv_brain/output/edl_generator.py
  - /home/dev/mv-brain/mv_brain/tests/test_editing.py
  - https://github.com/mfahsold/montage-ai
  - https://github.com/svyatoclav/RAC
  - https://github.com/valentinpop/auto-broll
  - https://reddit.com/r/VideoEditing/comments/p0peie/programmer_question_how_to_create_a_davinci/
  - https://reddit.com/r/VideoEditing/comments/m0quhn/how_to_premiere_resolve_roundtrip_with_transitions/
created: 2026-05-06
reviewed_at: 2026-05-06
---
# Universal EDL handoff smoke

## 한 줄 요약

MV BRAIN의 FCPXML 중심 포지셔닝에 더해, 이미 존재하는 `CMX 3600 EDL` 생성기를 **Premiere/Resolve도 이해할 수 있는 최소 handoff proof**로 포장한다. 새 영상 처리나 provider 호출 없이, fixture 기반 EDL 샘플·검증 명령·README 문구만으로 “Final Cut Pro가 없어도 러프컷 전달물을 확인할 수 있다”는 포트폴리오 증거를 만든다.

## 왜 지금인가

- README는 MV BRAIN을 Final Cut Pro handoff 중심으로 설명한다. 이는 명확하지만, 포트폴리오 관찰자나 Windows/Premiere/Resolve 사용자는 FCP가 없어도 볼 수 있는 교환 파일 proof를 원할 수 있다.
- repo 내부에는 이미 `/home/dev/mv-brain/mv_brain/output/edl_generator.py`가 있다. `generate_edl()`, `validate_edl_file()`, `seconds_to_edl_timecode()`가 구현되어 있고, 설명도 “Premiere/Resolve smoke tests”로 적혀 있다.
- `/home/dev/mv-brain/mv_brain/tests/test_editing.py`는 EDL generator를 import하고 편집 파이프라인 테스트에 포함한다. 오늘 안전 점검으로 `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과, `python3 -m pytest mv_brain/tests/test_editing.py -q`도 `41 passed`로 통과했다.
- 외부 GitHub 신호: `mfahsold/montage-ai`는 “local-first AI video editor”와 “OTIO/EDL export for Premiere/Resolve”를 함께 내세운다. 즉 local AI video editor에서 EDL/Resolve/Premiere handoff는 실제 비교 지점이다.
- 외부 GitHub 신호: `svyatoclav/RAC`는 Premiere Pro, DaVinci Resolve 등에서 쓸 `.edl` 파일을 생성한다고 설명하고, `valentinpop/auto-broll`도 Premiere XML / Resolve EDL timeline export를 내세운다. 자동 컷/AI B-roll 계열에서 EDL은 범용 전달물로 쓰인다.
- 외부 Reddit 신호: r/VideoEditing에는 DaVinci Resolve용 Marker/EDL 파일을 어떻게 만들지 묻는 “Programmer question” 글과 Premiere↔Resolve roundtrip 관련 글이 있다. 편집자 커뮤니티에서도 “툴 간 timeline/marker 교환”은 반복 pain이다.

## 선택 전 후보 검토

- 후보 A: Provider-key privacy receipt — 2026-05-01 local action safety gate와 보안 축이 겹친다.
- 후보 B: Visual screenshot/GIF proof flow — no-media demo, MP4 preview, clip pack 카드와 일부 중복되고 UI build/실행 증거가 추가로 필요하다.
- 후보 C: Universal EDL handoff smoke — 기존 FCPXML 카드들과 달리 Premiere/Resolve까지 “확인 가능한 범용 교환 파일”로 좁혀 차별된다.
- 후보 D: 실제 Resolve/Premiere import 검증 — 외부 앱 실행과 OS 의존성이 커서 daily scout 범위를 넘는다.

## 내부 토론

### idea-scout

MV BRAIN은 FCPXML이 핵심이지만, “Final Cut Pro가 없는 사람도 산출물을 이해할 수 있나?”가 포트폴리오 데모의 병목이다. 이미 EDL generator가 있으므로 오늘의 wedge는 새 기능 발명보다 숨은 기능을 product proof로 승격하는 것이다.

### builder

MVP는 1~2일이면 충분하다. 실제 media ingest 없이 `test_editing.py` fixture timeline으로 golden `.edl`을 생성하고, `validate_edl_file()` 결과를 문서화한다. README에는 “FCPXML first, EDL smoke handoff for Premiere/Resolve review” 정도로만 넣는다. Resolve/Premiere import 성공을 주장하지 않고 “portable timeline smoke artifact”로 제한하면 테스트 가능하다.

### market

타깃은 Final Cut Pro 사용자만이 아니라 Premiere/Resolve도 쓰는 technical editor, 그리고 로컬 AI video assistant를 비교하는 개발자다. 수익 경로는 “다중 NLE 자동화 SaaS”가 아니라, editor handoff template, local roughcut workflow setup, FCPXML/EDL export 품질 개선 컨설팅으로 좁게 잡는다.

### critic

가장 큰 실패 이유는 EDL이 오래된 포맷이고 효과/멀티캠/타임리맵 같은 복잡한 편집 정보를 온전히 담지 못한다는 점이다. 그래서 이 아이디어는 “완전한 Premiere/Resolve export”가 아니라 “검토 가능한 최소 roughcut/marker handoff”로만 승인해야 한다. 실제 앱 import 호환성은 후속 수동 검증 전까지 주장 금지다.

### curator

승인. 최근 아이디어가 FCPXML, MP4 preview, benchmark, clip pack에 집중되어 있었기 때문에 EDL smoke는 중복이 낮고, repo 내부 코드와 외부 시장 신호가 모두 있다. critic의 우려는 범위를 “minimal EDL smoke”로 제한하면 fatal flaw가 아니다.

## 점수

| 항목 | 점수 | 근거 |
|---|---:|---|
| portfolio_fit | 5 | EDL 파일, validator, pytest 결과가 개발자에게 보이는 명확한 artifact |
| revenue_fit | 4 | Premiere/Resolve 사용자까지 handoff consulting/template 범위를 넓힘 |
| build_ease | 5 | 기존 `edl_generator.py`와 test fixture 재사용, provider/media/FCP 불필요 |
| proof_speed | 5 | 1일 내 golden EDL 샘플, validation transcript, README snippet 가능 |
| mv_brain_synergy | 5 | roughcut handoff 가치를 FCPXML-only에서 범용 timeline smoke로 확장 |
| risk_inverse | 4 | 안전하지만 EDL 포맷 한계와 import 과장 리스크가 있음 |
| **합계** | **28/30** | approved |

## Verdict

`approved` — 총점 28/30, fatal flaw 없음. 단, 첫 산출물은 “minimal EDL handoff smoke”이며 Premiere/Resolve 완전 호환이나 효과 보존을 주장하지 않는다.

## MVP 계획

1. fixture timeline을 사용해 `docs/samples/demo_roughcut.edl` 같은 golden EDL 샘플을 만든다.
2. `validate_edl_file()` 실행 결과와 `python3 -m pytest mv_brain/tests/test_editing.py -q` 통과 transcript를 `docs/edl-handoff.md`에 짧게 정리한다.
3. README에 “FCPXML is the primary handoff; EDL is a minimal Premiere/Resolve smoke artifact”라는 2~3문장 문구를 추가할 초안을 만든다.
4. 실제 Resolve/Premiere import, 효과/멀티캠 보존, real media path relink는 후속 수동 검증으로 분리한다.

## 리스크와 제한

- EDL은 단순 컷 리스트 중심이라 FX, compound clip, multicam, speed ramp 보존을 기대하면 안 된다.
- 실제 NLE import 검증 전에는 “Premiere/Resolve compatible” 대신 “Premiere/Resolve review smoke”로 표현해야 한다.
- Windows MP4 preview, agent-readable clip pack과 메시지가 섞이지 않게, 이 카드는 “교환 파일 포맷 다양화” 하나만 다룬다.

## 다음 1개 액션

`/home/dev/mv-brain/mv_brain/tests/test_editing.py`의 fixture timeline을 기준으로 golden EDL 샘플 생성 절차를 먼저 문서화한다. 코드 변경은 별도 개발 세션에서만 하고, 문구는 “FCPXML primary + EDL minimal smoke”로 제한한다.

## 관련 문서

- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML contract demo pack]]
- [[Research/Ideas/2026-05-02-readonly-fcpxml-mcp-inspector|Read-only FCPXML MCP inspector]]
- [[Research/Ideas/2026-05-05-agent-readable-cutlist-clip-pack|Agent-readable cutlist clip pack]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
