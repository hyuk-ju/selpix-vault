---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-05-09-provider-data-receipt.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/mv_brain/api/app_settings.py
  - /home/dev/mv-brain/mv_brain/api/agent/gemini.py
  - /home/dev/mv-brain/mv_brain/api/agent/openai_provider.py
  - /home/dev/mv-brain/mv_brain/embedding/gemini_embedder.py
created: 2026-05-09
reviewed_at: 2026-05-09
---
# MV BRAIN Provider Data Receipt 적용 점검

## 적용 판정

- **결정**: apply
- **대상 프로젝트**: MV BRAIN (`/home/dev/mv-brain`)
- **아이디어 카드**: [[2026-05-09-provider-data-receipt|Provider Data Receipt]]
- **repo 상태**: `main...origin/main`, working tree clean. 최근 커밋: `fe8852f docs: align Korean overview with public positioning`.

## 검증 결과

- `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server`: 통과
- `python3 -m pytest mv_brain/tests/test_cli.py mv_brain/tests/test_demo_flow.py -q`: `10 passed in 0.36s`
- 중복 검색: `Tasks/`에서 `provider-data-receipt`, `데이터 영수증`, `privacy receipt`, `provider` 관련 기존 적용 노트 없음
- 정적 확인:
  - `mv_brain/api/app_settings.py`: server settings에 provider API key 저장/마스킹 흐름 존재
  - `mv_brain/api/agent/gemini.py`: chat 메시지와 tool schema가 Gemini provider로 전달됨
  - `mv_brain/api/agent/openai_provider.py`: chat 메시지, system prompt, tool schema가 OpenAI provider로 전달됨
  - `mv_brain/embedding/gemini_embedder.py`: text embedding, label text, frame bytes가 Gemini API로 전달될 수 있음

## 내부 토론

- **builder**: 코드 구현 전 문서/fixture부터 만드는 것이 맞다. 현재 코드 경로만으로 provider별 데이터 유형을 정적 목록화할 수 있고, provider 호출 없이 README/데모 신뢰도를 올릴 수 있다.
- **market**: local-first AI 편집 도구에서 “무엇이 외부로 나갈 수 있는지”를 보여주는 것은 포트폴리오 신뢰 포인트다. 단순 AI 검색보다 early user/개발자 리뷰어에게 차별화된다.
- **critic**: 실제 호출 로그처럼 과장하면 위험하다. 첫 적용은 반드시 `planned/dry-run receipt` 문구로 제한해야 한다.
- **curator**: apply. 구현 착수 전 1일 작업으로 문서 초안 + 샘플 JSON + README 문구까지 좁히면 충분히 안전하고 가시적이다.

## 1-3단계 적용안

1. `docs/public/provider-data-receipt-example.md` 초안 작성: provider별 전송 가능 데이터 유형을 `chat`, `embedding`, `frame_analysis`, `tool_schema`로 나누고 local path/API key는 redacted 처리한다.
2. `output/demo_provider_receipt.json` 또는 문서 내 fixture JSON 예시를 만든다: `provider`, `operation`, `data_types`, `redactions`, `dry_run: true`, `created_at` 필드를 포함한다.
3. README Privacy Model 아래에 “Planned Provider Data Receipt” 짧은 섹션을 추가해, 실제 provider 호출 전 사용자가 확인할 수 있는 투명성 아티팩트임을 명확히 쓴다.

## 완료 기준

- no-provider 상태에서 문서/fixture만으로 설명 가능하다.
- API key, 실제 파일 경로, 개인 영상 정보가 문서나 fixture에 노출되지 않는다.
- `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server`와 `python3 -m pytest mv_brain/tests/test_cli.py mv_brain/tests/test_demo_flow.py -q`가 계속 통과한다.
- README 문구가 “완전 로컬”이 아니라 “cloud provider를 켤 때 무엇이 나갈 수 있는지 보여준다”로 정확히 표현된다.

## 관련 문서

- [[Research/Ideas/2026-05-09-provider-data-receipt|Provider Data Receipt]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
