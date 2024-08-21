---
title: "서버 및 클라이언트 구성 패턴 Nextjs 14에서 효과적으로 사용하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:22
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Server and Client Composition Patterns"
link: "https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns"
isUpdated: true
---




# 서버 및 클라이언트 구성 패턴

리액트 애플리케이션을 구축할 때, 서버 또는 클라이언트에서 렌더링해야 하는 애플리케이션의 부분을 고려해야 합니다. 이 페이지에서는 서버 및 클라이언트 컴포넌트를 사용할 때 권장되는 구성 패턴을 다룹니다.

## 서버 및 클라이언트 컴포넌트를 언제 사용해야 하나요?

다음은 서버 및 클라이언트 컴포넌트를 사용할 때 다양한 사용 사례에 대한 간단한 요약입니다:

<div class="content-ad"></div>

아래는 서버 컴포넌트와 함께 작업할 때 일반적으로 사용되는 패턴들입니다:


| 해야 할 일 | 서버 컴포넌트 | 클라이언트 컴포넌트 |
| ---------- | ------------ | ------------------- |
| 데이터 가져오기 | <span class="inline-flex align-middle text-green-600"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24" style="color:currentColor;width:18px;height:18px"><path d="M8 11.857l2.5 2.5L15.857 9M22 12c0 5.523-4.477 10-10 10S2 17.523 2 12 6.477 2 12 2s10 4.477 10 10z"></path></svg></span> | <span class="inline-flex align-middle text-gray-900"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24" style="color:currentColor;width:18px;height:18px"><path d="M18 6L6 18"></path><path d="M6 6l12 12"></path></svg></span> |
| 백엔드 리소스에 직접 액세스 | <span class="inline-flex align-middle text-green-600"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24" style="color:currentColor;width:18px;height:18px"><path d="M8 11.857l2.5 2.5L15.857 9M22 12c0 5.523-4.477 10-10 10S2 17.523 2 12 6.477 2 12 2s10 4.477 10 10z"></path></svg></span> | <span class="inline-flex align-middle text-gray-900"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24" style="color:currentColor;width:18px;height:18px"><path d="M18 6L6 18"></path><path d="M6 6l12 12"></path></svg></span> |
| 서버에 민감한 정보 보관(액세스 토큰, API 키 등) | <span class="inline-flex align-middle text-green-600"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width=...


<div class="content-ad"></div>

### 컴포넌트 간 데이터 공유

서버에서 데이터를 가져올 때, 서로 다른 컴포넌트 간에 데이터를 공유해야 하는 경우가 있을 수 있습니다. 예를 들어 레이아웃과 페이지가 동일한 데이터에 의존하는 경우 등이 있을 수 있습니다.

React Context를 사용하는 대신 (서버에서 사용할 수 없음)이나 데이터를 속성으로 전달하는 대신, 필요한 컴포넌트에서 동일한 데이터를 가져오는 데 fetch 또는 React의 캐시 함수를 사용할 수 있습니다. 반복해서 동일한 데이터에 대한 요청을 처리할 필요 없이 필요한 컴포넌트에서 동일한 데이터를 가져올 수 있습니다. React는 데이터 요청을 자동으로 메모이제이션하도록 fetch를 확장하며, fetch를 사용할 수 없을 때 캐시 함수를 사용할 수 있습니다.

React에서 메모이제이션에 대해 더 자세히 알아보세요.

<div class="content-ad"></div>

### 클라이언트 환경에서 서버 전용 코드를 방지하기

자바스크립트 모듈은 서버 및 클라이언트 컴포넌트 모듈 사이에서 공유될 수 있기 때문에, 서버에서만 실행되도록 의도된 코드가 클라이언트로 슬쩍 들어갈 수 있습니다.

예를 들어, 다음과 같은 데이터 가져오는 함수를 살펴봅시다:

```js
export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  })
 
  return res.json()
}
```

<div class="content-ad"></div>

첫 눈에 보면 getData 함수가 서버 및 클라이언트에서 작동하는 것으로 보입니다. 그러나 이 함수에는 API_KEY가 포함되어 있고, 이 API_KEY는 서버에서만 실행되기를 의도하여 작성되었습니다.

환경 변수 API_KEY가 NEXT_PUBLIC로 접두사가 붙이지 않았기 때문에, 이는 클라이언트에서 액세스할 수 없는 프라이빗 변수입니다. 환경 변수가 클라이언트에 노출되지 않도록 하기 위해, Next.js는 프라이빗 환경 변수를 빈 문자열로 대체합니다.

결과적으로, getData() 함수를 클라이언트에서 가져와 실행할 수 있지만 기대한대로 작동하지 않을 것입니다. 또한 변수를 퍼블릭으로 만들면 함수가 클라이언트에서 작동하지만, 민감한 정보를 클라이언트에 노출하고 싶지 않을 수도 있습니다.

서버 코드의 의도치 않은 클라이언트 사용을 방지하기 위해, 이런 모듈 중 하나를 Client Component로 실수로 가져올 때 다른 개발자에게 빌드 타임 오류를 제공하기 위해 서버 전용 패키지를 사용할 수 있습니다.

<div class="content-ad"></div>

서버 전용으로 사용하려면 먼저 패키지를 설치하세요:

```js
npm install server-only
```

그런 다음 서버 전용 코드가 포함된 모듈로 패키지를 가져오세요:

```js
import 'server-only'
 
export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  })
 
  return res.json()
}
```

<div class="content-ad"></div>

이제 getData()를 가져오는 모든 클라이언트 컴포넌트는 이 모듈이 서버에서만 사용할 수 있다는 빌드 시간 오류를 받게 될 것입니다. 

client-only라는 해당 패키지를 사용하여 window 객체에 액세스하는 코드와 같이 클라이언트 전용 코드를 포함하는 모듈을 표시할 수 있습니다.

### 서드파티 패키지 및 제공업체 사용

서버 컴포넌트는 새로운 리액트 기능이므로, 생태계의 써드파티 패키지 및 제공업체들은 useState, useEffect, createContext와 같은 클라이언트 전용 기능을 사용하는 컴포넌트에 "use client" 지시문을 추가하기 시작했습니다.

<div class="content-ad"></div>

오늘은 아직 클라이언트 전용 기능을 사용하는 npm 패키지의 많은 구성 요소들이 지시문을 갖고 있지 않습니다. 이러한 서드파티 구성 요소들은 "use client" 지시문이 있기 때문에 클라이언트 구성 요소 내에서 예상대로 작동하지만 서버 구성 요소 내에서는 작동하지 않습니다.

예를 들어, 가상의 acme-carousel 패키지를 설치했다고 가정해보겠습니다. 여기에는 `Carousel` 컴포넌트가 있습니다. 이 컴포넌트는 useState를 사용하지만 아직 "use client" 지시문이 없습니다.

만약 클라이언트 구성 요소 내에서 `Carousel`을 사용한다면 예상대로 작동할 것입니다:

```js
'use client'

import { useState } from 'react'
import { Carousel } from 'acme-carousel'

export default function Gallery() {
  let [isOpen, setIsOpen] = useState(false)

  return (
    <div>
      <button onClick={() => setIsOpen(true)}>사진 보기</button>

      {/* Works, since Carousel is used within a Client Component */}
      {isOpen && <Carousel />}
    </div>
  )
}
```

<div class="content-ad"></div>

그러나 서버 구성 요소 내에서 직접 사용하려고 한 다음 오류가 표시됩니다:

```js
import {Carousel} from 'acme-carousel'

export default function Page() {
  return (
    <div>
      <p>사진 보기</p>

      {/* 오류: 'useState'는 서버 구성 요소 내에서 사용할 수 없음 */}
      <Carousel />
    </div>
  )
}
```

이는 Next.js가 `Carousel`이 클라이언트 전용 기능을 사용하고 있음을 알지 못하기 때문입니다.

이를 해결하기 위해 클라이언트 전용 기능에 의존하는 타사 구성 요소를 자체 클라이언트 구성 요소로 래핑할 수 있습니다:

<div class="content-ad"></div>

```js
'use client'

import { Carousel } from 'acme-carousel'

export default Carousel
```

이제 서버 컴포넌트에서 `Carousel`을 직접 사용할 수 있습니다:

```js
import Carousel from './carousel'

export default function Page() {
  return (
    <div>
      <p>사진 보기</p>

      {/* `Carousel`은 클라이언트 컴포넌트이기 때문에 작동합니다. */}
      <Carousel />
    </div>
  )
}
```

세 번째 파티 컴포넌트 대부분을 감싸야 할 필요는 없을 것으로 예상됩니다. 대부분의 경우 클라이언트 컴포넌트 내에서 이 컴포넌트를 사용할 것이기 때문입니다. 그러나 제공자는 React 상태와 컨텍스트에 의존하며 일반적으로 애플리케이션의 루트에서 필요합니다. 아래에서 세 번째 파티 컨텍스트 제공자에 대해 자세히 알아보세요.


<div class="content-ad"></div>

#### 컨텍스트 제공자 사용하기

컨텍스트 제공자는 일반적으로 응용 프로그램의 루트 부근에 렌더링되어 현재 테마와 같은 전역 고려 사항을 공유합니다. React 컨텍스트는 서버 컴포넌트에서 지원되지 않기 때문에 응용 프로그램의 루트에 컨텍스트를 생성하려고 하면 오류가 발생합니다:

```js
import { createContext } from 'react'

//  createContext is not supported in Server Components
export const ThemeContext = createContext({})

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>
      </body>
    </html>
  )
}
```

이 문제를 해결하려면 컨텍스트를 만들고 해당 제공자를 클라이언트 컴포넌트 내부에서 렌더링하십시오:

<div class="content-ad"></div>

```js
'클라이언트 사용'
 
import { createContext } from 'react'
 
export const ThemeContext = createContext({})
 
export default function ThemeProvider({
  children,
}: {
  children: React.ReactNode
}) {
  return <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>
}
```

이제 Server Component로 직접 프로바이더를 렌더링할 수 있습니다. 클라이언트 컴포넌트로 표시되었습니다.

```js
import ThemeProvider from './theme-provider'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html>
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  )
}
```

루트에서 프로바이더를 렌더링하면 앱 전반에 걸쳐 모든 다른 클라이언트 컴포넌트가 이 컨텍스트를 사용할 수 있게 됩니다.


<div class="content-ad"></div>

> 좋은 정보: 제공자(Providers)를 트리 안에서 가능한 깊이까지 렌더링하는 것이 좋습니다. ThemeProvider가 'children'을 감싸고 전체 `html` 문서 전체를 감싸지 않는 것에 주목해보세요. 이렇게 함으로써 Next.js가 서버 구성 요소(Server Components)의 정적 부분을 최적화하는 데 도움이 됩니다.

#### 라이브러리 작성자를 위한 조언

비슷한 방식으로, 다른 개발자에 의해 사용될 패키지를 만들어야 하는 라이브러리 작성자는 패키지의 클라이언트 진입점(client entry points)을 표시하기 위해 "use client" 지시문을 사용할 수 있습니다. 이는 패키지의 사용자가 패키지 구성 요소(component)를 직접 서버 구성 요소(Server Components)에 가져오기 위해 래핑 경계를 생성할 필요없이 사용할 수 있게 합니다.

패키지를 최적화하는 데에는 트리 깊숙한 곳에 `use client`를 사용함으로써 가져온 모듈을 서버 구성 요소 모듈 그래프의 일부로 만들 수 있습니다.

<div class="content-ad"></div>

일부 번들러가 "use client" 지시문을 제거할 수 있다는 것을 주목할 가치가 있습니다. React Wrap Balancer와 Vercel Analytics 저장소에서 "use client" 지시문을 포함하도록 esbuild를 구성하는 예제를 찾을 수 있습니다.

## 클라이언트 구성요소

### 클라이언트 구성요소를 트리 하단으로 이동

클라이언트 자바스크립트 번들 크기를 줄이기 위해, 클라이언트 구성요소를 컴포넌트 트리 아래로 이동하는 것을 권장합니다.

<div class="content-ad"></div>

예를 들어, 레이아웃에는 정적 요소(예: 로고, 링크 등)와 상태를 사용하는 대화형 검색 바가 있는 경우가 있습니다.

전체 레이아웃을 클라이언트 컴포넌트로 만들지 말고 대화형 로직을 클라이언트 컴포넌트(예: `SearchBar /`)로 이동하고 레이아웃을 서버 컴포넌트로 유지하세요. 이렇게 하면 레이아웃의 모든 컴포넌트 Javascript를 클라이언트로 보내지 않아도 됩니다.

```js
// SearchBar는 클라이언트 컴포넌트입니다.
import SearchBar from './searchbar'
// Logo는 서버 컴포넌트입니다.
import Logo from './logo'
 
// 레이아웃은 기본적으로 서버 컴포넌트입니다.
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <>
      <nav>
        <Logo />
        <SearchBar />
      </nav>
      <main>{children}</main>
    </>
  )
}
```

### 서버에서 클라이언트 컴포넌트로 속성 전달하기 (직렬화)

<div class="content-ad"></div>

서버 컴포넌트에서 데이터를 가져오는 경우, 데이터를 클라이언트 컴포넌트로 전달하기를 원할 수 있습니다. 서버에서 클라이언트 컴포넌트로 전달되는 프롭은 React에서 직렬화될 수 있어야 합니다.

직렬화될 수 없는 데이터에 의존하는 클라이언트 컴포넌트가 있다면, 서드 파티 라이브러리를 사용하여 클라이언트에서 데이터를 가져오거나 Route Handler를 통해 서버에서 데이터를 가져올 수 있습니다.

## 서버 컴포넌트와 클라이언트 컴포넌트를 교차하는 경우

서버 및 클라이언트 컴포넌트를 교차하는 경우, UI를 컴포넌트 트리로 시각화하는 것이 도움이 될 수 있습니다. 루트 레이아웃부터 시작하여 클라이언트에서 "use client" 지시문을 추가하여 일부 하위 컴포넌트 트리를 클라이언트에서 렌더링할 수 있습니다.

<div class="content-ad"></div>

클라이언트 서브트리 내에서도 서버 구성 요소를 중첩하거나 서버 액션을 호출할 수 있지만 몇 가지 주의할 점이 있습니다:

- 요청-응답 수명 주기 동안 코드는 서버에서 클라이언트로 이동합니다. 클라이언트에서 서버의 데이터나 리소스에 액세스해야 할 경우, 서버로 새 요청을 보내야 하며 계속 왔다갔다하는 것이 아님을 기억해야 합니다.
- 서버로 새 요청을 보낼 때, 모든 서버 구성 요소가 먼저 렌더링되며, 이는 클라이언트 구성 요소 내에 중첩된 서버 구성 요소를 포함합니다. 렌더링된 결과(던져진 서버 클라이언트 페이로드)에는 클라이언트 구성 요소의 위치 참조가 포함됩니다. 그런 다음 클라이언트에서 React는 던져진 서버 클라이언트 페이로드를 사용하여 서버 및 클라이언트 구성 요소를 단일 트리로 조화시킵니다.

- 클라이언트 구성 요소가 서버 구성 요소보다 뒤에 렌더링되기 때문에, 클라이언트 구성 요소 모듈에 서버 구성 요소를 가져올 수는 없습니다(새 서버 요청이 필요하기 때문입니다). 대신, 클라이언트 구성 요소에 서버 구성 요소를 프롭스로 전달할 수 있습니다. 지원되지 않는 패턴 및 지원되는 패턴 섹션을 아래에서 참조하세요.

### 지원되지 않는 패턴: 클라이언트 구성 요소로 서버 구성 요소 가져오기

<div class="content-ad"></div>

다음 패턴은 지원되지 않습니다. 클라이언트 컴포넌트에 서버 컴포넌트를 가져올 수 없습니다:

```js
'use client'
 
// 클라이언트 컴포넌트에 서버 컴포넌트를 가져올 수 없습니다.
 
export default function ClientComponent({
  children,
}: {
  children: React.ReactNode
}) {
  const [count, setCount] = useState(0)
 
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
 
    </>
  )
}
```

### 지원되는 패턴: 서버 컴포넌트를 프롭으로 클라이언트 컴포넌트에 전달하는 경우

다음 패턴은 지원됩니다. 서버 컴포넌트를 프롭으로 클라이언트 컴포넌트에 전달할 수 있습니다.

<div class="content-ad"></div>

React Children 속성을 사용하여 Client Component에 "슬롯"을 만드는 것이 일반적인 패턴입니다.

아래 예시에서 `ClientComponent`는 children 속성을 받아들입니다:

```js
'use client'
 
import { useState } from 'react'
 
export default function ClientComponent({
}: {
  children: React.ReactNode
}) {
  const [count, setCount] = useState(0)
 
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
      {children}
    </>
  )
}
```

`ClientComponent`는 children이 최종적으로 Server Component의 결과로 채워질 것을 알지 못합니다. `ClientComponent`의 유일한 책임은 children이 최종적으로 배치될 위치를 결정하는 것입니다.

<div class="content-ad"></div>

부모 서버 컴포넌트에서는 `ClientComponent`와 `ServerComponent`를 모두 가져와서 `ServerComponent`를 `ClientComponent`의 자식으로 전달할 수 있습니다:

```js
// 이 패턴은 동작합니다:
// 서버 컴포넌트를 클라이언트 컴포넌트의 자식 또는 속성으로 전달할 수 있습니다.
import ClientComponent from './client-component'
import ServerComponent from './server-component'
 
// Next.js의 페이지는 기본적으로 서버 컴포넌트입니다
export default function Page() {
  return (
    <ClientComponent>
    </ClientComponent>
  )
}
```

`ClientComponent`와 `ServerComponent`를 이 방식으로 사용하면 디커플링되어 독립적으로 렌더링할 수 있습니다. 이 경우 서버에 있는 자식 `ServerComponent`가 클라이언트에서 렌더링되는 `ClientComponent`보다 훨씬 먼저 렌더링될 수 있습니다.

> 참고사항:
"컨텐츠를 올리는" 패턴은 부모 컴포넌트가 다시 렌더링될 때 중첩된 자식 컴포넌트를 다시 렌더링하는 것을 피하기 위해 사용되었습니다.
자식 속성에 국한되지 않습니다. 어떤 prop이든 JSX를 전달하는 데 사용할 수 있습니다.