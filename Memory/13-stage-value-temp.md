---
type: discussion
note_status: permanent
confidence_level: medium
source_agent: openclaw-main
generated_via: manual
verified_by: human
sources: []
created: 2026-02-27
reviewed_at: 2026-03-01
---
## 2026-02-25 운영 고정값 (벨류) — 상세페이지 빌더 13단계 마케팅 심리학 템플릿 통합 완료
- 변경/결정: 
  - Selpix 상세페이지 빌더(`detail-builder`) 코어 엔진에 마케팅 심리학 기반의 13단계 세일즈 파이프라인(Hook -> Problem -> Agitation -> Solution -> USP -> How it Works -> Testimonial -> Authority -> Benefits -> Offer -> Risk Reversal -> FAQ -> CTA) 신규 템플릿 추가 및 AI 프롬프트 지시어 통합. 
  - 랜딩페이지 Features 섹션에 해당 기능 카드 하이라이트 노출 후 Vercel 운영 서버 배포 완료.
- 근거(증거 파일):
  - `/home/dev/selpix/src/lib/detail-builder/templates/types.ts`
  - `/home/dev/selpix/src/lib/detail-builder/templates/conversion-13-step.ts`
  - `/home/dev/selpix/src/lib/detail-builder/templates/index.ts`
  - `/home/dev/selpix/src/components/landing/features-section.tsx`
  - 유튜브 13단계 상세페이지 파이프라인 분석 데이터
- 영향 범위:
  - 기존 백엔드/API 런타임을 수정하지 않고 Section Type만 확장하여 완벽히 호환. 유저가 '초고효율 13단계 전환형' 템플릿 선택 시, AI가 13단계 프로세스를 순차적으로 밟으며 고도화된 세일즈 카피 자동 생성.
- 롤백 방법:
  - `src/lib/detail-builder/templates/index.ts`에서 `conversion-13-step` 라우팅 제거 및 Vercel 롤백 배포.
- 다음 점검 시각:
  - 지속 모니터링 및 실제 전환율 데이터 실측 시점.
