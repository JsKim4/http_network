# HTTP 기본

## 목차
- 모든것이 HTTP
- 클라이언트 서버 구조
- Stateful, Stateless
- 비 연결성 (connectionless)
- HTTP 메세지

# 모든것이 HTTP
- Hyper Text Transfer Protocol
- HTTP 메세지는 모든 것을 전송
    - HTML, TEXT
    - IMAGE, 음성, 영상, 파일
    - JSON, XML(API)
    - 거의 모든 형태의 데이터 전송가능
    - 서버간 데이터 전송도 대부분 HTTP 사용
- HTTP 역사
    - HTTP/0.9 GET 메소드만 지원, HTTP 헤더 X
    - HTTP/1.0 메서드, 헤더 추가
    - HTTP/1.1 가장 많이사용, 대부분의 기능 추가됨
        - RFC2068 -> RFC2616 -> RFC7230~7235
    - HTTP/2 성능개선
    - HTTP/3 TCP 대신 UDP 사용, 성능 개선
- `기반 프로토콜`
    - TCP: HTTP/1.1, HTTP/2.0
    - UDP: HTTP/3
- 크롬 개발자 도구에서 Network -> Protocol 로 HTTP 버전 확인가능
- `HTTP 특징`
    - 클라이언트 서버 구조
    - 무상태 프로토콜(stateless), 비연결성
    - HTTP 메세지
    - 단순함, 확장 가능

# 클라이언트 서버 구조
- Request Response 구조
- 클라이언트는 서버에 요청을 보내고, 응답을 대기
- 서버가 요청에 대한 결과를 만들어서 응답
- 비즈니스 로직 / 데이터 = 서버
- UI / 사용성 = 클라이언트

# Stateful, Stateless
- HTTP는 무상태 프로토콜
- Stateful
    - 서버가 클라이언트 이전 상태를 보존
    - 단점: 항상 같은 서버가 응답을 위해 유지되어야 한다.

- Stateless
    - 서버가 클라이언트 상태를 보존 X
    - 장점: 서버 확장성 높음 (스케일 아웃)
    - 단점: 클라이언트가 추가 데이터 전송
    - 실무한계
        - 모든 것을 무상태로 설계 할 수 없는 겅우가 있음
        - ex) Login, Cookie 등
        - 전송하는 데이터량이 증가함

# 비 연결성 (connectionless)
- HTTP는 기본이 연결을 유지하지 않는 모델
- 일반적으로 초 단위의 이하의 빠른 속도로 응답
- 서버자원을 효율적으로 사용가능
- 한계
    - TCP/IP 연결을 새로 맺어야함 ->  3 way handshake 시간 추가
    - HTML외애 js, css, 추가이미지 등등 수많은 자원이 함께 다운로드됨
    - HTTP 지속연결로 해결 => 기존엔 리소스 하나당 연결과 종료를 진행했었음
        1. TCP/IP 연결
        2. HTML 요청 / 응답
        3. 자바스크립트 요청 / 응답
        4. 이미지 요청 / 응답 
        5. 종료

### `HTTP 메세지`
- HTTP 요청 메세지

     <span style="border:1px solid red"> GET /search?q=hello&h1=ko HTTP/1.1 </span>

    <span style="border:1px solid yellow">Host: www.google.com </span>
    
    <span style="color:green;border:1px solid green;"> </span>

    <span style="border:1px solid blue;"> 생략가능</span>

- HTTP 응답 메세지

    <span style="border:1px solid red"> HTTP/1.1 200 OK </span>

    <span style="border:1px solid yellow">ContentType: text/html;charsert=UTF-8 Content-Length: 3423</span>
    
    <span style="border:1px solid green;"> </span>

    <span style="border:1px solid blue;">
        body
    </span>

- HTTP 메세지 구조

    <span style="border:1px solid red"> start-line </span>

    <span style="border:1px solid yellow">header </span>
    
    <span style="border:1px solid green;"> empty line </span>

    <span style="border:1px solid blue;"> message body </span>

- 시작라인
    - 요청메세지
        - start-line = request-line
        - request-line = method SP(공백) request-targer SP HTTP-version CRLF(엔터)
    - 응답메세지
        - start-line = status-line
        - status-line = HTTP-version SP status-code SP reason-pharse CRLF
    
- Header
    - header-field = field-name ":" OWS field-value OWS (OWS:띄어쓰기 허용)
    - 용도
        - HTTP 전송에 필요한 부가 정보 
        - 필요시 임의의 헤더 추가가능
        - [표준 헤더 정보](http://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
- HTTP 메시지 바디
    - 용도
        - 실제 전송할 데이터
        - HTML 문서, 이미지 영상, JSON등등 byte 로 표현할 수 있는 모든 데이터 전송 가능