---
layout: single
title: "[HTML] HTML 기초"
categories: HTML
tag: [HTML]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"

---

### :bone: HTML 이란?

- HTML은 Hyper Text Markup Language 의 약어이다.
  
  - Hyper Text : 하이퍼링크(문서의 요소들을 다른 문서와 연결해놓은 것)를 나타낼 수 있는 텍스트
  
  - Markup : 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어

- 웹 페이지를 구성하기 위한 표준 마크업 언어이다.

- 웹 페이지의 구조를 나타낸다.

- 요소들로 구성되고, 요소들은 내용들을 어떻게 표시할지 알려준다.

```html
<!DOCTYPE html>  -> 이 문서가 HTML5 임을 알려주고 브라우저가 페이지를 올바르게 표시하도록 도와준다
<html>  -> HTML 페이지의 root
<head>  -> 메타정보들을 포함
<title>Page Title</title>  -> 페이지의 제목 (상단 상태바 등에  표시)
</head>  
<body>  -> 우리가 눈으로 보는 모든 내용들의 컨테이

<h1>My First Heading</h1>  -> large heading
<p>My first paragraph.</p>  -> paragraph

</body>
</html>
```

### :bone: HTML 요소란?

> HTML 요소는 아래와 같이 시작태그 + 내용 + 종료태그로 구성된다

```html
<tagname> Content goes here... </tagname>
```

- HTML 요소들은 중첩될 수 있다.

- 종료태그를 절대 잊지말자. 종료 태그 없이도 브라우저에 올바르게 표시되는 경우가 있지만 이에 의존하면 안된다.

- ```html
  <p>This is a <br> paragraph with a line break.</p>
  ```
  
  위의 br태그와 같이 종료태그가 없는 요소들도 있다. 이를 empty tag라고 한다.

### :bone: HTML 속성이란?

> 모든 HTML 요소들은 속성(<u>attribute</u>)를 가질 수 있다. 이런 속성들은 **요소들에 대한 추가적인 정보**들을 제공한다. 반드시 시작태그안에 명시되어야 하며 ( name = "value" ) 와 같이 표기한다.

### :bone: HTML Headings

> HTML Heading 태그는 웹 페이지에 표시하려는 제목 또는 부제목을 나타낸다. <h1> 에서부터 <h6> 까지 나타낼 수 있으며, 숫자가 낮을수록 더 중요한 내용들을 담고 있다. 
