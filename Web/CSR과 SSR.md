# CSR과 SSR

## MVC 패턴

- 소프트웨어 공학의 기본적인 개발 모델

- Model
  - 데이터베이스나 파일을 담는다.
  - 사용자가 컨트롤러를 통해 데이터를 바꾸고자 한다면 이 요청을 모델이 듣고 컨트롤러의 제어에 맞게 데이터베이스의 데이터가 수정되게 된다.

- View
  - 뷰는 사용자에게 데이터를 보여주는 기능을 수행한다. 사용자가 데이터를 조회하기 위해서는 뷰를 통해 보게 된다.
  - 요청을 받은 뷰는 모델에 데이터 질의어를 수행한다. 뷰는 데이터를 제어할 권한이 없다.

- Controller
  - 컨트롤러는 사용자의 요청을 들어주는 역할이다.
  - 웹 환경이라면 우리는 URL을 통해 요청을 제어하게 되는데, URL 역할이 바로 컨트롤러다.
  - 요청을 들은 컨트롤러는 모델에 질의를 하고, 모델의 답변을 받아 뷰에 데이터를 제정하게 된다.

## SSR

- 서버 사이드 렌더링 (Server-Side Rendering)
- 기존의 웹 구조에서는 클라리언트에서 데이터 수정이 불가능 했다.
- 클라이언트와 서버의 구분이 명확하게 나뉘어 클라이언트 사이드에서는 데이터 수정이 전혀 불가능하다.
- 전통적으로 하는 개발에서는 HTML에서는 데이터 수정이 불가능 해 클라이언트의 기능은 단순히 데이터 조회와 요청을 보내는 기능만 수행 했다.

![SSR](https://t1.daumcdn.net/cfile/tistory/99CEDE3B5DBE4FEE23)
![SSR](https://d2.naver.com/content/images/2020/06/ssr.png)

- 여기서 말하는 렌더링이란 우리가 현재 작성해놓은 HTML 코드를 사용자가 볼 수 있는 화면으로 바꾸는 것
- 즉, 서버 사이드 렌더링이란 HTML을 보게 만들어 주는 작업을 서버에서 수행한다.
- SSR에서는 라디오 버튼 하나만 눌러 웹 페이지 상의 데이터를 교체 한다고 하더라도 서버의 수행작업을 거치게 되는 방식.

### SSR의 장점

- 초기 렌더링 속도가 매우 빠르다
- 검색 엔진 최적화(SEO)를 사용할 수 있다.

### SSR의 단점

- 간단한 데이터 수정에도 서버를 거쳐야 하는 불편함이 따른다.
- 초기 렌더링 수행은 빠르지만 연속적으로 렌더링 수행을 할 경우 서버에 과부화가 올 수 있다.

## CSR

- 클라이언트 사이드 렌더링 (Client-Side Rendering)
- 네트워크 기술이 발전됨에 따라 사용자들은 실시간으로 브라우저를 통해 데이터를 빠르게 확인 하고 싶어한다.
  - 모바일 환경이서 이러한 특징이 두드러지는편
- 기존의 SSR를 사용하게 되면 서버에 엄청난 과부하가 생기게 되는 불편함이 생겼다.
- 이로 인해 클라이언트에 모델-뷰-컨트롤러를 장착하는 CSR이 발달되었다.

![CSR](https://t1.daumcdn.net/cfile/tistory/99D5DB395DBE55AF28)
![CSR](https://d2.naver.com/content/images/2020/06/csr.png)

- 사용자가 필요한 부분만 서버에서 로딩하기 때문에 주고 받게 되는 데이터의 양이 현저하게 줄어든다.
- 작은 데이터 수정을 위해서 HTML을 통으로 내려받는 불편함이 존재했지만, 다양한 프레임워크에서 클라이언트 사이드 렌더링 방식을 채택해 좀 더 간편하게 데이터를 주고받을 수 있게 되었다.

- 현재 다양한 프레임워크에서 CSR 방식의 개발을 제공

- AngularJS, BackboneJS 등 SPA 개발에 쉬운 JS 프레임워크가 등장하고, 여기에 클라이언트가 무거워지니 view만 관리하자는 철학의 ReactJS가 등장

### CSR의 장점

- 서버와 클라이언트 사이의 데이터 양과 트래픽이 현자하게 감소
- 연속된 렌더링으로 인한 과부하를 줄일 수 있다.

### CSR의 단점

- 검색 엔진 최적화(SEO) 사용이 불가능
- 렌더링을 위해 필요한 작업이 많아진다. (유지보수가 많아짐)

## CSR 에서 SSR으로

- CSR의 경우 javascript가 필요하기 때문에 웹 크롤러들은 내용을 알 수 없고, 제대로된 데이터를 수집 할 수 없게 된다.
- 초기 페이지는 HTML을 활용한 서버사이드 렌더링을 사용하고, 그 후에 모든 페이지 로드에는 클라이언트 사이드 렌더링을 활용하는 방안으로 변화중

## 검색 엔진 최적화(SEO)

- 검색 엔진 최적화 (Search Engine Optimization)
- 대부분의 웹 크롤러, 봇들이 자바스크립트 파일을 실행시키지 못하고, HTML에서만 컨텐츠를 수집한다.
- 때문에 CSR 방식으로 개발된 페이지를 빈 페이지로 인식하게 된다.
- 검색엔진에 제대로 노출이 되지 않으면 웹페이지의 유입이 줄어드는 악순환이 반복된다.

## 보안

- 기존의 SSR에서는 사용자에 대한 정보를 서버 측에서 세션으로 관리를 하지만
- CSR 방식은 클라이언트 측의 쿠키 말고는 사용자에 대한 정보를 저장할 공간이 마땅치 않다.

## Javascript

- 서버와 클라이언트 개발에 javascript가 사용되는 이유
- 서버 사이드 언어 : Node.js, Nuxt.js
- 클라이언트 사이드 언어 : Vue.js, React.js, Angular.js

- 동적인 웹사이트 개발이 가능해지기 때문
- HTML은 정적인 언어이기 때문에 동적으로 작용하는 프로그래밍 언어를 위해 Javascript 를 사용

