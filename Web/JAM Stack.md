# JAM Stack 개념 정리하기

- [참고 포스팅](https://medium.com/@pks2974/jam-stack-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-17dd5c34edf7)

- JAM stack은 Javscript, Api, Markup Stack의 약자이다.

- Javascript와 API 그리고 Markup(HTML) 만으로 이루어진 웹의 구성을 이야기하는것인데, SPA와 약간 다르다

## SPA(Single Page Application)와 CSR(Client-Side Rendering)

- 웹은 보통 완성된 Static HTML과 CSS를 네트워크로 전달 받아서, 화면을 보여준다.
- 서버가 동적으로 HTML을 생성할 수 있게 되면서 Server Side Rendering(SSR)의 개념이 등장하였고, 사용자의 요청에 따라 HTML을 만들어서 전달하고, 화면을 보여줄 수 있게 되었다.

- SSR만으로는 더 많은 HTML 조작이 힘들어져, 보조 역할만 하던 Javscript, jQuery의 만남과 함께 역할이 커졌다.

- JS를 통해 화면을 조작하고, JS와 함께 브라우저의 성능이 발전해 AngularJS, React, Vue 등의 가상 DOM을 조작하기 쉽게 도와주는 라이브러리와 웹 프레임워크가 등장
- Client-Side Rendering 개념이 등장

- 서버 상에서 HTML을 만들어 전달하는 것보다(SSR), 클라이언트 상에서 HTML을 만드는것이 (CSR), 서버 비용과 렌더링 속도에서 훨씬 유리한 환경이 되었다.

- 페이지를 이동할 때 마다 HTML을 새로 그리는 것 보다, 필요한 데이터만 받아서 JS로 화면을 그리는 것이 유리해졌다.
- SPA(Single Page Application)이 등장
  - 정보가 없는 HTML과 매우 큰 Javascript 그리고 분리된 환경의 API

## SSR이 필요한 이유

- SEO를 맞추기 힘들어 졌기 때문
- JS로 조작하는 환경은 매력적, SSR과 CSR의 혼합한 형태를 만들어내게 되는데 
- JS에서 사용하는 환경을 그대로 이용한 __Server__ 인 Angular Universal와 NextJS 그리고 NuxtJS가 등장

- 첫 페이지는 SSR 형태로 HTML을 만들어 보여주고, 이후 모든 화면 조작과 이후 Rendering(CSR)을 JS가 처리하게 하는 하이브리드형태

- SEO에 대응할 수 있게 된다.

## JAM Stack

- Javascript와 API 그리고 Markup으로 구성된 최신 웹 사이트를 구성하는 방법

> Javscript : Client의 모든 처리는 Javscript에게 맞긴다.

> API : 모든 기능 및 비즈니스 로직은 재사용 가능한 API로 추상화한다.

> Markup : SSG(Static Stie Generator)나 Template Engine(Webpack 등)을 이용하여 Markup을 미리 생성한다.

- 여기에서 가장 중요한 부분은 __Markup__ 부분이다.

## Markup

- Markup을 만드는 방법
  - HTML을 직접 작성
  - Template Engine 같은 툴을 이용
  - Jekyll(Ruby), Hugo(go), Nuxt(vue), Next(react), Gatsby 같은 정적 사이트 생성기 (Static Site Generator, SSG)를 이용해서 Static HTML을 생성할 수도 있다.

- 미리 작성된 Static HTML은 웹서버의 리소스를 쓸 필요없이, 사용자에게 HTML만을 전달 해주면 된다.

- 여기서의 장점
  - Static HTML을 CDN을 통해 Cache하고 배포하여, 빠른 속도를 유지한다.
  - 따로 동적으로 HTML을 생성하지 않기 때문에, 따로 웹서버가 필요 없어 서버 비용이 높지 ㅇ낳다.

- 하지만 모든 HTML이 Static HTML만으로 이루어진 것을 뜻하지는 않는다.
  - 모든 Markup을 정적으로 유지하게 되면, 최신 데이터를 유지하는게 어렵기 때문

- 중요한 것은 최대한 HTML Build을 빌드하여, Cache 하고 사용자를 위한 First Meaningful Paint(첫 페이지 호출)의 속도를 높히자는 점

## 간단한 JAM stack 과정

- gatsbyjs와 netlify를 활용
  - gatsbyjs는 React와 GraphQL를 이용한 정적 사이트 생성기  
  - netlify는 javascript 코드를 빌드하고 배포하고 운영할 수 있게 도와주는 플랫폼

1. Github으로 프로젝트 관리
1. Gatsbyjs(SSG)로 정적 사이트 생성기 구축
1. Netlify에 배포환경 구성
1. Github의 코드가 변경되면, Netlify에서 빌드를 시작
1. Netlify로 Gatsbyjs으로 빌드하고, 사이트를 배포

- 한번 배포된 Package는 더 이상 빌드를 위한 웹서버의 자원은 필요하지 않게 되고, 모든 처리는 Javscript와 API에 의해 이루어지게 된다.

- JAM stack은 하나의 개념
  - 특정 라이브러리나 플랫폼을 이용하지 않고, 직접 빌드툴 혹은 프레임워크를 만들거나, 호스팅 서버나 CDN을 운영해도 문제가 없다.