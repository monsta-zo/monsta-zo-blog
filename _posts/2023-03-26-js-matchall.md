---
layout: single
title: "[JavaScript] matchAll() : 일치하는 모든 문자열 찾기"
categories: JavaScript
tag: [JavaScript]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"

---

# String.prototype.matchAll()

`matchAll()` 은 일치하는 모든 결과를 캡처 그룹을 포함해서 배열로 반환한다.

```js
"12345".matchAll(regexp)
```

`regexp`

    정규 표현식이다. 반드시 `g` 플래그를 포함해야 한다.

**`반환값`**

    일치하는 모든 결과의 `iterator`를 캡처 그룹을 포함해서 반환한다.
