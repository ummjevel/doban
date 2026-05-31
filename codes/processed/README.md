# 코드값 CSV 사용 안내

이 폴더는 도반(Doban) 챗봇과 엔노이아 API 커넥터에서 쓰기 쉽도록 원본 코드 파일을 가공한 결과물이다.

## 파일 목록

### `region_codes_for_api.csv`

시군구 코드 기본 테이블.

주요 컬럼:

- `areaCd`: 시도 코드
- `areaNm`: 시도명
- `signguCd`: 시군구 코드. API 파라미터명 기준
- `signguNm`: 시군구명. API 파라미터명 기준
- `sigunguCd`, `sigunguNm`: 원본 파일 표기 호환용
- `fullRegionNm`: 시도명 + 시군구명
- `areaNmNormalized`, `signguNmNormalized`, `fullRegionNmNormalized`: 공백 제거 검색용 값

### `area_codes_for_api.csv`

시도 코드와 시도명 별칭 테이블.

사용 예:

- 사용자가 `서울`, `서울시`, `서울특별시`라고 입력하면 `areaCd=11`로 매핑
- 사용자가 시군구 없이 시도명만 입력하면 시군구를 추가로 물어보는 것이 안전함

### `region_name_lookup_for_chatbot.csv`

사용자 입력 지역명을 API 코드로 바꾸기 위한 챗봇용 lookup 테이블.

주요 컬럼:

- `queryName`: 사용자가 입력할 수 있는 지역명 별칭
- `queryNameNormalized`: 공백 제거 검색용 값
- `areaCd`, `areaNm`: 시도 코드/시도명
- `signguCd`, `signguNm`: 시군구 코드/시군구명
- `fullRegionNm`: 전체 지역명
- `matchType`: `area_sigungu` 또는 `sigungu_only`
- `matchNote`: 매칭 주의사항

주의:

- `서울` 같은 시도명 단독 입력은 이 파일에서 특정 구로 바로 매핑하지 않는다.
- 시도명 단독 입력은 `area_codes_for_api.csv`로 확인한 뒤 시군구를 추가 질문한다.
- `중구`, `강서구`처럼 여러 시도에 중복될 수 있는 시군구명은 `matchType=sigungu_only`로 표시된다.

### `indicator_codes_for_api.csv`

API 요청 파라미터에 들어가는 지표 코드값 테이블.

예:

- `tarSjrnDsIxCd`: 관광 체류 강도 지표 코드
- `tarExpDsIxCd`: 관광 소비 강도 지표 코드
- `tarSvcDemIxCd`: 관광 서비스 수요 지표 코드
- `culResDemIxCd`: 문화 자원 수요 지표 코드
- `wellnessThemaCd`: 웰니스 테마 코드

사용 예:

- 숙박/체류 관련 분석: `tarSjrnDsIxCd=2102`
- 식음료 창업 분석: `tarSvcDemIxCd=1106` 또는 `tarSvcDemIxCd=1111`
- 웰니스 자연 치유 분석: `wellnessThemaCd=EX050700`

## 엔노이아 첨부 추천

우선 첨부할 파일:

1. `region_name_lookup_for_chatbot.csv`
2. `area_codes_for_api.csv`
3. `indicator_codes_for_api.csv`

보조로 첨부할 파일:

4. `region_codes_for_api.csv`

