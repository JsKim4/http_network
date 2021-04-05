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


# 일반 정보
- From
    - 유저 에이전트의 이메일 정보
    - 일반적으로 잘 사영되지 않음
    - 검색 엔진 같은 곳에서, 주로 사용
    - 요청에서사용
- Referer
    - 이전 웹 페이지 주소
    - A -> B로 이동하는 경우 B를 요청할 떄 Referer:A 를 포함해서 요청
    - 유입 경로 분석 가능
    - 요청에서 사용
    - 참고: referer는 단어 referrer의 오타
- User-agent
    - 클라이언트의 애플리케이션 정보(웹 브라우저 정보,등등)
    - 통계 정보
    - 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
    - 요청에서 사용
- Server
    - 요청을 처리하는 ORIGIN 서버의 소프트 웨어 정보
    - Server: Apache/2.2.22(Debian)
    - 응답에서 사용
- Date
    - 메세지가 발생한 날짜와 시간
    - 응답에서 사용

# 특별한 정보
- HOST
    - 요청한 호스트 정보(도메인)
    - 필수값
    - 하나의 서버가 여러 도메인을 처리해야 할때
    - 하나의 IP 주소에 여러 도메인이 적용되어있을 경우
- Location
    - 웹 브라우저는 3XX 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동
- Allow
    - 허용 가능한 HTTP 메서드
    - 405에서 응답에 포함해야함
    - Allow: GET, HEAD, PUT
- Retry-After
    - 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
    - 날짜표기 혹은 초단위 표기 가능

# 인증
- Authorization
    - 클라이언트 인증 서버를 서버에 전달
    - Basic XXXXXXXXXXXXXXXXXXXX
- WWW-Authenticate 
    - 리소스 접근시 필요한 인증 방법 정의
    - 401 Unautorized 응답과 함께 사용
# 쿠키
- Set-Cookie
    - 서버에서 클라이언트로 쿠키 전달(응답)
- Cookie
    - 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버 전달

- 쿠키
    1. 로그인 시도
    2. 응답시 서버에서 Set-Cookie 헤더로 유저 정보 저장
    3. 브라우저 내부에 쿠키 저장소에 저장
    4. 요청시 Cookie 헤더를 만들어서 서버에 전달

- 주 사용처
    - 사용자 로그인 세션 관리
    - 광고 정보 트레킹
- 쿠키 정보는 항상 서버에 전송됨
    - 네트워크 트래픽이 추가 발생함
    - sessionId등의 최소 정보만 사용해서 서버에서 처리함
    - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 참고
- 주의사항
    - 보안에 민감한 데이터는 저장하면 안됨

- 생명주기
    - Set-Cookie: expires = 만료일이 되면 쿠키 삭제
    - Set-Cookie: maxAge = 0이나 음수가되면 쿠키 삭제
    - 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
    - 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지

- 도메인
    - 명시
        - 명시한 문서 기준 도메인 + 서브 도메인 포함
        - domain=example.org를 지정해서 쿠키를 생성하면 서브도메인도 접근 가능
    - 생략
        - 생성한 도메인만 접근 가능 

- 경로
    - path=/home
    - 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
    - 일반적으로 path=/ 루트로 지정
- 보안
    - Secure
        - https인 경우에만 클라이언트에서 서버로 전송
    - HttpOnly
        - 자바스크립트에서 쿠키접근 불가
    - SameSite
        - XSRF
        - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우에만 쿠키 전송 가능
