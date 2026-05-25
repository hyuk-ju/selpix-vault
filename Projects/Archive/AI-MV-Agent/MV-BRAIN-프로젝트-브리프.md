---
type: project
note_status: permanent
confidence_level: medium
source_agent: codex-ops
generated_via: manual
verified_by: agent
sources:
  - /home/dev/mv-brain/README.md
  - /home/dev/mv-brain/pyproject.toml
  - /home/dev/mv-brain/ui/package.json
created: 2026-04-27
reviewed_at: 2026-04-27
---
# MV BRAIN 프로젝트 브리프

MV BRAIN은 로컬 우선 AI 뮤직비디오 편집 보조 도구다. 핵심 포지션은 "AI가 최종 MV를 혼자 완성"이 아니라, 편집자가 많은 클립에서 쓸만한 장면을 빠르게 찾고 러프컷/FCPXML까지 이어가도록 돕는 개발자 친화 제품이다.

## Repo

- GitHub: `hyuk-ju/mv-brain`
- 로컬 경로: `/home/dev/mv-brain`
- 기본 브랜치: `main`
- 공개 상태: private

## 제품 방향

- 로컬 영상 클립 분석
- 씬 분할, BPM/에너지 분석, 키프레임 추출
- Gemini 기반 시각 라벨링과 임베딩
- Qdrant 기반 자연어 클립 검색
- FX/트랜지션 추천
- Final Cut Pro용 FCPXML 러프컷 export

## 포트폴리오 각도

- local-first editor assistant
- source-first clip triage
- natural-language clip search
- 3-channel embedding: visual/audio/style
- FCPXML export pipeline
- developer-facing build log and architecture notes

## 안전한 점검 명령

```bash
cd /home/dev/mv-brain
python3 -m compileall -q mv_brain
```

UI는 아직 `node_modules`가 없으면 build가 실패한다. 필요한 경우:

```bash
cd /home/dev/mv-brain/ui
npm install
npm run build
```

실제 미디어 ingest, Gemini 분석, Qdrant 쓰기, provider API 호출은 사용자가 요청할 때만 실행한다.

## 현재 확인 상태

- repo clone 완료: `/home/dev/mv-brain`
- Python syntax compile: 통과
- UI build: `ui/node_modules` 부재로 `tsc` 없음
- Hermes cron: daily progress, build-log draft, Saturday ship plan, weekly portfolio review가 repo 경로를 기준으로 갱신됨

## 다음 좋은 작업

1. README를 실제 데모 기준으로 다듬기
2. sample media 없이도 동작하는 smoke test 추가
3. FCPXML export 예시 파일/스크린샷 준비
4. 3-channel search 구조를 개발자용 아키텍처 글로 정리
5. `npm install` 후 UI build 상태 확인

## 관련 문서

- [[Runbooks/Hermes-Obsidian-운영-규칙|Hermes Obsidian 운영 규칙]]
- [[Tasks/할일-보드|할일 보드]]
- [[📊 대시보드|대시보드]]
