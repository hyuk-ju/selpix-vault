---
type: idea-card
note_status: fleeting
confidence_level: medium
source_agent: human
generated_via: manual
verified_by: none
sources: []
created: 2026-02-28
reviewed_at: 2026-02-28
---
# ADR-028: AI 바이브코딩 파이프라인

> AI 딸깍으로 기획부터 구현까지 자동화하는 4단계 프로세스

## 파이프라인 구조

```
1. Show me the PRD   →  서비스 기획 + 개발문서 작성
       ↓
2. Speckit            →  PRD 기반 SDD(Software Design Document) 생성
       ↓
3. Pumasi             →  Claude Code + Codex 병렬 코드 작성 + 머지
       ↓
4. Ralph-loop         →  SDD 기반 끊임없는 자동 구현 반복
```

## 각 단계 역할

### 1. Show me the PRD
- 서비스 아이디어 → PRD(Product Requirements Document) 자동 생성
- 사람의 의도를 구조화된 기획서로 변환
- 여기서 의도가 정확해야 뒤가 돌아감

### 2. Speckit
- PRD → SDD(Software Design Document) 변환
- 파일 구조, API 설계, 컴포넌트 구조, DB 스키마 등 개발 설계
- **파이프라인의 핵심** — SDD 품질이 전체 결과를 결정

### 3. Pumasi
- Claude Code와 Codex를 병렬로 돌려서 코드 작성
- 기능 단위로 분리해서 충돌 최소화
- 작성 → 검증 → 머지 반복

### 4. Ralph-loop
- SDD의 TODO/구현계획을 자동으로 순회하며 구현
- P0 → P1 → P2 우선순위 순서
- 한 루프에 한 작업, 검증(test/build) 통과 필수
- 종료 조건 없으면 무한 삽질 — 게이트 필요

## 적합한 프로젝트

| 적합 | 부적합 |
|------|--------|
| 새 프로젝트 0→1 빌드 | 레거시 코드베이스 수정 |
| Next.js/React SaaS MVP | 인프라/DevOps 작업 |
| 2주 내 배포 가능한 규모 | 기존 코드 이해가 선행되는 작업 |
| 컴포넌트 단위 병렬화 가능 | 단일 파일 복잡 로직 |

## Claude Code에서의 적용

```
1. /plan 모드          → PRD + SDD 역할 (Speckit 대체)
   - CLAUDE.md에 설계 규약
   - IMPLEMENTATION_PLAN.md에 SDD

2. Agent tool (worktree) → Pumasi 역할
   - general-purpose agent 병렬 실행
   - 기능 단위로 파일 충돌 방지

3. 크론 or 반복 실행     → Ralph-loop 역할
   - TaskCreate/TaskUpdate로 진행 추적
   - compact 후 반복
```

## 핵심 교훈 (OpenClaw/Selpix 실전 경험)

1. **SDD가 부실하면 loop이 삽질함** — ralph-loop 5연속 에러의 원인은 설계에서 빠진 디테일(경로 등)
2. **병렬 구현은 merge conflict 관리가 관건** — 같은 파일 건드리면 결국 사람이 봐야 됨
3. **Loop의 종료 조건 필수** — 우선순위 + 검증 게이트 없으면 의미없는 반복
4. **사람 개입은 앞쪽(기획)에 집중** — 뒤로 갈수록 자동화 비중 높이기

## 상태

- [ ] 구체적 적용 프로젝트 선정
- [ ] 각 도구별 프롬프트/설정 템플릿화
- [ ] 실전 적용 후 회고 기록
