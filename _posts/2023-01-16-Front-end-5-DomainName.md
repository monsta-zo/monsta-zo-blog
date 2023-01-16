---
layout: single
title: "[Internet] Domain Name에 대해서"
categories: Front-end
tag: [Front-end, Internet]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"

---

### :envelope: Domain Name이란 무엇일까?

- <u>IP 주소를 사람이 읽고 기억하기 쉬운 주소로 변경한 것</u>

> 모든 인터넷에 연결된 컴퓨터들은 고유의 IP주소 (IPv4 or IPv6)를 가지고 있다
> 
> 컴퓨터는 이러한 주소를 쉽게 처리하지만 사람은 IP주소만 봐서는 어디인지 무슨 서비스를 제공하는지 알수 없기 때문에 
> 
> IP주소 대신에 **Domain Name**을 사용한다

### :envelope: Domain Name의 구조

<img title="" src="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_domain_name/structure.png" alt="Anatomy of the MDN domain name" width="360" data-align="center">

- 점으로 구분되고 오른쪽에서 왼쪽으로 읽는다 

> **<u>TLD ( Top-Level Domain )</u>**
> 
> 서비스의 일반적인 목적을 알려준다. 일반적인 TLD인 .com, .org, .net 등은 충족해야할 기준이 없지만 일부 TLD는 엄격한 정책을 가지기도 한다 ( ex, .us .fr .gov .edu ... )
> 
> 특수 문자와 라틴문자로 구성되며 최대 63자이다 

> **<u>Label ( or Component )</u>**
> 
> TLD의 앞에 있는 것들이다. 문자, 숫자, - 을 포함한 최대 63자로 이루어져 있으며 상위 도메인 (ex, mozilla.org ) 에 대해서 하위 도메인들 (developer.mozilla.org, iot.mozilla.org ) 들을 만들수 있다. 이때, TLD바로 앞의 lable을 SLD (Second Level Domain) 이라고도 한다 

### :envelope: Domain Name의 소유

- 도메인 이름을 구입할 순 없지만, 도메인 이름을 사용할 수 있는 <u>권리에 대한 비용을 지불해서 우선순위</u>를 가질 수 있다 

- **registrars**로 불리는 회사가 이를 담당한다

> **사용가능한 Domain Name 찾기**
> 
> 1. registrar 홈페이지에 가서 'whois' 서비스를 이용한다
> 
> 2. shell 에서 'whois' 명령어를 사용하면 해당 Domain Name을 누가 사용하고 있는지 알 수 있다. 

> **Domain Name 구매하기**
> 
> 1. registrar 홈페이지에 간다
> 
> 2. "Get a domain name" 과 같은 버튼을 클릭한다
> 
> 3. 모든 세부 정보 양식을 작성한다
> 
> 4. 등록기관은 Domain Name을 등록하고 모든 DNS 서버들은 이를 수신한다

### :envelope: DNS request는 어떻게 동작할까?

> 1. mozilla.org 를 브라우저의 주소창에 입력한다
> 
> 2. **local DNS cache** 를 이용하여 컴퓨터에게 이미 Domain Name에 대응하는 IP주소를 아는지 물은 후, IP주소로 변환해 브라우저를 통해 출력한다
> 
> 3. 모른다면, DNS 서버에 요청해서 IP주소를 받아온다
> 
> 4. IP주소를 통해 브라우저에 출력한다 
