---
type: project
note_status: permanent
confidence_level: high
source_agent: claude-code
generated_via: sessions-spawn
verified_by: human
sources:
  - https://github.com/Vasallo94/ObsidianRAG
  - https://github.com/chroma-core/chroma
  - https://dev.to/bohowhizz/from-markdown-to-meaning-turn-your-obsidian-notes-into-a-conversational-database-using-langchain-4pi7
  - https://realpython.com/chromadb-vector-database/
created: 2026-02-28
reviewed_at: 2026-02-28
---

# OpenClaw RAG 레이어 구현 지시서

> Vault 마크다운 165개 파일을 벡터화하여 의미 기반 검색을 추가하는 프로젝트

---

## 아래 프롬프트를 OpenClaw에 전달하세요

---

```
## 임무: Obsidian Vault RAG 레이어 구현

### 배경
현재 Vault에 165개 .md 파일이 있고, 에이전트들이 fs로 직접 읽고 쓰고 있다.
파일 경로를 모르면 관련 문서를 찾기 어렵다.
벡터 임베딩 기반 검색을 추가해서, 자연어 질문으로 관련 파일을 찾을 수 있게 한다.

### 절대 원칙
1. 기존 Vault 파일 구조를 절대 변경하지 않는다
2. 기존 에이전트(Manager/Planner/Engineer/Scout/Worker)의 파일 읽기/쓰기 방식을 변경하지 않는다
3. RAG는 기존 시스템 "옆에" 별도 레이어로 추가한다
4. 실패해도 기존 시스템에 영향 없도록 독립적으로 구현한다

### 시스템 환경 (참고)
- 호스트 경로: /home/dev/openclaw/
- Vault 호스트 경로: /home/dev/obsidian-vault/ (Syncthing 동기화 중)
- 컨테이너 내부: /home/node/.openclaw/
- Python: 3.11.2
- .env 위치: /home/dev/openclaw/.env
- 기존 크론: 호스트 crontab에 등록 (trend_aggregator, pipeline_sourcing 등)

⚠️ 위 경로는 예시다. 실제 Vault 경로는 반드시 `ls`, `find` 등으로 직접 확인한 후 작업할 것.

### 구현할 것 (3단계)

---

#### Step 1: ChromaDB + 인덱서 설치

호스트(컨테이너 밖)에서 실행한다.

1. Python 가상환경 생성:
   ```bash
   cd /home/dev/openclaw
   python3 -m venv rag-env
   source rag-env/bin/activate
   ```

2. 패키지 설치:
   ```bash
   pip install chromadb openai python-dotenv watchdog
   ```
   - chromadb: 벡터 DB (로컬 저장, 서버 불필요)
   - openai: 임베딩 API (text-embedding-3-small 사용)
   - python-dotenv: .env에서 API키 로드
   - watchdog: 파일 변경 감지 (선택)

3. .env에 추가 (OpenAI API 키가 없으면 사용자에게 알림):
   ```
   OPENAI_API_KEY=sk-...
   RAG_VAULT_PATH=/home/dev/obsidian-vault
   RAG_CHROMA_PATH=/home/dev/openclaw/rag-data
   ```

   ⚠️ OpenAI 키가 없는 경우 대안:
   - Ollama 로컬 임베딩 (nomic-embed-text) 사용 가능
   - chromadb 기본 임베딩 (all-MiniLM-L6-v2) 사용 가능 → 추가 API 불필요
   - 기본 임베딩으로 먼저 구현하고, 나중에 OpenAI로 교체 가능

---

#### Step 2: 인덱서 스크립트 작성

`/home/dev/openclaw/scripts/rag_indexer.py` 생성

기능 요구사항:
1. Vault 폴더의 모든 .md 파일을 읽는다
2. 각 파일을 청크로 분할한다 (파일 단위 또는 섹션 단위)
3. 청크를 임베딩하여 ChromaDB에 저장한다
4. 메타데이터로 파일 경로, 제목, frontmatter(type, source_agent, created) 포함
5. 이미 인덱싱된 파일은 수정일(mtime) 비교하여 변경된 것만 업데이트
6. 삭제된 파일은 ChromaDB에서도 제거

청킹 전략:
- 기본: 파일 단위 (165개 파일이면 파일 통째로 넣어도 충분)
- 파일이 너무 크면 (3000자 이상): ## 헤딩 기준으로 분할
- frontmatter(---...---)는 메타데이터로 파싱하되, 본문에서는 제거

인덱서 실행 방식:
```bash
# 전체 재인덱싱
python3 scripts/rag_indexer.py --full

# 변경분만 업데이트 (크론용)
python3 scripts/rag_indexer.py --incremental
```

---

#### Step 3: 검색 API (에이전트용 스킬)

`/home/dev/openclaw/config/workspace/skills/vault-search/index.js` 생성

또는 Python HTTP 서버로 구현:
`/home/dev/openclaw/scripts/rag_search_server.py`

기능 요구사항:
1. 자연어 질문을 받는다
2. 질문을 임베딩한다
3. ChromaDB에서 유사도 상위 5개 문서를 검색한다
4. 결과를 반환한다: [{file_path, title, score, snippet}, ...]

에이전트 연동:
- Manager가 "관련 문서 찾아줘"라고 할 때 이 스킬을 호출
- 결과로 나온 file_path를 fs로 읽어서 전체 내용 참조

간단한 구현 (Python HTTP):
```python
# POST /search
# Body: {"query": "비트코인 관련 논의", "top_k": 5}
# Response: [{"path": "ADR/ADR-021-...", "title": "...", "score": 0.87, "snippet": "..."}]
```

포트: 8400 (다른 서비스와 충돌 안 되는 포트로 설정)

---

### 크론 등록

호스트 crontab에 추가:
```
# RAG 인덱서 - 매 2시간마다 증분 업데이트
0 */2 * * * cd /home/dev/openclaw && rag-env/bin/python3 scripts/rag_indexer.py --incremental >> logs/rag_indexer.log 2>&1
```

---

### 테스트 방법

구현 완료 후 아래 테스트를 실행하고 결과를 보고해라:

1. 인덱싱 테스트:
   ```bash
   python3 scripts/rag_indexer.py --full
   # 기대: "Indexed 165 files, X chunks" 출력
   ```

2. 검색 테스트 (3가지 쿼리):
   ```
   쿼리 1: "비트코인 매매봇"
   → 기대: ADR-021 또는 관련 프로젝트 문서가 상위에 나와야 함

   쿼리 2: "쿠팡 SEO 최적화"
   → 기대: 셀픽스-쿠팡-파이프라인 또는 SYSTEM_STATE가 나와야 함

   쿼리 3: "에이전트 아키텍처"
   → 기대: AGENT_ARCHITECTURE.md가 나와야 함
   ```

3. 증분 업데이트 테스트:
   - 임의 파일 하나 수정 → --incremental 실행 → 해당 파일만 업데이트 확인

---

### 주의사항

1. chromadb 기본 임베딩(all-MiniLM-L6-v2)으로 먼저 구현해라.
   OpenAI API 키가 있으면 나중에 교체할 수 있다.
2. 검색 서버는 localhost에서만 접근 가능하게 바인딩 (0.0.0.0 금지)
3. Vault 파일에 쓰기/수정/삭제를 절대 하지 마라. 읽기 전용이다.
4. 에러 발생 시 기존 시스템에 영향 주지 않도록 try-except 처리
5. 구현 완료 후 Memory/변경로그.md에 기록

### 완료 후 보고

1. 설치한 패키지 목록
2. 생성한 파일 목록과 경로
3. 인덱싱 결과 (파일 수, 청크 수, 소요 시간)
4. 테스트 3개 검색 결과
5. 크론 등록 여부
```

---

## 참고 레퍼런스

### 가장 유사한 오픈소스 프로젝트

1. **ObsidianRAG** — Obsidian Vault + ChromaDB + LangGraph
   - https://github.com/Vasallo94/ObsidianRAG
   - 하이브리드 검색 (Vector + BM25) + 위키링크 따라가기
   - 우리 Vault와 가장 유사한 구조

2. **obsidian-rag** — Obsidian + ChromaDB 심플 버전
   - https://github.com/ParthSareen/obsidian-rag
   - 가볍고 단순한 구현, 참고하기 좋음

3. **local_rag** — ChromaDB + Ollama 완전 로컬
   - https://github.com/jan-gerritsen/local_rag
   - OpenAI 없이 로컬만으로 동작하는 예제

### 튜토리얼 / 가이드

4. **From Markdown to Meaning** — LangChain + ChromaDB 단계별 가이드
   - https://dev.to/bohowhizz/from-markdown-to-meaning-turn-your-obsidian-notes-into-a-conversational-database-using-langchain-4pi7

5. **RAG for Personal Knowledge Management** — Obsidian 통합 실전 가이드
   - https://dasroot.net/posts/2025/12/rag-personal-knowledge-management-obsidian-integration/

6. **Embeddings and Vector Databases With ChromaDB** — Real Python 공식 튜토리얼
   - https://realpython.com/chromadb-vector-database/

7. **ChromaDB Step-by-Step Guide** — DataCamp 초보자 가이드
   - https://www.datacamp.com/tutorial/chromadb-tutorial-step-by-step-guide

### 고급 옵션 (나중에 참고)

8. **Graph RAG MCP Server** — 위키링크 기반 그래프 검색
   - https://lobehub.com/mcp/ferparra-graph-rag-mcp-server
   - 메타데이터 기반 그래프 관계 + ChromaDB 통합

9. **ChromaDB 공식 GitHub**
   - https://github.com/chroma-core/chroma

---

## 관련 문서

- [[📊 대시보드|📊 대시보드]]
- [[Memory/AGENT_ARCHITECTURE|에이전트 아키텍처]]
- [[Memory/SYSTEM_STATE|시스템 상태]]
- [[Memory/변경로그|변경로그]]
