---
title: "Nextjs 14  따끈따끈한 Lazy Loading 도입 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:40
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Lazy Loading"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading"
---


# Lazy Loading

레이지 로딩은 Next.js에서 경로를 렌더링하는 데 필요한 JavaScript 양을 줄여 애플리케이션의 초기 로딩 성능을 향상시켜줍니다.

클라이언트 컴포넌트 및 가져온 라이브러리의 로딩을 연기하고, 필요할 때에만 클라이언트 번들에 포함시키는 기능을 제공합니다. 예를 들어, 사용자가 모달을 열 때까지 해당 모달의 로딩을 연기할 수 있습니다.

Next.js에서 레이지 로딩을 구현하는 두 가지 방법이 있습니다:

<div class="content-ad"></div>

- **다이나믹 임포트를 next/dynamic으로 사용하기**
- **React.lazy() 및 Suspense를 사용하기**

기본적으로, 서버 컴포넌트는 자동으로 코드를 분할하며, 스트리밍을 사용하여 서버에서 클라이언트로 UI 조각을 점진적으로 전송할 수 있습니다. Lazy loading은 클라이언트 컴포넌트에 적용됩니다.

## next/dynamic

next/dynamic은 React.lazy() 및 Suspense의 조합입니다. 앱과 페이지 디렉터리에서 동일하게 작동하여 점진적인 마이그레이션을 허용합니다.

<div class="content-ad"></div>

## 예시

### 클라이언트 컴포넌트 가져오기

```js
'use client'
 
import { useState } from 'react'
import dynamic from 'next/dynamic'
 
// 클라이언트 컴포넌트:
const ComponentA = dynamic(() => import('../components/A'))
const ComponentB = dynamic(() => import('../components/B'))
const ComponentC = dynamic(() => import('../components/C'), { ssr: false })
 
export default function ClientComponentExample() {
  const [showMore, setShowMore] = useState(false)
 
  return (
    <div>
      {/* 즉시 로드하지만 별도의 클라이언트 번들에서 로드합니다 */}
      <ComponentA />
 
      {/* 필요할 때만 로드하며 조건이 충족될 때에만 로드합니다 */}
      {showMore && <ComponentB />}
      <button onClick={() => setShowMore(!showMore)}>전환</button>
 
      {/* 클라이언트 사이드에서만 로드합니다 */}
      <ComponentC />
    </div>
  )
}
```

### SSR 생략

<div class="content-ad"></div>

React.lazy()과 Suspense를 사용할 때, 클라이언트 컴포넌트는 기본적으로 사전 렌더링(SSR)됩니다.

클라이언트 컴포넌트의 사전 렌더링을 비활성화하려면 다음과 같이 ssr 옵션을 false로 설정할 수 있습니다:

```js
const ComponentC = dynamic(() => import('../components/C'), { ssr: false })
```

### 서버 컴포넌트 가져오기

<div class="content-ad"></div>

서버 구성 요소를 동적으로 가져오면 서버 구성 요소 그 자체가 게으르게로드되는 것이 아니라, 서버 구성 요소의 자식인 클라이언트 구성 요소만 게으르게로드됩니다.

```js
import dynamic from 'next/dynamic'

// 서버 구성 요소:
const ServerComponent = dynamic(() => import('../components/ServerComponent'))

export default function ServerComponentExample() {
  return (
    <div>
      <ServerComponent />
    </div>
  )
}
```

### 외부 라이브러리 로드하기

import() 함수를 사용하여 필요할 때 외부 라이브러리를로드할 수 있습니다. 이 예에서는 뿌리 검색에 fuse.js 외부 라이브러리를 사용하고 있습니다. 사용자가 검색 입력을 입력할 때 해당 모듈이 클라이언트에로드됩니다.

<div class="content-ad"></div>

```js
'use client'

import { useState } from 'react'

const names = ['Tim', 'Joe', 'Bel', 'Lee']

export default function Page() {
  const [results, setResults] = useState()

  return (
    <div>
      <input
        type="text"
        placeholder="Search"
        onChange={async (e) => {
          const { value } = e.currentTarget
          // 동적으로 fuse.js를 로드합니다
          const Fuse = (await import('fuse.js')).default
          const fuse = new Fuse(names)

          setResults(fuse.search(value))
        }}
      />
      <pre>Results: {JSON.stringify(results, null, 2)}</pre>
    </div>
  )
}
```

### 사용자 정의 로딩 컴포넌트 추가

```js
import dynamic from 'next/dynamic'

const WithCustomLoading = dynamic(
  () => import('../components/WithCustomLoading'),
  {
    loading: () => <p>Loading...</p>,
  }
)

export default function Page() {
  return (
    <div>
      {/* <WithCustomLoading/>이 로딩 중일 때 로딩 컴포넌트가 렌더링됩니다 */}
      <WithCustomLoading />
    </div>
  )
}
```

### Named Export 가져오기



<div class="content-ad"></div>

동적으로 명명된 내보내기를 가져오려면, import() 함수가 반환하는 Promise로부터 해당 값을 반환할 수 있습니다:

```js
'use client'

export function Hello() {
  return <p>Hello!</p>
}
```

```js
import dynamic from 'next/dynamic'

const ClientComponent = dynamic(() =>
  import('../components/hello').then((mod) => mod.Hello)
)
```