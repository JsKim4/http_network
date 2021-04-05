# HTTP 헤더2 - 캐시와 조건부 요청

## 목차
- 캐시 기본 동작
- 검증 헤더와 조건부 요청
- 캐시와 조건부 요청 헤더
- 프록시 캐시
- 캐시 무효화

# 캐시 기본 동작
- 캐시가 없을 경우
    - 여러번 요청해도 헤더와 바디를 생성하여 전달함

- 캐시 적용
    1. 서버에 첫번째 리소스 요청
    2. 서버에서 응답시 cache-control 헤더를 넣음
        - cache-control: max-age=60
    3. 브라우저 캐시에 데이터 저장
    4. 사용자가 두번째 리소스 요청시 브라우저 캐시에서 조회
    5. cache-control 시간 초과시 서버에 재요청함
    
# 검증 헤더와 조건부 요청
- 캐시 유효시간 초과 후 서버에 요청시 시나리오
    1. 서버에서 기존 데이터를 변경함
    2. 서버에서 기존 데이터를 변경하지 않음
- 검증 헤더 추가
    1. 첫번째 응답시 cache-control과 last-modified(최종 수정일) 헤더 추가
    2. cache-control 시간 초과시 if-modified-since 헤더를 추가
    3. 서버에서 304 Not Modified응답, HTTP BODY 없음
    4. 클라이언트에서 브라우저 캐시 사용
- 조건부 요청 헤더
    - 검증 헤더로 조건에 따른 분기
    - if-modified-since: Last-Modified 사용
    - if-none-match: etag 사용
    - 조건이 만족하면 200 OK
    - 조건이 만족하지 않으면 304 Not Modified
- if-modified-since
    - 데이터 미변경 예시
        - 캐시 2020년 11월 10일 10:00:00 vs 서버 2020년 11월 10일 10:00:00
        - 304 Not Modified, 헤더 데이터만 전송(BODY 미포함)
        - 전송용량 0.1M(헤더 0.1M, 바디 1.0M)
    - 데이터 변경 예시
        - 캐시 2020년 11월 10일 10:00:00 vs 서버 2020년 11월 10일 11:00:00
        - 200 OK, 모든 데이터 전송(BODY 포함)
        - 전송용량 1.1M(헤더 0.1M, 바디 1.0M)
    - 단점
        - 1초 미만으로 캐시 조정 불가
        - 날짜 기반의 로직 사용
        - 데이터 수정으로 날짜가 다르지만, 같은데이터를 수정해서 데이터 결과가 같은 경우
        - 서버에서 별도의 캐시로직을 관리 하고 싶은 경우
- etag
    - 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
    - 캐시 관련된 로직을 모두 서버에서 처리

# 캐시와 조건부 요청 헤더
- Cache-Control: max-age
    - 캐시 유효 시간, 초 단위
- Cache-Control: no-cache
    - 데이터는 캐시해도 되지만, 항상 원(origin) 서버에 검증하고 사용
- Cache-Control: no-store
    - 데이터에 민감한 정보가 있으므로 저장하면 안됨
- Pragma: no-cache  
    - http 1.0 이하 버전
- expires
    - 캐시 만료일을 정확한 날짜로 지정
    - max-age 사용 권장
- 검증 헤더와 조건부 요청 헤더
    - 검증 헤더
        - etag
        - lastmodified
    - 조건부 요청 헤더
        - if-match, if-none-match
        - if-modified-since,if-none-modified-since 
# 프록시 캐시
- 물리적으로 가까운 곳에 캐싱 서버를 둠
- 서버 = public 캐시
- 클라이언트 = private 캐시
- 캐시 지시어
    - Cache-Control: public
        - 응답이 public 캐시에 저장됨
    - Cache-Control: private
        - 응답이 private 캐시에 저장됨
    - Cache-Control: s-maxage
        - 프록시 캐시에만 적용되는 max-age
    - Age
        - 프록시 서버에서 머무른 시간

# 캐시 무효화
- 확실한 캐시 무효화 응답
    - Cache-Control: no-cache, no-store, must-revalidate
    - Pragma:no-cache
        - HTTP 1.0 하위 호환
- Cache-Control: no-cache
    - 데이터는 캐시해도 되지만, 항상 원 서버에 검증하고 사용
- Cache-Control: no-store
    - 데이터에 민감한 정보가 있으므로 저장하면 안됨
- Cache-Control: must-revalidate
    - 캐시 만료후 최초 조회시 원서버에 검증해야함
    - 원 서버 접근 실패시 반드시 오류 발생해야함 - 504
    - 캐시 유효시간이라면 캐시를 사용함 

- no-cache vs must-revalidate
    - 원서버 검증 실패시(gateway exception) no-cache 는 프록시 캐시라도 보여줌
    - 원서버 검증 실패시(gateway exception) must-revalidate 는 504 error