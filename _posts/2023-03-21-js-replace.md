---
layout: single
title: "[JavaScript] replace(),replaceAll() : 문자열 치환하기"
categories: JavaScript
tag: [JavaScript]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"

---

우리는 가끔 특정 패턴의 문자열만 찾아서 다른 문자열로 바꿔줘야하는 상황이 생긴다. 그럴 땐 String 메소드인 `replace()` 와 `replaceAll()` 을 사용할 수 있다. 정규 표현식을 사용하면 더 다양하고 더 많은 패턴을 치환해줄 수 있다. 

패턴의 문자열을 하나만 치환하냐, 모두 치환하냐에 따라서 두 가지 다른 메소드를 사용할 수 있다.

# String.prototype.replace()

- `replace()` 메서드는 `pattern` 과 일치하는 첫번째 항목이 `replacement` 로 대체된 새 문자열을 반환한다. 

```js
replace(pattern, replacement);
```

- `pattern` 
  - 문자열이거나, 정규식일 수 있다.
  - 문자열일경우, **첫번째 일치만이 대체된다**.

- `replacement` 

  - 문자열이거나, 함수일 수 있다.

  - 문자열인경우, `pattern`을 문자열로 대체한다.


  - 함수인경우, `pattern`과 일치할때 마다 함수가 실행되고, 함수의 `return값`이 대체문자열이 된다.


- `반환값`   
  - 패턴과 일치하는 문자열이 `replacement` 로 대체된 **새 문자열**
  - 기존의 문자열을 바꾸는 것이 아닌 새 문자열을 반환한다. 
  - 기존의 배열을 바꾸고 싶다면 `map()` 과 함께 사용하자.

## 1. 예시

***함수를 이용해서 대체하는 경우***

```js
numbers.replace(/zero|one|two|three|four|five|six|seven|eight|nine/g, (v) => {
        return obj[v];
    });
```

# String.prototype.replaceAll()

- `replace()` 메서드는 `pattern` 과 일치하는 모든 항목이 `replacement` 로 대체된 새 문자열을 반환한다. 

```js
replace(pattern, replacement);
```

- `pattern` 
  - 문자열이거나, 정규식일 수 있다.
  - 정규식일 경우 반드시 **전역(g) 플래그가 포함되어야 한다.**

- `replacement` 

  - 문자열이거나, 함수일 수 있다.

  - 문자열인경우, `pattern`을 문자열로 대체한다.

  - 함수인경우, `pattern`과 일치할때 마다 함수가 실행되고, 함수의 `return값`이 대체문자열이 된다.

- `반환값`   
  - 패턴과 일치하는 모든 문자열이 `replacement` 로 대체된 **새 문자열**

## 1. 예시

```js
"ayaye".replaceAll(/aya|ye|woo|ma/g,"") // ""
```

- "ayaye" 라는 문자열에서 "aya" 또는 "ye" 또는 "woo" 또는 "ma"에 해당하는 문자열을 **모두** ""(빈 문자열)로 대체한다.
