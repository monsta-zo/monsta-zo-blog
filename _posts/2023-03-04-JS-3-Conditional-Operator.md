---
layout: single
title: "[JavaScript] 삼항연산자"
categories: JavaScript
tag: [JavaScript]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"

---

### 삼항연산자

자바스크립트에서 유일하게 세 개의 피연산자를 받는 연산자이다. 아래와 같은 형식으로 사용한다.

> **삼항연산자**
> 
> 조건문 ? 참일때 실행될 문장 : 거짓일때 실행될 문장 ; 

```javascript
const solution = (num1,num2) => num1===num2 ? 1 : -1;
// num1 과 num2 가 같으면 return 1
// 다르면 return -1
```
