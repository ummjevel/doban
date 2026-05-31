# 엔노이아 문서 폴더 업로드용 CSV

이 폴더의 CSV는 엔노이아 문서 폴더 규칙에 맞춰 재가공한 파일이다. 첫 번째 열은 모두 `ID`이며, ID는 파일 안에서 중복되지 않는다.

## 업로드 추천 파일

1. `region_name_lookup_rag.csv`
2. `area_codes_rag.csv`
3. `indicator_codes_rag.csv`
4. `region_codes_rag.csv`

## 중복처럼 보이는 값 설명

`region_name_lookup_rag.csv`에는 `query_name` 또는 `query_name_normalized`가 같은 행이 여러 개 있을 수 있다. 이는 오류가 아니라 같은 시군구명이 여러 시도에 존재하기 때문이다. 예: `중구`, `강서구`, `동구` 등.

이 경우 `is_ambiguous=Y`, `candidate_count`에 후보 개수를 표시했다. 챗봇은 모호한 지역명만 입력되면 시도명을 추가로 물어봐야 한다.

## 생성 파일 행 수

- `area_codes_rag.csv`: 17행
- `indicator_codes_rag.csv`: 36행
- `region_codes_rag.csv`: 252행
- `region_name_lookup_rag.csv`: 1948행
