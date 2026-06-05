# 도반(Doban)

도반(Doban)은 한국관광공사 OpenAPI를 활용해 관광지명 검색, 지역별 관광 흐름, 관광 수요, 관광 자원 수요, 중심 관광지, 연관 관광지, 웰니스 관광정보를 분석하고 국내 예비창업자와 기존 소상공인에게 지역 창업 분석 결과를 제공하는 엔노이아 Studio 기반 챗봇이다.

## 서비스 제목

도반(Doban): 관광 데이터 기반 지역 창업 분석 챗봇

## 대상 사용자

- 국내 예비창업자
- 관광 상권에서 운영 중인 기존 소상공인

## 결과물 형태

도반은 단순 추천 답변이 아니라 미니 컨설팅 리포트 형태로 답변한다.

예상 답변 구조:

1. 한 줄 결론
2. 지역 관광 데이터 요약
3. 관광 수요/소비/체류 특성
4. 중심 관광지와 연관 관광 동선
5. 웰니스 및 관광 자원 수요 분석
6. 창업 아이템 제안
7. 리스크와 추가 확인사항

## MVP 선택 API

| 번호 | 제출 API명 | 공공데이터포털 링크 | 활용 목적 |
|---:|---|---|---|
| 1 | 관광 빅데이터 정보 서비스 | https://www.data.go.kr/data/15101972/openapi.do | 지역별 방문자 규모와 관광 흐름 파악 |
| 2 | 지역별 관광 수요 강도 정보 서비스 | https://www.data.go.kr/data/15151868/openapi.do | 관광 체류 강도와 관광 소비 강도 분석 |
| 3 | 웰니스 관광정보 서비스 | https://www.data.go.kr/data/15144030/openapi.do | 웰니스/힐링 창업 아이템과 지역 콘텐츠 후보 확인 |
| 4 | 관광지별 연관관광지 정보 서비스 | https://www.data.go.kr/data/15128560/openapi.do | 중심 관광지와 연결되는 주변 관광 동선 분석 |
| 5 | 지역별 관광 자원 수요 정보 서비스 | https://www.data.go.kr/data/15152138/openapi.do | 식음료, 쇼핑, 숙박, 체험, 문화/자연 자원 수요 분석 |
| 6 | 기초지자체 중심관광지 정보 서비스 | https://www.data.go.kr/data/15128559/openapi.do | 지역 내 관광 동선의 중심 거점 파악 |
| 7 | 국문 관광정보 서비스 | https://www.data.go.kr/data/15101578/openapi.do | 관광지명 키워드 검색, 주소/좌표/contentId 확인 |

## 현재 진행 상태

- OpenAPI 신청자명: 전민정, 안중현
- 선택 API 엔노이아 API 커넥터 등록 진행
- API 키와 기본값 기반 호출 테스트 완료
- API 명세, 엔드포인트, 파라미터, 응답 필드 정리 완료
- 코드값 파일을 엔노이아 문서 폴더 업로드 규칙에 맞춰 CSV로 정제 완료
- 코드값 파일 엔노이아 첨부 완료

## 코드값 파일

엔노이아 문서 폴더 업로드용 CSV는 아래 폴더에 있다.

```text
codes/rag_upload/
```

주요 파일:

```text
area_codes_rag.csv
indicator_codes_rag.csv
place_region_overrides_rag.csv
region_codes_rag.csv
region_name_lookup_rag.csv
```

각 CSV는 엔노이아 문서 폴더 규칙에 맞춰 처리했다.

- 첫 번째 열은 `ID`
- ID 중복 없음
- 헤더는 영문 소문자, 숫자, 밑줄 중심으로 정리
- UTF-8 CSV

## 문서 구조

```text
README.md
progress.md
todo.md
api.md
api-list.md
docs/
codes/
  processed/
  rag_upload/
```

파일 역할:

- `README.md`: 프로젝트 개요
- `progress.md`: 현재 진행 상황 요약
- `todo.md`: 날짜별 할 일과 체크리스트
- `api.md`: API별 상세 명세와 해석 규칙
- `api-list.md`: API 선택 목록, 엔노이아 테스트 상태, 제출 API 관리
- `docs/troubleshooting-and-improvements.md`: 시행착오와 개선 기록
- `codes/processed/`: 가공 중간 산출물
- `codes/rag_upload/`: 엔노이아 문서 폴더 업로드용 CSV

## 다음 작업

1. 미니 컨설팅 리포트 출력 형식 확정
2. 도반 시스템 프롬프트 v1 작성
3. 엔노이아 챗봇 플로우에서 지역명 -> 지역 코드/시군구 코드 변환 테스트
4. 엔노이아 챗봇 플로우에서 선택 API 호출 연결
5. 대표 질문 5개 이상으로 엔드투엔드 테스트
6. 서비스 소개서 작성

## 테스트 질문

```text
강릉에서 관광객 대상으로 창업하기 좋은 아이템 추천해줘.
전주 한옥마을 근처에서 디저트 가게를 하면 괜찮을까?
부산 해운대 주변 관광 동선을 보고 소상공인 아이템 추천해줘.
경주에서 가족 관광객 대상 창업 아이템 추천해줘.
제주에서 웰니스 관광객을 대상으로 할 만한 창업 아이템 추천해줘.
```
