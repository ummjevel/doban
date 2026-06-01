# 도반(Doban) 진행 내용

마지막 업데이트: 2026-06-02 KST

## 확정된 방향

- 최종 서비스 제목: 도반(Doban): 관광 데이터 기반 지역 창업 분석 챗봇
- 핵심 사용자: 국내 예비창업자 + 기존 소상공인
- 결과물 형태: 미니 컨설팅 리포트
- 답변 톤: 차분한 데이터 컨설턴트
- 플랫폼: 엔노이아 Studio
- 제출 마감일: 2026-06-09
- OpenAPI 신청자명: 전민정, 안중현

## MVP 선택 API 7개

| 번호 | 제출 API명 | 링크 | 상태 |
|---:|---|---|---|
| 1 | 관광 빅데이터 정보 서비스 | https://www.data.go.kr/data/15101972/openapi.do | 엔노이아 등록 및 기본값 호출 성공 |
| 2 | 지역별 관광 수요 강도 정보 서비스 | https://www.data.go.kr/data/15151868/openapi.do | 엔노이아 등록 및 기본값 호출 성공 |
| 3 | 웰니스 관광정보 서비스 | https://www.data.go.kr/data/15144030/openapi.do | 엔노이아 등록 및 기본값 호출 성공 |
| 4 | 관광지별 연관관광지 정보 서비스 | https://www.data.go.kr/data/15128560/openapi.do | 엔노이아 등록 및 기본값 호출 성공 |
| 5 | 지역별 관광 자원 수요 정보 서비스 | https://www.data.go.kr/data/15152138/openapi.do | 엔노이아 등록 및 기본값 호출 성공 |
| 6 | 기초지자체 중심관광지 정보 서비스 | https://www.data.go.kr/data/15128559/openapi.do | 엔노이아 등록 및 기본값 호출 성공 |
| 7 | 국문 관광정보 서비스 | https://www.data.go.kr/data/15101578/openapi.do | 추가 예정 |

## 완료한 작업

- [x] 프로젝트 폴더명을 `doban`으로 변경
- [x] 프로젝트 관리 문서 생성: `todo.md`
- [x] API 관리 문서 생성: `api.md`, `api-list.md`
- [x] 진행 내용 문서 생성: `progress.md`
- [x] 서비스 제목 확정
- [x] 타겟 사용자 확정
- [x] 결과물 형태 확정
- [x] 답변 톤 확정
- [x] MVP API 7개 확정
- [x] OpenAPI 신청자명 확인
- [x] 선택 API 서비스키 발급 또는 확인
- [x] 선택 API를 엔노이아 API 커넥터에 등록
- [x] API 키와 기본값으로 엔노이아 호출 테스트 완료
- [x] 주요 API Swagger 명세 확인
- [x] 선택 API별 엔드포인트, 필수 파라미터, 주요 응답 필드 정리
- [x] 코드값 원본 파일 확인
- [x] 코드값을 엔노이아 문서 폴더 규칙에 맞춰 CSV로 정제
- [x] 지역명 -> 지역 코드/시군구 코드 변환용 lookup CSV 생성
- [x] API 요청용 지표 코드 CSV 생성
- [x] 코드값 CSV의 `ID` 중복 여부와 헤더 규칙 검증
- [x] 코드값 파일을 엔노이아에 첨부

## 코드값 파일 정리

엔노이아 문서 폴더 업로드용 파일 위치:

```text
codes/rag_upload/area_codes_rag.csv
codes/rag_upload/indicator_codes_rag.csv
codes/rag_upload/region_codes_rag.csv
codes/rag_upload/region_name_lookup_rag.csv
```

검증 결과:

- 모든 CSV 첫 번째 열은 `ID`
- ID 중복 없음
- 헤더는 영문 소문자, 숫자, 밑줄 기준으로 정리
- UTF-8 CSV로 생성

중복처럼 보이는 값 설명:

- `중구`, `동구`, `강서구`, `고성군`처럼 여러 시도에 같은 시군구명이 존재한다.
- 이는 데이터 오류가 아니라 행정구역명 중복이다.
- `region_name_lookup_rag.csv`에는 이런 경우 `is_ambiguous=Y`와 `candidate_count`를 표시했다.
- 챗봇은 모호한 지역명만 입력되면 시도명을 추가로 물어봐야 한다.

## 아직 남은 작업

- [x] 도반 시스템 프롬프트 v1 작성
- [x] 미니 컨설팅 리포트 출력 형식 1차 확정
- [ ] 엔노이아 챗봇 플로우에서 지역명 -> 지역 코드/시군구 코드 변환 테스트
- [ ] 엔노이아 챗봇 플로우에서 선택 API 호출 연결
- [ ] 실제 응답 예시값을 `api-list.md`에 기록
- [ ] 대표 질문 5개 이상으로 엔드투엔드 테스트
- [ ] API 빈 결과/오류 응답 fallback 작성
- [ ] 엔노이아 Studio 공유 URL 생성
- [ ] 로그인 없이 공유 URL 테스트
- [ ] 서비스 소개서 작성

## 다음 작업 추천

1. 엔노이아에 `prompts/system-prompt-v1.md` 적용
2. 엔노이아에서 지역명 변환과 API 호출 연결
3. 대표 질문으로 엔드투엔드 테스트
