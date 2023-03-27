---
layout: single
title: "[JavaScript] replace(), 문자 치환하기"
categories: JavaScript
tag: [JavaScript]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"

---

# String.prototype.replace()

`replace()` 메서드는 `pattern` 의 모든 일치 항목이 `replacement` 로 대체된 새 문자열을 반환한다. 

```js
replace(pattern, replacement);
```

`pattern` 

    문자열이거나, 정규식일 수 있다.

    문자열일경우, 첫번째 일치만이 대체된다.

`replacement` 

    문자열이거나, 함수일 수 있다.

- 문자열인경우, `pattern`을 문자열로 대체한다.

- 함수인경우, `pattern`과 일치할때 마다 함수가 실행되고, 함수의 `return값`이 대체문자열이 된다.

`반환값`   

    패턴의 모든 일치 항목이 `replacement` 로 대체된 **새 문자열**

## 1. 예시

**함수를 이용해서 대체하는 경우**

```js
numbers.replace(/zero|one|two|three|four|five|six|seven|eight|nine/g, (v) => {
        return obj[v];
    });
```
