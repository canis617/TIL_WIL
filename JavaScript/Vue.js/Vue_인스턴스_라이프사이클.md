# Vue LifeCycle

- Vue LifeCycle은 크게 Creation, Mounting, Updating, Destruction으로 나뉜다.

- [참조사이트1](https://blog.martinwork.co.kr/vuejs/2018/02/05/vue-lifecycle-hooks.html)

![Vue LifeCycle](https://miro.medium.com/max/700/1*tnSXRrpLBYmfHnIagITlcg.png)

## beforeCreate

- 이 훅은 컴포넌트가 돌아갈 때 가장 먼저 실행되는 훅이다.
- Vue 인스턴스가 생성된 직후에 실해오디는 훅으로서, 아직 데이터와 이벤트가 생성되지 않아 접근할 수 없는 단계이다.

```js
var app = new Vue({
   el: '#app',
   beforeCreate () {
    console.log('인스턴스 초기화 후, 데이터 관찰/이벤트/감시자 설정 전 호출됩니다.')
   }
 })
```

## created

- 이 훅은 beforeCreate의 다음 단계이다.
- beforeCreate 훅이 호출된 직후 데이터와 이벤트가 초기화 되어 created 훅에서는 데이터와 이벤트에 접근할 수 있다.
- 이 다음 단계에서 el option에 대해 체크하는것만 봐도 아직 virtual DOM은 생성되지 않은 상태라는 것을 알 수 있다.

```js
var app = new Vue({
   el: '#app',
   created() {
   	  //$el 속성을 사용할 수 없습니다.
   	  console.log('인스턴스가 작성된 후 동기적으로 호출됩니다.');
    },
 })
```

- vuex에서 초기 데이터를 패치하는 actions을 실행하는 곳으로 많이 사용(DOM 생성전에)

## beforeMount

- 이 훅은 이 후부터는 컴포넌트에 접근할 수 있다.
- 많은 블로그들에서 이 단계에서 초기 데이터를 패치하는 용도로 사용하지 말라고 되어있다.
- 오히려 created 단계에서 사용하는 것을 권장하는 데 그 이유에 대해서는 따로 기술을 하지 않아 이해하는데 어려움이 많았다.
- 이 부분에 대한 것은 Vuejs Korea 커뮤니티에 질문한 결과 서버 사이드 랜더링 사용성에서 문제가 터진다고 한다.
- 서버에서 실행된 것과 클라이언트에서 실행된 것이 다를 경우, Nuxt에서 경고를 주거나 해당 Dom 노드를 제거한다고 한다.
- 서버사이드 랜더링의 기본 전제가 서버에서 내려온 Dom + Data가 마운트 된 후, 클라이언트에서 바꾸는 것인데 서로 일치가 되지 않으면 문제가 된다고 한다.
- 이 훅 이후에 가상돔의 el을 생성하여 __아직까지는 el에 접근할 수 없는 단계__ 이다.

```js
var app = new Vue({
   el: '#app',
   beforeMount() {
     //$el 속성을 사용할 수 없습니다.
     console.log('컴포넌트가 마운트되기 직전에 호출되며, el이 아직 생성되지 않아 접근할 수는 없지만 곧 생성됩니다.');
    },
 })
```

## mounted

- 이 훅에서는 el을 포함한 모든 컴포넌트에 접근할 수 있다.
- mounted 훅은 Vue의 셍명주기 중에서 제일 많이 사용하는 훅이다.
- 하지만 이 또한 블로그에 따라 기재되는 사용성이나, 방식이 다르다.
- 어떤 블로그에서는 mounted 단계에서 처음 init단계의 data를 fetch해오는 역할로 사용하라고 되어있으며, 또 다른 블로그에서는 created 단계에서 fetch data하는 것을 권장한다고 되어 있다.

```js
var app = new Vue({
   el: '#app',
   mounted() {
     console.log('$el을 포함한 모든 컴포넌트에 접근할 수 있다.');
},
```

## beforeUpdate

- 이 훅은 컴포넌트가 마운트가 다 된 후, 데이터의 변경이 감지되었을 때 DOM에 data를 업데이트 하여 랜더링을 하기 전에 실행된다.
- 이 단계에서는 컴포넌트의 새로운 state가 접근 할 수 있다.

```js
var app = new Vue({
   el: '#app',
   beforeUpdate() {
     console.log('data가 업데이트되어 DOM 을 re-render 시키기 전에 실행됩니다.');
    },
 })
```

## updated

- 이 훅은 beforeUpdate와는 반대라고 생각하면 된다. beforeUpdate는 re-render를 하기 전에 호출된다고 하면 이 훅 같은 경우는 re-render 된 후 호출된다.
- 만약 DOM이 re-render 된 후, 접근할 경우가 있다면 해당 훅을 이용하면 된다.

```js
var app = new Vue({
   el: '#app',
   beforeUpdate() {
       console.log('data가 업데이트되어 DOM아 re-render 된 후, 실행된다.');
    },
 })
```

## beforeDestory

- 이 훅은 vue 인스턴스가 파괴되기 전에 호출되는 훅이다.
- 만약에 이미 바인딩 된 이벤트를 제거해야할 때 해당 훅을 이용하면 된다.
- 한 예로 이 훅을 활용했던 상황은 다음과 같다.

```js
var app = new Vue({
   el: '#app',
   created () {
       document.body.addEventListener('click', funcion);
   },
   beforeDestroy () {
       document.body.removeEventListener('click', funcion);
   },
 })
```

- document.body에 특정 이벤트를 걸어놨다가, 이 후에 해당 이벤트는 초기화 시켜줄 때 사용을 하였다. 만약 beforeDestroy 단계에서 이벤트를 해제시켜주지 않는다면, document.body 에는 계속해서 event가 바인딩 되어 실행 될 것이다.

## destroyed

- 이 훅에서는 vue 인스턴스가 모두 사라진 후 실행되는 훅이다.
- vue 인스턴스의 component에 걸려있는 모든 이벤트가 해제된다.

```js
var app = new Vue({
   el: '#app',
   destroyed () {
       console.log('vue 인스턴스가 destroy 된 후 실행되는 hook이다.');
   },
 })
```