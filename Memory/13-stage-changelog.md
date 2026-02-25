### 2026-02-25 17:18 — 비즈니스 모델(벨류) 확장
- **변경**: Selpix 상세페이지 생성기에 13단계 세일즈 파이프라인(유튜브 공식) 템플릿 통합 및 랜딩페이지 Features 표출.
- **사유**: 기존의 일반 템플릿 제공에서 한발짝 나아가, '마케팅 심리학'에 기반하여 페인포인트부터 결제 유도까지 강력한 전환율을 이끌어내는 고효율 카피 생성기를 제공함으로써 Selpix SaaS의 본질적 가치(Value Proposition) 강화.
- **영향**: `detail-builder` 코어 템플릿 로직(`conversion-13-step.ts`), 랜딩페이지 UI(`features-section.tsx`), Vercel 운영망 반영.
- **롤백**: 기존 템플릿 index 복원 및 Vercel 이전 배포본 Revert.
