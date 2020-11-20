# vue - router

- vue 에서 제공하는 router


## 네이게이션 가드

- 참고 자료 : [https://joshua1988.github.io/web-development/vuejs/vue-router-navigation-guards/](https://joshua1988.github.io/web-development/vuejs/vue-router-navigation-guards/)

- 뷰 라우터로 특정 URL에 접근 할 때 해당 URL의 접근을 막는 방법
  - 예로, 사용자의 인증정보가 없으면 특정 페이지에 접근을 막는 등에 사용되는 기술

- 종류

  - 전역 가드 : 애플리케이션 전역에서 동작
  - 라우터 가드 : 특정 URL에서만 동작
  - 컴포넌트 가드 : 라우터 컴포넌트 안에 정의

### 전역 가드

- 라우터 인스턴스를 참조하는 객체로 설정 가능

- 인스턴스 생성 후 .beforeEach() API 호출

```js
var router = new VueRouter();

router.beforeEach(function (to, from, next) {
  // to : 이동할 url
  // from : 현재 url
  // next : to에서 지정한 url로 이동하기 위해 꼭 호출해야 하는 함수
});
```

- beforeEach()를 호출하면 다음의 3가지 인자를 받음
  - to : 이동할 url 정보가 담긴 라우터 객체
  - from : 현재 url 정보가 담긴 라우터 객체
  - next : to 에서 지정한 url로 이동하기 위해 꼭 호출해야 하는 함수

- router.beforeEach()를 호출하고 나면, 모든 라우팅이 대기 상태가 됨.
- 원래 url이 변경되고 나면 해당 url에 따라 화면이 자연스럽게 전환 되어야하나, 전역 가드가 설정되었기 때문에 화면이 전환되지 않음
- next()를 호출해줘야 해당 url로 전환
- next() 호출 이전까지는 화면이 전환되지 않음

---

### 기본 예제

```js
// 라우터 컴포넌트
var Login = { template: '<p>Login Component</p>' };
var Home = { template: '<p>Home Component</p>' };

// 라우팅 정보
var router = new VueRouter({
  routes: [
    { path: '/login', component: Login },
    { path: '/home', component: Home }
  ]
});
```

- 전역 가드를 설정하는 코드 추가

```js
router.beforeEach(function (to, from, next) {
  console.log('every single routing is pending');
});
```

- 로그만 띄우고, 실제로 이동은 이루어지지 않음

### 전역 가드에 페이지 인증하기

- Login 컴포넌트에 meta 정보를 추가

```js
var router = new VueRouter({
  routes: [
    // meta 정보에 authRequired라는 Boolean 값 추가
    { path: '/login', component: Login, meta: {authRequired: true} },
    { path: '/home', component: Home }
  ]
});
```

- beforeEach()의 콜백 함수에 사용자 인증 여부 체크하는 로직 추가

```js
router.beforeEach(function (to, from, next) {
  // to: 이동할 url에 해당하는 라우팅 객체
  if (to.matched.some(function(routeInfo) {
    return routeInfo.meta.authRequired;
  })) {
    // 이동할 페이지에 인증 정보가 필요하면 경고 창을 띄우고 페이지 전환은 하지 않음
    alert('Login Please!');
  } else {
    console.log("routing success : '" + to.path + "'");
    next(); // 페이지 전환
  };
});
```

---

## 라우터 가드와 컴포넌트 가드

- 같은 원리로 작동하나, URL 이동을 막기 위해 사용하는 API만 조금 다름

### 라우터 가드

- 특정 라우터에 가드를 설정하는 방법

```js
var router = new VueRouter({
  routes: [
    {
      path: '/login',
      component: Login,
      beforeEnter: function(to, from, next) {
        // 인증 값 검증 로직 추가
      }
    }
  ]
})
```

### 컴포넌트 가드

- 특정 컴포넌트에 가드를 설정하는 방법

```js
const Login = {
  template: '<p>Login Component</p>',
  beforeRouteEnter (to, from, next) {
    // Login 컴포넌트가 화면에 표시되기 전에 수행될 로직
    // Login 컴포넌트는 아직 생성되지 않은 시점
  },
  beforeRouteUpdate (to, from, next) {
    // 화면에 표시된 컴포넌트가 변경될 때 수행될 로직
    // `this`로 Login 컴포넌트를 접근할 수 있음
  },
  beforeRouteLeave (to, from, next) {
    // Login 컴포넌트를 화면에 표시한 url 값이 변경되기 직전의 로직
    // `this`로 Login 컴포넌트를 접근할 수 있음
  }
}
```