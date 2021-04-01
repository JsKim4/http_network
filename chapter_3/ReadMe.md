# HTTP 기본

## 목차
- 모든것이 HTTP
- 클라이언트 서버 구조
- Stateful, Stateless
- 비 연결성 (connectionless)
- HTTP 메세지

### `모든것이 HTTP`
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

### 클라이언트 서버 구조
- Request Response 구조
- 클라이언트는 서버에 요청을 보내고, 응답을 대기
- 서버가 요청에 대한 결과를 만들어서 응답
- 비즈니스 로직 / 데이터 = 서버
- UI / 사용성 = 클라이언트