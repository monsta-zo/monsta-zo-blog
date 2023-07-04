---

layout: single
title: "[Redux] Redux의 기본 개념들 "
categories: Redux
tag: [Redux]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"
header:
 teaser: /assets/images/redux.png

---

## 1. 리덕스를 왜 사용할까?

우리가 컴포넌트 각각에서 상태를 관리한다면, 상위 컴포넌트나 하위 컴포넌트간의 상태 공유는 쉽지만, 멀리 떨어져있는 컴포넌트와의 상태 공유는 굉장히 복잡하고 어렵다.

리덕스는 이런 상태들을 전역으로 다루게 해준다. 즉, 어떤 컴포넌트든 어떤 상태라도 접근할 수 있게 해주는 것이다.

*리덕스 라이브러리와 툴*

- React-Redux : 리덕스를 리액트에서 쓸 수 있도록 해놓은 것

- Redux Toolkit : 리덕스를 편하게 사용할 수 있도록 해놓은 것

## 2. 리덕스 기초 개념 및 용어

리덕스에선 낯선 용어와 개념들을 많이 사용한다. 하지만 리덕스의 데이터 흐름에 대해서 이해할 수 있다면 이 용어들에 익숙해질 것이다.

*Actions*

우리가 발생시킬 수 있는 어떠한 이벤트이다. 객체로 표현되며, 우리는 이 액션에 따라서 상태를 어떻게 변화시킬지 결정하게 된다. 

예를 들어, 버튼이 2개 있다면 버튼 1을 누르는 액션, 버튼 2를 누르는 액션 등이 있을 것이다.

```js
{
  type: 'counter/incrementByAmount',
  payload: 3
}
```

`type`은 어떠한 액션인지 나타내는 문자열이다. 나중에 우린 여러 상태를 관리하게 될 것이고 해당 상태를 변화시키는 액션도 여러가지일 것이다. `todos/todoAdded` 에서 `todos`는 여러 상태 중 하나이고, `todoAdded`는 여러 액션 중 하나이다.

`payload`는 해당 액션을 발생시키며 함께 넘겨주는 데이터이다. 이 액션을 인지할 때, `payload`를 함께 받아서 상태 변화에 적용시킬 수 있다.

*Action Creators*

Action Creator가 Action을 자동으로 생성해준다. 사실 우린 Redux-Toolkit을 사용할 것이고 Action Creator도 자동으로 생성될 것이다.

*Reducers*

현재 `state`와 발생한 `action`을 기반으로 새로운 `state`를 만들어내는 함수이다. 즉, 상태를 변화시키는 것이다. 기존의 리액트와 마찬가지로 불변성(immutability)를 지켜야 한다. 

```js
export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0
  },
  reducers: {
    increment: state => {
      // Redux Toolkit allows us to write "mutating" logic in reducers. It
      // doesn't actually mutate the state because it uses the immer library,
      // which detects changes to a "draft state" and produces a brand new
      // immutable state based off those changes
      state.value += 1
    },
    decrement: state => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    }
  }
})
```

*Store*

상태가 저장되는 곳이다. 한 store안에 여러 종류의 상태를 보관할 수 있다. 

```js
export default configureStore({
  reducer: {
    counter: counterReducer
  }
})
```

*Slice*

store에 저장된 여러 종류의 상태 중 하나이다. 여러 slice로 나눠서 각 상태를 다르게 관리한다.

```jsx
export default configureStore({
  reducer: {
    users: usersReducer,
    posts: postsReducer,
    comments: commentsReducer
  }
})
```

*Dispatch*

특정 액션이 발생했을 때, store에게 알려주는 역할을 한다. 

그럼 store는 그 액션을 reducer에게 보내주고 reducer는 그에 맞게 상태를 변화시킨다.

```js
 <button
          className={styles.button}
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          +
        </button>
        <span className={styles.value}>{count}</span>
        <button
          className={styles.button}
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          -
        </button>
```

*Selectors*

store에서 state 값을 가져오는 역할을 한다. 이를 통해 어떤 컴포넌트에서든 모든 상태에 접근할 수 있다. 

```js
// slice에서 아래와 같이 상태를 내보냄
export const selectCount = state => state.counter.value

// 다른 파일에서 아래와 같이 사용 가능하다.
const count = useSelector(selectCount)
```

*데이터의 흐름*

데이터의 흐름을 간략하게 정리해보자면

1. 특정 액션이 발생하면 해당 액션을 dispatch가 인식해서 store에게 보낸다

2. store는 액션을 reducer에게 보낸다

3. reducer는 액션을 보고, 어떤 함수를 실행할지 정한다. 그리고 상태가 변화된다.

4. selector를 통해 해당 상태를 사용하는 부분은 리렌더링된다.
