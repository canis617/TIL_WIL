# Vuex란

- [출처](https://ict-nroo.tistory.com/106)

- VueJS의 상태 관리 라이브러리로 공식 라이브러리이며 같이 업데이트 된다!
- 무수히 많은 컴포넌트의 데이터를 관리하기 위한 상태 관리 패턴이자 라이브러리
- React의 Flux 패턴에서 기인하였다고 한다.
- Vue.js 중고급 개발자로 성장하기 위한 필수 관문

## Flux란

- MVC 패턴의 복잡한 데이터 흐름 문제를 해결하는 개발 패턴 - Undirectional data flow
- 아래의 순서로 __단일방향__ 으로만 흐름이 흘러간다.
  1. actions : 화면에서 발생하는 이벤트 또는 사용자의 입력
  1. dispatcher : 데이터를 변경하는 방법, 메서드
  1. model : 화면에 표시할 데이터
  1. view : 사용자에게 비춰지는 화면, 화면에서 다시 action을 호출
  1. 1 > 2 > 3 > 4 flow 반복

## MVC 패턴의 문제점

- 뷰와 모델이 양방향 통신이 가능하므로 하나의 뷰가 모듈을 변경하였을 때, 다시 그 모듈이 연관된 뷰들을 갱신하고, 업데이트 된 뷰들이 연관된 모델을 다시 갱신하고, 엮이는 관계를 추적하기 힘듬
  - ex) 페이스분 채팅화면, 어느 사용자는 읽었고, 어느 사용자는 안 읽었고 이런 부분을 처리하기 힘듬

- 기능 추가 및 변경에 따라 생기는 문제점을 예측하기 히믇ㅁ
- 앱이 복잡할수록 생기는 업데이트 루프도 복잡해짐

<p align="center">
    <img src="https://github.com/namjunemy/TIL/blob/master/Vue/img/06.PNG?raw=true">
</p>

## Flux 패턴의 단방향 데이터 흐름

- 데이터의 흐름이 여러갈래로 나뉘지 않고 단방향으로 처리 가능
  - 데이터 흐름이 단방향이기 때문에 예측이 가능.
  - vue.js에서 생각하면 '상위에서 하위로 props가 내려가고, 하위에서 상위로 event가 올라간다.'
- 패턴 자체에서 데이터의 흐름을 정형화 시켜 향후 발생할 수 있는 문제를 방지

<p align="center">
    <img src="https://github.com/namjunemy/TIL/blob/master/Vue/img/07.PNG?raw=true">
</p>

## 상태 관리(State Management)란

- 상태 관리란 여러 컴포넌트 간의 데이터 전달과 이벤트 통신을 한곳에서 관리하는 패턴을 의미한다.
- 뷰와 성격이 비슷한 프레임워크인 리액트(React)에서는 Redux, Mobx와 같은 상태 관리 라이브러리를 사용하고 있고 뷰에서는 Vuex라는 상태 관리 라이브러리를 사용합니다.

## 상태 관리로 해결할 수 있는 문제점

- 상태 관리는 중대형 규모의 웹 애플리케이션에서 컴포넌트 간에 데이터를 더 효율적으로 전달할 수 있습니다.
- 일반적으로 앱의 규모가 커지면서 생기는 문제점들은 다음과 같습니다.
  1. 뷰의 컴포넌트 통신 방식인 props, event emit 때문에 중간에 거쳐할 컴포넌트가 많아지거나
  1. 이를 피하기 위해 Event Bus를 사용하여 컴포넌트 간 데이터 흐름을 파악하기 어려운 것
- 이러한 문제점을 해결하기 위해 모든 데이터 통신을 한 곳에서 중앙 집중식으로 관리하는 것이 상태 관리입니다.

## Vuex가 필요한 이유

- 컴포넌트 간의 통신이나 데이터 전달을 좀 더 유기적으로 관리할 필요성이 생긴다.

- 복잡한 애플리케이션에서 컴포넌트의 개수가 많아지면 컴포넌트 간에 데이터 전달이 어려워짐
  - Vue.js의 기본 개념을 공부했다면 알겠지만, 로그인 폼에서 id를 하위 컴포넌트로 계속해서 전달해 내려갈 때, 중간에 거치는 컴포넌트들이 많아 질수록 계속해서 props를 선언해야 하므로 불편하다.

- 이벤트 버스로 해결
  - 어디서 이벤트를 보냈는지, 혹은 어디서 이벤트를 받았는지 알기 어렵다.

```js
// Login.vue
eventBus.$emit('fetch', loginInfo);
​
// List.vue
eventBus.$on('display', data => this.displayOnScreen(data));
​
// Chard.vue
eventBus.$emit('refreshData', chartData);
```

- 즉 컴포넌트 간 데이터 전달이 명시적이지 않다.

## Vuex로 해결 가능한 문제

- MVC 패턴에서 발생하는 구조적 오류
- 컴포넌트간 데이터 전달 명시
- 여러 개의 컴포넌트에서 같은 데이터를 업데이트 할 때 동기화 문제(mutation, actions)

## Vuex 컨셉

- __State__ : 컴포넌트 간에 공유하는 데이터 (data())
- __View__ : 데이터를 표시하는 화면 (template)
- __Action__ : 사용자의 입력에 따라 데이터를 변경하는 (methods)

- 단방향 데이터 흐름 처리를 단순하게 도식화 한 그림
  - View(Template)에서 버튼을 클릭했을 때, 클릭이라는 Action(Methods)이 발생
  - 해당 Action이 동작을 통해서 State(data)를 변경

<p align="center">
    <img src="https://github.com/namjunemy/TIL/blob/master/Vue/img/09.PNG?raw=true">
</p>

## Vuex 구조

- 뷰 컴포넌트 -> 비동기 로직 -> 동기 로직 -> 상태
  - 시작점은 Vue Component이다.
  - 컴포넌트에서 비동기 로직(Method를 선언해서 API콜을 하는 등)인 Actions를 콜한다.
  - Actions는 비동기 로직만 처리할 뿐 State(Data)를 직접 변경하지는 않는다.
  - Actions가 동기 로직인 Mutations를 호출해서 State(Data)를 변경한다.
  - Mutations에서만 State(Data)를 변경할 수 있다.


<p align="center">
    <img src="https://github.com/namjunemy/TIL/blob/master/Vue/img/10.PNG?raw=true">
</p>
