# HTTP 헤더 - 일반 헤더

## HTTP 헤더 개요

### HTTP 헤더 용도
- HTTP 전송에 필요한 모든 부가정보
- 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 등
- 표준 헤더가 너무 많음
- 필요시 임의의 헤더 추가 가능
    - helloworld: hihi

### 과거 HTTP 헤더
RFC2616 (과거, 1999년)
- 헤더 분류
    - General 헤더 : 메시지 전체에 적용되는 정보, 예) Connection : close
    - Request 헤더 : 요청 정보, 예) User-Agent : Mozila/5.0 (Macintosh)
    - Response 헤더 : 응답 정보, 예) Server : Apache
    - Entity 헤더 : 엔티티 바디 정보, 예) Content-Type : text/html, Content-Length : 3423

### HTTP BODY
message body - RFC2616 (과거, 1999년)
- 메시지 본문(body)은 엔티티 본문(entity body)을 전달하는데 사용
- 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
- 엔티티 헤더는 엔티티 본문의 데이터를 해석할 수 있는 정보 제공
    - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등

### 현재(2014년) HTTP 헤더
RFC 7230 ~ 7235 등장

### RFC723x 변화
- 엔티티 -> 표현(Representation)
- Representation = representation Metadata + representation data
- 표현 = 표현 메타데이터 + 표현 데이터

### 최신 HTTP BODY
message body - RFC7230(최신)
- 메시지 본문(body)을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드(Payload)
- 표현은 요청이나 응답에서 전달할 실제 데이터
- 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
    - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등
- 참고 : 표현 헤더는 표현 메타데이터와 페이로드 메시지를 구분해야 하지만 여기서는 생략한다.

## 표현 헤더
- Content-Type : 표현 데이터의 형식
- Content-Encoding : 표현 데이터의 압축 방식
- Content-Language : 표현 데이터의 자연 언어
- Content-Lengh : 표현 데이터의 길이
- 표현 헤더는 전송, 응답 둘 다 사용

### Content-Type
표현 데이터의 형식 설명
- 미디어 타입, 문자 인코딩
- 예)
    - text/html; charset=utf-8
    - application/json
    - image/png

### Content-Encoding
표현 데이터 인코딩
- 표현 데이터를 압축하기 위해 사용
- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
- 예)
    - gzip
    - deflate
    - identity

### Content-Language
표현 데이터의 자연 언어
- 표현 데이터의 자연 언어를 표현
- 예)
    - ko
    - en
    - en-US

### Content-Length
표현 데이터의 길이
- 바이트 단위
- Transfer-Encoding을 사용하면 Content-Length를 사용하면 안 됨

## 협상 (Content 네고)
클라이언트가 선호하는 표현 요청
- Accept : 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset : 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
- Accept-Language : 클라이언트가 선호하는 자연 언어
- 협상 헤더는 **요청시에만** 사용

### 협상과 우선순위1
Quality Values(q)
- Quality Values(q) 값 사용
- 0~1, 클수록 높은 우선순위
- 생략하면 1
- Accept-Language : ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
    1. ko-KR;q=1 (q생략)
    2. ko;q=0.9
    3. en-US;q=0.8
    4. en;q=0.7

### 협상과 우선순위2
Quality Values(q)
- 구체적인 것이 우선
- Accept: text/*, text/plain, text/plain;format=flowed, */*
    1. text/plain;format=flowed
    2. text/plain
    3. text/*
    4. */*

### 협상과 우선순위3
Quality Values(q)
- 구체적인 것을 기준으로 미디어 타입을 맞춘다.
- Accept : text/*;q=0.3, text/html;q=0.7, text/html;level=1, text/html;level=2;q=0.4, */*;q=0.5

|Media Type|Quality|
|---|---|
|text/html;level=1|1|
|text/html|0.7|
|text/plain|0.3|
|image/jpeg|0.5|
|text/html;level=2|0.4|
|text/html;level=3|0.7|

## 전송 방식

### 단순 전송
Content-Length
- 한 번에 요청하고 한 번에 받는 것

### 압축 전송
Content-Encoding
- 예로 단순 전송했던 것을 gzip 같은 것으로 압축해서 요청하는 것

### 분할 전송
Transfer-Encoding
- chunked : 덩어리 로 보낸다
- Content-Length 를 보내면 안됨

### 범위 전송
Range, Content-Range
- 예로 이미지를 받는데 중간에 끊겨서 다시 요청할 때 처음부터 다시 받으면 용량이 낭비되기 때문에 끊긴 부분부터 범위 전송을 요청한다.

## 일반 정보

### From
유저 에이전트의 이메일 정보
- 일반적인 잘 사용되지 않음
- 검색 엔진 같은 곳에서 주로 사용
- 요청에서 사용

### Referer
이전 웹 페이지 주소
- 현재 요청된 페이지의 이전 웹 페이지 주소
- A -> B 로 이동하는 경우 B를 요청할 때 Referer : A를 포함해서 요청
- Referer를 사용해서 유입 경로 분석 가능
- 요청에서 사용
- 참고 : Referer는 단어 referrer의 오타 ㅋㅋ

### User-Agent
유저 에이전트 애플리케이션 정보
- user-agent: Mozila./5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36(KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36
- 클라이언트의 애플리케이션 정보 (웹 브라우저 정보 등)
- 통계 정보
- 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
- 요청에서 사용

### Server
요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
- Server : Apache/2.2.22 (Debian)
- server : nginx
- 응답에서 사용

### Date
메시지가 발생한 날짜와 시간
- Data : Tue, 15 Nov 1994 08:12:31 GMT
- 응답에서 사용