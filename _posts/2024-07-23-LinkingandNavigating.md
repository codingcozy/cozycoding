---
title: "Nextjs 14 App Router로 쉽게 링크 및 네비게이션하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:05
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Linking and Navigating"
link: "https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating"
---


# 링크 및 탐색

Next.js에서 루트 간 이동하는 네 가지 방법이 있습니다:

- `Link` 컴포넌트를 사용하는 방법
- useRouter 훅 사용하기 (클라이언트 컴포넌트)
- 리디렉션 함수 사용하기 (서버 컴포넌트)
- 네이티브 History API 사용하기

이 페이지에서는 각 옵션을 사용하는 방법을 알아보고, 탐색 작동 방식에 대해 자세히 살펴보겠습니다.

<div class="content-ad"></div>

## `Link` 구성 요소

`Link`는 HTML `a` 태그를 확장하여 프리페칭 및 클라이언트 측 라우트 간 탐색을 제공하는 내장된 구성 요소입니다. Next.js에서 라우트 간 이동을 위한 주요하고 권장되는 방법입니다.

`next/link`에서 가져와서 구성 요소에 href 프로퍼티를 전달하여 사용할 수 있습니다:

```js
import Link from 'next/link'
 
export default function Page() {
  return <Link href="/dashboard">대시보드</Link>
}
```

<div class="content-ad"></div>

`Link`에 전달할 수 있는 다른 선택적 props가 있습니다. 더 많은 정보를 보려면 API 레퍼런스를 참조하세요.

### 예시

#### 동적 세그먼트에 링크하기

동적 세그먼트에 링크할 때, 템플릿 리터럴과 보간법을 사용하여 링크 목록을 생성할 수 있습니다. 예를 들어, 블로그 포스트 목록을 생성하려면:

<div class="content-ad"></div>

```js
import Link from 'next/link'
 
export default function PostList({ posts }) {
  return (
    - [이것은 표 태그입니다.]
    {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${post.slug}`}>{post.title}</Link>
        </li>
      ))}
  )
}
```

#### 활성 링크 확인하기

활성 링크를 확인하려면 usePathname()을 사용할 수 있습니다. 예를 들어, 활성 링크에 클래스를 추가하려면 현재 경로명이 링크의 href와 일치하는지 확인할 수 있습니다:

```js
'use client'
 
import { usePathname } from 'next/navigation'
import Link from 'next/link'
 
export function Links() {
  const pathname = usePathname()
 
  return (
    <nav>
      <ul>
        <li>
          <Link className={`link ${pathname === '/' ? 'active' : ''}`} href="/">
            Home
          </Link>
        </li>
        <li>
          <Link
            className={`link ${pathname === '/about' ? 'active' : ''}`}
            href="/about"
          >
            About
          </Link>
        </li>
      </ul>
    </nav>
  )
}
```

<div class="content-ad"></div>

#### id로 스크롤 이동

Next.js 앱 라우터의 기본 동작은 새 경로로 스크롤을 맨 위로 올리거나 뒤로, 앞으로 탐색할 때 스크롤 위치를 유지하는 것입니다.

만약 특정 id로 스크롤하고 싶다면, URL에 # 해시 링크를 추가하거나 href 속성에 해시 링크를 전달할 수 있습니다. 이것은 `Link`가 `a` 요소로 렌더링되기 때문에 가능합니다.

```js
<Link href="/dashboard#settings">Settings</Link>

// 결과
<a href="/dashboard#settings">Settings</a>
```

<div class="content-ad"></div>

> 좋아 알아두세요:
다음.js는 페이지가 뷰포트에 보이지 않을 경우 내비게이션 시 페이지로 스크롤됩니다.

#### 스크롤 복구 비활성화

다음.js 앱 라우터의 기본 동작은 새 경로 상단으로 스크롤하거나 뒤로/앞으로 이동 시 스크롤 위치를 유지하는 것입니다. 이 동작을 비활성화하려면 `Link` 컴포넌트에 scroll='false'를 전달하거나 router.push() 또는 router.replace()에 scroll: false를 전달할 수 있습니다.

```js
// next/link
<Link href="/dashboard" scroll={false}>
  Dashboard
</Link>
```

<div class="content-ad"></div>

```js
// useRouter
import { useRouter } from 'next/navigation'
 
const router = useRouter()
 
router.push('/dashboard', { scroll: false })
```

## useRouter() 훅

useRouter 훅을 사용하면 Client Components에서 라우트를 프로그래밍적으로 변경할 수 있습니다.

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

<div class="content-ad"></div>

useRouter 메소드의 전체 목록은 API 참조를 참고해주세요.

> 권장 사항: 특별한 요구 사항이 없는 한 `Link` 컴포넌트를 사용하여 경로 간 이동을 하세요.

## redirect 함수

서버 컴포넌트의 경우, 대신 redirect 함수를 사용하세요.

<div class="content-ad"></div>

```js
import { redirect } from 'next/navigation'

async function fetchTeam(id: string) {
  const res = await fetch('https://...')
  if (!res.ok) return undefined
  return res.json()
}

export default async function Profile({ params }: { params: {id: string} }) {
  const team = await fetchTeam(params.id)
  if (!team) {
    redirect('/login')
  }

  // ...
}
```

**좋은 정보:**
기억해 두세요:
- redirect는 기본적으로 307 (일시적 리디렉션) 상태 코드를 반환합니다. 서버 작업에서 사용할 때는 POST 요청의 성공 페이지로 리디렉션하기 위해 일반적으로 사용되는 303 (다른 곳을 보라)를 반환합니다.
- redirect는 내부적으로 오류를 throw하므로 try/catch 블록 바깥에서 호출해야 합니다.
- 클라이언트 컴포넌트에서 렌더링 프로세스 중에 호출할 수 있지만 이벤트 핸들러에서는 호출할 수 없습니다. 대신 useRouter 훅을 사용할 수 있습니다.
- redirect는 절대 URL도 허용하며 외부 링크로 리디렉션하는 데 사용할 수 있습니다.
- 렌더링 프로세스 전에 리디렉션을 원한다면 next.config.js 또는 미들웨어를 사용하세요.

추가 정보를 보려면 리디렉트 API 참조를 확인하세요.

## 네이티브 History API 사용하기


<div class="content-ad"></div>

Next.js에서는 네이티브 window.history.pushState와 window.history.replaceState 메소드를 사용하여 페이지를 새로 고치지 않고 브라우저의 히스토리 스택을 업데이트할 수 있습니다.

pushState와 replaceState 호출은 Next.js 라우터에 통합되어 있어 usePathname과 useSearchParams와 동기화할 수 있습니다.

### window.history.pushState

브라우저의 히스토리 스택에 새 항목을 추가할 때 사용합니다. 사용자는 이전 상태로 돌아갈 수 있습니다. 예를 들어, 제품 목록을 정렬할 때 사용할 수 있습니다.

<div class="content-ad"></div>

```js
'use client'

import { useSearchParams } from 'next/navigation'

export default function SortProducts() {
  const searchParams = useSearchParams()

  function updateSorting(sortOrder: string) {
    const params = new URLSearchParams(searchParams.toString())
    params.set('sort', sortOrder)
    window.history.pushState(null, '', `?${params.toString()}`)
  }

  return (
    <>
      <button onClick={() => updateSorting('asc')}>Sort Ascending</button>
      <button onClick={() => updateSorting('desc')}>Sort Descending</button>
    </>
  )
}
```

### window.history.replaceState

Use it to replace the current entry on the browser's history stack. The user is not able to navigate back to the previous state. For example, to switch the application's locale:

```js
'use client'

import { usePathname } from 'next/navigation'

export function LocaleSwitcher() {
  const pathname = usePathname()

  function switchLocale(locale: string) {
    // e.g. '/en/about' or '/fr/contact'
    const newPath = `/${locale}${pathname}`
    window.history.replaceState(null, '', newPath)
  }

  return (
    <>
      <button onClick={() => switchLocale('en')}>English</button>
      <button onClick={() => switchLocale('fr')}>French</button>
    </>
  )
}
```

<div class="content-ad"></div>

## 라우팅 및 내비게이션 작동 방식

App Router는 라우팅 및 내비게이션에 대해 하이브리드 접근 방식을 사용합니다. 서버에서는 응용 프로그램 코드가 라우트 세그먼트로 자동으로 코드 분할됩니다. 클라이언트에서는 Next.js가 라우트 세그먼트를 사전로드하고 캐싱합니다. 이는 사용자가 새로운 경로로 전환할 때 브라우저가 페이지를 다시로드하지 않고 변경된 경로 세그먼트만 다시 렌더링되어 내비게이션 경험과 성능이 향상됩니다.

### 1. 코드 분할

코드 분할을 통해 응용 프로그램 코드를 작은 번들로 분할하여 브라우저에서 다운로드하고 실행할 수 있습니다. 이렇게 하면 전송되는 데이터 양과 각 요청에 대한 실행 시간이 감소하여 성능이 향상됩니다.

<div class="content-ad"></div>

서버 구성 요소를 사용하면 응용 프로그램 코드를 경로 세그먼트별로 자동으로 코드 분할할 수 있습니다. 이는 현재 경로에 필요한 코드만 네비게이션 중에 로드된다는 것을 의미합니다.

### 2. 사전로드

사전로드는 사용자가 방문하기 전에 백그라운드에서 경로를 미리로드하는 방법입니다.

Next.js에서 두 가지 방법으로 경로를 사전로드할 수 있습니다:

<div class="content-ad"></div>

- `Link` 컴포넌트: 사용자 뷰포트에 표시되는대로 라우트가 자동으로 프리페치됩니다. 프리페치는 페이지가 처음로드될 때나 스크롤을 통해 보이는 경우 발생합니다.
- router.prefetch(): useRouter 훅을 사용하여 라우트를 프로그램적으로 프리페치할 수 있습니다.

`Link`의 기본 프리페치 동작(프리페치 속성이 지정되지 않거나 null로 설정된 경우)은 loading.js의 사용 방식에 따라 다릅니다. 공유 레이아웃부터 첫 번째 loading.js 파일까지의 "트리"를 따라 렌더링된 컴포넌트만 프리페치되어 30초 동안 캐시됩니다. 이렇게 하면 전체 동적 라우트를 가져오는 비용이 줄어들며, 사용자에게 시각적인 피드백으로 즉시 로딩 상태를 보여줄 수 있습니다.

프리페치를 비활성화하려면 prefetch 속성을 false로 설정하면 됩니다. 또는 prefetch 속성을 true로 설정하여 로딩 경계를 넘어 전체 페이지 데이터를 프리페치할 수 있습니다.

더 많은 정보는 `Link` API 레퍼런스를 참조하세요.

<div class="content-ad"></div>

> 좋아 알아두기:
개발 환경에서는 프리페칭이 활성화되어 있지 않습니다. 프로덕션 환경에서만 활성화됩니다.

### 3. 캐싱

Next.js에는 라우터 캐시라고 불리는 인메모리 클라이언트 캐시가 있습니다. 사용자가 앱을 탐색하는 동안, 프리페치된 라우트 세그먼트 및 방문한 경로의 React 서버 컴포넌트 페이로드가 캐시에 저장됩니다.

이는 탐색 시 서버로의 새 요청 보다는 캐시를 최대한 재사용함으로써, 요청의 수와 전송되는 데이터 양을 줄여 성능을 향상시킵니다.

<div class="content-ad"></div>

라우터 캐시가 어떻게 작동하는지와 구성하는 방법에 대해 더 자세히 알아보세요.

### 4. 부분 렌더링

부분 렌더링은 탐색 시 변경되는 라우트 세그먼트만 클라이언트에서 다시 렌더링되고 공유 세그먼트는 보존된다는 것을 의미합니다.

예를 들어, 두 개의 동생 라우트인 /dashboard/settings와 /dashboard/analytics 사이를 탐색할 때, 설정과 분석 페이지가 렌더링되고 공유된 대시보드 레이아웃이 보존됩니다.

<div class="content-ad"></div>


![Linking and Navigating](/assets/img/2024-07-23-LinkingandNavigating_0.png)

부분 렌더링 없이 각 탐색마다 전체 페이지가 클라이언트에서 다시 렌더링됩니다. 변경 사항만 렌더링하면 전달되는 데이터 양과 실행 시간이 줄어들어 성능이 향상됩니다.

### 5. 부드러운 탐색

브라우저는 페이지 간 탐색 시 "하드 탐색"을 수행합니다. Next.js App Router를 사용하면 페이지 간 "부드러운 탐색"을 가능하게 하여 변경된 라우트 세그먼트만 다시 렌더링됩니다 (부분 렌더링). 이를 통해 탐색 중에 클라이언트 React 상태가 유지됩니다.


<div class="content-ad"></div>

### 6. 뒤로 가기 및 앞으로 가기 탐색

기본적으로 Next.js는 뒤로 가기 및 앞으로 가기 탐색에 대한 스크롤 위치를 유지하고 Router Cache에서 경로 세그먼트를 재사용합니다.

### 7. 페이지 간 라우팅(pages/와 app/ 간)

pages/에서 app/으로 점진적으로 이주할 때, Next.js 라우터는 두 가지 간의 강력한 탐색을 자동으로 처리합니다. pages/에서 app/으로 전환을 감지하기 위해 app 라우트를 확률적으로 확인하는 클라이언트 라우터 필터가 있습니다. 때로는 잘못된 양성 결과를 가져올 수 있습니다. 기본적으로 그러한 발생빈도는 매우 드빛게 되어 있으며, 잘못된 양성 결과 가능성을 0.01%로 구성합니다. 이 가능성은 next.config.js의 experimental.clientRouterFilterAllowedRate 옵션을 통해 사용자 정의할 수 있습니다. 잘못된 양성 결과 비율을 낮추면 클라이언트 번들에서 생성된 필터의 크기가 증가하게 됩니다.

<div class="content-ad"></div>

대신, 이처리를 완전히 비활성화하고 페이지와 응용 프로그램 간의 라우팅을 수동으로 관리하려면 next.config.js에서 experimental.clientRouterFilter를 false로 설정할 수 있습니다. 이 기능이 비활성화된 경우 페이지의 동적 경로가 앱 경로와 겹치면 기본적으로 올바르게 탐색되지 않을 수 있습니다.