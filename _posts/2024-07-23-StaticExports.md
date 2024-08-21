---
title: "Nextjs 14에서 정적 내보내기Static Exports 설정 및 활용 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:54
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Static Exports"
link: "https://nextjs.org/docs/app/building-your-application/deploying/static-exports"
isUpdated: true
---




# 정적 익스포트

Next.js는 정적 사이트나 Single-Page Application (SPA)로 시작한 후 나중에 선택적으로 서버가 필요한 기능을 사용하도록 업그레이드할 수 있습니다.

next build를 실행할 때 Next.js는 각 route마다 HTML 파일을 생성합니다. 엄격한 SPA를 개별 HTML 파일로 분리함으로써 Next.js는 클라이언트 측에서 불필요한 JavaScript 코드를 로드하지 않고 번들 크기를 줄이고 페이지 로딩을 빠르게 할 수 있습니다.

Next.js가 이러한 정적 익스포트를 지원하기 때문에 HTML/CSS/JS 정적 자산을 제공할 수 있는 모든 웹 서버에 배포하고 호스트할 수 있습니다.

<div class="content-ad"></div>

## 구성

정적 익스포트를 활성화하려면 next.config.js 내부의 출력 모드를 변경하세요:

```js
/**
 * @type {import('next').NextConfig}
 */
const nextConfig = {
 
  // 선택 사항: 링크를 변경하여 `/me` -> `/me/`, `/me.html` -> `/me/index.html` 으로 발행
  // trailingSlash: true,
 
  // 선택 사항: 자동으로 `/me` -> `/me/`로 변경하지 않고 `href` 를 유지
  // skipTrailingSlashRedirect: true,
 
  // 선택 사항: 출력 디렉토리를 `out` -> `dist`로 변경
  // distDir: 'dist',
}
 
module.exports = nextConfig
```

next build를 실행한 후, Next.js는 응용 프로그램을 위한 HTML/CSS/JS 자산들을 포함하는 out 폴더를 생성합니다.

<div class="content-ad"></div>

## 지원되는 기능

Next.js의 핵심은 정적 익스포트를 지원하도록 설계되었습니다.

### 서버 컴포넌트

정적 익스포트를 생성하기 위해 next build를 실행할 때, 앱 디렉토리 내에서 사용된 서버 컴포넌트는 빌드 중에 실행됩니다. 이는 기존 정적 사이트 생성과 유사합니다.

<div class="content-ad"></div>

결과 컴포넌트는 초기 페이지 로드를 위해 정적 HTML로 렌더링되며 클라이언트가 경로를 이동할 때 정적 페이로드로 렌더링됩니다. 정적 익스포트를 사용할 때는 서버 컴포넌트에 변경이 필요하지 않습니다. 단, 동적 서버 함수를 사용할 때는 예외입니다.

```js
export default async function Page() {
  // 이 fetch는 `next build` 중 서버에서 실행됩니다.
  const res = await fetch('https://api.example.com/...')
  const data = await res.json()
 
  return <main>...</main>
}
```

### 클라이언트 컴포넌트

클라이언트에서 데이터 가져오기를 수행하려면 SWR을 사용하여 요청을 메모이즈하는 클라이언트 컴포넌트를 사용할 수 있습니다.

<div class="content-ad"></div>

```js
'use client'
 
import useSWR from 'swr'
 
const fetcher = (url: string) => fetch(url).then((r) => r.json())
 
export default function Page() {
  const { data, error } = useSWR(
    `https://jsonplaceholder.typicode.com/posts/1`,
    fetcher
  )
  if (error) return '로드 실패'
  if (!data) return '로딩 중...'
 
  return data.title
}
```

라우트 전환은 클라이언트 측에서 발생하기 때문에 이는 전통적인 SPA처럼 작동합니다. 예를 들어, 다음 인덱스 루트는 클라이언트에서 다른 게시물로 이동할 수 있게 합니다:

```js
import Link from 'next/link'
 
export default function Page() {
  return (
    <>
      <h1>인덱스 페이지</h1>
      <hr />
      <ul>
        <li>
          <Link href="/post/1">게시물 1</Link>
        </li>
        <li>
          <Link href="/post/2">게시물 2</Link>
        </li>
      </ul>
    </>
  )
}
```

### 이미지 최적화

<div class="content-ad"></div>

next/image를 통한 이미지 최적화는 next.config.js에서 사용자 정의 이미지 로더를 정의해 정적 내보내기와 함께 사용할 수 있습니다. 예를 들어, Cloudinary와 같은 서비스로 이미지를 최적화할 수 있습니다:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'export',
  images: {
    loader: 'custom',
    loaderFile: './my-loader.ts',
  },
}
 
module.exports = nextConfig
```

이 사용자 정의 로더는 원격 소스로부터 이미지를 가져오는 방법을 정의할 것입니다. 예를 들어, 아래 로더는 Cloudinary를 위한 URL을 구성할 것입니다:

```js
export default function cloudinaryLoader({
  src,
  width,
  quality,
}: {
  src: string
  width: number
  quality?: number
}) {
  const params = ['f_auto', 'c_limit', `w_${width}`, `q_${quality || 'auto'}`]
  return `https://res.cloudinary.com/demo/image/upload/${params.join(
    ','
  )}${src}`
}
```

<div class="content-ad"></div>

클라우드니어리에 이미지의 상대 경로를 정의하여 애플리케이션에서 next/image를 사용할 수 있습니다:

```js
import Image from 'next/image'
 
export default function Page() {
  return <Image alt="turtles" src="/turtles.jpg" width={300} height={300} />
}
```

### 라우트 핸들러

라우트 핸들러는 next build를 실행할 때 정적 응답을 렌더링합니다. GET HTTP 동사만 지원됩니다. 캐시된 또는 캐시되지 않은 데이터에서 정적 HTML, JSON, TXT 또는 다른 파일을 생성하는 데 사용할 수 있습니다. 예를 들어:

<div class="content-ad"></div>

```js
export async function GET() {
  return Response.json({ name: 'Lee' })
}
```

위의 파일 app/data.json/route.ts는 다음 빌드 중에 정적 파일로 렌더링되어 ' name: `Lee` '를 포함하는 data.json 파일을 생성할 것입니다.

들어오는 요청에서 동적 값을 읽어야 하는 경우 정적 익스포트를 사용할 수 없습니다.

### 브라우저 API들


<div class="content-ad"></div>

클라이언트 컴포넌트는 다음 빌드 중에 HTML로 사전 렌더링됩니다. window, localStorage 및 navigator와 같은 Web API는 서버에서 사용할 수 없으므로 브라우저에서 실행될 때에만 안전하게 이 API에 액세스해야 합니다. 예를 들어:

```js
'use client';

import { useEffect } from 'react';

export default function ClientComponent() {
  useEffect(() => {
    // `window`에 접근할 수 있게 되었습니다.
    console.log(window.innerHeight);
  }, [])

  return ...;
}
```

## 지원되지 않는 기능

Node.js 서버가 필요한 기능이나 빌드 프로세스 중에 계산할 수 없는 동적 로직과 같은 기능은 지원되지 않습니다:

<div class="content-ad"></div>

- Dynamic Routes with dynamicParams: true
- Dynamic Routes without generateStaticParams()
- Route Handlers that rely on Request
- Cookies
- Rewrites
- Redirects
- Headers
- Middleware
- Incremental Static Regeneration
- Image Optimization with the default loader
- Draft Mode

위의 기능 중 하나를 next dev와 함께 사용하려고 시도하면, 루트 레이아웃에서 dynamic 옵션을 error로 설정하는 것과 유사한 오류가 발생합니다.

```js
export const dynamic = 'error'
```

## 배포하기

<div class="content-ad"></div>

정적 익스포트로 Next.js를 사용하면 HTML/CSS/JS 정적 에셋을 제공할 수 있는 모든 웹 서버에 배포하고 호스팅할 수 있습니다.

next build를 실행하면 Next.js는 정적 익스포트를 out 폴더에 생성합니다. 예를 들어 다음과 같은 경로가 있다고 가정해보겠습니다:

- /
- /blog/[id]

next build를 실행한 후에 Next.js는 다음 파일을 생성할 것입니다:

<div class="content-ad"></div>

- /out/index.html
- /out/404.html
- /out/blog/post-1.html
- /out/blog/post-2.html

만약 Nginx와 같은 정적 호스트를 사용 중이라면, 들어오는 요청을 올바른 파일로 리다이렉션할 수 있습니다:

```js
server {
  listen 80;
  server_name acme.com;
 
  root /var/www/out;
 
  location / {
      try_files $uri $uri.html $uri/ =404;
  }
 
  # `trailingSlash: false`를 사용할 때 이 부분은 필수입니다.
  # `trailingSlash: true`를 사용한다면 이 부분을 생략할 수 있습니다.
  location /blog/ {
      rewrite ^/blog/(.*)$ /blog/$1.html break;
  }
 
  error_page 404 /404.html;
  location = /404.html {
      internal;
  }
}
```

## 버전 히스토리

<div class="content-ad"></div>


| Version   | Changes                                                                                               |
|-----------|-------------------------------------------------------------------------------------------------------|
| `v14.0.0` | `next export` has been removed in favor of `"output": "export"`                                        |
| `v13.4.0` | App Router (Stable) adds enhanced static export support, including using React Server Components and Route Handlers |
| `v13.3.0` | `next export` is deprecated and replaced with `"output": "export"`                                      |
