---

layout: single
title: "[JavaScript] Iterable객체와 Array-like 객체"
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

[Airbnb JavaScript 코드 컨벤션](https://github.com/airbnb/javascript#arrays--from-iterable)을 공부하다 Iterable객체와 Array-like 객체를 마주쳤다. JS를 공부하다 종종 마주쳤었는데 언젠가 정리를 해야할 날이 올 줄 알았다.



# Iterable 객체

iterable 객체는 **배열을 일반화한 객체**이다. `for..of` 반복문을 사용할 수 있는 객체는 iterable 객체라고 볼 수 있는데, 예를 들면 문자열 또한 iterable 객체의 한 예이다.



iterable 객체의 핵심은 `Symbol.iterator` 라는 메서드를 가진다는 것이다.



## Symbol.iterator

`for..of` 를 호출하면 `Symbol.iterator` 메서드가 호출된다. 

또, `Symbol.iterator` 는 `next()` 라는 메서드를 가지는 객체를 반환해주는데 이때 `next()` 는 순회할 때 다음 값을 불러오는 역할을 한다.



간단히 정리하자면, Iterable 객체란 `for..of`를 통해 **순회가 가능한 객체** 이다.



# Array-like(유사 배열) 객체

iterable 객체와 비슷해 보이지만 다르다. 유사 배열은 **인덱스**와 `length` 속성이 있어 배열처럼 보이는 객체이다.



- iterable 객체 : `Symbol.iterator` 메서드를 가져서 순회가 가능한 객체

- array-like 객체 : 인덱스와 `length` 속성이 있어 배열처럼 보이는 객체



즉, 겉으로 봐서는 해당 객체가 iterable인지..? array-like인지..? 구별하기 힘들다. 객체가 어떤 속성을 가지느냐에 따라서 종류가 달라진다.



# Array.from()

배열의 메서드 `from()`은 iterable 객체나 array-like 객체를 **진짜 배열**로 만들어준다. JS에서는 배열 객체에 기본적으로 제공해주는 유용한 메서드가 매우매우 많기 때문에 이를 사용하기 위해서 `from()` 을 통해 배열로 바꿔주곤한다.



# Airbnb의 배열 변환 컨벤션



## 1. iterable 객체는 전개 구문 (`...`) 을 통해 배열로 바꾸자

```js
const foo = document.querySelectorAll('.foo');

// good
const nodes = Array.from(foo);

// best
const nodes = [...foo];
```



## 2. array-like 객체는 `Array.from` 을 통해 배열로 바꾸자

```js
const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

// bad
const arr = Array.prototype.slice.call(arrLike);

// good
const arr = Array.from(arrLike);
```


