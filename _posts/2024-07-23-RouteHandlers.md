---
title: "Nextjs 14 Route Handlers 쉽게 사용하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:13
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Route Handlers"
link: "https://nextjs.org/docs/app/building-your-application/routing/route-handlers"
isUpdated: true
---




# 라우트 핸들러

라우트 핸들러는 Web 요청 및 응답 API를 사용하여 특정 경로에 대한 사용자 정의 요청 핸들러를 생성할 수 있게 합니다.

<img src="/assets/img/2024-07-23-RouteHandlers_0.png" />

> 알아두면 좋은 사실: 라우트 핸들러는 앱 디렉토리 내에서만 사용할 수 있습니다. 이들은 페이지 디렉토리 내의 API 라우트의 동등물이며, API 라우트와 라우트 핸들러를 함께 사용할 필요가 없습니다.

<div class="content-ad"></div>

## 규칙

Route Handlers는 app 디렉토리 내의 route.js|ts 파일에 정의됩니다:

```js
export const dynamic = 'force-dynamic' // 기본값은 auto입니다
export async function GET(request: Request) {}
```

Route Handlers는 page.js 및 layout.js와 유사하게 app 디렉토리 내에 중첩될 수 있습니다. 그러나 page.js와 동일한 루트 세그먼트 레벨에 route.js 파일이 존재할 수는 없습니다.

<div class="content-ad"></div>

### 지원되는 HTTP 메서드

다음 HTTP 메서드가 지원됩니다: GET, POST, PUT, PATCH, DELETE, HEAD, 그리고 OPTIONS. 지원되지 않는 메서드를 호출하면 Next.js가 405 Method Not Allowed 응답을 반환합니다.

### 확장된 NextRequest 및 NextResponse API

기본 Request와 Response를 지원하는 것 외에도 Next.js는 NextRequest 및 NextResponse를 확장하여 고급 사용 사례에 편리한 도우미를 제공합니다.

<div class="content-ad"></div>

## 동작

### 캐싱

Route Handlers는 Response 객체와 함께 GET 메서드를 사용할 때 기본적으로 캐시됩니다.

```js
export async function GET() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY,
    },
  })
  const data = await res.json()
 
  return Response.json({ data })
}
```

<div class="content-ad"></div>

> TypeScript 경고: Response.json()은 TypeScript 5.2에서만 유효합니다. 낮은 TypeScript 버전을 사용하는 경우 대신 타입 지정된 응답을 위해 NextResponse.json()을 사용할 수 있습니다.

### 캐싱 비활성화 옵션

캐싱을 비활성화하려면 다음을 사용할 수 있습니다:

- GET 메서드와 함께 Request 객체 사용.
- 기타 HTTP 메서드 사용.
- 쿠키 및 헤더와 같은 동적 함수 사용.
- 세그먼트 구성 옵션을 수동으로 동적 모드로 지정하기.

<div class="content-ad"></div>

예를 들어:

```js
export async function GET(request: Request) {
  const { searchParams } = new URL(request.url)
  const id = searchParams.get('id')
  const res = await fetch(`https://data.mongodb-api.com/product/${id}`, {
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY!,
    },
  })
  const product = await res.json()
 
  return Response.json({ product })
}
```

마찬가지로, POST 메서드는 Route Handler가 동적으로 평가되도록 합니다.

```js
export async function POST() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY!,
    },
    body: JSON.stringify({ time: new Date().toISOString() }),
  })
 
  const data = await res.json()
 
  return Response.json(data)
}
```

<div class="content-ad"></div>

> 좋은 정보: API Routes와 마찬가지로 Route Handlers는 폼 제출과 같은 경우에 사용할 수 있습니다. React와 긴밀하게 통합되는 양식 및 변이 처리를 위한 새로운 추상화가 준비 중입니다.

### 경로 해석

경로를 가장 낮은 수준의 라우팅 기본 요소로 간주할 수 있습니다.

- 페이지와 같이 레이아웃이나 클라이언트 측 탐색에 참여하지 않습니다.
- 동일한 경로에 route.js 파일을 둘 수 없습니다.

<div class="content-ad"></div>


<table class="w-full table-auto">
<thead>
<tr>
<th>페이지</th>
<th>경로</th>
<th>결과</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>app/page.js</code></td>
<td><code>app/route.js</code></td>
<td><span class="inline-flex align-middle text-gray-900"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24" style="color:currentColor;width:18px;height:18px"><path d="M18 6L6 18"></path><path d="M6 6l12 12"></path></svg></span> 충돌</td>
</tr>
<tr>
<td><code>app/page.js</code></td>
<td><code>app/api/route.js</code></td>
<td><span class="inline-flex align-middle text-green-600"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24" style="color:currentColor;width:18px;height:18px"><path d="M8 11.857l2.5 2.5L15.857 9M22 12c0 5.523-4.477 10-10 10S2 17.523 2 12 6.477 2 12 2s10 4.477 10 10z"></path></svg></span> 유효한</td>
</tr>
<tr>
<td><code>app/[사용자]/page.js</code></td>
<td><code>app/api/route.js</code></td>
<td><span class="inline-flex align-middle text-green-600"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24" style="color:currentColor;width:18px;height:18px"><path d="M8 11.857l2.5 2.5L15.857 9M22 12c0 5.523-4.477 10-10 10S2 17.523 2 12 6.477 2 12 2s10 4.477 10 10z"></path></svg></span> 유효한</td>
</tr>
</tbody>
</table>

각 `route.js` 또는 `page.js` 파일은 해당 경로의 모든 HTTP 동사를 처리합니다.

```js
export default function Page() {
  return <h1>Hello, Next.js!</h1>
}
 
// ❌ 충돌
// `app/route.js`
export async function POST(request) {}
```

## 예시


<div class="content-ad"></div>

다음 예제는 Route Handlers를 다른 Next.js API 및 기능과 결합하는 방법을 보여줍니다.

### 캐시된 데이터 다시 유효화하기

다음 옵션 `next.revalidate`을 사용하여 캐시된 데이터를 다시 유효화할 수 있습니다:

```js
export async function GET() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    next: { revalidate: 60 }, // 60초마다 유효화
  })
  const data = await res.json()
 
  return Response.json(data)
}
```

<div class="content-ad"></div>

대신에, revalidate 세그먼트 구성 옵션을 사용할 수도 있어요:

```js
export const revalidate = 60
```

### 동적 함수

Route Handlers는 Next.js에서 쿠키와 헤더와 같은 동적 함수와 함께 사용할 수 있어요.

<div class="content-ad"></div>

#### 쿠키

쿠키를 다음/헤더에서 cookies로 읽거나 설정할 수 있습니다. 이 서버 함수는 라우트 핸들러 내에서 직접 호출하거나 다른 함수 안에 중첩시켜 사용할 수 있습니다.

또는 Set-Cookie 헤더를 사용하여 새로운 응답을 반환할 수도 있습니다.

```js
import { cookies } from 'next/headers'
 
export async function GET(request: Request) {
  const cookieStore = cookies()
  const token = cookieStore.get('token')
 
  return new Response('Hello, Next.js!', {
    status: 200,
    headers: { 'Set-Cookie': `token=${token.value}` },
  })
}
```

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 바꿀 수도 있어요:

```js
import { type NextRequest } from 'next/server'
 
export async function GET(request: NextRequest) {
  const token = request.cookies.get('token')
}
```

#### 헤더

next/headers에서 Header를 읽을 수 있어요. 이 서버 함수는 Route Handler에서 직접 호출하거나, 다른 함수 안에 중첩시켜서 사용할 수 있어요.

<div class="content-ad"></div>

이 헤더 인스턴스는 읽기 전용입니다. 헤더를 설정하려면 새로운 헤더가 포함된 새 응답을 반환해야 합니다. 

```js
import { headers } from 'next/headers'
 
export async function GET(request: Request) {
  const headersList = headers()
  const referer = headersList.get('referer')
 
  return new Response('안녕, Next.js!', {
    status: 200,
    headers: { referer: referer },
  })
}
```

요청에서 헤더를 읽는 데 내부 웹 API를 사용할 수도 있습니다 (NextRequest):

```js
import { type NextRequest } from 'next/server'
 
export async function GET(request: NextRequest) {
  const requestHeaders = new Headers(request.headers)
}
```

<div class="content-ad"></div>

### 리다이렉트

```js
import { redirect } from 'next/navigation'

export async function GET(request: Request) {
  redirect('https://nextjs.org/')
}
```

### 동적 경로 세그먼트

> 계속 진행하기 전에 경로 정의 페이지를 읽는 것을 권장합니다.

<div class="content-ad"></div>

라우트 핸들러는 동적 데이터에서 요청 핸들러를 생성하기 위해 동적 세그먼트를 사용할 수 있어요.

```js
export async function GET(
  request: Request,
  { params }: { params: { slug: string } }
) {
  const slug = params.slug // 'a', 'b', or 'c'
}
```

**Route** | **Example URL** | **params**
--- | --- | ---
`app/items/[slug]/route.js` | `/items/a` | `{ slug: 'a' }`
`app/items/[slug]/route.js` | `/items/b` | `{ slug: 'b' }`
`app/items/[slug]/route.js` | `/items/c` | `{ slug: 'c' }`

### URL Query Parameters

<div class="content-ad"></div>

Route Handler에 전달되는 요청 객체는 NextRequest 인스턴스입니다. 이 객체에는 쿼리 파라미터를 쉽게 처리할 수 있도록 몇 가지 편리한 메서드가 포함되어 있습니다.

```js
import { type NextRequest } from 'next/server'
 
export function GET(request: NextRequest) {
  const searchParams = request.nextUrl.searchParams
  const query = searchParams.get('query')
  // query is "hello" for /api/search?query=hello
}
```

### 스트리밍

스트리밍은 OpenAI와 같은 대형 언어 모델(LLM)과 함께 사용되어 AI 생성 콘텐츠를 생성하는 데 자주 사용됩니다. AI SDK에 대해 더 알아보세요.

<div class="content-ad"></div>

```js
import OpenAI from 'openai'
import { OpenAIStream, StreamingTextResponse } from 'ai'
 
const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
})
 
export const runtime = 'edge'
 
export async function POST(req: Request) {
  const { messages } = await req.json()
  const response = await openai.chat.completions.create({
    model: 'gpt-3.5-turbo',
    stream: true,
    messages,
  })
 
  const stream = OpenAIStream(response)
 
  return new StreamingTextResponse(stream)
}
```

이 추상화된 내용은 웹 API를 사용하여 스트림을 만듭니다. 원하는 경우 밑바닥부터 직접 웹 API를 사용할 수도 있습니다.

```js
// https://developer.mozilla.org/docs/Web/API/ReadableStream#convert_async_iterator_to_stream
function iteratorToStream(iterator: any) {
  return new ReadableStream({
    async pull(controller) {
      const { value, done } = await iterator.next()
 
      if (done) {
        controller.close()
      } else {
        controller.enqueue(value)
      }
    },
  })
}
 
function sleep(time: number) {
  return new Promise((resolve) => {
    setTimeout(resolve, time)
  })
}
 
const encoder = new TextEncoder()
 
async function* makeIterator() {
  yield encoder.encode('<p>One</p>')
  await sleep(200)
  yield encoder.encode('<p>Two</p>')
  await sleep(200)
  yield encoder.encode('<p>Three</p>')
}
 
export async function GET() {
  const iterator = makeIterator()
  const stream = iteratorToStream(iterator)
 
  return new Response(stream)
}
```

### 요청 본문

<div class="content-ad"></div>

표 형식을 Markdown 형식으로 변경하세요.

<div class="content-ad"></div>

```js
export async function POST(request: Request) {
  const formData = await request.formData();
  const name = formData.get('name');
  const email = formData.get('email');
  return Response.json({ name, email });
}
```

formData 데이터는 모두 문자열이므로 요청을 유효화하고 원하는 형식(예: 숫자)으로 데이터를 검색하려면 zod-form-data를 사용할 수 있습니다.

### CORS

특정 라우트 핸들러에 대해 CORS 헤더를 설정하려면 표준 Web API 방법을 사용할 수 있습니다.

<div class="content-ad"></div>

```js
export const dynamic = 'force-dynamic' // defaults to auto

export async function GET(request: Request) {
  return new Response('Hello, Next.js!', {
    status: 200,
    headers: {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type, Authorization',
    },
  })
}
```

> 알아 두면 좋아요:
여러 라우트 핸들러에 CORS 헤더를 추가하려면 미들웨어 또는 next.config.js 파일을 사용할 수 있습니다.
또는, CORS 예제 패키지를 참조하세요.

### 웹훅

외부 서비스로부터 웹훅을 받기 위해 라우트 핸들러를 사용할 수 있습니다.

<div class="content-ad"></div>

```js
async function POST(request: Request) {
  try {
    const text = await request.text()
    // 웹훅 페이로드 처리
  } catch (error) {
    return new Response(`웹훅 오류: ${error.message}`, {
      status: 400,
    })
  }
 
  return new Response('성공!', {
    status: 200,
  })
}
```

특히 페이지 라우터와 달리 API 라우트에 추가 구성을 사용할 필요가 없습니다.

### Edge 및 Node.js 런타임

라우트 핸들러는 streaming을 포함한 Edge 및 Node.js 런타임을 원할하게 지원하기 위한 동형 Web API를 갖고 있습니다. 페이지와 레이아웃과 같은 라우트 세그먼트 구성을 사용하는 라우트 핸들러는 일반적으로 정적으로 재생성된 라우트 핸들러와 같은 기다렸던 기능을 지원합니다.


<div class="content-ad"></div>

런타임을 지정하는 데 runtime 세그먼트 구성 옵션을 사용할 수 있어요:

```js
export const runtime = 'edge' // 'nodejs' is the default
```

### UI 응답이 아닌 경우

Route Handlers를 사용하여 UI가 아닌 내용을 반환할 수 있어요. sitemap.xml, robots.txt, 앱 아이콘 및 오픈 그래프 이미지는 모두 내장 지원되어 있어요.

<div class="content-ad"></div>

```js
내보내기 const dynamic = 'force-dynamic' // 기본값은 자동으로 설정됨

내보내기 async function GET() {
  return new Response(
    `<?xml version="1.0" encoding="UTF-8" ?>
<rss version="2.0>

<channel>
  <title>Next.js 문서</title>
  <link>https://nextjs.org/docs</link>
  <description>웹을 위한 React 프레임워크</description>
</channel>

</rss>`,
    {
      headers: {
        'Content-Type': 'text/xml',
      },
    }
  )
}
```

### 세그먼트 구성 옵션

Route Handlers는 페이지 및 레이아웃과 동일한 라우트 세그먼트 구성을 사용합니다.

```js
내보내기 const dynamic = 'auto'
내보내기 const dynamicParams = true
내보내기 const revalidate = false
내보내기 const fetchCache = 'auto'
내보내기 const runtime = 'nodejs'
내보내기 const preferredRegion = 'auto'
```

<div class="content-ad"></div>

API 참조에서 더 자세한 내용을 확인해보세요!