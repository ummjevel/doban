# 도반(Doban) OpenAPI 목록

목적: 도반에서 검토/연결한 한국관광공사 OpenAPI와 엔노이아 커넥터 테스트 상태를 관리한다.

## 상태 기준

- 신청/키 확인: 공공데이터포털 활용 신청 또는 서비스키 확인 완료
- 엔노이아 등록 완료: 엔노이아 API 커넥터에 등록 완료
- 기본값 호출 성공: 챗봇 플로우 연결 전, API 키와 기본 테스트값으로 호출 성공
- 챗봇 연결 완료: 도반 챗봇 플로우에서 실제 호출 및 답변 생성에 사용
- 최종 사용: 제출할 엔노이아 앱에서 실제로 사용
- 제외: 검토 또는 테스트했지만 최종 앱에서는 사용하지 않음

## 제출 주의사항

최종 제출 폼에는 `최종 사용` 상태인 API만 선택한다. 엔노이아에 등록했거나 테스트만 한 API는 제출 API 리스트에 넣지 않는다.

## 최종 MVP 선택 API 7개

| 번호 | 제출 API명 | 공공데이터포털 링크 | 도반 활용 목적 |
|---:|---|---|---|
| 1 | 관광 빅데이터 정보 서비스 | https://www.data.go.kr/data/15101972/openapi.do | 지역별 방문자 규모와 관광 흐름 파악 |
| 2 | 지역별 관광 수요 강도 정보 서비스 | https://www.data.go.kr/data/15151868/openapi.do | 관광 체류 강도와 관광 소비 강도 분석 |
| 3 | 웰니스 관광정보 서비스 | https://www.data.go.kr/data/15144030/openapi.do | 웰니스/힐링 창업 아이템과 지역 콘텐츠 후보 확인 |
| 4 | 관광지별 연관관광지 정보 서비스 | https://www.data.go.kr/data/15128560/openapi.do | 중심 관광지와 연결되는 주변 관광 동선 분석 |
| 5 | 지역별 관광 자원 수요 정보 서비스 | https://www.data.go.kr/data/15152138/openapi.do | 식음료, 쇼핑, 숙박, 체험, 문화/자연 자원 수요 분석 |
| 6 | 기초지자체 중심관광지 정보 서비스 | https://www.data.go.kr/data/15128559/openapi.do | 지역 내 관광 동선의 중심 거점 파악 |
| 7 | 국문 관광정보 서비스 | https://www.data.go.kr/data/15101578/openapi.do | 관광지명 키워드 검색, 주소/좌표/contentId 확인 |

## API 체크리스트

| 우선순위 | 제출 API명 | 도반에서의 역할 | 엔노이아 상태 | 테스트 기준 | 비고 |
|---|---|---|---|---|---|
| P0 | 관광 빅데이터 정보 서비스 | 지역별 방문자 규모와 관광 흐름 파악 | 기본값 호출 성공 | 기초/광역 지자체 방문자수 조회 | data.go.kr 15101972 |
| P0 | 지역별 관광 수요 강도 정보 서비스 | 체류/소비 강도 판단 | 기본값 호출 성공 | 체류 강도/소비 강도 조회 | data.go.kr 15151868 |
| P0 | 지역별 관광 자원 수요 정보 서비스 | 관광 서비스/문화 자원 수요 판단 | 기본값 호출 성공 | 서비스 수요/문화 자원 수요 조회 | data.go.kr 15152138 |
| P1 | 기초지자체 중심관광지 정보 서비스 | 지역 내 중심 관광지 파악 | 기본값 호출 성공 | 중심 관광지 목록 조회 | data.go.kr 15128559 |
| P1 | 관광지별 연관관광지 정보 서비스 | 중심 관광지와 연결되는 주변 동선 파악 | 기본값 호출 성공 | 지역/키워드 기반 연관 관광지 조회 | data.go.kr 15128560 |
| P0 | 웰니스 관광정보 서비스 | 웰니스/힐링 창업 아이템 분석 및 지역 콘텐츠 후보 확인 | 기본값 호출 성공 | 지역/위치/키워드 기반 웰니스 관광정보 조회 | data.go.kr 15144030. MVP 포함 |
| P2 | 지역별 관광 다양성 정보 서비스 | 관광객/소비/국제성 다양성 판단 | 미확인 | 다양성 지표 조회 | 링크/명세 확인 필요 |
| P0 | 국문 관광정보 서비스 | 관광지명 키워드 검색과 주소/좌표 확인 | 추가 예정 | `searchKeyword2` 키워드 검색 | KorService2. 관광지명 -> 주소/좌표 보조 API |
| P3 | 관광사진 정보 서비스 | 이미지 카드와 소개서/시연 화면 보강 | 미확인 | 관광사진 조회 | 선택 API |

## 추천 MVP API 세트

최종 MVP 선택 API:

- [x] 관광 빅데이터 정보 서비스
- [x] 지역별 관광 수요 강도 정보 서비스
- [x] 지역별 관광 자원 수요 정보 서비스
- [x] 기초지자체 중심관광지 정보 서비스
- [x] 관광지별 연관관광지 정보 서비스
- [x] 웰니스 관광정보 서비스
- [x] 국문 관광정보 서비스

이번 MVP에서는 제외 또는 보류:

- [ ] 지역별 관광 다양성 정보 서비스
- [ ] 관광사진 정보 서비스

## 엔노이아 커넥터 테스트 로그

### 1. 관광 빅데이터 정보 서비스

- 상태: 기본값 호출 성공
- 출처: https://www.data.go.kr/data/15101972/openapi.do
- 서비스명: 한국관광공사_빅데이터_지역별 방문자수_GW
- Base URL: `https://apis.data.go.kr/B551011/DataLabService`
- Endpoint:
  - `GET /metcoRegnVisitrDDList`: 광역 지자체 지역방문자수 집계 데이터 정보 조회
  - `GET /locgoRegnVisitrDDList`: 기초 지자체 지역방문자수 집계 데이터 정보 조회
- 필수 파라미터:
  - `serviceKey`: 인증키
  - `MobileOS`: OS 구분. 도반 기본값 `ETC`
  - `MobileApp`: 서비스명. 도반 기본값 `Doban`
  - `startYmd`: 시작연월일, `YYYYMMDD`
  - `endYmd`: 종료연월일, `YYYYMMDD`
- 선택 파라미터:
  - `numOfRows`: 한 페이지 결과 수
  - `pageNo`: 페이지 번호
- 응답 형식: JSON 또는 XML
- 주요 응답 필드:
  - `resultCode`, `resultMsg`
  - `totalCount`
  - `baseYmd`
  - `areaCode`, `areaNm`
  - `signguCode`, `signguNm`
  - `daywkDivCd`, `daywkDivNm`
  - `touDivCd`, `touDivNm`
  - `touNum`
- 해석 메모:
  - `touNum`은 방문자수 지표로 사용하고 실제 관광객 수와 동일하다고 단정하지 않는다.
  - 광역 지자체와 기초 지자체 수치를 임의 합산하지 않는다.
- 엔노이아 테스트 결과: API 키와 기본값으로 호출 완료
- 챗봇 연결 상태: 미연결

### 2. 지역별 관광 수요 강도 정보 서비스

- 상태: 기본값 호출 성공
- 출처: https://www.data.go.kr/data/15151868/openapi.do
- 서비스명: 한국관광공사_지역별 관광 수요 강도
- Base URL: `https://apis.data.go.kr/B551011/AreaTarDemDsService`
- Endpoint:
  - `GET /areaTarSjrnDsList`: 지역별 관광 체류 강도 정보 목록 조회
  - `GET /areaTarExpDsList`: 지역별 관광 소비 강도 정보 목록 조회
- 필수 파라미터:
  - `serviceKey`
  - `MobileOS`
  - `MobileApp`
  - `baseYm`: 기준 연월, `YYYYMM`
  - `areaCd`: 지역 코드
- 선택 파라미터:
  - `signguCd`
  - `tarSjrnDsIxCd`: 관광 체류 강도 지표 코드
  - `tarExpDsIxCd`: 관광 소비 강도 지표 코드
  - `numOfRows`
  - `pageNo`
  - `_type=json`
- 주요 응답 필드:
  - `baseYm`
  - `areaCd`, `areaNm`
  - `signguCd`, `signguNm`
  - `tarSjrnDsIxCd`, `tarSjrnDsIxNm`, `tarSjrnDsIxVal`
  - `tarExpDsIxCd`, `tarExpDsIxNm`, `tarExpDsIxVal`
- 도반 활용:
  - 체류 강도는 숙박/야간/체험형 아이템 근거로 사용
  - 소비 강도는 식음료/쇼핑/유료 체험 아이템 근거로 사용
- 엔노이아 테스트 결과: API 키와 기본값으로 호출 완료
- 챗봇 연결 상태: 미연결

### 3. 지역별 관광 자원 수요 정보 서비스

- 상태: 기본값 호출 성공
- 출처: https://www.data.go.kr/data/15152138/openapi.do
- 서비스명: 한국관광공사_지역별 관광 자원 수요
- Base URL: `https://apis.data.go.kr/B551011/AreaTarResDemService`
- Endpoint:
  - `GET /areaTarSvcDemList`: 지역별 관광 서비스 수요 정보 목록 조회
  - `GET /areaCulResDemList`: 지역별 문화 자원 수요 정보 목록 조회
- 필수 파라미터:
  - `serviceKey`
  - `MobileOS`
  - `MobileApp`
  - `baseYm`: 기준 연월, `YYYYMM`
  - `areaCd`: 지역 코드
- 선택 파라미터:
  - `signguCd`
  - `tarSvcDemIxCd`: 관광 서비스 수요 지표 코드
  - `culResDemIxCd`: 문화 자원 수요 지표 코드
  - `numOfRows`
  - `pageNo`
  - `_type=json`
- 주요 응답 필드:
  - `tarSvcDemIxCd`, `tarSvcDemIxNm`, `tarSvcDemIxVal`
  - `culResDemIxCd`, `culResDemIxNm`, `culResDemIxVal`
  - `baseYm`
  - `areaCd`, `areaNm`
  - `signguCd`, `signguNm`
- 도반 활용:
  - 미식, 식음료, 쇼핑, 숙박, 체험, 자연/역사/문화 수요에 맞춰 창업 아이템 후보를 좁힌다.
- 엔노이아 테스트 결과: API 키와 기본값으로 호출 완료
- 챗봇 연결 상태: 미연결

### 4. 기초지자체 중심관광지 정보 서비스

- 상태: 기본값 호출 성공
- 출처: https://www.data.go.kr/data/15128559/openapi.do
- 서비스명: 한국관광공사_기초지자체 중심 관광지 정보
- Base URL: `https://apis.data.go.kr/B551011/LocgoHubTarService1`
- Endpoint:
  - `GET /areaBasedList1`: 지역기반 중심 관광지 정보 목록 조회
- 필수 파라미터:
  - `serviceKey`
  - `pageNo`
  - `numOfRows`
  - `MobileOS`
  - `MobileApp`
  - `baseYm`: 기준 연월, `YYYYMM`
  - `areaCd`
  - `signguCd`
- 선택 파라미터:
  - `_type=json`
- 주요 응답 필드:
  - `hubRank`
  - `hubTatsCd`, `hubTatsNm`
  - `hubCtgryLclsNm`, `hubCtgryMclsNm`
  - `mapX`, `mapY`
  - `baseYm`
  - `areaCd`, `areaNm`, `signguCd`, `signguNm`
- 도반 활용:
  - 지역 내 관광 동선의 거점 후보로 사용
  - 연관 관광지 API와 함께 동선 기반 창업 아이템을 만든다.
- 엔노이아 테스트 결과: API 키와 기본값으로 호출 완료
- 챗봇 연결 상태: 미연결

### 5. 관광지별 연관관광지 정보 서비스

- 상태: 기본값 호출 성공
- 출처: https://www.data.go.kr/data/15128560/openapi.do
- 서비스명: 한국관광공사_관광지별 연관 관광지 정보
- Base URL: `https://apis.data.go.kr/B551011/TarRlteTarService1`
- Endpoint:
  - `GET /areaBasedList1`: 지역기반 관광지별 연관 관광지 정보 목록 조회
  - `GET /searchKeyword1`: 키워드 검색 관광지별 연관 관광지 정보 목록 조회
- 필수 파라미터:
  - `serviceKey`
  - `pageNo`
  - `numOfRows`
  - `MobileOS`
  - `MobileApp`
  - `baseYm`
  - `areaCd`
  - `signguCd`
  - `keyword`: `searchKeyword1`에서 필수
- 선택 파라미터:
  - `_type=json`
- 주요 응답 필드:
  - `baseYm`
  - `tAtsCd`, `tAtsNm`
  - `rlteRank`
  - `rlteTatsCd`, `rlteTatsNm`
  - `rlteRegnCd`, `rlteRegnNm`, `rlteSignguCd`, `rlteSignguNm`
  - `rlteCtgryLclsNm`, `rlteCtgryMclsNm`, `rlteCtgrySclsNm`
- 해석 메모:
  - Tmap 내비게이션 기반 차량 이동 데이터이므로 실제 도보 동선이나 전체 방문자 흐름으로 단정하지 않는다.
- 엔노이아 테스트 결과: API 키와 기본값으로 호출 완료
- 챗봇 연결 상태: 미연결

### 6. 웰니스 관광정보 서비스

- 상태: 기본값 호출 성공
- 출처: https://www.data.go.kr/data/15144030/openapi.do
- 서비스명: 한국관광공사_웰니스관광정보
- Base URL: `https://apis.data.go.kr/B551011/WellnessTursmService`
- Endpoint:
  - `GET /ldongCode`: 법정동 코드 조회
  - `GET /areaBasedList`: 지역기반 관광정보 조회
  - `GET /locationBasedList`: 위치기반 관광정보 조회
  - `GET /searchKeyword`: 키워드 검색
  - `GET /detailCommon`: 공통정보 조회
  - `GET /detailIntro`: 소개정보 조회
  - `GET /detailInfo`: 반복정보 조회
  - `GET /detailImage`: 이미지정보 조회
  - `GET /wellnessTursmSyncList`: 웰니스 관광 동기화 목록 조회
- 공통 필수 파라미터:
  - `serviceKey`
  - `MobileOS`
  - `MobileApp`
  - `langDivCd`: 한국 사용자 대상 기본값 `KOR`
- 주요 선택/추가 파라미터:
  - `lDongRegnCd`, `lDongSignguCd`
  - `wellnessThemaCd`
  - `mapX`, `mapY`, `radius`
  - `keyword`
  - `contentId`, `contentTypeId`
  - `_type=json`
- 주요 응답 필드:
  - `contentId`, `contentTypeId`
  - `title`
  - `baseAddr`, `detailAddr`
  - `mapX`, `mapY`
  - `tel`
  - `wellnessThemaCd`
  - `orgImage`, `thumbImage`
- 도반 활용:
  - 웰니스/힐링 관련 창업 아이템의 핵심 근거로 사용
  - 지역 내 웰니스 콘텐츠 후보, 테마, 위치, 이미지 정보를 미니 컨설팅 리포트에 반영
- 엔노이아 테스트 결과: API 키와 기본값으로 호출 완료
- 챗봇 연결 상태: 미연결

## 아직 미확인 또는 선택 후보

### 지역별 관광 다양성 정보 서비스

- 상태: 미확인
- 역할: 관광객 다양성, 소비 다양성, 국제적 다양성 판단
- 다음 작업:
  - data.go.kr 링크 확인
  - Swagger 명세 확인
  - 엔노이아 등록 여부 결정

### 국문 관광정보 서비스

- 상태: 추가 예정
- 출처: https://www.data.go.kr/data/15101578/openapi.do
- 서비스명: 한국관광공사_국문 관광정보 서비스_GW
- Base URL: `https://apis.data.go.kr/B551011/KorService2`
- 역할: 관광지명/상권명 키워드 검색, 주소/좌표/contentId 확인
- 주요 활용 endpoint:
  - `GET /searchKeyword2`: 키워드 검색
  - `GET /detailCommon2`: 공통정보 조회
  - `GET /detailImage2`: 이미지정보 조회
  - 필요 시 `GET /areaCode2`, `GET /ldongCode2`
- 기본 필수 파라미터:
  - `serviceKey`
  - `MobileOS`
  - `MobileApp`
  - `_type=json`
- 키워드 검색 추가 파라미터:
  - `keyword`
  - `numOfRows`
  - `pageNo`
  - `arrange`
- 주요 응답 필드:
  - `contentid`, `contenttypeid`
  - `title`
  - `addr1`, `addr2`
  - `mapx`, `mapy`
  - `firstimage`, `firstimage2`
- 주의:
  - KorService2의 관광정보 지역코드와 데이터랩 계열 `areaCd/signguCd`를 바로 동일하게 취급하지 않는다.
  - 키워드 검색 결과의 주소 또는 법정동 정보를 확인한 뒤 `region_name_lookup_rag.csv`/`region_codes_rag.csv`로 데이터랩용 코드를 매핑한다.
- 다음 작업:
  - 엔노이아 API 커넥터 등록 및 기본값 호출 테스트

### 관광사진 정보 서비스

- 상태: 미확인
- 역할: 이미지 카드, 소개서/시연 화면 보강
- 다음 작업:
  - 실제 MVP에 필요한지 결정

## 최종 제출 API 리스트 초안

아래 항목은 현재 MVP 선택 API다. 최종 제출 전에는 엔노이아 챗봇 플로우에서 실제 호출되는지 다시 확인한다.

- [x] 관광 빅데이터 정보 서비스
- [x] 지역별 관광 수요 강도 정보 서비스
- [x] 지역별 관광 자원 수요 정보 서비스
- [x] 기초지자체 중심관광지 정보 서비스
- [x] 관광지별 연관관광지 정보 서비스
- [x] 웰니스 관광정보 서비스
- [x] 국문 관광정보 서비스
