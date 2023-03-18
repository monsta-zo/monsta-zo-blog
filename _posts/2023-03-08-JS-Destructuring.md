---
layout: single
title: "[Javascript] 구조 분해 할당"
categories: Front-end
tag: [Javascript, Front-end]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"

---

### 구조 분해 할당

**구조 분해 할당** ( destructuring assignment ) 는 **배열이나 객체의 속성을 해체**하여 그 값을 **개별 변수**에 담을 수 있게 하는 자바스크립트의 표현식이다. 

### 1. 설명

객체 및 배열 리터럴 표현식을 사용하면 쉽게 데이터 뭉치를 만들 수 있다.

```js
let x = [1,2,3,4,5];
```

구조 분해 할당의 구문은 위와 비슷하지만, 대신 할당문 좌변에서 원래 변수의 어떤 값을 분해해 할당할지 정의합니다.

```js
var x = [1,2,3,4,5];
var [y,z] = x;
console.log(y); // 1
console.log(z); // 2
```

### 2. 배열 구조 분해

**기본 변수 할당**

```js
let foo = ["one", "two", "three"];

let [red, yellow, green] = foo;
console.log(red); // "one"
console.log(yellow); // "two"
console.log(green); // "three"
```

**기본값 사용**

```js
let a, b;

[a=5, b=7] = [1];
console.log(a); // 1
console.log(b); // 7
```

**변수 값 교환하기**

```js
let a = 1;
let b = 3;
[a,b] = [b,a];  // 3 1
```

**함수가 반환한 배열 분석**

함수는 배열을 반환할 수 있다. 구조 분해를 사용하면 반환된 배열에 대한 작업을 더 간결하게 수행할 수 있다.

```js
function f() {
    return [1,2];
}

let a, b;
[a, b] = f(); // 1 2
```

**일부 반환 값 무시하기**

구조 분해 할당시 필요 없는 부분을 비워놓는다면 일부 반환 값을 무시할 수 있다.

```js
function f() {
    return [1,2,3];
}

var [a, ,b] = f(); // 1 3
```

**변수에 배열의 나머지를 할당하기**

나머지 구문을 이요해 분해하고 남은 부분을 하나의 변수에 담을 수 있다.

```js
let [a, ...b] = [1,2,3];
// a -> 1
// b -> [2,3]
```

### 3. 객체 구조 분해

**기본할당**

객체의 프로퍼티를 가져와서 구조 분해할 수 있다. 여기서 중요한 점은 <mark>구조 분해시 key 값을 변수명과 맞춰줘야지만 값을 제대로 할당한다. </mark>

```js
let o = {p: 42, q: true};
let {p, q} = o;

// p -> 42
// q -> true
```

**일부 프로퍼티만 사용하기**

일부 프로퍼티만 구조 분해 할당하여 사용할 수 있다.

```js
const person = {
    name: 'Lee Sun-Hyoup',
    familyName: 'Lee',
    givenName: 'Sun-Hyoup'
    company: 'Cobalt. Inc.',
    address: 'Seoul',
}

const { familyName, givenName } = person;
```

**새로운 변수 이름으로 할당하기**

위에서 key값을 맞춰줘야지 값을 제대로 할당 받을 수 있다고 했는데 다른 이름의 변수에 할당하고 싶다면 아래와 같이 한다.

```js
let o = {p: 42, q: true};

let {p: foo, q: bar} = o;

// foo -> 42
// bar -> true
```

**기본값 사용**

```js
let {a = 10, b = 5} = {a: 3};

// a -> 3
// b -> 5
```

**중첩된 객체 및 배열의 구조 분해**

```js
let metadata = {
    title: "Scratchpad",
    translations: [
       {
        locale: "de",
        localization_tags: [ ],
        last_edit: "2014-04-14T08:43:37",
        url: "/de/docs/Tools/Scratchpad",
        title: "JavaScript-Umgebung"
       }
    ],
    url: "/en-US/docs/Tools/Scratchpad"
};

let { title: englishTitle, translations: [{ title: localeTitle }] } = metadata;

console.log(englishTitle); // "Scratchpad"
console.log(localeTitle);  // "JavaScript-Umgebung"
```

**for of 반복문과 구조 분해**

객체의 배열이 있을 때 각 객체에 for문으로 접근해서 구조 분해 할당 받을 수 있다. 

```js
let people = [
  {
    name: "Mike Smith",
    family: {
      mother: "Jane Smith",
      father: "Harry Smith",
      sister: "Samantha Smith"
    },
    age: 35
  },
  {
    name: "Tom Jones",
    family: {
      mother: "Norah Jones",
      father: "Richard Jones",
      brother: "Howard Jones"
    },
    age: 25
  }
];

for (let {name: n, family: { father: f } } of people) {
  console.log("Name: " + n + ", Father: " + f);
}

// "Name: Mike Smith, Father: Harry Smith"
// "Name: Tom Jones, Father: Richard Jones"
```
