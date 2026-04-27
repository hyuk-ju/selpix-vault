---
type: runbook
note_status: permanent
confidence_level: high
source_agent: codex-ops
generated_via: manual
verified_by: agent
sources: []
created: 2026-04-27
reviewed_at: 2026-04-27
---
# Hermes Obsidian 운영 규칙

Hermes `commerce` 프로필은 기존 OpenClaw Obsidian vault를 장기 기록 저장소로 사용한다.

## 경로

- 서버 vault: `/home/dev/openclaw/config/workspace/vault`
- Hermes env: `/home/dev/.hermes/profiles/commerce/.env`
- 환경변수: `OBSIDIAN_VAULT_PATH=/home/dev/openclaw/config/workspace/vault`
- GitHub remote: `hyuk-ju/selpix-vault`

## 읽기 규칙

- 전체 vault 검색과 읽기는 허용한다.
- 도매꾹/쿠팡 상품 매핑은 RAG보다 정확 조회 CLI를 먼저 사용한다.
- RAG는 인덱싱 상태와 API quota를 확인한 뒤 보조 검색으로만 사용한다.

## 쓰기 허용 위치

- `Projects/`: active project notes
- `Research/`: research and trend summaries
- `Products/`: product mapping and listing evidence
- `Business/`: business strategy and operations
- `Finance/`: revenue, cost, margin, settlement notes
- `Sourcing/`: sourcing notes
- `Tasks/`: task boards and action lists
- `Runbooks/`: repeatable operating procedures
- `Memory/Daily/`: dated daily notes
- `Memory/변경로그.md`: important system/process changes

## 쓰기 제한 위치

다음 위치는 사용자가 명시적으로 요청하지 않는 한 쓰지 않는다.

- `Memory/Conversations/`
- `Templates/`
- `History/`
- `00_Vault_Index/`
- `.obsidian/`

## 금지 작업

- 대량 삭제
- 대량 이동
- 폴더명 변경
- 스타일 정리를 이유로 한 과거 노트 일괄 재작성
- 민감정보, 토큰, API 키, 주문자 개인정보 기록

## 새 노트 frontmatter

```yaml
---
type: idea-card | research | adr | project | runbook | discussion
note_status: fleeting | literature | permanent
confidence_level: low | medium | high
source_agent: codex-ops | agent-cron | human
generated_via: manual | cron | daily-briefing | idea-discussion
verified_by: human | agent | none
sources: []
created: YYYY-MM-DD
reviewed_at: YYYY-MM-DD
---
```

## 백링크

새 노트 또는 의미 있는 수정이 있는 노트는 마지막에 `## 관련 문서` 섹션을 둔다.

예시:

```markdown
## 관련 문서
- [[📊 대시보드|📊 대시보드]]
- [[Tasks/할일-보드|📋 할일 보드]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
```

## 백업

의미 있는 vault 변경 후 사용자가 백업을 요청하면 서버 vault repo에서 커밋하고 push한다.

```bash
cd /home/dev/openclaw/config/workspace/vault
git status --short --branch
git add -A
git commit -m "auto: vault backup YYYY-MM-DD"
git push origin main
```

## 관련 문서

- [[📊 대시보드|📊 대시보드]]
- [[Memory/변경로그|변경로그]]
- [[Templates/README|템플릿 인덱스]]
- [[Memory/운영-규칙|운영 규칙]]
