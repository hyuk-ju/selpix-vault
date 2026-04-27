---
type: runbook
note_status: permanent
confidence_level: high
source_agent: codex-ops
generated_via: manual
verified_by: agent
sources:
  - /home/dev/openclaw/config/workspace/vault/Memory/아이디어-실험실-운영-규칙.md
  - /home/dev/openclaw/config/workspace/vault/Research/Ideas/아이디어-평가-프레임워크.md
created: 2026-04-27
reviewed_at: 2026-04-27
---
# Hermes Project Idea Lab 운영 규칙

Hermes Project Idea Lab은 현재 진행 중인 프로젝트를 기준으로 매일 아이디어 1개를 생성하고, 저녁에 적용 방법을 토론/점검한다.

## 크론

- Job: `project-idea-daily`
- Schedule: 매일 11:30 KST
- 역할: 현재 프로젝트 기준으로 오늘의 아이디어 1개 생성/토론/선별

- Job: `project-idea-apply-review`
- Schedule: 매일 22:00 KST
- 역할: 오늘 아이디어를 어떻게 적용할지 토론하고 적용 가능성 점검

- Hermes profile: `commerce`
- Skills: `domain/project-idea-lab`, `note-taking/obsidian`

## 목적

- 현재 진행 중인 프로젝트에서 파생되는 아이디어 발굴
- 랜덤 트렌드가 아니라, 현재 앵커 프로젝트의 다음 기능/데모/수익화 실험을 찾기
- 개발자 포트폴리오에 도움이 되는 아이디어 1개 선별
- 작게 수익화 실험 가능한 아이디어 선별
- 저녁에 실제 적용 방법을 토론하고 다음 액션으로 변환
- 중복/허황된/너무 큰 아이디어 자동 폐기

## 우선순위

1. MV BRAIN: `/home/dev/mv-brain`
2. Hermes/Obsidian 운영 시스템
3. Coupang/Domeggook 운영: 직접 수익 관련일 때만

기본 앵커 프로젝트는 MV BRAIN이다. 사용자가 우선순위를 바꾸지 않는 한,
아이디어는 MV BRAIN의 현재 상태를 먼저 읽고 그 다음 외부 리서치로 검증한다.

## 프로젝트 앵커 리서치 규칙

오전 아이디어 크론은 먼저 현재 프로젝트 상태를 확인한다.

- README, AGENTS, 프로젝트 브리프
- 최근 커밋과 현재 git status
- 미완성 워크플로우, 데모 부족, README/포트폴리오 약점
- 기존 아이디어 카드와 ADR 중복 여부

그 다음 프로젝트 상태에서 나온 질문으로만 외부 리서치를 한다.

- MV BRAIN: `r/videoediting`, `r/editors`, `r/finalcutpro`,
  `r/Filmmakers`, `r/musicproduction`, `r/WeAreTheMusicMakers`,
  FCPXML/GitHub issue, video editing automation, timeline search,
  local-first media tool, Qdrant/Whisper/Gemini/AI video workflow
- Hermes/Obsidian: agent workflow, Obsidian automation, local knowledge base,
  cron agent, developer agent 운영 사례
- Coupang/Domeggook: 상품 등록, 반려, 소싱, 마진, 운영 자동화에 직접 관련된 신호

`approved` 판정에는 최소 2개 이상의 구체적 근거가 필요하고, 그중 1개 이상은
Reddit, GitHub, HN, 문서, issue tracker, 제품/커뮤니티 리뷰 같은 외부 신호여야
한다. LLM 추측만 있으면 `rejected` 또는 `watch`로 둔다.

## 내부 역할

- `idea-scout`: 후보 아이디어 생성
- `builder`: 구현 난이도와 MVP 범위 평가
- `market`: 타겟 사용자, 수익 경로, 배포 각도 평가
- `critic`: 실패 이유와 치명적 결함 탐색
- `curator`: 최종 선별

## 점수표

각 항목 1-5점, 총 30점.

- `portfolio_fit`
- `revenue_fit`
- `build_ease`
- `proof_speed`
- `mv_brain_synergy`
- `risk_inverse`

판정:

- `approved`: 22점 이상이고 치명적 결함 없음
- `watch`: 18-21점 또는 근거 1개 부족
- `rejected`: 18점 미만, 중복, 치명적 결함 있음

## 저장 규칙

- 매일 정확히 1개 아이디어만 보고한다.
- `approved`만 전체 아이디어 카드로 저장한다.
- 저장 위치: `Research/Ideas/YYYY-MM-DD-{slug}.md`
- `watch`는 텔레그램 요약에만 남긴다.
- `rejected`는 중복/위험/너무 넓음 등 사유만 요약한다.
- 코드 변경, GitHub issue, branch, PR 생성은 하지 않는다.

## 저녁 적용 점검

저녁 크론은 오늘 생성된 아이디어 또는 최근 7일 내 `watch/approved` 아이디어를 하나 골라 적용 가능성을 본다.

판정:

- `apply`: `Tasks/`에 1-3단계 적용 작업으로 저장
- `watch`: 근거 1개가 부족하므로 보류
- `drop`: 중복/산만함/현재 프로젝트와 약함으로 폐기

저녁 크론도 코드는 수정하지 않는다.

## 관련 문서

- [[Memory/아이디어-실험실-운영-규칙|아이디어 실험실 운영 규칙]]
- [[Research/Ideas/아이디어-평가-프레임워크|아이디어 평가 프레임워크]]
- [[Projects/AI-MV-Agent/MV-BRAIN-프로젝트-브리프|MV BRAIN 프로젝트 브리프]]
- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
