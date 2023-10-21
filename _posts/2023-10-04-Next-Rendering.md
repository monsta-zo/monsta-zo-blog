---
layout: single
title: "Next의 기본 렌더링 방식은 대체 뭐야 SSR? CSR? SSG? "
categories: Next
tag: [Next]
toc: true
toc_sticky: true
author_profile: false
sidebar:
 nav: "docs"
header:
 teaser: /assets/images/next.png

---

Next.js를 공부하다보면, 또 프로젝트를 개발하다보면 가끔 헷갈리는 개념이 있다.

바로  **렌더링 방식**이다. 

Next.js는 다양한 렌더링 방식들을 지원하고 각 페이지별로 입맛대로 골라 사용할 수 있다. 

근데 여러 렌더링 방식들 중 아무것도 사용하지 않으면, 대체 Next는 어떤 방식으로 렌더링을 할까? 

그리고 우리가 아는 **SSR**, **CSR** 방식이 실제로 Next에서는 어떻게 적용이 될까? 한번 알아보자!

<br>

# CSR과 SSR 개념

먼저 Next의 관점이 아닌 통상적인 관점에서 SSR과 CSR에 대해서 알아보자.

## CSR (Client Side Rendering)

**클라이언트 측에서 렌더링한다.** 이게 과연 어떤 의미일까? 

우리는 리액트에서나, Next에서나 각 라우트 주소에 맞는 페이지를 만들게 되는데 해당하는 페이지에는 각종 컴포넌트들, 이벤트 핸들러, API를 통한 데이터 요청 등의 JS 코드들이 포함될 것이다. 

하지만 이러한 JS 코드들만으로는 웹 페이지를 만들 수 없다. 우리는 **<u>HTML이라는 구조가 있어야하고 그 구조에 JS를 입혀줘야지만</u>** 하나의 완성된 웹 페이지를 만들 수 있다.

HTML에 JS를 어디서 입혀주냐? 가 바로 CSR이냐 SSR이냐를 결정한다.

즉, CSR은 HTML에 JS를 "클라이언트"에서 입혀주는 것이다.

<br>

우리가 프로젝트를 빌드하면 서버에 내가 작성한 여러 파일들이 저장된다. ~~(물론 하나의 파일로 번들링되긴하지만)~~ 따라서 우리는 각 페이지에서 필요한 HTML과 JS를 모두 서버에 보관하게 된다.

여기서 잠깐!

> 여기서 얘기하는 서버는 데이터를 요청하기 위해 보내는 백엔드의 API 서버가 아닌, 프론트의 프로젝트를 배포하는 프론트의 서버를 얘기하는 것이다! 헷갈리지 말자

<br>

특정 페이지를 보고 싶을 때, 서버로부터 해당 페이지의 HTML과 JS 들을 받아와야하는데 **HTML과 JS를 둘다 받아와서 클라이언트에서 HTML에 JS를 입히는 방식**이 바로 CSR이다. 

말 그대로, 클라이언트 측에서 HTML에  JS를 입혀서 렌더링하는 것이다.

![loading-ag-962](../../images/2023-10-04-Next-multilayouts/193c7292e00ccd919bb093efc87a2c85dc109a16.png)

## SSR (Server Side Rendering)

**서버 측에서 렌더링한다.**

CSR의 반대이다. 즉, **HTML에 JS를 서버에서 입혀주고 클라이언트로 보내주는 것**이다. 여기서 얘기하는 서버도 절대 백엔드의 서버가 아닌 Next서버 즉, 프론트의 서버이다!

말 그대로, 서버에서 미리 HTML에 JS를 렌더링한 후 브라우저로 보내주는 방식이다.

![loading-ag-966](../../images/2023-10-04-Next-multilayouts/407a750951183bdc5441d64e1ea0fb8e00b74236.png)

# Next.js만의 렌더링

위에서 CSR과 SSR에 대해서 알아보았다. 하지만 Next.js에서 제공하는 CSR과 SSR은 위에서 알아본 내용들과 개념은 같지만, 방식은 사알짝 다르다. 실제로 Next.js에서는 어떤 방식으로 렌더링하는지 알아보자. 

<br>

그 전에 Next에서의 또 다른 렌더링 방식인 **SSG**와 아주아주 중요한 개념인 **Pre-rendering**에 대해서 먼저 알아 보자.

## SSG (Static Site Generation)

SSG는 SSR과 좀 비슷해보이지만 다른 방식이다.

프로젝트를 빌드 할 때, **각 페이지마다 HTML에 JS를 미리 입혀놓고, CDN에 저장해놓는 방식**이다. 그 후, 각 페이지를 렌더링해야할 때 CDN에 저장된 해당 페이지를 그대로 가져와서 사용하는 것이다. 

SSR과 뭐가 다를까? SSR은 각 페이지가 필요할 때가 되서야 서버에서 HTML에 JS를 입혀서 가져오는 것이고, SSG는 미리 다 입혀 놓고 필요할 때 그대로 가져오는 것이다.

<br>

가장 중요한 것은 클라이언트에 가져와서 입히는 것도 아닌, 서버에서 입히는 것도 아닌, <u>빌드 시에 미리 입혀놓고 저장한다는 것</u>이다.

## Pre-rendering

앞서 알아본, SSR과 SSG 방식을 Next에서는 Pre-rendering 이라고 한다. 즉, 서버에서 미리 렌더링해서 클라이언트로 보내준다는 것이다. 

![loading-ag-1011](../../images/2023-10-04-Next-multilayouts/c4c03413678c328279bd23294e69e6fcc6f33d8a.png)

그럼 이때까지 배운 CSR, SSR 그리고 SSG는 Next.js에서 어떻게 동작할까? 과연 Next.js의 기본 렌더링 방식은 CSR일까? SSR일까? SSG일까?

# Next.js의 진실과 기본 렌더링 방식

Next.js에서 SSR그리고 CSR을 사용하기 위해서는 각 렌더링 방식에 맞는 함수를 사용해야한다. 

- SSR -> `getServerSideProps()`

- SSG -> `getStaticProps()` 

그럼 이 두 함수를 둘다 사용하지 않으면 당연히 CSR 방식으로 렌더링을 해야할 것이다. CSR 방식은 클라이언트 측에서 JS를 HTML에 입히기 때문에 브라우저에서 JS를 사용하지 못하게 하면 빈 화면만 나올 것이다. 

![loading-ag-1100](../../images/2023-10-04-Next-multilayouts/2023-10-10-22-50-02-image.png)

실제로 현재 개발중인 프로젝트의 회원가입 페이지이며, `getServerSideProps()` , `getStaticProps()` 둘 다 사용하고 있지 않다. 따라서 CSR 방식일 것이다. 그럼 브라우저에서 JS 기능을 꺼보자.

![loading-ag-1108](../../images/2023-10-04-Next-multilayouts/2023-10-10-22-51-32-image.png)

어? 분명 CSR 방식일테고, 따라서 JS를 사용하지 못하면 컴포넌트들을 렌더링하지 못할텐데 잘 렌더링하고 있다. 즉, Next의 기본 렌더링 방식은 CSR이 아닌건가?

<br>

그렇다. Next의 **기본 렌더링 방식은 바로 SSG**이다.

<br>

미리 모든 페이지의 내용들을 다 렌더링해서 서버에 저장해놓고, 필요할 때 받아와서 사용하는 방식인 것이다. <u>단, 데이터 패칭을 하지 않는 선에서만 미리 SSG 방식으로 렌더링해서 저장해놓는다. </u> 

> By default, Next.js pre-renders pages using Static Generation without fetching data. Here's an example:
> 
> ```tsx
> function About() {
>   return <div>About</div>
> } 
> 
> export default About
> ```
> 
> Note that this page does not need to fetch any external data to be pre-rendered. In cases like this, Next.js generates a single HTML file per page during build time.

위의 Next 공식문서에서 볼 수 있듯이, Next.js는 데이터 패칭이 필요하지 않으면 기본적으로 SSG 방식으로 렌더링한다고 되어있다.

<br>

아니 그럼 SSR은 왜 필요하고, CSR은 왜 필요해?? 굳이 SSG는 왜 나눠놓은거야?

그 이유는, **"데이터 패칭을 언제 어떻게 해올 것인지"** 와 **"어느 데이터까지 Pre-rendering을 할 것인지"** 와 깊은 관련이 있다. 그럼 각 렌더링 방식에 대해서 각각 Next.js에서는 어떻게 동작하는 지 알아보자.

<br>

**반드시 기억하자, Next는 데이터 패칭이 필요없는 선까지는 SSG를 사용한다**

## Next의 CSR

> In Next.js, there are two ways you can implement client-side rendering:
> 
> 1. Using React's `useEffect()` hook inside your pages instead of the server-side rendering methods ([`getStaticProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props) and [`getServerSideProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)).
> 2. Using a data fetching library like [SWR](https://swr.vercel.app/) or [TanStack Query](https://tanstack.com/query/latest/) to fetch data on the client (recommended).

Next의 공식문서에서는 CSR을 사용하기 위한 두가지 방법을 소개한다. 이 중 2번째 방식의 TanStack Query를 사용해서 Next의 CSR은 어떤 방식으로 동작하는 지 알아보자.

```tsx
const Test = () => {
  const [csrData, setCsrData] = useState(' ');
  const { data } = useQuery({ queryKey: ['getCSR'], queryFn: getCSR });

  useEffect(() => {
    setCsrData(data?.result);
  }, [data]);
  return (
    <>
      <p>This is Test</p>
      <p>{csrData}</p>
    </>
  );
};

export default Test;
```

`useQuery` 를 통해 간단한 문자열 데이터를 받아와서 CSR 방식으로, 렌더링하였다. 

![loading-ag-1214](../../images/2023-10-04-Next-multilayouts/2023-10-11-00-42-01-image.png) 

브라우저에 출력되는 결과이다. 그럼 자바스크립트를 끈 상태로 결과를 확인해보자.

![loading-ag-1222](../../images/2023-10-04-Next-multilayouts/2023-10-11-00-43-13-image.png)

크롬의 개발자 도구에서 자바스크립트를 끈 상태로 확인한 결과이다. 뭔가 이상하다.

분명 CSR 방식은 클라이언트에서 HTML에 JS를 입히는 방식이다. 즉, 브라우저에서 JS를 사용할 수 없다면 아무 화면도 볼 수 없어야 한다. 하지만, TanStack Query를 통해 가져온 데이터인 "CSR 데이터 받아오기"는 안보이지만, "This is Test"는 정상적으로 출력된다. 

이게 당연한걸까?

리액트는 기본적으로 CSR을 사용한다. 그럼 리액트 프로젝트에서 똑같은 코드를 자바스크립트 없이 실행해보자.

![loading-ag-1244](../../images/2023-10-04-Next-multilayouts/2023-10-11-00-51-34-image.png)

자바스크립트 없이는 사용할 수 없다는 경고문이 나온다. 이 부분은 리액트 프로젝트의 `index.html` 에 있다.

```html
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
```

그럼 이 부분을 주석처리하고 한번 출력해보자.

![loading-ag-1259](../../images/2023-10-04-Next-multilayouts/2023-10-11-00-52-49-image.png)

아무것도 없다. 즉, 정상적인 CSR이라면 자바스크립트를 사용하지 못한다면 아무것도 렌더링하지 못하는 것이 정상이다. 하지만, Next에서는 분명 CSR 방식이지만 특정 부분은 렌더링하고 있다.

**이것이 통상적인 CSR과 Next의 CSR을 나눠서 소개한 이유이다. Next에서 CSR 방식을 사용한다는 것은 조금 다른 개념이기 때문이다.** 

<br>

앞서 설명했듯이, Next는 "데이터 패칭이 필요없는 선까지"는 SSG를 사용한다. 앞서 사용한 코드에서 

![loading-ag-1278](../../images/2023-10-04-Next-multilayouts/c32b7d279da524922d9dc50b1eed4c0b9160e99f.png)

- 데이터 패칭이 필요한 빨간 네모 박스 -> CSR 사용

- 데이터 패칭이 필요없는 파란 네모 박스 -> SSG 사용 

즉, 위의 코드가 렌더링 되는 순서를 보면

1. 파란 네모 박스의 코드는 SSG를 통해 미리 페이지를 만들어 서버에 저장

2. 해당 페이지를 보여달라는 요청이 들어오면 클라이언트로 전달

3. 그 후, 빨간 네모 박스의 코드를 실행해 CSR로 데이터 받아옴

<br>

정리하자면, **Next에서 CSR을 사용한다는 것은 데이터 패칭을 CSR로 한다는 것이고 나머지 부분은 SSG로 미리 만들어서 서버에 저장한다는 것** 이다. 

이러한 과정은 다른 렌더링 방식에서도 똑같다. 

## Next의 SSR

> To use Server-side Rendering for a page, you need to `export` an `async` function called `getServerSideProps`. This function will be called by the server on every request.

Next에서는 `getServerSideProps` 를 통해 SSR을 할 수 있다. 아래는 SSR을 사용하는 예시 코드이다.

![loading-ag-1374](../../images/2023-10-04-Next-multilayouts/415d33ffe2853d8575ea21211ebdbf9d820dac6c.png)

SSR 방식에서는 **"데이터 패칭을 언제 해올 것인지"** 와 **"어디까지 pre-rendering 할 것인지"** 둘의 개념이 모두 포함된다. 일단 CSR과 똑같이

- 데이터 패칭이 필요한 빨간 네모 박스 -> SSR 사용

- 데이터 패칭이 필요없는 파란 네모 박스 -> SSG 사용

즉, 위의 코드가 렌더링 되는 순서를 보면

1. 파란 네모 박스는 SSG를 통해 미리 페이지를 만들어 서버에 저장

2. 해당 페이지를 보여달라는 요청이 들어오면 서버에서 만들어 놓은 페이지 + `getServerSideProps` 에서 보내준 데이터들을 미리 렌더링

3. 그 후, 브라우저로 보내서 해당 페이지 출력 

<br>

정리하자면, **Next에서 SSR을 사용한다는 것은 출력할 데이터 패칭을 SSR로 한다는 것이고, 나머지 부분은 SSG로 미리 만들어서 서버에 저장한다는 것** 이다.

## Next의 SSG

> Some pages require fetching external data for pre-rendering. There are two scenarios, and one or both might apply. In each case, you can use these functions that Next.js provides:
> 
> 1. Your page **content** depends on external data: Use `getStaticProps`.
> 2. Your page **paths** depend on external data: Use `getStaticPaths` (usually in addition to `getStaticProps`).

Next에서는 SSG를 위해서 두 가지 방식을 사용할 수 있는데 그 중 `getStaticProps`를 통해 알아보자. 

SSG 방식은 이전보다 간단하다. 왜냐면 데이터 패칭을 해오는 부분이든 안 해오는 부분이든 모두 SSG를 통해 렌더링 해놓고 서버에 저장해놓으면 되기 때문이다.

![loading-ag-1456](../../images/2023-10-04-Next-multilayouts/ab57d5b8d44f8668e16eefb348bd31edc4cbfd06.png)

즉, 모든 부분에서 SSG를 사용한다.

# 마무리

**Next의 렌더링 방식과 우리가 알던 렌더링 방식을 혼돈하지 말자.**

Next에서는 최대한 많은 부분은 SSG 방식으로 미리 서버에 페이지를 만들어 놓는다. 그리고 SSR, CSR, SSG 각 방식에 맞춰서 동적으로 데이터를 받아오고, 완성된 페이지를 렌더링 해준다. 

그리고, 각 페이지의 특성에 맞춰서 여러 렌더링 방식들을 적절히 활용해보자.



[here](www.naver.com)
