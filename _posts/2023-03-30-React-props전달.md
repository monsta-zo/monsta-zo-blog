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

# props(프로퍼티)

**props(프로퍼티, 특성)** 은 <u>객체, 배열, 함수 등 자바스크립트의 모든 값</u>들을 말한다. 리액트 컴포넌트들은 서로 props를 주고 받을 수 있고, 주로 부모 컴포넌트가 자식 컴포넌트에 props를 통해 정보를 전달한다.

## 1. JSX 태그에서의 props

우리는 이미 JSX 태그를 만들 때, props를 사용해봤다. 예를 들어 `className`, `src`, `alt`, `width`, `height` 등은 `<img>` 태그에 넣은 수 있는 props이다. (HTML에서 속성과 같은 개념)

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

이와 같이 `<img>` 태그에 사용가능한 `props`는 `HTML 표준`으로 지정되어있지만 **우리가 직접 만든 컴포넌트에는 어떠한 props도 사용할 수 있다.**

## 2. 구성요소간의 props

이제는 부모 컴포넌트에서 자식 컴포넌트로 props를 전달해보겠다.

1. **자식 컴포넌트로 props 전달**

`Profile` 이라는 컴포넌트에서 `Avatar` 컴포넌트로 **두가지 props**, `person` (객체) 와 `size` (수)를 전달하는 코드이다. JSX 문법상 반드시 {} 안에 넣어서 전달해야 한다.  

```js
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

2. **자식 컴포넌트에서 props 사용하기**

아래와 같이 **구조 분해 할당**을 통해서 부모에서 온 props를 전달받을 수 있다.

```js
function Avatar({ person, size }) {
  // person and size are available here
}
```

이제 `Avatar` 컴포넌트의 정의 내부에서 `person`과 `size`를 마음껏 사용할 수 있다.

```js
// App.js
import { getImageUrl } from './utils.js';

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

export default function Profile() {
  return (
    <div>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi', 
          imageId: 'YfeOqp2'
        }}
      />
      <Avatar
        size={80}
        person={{
          name: 'Aklilu Lemma', 
          imageId: 'OKS67lh'
        }}
      />
      <Avatar
        size={50}
        person={{ 
          name: 'Lin Lanying',
          imageId: '1bX5QH6'
        }}
      />
    </div>
  );
}

// utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

함수에서 인자를 변수로써 사용하는 개념과 같이, 컴포넌트는 전달받은 props를 내부에서 변수처럼 사용할 수 있다. props를 구조 분해 할당을 통해서 사용할 수도 있지만, 하나의 객체로써 전달 받은 후 함수 내부에서 각각의 프로퍼티에 접근해서도 사용할 수 있다. 

```js
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
```

## 기본값(default) 지정하기

부모 컴포넌트에서 값을 지정해주지 않았을 때, default값을 사용하게 할 수 있다. 

```js
function Avatar({ person, size = 100 }) {
  // ...
}
```

이제 `<Avatar person={...}/>` 과 같이 부모 컴포넌트에서 `size` 값을 넘겨주지지 않더라도, 자식 컴포넌트에서는 100을 기본값으로 사용할 수 있다. 

> default값은 값이 넘어오지 않거나 `defined` 로 넘어왔을 때만 사용된다, 만약 `0`이거나 `null`로 넘어온다면, default값이 사용되는 것이 아닌 각 값이 그대로 사용된다.

## JSX spread문법을 통한 prop 전달

가끔 부모로부터 전달받은 props를 사용하지 않고, 통째로 자신의 자식 컴포넌트에 전달해야하는 경우가 있다. 예를 들면 아래와 같은 경우이다.

```js
function Profile({ person, size, isSepia, thickBorder }) {
  return (
    <div className="card">
      <Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
    </div>
  );
}
```

위의 코드는 문제가 없지만, **spread 문법**을 사용하면 더 간결하게 표현할 수 있다.

> **spread 문법**
> 
> spread 문법은 반복가능한 객체의 요소들을 하나씩 풀어서 나타내는 역할을 한다. 즉, {a, b, c}를 a, b, c 로 나눠서 표현해주는 것이다.

```js
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

이렇게 하면 굳이 props를 나열할 필요 없이 한번에 보낼 수 있다

## JSX를 props로 전달하기

우리는 JSX 코드 자체를 props로 전달할 수도 있다. 

**[Avatar.js]**

```js
import { getImageUrl } from './utils.js';

export default function Avatar({ person, size }) {
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

**[App.js]**

```js
import Avatar from './Avatar.js';

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

위의 코드를 보면 `Profile` 컴포넌트에서 `Card` 라는 자식 컴포넌트를 호출하는데 `props` 로써 특정 값을 넘겨주는 것이아닌 **JSX를 통째로 넘겨주고 있다.** 이 JSX를 자식 컴포넌트에서는 `children` 으로 받았고 그 값을 자식 컴포넌트 내부에서 그대로 사용하고 있다. 그리고 <u>이 JSX는 다른 컴포넌트 또한 가질 수 있다.</u> 

<br>

우리는 특정 컴포넌트에 대해서 props의 값이 다를 뿐 아니라 JSX 코드의 형태가 다를 경우 이런 식으로 **JSX자체를 children으로써 넘겨줄 수 있는 것이다.**

## 변화하는 props 값

**부모로부터 받는 props값이 변경되면 그 값은 컴포넌트에 반영된다**. 즉, 컴포넌트를 생성할 때 뿐만 아니라 생성된 후에 props값이 변경되더라도 생성된 컴포넌트에서 해당 값은 변경된 값으로 언제든 바뀔 수 있다. 하지만 이 개념은 props 자체가 변하는 것이 아닌, 해당 컴포넌트를 생성하는 부모가 props로써 다른 값을 넘겨주는 것이다. 이러한 기능을 사용하기 위해선 `state` 를 배워야한다. ( 추후에 배울 예정 )
