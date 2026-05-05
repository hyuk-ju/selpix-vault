---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-05-05-agent-readable-cutlist-clip-pack.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain-hermes/README.md
  - /home/dev/mv-brain-hermes/exports/chorus/README.md
created: 2026-05-05
reviewed_at: 2026-05-05
---
# MV BRAIN — Agent-readable cutlist clip pack 적용

## 적용 판정

`apply` — 오늘 아이디어는 코드 변경 없이 바로 문서/fixture 작업으로 전환할 수 있다. 핵심은 `mv-brain-hermes`가 이미 만드는 `cutlist.csv`, `cutlist.json`, `README.md`를 “editor-readable + agent-readable clip pack”이라는 포트폴리오 proof로 정리하는 것이다.

## 확인한 상태

- `/home/dev/mv-brain`: `main...origin/main`, working tree clean.
- 최근 커밋: `3604db7 chore: refresh UI lockfile audit`.
- 안전 점검: `python3 -m compileall -q mv_brain` 통과.
- `/home/dev/mv-brain-hermes`: `main...origin/main`, working tree clean.
- 기존 샘플: `/home/dev/mv-brain-hermes/exports/chorus/` 안에 `README.md`, `cutlist.csv`, `cutlist.json` 존재.
- 이번 적용 리뷰에서는 코드, GitHub issue/branch/PR, provider 호출, 실제 미디어 ingest, Qdrant mutation을 실행하지 않았다.

## 내부 토론

### builder

1~2일 안에 끝낼 수 있는 적용안이다. 먼저 `exports/chorus/README.md`를 기준으로 golden clip pack contract를 초안화하고, 그 다음 본체 README/overview에 “FCPXML handoff와 universal clip pack handoff는 다르다”는 설명을 붙이면 된다. 구현 변경은 필요하지 않다.

### market

Final Cut Pro가 없는 사람도 CSV/JSON/README 산출물을 보고 제품 흐름을 이해할 수 있다. 포트폴리오에서는 “자연어 검색 → 편집 가능한 전달물”이 눈에 보이고, 수익화 쪽으로는 local editor setup, clip pack 템플릿, agent handoff workflow 컨설팅으로 좁게 연결된다.

### critic

위험은 “그냥 CSV export”처럼 보이는 것이다. 따라서 산출물 이름과 설명을 `clip pack contract`로 묶고, CSV는 편집자용, JSON은 에이전트용, README는 human handoff용이라고 분명히 해야 한다. 또 `mv-brain-hermes`가 본체 분석 품질을 대체한다고 쓰면 안 된다.

### curator

`apply`로 둔다. 이미 승인된 아이디어이고, 안전 점검도 통과했으며, 첫 액션은 문서/fixture 정리라 저위험이다.

## 1-3 step 적용안

1. `/home/dev/mv-brain-hermes/exports/chorus/README.md`를 “golden clip pack” 설명으로 확장하는 초안을 만든다.
   - 포함 문장: “CSV는 편집자용, JSON은 에이전트용, README는 human handoff용, MP4 rendering은 `--render` opt-in.”
2. `/home/dev/mv-brain-hermes/README.md` 또는 별도 `docs/clip-pack.md`에 6줄 demo transcript를 추가하는 변경안을 작성한다.
   - 예: demo 생성 → 자연어 검색 → cutlist export → CSV 확인 → JSON을 agent context로 재사용 → optional render는 보류.
3. `/home/dev/mv-brain/README.md`의 handoff 설명에 “FCPXML handoff + universal clip pack handoff”를 분리해 보여주는 2~3문장 포지셔닝 초안을 만든다.

## 완료 기준

- `exports/chorus/README.md`만 봐도 clip pack의 파일 역할을 이해할 수 있다.
- README 초안에는 no-provider/no-media 기본 데모와 opt-in MP4 렌더링의 경계가 명확하다.
- 본체 MV BRAIN과 Hermes adapter의 역할이 분리되어 있다: 본체는 분석/검색/FCPXML, adapter는 agent-facing universal handoff.
- 코드 변경 전 단계에서는 provider 호출, 실제 미디어 처리, Qdrant mutation이 없다.

## 관련 문서

- [[Research/Ideas/2026-05-05-agent-readable-cutlist-clip-pack|Agent-readable cutlist clip pack]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Research/Ideas/2026-05-04-no-provider-search-quality-benchmark|No-provider search quality benchmark]]
- [[Research/Ideas/2026-05-03-windows-mp4-roughcut-preview|Windows MP4 roughcut preview]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
