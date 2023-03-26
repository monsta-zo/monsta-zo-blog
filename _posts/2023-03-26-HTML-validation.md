---

layout: single
title: "[HTML] 유효성 검사"
categories: HTML
tag: [HTML]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"

---

# Client-side form validation

우리가 데이터를 `form` 에 작성해서 제출하고 서버로 보내지기 전에, `form` 에서 필요한 모든 데이터를 작성했는지, 올바른 형식인지를 확인하는 것을 **Client-side form validation** 우리말로 **사용자 측 유효성 검사** 라고한다.

이러한 **유효성 검사**는 <u>형식에 맞지 않는 데이터를 서버로 보내기전에 잡아냄</u>으로써, 데이터가 서버로 왔다갔다하는 시간을 줄여주고, 그 자리에서 직접 수정할 수 있다는 좋은 사용자 경험을 이끌어 낼 수 있다.

하지만, **사용자 측 유효성 검사**는 우회하기가 쉽고 <u>보안성이 높지 않기</u> 때문에 반드시 **서버 측 유효성 검사**도 함께 실시해줘야한다. 

## 1. 유효성 검사란 무엇인가?
