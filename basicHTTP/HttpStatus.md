# HTTP 상태 코드

## HTTP 상태 코드란
클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능
- 1xx (Informational) : 요청이 수신되어 처리중
- 2xx (Successful) : 요청 정상 처리
- 3xx (Redirection) : 요청을 완료하려면 추가 행동이 필요
- 4xx (Client Error) : 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
- 5xx (Server Error) : 서버 오류, 서버가 정상 요청을 처리하지 못함

## 2xx - 성공

### 200 OK
요청 성공

### 201 Created
요청 성공해서 새로운 리소스가 생성됨

### 202 Accepted
요청이 접수되었으나 아직 처리가 완료되지 않았음

### 203 No Content
서버가 요청을 성공적으로 수행헀지만, 응답 페이로드 본문에 보낼 데이터가 없음
- 예) 웹 문서 편집기에서 save 버튼
- save 버튼의 결과로 아무 내용이 없어도 된다.
- save 버튼을 눌러도 같은 화면을 유지해야 한다.
- 결과 내용이 없어도 204 메시지(2xx)만으로 성공을 인식할 수 있다.

## 3xx - Redirection
요청을 완료하기 위해 유저 에이전트(웹 브라우저)의 추가 조치 필요

### 리다이렉션의 이해
- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동

### 자동 리다이렉트 흐름
1. GET 방식으로 /event HTTP/1.1을 요청
2. 서버가 HTTP/1.1 301 Moved Permanently **Location: /new-event** 로 event가 new-event로 리다이렉트 응답
3. 여기서 **자동 리다이렉트** 되어 다시 GET 방식으로 /new-event HTTP/1.1 로 요청
4. HTTP/1.1 200 OK 로 요청 성공되었다고 서버가 응답

### 리다이렉션 종류
- 영구 리다이렉션
특정 리소스의 URI가 영구적으로 이동
    - 예) /members -> /users
    - 예) /event -> /new-event
- 일시 리다이렉션
일시적인 변경
    - 주문 완료 후 주문 내역 화면으로 이동
    - PRG : POST/REDIRECT/GET
- 특수 리다이렉션
    - 결과 대신 캐시를 사용

### 영구 리다이렉션
301, 308
- 리소스의 URI가 영구적으로 이동
- 원래의 URL을 사용 X, 검색 엔진 등에서도 변경 인지
- 301 Moved Permanently
    - 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있다
- 308 Permanent Redirect
    - 301과 기능은 같다
    - 리다이렉트시 요청 메서드와 본문 유지 (처음 POST를 보내면 리다이렉트도 POST 유지)

