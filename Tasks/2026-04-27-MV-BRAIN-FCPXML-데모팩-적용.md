---
type: discussion
note_status: fleeting
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/2026-04-27-fcpxml-contract-demo-pack.md
  - /home/dev/openclaw/config/workspace/vault/Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프.md
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/AGENTS.md
  - /home/dev/mv-brain/mv_brain/cli.py
  - /home/dev/mv-brain/mv_brain/tests/test_fcp_bridge.py
created: 2026-04-27
reviewed_at: 2026-04-27
---
# MV BRAIN FCPXML 데모팩 적용 작업

## 적용 판정
`apply`

오늘 승인된 아이디어 `FCPXML 계약 테스트 + 데모 팩`은 MV BRAIN의 핵심 포지션인 “클립 검색/러프컷 → Final Cut Pro FCPXML export”를 가장 빠르게 검증 가능한 포트폴리오 산출물로 바꿀 수 있다.

## 확인한 현재 상태
- repo: `/home/dev/mv-brain`
- git 상태: `main...origin/main`, 미추적 `AGENTS.md` 존재
- 최근 커밋: `044628c Build source-first MV Brain MVP`
- 안전 점검: `python3 -m compileall -q mv_brain` 통과
- 테스트 실행 시도: `python3 -m pytest mv_brain/tests/test_fcp_bridge.py -q`는 현재 시스템 Python에 `pytest`가 없어 실행 불가
- README에는 `validate-fcpxml`, `open-fcpxml`, `fcp-import` 명령이 문서화되어 있음
- `mv_brain/cli.py`에도 위 명령 파서와 함수가 존재함
- 그러나 `mv_brain/output/` 경로가 현재 검색되지 않아 `mv_brain.output.fcpxml_generator`, `mv_brain.output.fcp_bridge` import 경로가 실제 구현/패키징과 맞는지 확인이 필요함

## 내부 적용 토론
### builder
1일 적용 범위는 “새 기능 구현”이 아니라 FCPXML 경로를 깨지지 않게 만드는 데모 계약 정리다. 먼저 `mv_brain/output/` 모듈 존재 여부와 CLI import 경로를 정렬하고, 샘플 FCPXML fixture + validate 명령 + README 데모를 하나의 smoke flow로 묶는다.

### market
FCPXML 데모팩은 외부인이 README만 보고도 “이 프로젝트는 실제 편집 툴과 연결된다”는 증거를 준다. 수익화는 당장 SaaS보다 FCPXML validator/template, local setup consulting, 편집 자동화 포트폴리오 리드로 좁게 시작하는 편이 좋다.

### critic
주의점은 Final Cut Pro 실제 import 검증을 서버에서 할 수 없다는 것이다. 따라서 “FCP에서 반드시 열린다”가 아니라 “XML well-formed + 내부 계약 검증 + macOS/FCP 수동 검증 체크리스트”까지만 주장해야 한다. 또 pytest 미설치 상태를 먼저 해결하지 않으면 테스트 카드가 말뿐이 된다.

### curator
`apply`로 전환한다. 코드 변경은 이 크론에서 하지 않고, 다음 수동/개발 세션에서 1-3단계로 처리한다.

## 1-3 step 적용안
1. **FCPXML 구현 경로 확인/정렬**
   - 확인: `mv_brain/output/`가 누락된 것인지, 다른 모듈에 기능이 있는지 검색한다.
   - 목표: `mv-brain validate-fcpxml`, `mv-brain fcp-import --no-open`이 import error 없이 동작할 최소 모듈 경로를 확정한다.

2. **fixture 기반 smoke test 추가/복구**
   - 샘플 위치 후보: `examples/fcpxml/sample.fcpxml` 또는 기존 `tools/fcpxml-mcp-server/examples/sample.fcpxml`.
   - 검증 기준: XML well-formed, root/tag 기본 확인, timeline/clip summary 출력, malformed XML 실패.
   - 먼저 테스트 환경의 `pytest` 의존성 설치/실행 방법을 README 또는 dev setup에 명시한다.

3. **README 데모 섹션을 실제 명령 기준으로 축소 정리**
   - 포함: fixture validate 명령, 기대 출력 예시, FCP 실제 import는 macOS/FCP 수동 검증 단계로 분리.
   - 제외: 대용량 media ingest, Gemini/Qdrant 쓰기, 실제 provider 호출.

## 완료 기준
- `python3 -m compileall -q mv_brain` 통과
- `python3 -m pytest mv_brain/tests/test_fcp_bridge.py -q` 또는 새 fixture smoke test 통과
- README에 “sample 없이 확인 가능한 FCPXML demo”가 실제 존재하는 명령과 일치
- Final Cut Pro 실제 import 보장 문구 없이, 수동 검증 체크리스트로 표현

## 관련 문서
- [[Research/Ideas/2026-04-27-fcpxml-contract-demo-pack|FCPXML 계약 테스트 + 데모 팩]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Runbooks/Hermes-Project-Idea-Lab-운영-규칙|Hermes Project Idea Lab 운영 규칙]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
