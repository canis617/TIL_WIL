# __THIS__

- javascript 에서 this는 다른 언어에서의 this와 약간 다름.

- 대부분 this 값은 함수를 호출한 방법에 의해 결정된다.

- 실행중에는 할당으로 설정 할 수 없고, 함수를 호출 할 때 마다 다를 수 있다.

- this 값을 설정 할 수 있는 bind 메서드가 존재

- 화살표 함수를 통해 this의 바인딩을 제공하지 않을 수 있다.

- 전역 실핼 문맥(global execution context)에서 `this`는 strict mode 여부와 관계없이 전역 객체를 참조한다.

```javascript
// 웹 브라우저에서는 window 객체가 전역 객체
console.log(this === window); // true

a = 37;
console.log(window.a); // 37

this.b = "MDN";
console.log(window.b)  // "MDN"
console.log(b)         // "MDN"
```

- 함수 문맥(functional context)에서는 함수를 호출한 방법에 의해 좌우된다.

- 단순 호출

```javascript
function f1() {
  return this;
}

// 브라우저
f1() === window; // true 

// Node.js
f1() === global; // true

function f2(){
  "use strict"; // 엄격 모드 참고
  return this;
}

f2() === undefined; // true
```

- `call()`과 `apply()`를 통해 this를 임의적으로 정해줄 수 있다.
```javascript
function bar() {
  console.log(Object.prototype.toString.call(this));
}

bar.call(7);     // [object Number]
bar.call('foo'); // [object String]
bar.call(undefined); // [object global]
```

- `bind()`를 통해서 this의 값을 영구적으로 고정시킬 수 있다. 이는 한번만 동작한다.

```javascript
function f() {
  return this.a;
}

var g = f.bind({a: 'azerty'});
console.log(g()); // azerty

var h = g.bind({a: 'yoo'}); // bind는 한 번만 동작함!
console.log(h()); // azerty

var o = {a: 37, f: f, g: g, h: h};
console.log(o.a, o.f(), o.g(), o.h()); // 37, 37, azerty, azerty
```

- 화살표 함수는 자신을 감싼 정적 범위(lexical context)를 뜻하며, 새로운 this를 할당하지 않는다.
  - call, bind, apply등의 호출을 무시한다.
