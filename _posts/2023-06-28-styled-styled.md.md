---
layout: single
title: "styled-components"
categories: Styled-Components
tag: [Styled-Components]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"
header:
 teaser: /assets/images/styled.png

---

*기존 CSS의 문제점*

1. Global Namespace : 전역 변수를 지양해야하는 JS와 대치

2. Dependencies : css간의 의존 관리

3. Dead Code Elimination : 안쓰는 css인지 알기 어려움

4. Minification : 클래스 이름 최소화 

5. Sharing Constants : JS의 코드와 값을 공유하고 싶을 때

6. Non-deterministic Resolution : 어떤 css 파일을 먼저 로드했냐에 따른 이슈

7. Isolation : 스타일이 컴포넌트 별로 격리되어 있지 않다

-> 이러한 문제점들을 해결하기 위해 나온 것이 **CSS in JS**

*styled-components*

스타일만 있는 리액트 컴포넌트를 만들어주는 라이브러리이다.

```jsx
// 스타일을 가지는 Title이라는 이름을 가지는 <h1> 컴포넌트를 만든다.
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: #BF4F74;
`;

// 스타일을 가지는 Wrapper라는 이름을 가지는 <section> 컴포넌트를 만든다.
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

// 다른 컴포넌트처럼 사용할 수 있다.
render(
  <Wrapper>
    <Title>
      Hello World!
    </Title>
  </Wrapper>
);
```

*props에 따라 적용하기*

아래와 같이 styled-components를 통해 생성한 컴포넌트에 props를 넘겨줌으로써

같은 컴포넌트에 대해서 다양한 스타일도 만들 수 있다.

```jsx
const Button = styled.button<{ $primary?: boolean; }>`
  /* Adapt the colors based on primary prop */
  background: ${props => props.$primary ? "#BF4F74" : "white"};
  color: ${props => props.$primary ? "white" : "#BF4F74"};

  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid #BF4F74;
  border-radius: 3px;
`;

render(
  <div>
    <Button>Normal</Button>
    <Button $primary>Primary</Button>
  </div>
);
```

*기존 컴포넌트의 확장*

생성한 컴포넌트를 상속해서 기존의 스타일을 적용시킨채로 새로운 컴포넌트를 만들 수 있다.

```jsx
// The Button from the last section without the interpolations
const Button = styled.button`
  color: #BF4F74;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid #BF4F74;
  border-radius: 3px;
`;

// A new component based on Button, but with some override styles
const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

render(
  <div>
    <Button>Normal Button</Button>
    <TomatoButton>Tomato Button</TomatoButton>
  </div>
);
```
