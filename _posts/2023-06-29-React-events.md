---

layout: single
title: "[React] 이벤트 "
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

우리는 JSX에 사용자가 발생시키는 어떤 이벤트에 반응하는 **이벤트 핸들러**를 추가할 수 있다. 리액트에서 주로 우린 이벤트 핸들러를 통해 `state`의 값을 변경해주곤 한다.

<br>

주로 이벤트 핸들러 함수를 컴포넌트 내부에 선언하고, props로 넘겨줌으로써 추가한다.

```jsx
export default function Button() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

또는 JSX 내부에 바로 핸들러를 선언할 수도 있다

```jsx
<button onClick={function handleClick() {
  alert('You clicked me!');
}}>
```

> **함수를 호출하는 것이 아닌, 넘겨줘야 한다.**
> 
> 함수 전달 : `<button onClick={handleClick}>` 
> 
> 함수 호출 : `<button onClick={handleClick()}>`
> 
> 함수를 전달하지 않고 호출하면, 이벤트가 발생하던 안하던 렌더링될 때마다 함수가 호출된다. 우린 이벤트가 발생했을 때만 핸들러가 실행되어야 하므로 함수를 넘겨주기만 해야한다.
> 
> 따라서 인라인으로 선언시에도 
> 
> `<button onClick={() => alert('...')}>`
> 
> 화살표 함수를 통해 함수 자체를 만들어서 넘겨주기만 해야지, 절대 호출하거나 실행시키면 안된다.

## 이벤트 핸들러와 props

이벤트 핸들러에서 props를 사용할 수도 있고, props로서 이벤트 핸들러를 넘겨줄 수도 있다.

*이벤트 핸들러에서 props 사용하기*

```jsx
function AlertButton({ message, children }) {
  return (
    <button onClick={() => alert(message)}>
      {children}
    </button>
  );
}
```

props로서 이벤트 핸들러 넘겨주기

props로 이벤트 핸들러를 넘겨줄 땐 꼭 브라우저가 정해준 이름이 아닌 `onSmash`와 같이 자유롭게 바꿔 쓸 수 있다. props로 넘겨준 후 사용할 때만 `onClick`에 잘 넣어주면 되는 것이다.

```js
function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}

function PlayButton({ movieName }) {
  function handlePlayClick() {
    alert(`Playing ${movieName}!`);
  }

  return (
    <Button onClick={handlePlayClick}>
      Play "{movieName}"
    </Button>
  );
}
```

## 이벤트 전파(event propagation)

부모와 자식이 같은 이벤트에 대해서 핸들러가 있을 때, 자식에서 이벤트가 발생하면 같은 이벤트에 대해서 부모에서도 이벤트가 발생한다. 이를 이벤트가 전파된다고 한다.

*이벤트 전파 방지*

우리는 이벤트가 발생하면 이벤트 핸들러로 **이벤트 객체**를 넘겨준다. 주로 `e`로 사용하며, 우리는 이 객체를 통해 여러 정보를 얻을 수 있다. 

이 객체를 통해 우리는 이벤트가 전파되는 것을 막을 수 있는데 그때 사용하는 함수가 `e.stopPropagtion()` 이다. 이벤트 핸들러에서 해당 함수를 호출하면 해당 컴포넌트를 마지막으로 더 이상 부모 컴포넌트로는 이벤트가 전파되지 않는다. 

*이벤트 캡처링*

이벤트 이름 뒤에 `Capture`를 붙이면 이벤트 캡처링을 할 수 있다. 이는 자식 컴포넌트에서 이벤트가 발생하면 가장 위의 캡처링 핸들러가 가장 먼저 실행되게 한다. 그 이후 순서는 아래와 같다.

1. 이벤트 발생

2. 가장 부모 컴포넌트의 캡처링 핸들러 실행

3. 아래로 내려가면서 캡처링 핸들러 실행

4. 이벤트 발생한 컴포넌트에 오면 이벤트 핸들러 실행

5. 위로 올라가면서 이벤트 핸들러 실행

## 기본 이벤트 방지

`<form>`의 `submit` 이벤트에 페이지가 리로드되는 것과 같이 브라우저에는 특정 이벤트에 대해서 기본적으로 실행되는 핸들러가 있을 수 있다. 

우리는 이를 `e.preventDefault()`를 통해 막을 수 있다.


