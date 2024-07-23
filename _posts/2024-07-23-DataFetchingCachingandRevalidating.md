---
title: "Nextjs 14 데이터 페칭, 캐싱, 및 재검증 방법 알아보기"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:16
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Data Fetching, Caching, and Revalidating"
link: "https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating"
---


# 데이터 가져 오기, 캐싱 및 재유효화

데이터 가져 오기는 모든 응용 프로그램의 핵심 부분입니다. 이 페이지에서는 React 및 Next.js에서 데이터를 가져 오고 캐시하며 유효성을 검사하는 방법에 대해 설명합니다.

데이터를 가져 오는 네 가지 방법이 있습니다:

- fetch를 사용하여 서버에서 가져 오기
- 서버에서 제 3 자 라이브러리를 사용하여 가져 오기
- 클라이언트에서 Route Handler를 통해 가져 오기
- 클라이언트에서 제 3 자 라이브러리를 사용하여 가져 오기

<div class="content-ad"></div>

## fetch를 사용하여 서버에서 데이터 가져오기

Next.js는 기본 fetch Web API를 확장하여 서버에서 각 fetch 요청의 캐싱 및 재유효화 동작을 구성할 수 있도록 합니다. React는 fetch를 확장하여 React 컴포넌트 트리를 렌더링하는 동안 fetch 요청을 자동으로 메모이제이션합니다.

서버 구성 요소, 라우트 핸들러 및 서버 액션에서 async/await로 fetch를 사용할 수 있습니다.

예시:

<div class="content-ad"></div>

```js
async function getData() {
  const res = await fetch('https://api.example.com/...')
  // 반환 값은 직렬화되지 않습니다
  // Date, Map, Set 등을 반환할 수 있습니다.

  if (!res.ok) {
    // 가장 가까운 `error.js` 에러 바운더리를 활성화합니다
    throw new Error('데이터를 가져오는 데 실패했습니다')
  }

  return res.json()
}

export default async function Page() {
  const data = await getData()

  return <main></main>
}
```

> 알아두면 좋은 사항:
Next.js는 Server Components에서 데이터를 가져올 때 필요한 함수를 제공합니다. 이 함수에는 쿠키 및 헤더와 같은 것들이 포함됩니다. 이러한 함수들은 요청 시간 정보에 의존하여 동적으로 렌더링되도록 할 것입니다.
Route 핸들러에서, fetch 요청은 React 컴포넌트 트리의 일부가 아니기 때문에 메모이제이션이 되지 않습니다.
Server Actions에서, fetch 요청은 캐시되지 않습니다 (기본값 cache: no-store).
TypeScript로 Server Component에서 async/await를 사용하려면 TypeScript 5.1.3 이상과 @types/react 18.2.8 이상이 필요합니다.

### 데이터 캐싱

캐싱은 데이터를 저장하여 매 요청마다 데이터 소스에서 다시 가져오지 않아도 되게 합니다.


<div class="content-ad"></div>

기본적으로 Next.js는 서버의 데이터 캐시에서 fetch의 반환 값을 자동으로 캐싱합니다. 이는 데이터가 빌드 시간이나 요청 시간에 검색되고 캐시되어 각 데이터 요청마다 재사용됨을 의미합니다.

```js
// 'force-cache'는 기본값이며 생략할 수 있음
fetch('https://...', { cache: 'force-cache' })
```

그러나 일부 예외 사항이 있습니다. fetch 요청이 캐시되지 않는 경우:

- 서버 액션 내에서 사용됨.
- POST 메소드를 사용하는 Route Handler 내에서 사용됨.

<div class="content-ad"></div>

> 데이터 캐시란 무엇인가요?  
데이터 캐시는 지속적인 HTTP 캐시입니다. 플랫폼에 따라 캐시는 자동으로 확장되어 여러 지역에서 공유할 수 있습니다.  
데이터 캐시에 대해 더 알아보기.

### 데이터 재검증

재검증은 데이터 캐시를 초기화하고 최신 데이터를 다시 가져오는 과정입니다. 데이터가 변경된 경우 최신 정보를 표시하도록 하려면 유용합니다.

캐시된 데이터는 두 가지 방법으로 재검증할 수 있습니다:

<div class="content-ad"></div>

- 시간 기반 재검증: 일정 시간이 경과한 후 데이터를 자동으로 재검증합니다. 이 방법은 드물게 변경되는 데이터에 유용하며 신선도가 매우 중요하지 않을 때 유용합니다.
- 요청에 따른 재검증: 이벤트(예: 폼 제출)에 기반하여 데이터를 수동으로 재검증합니다. 요청에 따른 재검증은 태그 기반 또는 경로 기반 접근을 사용하여 한번에 데이터 그룹을 재검증할 수 있습니다. 이 방법은 최신 데이터가 가능한 빨리 표시되도록 하려는 경우에 유용합니다 (예: 헤드리스 CMS에서 콘텐츠가 업데이트되는 경우).

#### 시간 기반 재검증

일정 시간 간격으로 데이터를 재검증하려면 fetch의 next.revalidate 옵션을 사용하여 리소스의 캐시 수명(초 단위)을 설정할 수 있습니다.

```js
fetch('https://...', { next: { revalidate: 3600 } })
```

<div class="content-ad"></div>

루트 세그먼트에서 모든 fetch 요청을 다시 유효성 검사하려면 세그먼트 구성 옵션을 사용할 수 있습니다.

```js
export const revalidate = 3600 // 최대 1시간마다 다시 유효성 검사
```

정적으로 렌더링된 라우트에 여러 개의 fetch 요청이 있고 각각 다른 유효성 검사 빈도가 있는 경우, 모든 요청에 대해 가장 낮은 시간이 사용됩니다. 동적으로 렌더링된 라우트의 경우 각 fetch 요청은 독립적으로 다시 유효성을 검사합니다.

시간 기반 유효성 검사에 대해 더 알아보기.

<div class="content-ad"></div>

#### 온디맨드 재검증

데이터는 서버 액션 또는 라우트 핸들러 내에서 경로(revalidatePath)나 캐시 태그(revalidateTag)를 사용하여 온디맨드로 재검증할 수 있습니다.

Next.js에는 루트 간 페치 요청을 무효화하는 캐시 태깅 시스템이 있습니다.

- 페치를 사용할 때 하나 이상의 태그로 캐시 항목을 태그할 수 있습니다.
- 그런 다음 해당 태그와 연관된 모든 항목을 재검증하려면 revalidateTag를 호출할 수 있습니다.

<div class="content-ad"></div>

예를 들어, 다음 fetch 요청은 캐시 태그 컬렉션을 추가합니다:

```js
export default async function Page() {
  const res = await fetch('https://...', { next: { tags: ['collection'] } })
  const data = await res.json()
  // ...
}
```

그런 다음 Server 작업에서 revalidateTag를 호출하여 "collection" 태그로 표시된 이 fetch 호출을 다시 유효화할 수 있습니다:

```js
'use server'

import { revalidateTag } from 'next/cache'

export default async function action() {
  revalidateTag('collection')
}
```

<div class="content-ad"></div>

온디맨드 재검증에 대해 더 알아보세요.

### 오류 처리 및 재검증

데이터를 재검증하는 동안 오류가 발생하면, 마지막으로 성공적으로 생성된 데이터는 캐시로부터 계속 제공됩니다. 다음 요청에서 Next.js는 데이터를 다시 검증하려고 시도할 것입니다.

### 데이터 캐싱 비활성화

<div class="content-ad"></div>

페치 요청이 캐시되지 않는 경우:

- cache: `no-store`가 페치 요청에 추가되었을 때.
- 개별 페치 요청에 revalidate: 0 옵션이 추가된 경우.
- 페치 요청이 POST 메서드를 사용하는 Router Handler 내부에 있는 경우.
- fetch 요청이 헤더나 쿠키를 사용한 후에 오는 경우.
- `force-dynamic` 경로 세그먼트 옵션을 사용하는 동적 const가 있는 경우.
- fetchCache 경로 세그먼트 옵션이 기본적으로 캐시 건너뛰기로 구성된 경우.
- fetch 요청이 Authorization 또는 Cookie 헤더를 사용하고 컴포넌트 트리에서 그 위에 캐시되지 않은 요청이 있는 경우.

#### 개별 페치 요청

개별 페치 요청에서 캐싱을 사용하지 않으려면 fetch에서 cache 옵션을 `no-store`로 설정할 수 있습니다. 이렇게 하면 매 요청마다 데이터를 동적으로 가져올 수 있습니다.

<div class="content-ad"></div>

```js
fetch('https://...', { cache: 'no-store' })
```

fetch API 레퍼런스에서 모든 사용 가능한 캐시 옵션을 확인하세요.

#### 여러 개의 fetch 요청

하나의 라우트 세그먼트(예: 레이아웃이나 페이지)에 여러 개의 fetch 요청이 있는 경우, 세그먼트 구성 옵션을 사용하여 세그먼트 내의 모든 데이터 요청의 캐싱 동작을 구성할 수 있습니다.

<div class="content-ad"></div>

그러나 각 fetch 요청의 캐싱 동작을 개별적으로 구성하는 것을 권장합니다. 이렇게 하면 캐싱 동작을 더 세밀하게 제어할 수 있습니다.

## 서버에서 서드파티 라이브러리를 사용하여 데이터 가져오기

서드파티 라이브러리를 사용하는 경우 (예: 데이터베이스, CMS 또는 ORM 클라이언트 등에서 fetch를 지원하거나 노출하지 않는 경우), Route Segment Config 옵션 및 React의 cache 함수를 사용하여 해당 요청의 캐싱 및 재유효화 동작을 구성할 수 있습니다.

데이터가 캐시되는지 여부는 route 세그먼트가 정적으로 또는 동적으로 렌더링되는지에 따라 달라집니다. 세그먼트가 정적인 경우 (기본값), 요청의 출력은 route 세그먼트의 일부로서 캐시되고 재유효화됩니다. 세그먼트가 동적인 경우 요청의 출력은 캐시 되지 않으며, 세그먼트가 렌더링될 때마다 모든 요청에서 다시 가져옵니다.

<div class="content-ad"></div>

아래 예시에서는 다음과 같습니다:

- React 캐시 함수를 사용하여 데이터 요청을 메모이즈합니다.
- 레이아웃 및 페이지 세그먼트에서 revalidate 옵션을 3600으로 설정하면 데이터가 최대 1시간마다 캐시되고 재유효화됩니다.

<div class="content-ad"></div>

```js
import { 캐시 } from '리액트'

export const getItem = 캐시(async (id: string) => {
  const item = await db.item.findUnique({ id })
  return item
})
```

getItem 함수가 두 번 호출되어도 데이터베이스에는 하나의 쿼리만 실행됩니다.

```js
import { getItem } from '@/utils/get-item'

export const revalidate = 3600 // 최대 한 시간마다 데이터 다시 유효성 검사

export default async function Layout({
  params: { id },
}: {
  params: { id: string }
}) {
  const item = await getItem(id)
  // ...
}
```

```js
import { getItem } from '@/utils/get-item'

export const revalidate = 3600 // 최대 한 시간마다 데이터 다시 유효성 검사

export default async function Page({
  params: { id },
}: {
  params: { id: string }
}) {
  const item = await getItem(id)
  // ...
}
```

<div class="content-ad"></div>

## 라우트 핸들러를 사용하여 클라이언트에서 데이터 가져오기

클라이언트 컴포넌트에서 데이터를 가져와야 하는 경우, 라우트 핸들러를 클라이언트에서 호출할 수 있습니다. 라우트 핸들러는 서버에서 실행되고 데이터를 클라이언트에 반환합니다. 이는 API 토큰과 같은 민감한 정보를 클라이언트에 노출시키고 싶지 않을 때 유용합니다.

예제는 라우트 핸들러 설명서를 참조해주세요.

> 서버 컴포넌트와 라우트 핸들러
서버 컴포넌트는 서버에서 렌더링되기 때문에 데이터를 가져오기 위해 서버 컴포넌트에서 라우트 핸들러를 호출할 필요가 없습니다. 대신에 서버 컴포넌트 내부에서 데이터를 직접 가져올 수 있습니다.

<div class="content-ad"></div>

## 써드파티 라이브러리를 사용하여 클라이언트에서 데이터 가져오기

SWR 또는 TanStack Query와 같은 써드파티 라이브러리를 사용하여 클라이언트에서 데이터를 가져올 수도 있습니다. 이러한 라이브러리는 요청을 메모이징하고 캐싱하며 데이터를 다시 유효화하고 변경하는 데 자체 API를 제공합니다.

> 향후 API:
use는 함수를 받아들이고 처리하는 React 함수입니다. 현재 Client Components에서 fetch를 use로 감싸는 것은 권장되지 않으며 여러 번의 다시 렌더링을 유발할 수 있습니다. React 문서에서 use에 대해 더 알아보세요.