---
type: research
note_status: literature
confidence_level: medium
source_agent: claude-code
generated_via: sessions-spawn
verified_by: none
sources:
  - https://github.com/jwhonce/obsidian-cli
  - https://github.com/iansinnott/obsidian-claude-code-mcp
  - https://github.com/smithery-ai/mcp-obsidian
  - https://help.obsidian.md/cli
created: 2026-03-01
reviewed_at: 2026-03-01
tags: [obsidian, cli, mcp, automation, vault, research]
---

# Obsidian CLI & MCP 생태계 분석

> OpenClaw vault 자동화에 Obsidian CLI/MCP를 적용할 수 있는지 조사한 리서치 노트.

---

## 1. Obsidian CLI란?

### 공식 CLI (Obsidian 1.12, 2026-02-27)

- Obsidian 앱 내장 CLI — `obsidian` 명령어로 터미널에서 조작
- "GUI에서 할 수 있는 건 CLI에서도 다 된다"
- 100+ 커맨드: 검색, 노트 생성, 템플릿 적용, 태스크 관리, 플러그인 관리, 프로퍼티 수정, 태그 관리 등
- **제약**: Obsidian 앱이 실행 중이어야 함 (앱이 백엔드 역할)
- **라이선스**: Early Access — Catalyst 라이선스 필요

### 커뮤니티 CLI들 (앱 불필요)

- **notesmd-cli** (Go) — 노트 CRUD, 검색, 이동
- **obsidian-cli (Rust)** — 노트 관리, 프로퍼티 조작
- **obsidian-cli (jwhonce, Python)** — MCP 통합, AI 어시스턴트 지원

---

## 2. jwhonce/obsidian-cli — MCP 상세

**구조**: Python CLI + 내장 MCP 서버

```bash
obsidian-cli --vault /path/to/vault serve
```

### MCP로 노출되는 4개 Tool

| Tool | 기능 |
|------|------|
| `create_note` | 노트 생성 (frontmatter 포함) |
| `find_notes` | 이름/제목으로 노트 검색 (exact + fuzzy) |
| `get_note_content` | 노트 내용 읽기 |
| `get_vault_info` | vault 통계 (파일 수, 구조 등) |

### CLI 커맨드 (MCP 없이도 사용 가능)

| 커맨드 | 용도 |
|--------|------|
| `ls` | 마크다운 파일 목록 |
| `cat` | 파일 내용 출력 |
| `new` | 노트 생성 |
| `edit` | 에디터에서 편집 |
| `find` | 파일 검색 |
| `query` | **frontmatter 프로퍼티 기반 필터링** |
| `meta` | frontmatter 읽기/수정 |
| `rm` | 삭제 |
| `journal` | 데일리 노트 (템플릿 변수 지원) |
| `add-uid` | UID 삽입 |
| `info` | vault 통계 |
| `serve` | MCP 서버 시작 |

### 설정 예시 (`obsidian-cli.toml`)

```toml
vault = "/home/dev/openclaw/config/workspace/vault"
editor = "vi"
ident_key = "uid"
blacklist = ["Assets/", ".obsidian/", ".git/", ".stfolder/"]
journal_template = "Memory/Daily/{year}-{month:02d}-{day:02d}"
verbose = false
```

### 평가

- **장점**: 앱 불필요, Python (서버에 바로 pip install), frontmatter query 강력, Apache 2.0
- **한계**: MCP Tool 4개뿐, wikilink 그래프 없음, 프로젝트 규모 작음 (6⭐, 46 commits), 시맨틱 검색 없음

---

## 3. iansinnott/obsidian-claude-code-mcp — Claude Code 전용

**구조**: Obsidian 플러그인 → WebSocket MCP 서버 → Claude Code 직접 연결

### MCP Tools

| Tool | 기능 |
|------|------|
| `view` | 파일 읽기 |
| `str_replace` | 파일 내용 수정 (패턴 교체) |
| `create` | 파일 생성 |
| `insert` | 내용 삽입 |
| `get_current_file` | 현재 열린 파일 정보 |
| `get_workspace_files` | vault 전체 파일 구조 |
| `obsidian_api` | Obsidian 내부 API 직접 호출 |

### 연결 방식

```
Mac Obsidian 앱 (플러그인 설치)
    ↕ WebSocket (port 22360)
Claude Code CLI (/ide → Obsidian 선택)
```

### 평가

- **장점**: Claude Code 네이티브 (`/ide`), Obsidian 내부 API 전체 접근, read+write, 활발 (166⭐)
- **한계**: Mac에서 Obsidian 앱 필수, 서버(OpenClaw)에서 사용 불가

---

## 4. smithery-ai/mcp-obsidian — 가벼운 읽기 전용

```bash
npx -y @smithery/cli install mcp-obsidian --client claude
```

- 읽기 + 검색 전용 (쓰기 없음)
- Obsidian 앱 불필요
- Claude Desktop용 설계, 아무 MCP 클라이언트에서 사용 가능
- AGPL-3.0 라이선스

---

## 5. 비교 매트릭스

|  | jwhonce/obsidian-cli | iansinnott/claude-code-mcp | smithery/mcp-obsidian |
|--|---------------------|---------------------------|----------------------|
| 앱 필요? | ❌ 불필요 | ✅ 필요 | ❌ 불필요 |
| 서버에서 실행? | ✅ 가능 | ❌ 불가 | ✅ 가능 |
| Mac에서 실행? | ✅ 가능 | ✅ 최적 | ✅ 가능 |
| 읽기 | ✅ | ✅ | ✅ |
| 쓰기 | ✅ | ✅ | ❌ |
| 검색 방식 | 파일명/제목 + frontmatter | Obsidian 내부 (그래프/링크) | 기본 검색 |
| frontmatter 조작 | ✅ query/meta | ✅ obsidian_api | ❌ |
| MCP Tools 수 | 4개 | 7개+ | ~3개 |
| 언어 | Python | TypeScript | TypeScript |
| 라이선스 | Apache 2.0 | 0BSD | AGPL-3.0 |
| 활성도 | 낮음 (6⭐) | 높음 (166⭐) | 보통 |

---

## 6. 현재 OpenClaw vault 자동화 vs CLI 도입

### 이미 갖춘 자동화

| 기능 | 현재 구현 | 방식 |
|------|----------|------|
| 노트 생성 | 크론잡 10+개 자동 작성 | Node.js `fs.writeFileSync` |
| 검색 | ChromaDB RAG + vault-search 스킬 | `rag_indexer.py` + `rag_search.py` |
| 프론트매터 검증 | `vault_health_check.js` | Phase 13 스키마 강제 |
| 신뢰도 승격 | `vault_review.js` | AI 분류 + Telegram HITL |
| Git 동기화 | vault-auto-sync + vault-git-backup 크론 | 자동 commit + push |
| 태스크 관리 | `manage_tasks.js --sync-vault` | JSON ↔ Markdown 동기화 |

### CLI 도입 시 비교

| 작업 | 현재 (Node.js 직접) | jwhonce CLI |
|------|---------------------|-------------|
| 노트 생성 | `fs.writeFileSync` + 수동 frontmatter | `obsidian-cli new --template` |
| frontmatter 검색 | vault_health_check.js 전체 스캔 | `obsidian-cli query confidence_level --equals low` |
| 시맨틱 검색 | ChromaDB RAG (**강력**) | 없음 (파일명/제목만) |
| frontmatter 수정 | 각 스크립트 자체 구현 | `obsidian-cli meta --set key=value` |

**판단**: frontmatter `query`와 `meta` 수정은 편리해짐. 검색은 현재 RAG가 훨씬 강력. **보완재로 쓸 수 있지 대체재는 아님.**

---

## 7. 추천 조합

### Mac (본인 작업)

1. **iansinnott/obsidian-claude-code-mcp** 설치 → Claude Code에서 vault를 IDE처럼 접근 (그래프/백링크 검색 + 노트 편집)
2. **Obsidian 1.12 공식 CLI** 같이 사용 → 터미널에서 빠른 노트/태스크 조작

### 서버 (OpenClaw 자동화)

1. **jwhonce/obsidian-cli** 설치 (`pip install`) → frontmatter query/meta 작업 단순화
2. **ChromaDB RAG 유지** → 검색은 이게 훨씬 강력

### 주의사항

- 기존 자동화 스크립트 대체 ❌ — 이미 10+개 크론잡이 안정적으로 동작 중
- CLI는 "추가 도구"로 활용, 기존 것 건드리지 말 것

---

## 8. 가장 임팩트 큰 한 가지

Mac에서 `iansinnott/obsidian-claude-code-mcp` 설치. Claude Code 세션에서 vault에 바로 접근하면 "vault 검색 → ADR 작성 → 변경로그 기록" 흐름이 훨씬 매끄러워짐.
