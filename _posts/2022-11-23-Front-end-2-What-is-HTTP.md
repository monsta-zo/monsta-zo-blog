---
layout: single
title: "[Internet]HTTP란?"
categories: WEB
tag: [WEB]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  nav: "docs"
---

## HTTP란 무엇일까?

### :pushpin: HTTP란?

<u>H</u>yper<u>t</u>ext <u>T</u>ransfer <u>P</u>rotocol 의 줄임말이다.

- Hypertext : 문서내의 특정 부분이 다른 문서와 연결되어 있는 텍스트

- Protocol : 데이터 통신을 원할하게 하기 위한 통신 규약

웹의 기반이며 하이퍼텍스트 링크를 통해 웹을 로드하기 위해 사용한다. 주 흐름은 **client**가 **server**에게 요청을 하면 server가 응답을 하는 방식이다.

### :pushpin:HTTP 요청이란?

HTTP요청이란 웹 브라우저가 웹 사이트를 로드하기 위해 필요한 정보를 요청하는 것이다. HTTP 요청은 아래와 같은 정보들을 가진다

1. HTTP version type

2. URL

3. HTTP method

4. HTTP request headers

5. Optional HTTP body

#### :round_pushpin:HTTP method 란?

HTTP verb 라고도 불리며, HTTP 요청이 서버에게 기대하는 바를 말한다.

크게 'GET' 과 'POST'가 있다.

- **GET** : 요청에 대한 응답으로 정보를 요구한다. ( 보통, 웹 사이트의 형태)

- **POST** : 클라이언트가 웹 서버에 정보를 제출한다. ( form 정보 e.g. 유저 네임, 비밀번호 )

#### :round_pushpin:HTTP request header 란?

HTTP header는 key-value 형태의 정보를 보함한다. 이러한 헤더는 클라이언트가 어떤 데이터를 요청하는지와 같은 정보를 전달한다.

<img title="" src="https://www.cloudflare.com/img/learning/ddos/glossary/hypertext-transfer-protocol-http/http-request-headers.png" alt="HTTP request headers" data-align="center" width="426">

#### :round_pushpin:HTTP request body 란?

body는 웹 서버로 **제출되는 정보**들을 포함한다. 예를 들어 유저네임이나 비밀번호 등, form으로 입력되는 모든 정보들이 있다.

### :pushpin:HTTP response 란?

HTTP 응답이란 웹 클라이언트가 웹 서버로부터 **HTTP 요청에 의해 요구된 정보**들이다. 아래와 같은것들을 포함한다.

1. an HTTP status code

2. HTTP response headers

3. optional HTTP body

#### :round_pushpin:HTTP status code 란?

3자리의 code이며 HTTP request가 성공적으로 완료되었는지에 대한 상태를 나타낸다. 크게 5가지로 나뉜다.

1. 1xx Informational

2. 2xx Success ( e.g. 200 OK)

3. 3xx Redirection

4. 4xx Client Error ( e.g. 404 NOT FOUND)

5. 5xx Server Error

#### :round_pushpin:HTTP response header 란?

request body로 부터 전송되는 데이터의 언어 및 형식과 같은 정보를 포함하고 있다.

<img title="" src="https://www.cloudflare.com/img/learning/ddos/glossary/hypertext-transfer-protocol-http/http-response-headers.png" alt="HTTP response headers" data-align="center" width="382">

#### :round_pushpin:HTTP response body 란?

'**GET**' 요청에 대한 성공적인 응답은 주로 요청된 정보를 포함한 body를 가진다. 이 body는 주로 웹 브라우저가 웹 페이지로 변환하는 **HTML 데이터**이다.
