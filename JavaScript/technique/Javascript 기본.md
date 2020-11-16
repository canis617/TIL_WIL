# __JavaScript 개요__

- 가벼운 인터프리터 또는 JIT 컴파일 프로그래밍 언어
- __일급 함수__ 를 지원, [예제](JavaScript\technique\일급함수.md)
  - 함수를 다른 변수와 동일하게 다루는 언어는 일급 함수를 가졌다고 표현
- 웹 페이지의 스크립트 언어

- 프로토타입 기반의 동적 다중 패러다임 스크립트 언어
  - 인터프리터 형태의 자바가 아니다!

- 다양한 프로그래밍 스타일 지원

  - 객체지향형
  - 명령형
  - 선언형(함수형 프로그래밍)

* Javascript가 DOM 모양을 바꿀 수 있다

<br><br>

# Java와 Javascript의 차이점

|Javascript|Java|
|---|---|
|객체 지향. 객체의 형 간에 차이 없음. 프로토타입 메커니즘을 통한 상속, 그리고 속성과 메서드는 어떤 객체든 동적으로 추가될 수 있음.|클래스 기반. 객체는 클래스 계층구조를 통한 모든 상속과 함께 클래스와 인스턴스로 나뉨. 클래스와 인스턴스는 동적으로 추가된 속성이나 메소드를 가질 수 없음.|
|변수 자료형이 선언되지 않음(dynamic typing, loosely typed).|변수 자료형은 반드시 선언되어야 함(정적 형지정, static typing).|
|하드 디스크에 자동으로 작성 불가.|하드 디스크에 자동으로 작성 가능.|

<br><br>

# 문법과 자료형

- 대소문자 구별, UniCode 문자셋 이용

- `;` (세미콜론)으로 명령문 구분
  - 한줄에 하나의 명령문은 생략가능 (별로 좋은 습관은 x)

## __주석__
```javascript
// 한 줄 주석

/*
 *  여러 줄 주석
 * / 

중첩된 주석은 사용 x
```

## __변수__

- 선언에는 3가지 종류.
  - var
    - 변수를 선언, 추가로 동시에 값을 초기화

  - let
    - 블록 범위(scope) 지역 변수를 선언, 동시에 값을 초기화

  - const
    - 블록 범위 읽기 전용 상수를 선언

- 선언되지 않은 변수는 `undefined` 값을 가진다.

- `undefined`값은 Boolean 변수형에서 false이다.

- string 에서는 `NaN`으로 변환

- null 값은 수치에서는 0으로, Boolean에서는 false로 동작

## __변수 범위__
  - var은 기본적으로 범위는 블록이 아닌 함수 단위
  - let 선언은 다른 언어의 지역번수와 동일
```javascript
if (true) {
    var x = 5;
    let y = 5;
}
console.log(x); // 5
console.log(y); // ReferenceError: y is not defined
```

## __변수 호이스팅__

- javascript의 좋은점(?)
- 나중에 선언된 변수를 참조할 수 있다는 개념
- 이를 위해서 var변수는 블록의 최상단으로 올리고, let은 위치가 상관없다.
- 함수의 경우에는 선언이 호출보다 위에 있어야 한다.

- 변수의 선언 > 함수의 선언 > 변수의 할당 순으로 이루어진다.

- __사실 호이스팅이 일어나지 않도록 코드를 작성하는게 낫다__
```javascript
/**
 * Example 1
 */
console.log(x === undefined); // logs "true"
var x = 3;


/**
 * Example 2
 */
// undefined 값을 반환함.
var myvar = "my value";

(function() {
  console.log(myvar); // undefined
  var myvar = "local value";
})();
```

## 데이터 형

- 원시 데이터 형
  - Boolean
  - null
  - undefined
  - Number
  - String
  - Symbol
  - Object

- 숫자와 문자의 자동변환에 대해서
```javascript
"37" - 7 // 30
"37" + 7 // 377

문자열 > 숫자의 변환은 다음을 사용을 권장
(+"1.1") + (+"1.1") //2.2
```

## 리터럴

- 배열

```javascript
var coffees = ['French", ...]
```

- Boolean
```javascript
true or false
```

- 부동 소수점
```javascript
3.1415926
-.123456789
-3.1E+12
.1e-23
```
- 정수
```javascript
0, 117 및 -345 (10진수)
015, 0001 및 -0o77 (8진수)
0x1123, 0x00111 및 -0xF1A7 (16진수)
0b11, 0b0011 및 -0b11 (2진수)
```
- 객체
```javascript

var car = { myCar: "Saturn", getCar: carTypes("Honda"), special: sales };

console.log(car.myCar);   // Saturn
console.log(car.getCar);  // Honda
console.log(car.special); // Toyota
```
- 정규식
```javascript
var re = /ab+c/;
```
정규식 같은 경우 다음의 [링크](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)에 자세히


- 문자열

```javascript
"foo"
'bar'

ES2015
var name = "foo", time = "today";
`Hello ${name}, how ar you ${time}`

```
문쟈열 리터럴을 자동으로 임시 문자열 객체로 변환, 메소드 호출 뒤 폐기  

<br><br>
# 흐름 제어와 오류 처리

- 블록 범위 : `{ }`
- 조건문 

  `if ... else ...`
- 거짓으로 취급하는 값
  - false
  - undefined
  - null
  - 0
  - NaN
  - the empty string ("")

  `switch ... case 1: ... case 2: ...`

- 예외처리문

  `throw expression;`  
  `try ... catch ... finally ...`  

- `finally`구문의 내용이 `try`, `catch`구문과 상관없이 호출되기 때문에, try catch 구문에 return 구문이 있어도, finally 구문에 return문이 있다면, finally의 return이 반환값이 됩니다.

## Promises

- ECMAScript2015에서의 변화점, 지연된 흐름과 비동기식의 연산은 제어할 수 있는 [`Promise`](JavaScript\technique\Promise.md) 객체에 대한 지원을 함

- 자세한건 [Promise](JavaScript\technique\Promise.md) 항목 참조

# 루프와 반복

- for
- do ... while
- while
- 레이블 문  
  - label :
  - break 등등 에서 활용할 수는 있으나 글쎄.. C언어의 goto문 같아서 안쓰는게 좋아보인다.
- break
- continue
- for ... in 
  - 객체의 열거 속성을 통해 지정된 변수를 반복한다.
  - javascript의 Object?
```javascript
let obj = {
  'key1': 'value1',
  'key2': 'value2'
}
for (var i in obj) {
  console.log(i, obj[i]) //key, value
}
```
- for ... of
  - 반복 가능한 객체를 통해 반복하는 루프 (배열, Map, Set, 인수 객체 등등)
```javascript
let arr = [3, 5, 7];
arr.foo = "hello";

for (let i in arr) {
   console.log(i); // logs "0", "1", "2", "foo"
}

for (let i of arr) {
   console.log(i); // logs "3", "5", "7"
}
```

# 함수
- 함수 선언
```javascript
function square(number) {
  return number * number;
}
```
- 기본 자료형인 매개변수(parameter)는 값으로 함수에 전달(call by value)
- 매개변수로 배열이나 사용자 정의 객체 등 기본 자료형이 아닌 경우 함수 밖에서도 값이 변경됩니다. (call by reference)

- 함수를 함수표현식을 통해 변수로 표현가능
```javascript
var square = funtion(number) {
  return number * number;
}
```
- 함수는 범위 내에 (함수 영역 등) 있어야 하며, 호이스팅이 일어난다.
  - 함수 호이스팅은 함수표현식에서는 작동하지 않는다!!
```javascript
console.log(square);   // square는 초기값으로 undefined를 가지고 호이스트된다.
console.log(square(5));  // TypeError: square는 함수가 아니다.
square = function (n) {
  return n * n;
}
```

## 함수의 범위
- 함수 내에서 정의된 변수는 함수의 범위에서만 정의 된다.
- 전역함수는 모든 전역 변수를 액세스 할 수 있다.

- 자기 자신을 참조하기 위한 방법
```javascript
var foo = function bar() {
  //statement
}
foo(), bar(), arguments.callee();
```

## 중첩된 함수와 클로저
- 함수 내에 함수를 끼워 넣을 수 있다.
- 중첩 된 함수는 외부 함수와 별개이며 클로저를 형성한다.
- 클로저 : 그 변수를 결합하는 환경을 자유롭게 변수와 함께 가질 수 있는 표현
  - 내부 함수는 외부 함수의 명령문에서만 액세스 가능
  - 내부 함수는 클로저를 형성 
    - 외부 함수는 내부 함수의 인수와 변수를 사용 할 수 없는 반면
    - 내부 함수는 외부 함수의 인수와 변수를 사용 가능

```javascript
function outside(x) {
  function inside(y) {
    return x + y;
  }
  return inside;
}
fn_inside = outside(3); // Think of it like: give me a function that adds 3 to whatever you give it
result = fn_inside(5); // returns 8

result1 = outside(3)(5); // returns 8
```
- 여기서 외부 함수의 인수 x가 보존되는 것을 볼 수 있다.
- 메모리는 그 무엇도 내부 함수에 접근하지 않을 때 (unreachable) 해제된다.

- 클로저에 더 자세히는 [여기](JavaScript\technique\클로저.md)서

## argument 객체
- `argument[i]`
- 보통 함수에 정의된 개수보다 많은 인수를 넘겨주며 함수를 호출 가능
- 배열과 비슷한 형태
- ~~이런식으로 함수 오버로딩이 가능하다니..~~

```javascript
function myConcat(separator) {
   var result = ""; // 리스트를 초기화한다
   var i;
   // arguments를 이용하여 반복한다
   for (i = 1; i < arguments.length; i++) {
      result += arguments[i] + separator;
   }
   return result;
}
// returns "red, orange, blue, "
myConcat(", ", "red", "orange", "blue");
```
- argument는 실제 배열은 아니라고 한다.
  - index로 색인이 가능해 비슷 할 뿐 배열의 모든 메소드를 가지진 않는다.

## 디폴트 매개변수
- 파라미터에 값은 기본적으로 `undefined`가 전달된다.
- 파라미터 기본값을 넣어주면, 디폴트 파라미터가 바뀌게 된다.

## 나머지 매개변수
- 불확실한 개수의 인수를 나타낼 수 있다.
```javascript
function multiply(multiplier, ...theArgs) {
  return theArgs.map(x => multiplier * x);
}

var arr = multiply(2, 1, 2, 3);
console.log(arr); // [2, 4, 6]
```

## 화살표 함수
- 함수 표현식과 비교적 짧은 문법을 가짐.
- 사전적으로 this 값을 묶음
- 언제나 익명 함수!!  
<br>
- 더 짧은 함수가 환영받음
```javascript
var a = [
  "Hydrogen",
  "Helium",
  "Lithium",
  "Beryl­lium"
];

var a2 = a.map(function(s){ return s.length });
console.log(a2); // logs [8, 6, 7, 9]
var a3 = a.map( s => s.length );
console.log(a3); // logs [8, 6, 7, 9]
```

- 사전적 __this__
- 다른 언어에서의 this처럼 사용할 수 있게 됨
- javascript에서의 this의 동작은 [여기](JavaScript\technique\this.md)서 공부