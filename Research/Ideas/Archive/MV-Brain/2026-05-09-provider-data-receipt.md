---
type: idea-card
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: idea-discussion
verified_by: agent
sources:
  - https://portswigger.net/web-security/cors
  - https://github.com/chn-lee-yumi/MaterialSearch
  - https://github.com/IliasHad/edit-mind
  - https://reddit.com/r/finalcutpro/comments/1rpt9sh/got_tired_of_boring_editing_and_subscriptions_so/
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/mv_brain/api/app_settings.py
  - /home/dev/mv-brain/mv_brain/api/main.py
created: 2026-05-09
reviewed_at: 2026-05-09
---

# Provider Data Receipt — 클라우드 호출 전 “무엇이 나가는지” 증명하기

## 요약

MV BRAIN의 다음 1–7일 웨지는 **Provider Data Receipt**다. 사용자가 Gemini/OpenAI/Claude 같은 외부 provider를 켤 때, 실제 호출 전후로 “어떤 데이터 유형이 외부로 나갈 수 있는지”를 UI와 JSON receipt로 남기는 기능이다. 코드 변경을 바로 하자는 뜻이 아니라, 포트폴리오용 신뢰 증명 아티팩트로 먼저 좁힌다.

## 왜 지금인가

- README는 local-first를 말하지만, 동시에 cloud AI provider가 selected frames/text/metadata/embeddings를 받을 수 있다고 명시한다. 이 경계가 포트폴리오 리뷰어에게 가장 먼저 검증받을 지점이다.
- 현재 설정 저장 경로는 `~/.mv_brain/server_settings.json`이고, API 키는 `to_dict()`에서 마스킹된다. 즉 “키를 숨김”은 이미 일부 되어 있으나 “외부 전송 데이터 설명/기록”은 별도 기능으로 보이지 않는다.
- FastAPI CORS는 기본 localhost만 허용하고 `allow_credentials=False`라 좋은 출발점이지만, provider 호출과 로컬 파일 처리의 신뢰 설명은 UI에서 더 명확히 보여줄 수 있다.

## 근거

1. PortSwigger CORS 가이드는 민감 데이터가 있는 애플리케이션에서 `Access-Control-Allow-Origin` 오설정이 외부 origin으로 데이터 노출을 만들 수 있음을 설명한다. MV BRAIN은 로컬 앱이지만 설정, output, ingest, provider 연결을 다루므로 “local-only + origin 명시 + 데이터 receipt”가 신뢰 포인트다.
2. GitHub에는 `MaterialSearch`(AI semantic local photo/video search), `edit-mind`(local-first video knowledge base)처럼 로컬 영상 검색/지식베이스 프로젝트가 이미 있다. MV BRAIN이 차별화하려면 단순 “AI 검색”보다 “편집자에게 안전하고 검증 가능한 handoff/AI 사용”을 보여줘야 한다.
3. Reddit `r/finalcutpro`에는 로컬 AI FCP 도구, FCP 자동화, 편집 스타일 학습 도구에 대한 게시물이 있다. FCP 사용자 관심은 있지만, provider/로컬 경계가 불명확하면 early user가 신뢰하기 어렵다.
4. repo smoke check: `python3 -m compileall -q mv_brain tools/fcpxml-mcp-server` 통과. 이 아이디어는 미디어 ingest나 provider 호출 없이 구현/데모 가능한 안전한 wedge다.

## 내부 토론

- idea-scout: “local-first” 주장을 강화하는 작은 신뢰 기능이 필요하다. 이미 no-provider demo, FCPXML, transcript/roughcut 아이디어가 있으므로 오늘은 기능 추가보다 신뢰/투명성 wedge가 중복을 피한다.
- builder: MVP는 작다. provider action마다 `provider`, `operation`, `data_types`, `estimated_items`, `local_paths_redacted`, `timestamp`, `dry_run`을 JSON으로 생성하고 Settings 또는 Output 페이지에 “외부 전송 예정 데이터” 카드로 노출하면 된다. 실제 provider 호출은 필요 없다.
- market: 기술 편집자/개발자 포트폴리오 관점에서 “AI 앱을 만들었다”보다 “AI 앱의 privacy boundary를 설계했다”가 더 설득력 있다. 유료 전환 시에도 BYO key/local-first 신뢰 문구의 근거가 된다.
- critic: 너무 보안 문서처럼 보이면 제품 데모가 밋밋해질 수 있다. 반드시 UI 스크린샷/JSON 예시/README 문구까지 같이 나와야 한다.
- curator: 승인. 최근 아이디어들과 중복이 낮고, 1–2일 안에 visible artifact를 만들 수 있으며, provider 호출 없이 검증 가능하다.

## 점수

| 항목 | 점수 | 이유 |
|---|---:|---|
| portfolio_fit | 5 | 로컬 AI 앱의 보안/신뢰 설계 역량을 보여준다. |
| revenue_fit | 4 | BYO-key/local-first 유료 템플릿 또는 컨설팅 wedge에 맞다. |
| build_ease | 4 | receipt JSON + UI 표시 + README 섹션으로 작게 가능하다. |
| proof_speed | 5 | no-provider fixture와 스크린샷으로 바로 증명 가능하다. |
| mv_brain_synergy | 5 | README의 privacy model, settings/provider 흐름과 직접 연결된다. |
| risk_inverse | 5 | 실제 provider 호출/미디어 처리 없이 구현 가능하다. |
| **합계** | **28/30** | approved |

## MVP 범위

1. `mv-brain demo` 또는 settings 화면에서 볼 수 있는 샘플 receipt JSON 생성: `output/demo_provider_receipt.json`.
2. UI에 “Provider Data Receipt” 카드 추가: provider별 키 존재 여부가 아니라, 전송 가능 데이터 유형과 local path redaction 여부를 표시.
3. README에 “Before cloud AI runs, MV BRAIN shows a provider data receipt” 섹션과 예시 JSON 1개 추가.

## 리스크와 완화

- 리스크: 실제 provider 호출 로깅처럼 과장될 수 있음. → MVP 이름을 `dry-run receipt` 또는 `planned data receipt`로 명확히 한다.
- 리스크: 보안 기능처럼 보이지만 강제 게이트가 아닐 수 있음. → 첫 버전은 “transparency artifact”로 제한하고, 이후 confirmation gate와 연결한다.
- 리스크: 기존 local-action-safety-gate 아이디어와 일부 겹침. → action 승인 게이트가 아니라 “외부 provider 데이터 투명성”으로 범위를 분리한다.

## 다음 1개 액션

- 코드 구현 전, `mv_brain/api/agent/*`, `mv_brain/embedding/*`, `mv_brain/ingestion/*`에서 provider로 전달되는 데이터 유형을 정적 목록으로 뽑아 `docs/public/provider-data-receipt-example.md` 초안을 만든다.

## 관련 문서

- [[MV-BRAIN-프로젝트-브리프]]
- [[2026-05-01-local-action-safety-gate]]
- [[2026-05-04-no-provider-search-quality-benchmark]]
- [[2026-05-08-transcript-roughcut-sidecar]]
