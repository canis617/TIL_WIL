# Vue 3

- ~~곧 출시예정이라고 하는듯?~~ 공식홈페이지에는 이미 업데이트 됬다. [링크](https://v3.vuejs.org/guide/introduction.html#declarative-rendering)

- 저희가 자주 사용하는 뷰 코어 라이브러리 뿐만 아니라 주변 생태계 라이브러리(뷰엑스, 뷰 라우터)도 베타 단계

- 새로 추가된 기능 : Composition API?

- 처음에는 타입스크립트를 좀 더 매끄럽게 포용하기 위해 클래스(ES6 Class) 기반 컴포넌트 코드 작성 방식을 고민하다가 최종적으로 현재의 객체 스타일 컴포넌트 옵션 속성 정의 방식을 채택

- 뷰 3이 추구하는 방향은 크게 2가지로 요약해 볼 수 있을 것 같습니다.

  - 컴포넌트 코드 재사용성 향상
  - 타입스크립트 문법 지원

## 기존 Vue.js의 한계점

- 컴포넌트 기반 UI 개발 방식을 추구.
- 코드를 재사용하기 편하게 만들며, 추후 디버깅도 수월하게 할 수 있기 때문

- Vue는 컴포넌트 코드를 재사용하기 위해 대표적으로 아래 3가지 방식 사용
  - 슬롯 (Slots & Scoped Slots)
  - 믹스인 (Mixins)
  - 하이 오더 컴포넌트 (High Order Components)

- 비즈니스 로직에 밀접하게 연관된 로직을 재사용하기 위해 보통 믹스인을 많이 사용(~~그렇다고한다~~)

- 타입스크립트 지원

## 달라진 점

- Teleport (Vue Portal과 유사)
- 템플릿 표현식 관련 추가 문법 제공
- Suspense
- Reactivity 주입 API
- Composition API

### composition API

- 보다 나은 조직화
- 코드의 재사용 / 공유

- 기존의 Option API
  - `counter`와 `msg`에 대한 로직이 분리되어 있지않아 보기 힘듬

```html
<template>
  <div class="counter">
    <p>count: {{ count }}</p>
    <p>NewVal (count + 2): {{ countDouble }}</p>
    <button @click="inc">Increment</button>
    <button @click="dec">Decrement</button>
    <p> Message: {{ msg }} </p>
    <button @click="changeMessage()">Change Message</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      count: 0,
      msg: 'some message'
    }
  },
  computed: {
    countDouble() {
      return this.count*2
    }
  },
  watch: {
    count(newVal) {
      console.log('count changed', newVal)
    },
    msg(newVal) {
      console.log('msg changed', newVal)
    }
  },
  methods: {
     inc() {
      this.count += 1
    },
    dec() {
      if (this.count !== 0) {
        this.count -= 1
      }
    },
    changeMessage() {
      msg = "new Message"
    }
  }

}
</script>
```

- Compostion API
  - `counter`와 `msg`에 대한 로직이 분리됨

```html
<template>
  <div class="counter">
    <p>count: {{ count }}</p>
    <p>NewVal (count + 2): {{ countDouble }}</p>
    <button @click="inc">Increment</button>
    <button @click="dec">Decrement</button>
    <p> Message: {{ msg }} </p>
    <button @click="changeMessage()">Change Message</button>
  </div>
</template>
<script>
import { ref, computed, watch } from 'vue'
export default {
  setup() {
/* ---------------------------------------------------- */
    let count = ref(0)
    const countDouble = computed(() => count.value * 2)
    watch(count, newVal => {
      console.log('count changed', newVal)
    })
    const inc = () => {
      count.value += 1
    }
    const dec = () => {
      if (count.value !== 0) {
        count.value -= 1
      }
    }
/* ---------------------------------------------------- */
    let msg= ref('some text')
    watch(msg, newVal => {
      console.log('msg changed', newVal)
    })
    const changeMessage = () => {
      msg.value = "new Message"
    }
/* ---------------------------------------------------- */
    return {
      count,
      inc,
      dec,
      countDouble,
      msg,
      changeMessage
    }
  }
}
</script>
```

- msg 로직을 모듈로 만들어 사용가능

```js
//message.js
import { ref, watch } from 'vue'
export function message() {
  let msg = ref(123)
  watch(msg, newVal => {
    console.log('msg changed', newVal)
  })
  const changeMessage = () => {
    msg.value = 'new Message'
  }
  return { msg, changeMessage }
}

//.vue
import { message } from './common/message'
```

### 템플릿 표현식 관련 추가 문법 제공

- vue 2에서는, template tag는 오직 하나의 root element만 가질 수 있다.
  - 만약 두개의 `<p>` 태그가 존재하면 하나의 `<div>` 태그로 묶어야한다.

- vue 3에서는 root 의 제한이 풀렸다.
  - 이제 template 아래 태그의 개수 제한이 없다!

```html
<template>
  <p> Count: {{ count }} </p>
  <button @click="increment"> Increment </button>
  <button @click="decrement"> Decrement</button>
</template>
```

### Suspense

- data를 fetch할 동안, v-if로 표현하던 상태를 `#default`와, `#fallback`으로 표현 가능

```html
<template>
  <Suspense>
    <template #default>
      <div v-for="item in articleList" :key="item.id">
        <article>
          <h2>{{ item.title }}</h2>
          <p>{{ item.body }}</p>
        </article>
      </div>
    </template>
    <template #fallback>
      Articles loading...
    </template>
  </Suspense>
</template>
<script>
import axios from 'axios'
export default {
  async setup() {
    let articleList = await axios
      .get('https://jsonplaceholder.typicode.com/posts')
      .then(response => {
        console.log(response)
        return response.data
      })
    return {
      articleList
    }
  }
}
</script>
```

### Better Reactivity

- vue 2도 충분한 reactivity(반응성)을 가지고 있지만 부족한 부분이 있다.

- 반응성을 보여주기 위해서 한 watchers를 사용한다. 이후 상태 변수를 확인하고 변경하여, watchers가 trigger되는지 확인한다.

- vue 2에서는 3개의 수정사항에 대해서 반응하지 않는다.
  - 인덱스를 기반으로 배열에 새 항목을 추가하거나
  - 객체에 새 항목을 추가하거나
  - 객체에서 항목을 삭제하는
- 따라서 watchers들이 trigger되지 않거나, DOM이 업데이트 되야한다.
- 이를 해결하기 위해 vue.set(), vue.delete() 메소드를 활용했다.

```html
<template>
  <div class="hello" @click="test">test {{list }} {{ myObj }}</div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      list: [1, 2],
      myObj: { name: "Preetish" }
    };
  },
  watch: {
    list: {
      handler: () => {
        console.log("watcher triggered");
      },
      deep: true
    }
  },
  methods: {
    test() {
      this.list[2] = 4;
      this.myObj.last = "HS";
      delete this.myObj.name;
    }
  }
};
</script>
```

- vue 3 에서는 다른 메소드 없이 잘 작동한다.
  - 아래에서 watchers는 setup()에서 4번 trigger된다.

```js
export default {
  setup() {
    let list = ref([1, 2])
    let a = ref(0)
    let myObj = ref({ name: 'Preetish' })
    function myFun() {
      list.value[3] = 3
      myObj.value.last = 'HS'
      delete myObj.value.name
    }
    return { myFun, list, myObj }
  }
}
```

### Global Mounting

- main.js에서 더이상 Vue instance를 직접 생성하지 않는다.

- createApp 메소드로 대체된다.

- Vue application을 third party library/plugin으로부터 보호한다.
  - 주로 믹스인으로 부터 global instance가 override되고, 변경되는것

```js
import { createApp } from 'vue'
import App from './App.vue'
const myApp = createApp(App)
myApp.use(/* plugin name */)
myApp.use(/* plugin name */)
myApp.use(/* plugin name */)
myApp.mount('#app')
```

### Portals

- portal 이란
  - 코드의 일부를 현재 위치에서 다른 위치로 옮기는 것(다른 DOM tree?)

- vue 2 에서는 portal-vue third-party plugin을 통해 사용

- vue 3 에서 `<Teleport>` 라는 태그로 사용가능
- Any code inside `<Portal></Portal>` will be displayed in the target location mentioned.

```html
<Teleport to="#modal-layer">
  <div class="modal">
      hello
  </div>
</Teleport>
```

```html
<div id="modal-target"></div>
```

### 디렉티브 문법의 인자(Arguments)

- 동적 인자(Dynamic Arguments)라는 개념이 추가
- 디렉티브의 대상이 뷰 인스턴스 데이터와 연결될 수 있게 문법이 지원된다.
- `v-on:` 디렉티브나 `v-bind:` 디렉티브의 대상을 아래와 같이 뷰 데이터 속성으로 연결하여 선언할 수 있다.

```html
<a v-bind:[linkAttribute]="url">...</a>
```

```javascript
data() {
  return {
    linkAttribute: 'href' // 위 `myAttribute`가 `href`로 선언됨
  }
}
```

### 축약 문법

- 디렉티브에 동적 인자를 사용할 수 있기 때문에 아래와 같은 축약 문법도 지원

```html
<!-- 기본 문법 -->
<a v-bind:href="imageUrl">
<a v-on:click="logText">
<a v-bind:[myAttribute]="imageUrl"> <!-- 추가된 문법 -->
<!-- 축약 문법 -->
<a :href="imageUrl">
<a @click="logText">
<a :[myAttribute]="imageUrl"> <!-- 추가된 문법(축약형) -->
```

### 멀티 이벤트 핸들러

- 일반적으로 기존 뷰 문법에서 DOM 이벤트 핸들러는 아래와 같이 하나만 부착하여 사용
- 템플릿 표현식에서 특정 DOM의 이벤트를 여러 메소드로 처리할 수 있는 문법이 지원

```html
<!-- 예시 -->
<button @click="logText">log</button>
```

```javascript
methods: {
  logText(event) {
    if (event) {
      event.preventDefault();
    }
  }
}
```

```html
<!-- 예시 -->
<button @click="logText($event), sayHi($event)">log</button>
```

```javascript
methods: {
  logText(event) {
    // 이벤트 처리 관련 첫 번째 로직
  },
  sayHi(event) {
    // 이벤트 처리 관련 두 번째 로직
  }
}
```

### 키보드 이벤트 제어 문법 추가

```html
<!-- 기존의 작동 방식 -->
<input @keyup.enter="addTodo">

<!-- "KeyboardEvent.key" 값이 PageDown인 경우 아래와 같이 케밥으로 변환하여 붙일 수 있음 -->
<input @keyup.page-down="onPageDown" />
```

### 컴포넌트 통신 방법 - 이벤트 에밋 인자 전달

```bash
App
└─TodoList
  └─TodoItem
```

```html
<!-- TodoItem.vue -->
<template>
  <li>
    <button @click="$emit('remove', 10)">remove</button>
  </li>
</template>

<!-- TodoList.vue -->
<template>
  <ul>
    <todo-item @remove="removeItem"></todo-item>
  </ul>
</template>

<script>
export default {
  methods: {
    removeItem(num) {
      this.$emit('remove', num);
    }
  }
}
</script>

<!-- App.vue -->
<template>
  <div>
    <todo-list @remove="removeTodo"></todo-list>
  </div>
</template>

<script>
export default {
  methods: {
    removeTodo(num) {
      axios.delete('/todo/' + num);
    }
  }
}
</script>
```

### 이제는 중간 메소드 없이 바로 인자 전달 가능

```html
<!-- TodoItem.vue -->
<template>
  <li>
    <button @click="$emit('remove', 10)">remove</button>
  </li>
</template>


<!-- TodoList.vue -->
<template>
  <ul>
    <todo-item @remove="$emit('remove', $event)"></todo-item>
  </ul>
</template>

<!-- App.vue -->
<template>
  <div>
    <todo-list @remove="removeTodo"></todo-list>
  </div>
</template>

<script>
export default {
  methods: {
    removeTodo(num) {
      axios.delete('/todo/' + num);
    }
  }
}
</script>
```

### 프롭스 속성

- 프롭스 속성에 문자열을 연결하는 것과 뷰 인스턴스의 data를 연결하는 것의 차이점

```html
<!-- 프롭스 속성에 일반 문자열을 연결하는 경우 -->
<template>
  <app-header title="News Tonight"></app-header>
</template>
```

```html
<!-- 프롭스 속성에 뷰 데이터를 연결하는 경우 -->
<template>
  <app-header :title="appTitle"></app-header>
</template>

<script>
export default {
  data() {
    return {
      appTitle: 'News Tonight'
    }
  }
}
</script>
```

### 값을 전달하지 않으면 `true`를 전달

```html
<template>
  <todo-item is-completed></todo-item>
</template>

<script>
export default {
  props: ['isCompleted'],
  created() {
    console.log(this.isCompleted); // true
  }
}
</script>
```

### 템플릿 표현식에서의 프롭스 속성 정의 축약 문법

- data 속성 객체의 프로퍼티를 모두 프롭스 속성으로 각각 연결해주는 축약 문법이 등장
- ~~와 사랑해요!!!~~

```html
<script>
export default {
  data() {
    return {
      appInfo: {
        title: '제목',
        version: '버전'
      }
    }
  }
}
</script>
```

```html
<!-- 축약 문법 -->
<app-title v-bind="appInfo"></app-title>
<!-- 실제로는 아래와 같이 동작 - 기존 문법 -->
<app-title v-bind:title="appInfo.title" v-bind:version="appInfo.version"></app-title>
```

### 이벤트 에밋 문법

- 인스턴스 옵션 속성에 emits 옵션 속성이 추가
- 또한 이벤트 에밋에 대한 유효성 검사(validation) 문법도 정의할 수 있는것 같다.

```js
export default {
  // 인스턴스 옵션 속성
  components: { TodoItem },
  emits: ['remove', 'add:todo']
}
```

```js
export default {
  emits: {
    remove: false,
    'add:todo': ({ item }) => {
      if (item) {
        return true;
      } else {
        console.log('invalid event payload');
        return false;
      }
    }
  }
}
```

### `inheritAttrs` 옵션 속성

- 하위 컴포넌트에 props 속성이 아닌 HTML 표준 속성을 정의하면 하위 컴포넌트의 Root Element에 HTML 표준 속성이 추가된다.
- 이와 같은 __속성 상속(attribute inheritance)__ 을 막는 문법이 추가됨

```html
<!-- App.vue - 상위 컴포넌트 -->
<template>
  <div>
    <todo-item class="warn"></todo-item>
  </div>
</template>

<!-- TodoItem.vue - 하위 컴포넌트 -->
<template>
  <li>
    <p>아이템 1</p>
  </li>
</template>
```

```html
<!-- TodoItem.vue -->
<template>
  <li class="warn">
    <p>아이템 1</p>
  </li>
</template>
```

```javascript
export default {
  inheritAttrs: false
}
```

### v-model 문법

- 사용자 입력을 받을 때 자주 사용되는 `v-model` 디렉티브는 컴포넌트 태그에 연결하였을 때 아래와 같이 정의하여 사용 가능
- props : value => modelValue로
- event : input => update:modelValue

```html
기존의 문법
<div>
  <my-input v-model="inputText"></my-input>
</div>

<template>
  <input :value="value" @input="onInput">
</template>

<script>
// MyInput.vue
export default {
  props: ['value'],
  methods: {
    onInput(event) {
      this.$emit('input', event.target.value);
    }
  }
}
</script>
```

```html
바뀐 문법
<!-- App.vue -->
<template>
  <div>
    <my-input v-model="inputText"></my-input>
  </div>
</template>

<script>
export default {
  data() {
    return { inputText: '' }
  }
}
</script>

<!-- MyInput.vue -->
<template>
  <input :value="modelValue" @input="onInput">
</template>

<script>
export default {
  props: ['modelValue'],
  methods: {
    onInput(event) {
      this.$emit('update:modelValue', event.target.value);
    }
  }
}
</script>
```

### 컴포넌트 태그에서 `v-model` 여러 개 사용

- 컴포넌트 태그에 아래와 같이 `v-model` 태그를 여러 개 연결 할 수 있게 됨

```html
<!-- App.vue -->
<template>
  <div>
    <user-profile 
      v-model:firstName="firstName"
      v-model:lastName="lastName"
    ></user-profile>
  </div>
</template>

<script>
export default {
  data() {
    return { 
      firstName: '',
      lastName: ''
    }
  }
}
</script>
```

```html
<!-- UserProfile.vue -->
<template>
  <input :value="firstName" @input="$emit('update:firstName', $event.target.value)">
  <input :value="lastName" @input="$emit('update:lastName', $event.target.value)">
</template>

<script>
export default {
  props: ['firstName', 'lastName'],
}
</script>
```
