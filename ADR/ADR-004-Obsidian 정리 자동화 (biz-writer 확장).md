---
type: adr
status: active
created: 2026-02-20
updated: 2026-02-20
owner: main
tags: [idea, openclaw, adr]
related: [[Research/Ideas/2026-02-20-Obsidian 정리 자동화 (biz-writer 확장)]]
---
# ADR-004: Obsidian 정리 자동화 (biz-writer 확장)

## 맥락 (Context)
매일 21:30 KST obsidian-daily-cleanup 크론으로 Daily 링크체인/백링크 보정, 대시보드 날짜·핵심 링크 업데이트, 할일-보드 상태 정리. 보고는 리포트 토픽(29)에 요약 5줄+변경 파일 경로. 1주 파일럿 후 누락률/재작업률/정리시간 KPI로 유지 여부 판단.

## 결정 (Decision)
**⏸️ 보류**
(Fast Track - 시스템 내부 작업)
- 기술: 보류 — 기술적 난이도가 극도로 낮으며 시스템 유지보수에 큰 도움을 줌

- Critic: ? — 

## 검토한 대안 (Options)
(critic이 대안을 제시하지 않음)

## 트레이드오프 (Consequences)
- 실행 시: 개발 비용 및 시스템 유지보수 복잡도 증가 가능성
- 미실행 시: 기술 부채 증가 및 생산성 손실

## 리스크 및 탐지 신호
| 리스크 | 내용 | 탐지 신호 |
|--------|------|-----------|
| 치명적 결함 | - | 초기 검증 실패 |
| 시장 리스크 | - | 수요 지표 하락 |
| 실행 리스크 | - | MVP 일정 초과 |

## 관련 Spec / Runbook
- [[Research/Ideas/2026-02-20-Obsidian 정리 자동화 (biz-writer 확장)|아이디어 카드]]
