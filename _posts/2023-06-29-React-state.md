---
layout: single
title: "[React] 컴포넌트의 state "
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

우리는 컴포넌트를 통해 화면상에 보이는 값, 사진 등이 사용자와의 상호작용에 따라 바뀌어야 하는 경우가 있다. 이 때 우린 컴포넌트의 `state`를 바꿔주면 된다. 

<br>

우리는 아래와 같은 단순한 방법을 통해 화면상의 이미지를 바꾸는 시도를 해볼 수 있다

```jsx
import { sculptureList } from './data.js';

export default function Gallery() {
  let index = 0;

  function handleClick() {
    // 값을 변경해도 리렌더링하지 않아서 반영이 안된다. 
    // 만약 리렌더링한다해도, 이전의 값을 기억하지 못할 것이다.
    index = index + 1;
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i> 
        by {sculpture.artist}
      </h2>
      <h3>  
        ({index + 1} of {sculptureList.length})
      </h3>
      <img 
        src={sculpture.url} 
        alt={sculpture.alt}
      />
      <p>
        {sculpture.description}
      </p>
    </>
  );
}
```

하지만 이는 동작하지 않고 그 이유는 아래와 같다

1. 지역변수를 변화해봤자 리렌더링할 때, 컴포넌트를 처음부터 렌더링하기 때문에 지역변수는 다시 초기화돼있을 것이다.

2. 애초에 지역변수를 변화시켜도 리렌더링은 일어나지 않는다. 

따라서 우리는 아래의 두 조건을 만족시켜야 컴포넌트를 업데이트 시킬 수 있다.

1. 리렌더링될때 이전의 값을 기억할 수 있어야 한다.

2. 값이 변경되면 리렌더링을 시켜줘야한다. 

`useState` 라는 훅을 사용하면 위의 조건을 만족시킬 수 있다. 

## 1. useState 사용하기

```jsx
import { useState } from 'react';

...
...
...

const [index, setIndex] = useState(0);
```

기본 사용법이다. 반드시 사용전에 `import` 를 해줘야 한다.

우리는 위에서의 두가지 조건을 충족시켜줄 수 있다. 

`index` -> **이전의 상태(값)** 을 기억

`setIndex` -> 값을 변경하고 **리렌더링**

그럼 우린 이제 아래와 같이 사진을 변경해줄 수 있다. 

```jsx
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);

  function handleClick() {
    // 값을 변경함과 동시에 리렌더링한다. 
    setIndex(index + 1);
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i> 
        by {sculpture.artist}
      </h2>
      <h3>  
        ({index + 1} of {sculptureList.length})
      </h3>
      <img 
        src={sculpture.url} 
        alt={sculpture.alt}
      />
      <p>
        {sculpture.description}
      </p>
    </>
  );
}
```

> **Hook**
> 
> 우리는 `useState`라는 hook을 사용해봤다. 리액트에서 `use`로 시작하는 함수를 hook이라고 부르는데, 리액트가 렌더링될 때 사용할 수 있는 특별한 함수이다.
> 
> 조건문, 반복문 등에서 사용할 수 없고 반드시 컴포넌트의 가장 바깥에서 사용해야한다. 

### useState 설명

`useState`에 대해서 자세히 알아보고 동작 순서도 알아보자.

```jsx
const [index, setIndex] = useState(0);
```

`useState`의 인자는 하나이고, 그 값은 `state`의 초기값이다. 

*동작 순서*

`useState`는 아래의 과정을 통해 동작한다.

1. 컴포넌트가 처음 렌더링된다. 값은 초기값 `0`으로 초기화되고 이 값을 기억하고 있는다

2. 사용자가 버튼을 클릭하고 `setIndex(index+1)` 이 호출되면, `index` 값은 `1`로 바뀌고 리액트는 이를 기억한다. 그 후, 리렌더링을 요청한다. 

3. 리렌더링되면 `useState(0)` 가 또 실행되지만 리액트는 `index`가 `1`임을 기억하고 있기 때문에 `useState()`는 `[1, setIndex]` 를 반환한다. 

> **상태는 독립적이다.**
> 
> 같은 컴포넌트에 대해서 서로 다른 인스턴스가 2개 있다고 할 때, <u>두 상태는 독립적으로 존재</u>한다. 즉 한쪽의 상태가 바뀌어도 다른 인스턴스에는 영향이 가지 않는 것이다. 
