# openclaw 메모리 표준

## 목적

- `memory/YYYY-MM-DD.md`: 날짜별 raw 로그(업무 이벤트/결정/점검)
- `MEMORY.md`: 장기 핵심 메모(주제/판단 기준/교훈)
- 검색은 `scripts/memory_search.sh <query>`로 통합.

## Daily 파일 표준 형식

각 `memory/YYYY-MM-DD.md`는 다음 형태를 기본으로 합니다.

```
# YYYY-MM-DD 메모

## [N] 주제
- HH:MM [N] 핵심 내용
  - 세부 내용

## [C] 커뮤니케이션
- HH:MM [C] 요청/질문/의사결정

## [H] 하트비트/점검
- HH:MM [H] 점검 항목/결과

## [L] 학습/교훈
- HH:MM [L] 시행착오/교훈
```

### 태그 규칙

- `[N]`: 일반 노트
- `[C]`: 커뮤니케이션/컨텍스트
- `[H]`: 하트비트/정기 점검
- `[L]`: 학습/교훈

필요 시 섹션명은 다르게 써도 되지만, 태그는 `[N/H/C/L]` 형식을 유지합니다.

## 작성 도구

- `memory_append.sh`: 오늘 파일에 규격 엔트리 추가
- `scripts/memory_search.sh`: 의미 기반 없는 대신 즉시 텍스트 검색

