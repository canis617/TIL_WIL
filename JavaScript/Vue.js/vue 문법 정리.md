# Vue 문법 정리

## vue 폴더 구조 정리

```bash
.
├─ README.md
├─ index.html
├─ webpack.config.js
├─ package.json
└─ src
   ├─ main.js
   ├─ App.vue
   ├─ components        컴포넌트
   │  ├─ common
   │  └─ ...
   ├─ routes            라우터
   │  ├─ index.js
   │  └─ routes.js
   ├─ views             라우터 페이지
   │  ├─ MainView.vue
   │  └─ ...
   ├─ store             상태 관리
   │  ├─ auth
   │  ├─ index.js
   │  └─ ...
   ├─ api               api 함수
   │  ├─ index.js
   │  ├─ users.js
   │  └─ ...
   ├─ utils             필터 등의 유틸리티 함수
   │  ├─ filters.js
   │  ├─ bus.js
   │  └─ ...
   ├─ mixins            믹스인
   │  ├─ index.js
   │  └─ ...
   ├─ plugins           플러그인
   │  ├─ ChartPlugin.js
   │  └─ ...
   ├─ translations      다국어
   │  ├─ index.js
   │  ├─ en.json
   │  └─ ...
   ├─ images            이미지
   ├─ fonts             폰트
   └─ assets            기타 자원
```

---

## slot 자세히 알아보기

- 컴포넌트의 마크업을 확장하는 방법

- __slot__ 은 컴포넌트의 재사용성을 높여주는 기능
- 특정 컴포넌트에 등록된 하위 컴포넌트의 마크업을 확장하거나 재정의 할 수 있다.

```html
<!-- ButtonTab.vue -->
<template>
  <div class="tab panel">
    <!-- 탭 헤더 -->
    <slot></slot>
    <!-- 탭 본문 -->
    <div class="content">
      Tab Contents
    </div>
  </div>
</template>
```

- 탭 헤더에 들어갈 구체적인 태그를 정하지 않고 `slot`으로 남겨놓음
- 이 컴포넌트를 등록한 상위 컴포넌트에서 `<slot>` 구문을 구현하지 않으면 해당 부분은 공백으로 표시

```html
<!-- TabContainer.vue -->
<template>
  <button-tab>
    <!-- slot 영역 -->
    <h1>First Header</h1>
  </button-tab>
  <button-tab>
    <!-- slot 영역 -->
    <h1>Second Header</h1>
  </button-tab>
  <button-tab>
    <!-- slot 영역 -->
    <h1>Third Header</h1>
  </button-tab>
</template>

<script>
export default {
  components: {
    ButtonTab
  }
}
</script>
```

- slot 영역을 각기 다른 헤더의 내용으로 정의
- slot 태그를 정의하지 않았다면, 컴포넌트를 등록하는 시점에서 마크업을 재정의 할 수 없음

### Named Slot

- 위에서는 슬롯의 개념을 이해하기 위해 1개의 슬롯만 사용했습니다. 
- 슬롯은 name 속성을 지정하여 여러 개 사용할 수도 있습니다. 
- 좀 전 예제에 네임드 슬롯을 적용해보겠습니다.

```html
<!-- ButtonTab.vue -->
<template>
  <div class="tab panel">
    <!-- 탭 헤더 -->
    <slot name="header"></slot>
    <!-- 탭 본문 -->
    <slot name="content"></slot>
  </div>
</template>
```

```html
<!-- TabContainer.vue -->
<template>
  <button-tab>
    <!-- slot 영역 -->
    <h1 slot="header">First Header</h1>
    <div slot="content" class="content">Tab Contents #1</div>
  </button-tab>
  <button-tab>
    <!-- slot 영역 -->
    <h1 slot="header">Second Header</h1>
    <div slot="content" class="content">Tab Contents #2</div>
  </button-tab>
  <button-tab>
    <!-- slot 영역 -->
    <h1 slot="header">Third Header</h1>
    <div slot="content" class="content">Tab Contents #3</div>
  </button-tab>
</template>

------------------------

<button-tab>

  <!-- slot 영역 -->
  <template slot="header">
    <h1>First Header</h1>
  </template>
  <template slot="content">
    <div class="content">Tab Contents #1</div>
  </template>
</button-tab>
```

---

## v-model 자세히 알아보기

- `v-model`의 사용 방법

```html
<input v-model="inputText">
```

```js
new Vue({
  data: {
    inputText: ''
  }
})
```

- `v-model`은 `v-bind`와 `v-on`의 기능의 조합으로 동작.

```html
<input v-bind:value="inputText" v-on:input="updateInput">
```

```js
new Vue({
  data: {
    inputText: ''
  },
  methods: {
    updateInput: function(event) {
      var updatedText = event.target.value;
      this.inputText = updatedText;
    }
  }
})
```

- v-bind 속성은 뷰 인스턴스의 데이터 속성을 해당 HTML 요소에 연결할 때 사용한다.
- v-on 속성을 해당 HTML 요소의 이벤트를 뷰 인스턴스의 로직과 연결할 때 사용
- 사용자 이벤트에 의해 실행된 뷰 메소드(methods) 함수의 첫 번째 인자에는 해당 이벤트(event)가 들어온다.

- 현재는 데이터 속성이 value가 아닌 modelValue, event는 input이 아닌 update:modelValue로 변경됨

> HTML 입력 요소의 종류에 따라 v-model의 속성이 달라짐
>> (1) input 태그에는 `value / input`  
>> (2) checkbox 태그에는 `checked / change`  
>> (3) select 태그에는 `value / change`  

### 한국어 관련

- 한글의 경우에는 IME입력(한국어, 일본어, 중국어)에 대해 한계가 있다.
- 한글 입력의 경우, 한 글자에 대한 입력이 끝나야지만 inputText 데이터가 인풋 박스의 텍스트 값과 동기화 된다.

- 이를 해결하기 위해서 v-bind:modelValue와, v-on:"update:modelValue"를 권장함

- 한국어 입력을 더 쉽게 하는 방법

```html
<!-- BaseInput.vue - 싱글 파일 컴포넌트 구조-->
<template>
  <input v-bind:value="value" v-on:input="updateInput">
</template>

<script>
export default {
  props: ['value'],
  methods: {
    updateInput: function(event) {
      this.$emit('input', event.target.value);
    }
  }
}
</script>
```

- BaseInput 컴포넌트의 상위 컴포넌트에서 props로 받은 value를 인풋 태그에 값으로 연결
- input 태그에서 값이 입력되면 input 이벤트가 발생하고 updateInput 메서드가 실행
- updateInput 메서드에서 인풋 태그에 입력된 값을 상위 컴포넌트에 input 이벤트로 올려보냄

```html
<!-- App.vue - 싱글 파일 컴포넌트 구조 -->
<template>
  <div>
    <base-input v-model="inputText"></base-input>
  </div>
</template>

<script>
import BaseInput from './BaseInput.vue';

export default {
  components: {
    'base-input': BaseInput
  },
  data: function() {
    return {
      inputText: ''
    }
  }
}
</script>
```

- 상위 컴포넌트에서 다음과 같이 사용하며, 정의한 데이터 값을 하위 컴포넌트로 내려보내는 부분을 주시
  - value가 프롭스 속성으로 내려간다는 사실을 알아야함
