---
layout: single
title: "[React] Render Props 패턴과 상태 끌어올리기 "
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

## Render Props 패턴

render props 패턴은 자식 컴포넌트에게 어떤 컴포넌트를 렌더링할지에 대한 함수를 넘겨주는 패턴이다. 이게 무슨 말이냐면, 컴포넌트의 재사용성을 높이기 위해서 컴포넌트 내부에서 다른 컴포넌트를 직접 렌더링하는 것이 아닌, 부모 컴포넌트에서 어떤 컴포넌트를 렌더링할지 넘겨주는 것이다. 

근데 그냥 컴포넌트를 넘겨주면 되지 왜 함수를 넘겨주는 것일까? 그 이유는 하위 컴포넌트의 상태를 사용하기 위해서이다. 이렇게 얘기하면 너무 감이 안오니 아래의 예를 보자.

```jsx
const Euro = ({ amount }) => <p>Euro: {amount * 0.86}</p>;

const Pound = ({ amount }) => <p>Pound: {amount * 0.76}</p>;

const App = () => {
  return (
    <>
      <Amount>
        {(amount) => (
          <div>
            <Euro amount={amount}></Euro>
            <Pound amount={amount}></Pound>
          </div>
        )}
      </Amount>
    </>
  );
};

export default App;
```

이런 `App`이라는 컴포넌트가 있을 때, `Amount`라는 컴포넌트를 렌더링하고, 또 `Amount`라는 컴포넌트 내부에서는 `Euro`와 `Pound`를 렌더링하고 싶을 때

```jsx
<Amount>
    <Pound amount={amount} />
    <Euro amount={amount} />
</Amount>
```

단순히 이렇게 컴포넌트를 `children`으로 넘겨주지 않고

```jsx
<Amount>
  {(amount) => (
    <div>
      <Euro amount={amount}></Euro>
      <Pound amount={amount}></Pound>
    </div>
   )}
</Amount>
```

렌더링 함수로 넘겨주는 것이다. 위에서 말했듯이 그 이유는 `Amount`라는 컴포넌트의 상태를 사용하기 위해서이다. 

```jsx
const Amount = ({ children }) => {
  const [amount, setAmount] = useState(0);

  const onIncrement = () => {
    setAmount((prev) => prev + 1);
  };

  const onDecrement = () => {
    setAmount((prev) => prev - 1);
  };
  return (
    <div>
      <span>US Dollar: {amount} </span>

      <button type="button" onClick={onIncrement}>
        +
      </button>
      <button type="button" onClick={onDecrement}>
        -
      </button>
      {children(amount)}
    </div>
  );
};
```

`Amount` 컴포넌트는 `amount`라는 내부 상태를 가지고 있는데 `children`으로 넘어온 컴포넌트에서 이 `amount`를 사용하기 위해서는 위와 같이 Props Render 패턴으로 넘겨줘야한다. 

```jsx
return (
  <div>
    <span>US Dollar: {this.state.amount} </span>

    <button type="button" onClick={this.onIncrement}>
      +
    </button>
    <button type="button" onClick={this.onDecrement}>
      -
    </button>

    {children}
  </div>
);
```

그냥 이렇게 `children`으로 받아버리면 `amount`를 `children` 컴포넌트 내부에서 쓸수가 없는 것이다. 

그렇다면 이 방식이 단순히 상태를 끌어올리는 방식과 뭐가 다를 까?

## 상태 끌어 올리기

아래는 `amount` 상태를 부모 컴포넌트인 `App`에 끌어 올린 것이다. 이 상태와 함수들을 props로 넘겨줌으로써 `Amount`와 `Euro`, `Pound` 모두에서 사용할 수 있도록 만들었다. 

```jsx
const App = () => {
  const [amount, setAmount] = useState(0);

  const onIncrement = () => {
    setAmount((prev) => prev + 1);
  };

  const onDecrement = () => {
    setAmount((prev) => prev - 1);
  };
  return (
    <>
      <Amount
        amount={amount}
        onIncrement={onIncrement}
        onDecrement={onDecrement}
      >
        <Euro amount={amount}></Euro>
        <Pound amount={amount}></Pound>
      </Amount>
    </>
  );
};
```

## Render Props와 상태 끌어 올리기

이제 두가지 방법을 한번 비교해보자. 

*코드의 직관성*

코드의 직관성은 상태 끌어 올리기 패턴이 더 직관적인 것 같다. 부모 컴포넌트에서 봤을 때, 아 `amount`가 무슨 상태이고 어떤식으로 상태를 변화시키는 지, 또 어떤 자식 컴포넌트들이 이를 사용하는지 바로 알 수 있다. 하지만, 넘겨야할 props들이 많아진다는 단점이 있다. 

반면, render props 패턴은 부모 컴포넌트에서 봤을 때, `amount`가 뭔지 바로 알기가 힘들다. `Amount` 컴포넌트에 들어가서 확인을 해봐야 비로서 알 수 있다.

*상태 관리*

상태 관리 측면에서는 render props가 우수하다고 볼 수 있다. 상태를 끌어올릴수록 넘겨야 할 props들이 많아지고 상태가 컴포넌트로부터 멀어짐으로써 유지보수가 힘들어질 수 있다. 

<br>

<br>

*결론*

Render Props 패턴은 컴포넌트 내부에서 **매번 다른 자식 컴포넌트를 렌더링**하고 싶고 **그 자식 컴포넌트에서 데이터가 필요**할 때 주로 사용한다. 굳이 자식 컴포넌트에서 부모 컴포넌트의 데이터가 필요없다면 그냥 컴포넌트만 넘겨줄 수 있다. 하지만 데이터를 같이 넘겨주기위해 함수를 넘겨주는 것이다!
