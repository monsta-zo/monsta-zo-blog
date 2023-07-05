---

layout: single
title: "[Axios] Axios 인터셉터 "
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

Axios를 통해 요청을 보내기전에 해당 요청에 대해서 전처리를 수행하거나, 응답을 받기전에 응답을 전처리할 수 있다. 

즉, 요청 구성 객체(`config`)를 가로채서 수정한 후 다시 반환해주면 해당 요청 구성 객체로 요청을 보내는 것이다. 응답도 똑같다.

## 요청 인터셉터

요청을 보내기전에 가로챈다. 아래에서 볼 예시는 요청을 보내기전에 JWT 토큰을 헤더에 같이 포함시키는 코드이다.

```jsx
instance.interceptors.request.use((config) => {
  const token = localStorage.getItem("token");
  if (token) {
    config.headers["Authorization"] = `Bearer ${token}`;
  }
  return config;
});
```

요청을 보내기 전에 요청 객체 `config`를 받아와서 헤더에 사용자 인증 토큰을 넣은 후 다시 반환해서 요청을 보내고 있다.
