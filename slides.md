---
theme: default
class: "text-center"
highlighter: shiki
lineNumbers: true
info: |
  ## Next.js 13 seminar
drawings:
  persist: false
# use UnoCSS
# css: unocss
---

<br>
<br>
<br>
<br>

# NEXT.JS 13

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## Next.js 13 변경사항

1.  Brand new routing system, **/app** directory (Beta)
2.  Layouts
3.  React Server Components
4.  Data Fetching
5.  Turbopack
6.  next/image & @next/font & next/link ->improvements
7.  OG images with **@vercel/og**

<br>
<br>

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## New **app** Directory

- 새로운 app directory 는 아직 beta 버전이므로, production 단계에서의 사용은 권장 하지 않는다.
- 아직 pages directory 를 사용 할 수 있으며, 향상된 **next/image** , **next/link** 들 또한 pages directory 에서 사용 가능하다.

1. **Layouts**: 값비싼 리렌더를 방지하고, 스테이트를 보존하는 동시에 라우트 간의 UI를 쉽게 공유 할 수 있다.
2. **Server Components**: 대부분의 dynamic applications 를 위한 서버 우선을 기본값으로 한다.
3. **Streaming**: UI 의 유닛들이 렌더 됨과 동시에 순차적으로 렌더를 시켜주며, 로딩 스테이트도 보여준다.
4. **Data Fetching**: async Server Components 및 확장된 fetch API 는 component-level fetching 을 가능케 한다.

<br>
<br>

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## How to use app directory

1. 아래와 같이 next.config.js 설정을 해줘야 한다.

```js
// next.config.js
const nextConfig = {
  experimental: { appDir: true },
};
```

2. app directory 안에 page.tsx 파일을 아래와 같이 생성을 하면, app/layout.tsx 가 next 에 의해 자동생성 된다.

```js
// app/page.tsx
// This file maps to the index route (/)
export default function Page() {
  return <h2>Hello, I am Page!</h2>;
}
```

- pages/index.tsx 와 혼용해서 사용은 불가하다.

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## Layouts

- layout.tsx 는 다른 layout 이나 Page 를 Child 로 받을 수 있게 해줌으로써 여러 페이지에서 공유 되는 코드를 추출하기가 쉬워졌다.
- global.css 와 같은 전역 스타일은 app/layout.tsx 에서 import 해주면 된다.

```js
// app/layout.tsx
import "./global.css";
export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html>
      <head></head>
      <body>{children}</body>
    </html>
  );
}
```

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## Server Components

- Server Component 를 사용함으로써 클라이언트로 보내지는 JavaScript 의 양을 줄일 수 있다.
- App directory 안의 모든 컴포넌트는 기본적으로 Server Component 로 설정이 되어 있다.
- 이러한 이유로, app directory 를 사용하면 추가적인 작업 없이 퍼포먼스 증가의 이점을 얻을 수 있다.
- client-side interaction 이 필요한 Client Component를 사용 하고 싶다면, 컴포넌트 상단에 "use client" 를 추가하는 것으로 가능하다.

```js
"use client";
import { useState } from "react";
export default function Avatar() {
  const [person, setPerson] = useState(0);
  return (
  ...
  );
}
```

- useState 또는 useEffect 를 사용할때, 특정 브라우저 API 를 사용 할때, 특정 event listener 를 사용할 때 "use client" 를 사용해야한다.
- client 가 필요 없는 부분에선 server component 로 사용하며 client-side JavasScript 코드 양을 최소한으로 줄일 수 있다.

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## Streaming and Data Fetching

- Next 13 에선 React Suspense 를 통해 클라이언트에게 UI 의 일부를 손쉽게 stream 할 수 있다.
- 예를들어, data fetching 을 기다리는 동안 loading state 을 사용자에게 먼저 보여주고, data fetching 이 완료 될 시, 새로운 정보를 page 에 stream 하여 보여줄 수 있다.
- Data fetch 를 위한 API 가 간소화 되었으며, getServerSideProps, getStaticProps 및 getInitialProps는 app directory 에서 지원되지 않는다.
- 서버와 클라이언트 간의 이동을 최소화 하기 위한 Fetching 과 Rendering 이 같은 환경에서 이루어 지도록 Server Component 에서 data fetching 을 하는것이 바람직 하다.
- app directory 내의 어디에서든 data fetching 이 가능하지만, request 를 여러 컴포넌트에서 하는 한이 있더라도 해당 데이터를 직접적으로 사용하는 컴포넌트 내에서 data fetching 을 하는것이 좋다. 동일한 fetch 가 코드상으로 여러번 이루어진다고 해도, Next 는 자동으로 중복된 fetch 를 cache and dedupe 하여 동일한 data 가 두번 이상 fetch 되는 것을 방지한다.

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
/* h3 { text-transform: lowercase; } */
/* h3:first-letter { text-transform: uppercase; } */
</style>

---

## Streaming and Fetching in Server Components

- Next 13 의 새로운 좋은점은 loading.tsx 를 생성하는것 만으로 로딩 state 을 보여줄 수 있는 점이다.

```js
// app/avatar/loading.tsx
export default function Loading() {
  return <p>Loading Avatars...</p>;
}
```

- app/avatar/page.tsx 는 다음과 같이 작성 할 수 있다.

```js
async function getData() {
  // 기존의 API 호출 방식은 아래와 비슷 할 것이다.
  // const res = await fetch("https://api.github.com/");

  // 여기서 3초정도 기다려 준다.
  await new Promise((res) => setTimeout(res, 3000));

  // 기존의 data return 방식은 아래와 비슷 할 것이다.
  // return res.json();
  return "Avatar data";
}

export default async function Page() {
  const name = await getData();

  return <p>🤩 Hello there, {name}!</p>;
}
```

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## Client Component 에서의 Streaming & Fetching

- 이전 슬라이드에서의 동작을 client component 에서 적용을 하려면 **use** 훅을 사용해야 한다.

```js
"use client";
import { use } from "react";

async function getData() {
  await new Promise((res) => setTimeout(res, 3000));
  return "Avatar data in Client Components";
}

export default function Page() {
  const name = use(getData());
  return <p>🤩 Hello there, {name}!</p>;
}
```

- **use** 는 새로운 React 함수이며, Promise 를 accept 하며 await 이랑 개념적으로 비슷하다.

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## Caching with Fetch

- **fetch()** 함수는 원격의 리소스를 가져와 promise 를 리턴 하는 web API 이다.
- React 는 fetch 를 확장하여 자동으로 request dedupe(중복 리퀘스트 거르기) 을 하고, Next.js 는 fetch option object 를 확장하여 각 request 에 대한 caching 을 제공한다.

```js
// 수동으로 invalidate 될때까지 cache 가 된다.
//  `getStaticProps` 와 비슷하다.
// `force-cache` 가 기본설정이며, 간결성을 위해 생략 될 수 있다.
// Static data fetching 을 의미한다.
fetch(URL, { cache: "force-cache" });

// 매 request 마다 refetch 가 이루어진다.
// `getServerSideProps` 와 비슷하다.
// Dynamic data fetching 을 의미한다
fetch(URL, { cache: "no-store" });

// 이 요청은 10초의 캐시 유효기간을 가진다.
// 'revalidate'옵션을 가진  `getStaticProps` 와 비슷하다.
fetch(URL, { next: { revalidate: 10 } });
```

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## Data Fetching Patterns

- Parallel Data Fetching : client-server waterfalls 를 최소화 하기에 좋은 패턴

```js
import Albums from "./albums";
async function getArtist(username) {
  const res = await fetch(`https://api.github.com/artist/${username}`);
  return res.json();
}
async function getArtistAlbums(username) {
  const res = await fetch(`https://api.github.com/artist/${username}/type`);
  return res.json();
}
export default async function Page({ params: { username } }) {
  // 두개의 request 를 동시에 실행한다.
  const artistData = getAvatar(username);
  const albumData = getAvatarType(username);
  // promise 가 resolve 될때까지 기다린다.
  const [artist, album] = await Promise.all([artistData, albumData]);
  return (
    <div>
      <h1>{artist.name}</h1>
      <Albums list={albumData} />
    </div>
  );
}
```

<style>
h2 {
background-color: #2B90B6;
background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
background-size: 100%;
-webkit-background-clip: text;
-moz-background-clip: text;
-webkit-text-fill-color: transparent;
-moz-text-fill-color: transparent;
}
</style>

---

- Server Component 내에서 await 을 호출하기 이전에 fetch 를 시작함으로써 각 request 가 동시에 request 를 fetch 한다. 이를 통해 client-server waterfalls 를 최소화 할 수 있다.
- 하지만 사용자의 입장에서 모든 promises 가 resolve 될때 까지 render result를 볼수가 없으므로, React 의 Suspense 를 이용해 better user experience 를 도출 할 수 있다.

```js
// parallel 로 initiate 를 하지만,
const artistData = getArtist(username);
const albumData = getArtistAlbums(username);
// artistData 의 promise 가 먼저 resolve 되도록 기다린다.
const artist = await artistData;
return (
  <div>
    {/* 아티스트 이름 정보를 먼저 렌더 시키고, 앨범 정보는 Suspense 안에 wrap 하여 나중에 보여지도록 한다*/}
    <h1>{artist.name}</h1>
    <React.Suspense fallback={<h1>Loading...</h1>}>
      <Album promise={albumData} />
    </React.Suspense>
  </div>
);
// 그리고 Album 컴포넌트 내에서 albums 의 promise 가 resolve 될때까지 기다린다.
async function Album({ promise }) {
  const albums = await promise;
  return <p>{album.name}</p>;
}
```

---

## Sequential(순차적) Data Fetching

- data 를 순차적으로 fetch 하기 위해서 두가지 방법이 있다.

1. 해당 데이터를 필요로 하는 component 내에서 직접적으로 fetch 한다.
2. 해당 데이터를 필요로 하는 component 내에서 그 결과를 await 한다.

<style>
h2 {
background-color: #2B90B6;
background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
background-size: 100%;
-webkit-background-clip: text;
-moz-background-clip: text;
-webkit-text-fill-color: transparent;
-moz-text-fill-color: transparent;
}
</style>

---

## Turbopack (alpha)

- Turbopack 은 Rust 를 기반으로 개발 되었으며 Webpack 의 대체자로 서 등장 했다.
- 700배 이상 빠르다고 브랜딩이 되어 있지만 그짓말(?) 인걸로 밝혀졌다..
- "700배 이상" 이라는 결과를 도출 하기 위해 use cases 를 cherry-pick 했다는 의혹을 받고 있다.
- 하지만 더 빠른것은 사실이고, 아직 알파 단계의 개발이기 때문에 추후에 많은 개선 및 지원이 이루어 진 이후에는 정말로 webpack 을 대체 할 수 있을것이다.

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## next/image 개선사항

- Next 13의 새로운 Image Component 는 client-side 의 JavaScript 양이 줄었으며, 스타일을 입히고 configure 하기에 더욱 쉬워졌다.
- 이전의 next/image 가 next/legacy/image 로 바뀌었으며, 이전의 next/future/image 가 next/image 가 되었다.
- non-static 이미지에서 width, height 을 설정하지 않았었다면, 이제 이 부분은 필수 값이 되었다.

## @next/font

- 새로운 @next/font 는 브라우저 요청 없이 Google Fonts 를 사용 할 수 있게 해준다.
- css 와 폰트 파일들은 build 시에 다운로드 된다.
- yarn add @next/font 를 통해 설치가 가능하며, 아래와 같이 import 하여 사용할 수 있다.

```js
import { Montserrat } from "@next/font/google";
const montserrat = Montserrat();
<p> className={montserrat.className}> Hello World </p>;
```

- custom font 또한 아래와 같이 사용이 가능하다.

```js
import localFont from "@next/font/local";
const myFont = localFont({src: './my-font.woff2});
<p className={myFont.className}> Hello World </p>;
```

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## next/link

- next/link 는 이제 anchor tag 를 child 로서 요구 하지 않고, 자동으로 렌더 해준다.

```js
//아래와 같은 사용에서
<Link href="/about">
  <a>About</a>
</Link>

//이렇게 보다 간단하게 변경되었다.
<Link href="/about">
  About
</Link>
```

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## Open Graph Image Generation

- Static OG 는 변경, 유지, 관리가 어렵고 비쌌지만, 새로운 @vercel/og 라이브러리를 통해 dynamic 한 OG 를 손쉽게 생성 할 수 있다.

```js
// pages/api/og.jsx
import { ImageResponse } from "@vercel/og";

export const config = {
  runtime: "experimental-edge",
};

export default function () {
  return new ImageResponse(
    (
      <div
        style={{
          display: "flex",
          fontSize: 128,
          background: "white",
          width: "100%",
          height: "100%",
        }}
      >
        Hello, World!
      </div>
    )
  );
}
```

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## Middleware API Updates

- Vercel 의 middleware 란 사이트에서 요청이 처리되기 이전에 실행되는 코드를 말한다.
- middleware 를 통해 incoming request 를 수정하거나, 요청이 처리되기 전에 로직을 추가할 수도 있다.
- 이제는 middleware 의 header 를 세팅하는것이 더 쉬워졌으며, rewrite 나 redirect 없이 middleware 에서 직접 response 를 반환 할 수도 있다.
- next.config.js 안의 experimental.allowMiddlewareResponseBody 를 enable 해야 사용 가능.

  ```js
  // middleware.ts
  import { NextResponse } from "next/server";
  import type { NextRequest } from "next/server";

  export function middleware(request: NextRequest) {
    // request header 를 복사하여 새로운 header를 생성, 이 새로운 헤더는 `header-for-the-server` 서버로 보내진다.
    const requestHeaders = new Headers(request.headers);
    requestHeaders.set("header-for-the-server", "hello server");
    // NextResponse.rewrite 을 통해 request header 를 설정하는 것도 가능하다.
    const response = NextResponse.next({
      request: {
        // 새로운 request headers
        headers: requestHeaders,
      },
    });
    // 브라우저에서 inspect 가 가능한 새로운 response header 설정
    // 브라우저의 네트워크 탭에서 "header-for-the-client" 를 확인해보자.
    response.headers.set("header-for-the-client", "hello client");
    return response;
  }
  ```

<style>
h2 {
background-color: #2B90B6;
background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
background-size: 100%;
-webkit-background-clip: text;
-moz-background-clip: text;
-webkit-text-fill-color: transparent;
-moz-text-fill-color: transparent;
}
</style>

---

## Breaking Changes

- React 의 최소버전이 17.0.2 에서 18.2.0 으로 상향 되었다.
- Node.js 의 최소 버전이 12.22.0 에서 14.6.0 으로 상향 되었다.

<style>
h2 {
background-color: #2B90B6;
background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
background-size: 100%;
-webkit-background-clip: text;
-moz-background-clip: text;
-webkit-text-fill-color: transparent;
-moz-text-fill-color: transparent;
}
</style>
