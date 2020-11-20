# HTTP 프로토콜

- HTTP(Hypertext Transfer Protocol) 통신 프로토콜

- 프로토콜 : 상호 간에 정의한 규칙을 의미하며 특정 기기 간에 데이터를 주고받기 위해 정의

- 웹에서는 브라우저와 서버 간 데이터를 주고받기 위한 방식으로 HTTP 프로토콜이 많이 사용

## 특징

- 상태가 없는(stateless) 프로토콜
- 상태가 없다 = 데이터를 주고 받기 위한 각각의 데이터 요청이 서로 독립적으로 관리가 된다.

- 이전 데이터 요청과 다음 데이터 요청이 서로 관련이 없다.

- 서버는 세션과 같은 별도의 추가 정보를 관리하지 않아도 되고, 다수의 요청 처리 및 서버의 부하를 줄일 수 있는 성능 상의 이점이 생김

## HTTP Request & Response

- 요청(Request)과 응답(Response)

![예제 그림](https://joshua1988.github.io/images/posts/web/http/http-full-structure.png)

- 클라이언트(Client) : 요청을 보내는 쪽, 일반적으로 웹 관점의 브라우저
- 서버(Server) : 요청을 받는 쪽, 일반적으로 데이터를 보내주는 원격지의 컴퓨터

## URL

- URL(Uniform Resource Locators) : 서버에 자원을 요청하기 위한 영문 주소

- 구조

![URL구조](https://joshua1988.github.io/images/posts/web/http/url-structure.png)

## HTTP 요청 메소드

- URL을 이용해 서버에 특정 데이터를 요청할 수 있다.
- 데이터에 특정 동작을 연결하기 위해 HTTP 요청 메소드(HTTP Request Methods)를 이용

- 일반적으로 HTTP 요청 메소드는 HTTP Verbs라고 불리며 다음과 같은 메소드를 가짐
  - __GET__ : 존재하는 자원에 대한 요청
  - __POST__ : 새로운 자원을 생성
  - __PUT__ : 존재하는 자원에 대한 변경
  - __DELETE__ : 존재하는 자원에 대한 삭제

- 조회, 생성, 변경, 삭제(CRUD)에 대한 동작은 HTTP 요청 메소드로 정의 가능.
- 상황에 따라서는 POST 메소드로 PUT, DELETE의 동작도 수행할 수 있다.

- 기타 요청 메소드
  - __HEAD__ : 서버 헤더 정보를 얻음, __GET__ 과 비슷하나, Response Body를 반환하지 않는다.
  - __OPTIONS__ : 서버 옵션들을 확인하기 위한 요청
    - __CORS__에서 사용

## HTTP 상태 코드

- 자세한 정리는 [여기]
(Web\HTTP스테이터스.md)

- HTTP 상태코드(HTTP Status Code)는 서버에서 설정해주는 응답(Response)

- 이 상태 코드로 에러처리를 할 수 있다.
