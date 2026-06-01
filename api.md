# Doban API Notes

This file stores implementation notes for Korea Tourism Organization OpenAPI connectors used by Doban.

## 최종 MVP 선택 API 7개

도반 MVP에서 사용할 API는 아래 7개로 확정한다.

| 번호 | 제출 API명 | 데이터명 | 공공데이터포털 링크 |
|---:|---|---|---|
| 1 | 관광 빅데이터 정보 서비스 | 한국관광공사_빅데이터_지역별 방문자수_GW | https://www.data.go.kr/data/15101972/openapi.do |
| 2 | 지역별 관광 수요 강도 정보 서비스 | 한국관광공사_지역별 관광 수요 강도 | https://www.data.go.kr/data/15151868/openapi.do |
| 3 | 웰니스 관광정보 서비스 | 한국관광공사_웰니스관광정보 | https://www.data.go.kr/data/15144030/openapi.do |
| 4 | 관광지별 연관관광지 정보 서비스 | 한국관광공사_관광지별 연관 관광지 정보 | https://www.data.go.kr/data/15128560/openapi.do |
| 5 | 지역별 관광 자원 수요 정보 서비스 | 한국관광공사_지역별 관광 자원 수요 | https://www.data.go.kr/data/15152138/openapi.do |
| 6 | 기초지자체 중심관광지 정보 서비스 | 한국관광공사_기초지자체 중심 관광지 정보 | https://www.data.go.kr/data/15128559/openapi.do |
| 7 | 국문 관광정보 서비스 | 한국관광공사_국문 관광정보 서비스_GW | https://www.data.go.kr/data/15101578/openapi.do |

## 한국관광공사_국문 관광정보 서비스_GW

- Public data portal URL: https://www.data.go.kr/data/15101578/openapi.do
- Submit API name: 국문 관광정보 서비스
- Service host: `https://apis.data.go.kr/B551011/KorService2`
- Doban role: 사용자가 행정구역명이 아니라 관광지명/상권명을 입력했을 때 키워드 검색으로 관광지의 주소, 좌표, 콘텐츠 ID를 확인하는 보조 API
- Data caution: KorService2의 관광정보 코드 체계와 데이터랩 계열 `areaCd`, `signguCd` 체계를 바로 동일하게 취급하지 않는다. 키워드 검색 결과의 `addr1`, `addr2`, 법정동 정보, 좌표를 확인한 뒤 도반의 지역 코드 CSV로 다시 매핑한다.

### Recommended Operations for Doban

```text
GET /searchKeyword2
키워드 검색

GET /detailCommon2
공통정보 조회

GET /detailImage2
이미지정보 조회

GET /areaCode2
지역코드 조회

GET /ldongCode2
법정동코드 조회
```

Common query parameters:

| Parameter | Required | Example | Description |
|---|---:|---|---|
| serviceKey | Yes | API key | 인증키 |
| MobileOS | Yes | ETC | OS 구분 |
| MobileApp | Yes | Doban | 서비스명/앱명 |
| _type | No | json | JSON 응답 요청 |
| numOfRows | No | 10 | 한 페이지 결과 수 |
| pageNo | No | 1 | 페이지 번호 |

`searchKeyword2` frequently used parameters:

| Parameter | Required | Example | Description |
|---|---:|---|---|
| keyword | Yes | 전주 한옥마을 | 검색어 |
| arrange | No | A | 정렬 구분 |

Useful response fields:

| Field | Meaning |
|---|---|
| contentid | 콘텐츠 ID |
| contenttypeid | 콘텐츠 타입 ID |
| title | 콘텐츠 제목 |
| addr1, addr2 | 주소/상세주소 |
| mapx, mapy | 경도/위도 |
| firstimage, firstimage2 | 대표 이미지 |
| tel | 전화번호 |

### Doban Interpretation Rules

- 사용자가 관광지명이나 상권명을 입력하면 `searchKeyword2`로 먼저 장소 후보를 찾는다.
- 검색 결과가 여러 개이면 제목과 주소를 기준으로 가장 관련 높은 후보를 선택하되, 확신이 낮으면 사용자에게 확인 질문을 한다.
- 검색 결과의 주소가 확인되면 `region_name_lookup_rag.csv` 또는 `region_codes_rag.csv`를 사용해 데이터랩 API용 `areaCd`, `signguCd`로 변환한다.
- `searchKeyword2` 결과만으로 창업 판단을 하지 않는다. 장소 확인용으로 사용하고, 창업 분석은 방문자수/수요강도/자원수요/중심관광지/연관관광지 API와 조합한다.

## 한국관광공사_빅데이터_지역별 방문자수_GW

- Public data portal URL: https://www.data.go.kr/data/15101972/openapi.do
- Submit API name: 관광 빅데이터 정보 서비스
- Doban role: 지역별 방문자 규모와 흐름을 확인해 창업 아이템 추천의 기본 수요 신호로 사용
- Data caution: 이 API의 "방문자"는 관광객과 동일한 의미로 단정하면 안 된다. 거주/통근/통학 등 일상생활권을 벗어나 일정 시간 머문 사람 기준이며, 방문 목적을 정확히 알 수 없는 한계가 있다.

### Service Host

```text
https://apis.data.go.kr/B551011/DataLabService
```

Swagger also lists the gateway operation URL:

```text
https://gwapi.visitkorea.or.kr/openapi/service/gwrest/DataLabService/locgoRegnVisitrDDList
```

For Ennoia tests, prefer the `apis.data.go.kr` host first unless the connector template uses the gateway URL.

### Operations

#### 1. 광역 지자체 지역방문자수 집계 데이터 정보 조회

```text
GET /metcoRegnVisitrDDList
```

Use when Doban needs province/metropolitan-level visitor data.

Required query parameters:

| Parameter | Required | Example | Description |
|---|---:|---|---|
| serviceKey | Yes | API key | 인증키 |
| MobileOS | Yes | ETC | OS 구분: IOS, AND, WIN, ETC |
| MobileApp | Yes | Doban | 서비스명/앱명 |
| startYmd | Yes | 20240501 | 시작연월일, YYYYMMDD |
| endYmd | Yes | 20240531 | 종료연월일, YYYYMMDD |
| numOfRows | No | 10 | 한 페이지 결과 수 |
| pageNo | No | 1 | 페이지 번호 |

Useful response fields:

| Field | Meaning |
|---|---|
| resultCode | API 호출 결과 상태 코드 |
| resultMsg | API 호출 결과 상태 메시지 |
| totalCount | 전체 데이터 수 |
| baseYmd | 기준연월일 |
| areaCode | 시도코드 |
| areaNm | 시도명 |
| daywkDivCd | 요일구분코드 |
| daywkDivNm | 요일구분명 |
| touDivCd | 관광객구분코드 |
| touDivNm | 관광객구분명 |
| touNum | 관광객수 |

#### 2. 기초 지자체 지역방문자수 집계 데이터 정보 조회

```text
GET /locgoRegnVisitrDDList
```

Use this as Doban's primary visitor-count operation because startup analysis usually needs city/county/district-level signals.

Required query parameters:

| Parameter | Required | Example | Description |
|---|---:|---|---|
| serviceKey | Yes | API key | 인증키 |
| MobileOS | Yes | ETC | OS 구분: IOS, AND, WIN, ETC |
| MobileApp | Yes | Doban | 서비스명/앱명 |
| startYmd | Yes | 20240501 | 시작연월일, YYYYMMDD |
| endYmd | Yes | 20240531 | 종료연월일, YYYYMMDD |
| numOfRows | No | 10 | 한 페이지 결과 수 |
| pageNo | No | 1 | 페이지 번호 |

Useful response fields:

| Field | Meaning |
|---|---|
| resultCode | API 호출 결과 상태 코드 |
| resultMsg | API 호출 결과 상태 메시지 |
| pageNo | 현재 페이지 번호 |
| totalCount | 전체 데이터 수 |
| numOfRows | 한 페이지 결과 수 |
| baseYmd | 기준연월일 |
| signguCode | 시군구코드 |
| signguNm | 시군구명 |
| daywkDivCd | 요일구분코드 |
| daywkDivNm | 요일구분명 |
| touDivCd | 관광객구분코드 |
| touDivNm | 관광객구분명 |
| touNum | 관광객수 |

### Ennoia Test Template

Use this shape when registering or testing the connector.

```text
Method: GET
Base URL: https://apis.data.go.kr/B551011/DataLabService
Path: /locgoRegnVisitrDDList
Query:
  serviceKey: {serviceKey}
  MobileOS: ETC
  MobileApp: Doban
  startYmd: 20240501
  endYmd: 20240531
  numOfRows: 10
  pageNo: 1
```

### Doban Interpretation Rules

- Treat `touNum` as visitor count, not confirmed tourist count.
- Do not sum basic local government and metropolitan government counts together because their aggregation criteria differ.
- Use date range and weekday fields when explaining seasonality or weekday/weekend differences.
- Use visitor count as one demand signal only; combine it with tourism demand intensity, diversity, resource demand, and center/related attraction APIs before recommending a startup idea.
- If `totalCount` is 0 or `items` is empty, say that visitor data could not be confirmed for the selected date range and ask the user to try another period or region.

## 한국관광공사_지역별 관광 수요 강도

- Public data portal URL: https://www.data.go.kr/data/15151868/openapi.do
- Submit API name: 지역별 관광 수요 강도 정보 서비스
- Service host: `https://apis.data.go.kr/B551011/AreaTarDemDsService`
- Doban role: 체류 강도와 소비 강도를 근거로 창업 아이템의 수요 가능성을 판단

### Operations

```text
GET /areaTarSjrnDsList
지역별 관광 체류 강도 정보 목록 조회

GET /areaTarExpDsList
지역별 관광 소비 강도 정보 목록 조회
```

Common required query parameters:

| Parameter | Required | Example | Description |
|---|---:|---|---|
| serviceKey | Yes | API key | 인증키 |
| MobileOS | Yes | ETC | OS 구분: IOS, AND, WEB, ETC |
| MobileApp | Yes | Doban | 서비스명/앱명 |
| baseYm | Yes | 202509 | 조회 기준 연월, YYYYMM |
| areaCd | Yes | 11 | 지역 코드 |
| signguCd | No | 11530 | 시군구 코드 |
| numOfRows | No | 10 | 한 페이지 결과 수 |
| pageNo | No | 1 | 페이지 번호 |
| _type | No | json | JSON 응답 요청 |

Operation-specific optional parameters:

| Operation | Parameter | Codes |
|---|---|---|
| areaTarSjrnDsList | tarSjrnDsIxCd | 21 전체, 2101 타권역 방문자 비중, 2102 숙박 비중, 2103 1박 방문자수, 2104 2박 방문자수, 2105 3박 방문자수 |
| areaTarExpDsList | tarExpDsIxCd | 22 전체, 2201 외지인 소비액, 2202 전체 대비 외지인 소비액 비중, 2203 방문량 대비 방문 소비액 |

Useful response fields:

| Field | Meaning |
|---|---|
| baseYm | 기준 연월 |
| areaCd, areaNm | 지역 코드/지역명 |
| signguCd, signguNm | 시군구 코드/시군구명 |
| tarSjrnDsIxCd, tarSjrnDsIxNm, tarSjrnDsIxVal | 관광 체류 강도 지표 코드/명/값 |
| tarExpDsIxCd, tarExpDsIxNm, tarExpDsIxVal | 관광 소비 강도 지표 코드/명/값 |

### Doban Interpretation Rules

- 체류 강도가 높으면 숙박, 야간 소비, 체험형 콘텐츠 기회를 검토한다.
- 소비 강도가 높으면 식음료, 쇼핑, 로컬 상품, 유료 체험 아이템의 근거로 쓸 수 있다.
- 단일 지표값만으로 창업 적합성을 단정하지 말고 방문자수, 자원 수요, 중심/연관 관광지와 함께 해석한다.

## 한국관광공사_웰니스관광정보

- Public data portal URL: https://www.data.go.kr/data/15144030/openapi.do
- Submit API name: 웰니스 관광정보 서비스
- Service host: `https://apis.data.go.kr/B551011/WellnessTursmService`
- Doban role: MVP 핵심 API는 아니지만, 웰니스/힐링 관련 창업 아이템을 보강할 때 지역 내 웰니스 콘텐츠 후보를 확인하는 보조 API

### Operations

```text
GET /ldongCode
법정동 코드 조회

GET /areaBasedList
지역기반 관광정보 조회

GET /locationBasedList
위치기반 관광정보 조회

GET /searchKeyword
키워드 검색

GET /detailCommon
공통정보 조회

GET /detailIntro
소개정보 조회

GET /detailInfo
반복정보 조회

GET /detailImage
이미지정보 조회

GET /wellnessTursmSyncList
웰니스 관광 동기화 목록 조회
```

Common required query parameters:

| Parameter | Required | Example | Description |
|---|---:|---|---|
| serviceKey | Yes | API key | 인증키 |
| MobileOS | Yes | ETC | OS 구분: IOS, AND, WIN, ETC |
| MobileApp | Yes | Doban | 서비스명/앱명 |
| langDivCd | Yes | KOR | 언어 구분 코드. 한국 사용자 대상이면 KOR |
| numOfRows | No | 10 | 한 페이지 결과 수 |
| pageNo | No | 1 | 페이지 번호 |
| _type | No | json | JSON 응답 요청 |

Frequently useful optional parameters:

| Operation | Parameter | Meaning |
|---|---|---|
| areaBasedList | lDongRegnCd, lDongSignguCd | 법정동 시도/시군구 코드 |
| areaBasedList | wellnessThemaCd | 웰니스 테마 코드 |
| locationBasedList | mapX, mapY, radius | 좌표와 반경, radius max 20000m |
| searchKeyword | keyword | 검색 키워드 |
| detailCommon/detailIntro/detailInfo/detailImage | contentId, contentTypeId | 콘텐츠 상세 조회 |

Useful response fields:

| Field | Meaning |
|---|---|
| contentId, contentTypeId | 콘텐츠 ID/타입 |
| title | 콘텐츠 제목 |
| baseAddr, detailAddr | 주소/상세주소 |
| mapX, mapY | 경도/위도 |
| tel | 전화번호 |
| wellnessThemaCd | 웰니스 테마 코드 |
| orgImage, thumbImage | 원본/썸네일 이미지 |

Wellness theme codes:

| Code | Meaning |
|---|---|
| EX050100 | 온천/사우나/스파 |
| EX050200 | 찜질방 |
| EX050300 | 한방 체험 |
| EX050400 | 힐링 명상 |
| EX050500 | 뷰티 스파 |
| EX050600 | 기타 웰니스 |
| EX050700 | 자연 치유 |

## 한국관광공사_관광지별 연관 관광지 정보

- Public data portal URL: https://www.data.go.kr/data/15128560/openapi.do
- Submit API name: 관광지별 연관관광지 정보 서비스
- Service host: `https://apis.data.go.kr/B551011/TarRlteTarService1`
- Doban role: 중심 관광지 주변의 연계 방문 동선과 업종 기회를 분석
- Data caution: Tmap 내비게이션 기반 차량 이동 데이터이므로 실제 도보 동선이나 전체 방문자 흐름과 동일하다고 단정하지 않는다.

### Operations

```text
GET /areaBasedList1
지역기반 관광지별 연관 관광지 정보 목록 조회

GET /searchKeyword1
키워드 검색 관광지별 연관 관광지 정보 목록 조회
```

Common required query parameters:

| Parameter | Required | Example | Description |
|---|---:|---|---|
| serviceKey | Yes | API key | 인증키 |
| pageNo | Yes | 1 | 페이지 번호 |
| numOfRows | Yes | 10 | 한 페이지 결과 수 |
| MobileOS | Yes | ETC | OS 구분 |
| MobileApp | Yes | Doban | 서비스명/앱명 |
| baseYm | Yes | 202503 | 기준 연월, YYYYMM |
| areaCd | Yes | 51 | 관광지 지역 코드 |
| signguCd | Yes | 51130 | 관광지 시군구 코드 |
| keyword | searchKeyword1 only | 뮤지엄산 | 관광지명 키워드 |
| _type | No | json | JSON 응답 요청 |

Useful response fields:

| Field | Meaning |
|---|---|
| baseYm | 기준연월 |
| tAtsCd, tAtsNm | 기준 관광지 코드/명 |
| areaCd, areaNm, signguCd, signguNm | 기준 관광지 지역/시군구 |
| rlteRank | 연관 순위 |
| rlteTatsCd, rlteTatsNm | 연관 관광지 코드/명 |
| rlteRegnCd, rlteRegnNm, rlteSignguCd, rlteSignguNm | 연관 관광지 지역/시군구 |
| rlteCtgryLclsNm, rlteCtgryMclsNm, rlteCtgrySclsNm | 연관 관광지 분류 |

### Doban Interpretation Rules

- `rlteRank`가 높은 장소는 기준 관광지와 함께 이동되는 가능성이 높은 후보로 해석한다.
- 음식/숙박/관광 유형별 연관지를 보고 창업 아이템을 동선에 맞춰 설명한다.
- 차량 기반 데이터라는 한계를 명시하고, 실제 입지는 임대료/보행 동선/상권 조사를 추가 확인하도록 안내한다.

## 한국관광공사_지역별 관광 자원 수요

- Public data portal URL: https://www.data.go.kr/data/15152138/openapi.do
- Submit API name: 지역별 관광 자원 수요 정보 서비스
- Service host: `https://apis.data.go.kr/B551011/AreaTarResDemService`
- Doban role: 식음료, 쇼핑, 숙박, 체험, 레포츠, 자연/역사/문화 관광 등 어떤 자원 수요가 강한지 판단

### Operations

```text
GET /areaTarSvcDemList
지역별 관광 서비스 수요 정보 목록 조회

GET /areaCulResDemList
지역별 문화 자원 수요 정보 목록 조회
```

Common required query parameters:

| Parameter | Required | Example | Description |
|---|---:|---|---|
| serviceKey | Yes | API key | 인증키 |
| MobileOS | Yes | ETC | OS 구분: IOS, AND, WEB, ETC |
| MobileApp | Yes | Doban | 서비스명/앱명 |
| baseYm | Yes | 202509 | 조회 기준 연월, YYYYMM |
| areaCd | Yes | 11 | 지역 코드 |
| signguCd | No | 11530 | 시군구 코드 |
| numOfRows | No | 10 | 한 페이지 결과 수 |
| pageNo | No | 1 | 페이지 번호 |
| _type | No | json | JSON 응답 요청 |

Operation-specific optional parameters:

| Operation | Parameter | Codes |
|---|---|---|
| areaTarSvcDemList | tarSvcDemIxCd | 11 전체, 1101 레포츠 SNS, 1102 휴식/힐링 SNS, 1103 미식 SNS, 1104 체험 SNS, 1105 쇼핑업 소비액, 1106 식음료 소비액, 1107 숙박업 소비액, 1108 여가 서비스업 소비액, 1109 운송업 소비액, 1110 숙박 검색량, 1111 음식 검색량, 1112 쇼핑 검색량 |
| areaCulResDemList | culResDemIxCd | 12 전체, 1201 문화 관광 검색량, 1202 레저 스포츠 검색량, 1203 역사 관광 검색량, 1204 체험 관광 검색량, 1205 자연 관광 검색량 |

Useful response fields:

| Field | Meaning |
|---|---|
| tarSvcDemIxCd, tarSvcDemIxNm, tarSvcDemIxVal | 관광 서비스 수요 코드/명/값 |
| culResDemIxCd, culResDemIxNm, culResDemIxVal | 문화 자원 수요 코드/명/값 |
| baseYm | 기준 연월 |
| areaCd, areaNm | 지역 코드/명 |
| signguCd, signguNm | 시군구 코드/명 |

### Doban Interpretation Rules

- 식음료 소비액/음식 검색량/미식 SNS가 높으면 F&B 아이템 근거로 사용한다.
- 숙박 소비액/숙박 검색량이 높으면 숙박 연계 서비스, 야간 콘텐츠, 체크인 전후 소비 아이템을 검토한다.
- 체험/자연/역사/문화 수요가 높으면 원데이 클래스, 투어, 굿즈, 해설형 콘텐츠 아이템을 검토한다.

## 한국관광공사_기초지자체 중심 관광지 정보

- Public data portal URL: https://www.data.go.kr/data/15128559/openapi.do
- Submit API name: 기초지자체 중심관광지 정보 서비스
- Service host: `https://apis.data.go.kr/B551011/LocgoHubTarService1`
- Doban role: 지역 내 관광객 이동의 핵심 거점 후보를 찾고, 창업 입지/동선 분석의 시작점으로 사용
- Data caution: Tmap 내비게이션 기반 차량 이동 데이터이므로 실제 도보 동선이나 전체 방문자 흐름과 동일하다고 단정하지 않는다.

### Operations

```text
GET /areaBasedList1
지역기반 중심 관광지 정보 목록 조회
```

Required query parameters:

| Parameter | Required | Example | Description |
|---|---:|---|---|
| serviceKey | Yes | API key | 인증키 |
| pageNo | Yes | 1 | 페이지 번호 |
| numOfRows | Yes | 10 | 한 페이지 결과 수 |
| MobileOS | Yes | ETC | OS 구분 |
| MobileApp | Yes | Doban | 서비스명/앱명 |
| baseYm | Yes | 202503 | 기준 연월, YYYYMM |
| areaCd | Yes | 11 | 중심지 지역 코드 |
| signguCd | Yes | 11530 | 중심지 시군구 코드 |
| _type | No | json | JSON 응답 요청 |

Useful response fields:

| Field | Meaning |
|---|---|
| hubRank | 중심지 순위 |
| hubTatsCd, hubTatsNm | 중심지 관광지 코드/명 |
| hubCtgryLclsNm, hubCtgryMclsNm | 중심지 카테고리 대분류/중분류 |
| mapX, mapY | 좌표 |
| baseYm | 기준연월 |
| areaCd, areaNm, signguCd, signguNm | 지역/시군구 코드와 이름 |

### Doban Interpretation Rules

- `hubRank`가 높은 관광지는 해당 지역의 관광 동선 시작점 또는 거점 후보로 사용한다.
- 중심 관광지 분류를 바탕으로 주변에 어울리는 업종을 제안한다.
- 연관 관광지 API와 함께 사용하면 기준 관광지와 다음 방문지를 묶어 동선 기반 아이템을 만들 수 있다.
