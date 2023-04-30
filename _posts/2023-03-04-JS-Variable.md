---
layout: single
title: "[Javascript] 자바스크립트의 변수들"
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

### 변수

변수는 데이터에 대한 "**이름이 있는 저장소**"이다. 자바스크립트에서는 변수를 만들 때 `let` 키워드를 사용하고 할당 연산자 `=` 를 사용하여 데이터를 변수에 넣을 수 있다.

```javascript
let message;

message = "Hello";
```

위와 같이 **선언된 변수**에 **값**을 넣으면 변수와 연결되어 있는 **메모리 영역**에 값이 저장된다.

<br>

변수는 한 줄에 하나씩 선언하는 것이 더 가독성에 좋다.

```javascript
let user = "John",
  age = 25,
  message = "Hello";
// 한줄에 위와같이 변수를 여러개 선언할 수 있지만
// 더 나은 가독성을 위해 아래와 같이 변수 하나당 한줄이 더 적절한다
let user = "John";
let age = 25;
let message = "Hello";
```

<br>

변수에 저장된 값을 변경될 수도 있고 한 변수에서 다른 변수로 데이터를 복사도 할 수 있다.

```javascript
let message = "Hello";
message = "World";

let hello;
hello = message;

alert(hello); // 'World'
```

> :warning: **두 번 선언하면 오류가 발생한다**
>
> 동일한 이름의 변수를 두 번 선언하면 오류가 발생한다

<br>

### 변수 네이밍

자바스크립트의 변수 이름에는 몇 가지 제한 사항이 있다.

1. 이름에는 문자, 숫자 또는 `$`와`_` 만 들어갈 수 있다.

2. 첫 번째 문자는 숫자가 아니어야 한다

3. 첫 번째 문자로 `_` 을 사용하지 말자 (권장)

4. 예약어는 사용할 수 없다 ( ex, `let` `class` `return` 등)

5. **camel case** 를 사용하자

6. 변수의 이름은 대소문자를 구별한다 ( myage, myAge 서로 다른 변수이다 )

<br>

### 상수

상수는 한번 선언되고 나면 **값을 변경할 수 없는 변수**를 말한다. 자바스크립트에서는 `const`를 이용하여 선언할 수 있다.

```js
const myBirthday = "11.09.1999";
```

> :memo:**대문자 상수**
>
> 실행 전에 기억하기 어려운 값들을 미리 정의해놓는 것이다. 예를 들어 색상 코드 등이 있다.
>
> ```js
> const COLOR_RED = "#F00";
> const COLOR_GREEN = "#0F0";
> const COLOR_BLUE = "#00F";
> const COLOR_ORANGE = "#FF7F00";
> ```
>
> <br>
>
> 이렇게 사용하면 색상코드를 직접 쓰는 것보다 훨씬 기억하기도 쉽고 의미도 확실하다.
>
> 실행전에 이미 정해지게 되고 변하지 않는 값들은 이렇게 코드의 맨 앞에 정의해주면 좋다.

<br>

### 변수 네이밍 시 유의할 점

- 사람이 읽을 수 있는 이름을 사용하자

- 자신만아는 `a` `b` 와 같은 변수 이름을 사용하지 말자

- 이름을 최대한 간결하게 만들자
