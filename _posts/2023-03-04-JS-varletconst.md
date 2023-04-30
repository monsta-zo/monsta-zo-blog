---
layout: single
title: "[JavaScript] var,let,const의 차이"
categories: JavaScript
tag: [JavaScript]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  nav: "docs"
header:
  teaser: /assets/images/javascript.png
---

### 1. var

`var` 문은 변수를 함수범위 또는 전역범위로 선언할 수 있으며, 초기화할 수 있다.

```js
var varname = value;
```

<br>

#### var 호이스팅 (hoisting)

코드 안 어디선가 변수가 선언되었다면 해당 변수를 먼저 사용할 수 있다. 즉, 변수가 어디서 선언이 되었든 가장 최상위에서 선언된 것처럼 여기는 것이다. 이처럼 변수 선언이 함수 또는 전역 코드의 상단에 이동하는 것과 같은 행동을 "**호이스팅**" 이라고 한다.

<br>

#### var 선언의 특징

선언된 변수들은 선언된 범위안에서 만들어진다. 선언되지 않은 변수들은 (var 없이 선언) 전역변수로 여겨진다

```js
function x() {
  y = 1; // strict 모드에서는 ReferenceError
  var z = 2;
}

x();

console.log(y); // var없이 선언되었기 때문에 전역변수로 여겨지고 "1"을 출력
console.log(z); // x함수 안에서 선언되었기 때문에 ReferenceError
```

<br>

### 2. let

`let` 선언은 블록 스코프의 범위를 가지는 지역 변수를 선언하며, 선언과 동시에 임의의 값으로 초기화할 수 있다.

```js
let name = value;
```

<br>

#### let 선언의 특징

블록의 범위에 상관없이 함수 또는 전역의 범위를 가지는 `var`선언과 달리 `let` 선언은 블록의 범위를 가진다.

```js
function varTest() {
  var x = 1;
  {
    var x = 2; // 함수의 범위를 가지기 때문에 block 밖의 x와 같은 변수이다.
    console.log(x); // 2
  }
  console.log(x); // 2
}

function letTest() {
  let x = 1;
  {
    let x = 2; // block의 범위를 가지기 때문에 block 밖의 x와 다른 변수이다.
    console.log(x); // 2
  }
  console.log(x); // 1
}
```

<br>

또한 `var` 과 달리 `let` 은 선언되기 이전에 사용될 수 없다 ( **hoisting이 허용되지 않는다** )

```js
{
  // TDZ starts at beginning of scope
  console.log(bar); // undefined, hoisting이 허용되므로 접근가능
  console.log(foo); // ReferenceError, hoisting이 허용되지 않
  var bar = 1;
  let foo = 2; // End of TDZ (for foo) , TDZ가 끝난후 접근 가
}
```

<br>

또한 `let` 으로 전역에서 선언될 경우 `var` 와 다르게 `window` 객체에 새로운 속성을 추가하지 않는다.

```js
var x = "global";
let y = "global";
console.log(this.x); // "global"
console.log(this.y); // 전역에서 선언될 경우에 window 객체에 속성을 추가하지 않는
```

<br>

`var`과 달리 `let`은 재선언이 허용되지 않는다

```js
if (x) {
  let foo;
  let foo; // SyntaxError 발생
}
```

같은 이유로 switch문에서 block을 나누어 주지 않는다면 에러가 발생할 수 있다.

```js
let x = 1;
switch (x) {
  case 0:
    let foo;
    break;

  case 1:
    let foo; // 재선언으로 인한 SyntaxError
    break;
}
```

<br>

#### let의 Temporal Dead Zone ( TDZ )

위에서 `let`은 hoisting이 허용되지 않는다고 했는데 그 이유는 `let`과 `const`는 `TDZ`를 가지기 때문이다. 여기서 `TDZ` 란 블록이 시작되었을 때 부터 `let`변수가 선언되기 이전까지는 해당 변수에 접근을 해도 `ReferenceError` 가 발생한다. 이와 다르게 `var`는 뒤에서 선언되기만 한다면 그 전에 변수에 접근이 가능한데 이는 `let`과 달리 `TDZ` 를 가지지 않기 때문이다.

```js
{
  // TDZ starts at beginning of scope
  console.log(bar); // undefined, hoisting이 허용되므로 접근가능
  console.log(foo); // ReferenceError, hoisting이 허용되지 않
  var bar = 1;
  let foo = 2; // End of TDZ (for foo) , TDZ가 끝난후 접근 가
}
```

### 3. const

`const` 는 `let` 과 같이 블록 범위이지만 상수 선언이다. 상수의 값을 재할당할 수 없으며 재선언도 불가능하다. 하지만 만약 상수가 **객체**이거나 **배열**이라면 객체의 프로퍼티나 배열의 요소들은 업데이트하거나 제거할 수 있다.

```js
const name1 = value1;
```

<br>

#### const선언의 특징

`const`선언은 선언된 블록의 전역 또는 지역 상수를 만든다. `let`과 같고 `var`와 다르게 전역으로 선언할 경우 `window`객체의 프로퍼티를 생성하지 않는다.

<br>

선언과 동시에 반드시 초기화되어야 하며 선언만 할 수 없다. ( 즉, 나중에 변경될 수 없다 )

<br>

`const` 선언은 값을 읽을 수만 있는 참조를 만든다. 이것은 그 값이 절대 변하지 않는다는 것을 의미하지는 않고 재할당을 할 수 없다는 것을 의미하는데 예를 들어, 객체를 `const`로 선언한다면 객체의 속성은 변경될 수 있음을 의미한다.

<br>

TDZ(Temporal Dead Zone)가 `let`과 같이 `const`에도 적용되며 이것은 `hoisting`될 수 없다는 것을 의미하기도 한다.

<br>

#### const 예제

상수는 대문자 소문자 상관없이 선언될 수 있지만, 주로 대문자 이름으로 선언한다.

```js
const MY_FAV = 7;

MY_FAV = 20; // 상수로의 재할당이기 때문에 TypeError 발생

const MY_FAV = 20; // 재선언 불가하기 때문에 Syntax Error 발생

var MY_FAV = 20;
let MY_FAV = 20;
// 다른 변수들과 이름을 공유할 수 없다.
```

<br>

상수는 반드시 선언과 동시에 초기화 되어야 한다

```js
const FOO; // Syntax Error
```

<br>

상수로 객체나 배열을 선언할 경우 그 자체를 재할당 할 순 없지만, 프로퍼티나 요소들은 변경할 수 있다.

```js
const MY_OBJECT = { key: "value" };
MY_OBJECT = { OTHER_KEY: "value" };
// "Assignment to constant variable" 에러 발생

MY_OBJECT.key = "otherValue";
// 객체의 프로퍼티는 수정가능하다.
```

객체의 프로퍼티도 수정 불가능 하게 하려면 `Object.freeze()` 를 사용하면 된다.

```js
const MY_ARRAY = [];
MY_ARRAY = ["B"]; // 배열 상수로의 재할당은 불가능 하다

MY_ARRAY.push("A"); // 배열 상수의 요소는 수정 가능하
```

<br>

### 4. var, let 과 const의 차이점

자바스크립트에서 변수를 선언할 수 있는 방법은 `var`, `let`, `const` 3가지이다. 이 세가지 선언에는 어떤 차이점이 있고 언제 사용하는지 알아보자.

| var                                                  | let                                   | const                                  |
| ---------------------------------------------------- | ------------------------------------- | -------------------------------------- |
| 재선언 O                                             | 재선언 X                              | 재선언 X                               |
| 재할당 O                                             | 재할당 O                              | 재할당 X                               |
| 전역 또는 함수 범위                                  | 블록 범위                             | 블록 범위                              |
| 범위의 맨위에 호이스팅 가능하므로 어디서든 사용 가능 | 사용전에 초기화 되어야함 (호이스팅 X) | 사용전에 초기화 되어야 함 (호이스팅 X) |
| 어디서든 재선언 O                                    | 블록안에서만 재선언 O                 | 재선언 X                               |

#### var 선언

변수의 재선언이 가능하다. 이는 유연한 변수 선언으로 간단한 테스트에는 편리할 수 있겠지만, 코드량이 많아진다면 어디에서 어떻게 사용될지 파악하기 힘들뿐더러 값이 바뀔 우려도 있다.

<br>

그래서 ES6 이후 , 이를 보완하기 위해 추가 된 변수 선언 방식이 `let`과 `const`이다.

<br>

#### let 선언

재선언이 불가능하기 때문에 혼란을 줄일 수 있고 그래서 `var`를 사용하는 대신 주로 `let`을 사용한다.

<br>

#### const 선언

재할당이 불가능하기 때문에 `let`과 달리 상수로 선언하고 싶을 때 `const`를 사용한다.

<br>

### 5. 정리

변수 선언에는 기본적으로 `const`를 사용하고, 재할당이 필요한 경우에 한정에 `let`을 사용하는 것이 좋다. ( `var` 사용은 나와 모두에게 혼란을 야기할 수 있으니 피하자 )
