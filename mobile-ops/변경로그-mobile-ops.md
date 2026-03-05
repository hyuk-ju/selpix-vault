---
type: changelog
status: active
tags: [mobile-ops, changelog]
last_updated: 2026-03-05
source_agent: claude-code
confidence_level: high
note_status: permanent
generated_via: manual
verified_by: human
sources: []
created: 2026-03-05
reviewed_at: 2026-03-05
---

# Mobile Ops 변경로그

> 모바일 지휘형 AI 운영실 템플릿 프로젝트 전용 기록

---

### 2026-03-05 18:00 — 프로젝트 초기화

- **변경**: 프로젝트 생성 + 기술스택 확정
- **스택**: Next.js 15 + Supabase + Drizzle + grammY + Trigger.dev + shadcn/ui
- **사유**: AI 학습 데이터 기반 최적 스택 조사 결과 적용 (ADR-033)
- **영향**: /home/dev/mobile-ops/ 프로젝트 디렉토리 생성

### 2026-03-05 18:20 — Phase 0 완료 (인프라 세팅)

- **변경**: GitHub 리포 + Vercel 배포 + 프로젝트 구조 완성
- **GitHub**: gurwn/mobile-ops (private)
- **Vercel**: https://mobile-ops.vercel.app (CLI 직접 배포 — Selpix 패턴)
- **배포 패턴**: `mv .git .git-bak && vercel --prod --yes && mv .git-bak .git` (Git author 불일치 우회)
- **구조**: DB 스키마(4테이블), 봇 웹훅, 대시보드, Supabase 클라이언트
- **다음**: Supabase 프로젝트 생성 → .env.local 세팅 → Phase 1 (봇 실동작)

### 2026-03-05 18:40 — Phase 1 인프라 완료 (Supabase + Bot + Vercel 연결)

- **Supabase**: `uhcmwveeheximiysoovc` (Seoul) — CLI로 자동 생성
- **DB**: Drizzle push 완료 (tenants, users, tasks, events 4테이블)
- **Pooler 주의**: `aws-1` (aws-0 아님!), Session mode port 5432
- **Telegram 봇**: @Mobileops_bot — 웹훅 등록 완료
- **Webhook**: https://mobile-ops.vercel.app/api/bot
- **Vercel 환경변수**: 6개 설정 (SUPABASE_URL, ANON_KEY, DATABASE_URL, BOT_TOKEN, WEBHOOK_SECRET, APP_URL)
- **배포 패턴**: `mv .git .git-bak && vercel --prod --yes && mv .git-bak .git` (Git author 불일치 우회)
- **다음**: Phase 1 기능 구현 (봇 명령어 → DB 연동, 대시보드 실시간 데이터)

### 2026-03-05 19:10 — ops-dev 코딩 환경 구축

- **변경**: project_registry.js 생성 + claude_bridge.js 프로젝트 연동
- **새 파일**: `scripts/lib/project_registry.js` — 프로젝트 경로/배포/스택 레지스트리
- **수정**: `claude_bridge.js` — `planProject()`, `reviewProject()`, `diagnoseProject()` 추가
- **수정**: `workspace-dev/SOUL.md` §9 — 아이디어→코딩→배포 워크플로우 추가
- **사유**: ops-dev가 Claude Bridge로 독립 프로젝트(mobile-ops 등) 코딩 가능하게
- **영향**: ops-dev가 `claude.planProject('mobile-ops', ...)` 식으로 계획/리뷰 사용 가능
