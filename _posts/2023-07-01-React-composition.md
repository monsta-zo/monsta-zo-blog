---

layout: single
title: "[React] 합성(composition) "
categories: React
tag: [React]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"
header:
 teaser: /assets/images/react.png

---

## Composition(합성)

우리는 메세지 상자를 띄울 때, 해당 메세지에는 축하 인사가 들어갈 수도 있고, 환영 인사, 아니면 어떤 이벤트를 설명할 수도 있다. 

이때, 메세지 상자는 일반적인 컴포넌트이고 축하 인사가 들어간 메세지 상자, 환영 인사가 들어간 메세지 상자 등은 특별한 컴포넌트이다. 

즉, 우리는 일반적인 컴포넌트를 만들어 놓고 여기에 어떤 props를 전달해주냐에 따라서 여러 특별한 컴포넌트들을 만들 수 있다. 

```jsx
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

이 방법을 통해 우린 코드의 재사용성을 높일 수 있다.
