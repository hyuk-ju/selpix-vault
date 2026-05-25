---
type: idea-card
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: idea-discussion
verified_by: agent
sources:
  - https://reddit.com/r/finalcutpro/comments/1r49yz6/transcriptbased_rough_cutting_for_final_cut/
  - https://reddit.com/r/editors/comments/1r2yi48/alternative_to_descript_for_text_based/
  - https://github.com/Konadu-Akwasi-Akuoko/katto
  - /home/dev/mv-brain/README.md
created: 2026-05-08
reviewed_at: 2026-05-08
---
# Transcript Roughcut Sidecar

## 요약
MV BRAIN에 실제 음성 인식/미디어 처리 없이도 보여줄 수 있는 `transcript sidecar → clip 후보 → FCPXML/EDL roughcut` 데모를 추가하는 아이디어다. 핵심은 Whisper나 provider 호출이 아니라, 사용자가 이미 가진 `.srt/.vtt/.json` 대본을 입력으로 받아 “가사/대사 기반 러프컷 설계”를 포트폴리오 데모로 증명하는 것이다.

## 근거
- Reddit `r/finalcutpro`에 “Transcript-based rough cutting for Final Cut?” 흐름이 잡혀 있어, FCP 사용자에게 대본 기반 러프컷은 실제 관심사다.
- Reddit `r/editors`에도 “Alternative to Descript for text based editing/transcription” 논의가 있어, 텍스트 기반 편집 도구 대체/보완 수요가 있다.
- GitHub에는 `Konadu-Akwasi-Akuoko/katto`처럼 transcript-driven rough-cut + FCPXML export를 표방하는 작은 프로젝트가 있어, 좁은 wedge로 구현 가능한 영역임을 보여준다.
- 현재 MV BRAIN README는 clip triage, natural-language search, FX recommendation, FCPXML rough-cut export를 이미 전면에 두지만 `transcript/srt` 경로는 아직 보이지 않는다. 따라서 중복보다 확장성이 크다.

## 내부 토론
- `idea-scout`: 최근 카드가 FCPXML, EDL, no-provider demo, Windows MP4, FX 설명성까지 다뤘으므로 오늘은 “검색/러프컷 입력 소스”를 넓히는 게 좋다. 대본 sidecar는 실제 영상 분석 없이도 데모 가능하다.
- `builder`: 1차 MVP는 미디어 파일을 열지 않고 fixture `demo_transcript.srt`를 파싱해 timecode 후보를 만들고 기존 roughcut/FCPXML 경로에 넘기는 형태면 된다. 실패해도 독립 CLI/문서 데모로 격리 가능하다.
- `market`: 편집자·뮤직비디오 제작자는 “후렴/브릿지/가사 구간 기준으로 후보 컷을 뽑는다”는 설명을 즉시 이해한다. 향후 유료 템플릿/컨설팅은 `lyrics/transcript-to-FCPXML pack`으로 좁힐 수 있다.
- `critic`: 실제 ASR 품질이나 저작권 있는 가사 처리로 확장하면 위험해진다. 그래서 승인 조건은 provider 없는 sidecar-only, 샘플/사용자 제공 대본, 로컬 파일 출력으로 제한한다.
- `curator`: 기존 카드들과 직접 중복되지 않고, 1-7일 내 README/GIF/fixture artifact가 가능하므로 승인한다.

## 점수
| 항목 | 점수 | 이유 |
|---|---:|---|
| portfolio_fit | 5 | 대본 파싱, timecode 매핑, 편집 툴 handoff를 보여준다. |
| revenue_fit | 4 | FCPXML/EDL 템플릿, 편집자 워크플로 컨설팅, 데모 리드마그넷으로 연결 가능하다. |
| build_ease | 4 | SRT/VTT 파서와 fixture 중심이면 미디어/AI 호출 없이 가능하다. |
| proof_speed | 5 | 샘플 대본 → cutlist → FCPXML/EDL 출력 스크린샷을 빠르게 만들 수 있다. |
| mv_brain_synergy | 5 | 기존 roughcut/FCPXML/검색 포지셔닝을 강화한다. |
| risk_inverse | 4 | provider·미디어 처리 없이 제한하면 낮은 위험. 단, 가사/저작권 샘플은 주의 필요. |
| 총점 | 27/30 | approved |

## MVP 범위
1. `demo_transcript.srt` 또는 JSON fixture를 읽어 `segment_id, start, end, text, energy_hint` 후보를 만든다.
2. “후렴/bridge/high energy” 같은 키워드 선택으로 cutlist JSON을 출력한다.
3. 기존 FCPXML/EDL roughcut 경로에 연결하거나, 최소한 README에 no-media transcript demo 명령과 예상 출력 파일을 추가한다.

## 리스크와 제한
- 실제 Whisper/ASR 호출은 이번 MVP에서 제외한다.
- 저작권 있는 가사 샘플 대신 자체 작성 샘플 문장을 사용한다.
- 자동 최종 편집이 아니라 editor-in-the-loop roughcut 후보 생성으로 표현한다.

## 다음 1개 액션
- 코드 수정 전에 `mv_brain`에 SRT/VTT 관련 경로가 정말 없는지 확인하고, `demo_transcript.srt → cutlist JSON`만 만드는 no-media CLI/문서 스펙을 1페이지로 잡는다.

## 관련 문서
- [[MV-BRAIN-프로젝트-브리프]]
- [[2026-05-07-fx-recommendation-explainability-gallery]]
- [[2026-05-06-universal-edl-handoff-smoke]]
- [[2026-05-04-no-provider-search-quality-benchmark]]
