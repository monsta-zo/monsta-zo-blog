---
layout: single
title: "[React] 리스트와 키 "
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

우리는 반복되는 비슷한 컴포넌트가 있거나, 자바스크립트의 배열의 데이터들을 여러 컴포넌트로 만들고 싶을 때, `map()`이나 `filter()`를 사용할 수 있다. 

## 1. `map()` 이용하기

`map()`을 이용해서 배열에 들어있는 데이터들을 모양이 같은 여러 컴포넌트로 만들어보자.

```jsx
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];
```

1. `people`의 요소들을 JSX를 통해 `<li>` 요소로 변환한다.

```jsx
const listItems = people.map(person => <li>{person}</li>);
```

2. 만들어진 `listItems`를 렌더링한다.

```jsx
return <ul>{listItems}</ul>;
```

하지만 이렇게 만들고나면 아마 콘솔에 아래와 같은 에러가 나올 것이다.

```
Warning: Each child in a list should have a unique “key” prop.
```

이는 반복적으로 생성된 각 요소에 `key`가 필요하다는 것인데 뒤에서 다루자.

## 2. `filter()` 이용하기

`filter()`를 이용하면 배열에 있는 데이터들 중 조건에 만족하는 데이터들만 컴포넌트로 만들 수 있다. 

그리고 객체가 담겨있는 배열을 잘 활용하면 각 필드들의 값을 통해 더 구조화된 컴포넌트들을 만들 수 있다. 아래의 예시를 보자

```jsx
const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
}, {
  name: 'Percy Lavon Julian',
  profession: 'chemist',  
}, {
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
}];
```

이런 배열이 있다고 했을 때, 먼저 `profession`이 `chemist`인 데이터들만 골라보자

```jsx
const chemists = people.filter(person =>
  person.profession === 'chemist'
);
```

이렇게 필터링된 배열을 다시 `map()`을 이용해서 컴포넌트화 시킬 수 있다.

```jsx
const listItems = chemists.map(person =>
  <li>
     <img
       src={getImageUrl(person)}
       alt={person.name}
     />
     <p>
       <b>{person.name}:</b>
       {' ' + person.profession + ' '}
       known for {person.accomplishment}
     </p>
  </li>
);
```

하지만 이 또한 1번과 같은 에러가 나온다. 이제 3번에서 `key`에 대해서 알아보자

## 3. `key` 값 넣어주기

우리는 `map()`을 통해 배열을 렌더링할 때 각 요소의 고유값으로 `key`를 넣어줘야 한다. 

```jsx
export const people = [{
  id: 0, // Used in JSX as a key
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1, // Used in JSX as a key
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2, // Used in JSX as a key
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3, // Used in JSX as a key
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4, // Used in JSX as a key
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}</b>
          {' ' + person.profession + ' '}
          known for {person.accomplishment}
      </p>
    </li>
  );
  return <ul>{listItems}</ul>;
}
```

> **왜 `key` 값을 명시해줘야 할까?**
> 
> 리액트에서 배열에 어떤 값이 추가되거나 수정될 때 해당 배열을 통해 생성된 컴포넌트는 리렌더링되어야 한다. 이때, `key`값을 주지않으면 리액트는 배열 전체의 컴포넌트를 모두 리렌더링할 것이다. `key`값을 통해 어떤 요소가 변경되었는지 리액트에게 명시해줘야 해당 컴포넌트만 쉽게 리렌더링할 것이다.
> 
> 즉, 단순히 순서로 요소들을 구분하는 것이 아닌, `key`를 통해 구분할 수 있도록 하는 것이다.

> **`key` 값은 어떤걸 써야할까?**
> 
> 만약 DB로부터 데이터를 받아온다면 해당 데이터의 `key`나 `id`등을 쓸 수 있을 것이다. 
> 
> 만약 데이터가 DB로부터 오는 것이 아닌 로컬에서 직접 만들어진다면 `crypto.randomUUID()`나 `uuid` 같은 패키지를 이용해보자

> **컴포넌트를 추출할 때**
> 
> 구조가 복잡해서 컴포넌트를 추출할 때, 배열을 이용한다면 `key`는 컴포넌트 내부에서 주는 것이 아닌, 기존의 부모 컴포넌트에서 줘야한다. 
> 
> 즉, 배열의 각 아이템들 그 자체에 주어져야한다. 


