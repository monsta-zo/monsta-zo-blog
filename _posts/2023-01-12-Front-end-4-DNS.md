---
layout: single
title: "[Internet] DNS란 무엇일까?"
categories: WEB 이론
tag: [WEB 이론]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"

---

## :orange_book: DNS란 무엇일까?

- **DNS(Domain Name System)**는 쉽게 말하자면 인터넷의 전화번호부이다

> 우리는 전화를 걸때 번호를 일일이 외우지 않고 이름을 검색해서 전화를 건다.
> 
> **<u>전화번호 -> IP주소 , 이름 -> Domain Name</u>**
> 
> 즉, 다른 기기에 접속할 때 직접 IP주소를 입력해서 접속하는 것이 아니라 
> 
> naver.com, google.com 과 같은 IP주소에 이름을 붙힌 Domain Name으로 접근하고
> 
> 이것들을 모아둔 **<u>전화번호부가 DNS</u>** 이다.

### DNS는 어떻게 동작할까?

- DNS 프로세스에는 hostname(예: www.example.com)을 IP 주소(예:192.168.1.1)로 변환하는 과정이 포함된다

> 웹페이지를 로드하는데 필요한 4개의 DNS 서버
> 
> - DNS recursor : 도서관의 사서와 같은 역할, 웹 브라우저를 통해 클라이언트로 부터 해당 IP주소를 찾아달라는 쿼리를 받는다
> 
> - Root nameserver : 책장의 위치를 가르치는 인덱스 역할, IP주소로 변환하는 첫 단계
> 
> - TLD nameserver : Top Level Domain 서버 , 도서관의 특정 책장, 호스트 이름의 마지막 부분을 호스팅한다 (예: example.com 에서의 'com')
> 
> - Authoritative nameserver : 최종 nameserver, IP주소를 DNS rucursor한테 반환한다. 

### DNS 조회의 단계

- DNS는 도메인 이름을 적절한 IP주소로 변환하는 일에 관여한다. 이를 DNS 조회 프로세스를 통해 알아보자 

> 일반적으로 DNS 조회는 캐시된 정보를 통해 더 빠르게 이루어진다. 하지만 캐시되지 않았을 때의 8단계 모두를 확인해보자. 
> 
> (여기서 **쿼리**라는 것은 모두 **<u>질문</u>**으로 해석하면 이해가 빠를 것이다)

![DNS query diagram](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-lookup-diagram.png)

1. 사용자가 'naver.com'을 입력하면 쿼리가 DNS Resolver가 등록이 되어있다면 바로 알려준다.

2. 등록이 되어 있지 않다면 Root Server에 문의한다.

3. Root Server는 TLD가 .com 인것을 확인한 후, com Server에 문의 해보라고 ".com" 이 등록된 TLD Server의 IP주소를 알려준다.

4. com DNS에 'naver.com'을 문의한다.

5. com DNS에서는 'naver.com'을 관리하는 서버에 문의하도록 naver.com Server의 IP주소를 알려준다.

6. 마지막으로 naver.com Server에 문의한다.

7. 해당 IP주소가 반환된다.

8. 해당 IP주소를 웹브라우저에 반환한다.

9. 브라우저가 IP주소로 HTTP 요청을 보낸다

10. 해당 IP의 서버가 렌더링할 웹페이지를 반환한다.

> DNS Resolver란?
> 
> 클라이언트에게 최초로 요청을 받아서 IP주소를 반환하기 위한 나머지 과정들을 수행하는 역할 

#### DNS query 의 종류

1. Recursive query : 클라이언트가 Resolver에게 요청한 IP주소 또는 에러메세지를 받는 것

2. Iterative query : Resolver가 서버에게 주소를 요청하고 서버에 없다면 하위 수준의 DNS 서버의 주소를 반환해준다. 그러면 다시 resolver는 하위에 서버에 가서 요청한다

3. Non-recursive query : DNS서버가 해당 주소를 캐시에 가지고 있거나 권한이 있어서 주소를 반환해주는 것

#### DNS 캐싱

- 데이터를 클라이언트와 가까운 곳에 임시 저장하는 것

- 쿼리를 초기해 확인하고 조회 과정의 추가 쿼리를 피할 수 있으므로 로딩 시간이 향상되고 여러가지 소비가 줄어든다. 
