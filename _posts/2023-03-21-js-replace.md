---
layout: single
title: "[JavaScript] replaceAll() 문자 치환하기"
categories: JavaScript
tag: [JavaScript]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"

---

# String.prototype.replaceAll()

`replaceAll()` 메서드는 `pattern` 의 모든 일치 항목이 `replacement` 로 대체된 새 문자열을 반환한다. 

```js
replaceAll(pattern, replacement);
```

`pattern` 

    문자열이거나, 정규식일 수 있다.

`replacement` 

    문자열이거나, 함수일 수 있다.

`반환값`   

    패턴의 모든 일치 항목이 `replacement` 로 대체된 새 문자열
