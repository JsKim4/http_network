# URI와 웹 브라우저 요청 흐름

## 목차
- URI
- 웹 브라우저 요청 흐름

### `URI`
- URI (Uniform Resource Identifier)
- URI, URL, URN
    - URI는 로케이터, 이름 또는 둘다 추가로 분류될 수 있다.
    - URL (Resource Locator) 는 리소스의 위치
        - foo://example:8042/over/there?name=ferret#nose
    - URN (Resource Name) 는 리소스의 이름
        - urn:example:animal:ferret:nose
    
- URI
    - Uniform: 리소스를 식별하는 통일된 방식
    - Resousece: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
    - Identifier: 다른 항목과 구분하는데 필요한 정보

- 단어 정리
    - URL - Locator: 리소스가 있는 위치를 지정
    - URN - Name: 리소스에 이름을 부여
    - 위치는 변할 수 있지만, 이름은 변하지 않는다.
    - urn:isbn:8960777331 (어떤 책의 isbn URN)
    - URN 이름만으로 실제 리소스를 찾는 방법이 보편화 되지않음
    - URL ~= URI
- URL 분석 (https://www.google.com/search?q=hello&hl=ko)
    - sheme://[userinfo@]host[:port][/path][?query][#fragment]
    - 주로 프로토콜 사용
    - 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
    - http는 80포트, https는 443포트를 주로 사용

    - 분석
        - userinfo
            - URL에 사용자 정보를 포함해서 인증
            - 거의 사용 X
        - host
            - 호스트명
            - 도메인 혹은 IP주소를 입력
        - port
            - 접속 포트
            - 일반적으로 생략, 생략시 http는 80, https는 443
        - path
            - 리소스 경로, 계층적 구조
        - query
            - key=value 형태
            - ?로 시작, &로 추가 가능
        - fragment
            - html 내부 북마크에 사용
### `웹 브라우저 요청 흐름`
- HTTP 메세지 전송
    1. 웹 브라우저가 HTTP 메세지 생성
    2. SOCKET 라이브러리를 통해 전달
        - A: TCP/IP 연결
        - B: 데이터 전달
    3. TCP/IP 패킷 생성, HTTP 메세지 포함
    4. 수신서버 => TCP/IP 패킷 버림, HTTP 메세지 해석
    5. 수신서버에서 HTTP응답 메세지 생성
    6. TCP/IP 패킷 패키징
    7. 클라이언트 서버 수신및 HTML 렌더링