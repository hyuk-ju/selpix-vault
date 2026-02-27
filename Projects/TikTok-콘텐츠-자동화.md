---
type: research
note_status: fleeting
confidence_level: low
source_agent: openclaw-main
created: 2026-02-19
---
# 📱 TikTok 콘텐츠 자동화

## 상태: 🟡 보류 (고도화 예정)

## 전략
- 플랫폼: TikTok 단독
- 훅 3종: 가격충격형 / 공감반응형 / 꿀팁발굴형
- 수익화: 쿠팡 파트너스 링크 전환 추적
- 계정: today_item_hunt

## 구현 현황
- larry 스킬 (`skills/larry/`) — 슬라이드 생성
- postiz 스킬 (`skills/postiz/`) — 업로드
- 한글 폰트 수정 완료 (Noto Sans CJK KR)

## 미완 항목
- [ ] 이미지 퀄리티 개선 (OpenAI API 활용 예정)
- [ ] 파트너스 링크 5개 생성 후 links.html 추가
- [ ] links.html 호스팅

## 고도화 방향
- OpenAI API로 상품 이미지/카피 자동 생성
- 도매꾹 원본 이미지 → AI 리터칭
- 자동 포스팅 → 성과 분석 → 소싱 최적화 피드백 루프

## 관련 문서
- [[Projects/셀픽스-쿠팡-파이프라인|🛒 쿠팡 파이프라인]] — 상품 소싱 연결
- [[Research/소싱-트렌드-분석|📦 소싱 트렌드 분석]] — TikTok 상품 선정 기준
- [[Research/비즈니스-방향|🧭 비즈니스 방향]] — 전략 선택지
- [[Tasks/할일-보드|📋 할일 보드]] — TikTok 관련 할일

## 서버 파일 경로
- `tmp/selpix-tiktok/links.html`
- `tmp/selpix-saas/data/tiktok_products.json`
- `tmp/selpix-saas/data/register_queue.json` (tiktokContent 필드)
