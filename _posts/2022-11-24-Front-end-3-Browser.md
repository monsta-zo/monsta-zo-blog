---
layout: single
title: "[Internet] Browser는 어떻게 동작할까?"
categories: Front-end
tag: [Front-end, Internet]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  nav: "docs"
---

## :speech_balloon:브라우저는 무엇이고 어떻게 동작할까?

### 브라우저의 주요 기능

---

> 웹 브라우저는 가장 널리 사용되는 소프트웨어이다.
>
> 'google.com' 을 주소창에 썼을 때 구글 홈페이지가 표시되는 동안
>
> 뒤에서는 무슨 일이 일어나는지 알아보자.

- 선택한 웹 리소스를 서버에 요청하고 그것을 보여주는 것

- 리소스 : **<u>HTML</u>**, PDF, image ... , URI로 리소스의 위치를 결정

### 브라우저의 구조

---

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

---

> 브라우저마다 다른 엔진을 사용한다
>
> IE : Trident , Firefox : Gecko , Safari : Webkit, Chrome : Bling(Webkit)
>
> Webkit은 Mac 및 Windows를 지원하도록 Apple에서 수정한 렌더링 엔진

#### 기본 흐름

---

<img src="https://web-dev.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/bPlYx9xODQH4X1KuUNpc.png?auto=format" title="" alt="렌더링 엔진 기본 흐름" data-align="center">

1. 엔진은 HTML을 파싱하여 Content tree ( DOM tree) 를 구성

2. CSS, 스타일 요소를 분석하여 Render tree 구성 (DOM tree와 attachment)

3. "layout" : 각 노드가 화면 어디에 표시되어야 하는지 좌표 제공

4. "painting" : render tree를 순회해서 UI 백엔드를 통해 각 노드를 painting

#### 기본 흐름의 예

---

<img src="https://web-dev.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/S9TJhnMX1cu1vrYuQRqM.png?auto=format" title="" alt="WebKit 기본 흐름." data-align="center">
