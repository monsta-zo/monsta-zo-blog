---
layout: single
title: "[JavaScript] 나누기 연산 (몫 구하기)"
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

### 나누기 연산

자바스크립트에서 기본 나누기 연산은 '/' 이다

```javascript
const num = 10 / 3; // 3.33333333....
```

### 몫 구하기

자바스크립트에서는 기본적으로 숫자형 연산은 부동소수점까지 연산하기 때문에

위와같이 나누기 연산을 하면 몫이 나오지 않는다. 몫은 나누어 떨어지는 만큼만 계산하기 때문에

소수점을 버려주어야 한다

> **Math.floor()**
>
> 주어진 숫자와 같거나 작은 정수중 가장 큰 수를 반환한다. ( 소수부분을 버려주는 효과 )

```javascript
const num = Math.floor(10 / 3); // 3
```
