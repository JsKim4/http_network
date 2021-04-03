# HTTP 메서드 활용

## 목차
- 클라이언트에서 서버로 데이터 전송
- HTTP API 설계 예시

## 참고 URI
[restful 관련 참고자료](https://restful.net/resource-naming)

# 클라이언트에서 서버로 데이터 전송
- 데이터 전달 방식은 크게 2가지
    - 쿼리 파라미터를 통한 데이터 전송
        - GET
        - 주로 정렬혹은 필터(검색어)
    - 메세지 바디를 통한 데이터 전송
        - POST, PUT, PATCH
        - 회원가입, 상품주문, 리소스 등록, 리소스 변경

- 상황별 메소드
    - 정적 데이터 조회
        - 이미지, 정적 텍스트 문서
        - 쿼리 파라미터 미사용
        - GET을 사용
    - 동적 데이터 조회
        - 쿼리 파라미터 사용
        - 퀄리 파라미터를 기반으로 정렬 필터해서 결과를 동적으로 생성
        - GET을 사용
    - HTML Form을 통한 데이터 전송
        - POST 전송 => 바디로 전송 Content-Type: application/x-www-form-unlencoded
        - GET 전송 => 쿼리 파라미터로 전송
        - 파일 전송 => 바디로 전송 Content-Type: multipart/form-data 
    - HTTP API를 통한 데이터 전송
        - 서버 to 서버 통신
        - 앱 클라이언트
        - 웹 클라이언트
        - Content-Type: application/json을 주로 사용
    

# HTTP API 설계 예시
- HTTP API - 컬렉션
    - POST 기반 등록
    - ex) 회원관리 API 제공
- HTTP API - 스토어
    - PUT 기반 등록
    - ex) 정적 콘텐츠 관리, 원결 파일 관리

## 예시 시스템 (회원관리 시스템)
`POST 기반 등록`
- 회원 목록 /members -> GET
- 회원 등록 /members -> POST
- 회원 조회 /members/{id} -> GET
- 회원 수정 /members/{id} -> PATCH, PUT, POST
- 회원 삭제 /members/{id} -> DELETE

- 클라이언트가 등록될 리소스의 URI를 알지 못함

## 예시 시스템 (파일관리 시스템)
`PUT 기반 등록`
- 파일 목록 /files -> GET
- 파일 조회 /files/{filename} -> GET
- 파일 등록 /files/{filename} -> PUT
- 파일 삭제 /files/{filename} -> DELETE
- 파일 대량 등록 /files -> POST

- 클라이언트가 등록될 리소스의 URI를 알고 있음

# 정리
`참고하면 좋은 URI 설계 개념`
- 문서
    - 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
    - ex) /members/100, /files/star.jpg
- 컬렉션
    - 서버가 관리하는 리소스 디렉터리
    - 서버가 리소스의 URI를 생성하고 관리
    - ex) /members
- 스토어
    - 클라이언트가 관리하는 자원 저장소
    - 클라이언트가 리소스의 URI를 알고 등록
    - ex) /fiels
- 컨트롤러(controller), 컨트론 URL
    - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
    - 동사를 직접사용
    - ex) POST /orders/{id}/delivery