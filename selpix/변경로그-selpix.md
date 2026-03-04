---
type: changelog
status: active
tags: [selpix, saas, changelog]
last_updated: 2026-03-05
source_agent: claude-code
confidence_level: high
note_status: permanent
generated_via: sessions-spawn
verified_by: agent
sources: []
created: 2026-03-05
reviewed_at: 2026-03-05
---

# Selpix SaaS 변경로그

> OpenClaw 변경로그와 분리된 Selpix 전용 기록

---

### 2026-03-05 04:30 — 소싱 페이지 버그 수정 + 보안 강화 + 미구현 감사

#### 1. 소싱 페이지 버그 수정 (3건)

**1-1. 소싱 검색 에러 무시 → 에러 배너 표시**
- **변경**: `sourcing/page.tsx` — `searchError` state 추가, catch에서 API 에러 메시지 파싱, 빨간 배너 + 재시도 버튼 표시
- **변경**: `api/sourcing/route.ts` — proxy 실패 catch에 `console.error` 추가
- **사유**: 503/500 에러도 "검색 결과가 없습니다"로만 보여 사용자 혼란
- **영향**: 소싱 검색 UX — 에러와 빈 결과 구분 가능
- **롤백**: `searchError` state + 관련 JSX 제거

**1-2. 자격증명 로딩 불일치 → 엔드포인트 통일**
- **변경**: `sourcing/page.tsx` — 마운트 시 credential 체크를 `/api/coupang` → `/api/stores`로 변경
- **변경**: `credential-manager.ts` — `loadCredentials` catch에 `console.error` 추가
- **사유**: 설정 페이지는 `/api/stores` (StoreCredential 테이블), 소싱 페이지는 `/api/coupang` (CoupangCredential 테이블) — 두 시스템 불일치
- **영향**: 설정에서 키 저장 후 소싱에서 "키 없음" 표시되던 문제 해결
- **롤백**: `/api/coupang` GET 호출로 복원

**1-3. 이미지 사용 불가 시 등록 차단**
- **변경**: `register-step-product.tsx` — `enrichData?.imageUsageStatus === "unavailable"` 시 경고 배너 + "다음" 버튼 disabled
- **사유**: 이미지 사용 불가 상품이 등록 진행 가능했음
- **영향**: 등록 모달 — 이미지 불가 시 진행 차단
- **롤백**: disabled 조건에서 imageUsageStatus 제거

#### 2. Credential 이중 시스템 통일 (3건)

**배경**: `CoupangCredential` (레거시)과 `StoreCredential` (신규)이 혼재. 설정 페이지는 StoreCredential만 사용하는데, 등록/삭제/크론은 CoupangCredential만 읽음.

- **변경**: `api/coupang/listing/route.ts` — StoreCredential 우선 조회, 없으면 CoupangCredential fallback
- **변경**: `api/coupang/listing/[id]/route.ts` — DELETE 핸들러도 동일 fallback 적용
- **변경**: `api/cron/listing-dispatcher/route.ts` — 크론 배치도 동일 fallback 적용
- **사유**: 신규 사용자가 설정 후 등록/삭제 시 "키 없음" 에러 발생
- **영향**: 쿠팡 등록, 삭제, 크론 디스패치 전부 두 시스템 호환
- **롤백**: fallback 로직 제거, CoupangCredential만 사용으로 복원

#### 3. 보안 취약점 수정 (Critical 4건 + Medium 4건)

**Critical:**
- `competitor/search/route.ts` — `count` 파라미터 1~100 제한 (무제한이었음)
- `cron/billing-charge/route.ts` — timing-safe 비교로 변경 (`!==` → `crypto.timingSafeEqual`)
- `listing/multi/route.ts` — 일일 할당량(10건) 체크 추가 (우회 가능했음)
- Rate Limiting 구현:
  - 신규 파일: `lib/utils/rate-limit.ts` (인메모리 sliding-window)
  - 신규 파일: `lib/utils/with-rate-limit.ts` (API route 래퍼)
  - 적용: `trends/analyze`, `trends/route`, `trends/surge`, `benchmark/analyze`, `ai/optimize-listing`, `ai/generate-descriptions`, `sourcing/route`, `competitor/search` (8개 route)

**Medium (입력 검증):**
- `competitor/track/route.ts` — keyword 100자 제한 + condition allowlist
- `ab-test/route.ts` — name/productName 200자 + productInfo 10KB 제한
- `ab-test/[id]/metrics/route.ts` — 숫자 필드 clamp (0~10M)
- `benchmark/analyze/route.ts` — competitorUrls 최대 10개 제한

#### 4. 미구현 기능 감사 결과 (수정 예정 — 내일)

| # | 심각도 | 문제 | 상태 |
|---|--------|------|------|
| 1 | Critical | 결제 플로우 비활성 (모든 유료 플랜 "출시 예정") | 미수정 |
| 2 | Critical | Toss 결제 페이지 orphaned (링크 없음) | 미수정 |
| 3 | Critical | CTA 뉴스레터 API 미호출 (TODO 주석만) | 미수정 |
| 4 | Critical | 네이버 스마트스토어 OAuth 미완성 | 미수정 |
| 5 | Critical | 토스샵 API URL 플레이스홀더 | 미수정 |
| 6 | Critical | `via.placeholder.com` 폴백 이미지 → 쿠팡 거부 | 미수정 |
| 7 | Critical | `@placeholder.com` 이메일 → 영수증 유실 | 미수정 |
| 8 | Nice | 경쟁사 모니터링 UI 없음 (API만 완성) | 미수정 |
| 9 | Nice | 대시보드 error boundary 없음 | 미수정 |

---

## 수정 파일 전체 목록

| 파일 | 변경 유형 |
|------|-----------|
| `src/app/(dashboard)/sourcing/page.tsx` | 에러 UI + credential 통일 |
| `src/app/api/sourcing/route.ts` | proxy 로깅 + rate limit |
| `src/lib/stores/credential-manager.ts` | decrypt 로깅 |
| `src/components/listing/register-step-product.tsx` | 이미지 불가 차단 |
| `src/app/api/coupang/listing/route.ts` | credential fallback |
| `src/app/api/coupang/listing/[id]/route.ts` | credential fallback |
| `src/app/api/cron/listing-dispatcher/route.ts` | credential fallback |
| `src/app/api/competitor/search/route.ts` | count cap + rate limit |
| `src/app/api/cron/billing-charge/route.ts` | timing-safe |
| `src/app/api/listing/multi/route.ts` | quota check |
| `src/app/api/trends/analyze/route.ts` | rate limit |
| `src/app/api/trends/route.ts` | rate limit |
| `src/app/api/trends/surge/route.ts` | rate limit |
| `src/app/api/benchmark/analyze/route.ts` | rate limit + URL cap |
| `src/app/api/ai/optimize-listing/route.ts` | rate limit |
| `src/app/api/ai/generate-descriptions/route.ts` | rate limit |
| `src/app/api/competitor/track/route.ts` | 입력 검증 |
| `src/app/api/ab-test/route.ts` | 입력 검증 |
| `src/app/api/ab-test/[id]/metrics/route.ts` | 숫자 clamp |
| `src/lib/utils/rate-limit.ts` | **신규** |
| `src/lib/utils/with-rate-limit.ts` | **신규** |
