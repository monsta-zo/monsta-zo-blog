---
layout: single
title: "[Axios] Axios의 기본 개념들 "
categories: Axios
tag: [Axios]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"
header:
 teaser: /assets/images/axios.png

---

## 1. Axios란?

Axios란 HTTP 클라이언트다. 서버에 요청해서 데이터를 주고 받을 수 있게 해주는 라이브러리이다.

## 2. 리액트에서 Axios 사용해보기

### GET 요청

데이터를 가져오거나 검색하기 위해서는 GET 요청을 사용할 수 있다.

```jsx
const baseURL = "https://jsonplaceholder.typicode.com/posts/1";

export default function App() {
  const [post, setPost] = React.useState(null);

  React.useEffect(() => {
    axios.get(baseURL).then((response) => {
      setPost(response.data);
    });
  }, []);

  if (!post) return null;

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </div>
  );
}
```

`useEffect`를 통해 컴포넌트가 마운트될 때 GET 요청이 실행된다. 

`.get()` 메소드를 통해 요청을 보내고, `.then()`을 통해 응답을 받아온다. 

`response.data` 를 통해 응답의 데이터에 접근해서 상태를 업데이트한다.

### POST 요청

새로운 데이터를 만들기 위해 POST 요청을 사용한다.

```jsx
const baseURL = "https://jsonplaceholder.typicode.com/posts";

export default function App() {
  const [post, setPost] = React.useState(null);

  React.useEffect(() => {
    axios.get(`${baseURL}/1`).then((response) => {
      setPost(response.data);
    });
  }, []);

  function createPost() {
    axios
      .post(baseURL, {
        title: "Hello World!",
        body: "This is a new post."
      })
      .then((response) => {
        setPost(response.data);
      });
  }

  if (!post) return "No post!"

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
      <button onClick={createPost}>Create Post</button>
    </div>
  );
}
```

`.post()` 함수를 통해 POST 요청을 한다. 두번째 인자로 어떤 데이터를 생성할건지 넘겨줄 수 있고 똑같이 `.then()`을 통해 응답을 받는다. 

### PUT 요청

기존의 데이터를 업데이트하기 위해서 PUT 요청을 사용한다.

```jsx
const baseURL = "https://jsonplaceholder.typicode.com/posts";

export default function App() {
  const [post, setPost] = React.useState(null);

  React.useEffect(() => {
    axios.get(`${baseURL}/1`).then((response) => {
      setPost(response.data);
    });
  }, []);

  function updatePost() {
    axios
      .put(`${baseURL}/1`, {
        title: "Hello World!",
        body: "This is an updated post."
      })
      .then((response) => {
        setPost(response.data);
      });
  }

  if (!post) return "No post!"

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
      <button onClick={updatePost}>Update Post</button>
    </div>
  );
}
```

POST 요청과 사용하는 방법이 똑같다.

### DELETE 요청

데이터를 삭제하기 위해서 DELETE 요청을 사용한다.

```jsx
const baseURL = "https://jsonplaceholder.typicode.com/posts";

export default function App() {
  const [post, setPost] = React.useState(null);

  React.useEffect(() => {
    axios.get(`${baseURL}/1`).then((response) => {
      setPost(response.data);
    });
  }, []);

  function deletePost() {
    axios
      .delete(`${baseURL}/1`)
      .then(() => {
        alert("Post deleted!");
        setPost(null)
      });
  }

  if (!post) return "No post!"

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
      <button onClick={deletePost}>Delete Post</button>
    </div>
  );
}
```

`.delete()` 함수의 응답은 대부분 필요없지만, 정상적으로 삭제되었는지 확인하기 위해 `.then()` 메소드를 사용한다.

## 3. 에러 핸들링

아래와 같이 간단하게 `.catch()` 메소드를 통해 에러 핸들링을 할 수 있다.

```jsx
const baseURL = "https://jsonplaceholder.typicode.com/posts";

export default function App() {
  const [post, setPost] = React.useState(null);
  const [error, setError] = React.useState(null);

  React.useEffect(() => {
    // invalid url will trigger an 404 error
    axios.get(`${baseURL}/asdf`).then((response) => {
      setPost(response.data);
    }).catch(error => {
      setError(error);
    });
  }, []);

  if (error) return `Error: ${error.message}`;
  if (!post) return "No post!"

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </div>
  );
}
```

## 4. Axios 인스턴스 사용하기

유사한 요청을 보낼때마다 `baseURL`을 입력해서 요청을 만드려면 귀찮을 것이다. 그럴 때 인스턴스에 `baseURL` 이나 헤더 등을 지정해놓고 계속 불러서 사용할 수 있다.

```jsx
const client = axios.create({
  baseURL: "https://jsonplaceholder.typicode.com/posts" 
});

export default function App() {
  const [post, setPost] = React.useState(null);

  React.useEffect(() => {
    client.get("/1").then((response) => {
      setPost(response.data);
    });
  }, []);

  function deletePost() {
    client
      .delete("/1")
      .then(() => {
        alert("Post deleted!");
        setPost(null)
      });
  }

  if (!post) return "No post!"

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
      <button onClick={deletePost}>Delete Post</button>
    </div>
  );
}
```

인스턴스를 미리 만들어 놓고 필요할 때 뒤에 요청 메소드를 붙여서 사용하면 된다. 추가적인 route가 필요하다면, 메소드의 인수로 넘겨서 사용할 수 있다.

## Async-Await와 Axios 함께 사용하기

```jsx
const client = axios.create({
  baseURL: "https://jsonplaceholder.typicode.com/posts" 
});

export default function App() {
  const [post, setPost] = React.useState(null);

  React.useEffect(() => {
    async function getPost() {
      const response = await client.get("/1");
      setPost(response.data);
    }
    getPost();
  }, []);

  async function deletePost() {
    await client.delete("/1");
    alert("Post deleted!");
    setPost(null);
  }

  if (!post) return "No post!"

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
      <button onClick={deletePost}>Delete Post</button>
    </div>
  );
}
```

`.then()` 대신 `async` 로 함수를 만들고 비동기처리할 코드에 `await`을 붙여주면 된다. 그 반환값으로는 응답이 나온다.

## `useAxios` 훅 사용하기

일단 라이브러리를 설치한다.

```shell
npm install use-axios-client
```

사용법은 아래와 같다. `data`, `erroe`, `loading`을 반환하며 상황에 맞게 사용할 수 있다.

```jsx
import { useAxios } from "use-axios-client";

export default function App() {
  const { data, error, loading } = useAxios({
    url: "https://jsonplaceholder.typicode.com/posts/1"
  });

  if (loading || !data) return "Loading...";
  if (error) return "Error!";

  return (
    <div>
      <h1>{data.title}</h1>
      <p>{data.body}</p>
    </div>
  ) 
}
```
