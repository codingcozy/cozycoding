---
title: "Nextjs 14 미들웨어 App Router에서 사용하는 방법과 팁"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:14
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Middleware"
link: "https://nextjs.org/docs/app/building-your-application/routing/middleware"
---


# 미들웨어

미들웨어를 사용하면 요청이 완료되기 전에 코드를 실행할 수 있습니다. 그리고 들어오는 요청에 따라 응답을 재작성하거나 리다이렉팅, 요청이나 응답 헤더를 수정하거나 직접 응답할 수 있습니다.

미들웨어는 캐시된 콘텐츠 및 경로가 일치하기 전에 실행됩니다. 자세한 내용은 일치하는 경로를 참조하세요.

## 사용 사례

<div class="content-ad"></div>

애플리케이션에 미들웨어를 통합하면 성능, 보안 및 사용자 경험을 크게 향상시킬 수 있습니다. 미들웨어가 특히 효과적인 일반적인 시나리오 중 몇 가지는 다음과 같습니다:

- 인증 및 권한 부여: 특정 페이지 또는 API 경로에 액세스 권한을 부여하기 전에 사용자 식별을 보장하고 세션 쿠키를 확인합니다.
- 서버 측 리디렉션: 특정 조건(예: 지역, 사용자 역할)에 따라 서버 수준에서 사용자를 리디렉션합니다.
- 경로 재작성: 요청 속성에 따라 API 경로나 페이지로 동적으로 경로를 다시 작성하여 A/B 테스트, 기능 롤아웃 또는 레거시 경로를 지원합니다.
- 봇 탐지: 봇 트래픽을 탐지하고 차단하여 리소스를 보호합니다.
- 로깅 및 분석: 페이지나 API가 처리되기 전에 요청 데이터를 캡처하고 분석하여 통찰을 얻습니다.
- 특징 플래깅: 신뢰성 있는 기능 롤아웃이나 테스트를 위해 기능을 동적으로 활성화 또는 비활성화합니다.

미들웨어가 최적의 접근 방식이 아닐 수 있는 상황을 인지하는 것이 중요합니다. 주의해야 할 몇 가지 시나리오는 다음과 같습니다:

- 복잡한 데이터 검색 및 조작: 미들웨어는 직접 데이터 검색 또는 조작을 위해 설계되지 않았습니다. 이 작업은 대신 라우트 핸들러나 서버 측 유틸리티에서 수행해야 합니다.
- 무거운 계산 작업: 미들웨어는 가볍고 빠르게 응답해야 하며 그렇지 않으면 페이지 로드 지연을 초래할 수 있습니다. 무거운 계산 작업이나 장시간 실행되는 프로세스는 전용 라우트 핸들러에서 수행해야 합니다.
- 광범위한 세션 관리: 미들웨어는 기본 세션 작업을 관리할 수 있지만, 광범위한 세션 관리는 전용 인증 서비스나 라우트 핸들러 내에서 처리해야 합니다.
- 직접 데이터베이스 작업: 미들웨어 내에서 직접 데이터베이스 작업을 수행하는 것은 권장되지 않습니다. 데이터베이스 상호 작용은 라우트 핸들러나 서버 측 유틸리티 내에서 수행되어야 합니다.

<div class="content-ad"></div>

## 규칙

프로젝트 루트에 middleware.ts (또는 .js) 파일을 사용하여 미들웨어를 정의하세요. 예를 들어, 페이지나 앱과 동일한 수준에 위치하거나 해당되는 경우 src 내부에 위치시킬 수 있습니다.

> 참고: 프로젝트 당 미들웨어 파일은 하나만 지원되지만, 여전히 미들웨어 논리를 모듈식으로 구성할 수 있습니다. 미들웨어 기능을 별도의 .ts 또는 .js 파일로 분리하고 이를 주요 middleware.ts 파일로 가져올 수 있습니다. 이를 통해 경로별 미들웨어를 깔끔하게 관리할 수 있으며, 미들웨어.ts에서 집중된 제어를 위해 집계된 미들웨어 레이어를 활용할 수 있습니다. 단일 미들웨어 파일을 강제함으로써 구성을 단순화하고 잠재적인 충돌을 방지하며, 여러 미들웨어 레이어를 피함으로써 성능을 최적화할 수 있습니다.

## 예시

<div class="content-ad"></div>

```js
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

// `await`을 사용한다면 이 함수를 `async`로 표시할 수 있습니다
export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL('/home', request.url))
}

// 자세한 내용은 아래 "경로 매핑"을 참조하세요
export const config = {
  matcher: '/about/:path*',
}
```

## 경로 매핑

미들웨어는 프로젝트의 각 경로에 대해 호출됩니다. 따라서 특정 경로를 정확하게 대상화하거나 제외하기 위해 매처(matchers)를 사용하는 것이 중요합니다. 다음은 실행 순서입니다:

- next.config.js에서의 헤더
- next.config.js에서의 리다이렉트
- 미들웨어 (리라이트, 리다이렉트 등)
- next.config.js의 beforeFiles (리라이트)
- 파일시스템 경로 (public/, _next/static/, pages/, app/ 등)
- next.config.js의 afterFiles (리라이트)
- 동적 경로 (/blog/[slug])
- next.config.js의 fallback (리라이트)


<div class="content-ad"></div>

Middleware가 실행될 경로를 정의하는 두 가지 방법이 있습니다:

- 사용자 정의 matcher 구성
- 조건문

### Matcher

matcher를 사용하면 Middleware가 특정 경로에서 실행되도록 필터링할 수 있습니다.

<div class="content-ad"></div>

```js
export const config = {
  matcher: '/about/:path*',
}
```

하나의 경로 또는 배열 구문을 사용하여 여러 경로를 일치시킬 수 있습니다:

```js
export const config = {
  matcher: ['/about/:path*', '/dashboard/:path*'],
}
```

matcher 구성은 완전한 정규식을 허용하므로 부정적 전방탐색이나 글자 일치와 같은 매칭이 지원됩니다. 모든 특정 경로를 제외한 항목들을 일치시키기 위한 부정적 전방탐색의 예시는 여기에서 확인할 수 있습니다:

<div class="content-ad"></div>

```js
export const config = {
  matcher: [
    /*
     * 모든 요청 경로와 일치시킵니다. 단, 다음과 같은 경로는 제외합니다:
     * - api (API 라우트)
     * - _next/static (정적 파일)
     * - _next/image (이미지 최적화 파일)
     * - favicon.ico (파비콘 파일)
     */
    '/((?!api|_next/static|_next/image|favicon.ico).*)',
  ],
}
```

특정 요청에 대해 Middleware를 우회하기 위해 누락 또는 has 배열 또는 두 가지를 결합하여 사용할 수도 있습니다:

```js
export const config = {
  matcher: [
    /*
     * 모든 요청 경로와 일치시킵니다. 단, 다음과 같은 경로는 제외합니다:
     * - api (API 라우트)
     * - _next/static (정적 파일)
     * - _next/image (이미지 최적화 파일)
     * - favicon.ico (파비콘 파일)
     */
    {
      source: '/((?!api|_next/static|_next/image|favicon.ico).*)',
      missing: [
        { type: 'header', key: 'next-router-prefetch' },
        { type: 'header', key: 'purpose', value: 'prefetch' },
      ],
    },
 
    {
      source: '/((?!api|_next/static|_next/image|favicon.ico).*)',
      has: [
        { type: 'header', key: 'next-router-prefetch' },
        { type: 'header', key: 'purpose', value: 'prefetch' },
      ],
    },
 
    {
      source: '/((?!api|_next/static|_next/image|favicon.ico).*)',
      has: [{ type: 'header', key: 'x-present' }],
      missing: [{ type: 'header', key: 'x-missing', value: 'prefetch' }],
    },
  ],
}
```

> 참고: matcher 값은 빌드 시 정적으로 분석될 수 있도록 상수이어야 합니다. 변수와 같은 동적 값은 무시됩니다.


<div class="content-ad"></div>

설정된 매처:

- 반드시 /로 시작해야 함
- 이름이 있는 매개변수를 포함할 수 있음: /about/:path은 /about/a와 /about/b에 매치되지만 /about/a/c에는 매치되지 않음
- 이름이 있는 매개변수에 수정자를 사용할 수 있음( :로 시작함): /about/:path*는 *이 0회 이상을 뜻하기 때문에 /about/a/b/c에 매치됨. ?는 0회 또는 1회, +는 1회 이상을 의미함
- 소괄호 안에 정규 표현식을 사용할 수 있음: /about/(.*)는 /about/:path*와 같음

path-to-regexp 문서에서 더 자세히 알아보세요.

> 알아두면 좋은 것: 역호환성을 위해 Next.js는 항상 /public을 /public/index로 간주합니다. 따라서 /public/:path의 매처는 일치합니다.

<div class="content-ad"></div>

### 조건문

```js
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  if (request.nextUrl.pathname.startsWith('/about')) {
    return NextResponse.rewrite(new URL('/about-2', request.url))
  }

  if (request.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.rewrite(new URL('/dashboard/user', request.url))
  }
}
```

## NextResponse

NextResponse API를 사용하면 다음을 수행할 수 있습니다:

<div class="content-ad"></div>

- 다른 URL로 들어오는 요청을 리디렉션합니다.
- 지정된 URL을 표시하여 응답을 다시 작성합니다.
- API Routes, getServerSideProps 및 리디렉션 대상을 위한 요청 헤더를 설정합니다.
- 응답 쿠키를 설정합니다.
- 응답 헤더를 설정합니다.

Middleware에서 응답을 생성하려면 다음과 같이 할 수 있습니다:

- 응답을 생성하는 route (페이지 또는 라우트 핸들러)로 리다이렉트합니다.
- 직접 NextResponse를 반환합니다. 응답 생성 참조

## 쿠키 사용하기

<div class="content-ad"></div>

쿠키는 일반 헤더입니다. 요청(request)에서는 Cookie 헤더에 저장되고, 응답(response)에서는 Set-Cookie 헤더에 저장됩니다. Next.js는 NextRequest와 NextResponse의 cookies 확장을 통해 이러한 쿠키에 쉽게 접근하고 조작할 수 있는 편리한 방법을 제공합니다.

- 들어오는 요청에 대한 쿠키에는 다음과 같은 메소드가 있습니다: get, getAll, set, delete, has, clear로 쿠키의 존재 여부를 확인하거나 모든 쿠키를 제거할 수 있습니다.
- 나가는 응답에 대한 쿠키에는 get, getAll, set, delete 메소드가 있습니다.

```js
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
 
export function middleware(request: NextRequest) {
  // 들어오는 요청에서 'Cookie: nextjs=fast' 헤더가 있는 것으로 가정
  // RequestCookies API를 사용하여 요청에서 쿠키 가져오기
  let cookie = request.cookies.get('nextjs')
  console.log(cookie) // => { name: 'nextjs', value: 'fast', Path: '/' }
  const allCookies = request.cookies.getAll()
  console.log(allCookies) // => [{ name: 'nextjs', value: 'fast' }]
 
  request.cookies.has('nextjs') // => true
  request.cookies.delete('nextjs')
  request.cookies.has('nextjs') // => false
 
  // ResponseCookies API를 사용하여 응답에 쿠키 설정
  const response = NextResponse.next()
  response.cookies.set('vercel', 'fast')
  response.cookies.set({
    name: 'vercel',
    value: 'fast',
    path: '/',
  })
  cookie = response.cookies.get('vercel')
  console.log(cookie) // => { name: 'vercel', value: 'fast', Path: '/' }
  // 나가는 응답은 `Set-Cookie: vercel=fast; path=/` 헤더를 포함합니다.
 
  return response
}
```

## 헤더 설정

<div class="content-ad"></div>

NextResponse API를 사용하여 요청 및 응답 헤더를 설정할 수 있어요 (요청 헤더 설정은 Next.js v13.0.0부터 가능해요).

```js
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
 
export function middleware(request: NextRequest) {
  // 요청 헤더를 복제하고 새로운 헤더 `x-hello-from-middleware1`를 설정해요
  const requestHeaders = new Headers(request.headers)
  requestHeaders.set('x-hello-from-middleware1', 'hello')
 
  // NextResponse.rewrite에서도 요청 헤더를 설정할 수 있어요
  const response = NextResponse.next({
    request: {
      // 새로운 요청 헤더
      headers: requestHeaders,
    },
  })
 
  // 새로운 응답 헤더 `x-hello-from-middleware2`를 설정해요
  response.headers.set('x-hello-from-middleware2', 'hello')
  return response
}
```

> 잘 알고 계세요: 큰 헤더를 설정하면 백엔드 웹 서버 구성에 따라 431 Request Header Fields Too Large 오류가 발생할 수 있으니 주의해주세요.

### CORS

<div class="content-ad"></div>

CORS 헤더를 미들웨어에서 설정하여 간단하고 사전 요청을 포함한 교차 출처 요청을 허용할 수 있어요.

```js
import { NextRequest, NextResponse } from 'next/server'

const allowedOrigins = ['https://acme.com', 'https://my-app.org']

const corsOptions = {
  'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
  'Access-Control-Allow-Headers': 'Content-Type, Authorization',
}

export function middleware(request: NextRequest) {
  // 요청에서 출처 확인하기
  const origin = request.headers.get('origin') ?? ''
  const isAllowedOrigin = allowedOrigins.includes(origin)

  // 사전 요청 처리하기
  const isPreflight = request.method === 'OPTIONS'

  if (isPreflight) {
    const preflightHeaders = {
      ...(isAllowedOrigin && { 'Access-Control-Allow-Origin': origin }),
      ...corsOptions,
    }
    return NextResponse.json({}, { headers: preflightHeaders })
  }

  // 간단한 요청 처리하기
  const response = NextResponse.next()

  if (isAllowedOrigin) {
    response.headers.set('Access-Control-Allow-Origin', origin)
  }

  Object.entries(corsOptions).forEach(([key, value]) => {
    response.headers.set(key, value)
  })

  return response
}

export const config = {
  matcher: '/api/:path*',
}
```

> 좋은 정보: 루트 핸들러에서 개별 경로에 대한 CORS 헤더를 구성할 수 있어요.

## 응답 생성하기

<div class="content-ad"></div>

Middleware에서 바로 Response 또는 NextResponse 인스턴스를 반환하여 응답할 수 있습니다. 이 기능은 Next.js v13.1.0 이후부터 사용할 수 있습니다.

```js
import { NextRequest } from 'next/server'
import { isAuthenticated } from '@lib/auth'
 
// 미들웨어를 '/api/'로 시작하는 경로에 제한합니다.
export const config = {
  matcher: '/api/:function*',
}
 
export function middleware(request: NextRequest) {
  // 요청을 확인하기 위해 인증 함수를 호출합니다.
  if (!isAuthenticated(request)) {
    // 인증 실패를 나타내는 JSON을 응답합니다.
    return Response.json(
      { success: false, message: '인증 실패' },
      { status: 401 }
    )
  }
}
```

### waitUntil 및 NextFetchEvent

NextFetchEvent 객체는 기본 FetchEvent 객체를 확장하며 waitUntil() 메서드를 포함합니다.

<div class="content-ad"></div>

waitUntil() 메소드는 약속(promise)을 전달받아 Middleware의 수명을 약속이 처리될 때까지 연장합니다. 이것은 백그라운드에서 작업을 수행하는 데 유용합니다.

```js
import { NextResponse } from 'next/server'
import type { NextFetchEvent, NextRequest } from 'next/server'
 
export function middleware(req: NextRequest, event: NextFetchEvent) {
  event.waitUntil(
    fetch('https://my-analytics-platform.com', {
      method: 'POST',
      body: JSON.stringify({ pathname: req.nextUrl.pathname }),
    })
  )
 
  return NextResponse.next()
}
```

## 고급 Middleware 플래그

Next.js v13.1에서 두 가지 추가 플래그인 skipMiddlewareUrlNormalize 및 skipTrailingSlashRedirect가 도입되어 고급 사용 사례를 처리할 수 있습니다.

<div class="content-ad"></div>

`skipTrailingSlashRedirect`은 마지막 슬래시를 추가하거나 제거하는 Next.js 리디렉션을 비활성화합니다. 이를 통해 미들웨어 내에서 특정 경로에 대해 마지막 슬래시를 유지하면서 다른 경로에 대해서는 그렇지 않도록 사용자 정의 처리를 할 수 있어, 점진적인 이주가 더 쉬워집니다.

```js
module.exports = {
  skipTrailingSlashRedirect: true,
}
```

```js
const legacyPrefixes = ['/docs', '/blog']

export default async function middleware(req) {
  const { pathname } = req.nextUrl

  if (legacyPrefixes.some((prefix) => pathname.startsWith(prefix))) {
    return NextResponse.next()
  }

  // 마지막 슬래시 처리 적용
  if (
    !pathname.endsWith('/') &&
    !pathname.match(/((?!\.well-known(?:\/.*)?)(?:[^/]+\/)*[^/]+\.\w+)/)
  ) {
    req.nextUrl.pathname += '/'
    return NextResponse.redirect(req.nextUrl)
  }
}
```

`skipMiddlewareUrlNormalize`은 Next.js에서 URL 정규화를 비활성화하여 직접 방문과 클라이언트 전환을 처리하는 방법을 동일하게 만듭니다. 일부 고급 케이스에서 이 옵션은 원래 URL을 사용하여 완전한 제어를 제공합니다.

<div class="content-ad"></div>

```js
module.exports = {
  skipMiddlewareUrlNormalize: true,
}
```

```js
export default async function middleware(req) {
  const { pathname } = req.nextUrl
 
  // GET /_next/data/build-id/hello.json
 
  console.log(pathname)
  // 비활성화된 플래그와 함께 이제 /_next/data/build-id/hello.json입니다.
  // 활성화하지 않으면 이것은 정규화되어 /hello로 바뀝니다.
}
```

## 런타임

현재 미들웨어는 Edge 런타임만 지원합니다. Node.js 런타임을 사용할 수 없습니다.

<div class="content-ad"></div>

## 버전 이력


| Version | Changes                                                                           |
|---------|-----------------------------------------------------------------------------------|
| v13.1.0 | Advanced Middleware flags added                                                  |
| v13.0.0 | Middleware can modify request headers, response headers, and send responses       |
| v12.2.0 | Middleware is stable, please see the [upgrade guide](/docs/messages/middleware-upgrade-guide) |
| v12.0.9 | Enforce absolute URLs in Edge Runtime ([PR](https://github.com/vercel/next.js/pull/33410)) |
| v12.0.0 | Middleware (Beta) added                                                           |
