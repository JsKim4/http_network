# HTTP 헤더1 - 일반 헤더

## 목차
- HTTP 헤더 개요
- 표현
- 콘텐츠 협상
- 전송방식
- 일반 정보
- 특별한 정보
- 인증
- 쿠키

# HTTP 헤더 개요
- header-field = field-name":"OWS field value OWS (OWS: 띄어쓰기 허용)
- 용도
    - HTTP  전송에 필요한 모든 부가 정보
    - [표준 헤더 정보](http://en.wikipedia.org/wiki/List_of_HTTP_header_fields)

- HTTP BODY (RFC7230)
- 표현 = 표현 헤더 (헤더) + 표현 데이터 (바디)
- 메세지 본문을 통해 표현 데이터 전달
- 메세지 본문 = 페이로드
- 표현은 요청이나 응답에서 전달할 실제 데이터
- 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
    - 데이터 유형(HTML, JSON), 데이터 길이, 압축 정보 등등
# 표현
- Content-Type: 표현 데이터 형식 
    - 미디어 타입, 문자 인코딩
    - 예시

            text/html;charset=utf-8
            application/json
            image/png

- Content-Encoding: 표현 데이터의 압축 방식
    - 표현 데이터를 합축하기 위해 사용
    - 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
    - 데이터를 읽는 쪽에서 헤더의 정보로 압축 해제
    - 예시

            gzip
            daflate
            identity / 압축하지 않았다는 의미

- Content-Language: 표현 데이터의 자연 언어
    - 표현 데이터의 자연 언어를 표현
    - 예시

            ko
            en
            en-US
    - 다국어 처리
- Content-Length: 표현 데이터의 길이
    - 표현 데이터 길이
    - 바이트 단위
    - Transfer-Encoding을 사용하면 해당 헤더는 사용 X
- 표현 헤더는 전송, 응답 둘다 사용

# 콘텐츠 협상
- 클라이언트가 선호하는 표현 요청
- `Accept`: 클라이언트가 선호하는 미디어 타입 전달
- `Accpet-Charset`: 클라이언트가 선호하는 문자 인코딩
- `Accept-Encoding`: 클라이언트가 선호하는 압축 인코딩
- `Accept-Language`: 클라이언트가 선호하는 자연 언어
- 협상 헤더는 요청시에만 사용


- 협상과 우선순위1
    - Quality Value(q) 값 사용
    - 0 ~ 1,클수록 높은 우선 순위
    - 생략하면 1
    - Accept-Language:ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
        1. ko-KR;q=1
        2. ko;q=0.9
        3. en-US;q=0.8
        4. en;q=0.7 
- 협상과 우선순위2
    - Accpet: text/*, text/plain, text/plain;format=flowed,*/*
        1. text/plain;format=flowed
        2. text/plain
        3. text/*
        4. */*

# 전송 방식 설명
- 단순 전송
    - 응답시 Content-Length를 같이 응답 압축 X
- 압축 전송
    - Content-Encoding 에 압축타입 응답
    - 서버에서 압축하여 전달
- 분할 전송
    - Transfer-Encoding: chunked
    - 분할해서 전달
    - 크기와 데이터를 전달
    - Content-Length 전달 X
    - 예시

            5
            Hello
            5
            World
            0
            \r\n

- 범위 전송
    - 요청시 Range: bytes=1001-2000 로 범위 전달


