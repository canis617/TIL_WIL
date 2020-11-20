# CORS 교차 출처 리소스 공유(CORS)

- 교차 출처 리소스 공유 (Cross-Origin Resource Sharing, CORS)
  - 추가 HTTP 헤더를 사용하여 한 출처에서 실행중인 웹 어플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제

> https://domain-a.com의 프론트 엔드 js 코드가 XMLHttpRequest를 사용해 https://domain-b.com/data.json을 호출하는 경우

- 보안 상의 이유로, 브라우저는 스크립트에서 시작한 교차 출처 HTTP 요청을 제한
  - 예로, XMLHttpRequest, Fetch API 는 동일 출처 정책을 따른다.

![CORS](https://mdn.mozillademos.org/files/14295/CORS_principle.png)

- 최신 브라우저는 헤더와 정책 집행을 포함한 클라이언트 측 교차 출처 공유를 처리
- CORS 표준에 맞춘다. => 서버에서도 새로운 요청과 응답 헤더를 처리해야 한다는 것.

- 결국 __도메인__ 혹은 __포트__ 가 다른 서버의 자원을 요청 할 때 나타난다.


## 어떤 요청이 CORS를 사용하나요?

- 교차 출처 공유 표준은 다음과 같은 경우에 사이트간 HTTP 요청을 허용

  - XMLHttpRequest, Fetch API
  - 웹 폰트(CSS, @font-face에서 교차 도메인 폰트 사용시)
  - WebGL 텍스쳐
  - drawImgae()를 사용해 캔버스에 그린 이미지/비디오 프레임
  - 이미지로부터 추출하는 CSS shape

## 기능적 개요

- CORS란 Cross Origin Resource Sharing의 약자로
  - 현재 도메인과 다른 도메인으로 리소스가 요청될 경우를 말한다.

- 예를 들어 도메인 http://A.com 에서 읽어온 HTML페이지에서
  - 다른 도메인 http://B.com/image.jpg 를 요청하는 경우이다.

- 이런 경우에 해당 리소스는 cross-origin HTTP 요청에 의해 요청된다.

- 요청을 서버로 전송할 때 브라우저는 Origin 헤더를 추가해 보내는데, 서버는 이를 받고 응답 헤더에 Access-Control-Allow-Origin 를 추가해 보낸다. 
  - Access-Control-Allow-Origin 헤더의 값에는 요청을 허용할 도메인이 담긴다. (모든 도메인으로부터의 요청을 허용할 경우 '*' 이 명시된다.)

- 보안 상의 이유로 브라우저는 CORS를 제한하고 있다.

- 하지만 SPA(Single Page Application)의 경우에는
  - RESTful API를 기반으로 비동기 네트워크 통신을 하기 때문에
  - API 서버와 웹 페이지 서버가 다를 수 있다.

- 이런 경우에 API 서버로 요청을 할 시에 CORS 제한이 걸리게 된다.

## 해결책

- Access-Control-Allow-Origin

- 이를 해결하기 위해서 가장 간단한 방법은 서버(API 서버)의 응답 헤더를 변경해주는 것이다.

- 서버의 헤더 중에는 Access-Control-Allow-Origin 프로퍼티가 있다.

- 이 프로퍼티에 CORS를 허용해 줄 도메인을 입력하는 곳이다.

- 모든 곳에서 CORS를 허용하기 위해서는 모두를 의미하는 *를 입력하면 된다.

```bash
- Example -
if( 모든 곳에 CORS를 허용 )
Access-Control-Allow-Origin: *

if( 특정 도메인에만 허용하길 원한다면 )
Access-Control-Allow-Origin: http://A.com, http://B.com, ...
```
