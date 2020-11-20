# Vue는 무엇인가

- 스택오버플러우에서 발표한 2019년 설문조사 중 `“Most Loved, Dreaded, and Wanted Web Frameworks”`항목에서 2등한 Vue. ~~1등은 React~~

![asdf](https://pbs.twimg.com/media/D365k6EUwAA81hU?format=jpg&name=small)

## 왜 Vue js 인가

- 가독성이 높고 직관적이다.
- ReactJS와 유사항 성능을 보여준다.

- 안정성
  - Evan You 개인의 프로젝트가 아닌 [팀 체제](https://kr.vuejs.org/v2/guide/team.html)로 안정적인 에코 시스템을 관리하고 장기적인 [로드맵](https://github.com/vuejs/vue/projects/6)을 가지고 업데이트가 이루어지고 있다고 한다.

- 빌드 시스템, 라우팅, 상태관리등 실제 서비스에서 필요한 기능들을 공식적으로 지원한다.
  - [Vuex](JavaScript\Vue.js\Vuex.md)는 Vue.js만큼이나 쉽게 접근가능하고, 코드도 간결해 사용하면 좋다!

- 유연함
  - 플러그인, 믹스인 등의 방법으로 순수 자바스크립트 오픈소스를 뷰에서 사용할 수 있도록 매핑하기 쉽다.
  - 심지어 기존 자바스크립트 라이브러리(jquery 등등)를 그대로 붙여서 사용할 수 도 있다.

## vue js 에서 알아두면 좋은 것들


### Computed Caching vs Methods

- [링크](https://v3.vuejs.org/guide/computed.html#computed-caching-vs-methods)

### watch

- 서비스를 개발할 때 watch는 최소로 사용하자? > 데이터간 의존 관계 복잡도를 낮추는 데 좋다
- 확인 [링크](https://v3.vuejs.org/guide/computed.html#computed-caching-vs-methods)

### 클래스 바인딩

- 뷰 데이터 값에 따라 클래스를 동적으로 바인딩 하는 부분의 예시
- [링크](https://v3.vuejs.org/guide/class-and-style.html#object-syntax)

```html
<div :class="classObject"></div>
```

```js
data() {
  return {
    isActive: true,
    error: null
  }
},
computed: {
  classObject() {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### 스타일 바인딩

- 인라인 스타일을 한 줄로 하는것보다 객체 형태로 하면 코드가 간결

```html
<div :style="styleObject"></div>
```

```js
data() {
  return {
    styleObject: {
      color: 'red',
      fontSize: '13px'
    }
  }
}
```

### v-if와 v-show의 차이점

- 초기 렌더링 비용과 변경 렌더링 비용을 나누어 비교해야함
- [링크](https://v3.vuejs.org/guide/conditional.html#v-if-vs-v-show)

### 목록 필터링 & 오더링 팁

- 특정 목록을 필터링 및 오더링 해야 할 때 computed와 methods를 어떻게 구분해야 써야 하는가
- [링크](https://v3.vuejs.org/guide/computed.html#computed-caching-vs-methods)

### `v-on` 디렉티브의 이벤트 제어자(event modifiers)에 대한 적절한 예시와 설명

- parevent, stop, capture, self 등 이벤트 처리 방식에 대한 예제와 주석이 기존 문서보다 더 와닿도록 자세하게 기술
- [링크](https://v3.vuejs.org/guide/events.html#event-modifiers)

### 템플릿 표현식에 이벤트 제어자를 적용했을 때의 장점 설명

- HTML 코드에 이벤트 제어 로직이 아래와 같이 붙어 있는게 왜 장점인지 코드 가독성, 테스팅, 리팩토링 관점에서 잘 설명해주고 있습니다.

```html
<button @click.prevent="doSomething">click me</button>
```

### 컴포넌트 등록 방식의 비교

- 뷰 3 문서 대부분의 컴포넌트 관련 코드는 전역 컴포넌트 코드로 예시를 들고 있습니다.
- 그런데 실제로 서비스를 구현할 때는 주로 로컬 컴포넌트 방식이 사용되는데 이에 대한 차이점을 아래와 같이 잘 설명하고 있습니다.

> "전역으로 뷰 인스턴스에 등록할 경우 사용하지 않더라도 최종 빌드에 해당 자원이 포함된다. 따라서 사용자가 불필요한 리소스를 다운로드 받아야 하는 단점이 생긴다."

### 슬롯의 렌더링 유효 범위

- 슬롯을 활용할 때 데이터의 유효 범위가 상위 컴포넌트와 하위 컴포넌트 중 어느 곳에 연관된 것인지 그림으로 설명
- [링크](https://v3.vuejs.org/guide/component-slots.html#render-scope)

### 믹스인

- 믹스인의 단점 2가지
- [Composition API](https://v3.vuejs.org/guide/composition-api-introduction.html)를 사용 해야 하는 이유

### 뷰 템플릿 익스플로러

- [링크](https://template-explorer.vuejs.org/#%3Cdiv%20id%3D%22app%22%3E%7B%7B%20msg%20%7D%7D%3C%2Fdiv%3E)

### 뷰 반응성(Reactivity) 설명

- 뷰의 강점인 반응성 체계(Reactivity System) > 데이터가 바뀌면 화면이 갱신된다.

