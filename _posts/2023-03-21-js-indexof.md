---
layout: single
title: "[JavaScript] indexOf(),lastIndexOf() 배열, 문자열에서 위치찾기"
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

# indexOf()

`indexOf()` 메서드는 배열 또는 문자열에서 원하는 요소나 문자의 첫번째 인덱스를 반환하고 존재하지 않으면 -1을 반환한다.

```js
arr.indexOf(searchElement, fromIndex);
string.indexOf(searchValue, fromIndex);
```

`searchElement (searchValue)`

배열에서 찾을 요소 또는 값이다.

`fromIndex` (optional)

검색을 시작할 인덱스이다.

`반환값`

첫번째로 발견된 위치, 발견되지 않으면 -1

# lastIndexOf()

`lastIndexOf()` 메서드는 배열 또는 문자열에서 주어진 값이 나타나는 마지막 인덱스를 반환하고, 존재하지 않으면 -1을 반환한다.

```js
arr.lastIndexOf(searchElement, fromIndex);
string.lastIndexOf(searchValue, fromIndex);
```

`searchElement (searchValue)`

배열에서 찾을 요소 또는 값이다.

`fromIndex` (optional)

검색을 시작할 인덱스이다.

`반환값`

마지막으로 발견된 위치, 발견되지 않으면 -1
