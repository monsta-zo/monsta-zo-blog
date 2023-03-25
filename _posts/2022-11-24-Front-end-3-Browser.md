---
layout: single
title: "[Internet] Browser는 어떻게 동작할까?"
categories: WEB 이론
tag: [WEB 이론]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  nav: "docs"
---

## :speech_balloon: 브라우저는 무엇이고 어떻게 동작할까?

### 브라우저의 주요 기능

> 웹 브라우저는 가장 널리 사용되는 소프트웨어이다.
> 
> 'google.com' 을 주소창에 썼을 때 구글 홈페이지가 표시되는 동안
> 
> 뒤에서는 무슨 일이 일어나는지 알아보자.

- 선택한 웹 리소스를 서버에 요청하고 그것을 보여주는 것

- 리소스 : **<u>HTML</u>**, PDF, image ... , URI로 리소스의 위치를 결정

### 브라우저의 구조

1. 유저 인터페이스 : 주소창, 뒤로 및 앞으로 버튼, 북마크 메뉴 등

2. 브라우저 엔진 : UI 와 렌더링 엔진 간의 작업을 marshal ( 정렬 )

3. 렌더링 엔진 : 리소스를 보여주는 역할
   
   > 예를 들어, 리소스가 HTML이라면 엔진이 HTLM과 CSS 를 파싱하고 그것을 화면에
   > 
   > 출력한다

4. 네트워킹 : HTTP 요청과 같은 네트워크 호출을 위해 플랫폼 독립적인 인터페이스 구현을 사용

5. UI 백엔드 : 기본 위젯을 그리는데 사용, 일반 인터페이스 노출

6. 자바스크립트 인터프리터 : Javascript 코드를 분석하고 실행

7. Data sotrage : 항상 지속, 쿠키와 같은 로컬 데이터를 저장, localStorage, IndexedDV, WebSQL, FileSystem과 같은 메커니즘 지원

> 각 탭은 별도의 프로세스에서 진행된다

<img src="https://web-dev.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/PgPX6ZMyKSwF6kB8zIhB.png?auto=format" title="" alt="Browser components" data-align="center">

### 렌더링 엔진

> 브라우저마다 다른 엔진을 사용한다
> 
> IE : Trident , Firefox : Gecko , Safari : Webkit, Chrome : Bling(Webkit)
> 
> Webkit은 Mac 및 Windows를 지원하도록 Apple에서 수정한 렌더링 엔진

### 주요 흐름

<img src="https://web-dev.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/bPlYx9xODQH4X1KuUNpc.png?auto=format" title="" alt="렌더링 엔진 기본 흐름" data-align="center">

1. 엔진은 HTML을 파싱하여 Content tree ( DOM tree) 를 구성

2. CSS, 스타일 요소를 분석하여 Render tree 구성 (DOM tree와 attachment)

3. "layout" : 각 노드가 화면 어디에 표시되어야 하는지 좌표 제공

4. "painting" : render tree를 순회해서 UI 백엔드를 통해 각 노드를 painting

#### 주요 흐름의 예<img src="https://web-dev.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/S9TJhnMX1cu1vrYuQRqM.png?auto=format" title="" alt="WebKit 기본 흐름." data-align="center">

### 1. HTML Parser

- HTML 마크업을 파스 트리로 파싱하는 것

> HTML은 DTD 형식이다, DTD는 문맥자유문법이 아니기 때문에 일반적인 파서들로는 HTML을 파싱할 수 없다. 

#### DOM(Document Object Model)

- HTML Parser의 결과물은 DOM tree

- HTML의 객체 표현, 자바스크립트로 조작하여 화면을 조정할 수 있다

- Tree 의 root 는 'Document'

- Markup 과 1대1 관계를 가진다 

```html
<html>
  <body>
    <p>
      Hello World
    </p>
    <div> <img src="example.png"/></div>
  </body>
</html>

```

위의 마크업은 아래의 DOM tree로 변환된다 

![DOM tree of the example markup](https://web-dev.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/DNtfwOq9UaC3TrEj3D9h.png?auto=format)

> 위에서 말한것 처럼, HTML은 일반적인 파서로 파싱할 수 없으므로 브라우저는 HTML을 위한 파서를 만든다.

#### HTML Parser의 두가지 단계

1. 토큰화
   
   > HTML문서를 분석하여 토큰화 한다.
   > 
   > open tag, tag name, close tag, data 등을 분석하여 반환한다.

2. 트리 구성
   
   > 토큰화된 토큰들을 받아서 DOM Tree 를 구성한다 
   > 
   > HTML 토큰, Head 토큰, Body 토큰 등을 받아서 차례대로 트리를 구성한다 

파싱이 모두 끝난 후 'load' 된다

> **Browser's error tolerance**
> 
> HTML 페이지에서는 'Invalid Syntax' 에러가 발생하지 않는다. 브라우저는 구문 오류를 허용하고 스스로 수정하며 로드한다. 

### 2. CSS Parsing

- CSS 는 문백 자유 문법이라서 일반적인 파서를 통해 구문 분석 가능하다
  
  ```
  ruleset
    : selector [ ',' S* selector ]*
      '{' S* declaration [ ';' S* declaration ]* '}' S*
    ;
  selector
    : simple_selector [ combinator selector | S+ [ combinator? selector ]? ]?
    ;
  simple_selector
    : element_name [ HASH | class | attrib | pseudo ]*
    | [ HASH | class | attrib | pseudo ]+
    ;
  class
    : '.' IDENT
    ;
  element_name
    : IDENT | '*'
    ;
  attrib
    : '[' S* IDENT S* [ [ '=' | INCLUDES | DASHMATCH ] S*
      [ IDENT | STRING ] S* ] ']'
    ;
  pseudo
    : ':' [ IDENT | FUNCTION S* [IDENT S*] ')' ]
    ;
  ```
  
  위와 같은 규칙을 통해 CSS를 분석한다 

### 3. Render tree 구성

1. DOM tree가 구성되는 동안 브라우저는 Render tree를 구성한다

이 트리는 내용을 올바른 순서로 'painting' 할 수 있도록 요소들을 순서대로 정렬해서 가진다

> Render tree는 DOM tree를 통해 생성되지만 1대1 관계는 아니다
> 
> 비시각적 DOM요소들은 Render tree에 삽입되지 않는다
> 
> ex) head, display:none ...

2. Render tree의 틀이 구성된 후 Style Sheet를 분석하여 삽입한다 

브라우저에는 style이 적용되는 여러가지 계층과 규칙이 있다 

이 규칙에 따라 style들을 DOM tree로 부터 만들어진 Render tree에 적용한다

### 4. Layout

- 렌더 트리가 만들어져도 위치나 크기 등을 가지지 않는다. 이것을 계산하는 것을 '**layout'** 또는 **'reflow'** 라고 한다. 

- 좌표는 루트 프레임을 기준으로 하고 위쪽, 왼쪽 좌표를 사용한다

- 레이아웃은 <html>에 해당하는 루트 렌더러부터 시작하는 재귀 프로세스이다. 루트 렌더러의 위치는 0,0 이고 크기는 브라우저의 창인 'viewport' 이다. 그 후 재귀적으로 자식의 layout method를 호출하며 위치와 크기를 계산해 나간다

> **Dirty bit system**
> 
> 작은 변경에 대해 모두를 다시 layout 하지 않기 위해 사용한다. 변경되거나 추가된 렌더러를 'dirty'로 표시 해놓는데 layout이 필요하다는 의미이다.

> **Global layout**
> 
> 모든 렌더트리가 다시 layout을 해야하는 상황이 생길 수 있다
> 
> 1. global style change : ex) font size의 변경
> 
> 2. 스크린의 크기가 조정되었을 때

> **레이아웃의 단계**
> 
> 1. 부모 렌더러는 자신의 넓이를 결정한다
> 
> 2. 자식 렌더러를 배친한다 (x, y 설정)
> 
> 3. 필요한 경우 자식의 레이아웃을 실행한다
> 
> 4. 부모는 자식의 높이와 마진, 패딩 등을 사용하여 자신의 높이를 설정한다
> 
> 5. Dirty bit를 false로 설정한다

#### 5. Painting

- 렌더트리가 순회되고 렌더러의 'paint()' 메서드를 호출한다

- 화면에 콘텐츠들을 표시한다

> **페인팅의 단계**
> 
> 1. background color
> 
> 2. background image
> 
> 3. border
> 
> 4. children
> 
> 5. outline

> **동적 변경**
> 
> background color 와 같은 변경은 요소만 다시 칠할 수 있다
> 
> 하지만, 요소의 위치 변경, DOM 추가, 'html' 태그의 폰트사이즈 변경과 같은 상황에서는 
> 
> layout과 repainting을 해야한다
