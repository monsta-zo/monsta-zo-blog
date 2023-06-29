---
layout: single
title: "[React] 컴포넌트간에 props 주고받기"
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

**props(프로퍼티, 특성)** 은 <u>객체, 배열, 함수 등 자바스크립트의 모든 값</u>들을 말하며 리액트 컴포넌트들은 서로 props를 주고 받을 수 있다. 

<br>

우리는 이미 props를 사용해봤다. 예를 들어 `className`, `src`, `alt`, `width`, `height` 등은 `<img>` 태그에 넣을 수 있는 props이다. (HTML에서 속성과 같은 개념)

```js
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Lin Lanying"
      width={100}
      height={100}
    />
  );
}

export default function Profile() {
  return (
    <Avatar />
  );
}
```

이와 같이 `<img>` 태그에 사용가능한 `props`는 <u>HTML 표준</u>으로 지정되어있지만, **우리가 직접 만든 컴포넌트에는 어떠한 props도 사용할 수 있다.**

## 1. props 전달하기

*1.자식 컴포넌트에게 props 전달*

아래와 같이 컴포넌트의 시작 태그의 안에 HTML의 속성과 같이 넘겨줄수 있다. 

```jsx
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

*2.자식 컴포넌트 안에서 props 사용하기*

부모 컴포넌트에서 넘겨준 props는 `props`라는 하나의 객체로 자식 컴포넌트에게 전달된다.

우리는 이 객체를 구조분해할당 해서 사용하거나, 그대로 사용할 수 있다. 즉, 위에서 넘겨준 props는 아래와 같은 것이다.

```jsx
props = {
    person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }},
    size={100}
}
```

해당 props들은 자식 컴포넌트의 인수로 넘어가게 되고 그냥 함수에서 사용하듯이 사용하면 된다. 하지만 JSX에서 JavaScript 문법을 사용하는 것이므로 반드시 중괄호와 함께 사용하자

```jsx
// 구조 분해 할당해서 사용하기
function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}
```

> **props 기본값 지정해주기**
> 
> 구조분해할당의 방법과 같다. 
> 
> ```jsx
> function Avatar({ person, size = 100 }) {
>   // ...
> }
> ```

> **props 통째로 넘겨주기**
> 
> 가끔 props를 현재 컴포넌트에서 사용하지 않고 모두 자식 컴포넌트로 넘겨주는 경우가 있다. 그럴 땐 아래와 같이 spread 문법을 이용해서 넘겨줄 수 있다.
> 
> ```jsx
> function Profile(props) {
>   return (
>     <div className="card">
>       <Avatar {...props} />
>     </div>
>   );
> }
> ```

## 2. JSX를 props로 전달하기

우리는 아래와 같이 `Avatar`라는 컴포넌트 자체를 `Card` 컴포넌트의 props로 넘겨줄 수 있다. 

우리는 `Card`라는 컴포넌트 안에서 유동적으로 컴포넌트들을 사용하기 위해 이러한 방법을 사용할 수 있다. 즉, `Avatar` 뿐만 아니라 내가 만든 어떠한 컴포넌트든지 넘겨줄 수 있는 것이다.

```jsx
function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      /> 
    </Card>
  );
}
```

> **props의 값은 변화할 수 있다.**
> 
> 만약 부모 컴포넌트에서 props 값을 계속 바꿔서 넘겨준다면 자식 컴포넌트에서는 바뀐 props 값들을 항상 반영한다 -> `state` 개념 
