---
type: idea
note_status: literature
confidence_level: medium
source_agent: antigravity
generated_via: user-request
verified_by: none
sources: []
created: 2026-03-01
reviewed_at: 2026-03-01
tags: [ultramemory, jarvis, memory, encryption, minimac, openclaw, architecture]
---

# Ultramemory 개발 현황 및 OpenClaw 구조 비교

> 2026-03-01 사용자 브리핑 기반 정리.

---

## 1. Ultramemory 현재 상태

| 단계 | 상태 |
|------|------|
| 메모리 암호화 및 저장 설계 | ✅ 완료 |
| Codex 작업 내용 → 미니맥 저장 | ✅ 완료 |
| Claude Code 작업 내용 → 미니맥 저장 | ✅ 완료 |

## 2. Ultramemory 로드맵

| 기능 | 상태 |
|------|------|
| 웹 메모장 + Ultramemory 연동 | 미구현 |
| OpenClaw 모든 활동 → 미니맥 저장 | 미구현 |
| ChatGPT/Claude/Gemini 활동 자동 저장 + 동기화 | 미구현 |
| 이미지/음성/영상 업로드 → 분석 → 메모리 저장 | 미구현 |
| 카카오톡/텔레그램 대화내역 → 메모리 저장 | 미구현 |

## 3. 핵심 아키텍처 비전

```
저장된 기억 ←→ 작업 시 호출 ←→ 세션에 반영
```

→ "Jarvis v0.5" — 모든 디지털 활동이 중앙 메모리에 축적되고, 작업 시 자동 호출되는 구조.

---

## 4. OpenClaw 현재 구조와 비교

### 이미 비슷한 부분 ✅

| Ultramemory 개념 | OpenClaw 현재 구현 |
|------------------|-------------------|
| 작업 내용 저장 | vault/ (마크다운 기반 지식 저장소) |
| 저장 → 검색 | ChromaDB RAG (vault-search, 시맨틱 검색) |
| 세션에 반영 | SOUL.md §4: vault-search 0순위 참조 규칙 |
| 주기적 인덱싱 | vault-rag-incremental-index 크론 (6시간마다) |
| 메모리 그래프 | memory-graph-index 크론 (2시간마다) |

### 빠져있는 부분 ❌

| Ultramemory 개념 | OpenClaw 갭 |
|------------------|------------|
| **암호화 저장** | vault은 평문 마크다운, 암호화 없음 |
| **텔레그램 대화 저장** | 다이제스트 전송 후 초기화, 대화 아카이브 없음 |
| **멀티소스 수집** | OpenClaw 내부 활동만 저장, 외부(ChatGPT/Gemini) 미연동 |
| **미디어 분석** | 이미지/음성/영상 분석 후 메모리화 없음 |
| **웹 인터페이스** | CLI/텔레그램만, 웹 메모장 없음 |
| **미니맥 동기화** | 서버 로컬 저장만, 외부 디바이스 동기화 없음 |

### 구조적 유사도

```
Ultramemory                    OpenClaw (현재)
┌────────────────┐            ┌────────────────┐
│  미니맥 (중앙)  │            │  vault/ (중앙)  │
│  암호화 저장    │            │  평문 마크다운   │
├────────────────┤            ├────────────────┤
│ Codex 연동 ✅   │            │ 크론잡 결과 ✅  │
│ Claude 연동 ✅  │            │ 인텔 토론 ✅    │
│ ChatGPT ❌     │            │ ChatGPT ❌     │
│ 카카오톡 ❌     │            │ 카카오톡 ❌     │
│ 텔레그램 ❌     │            │ 텔레그램 ❌     │
├────────────────┤            ├────────────────┤
│ 시맨틱 검색     │            │ ChromaDB RAG   │
│ 세션 자동 반영  │            │ SOUL.md 참조   │
└────────────────┘            └────────────────┘
```

**결론:** OpenClaw의 vault + ChromaDB RAG는 Ultramemory의 "저장 → 검색 → 세션 반영" 파이프라인과 **구조적으로 동일**. 차이는 (1) 암호화 유무, (2) 수집 소스 범위, (3) 동기화 대상.

---

## 5. 보안 메모

> "미니맥 누가 훔쳐가면 두뇌 뜯긴 느낌"

- 암호화 설계 완료했다면, **미니맥 분실 시 복호화 키 없이는 읽기 불가** (AES-256 등 사용 시)
- 추가 방어: 원격 와이프, 하드웨어 암호화(FileVault/BitLocker), 키 분리 저장
- OpenClaw vault은 현재 평문 → Ultramemory 수준의 암호화 도입 시 동일 보안 수준 확보 가능
