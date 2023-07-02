---
layout: single
title: "[React] Refs 사용하기 "
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

## 1. Refs의 기본적인 동작

컴포넌트 내부에서 사용할 수 있는 변수는 크게 3가지인데, 하나는 `state`, 하나는 `지역변수` 또 하나는 `ref`이다. 

`ref`는 컴포넌트 내부에서 사용할 수 있는 변수이며, `state`와 비슷한 것 같으면서도 다르다. 

가장 큰 특징은 언제나 값을 변경할 수 있다는 것이다. `state`는 `setter`를 통해 값을 변경한다. 하지만 `ref`는 언제든 값을 변경할 수 있다. 그리고 `state`와 다르게 값을 변경해도 컴포넌트가 **리렌더링되지 않는다**. 한마디로 `ref`는 리렌더링을 요구하지 않는 `state`라고 볼 수 있다. 하지만 그 값은 기억하고 있는다. 즉 이점에서는 또 `지역변수`와 다르다. 지역변수는 컴포넌트가 리렌더링될 때 마다 값이 초기화 되지만, `ref`는 값을 기억하고 있다. 

즉, `ref`와 `state`와 `지역변수`를 비교해보면 아래와 같다.

|                  | ref | state      | 지역변수 |
| ---------------- | --- | ---------- | ---- |
| 값이 변경되면<br/>리렌더링 | X   | O          | X    |
| 값 변경가능           | O   | set 함수로 변경 | O    |
| 값을 기억하는 지        | O   | O          | X    |

`useRef()`를 통해 `ref`를 초기화할 수 있으며, 값에 `ref.current`를 통해 접근가능하다.

```jsx
export default function Counter() {
  let ref = useRef(0);

  function handleClick() {
    ref.current = ref.current + 1;
    alert('You clicked ' + ref.current + ' times!');
  }

  return (
    <button onClick={handleClick}>
      Click me!
    </button>
  );
}
```

`ref`는 주로 컴포넌트 외부와 소통하기 위해 사용하는 경우가 많다. 하지만 `ref`의 가장 크고 쓰임새는 바로 **DOM에 접근**하기 위해서이다. 

하지만 특정 값을 기억하고 추적해야하지만, 변경되었을 때 리렌더링이 되는 걸 원하지 않는다면 `useRef`를 사용해보자

## 2. Refs로 DOM 다루기

*일반 요소들 다루기*

```jsx
const myRef = useRef(null);
```

컴포넌트 내부에서 `ref`를 선언해준 후, JSX에서 접근하고자하는 DOM에 `ref`를 넘겨주면 된다.

```jsx
<div ref={myRef}>
```

리액트에서 자동으로 해당 DOM노드를 `myRef`에 할당해준다. 이제 `myRef.current`를 통해 해당 DOM에 접근할 수 있으며, `focus()`와 같은 작업을 실행할 수 있다. 

```jsx
import { useRef } from 'react';

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}
```

*다른 컴포넌트 다루기*

리액트에서는 다른 컴포넌트의 DOM으로의 접근을 방지하고 있기 때문에, 위의 예제처럼 한다면 접근할 수 없다. 

```jsx
const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />;
});
```

위와 같이 컴포넌트를 `forwardRef`를 통해 정의해줘야하고 props로 받은 `ref`를 DOM에게 전달해줘야 한다. 

```jsx
import { forwardRef, useRef } from 'react';

const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />;
});

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}
```

> **refs를 통해 DOM 조작할 떄 주의해야 할점**
> 
> 리액트에서 특정 DOM을 제어한다면 그 DOM을 삭제하거나, 수정하면 안된다.
> 
> 단순한 `focus()`와 같은 작업은 DOM을 수정하지 않기 때문에 괜찮지만 그 외에 DOM에 변경사항을 가하는 것은 리액트가 수정된 DOM에 대해서 일관성을 잃게되고 여러가지 문제를 야기할 수 있다. 따라서 주로 DOM을 수정해야한다면 우린 `state`를 쓸 때가 많다.
> 
> 하지만 성능면에서 리렌더링을 안하는 것이 더 선호될 때가 있다. 이럴 떈 DOM을 직접 조작하곤 한다. 

## 3. callback ref로 DOM에 접근하기

리렌더링될 때마다 DOM에 접근하고 싶을 수 있다. 이때 callback ref를 사용할 수 있다.

```jsx
function ComponentWithRefRead() {
  const [text, setText] = React.useState('Some text ...');

  function handleOnChange(event) {
    setText(event.target.value);
  }

  const ref = (node) => {
    if (!node) return;

    const { width } = node.getBoundingClientRect();

    document.title = `Width:${width}`;
  };

  return (
    <div>
      <input type="text" value={text} onChange={handleOnChange} />
      <div>
        <span ref={ref}>{text}</span>
      </div>
    </div>
  );
}
```

위의 코드는 첫 렌더링 한번에만 `ref` 함수가 실행된다. 우린 `useCallback`을 이용하면 `useEffect`와 같이 어떤 상황마다 함수를 실행할지 정해줄 수 있다. 

```jsx
function ComponentWithRefRead() {
  const [text, setText] = React.useState('Some text ...');

  function handleOnChange(event) {
    setText(event.target.value);
  }

  const ref = React.useCallback((node) => {
    if (!node) return;

    const { width } = node.getBoundingClientRect();

    document.title = `Width:${width}`;
  }, []);

  return (
    <div>
      <input type="text" value={text} onChange={handleOnChange} />
      <div>
        <span ref={ref}>{text}</span>
      </div>
    </div>
  );
}
```
