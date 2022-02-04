# URI와 웹 브라우저 요청 흐름

## URI
URI(Uniform Resource Identifier)
- URI : URI는 로케이터(Locator), 이름(Name) 또는 둘 다 추가로 분류될 수 있다.
- URL
- URN

### URI 뜻
- Uniform : 리소스를 식별하는 통일된 방식
- Resource : 자원, URI로 식별할 수 있는 모든 것(제한 없음)
- Identifier : 다른 항목과 구분하는데 필요한 정보

### URL, URN 뜻
- URL : Locator, 리소스가 있는 위치를 지정
- URN : Name, 리소스에 이름을 부여
- 위치는 변할 수 있지만, 이름은 변하지 않는다.
- urn:isbn:8960777331 (어떤 책의 isbn URN)
- URN 이름 만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않았음.

### URL scheme
- **scheme**://[userinfo@]host[:port][/path][?query][#fragment]
- https://www.google.com:443/search?q=hello&hl=ko
    - 주로 프로토콜 사용
        - 프로토콜 : 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
        - https, http, ftp 등
    - http 는 80포트, https는 443 포트를 주로 사용, 포트는 생략 가능
    - https는 http에 보안 추가 (HTTP Secure)

### URL userinfo
- scheme://**[userinfo@]** host[:port][/path][?query][#fragment]
- https://www.google.com:443/search?q=hello&hl=ko
- URL에 사용자정보를 포함해서 인증
- 거의 사용하지 않는다.

### URL host
- scheme://[userinfo@]**host**[:port][/path][?query][#fragment]
- https://**www.google.com**:443/search?q=hello&hl=ko
- 호스트명
- 도메인명 또는 IP 주소를 직접 사용 가능

### URL port
- scheme://[userinfo@]host **[:port]**[/path][?query][#fragment]
- https://www.google.com **:443**/search?q=hello&hl=ko
- 포트(port)
- 접속 포트
- 일반적으로 생략, 생략시 http = 80, https = 443

### URL path
- scheme://[userinfo@]host[:port]**[/path]**[?query][#fragment]
- https://www.google.com:443 **/search**?q=hello&hl=ko
- 리소스 경로(path), 계층적 구조
- 예)
    - /home/file1.jpg
    - /members
    - /members/100, /items/iphone12

### URL query
- scheme://[userinfo@]host[:port][/path]**[?query]**[#fragment]
- https://www.google.com:443/search **?q=hello&hl=ko**
- key = value 형태
- `?` 로 시작, `&` 로 추가 가능 ?keyA=valueA&keyB=valueB
- query parameter, query string 등으로 불림, 웹서버에 제공하는 파라미터. 문자 형태

## 웹 브라우저 요청 흐름
- https://www.google.com:443/search?q=hello&hl=ko 를 웹 브라우저가 요청한다.
- 구글 서버를 찾기 위해 `DNS`를 조회해서 IP를 찾고 `port` 정보를 찾는다.
- HTTP 요청 메시지 생성
    - 요청 메시지 : GET /search?q=hello&hl=ko HTTP/1.1 HOST: www.google.com
- 요청 패킷 전달
- 구글 서버에 패킷이 도착하면 TCP/IP 까서 HTTP 메시지를 끄집어내어 해석을 한다.
- 구글 웹 서버에서 HTTP 응답 메시지 응답 패킷에 담아 웹 브라우저로 보낸다.
- 웹 브라우저에서 html 데이터를 렌더링한다.

### HTTP 메시지 전송
1. 웹 브라우저가 HTTP 메시지 생성
2. `socket` 라이브러리를 통해 전달
3. TCP/IP 패킷 생성, HTTP 메시지 포함
