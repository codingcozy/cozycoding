---
title: "Nextjs 14 Page Redirecting을 쉽게 구현하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:08
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Redirecting"
link: "https://nextjs.org/docs/app/building-your-application/routing/redirecting"
isUpdated: true
---




# 리다이렉팅

Next.js에서 리다이렉트를 처리하는 몇 가지 방법이 있습니다. 이 페이지에서는 각 옵션, 사용 사례 및 많은 수의 리다이렉트를 관리하는 방법을 알아보겠습니다.

| API | 목적 | 위치 | 상태 코드 |
| --- | ---- | ---- | --------- |
| [redirect](#redirect-function) | 변이 또는 이벤트 후 사용자를 리다이렉트 | 서버 컴포넌트, 서버 액션, 라우트 핸들러 | 307 (임시) 또는 303 (서버 액션) |
| [permanentRedirect](#permanentredirect-function) | 변이 또는 이벤트 후 사용자를 리다이렉트 | 서버 컴포넌트, 서버 액션, 라우트 핸들러 | 308 (영구) |
| [useRouter](#userouter-hook) | 클라이언트 측 내비게이션 수행 | 클라이언트 컴포넌트의 이벤트 핸들러 | N/A |
| [next.config.js](#redirects-in-nextconfigjs) 파일의 [redirects](code`next.config.js`) | 경로를 기반으로 들어오는 요청을 리다이렉트 | `next.config.js` 파일 | 307 (임시) 또는 308 (영구) |
| [NextResponse.redirect](#nextresponseredirect-in-middleware) | 조건에 따라 들어오는 요청을 리다이렉트 | 미들웨어 | 어떤 |

## redirect 함수

<div class="content-ad"></div>

리다이렉트 함수를 사용하면 사용자를 다른 URL로 리다이렉트할 수 있어요. 서버 컴포넌트, 라우트 핸들러 및 서버 액션에서 redirect를 호출할 수 있어요.

리다이렉트는 변이나 이벤트 후에 자주 사용돼요. 예를 들어, 포스트를 만들 때:

```js
'use server'
 
import { redirect } from 'next/navigation'
import { revalidatePath } from 'next/cache'
 
export async function createPost(id: string) {
  try {
    // 데이터베이스 호출
  } catch (error) {
    // 에러 처리
  }
 
  revalidatePath('/posts') // 캐시된 포스트 업데이트
  redirect(`/post/${id}`) // 새로운 포스트 페이지로 이동
}
```

> 알아두면 좋아요:
- 리다이렉트는 기본적으로 307 (임시 리다이렉트) 상태 코드를 반환해요. 서버 액션에서 사용될 때는 POST 요청의 결과로 성공 페이지로 리다이렉트할 때 일반적으로 사용되는 303 (다른 곳을 찾아봐)를 반환해요.
- 리다이렉트는 내부적으로 에러를 throw하기 때문에 try/catch 블록 외부에서 호출돼야 해요.
- 렌더링 프로세스 중에 Client Components에서 redirect를 호출할 수 있지만 이벤트 핸들러에서는 사용할 수 없어요. 대신 useRouter 훅을 사용할 수 있어요.
- redirect는 절대 URL을 허용하며 외부 링크로 리다이렉트하는 데 사용될 수 있어요.
- 렌더 프로세스 이전에 리다이렉트하려면 next.config.js 또는 미들웨어를 사용하세요.

<div class="content-ad"></div>

더 많은 정보를 원하시면 리다이렉트 API 참조를 참고하세요.

## permanentRedirect 함수

permanentRedirect 함수를 사용하면 사용자를 영구적으로 다른 URL로 리다이렉트할 수 있습니다. Server Components, Route Handlers, 그리고 Server Actions에서 permanentRedirect를 호출할 수 있습니다.

permanentRedirect는 종종 사용자가 유저네임을 변경한 후에 유저의 프로필 URL을 업데이트한 후와 같이 엔티티의 캐노니컬 URL을 변경하는 변이 또는 이벤트 이후에 사용됩니다.

<div class="content-ad"></div>

```js
'use server'
 
import { permanentRedirect } from 'next/navigation'
import { revalidateTag } from 'next/cache'
 
export async function updateUsername(username: string, formData: FormData) {
  try {
    // Call database
  } catch (error) {
    // Handle errors
  }
 
  revalidateTag('username') // Update all references to the username
  permanentRedirect(`/profile/${username}`) // Navigate to the new user profile
}
```

> 좋아요:
permanentRedirect는 기본적으로 308 (영구적 재지정) 상태 코드를 반환합니다.
permanentRedirect는 절대 URL도 허용하며 외부 링크로 리디렉션하는 데 사용할 수 있습니다.
렌더링 프로세스 전에 리디렉션을 원한다면 next.config.js 또는 미들웨어를 사용하세요.

더 자세한 정보는 permanentRedirect API 참조를 참고하세요.

## useRouter() 훅


<div class="content-ad"></div>

만일 클라이언트 컴포넌트 안에서 이벤트 핸들러 내에서 리디렉션을 해야 한다면, `useRouter` 훅의 `push` 메서드를 사용할 수 있어요. 예를 들면:

```js
'use client'
 
import { useRouter } from 'next/navigation'
 
export default function Page() {
  const router = useRouter()
 
  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  )
}
```

> 참고:
만약 사용자를 프로그래밍 방식으로 이동시킬 필요가 없다면, `Link` 컴포넌트를 사용해야 해요.

자세한 내용은 `useRouter` API 참조를 확인해주세요.

<div class="content-ad"></div>

## next.config.js에서의 리다이렉트

next.config.js 파일의 redirects 옵션을 사용하면 들어오는 요청 경로를 다른 대상 경로로 리다이렉트할 수 있습니다. 이는 페이지의 URL 구조를 변경하거나 미리 알려진 리다이렉트 목록이 있는 경우 유용합니다.

리다이렉트는 경로, 헤더, 쿠키 및 쿼리 매칭을 지원하므로 들어오는 요청에 따라 사용자를 리다이렉트할 수 있습니다.

리다이렉트를 사용하려면 next.config.js 파일에 해당 옵션을 추가하세요.

<div class="content-ad"></div>

```js
module.exports = {
  async redirects() {
    return [
      // 기본 리디렉션
      {
        source: '/about',
        destination: '/',
        permanent: true,
      },
      // 와일드카드 경로 일치
      {
        source: '/blog/:slug',
        destination: '/news/:slug',
        permanent: true,
      },
    ]
  },
}
```

더 많은 정보를 원하시면 리디렉션 API 참조를 확인해주세요.

> 알아두면 좋은 사실:
리디렉션은 permanent 옵션과 함께 307 (임시 리디렉션) 또는 308 (영구 리디렉션) 상태 코드를 반환할 수 있습니다.
리디렉션은 플랫폼에 제한이 있을 수 있습니다. 예를 들어 Vercel에서는 1,024개의 리디렉션 제한이 있습니다. 수많은 리디렉션(1000개 이상)을 관리하려면 Middleware를 사용하여 사용자 정의 솔루션을 만드는 것을 고려하십시오. 더 많은 정보는 대규모 리디렉션 관리를 참조해주세요.
리디렉션은 Middleware 이전에 실행됩니다.

## Middleware에서 NextResponse.redirect

<div class="content-ad"></div>

미들웨어는 요청이 완료되기 전에 코드를 실행할 수 있게 해줍니다. 그 후에 들어오는 요청을 기반으로 NextResponse.redirect를 사용하여 다른 URL로 리다이렉트할 수 있습니다. 사용자를 조건에 따라 리다이렉트하고자 할 때 유용하며(예: 인증, 세션 관리 등) 또는 많은 리다이렉트가 필요할 때 유용합니다.

예를 들어, 사용자가 인증되지 않은 경우 "/login" 페이지로 리다이렉트하는 방법은 다음과 같습니다:

```js
import { NextResponse, NextRequest } from 'next/server'
import { authenticate } from 'auth-provider'
 
export function middleware(request: NextRequest) {
  const isAuthenticated = authenticate(request)
 
  // 사용자가 인증되었을 경우 계속 진행
  if (isAuthenticated) {
    return NextResponse.next()
  }
 
  // 인증되지 않은 경우 로그인 페이지로 리다이렉트
  return NextResponse.redirect(new URL('/login', request.url))
}
 
export const config = {
  matcher: '/dashboard/:path*',
}
```

> 알아두면 좋은 점:
미들웨어는 next.config.js에서의 리다이렉트 이후에 실행되고 렌더링 이전에 실행됩니다.

<div class="content-ad"></div>

더 많은 정보를 찾으려면 Middleware 문서를 참조해보세요.

## 대규모 리다이렉트 관리 (고급)

대량의 리다이렉트(1000개 이상)를 관리하려면 Middleware를 사용하여 사용자 정의 솔루션을 작성하는 것이 좋습니다. 이를 통해 응용 프로그램을 다시 배포할 필요 없이 리다이렉트를 프로그래밍 방식으로 처리할 수 있습니다.

이를 위해서는 다음을 고려해야 합니다:

<div class="content-ad"></div>

- 리디렉트 맵을 작성하고 저장하는 중입니다.
- 데이터 조회 성능 최적화 중입니다.

> Next.js 예시: Bloom 필터를 사용한 미들웨어를 확인하여 아래 권장 사항을 구현해보세요.

### 1. 리디렉트 맵 작성 및 저장

리디렉트 맵은 데이터베이스(보통 키-값 저장소)나 JSON 파일에 저장할 수 있는 리디렉트 목록입니다.

<div class="content-ad"></div>

다음 데이터 구조를 고려해보세요:

```js
{
  "/old": {
    "destination": "/new",
    "permanent": true
  },
  "/blog/post-old": {
    "destination": "/blog/post-new",
    "permanent": true
  }
}
```

Middleware에서는 Vercel의 Edge Config나 Redis와 같은 데이터베이스에서 데이터를 읽어와서 들어오는 요청에 기반해 사용자를 리디렉션할 수 있습니다:

```js
import { NextResponse, NextRequest } from 'next/server'
import { get } from '@vercel/edge-config'

type RedirectEntry = {
  destination: string
  permanent: boolean
}

export async function middleware(request: NextRequest) {
  const pathname = request.nextUrl.pathname
  const redirectData = await get(pathname)

  if (redirectData && typeof redirectData === 'string') {
    const redirectEntry: RedirectEntry = JSON.parse(redirectData)
    const statusCode = redirectEntry.permanent ? 308 : 307
    return NextResponse.redirect(redirectEntry.destination, statusCode)
  }

  // 리디렉션이 없는 경우, 리디렉션 없이 계속 진행합니다
  return NextResponse.next()
}
```

<div class="content-ad"></div>

### 2. 데이터 조회 성능 최적화

모든 들어오는 요청마다 대규모 데이터 집합을 읽는 것은 느리고 비싸다. 데이터 조회 성능을 최적화하는 두 가지 방법이 있습니다:

- 빠른 조회를 위해 최적화된 데이터베이스인 Vercel Edge Config나 Redis를 사용합니다.
- Bloom 필터와 같은 데이터 조회 전략을 사용하여 큰 리디렉션 파일이나 데이터베이스를 읽기 전에 리디렉션이 존재하는지 효율적으로 확인합니다.

이전 예시를 고려해보면 Middleware에 생성된 bloom 필터 파일을 가져와서 들어오는 요청 경로명이 bloom 필터에 있는지 확인할 수 있습니다.

<div class="content-ad"></div>

만약 이렇게 된다면 실제 파일을 확인하는 Route Handler로 요청을 전달하여 사용자를 적절한 URL로 리디렉션합니다. 이는 미들웨어로 큰 리디렉션 파일을 가져오는 것을 피하고 모든 들어오는 요청을 느리게 만드는 것을 방지합니다.

```js
import { NextResponse, NextRequest } from 'next/server'
import { ScalableBloomFilter } from 'bloom-filters'
import GeneratedBloomFilter from './redirects/bloom-filter.json'

type RedirectEntry = {
  destination: string
  permanent: boolean
}

// 생성된 JSON 파일에서 블룸 필터 초기화
const bloomFilter = ScalableBloomFilter.fromJSON(GeneratedBloomFilter as any)

export async function middleware(request: NextRequest) {
  // 들어오는 요청의 경로 가져오기
  const pathname = request.nextUrl.pathname

  // 경로가 블룸 필터에 있는지 확인
  if (bloomFilter.has(pathname)) {
    // 경로를 Route Handler에 전달
    const api = new URL(
      `/api/redirects?pathname=${encodeURIComponent(request.nextUrl.pathname)}`,
      request.nextUrl.origin
    )

    try {
      // Route Handler에서 리디렉션 데이터 가져오기
      const redirectData = await fetch(api)

      if (redirectData.ok) {
        const redirectEntry: RedirectEntry | undefined =
          await redirectData.json()

        if (redirectEntry) {
          // 상태 코드 결정
          const statusCode = redirectEntry.permanent ? 308 : 307

          // 목적지로 리디렉션
          return NextResponse.redirect(redirectEntry.destination, statusCode)
        }
      }
    } catch (error) {
      console.error(error)
    }
  }

  // 리디렉션을 찾을 수 없으면 리디렉션하지 않고 요청 계속
  return NextResponse.next()
}
```

그런 다음, Route Handler에서:

```js
import { NextRequest, NextResponse } from 'next/server'
import redirects from '@/app/redirects/redirects.json'

type RedirectEntry = {
  destination: string
  permanent: boolean
}

export function GET(request: NextRequest) {
  const pathname = request.nextUrl.searchParams.get('pathname')
  if (!pathname) {
    return new Response('Bad Request', { status: 400 })
  }

  // redirects.json 파일에서 리디렉션 엔트리 가져오기
  const redirect = (redirects as Record<string, RedirectEntry>)[pathname]

  // 블룸 필터 거짓 양성 고려
  if (!redirect) {
    return new Response('No redirect', { status: 400 })
  }

  // 리디렉션 엔트리 반환
  return NextResponse.json(redirect)
}
```

<div class="content-ad"></div>

> 참고할 점:
블룸 필터를 생성하려면 bloom-filters와 같은 라이브러리를 사용할 수 있어요.
루트 핸들러로 요청을 보내기 전에 악의적인 요청을 방지하기 위해 요청을 확인해야 해요.