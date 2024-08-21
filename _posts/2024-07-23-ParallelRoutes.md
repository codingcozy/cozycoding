---
title: "Nextjs 14 Parallel Routes 사용법 앱 라우터 최적화 팁"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:12
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Parallel Routes"
link: "https://nextjs.org/docs/app/building-your-application/routing/parallel-routes"
isUpdated: true
---




# 병렬 경로

병렬 경로를 사용하면 동시에 또는 조건부로 동일한 레이아웃 내에서 하나 이상의 페이지를 렌더링할 수 있습니다. 이는 대시보드나 소셜 사이트의 피드와 같이 앱의 매우 동적인 섹션에 유용합니다.

예를 들어 대시보드를 생각해보면 병렬 경로를 사용하여 팀 페이지와 분석 페이지를 동시에 렌더링할 수 있습니다:

![병렬 경로](/assets/img/2024-07-23-ParallelRoutes_0.png)

<div class="content-ad"></div>

## 슬롯

병렬 경로는 명명된 슬롯을 사용하여 생성됩니다. 슬롯은 @폴더 규칙을 사용하여 정의됩니다. 예를 들어, 다음 파일 구조는 @analytics와 @team 두 개의 슬롯을 정의합니다:

![image](/assets/img/2024-07-23-ParallelRoutes_1.png)

슬롯은 공유 부모 레이아웃에 속성(props)로 전달됩니다. 위 예제에서, app/layout.js에 있는 컴포넌트는 이제 @analytics와 @team 슬롯 속성(props)을 허용하고, children 속성(props)과 병렬로 렌더링할 수 있습니다.

<div class="content-ad"></div>

```jsx
export default function Layout({
  children,
  team,
  analytics,
}: {
  children: React.ReactNode
  analytics: React.ReactNode
  team: React.ReactNode
}) {
  return (
    <>
      {children}
      {team}
      {analytics}
    </>
  )
}
```

그러나 슬롯은 경로 세그먼트가 아니며 URL 구조에 영향을 주지 않습니다. 예를 들어, /@analytics/views의 경우 URL은 @analytics가 슬롯이므로 /views가 됩니다.

> 알아두면 좋은 사항:
children 프롭은 폴더에 매핑되어야 하는 필수 슬롯이 아닙니다. 즉, app/page.js는 app/@children/page.js와 동일합니다.

## 활성 상태 및 네비게이션


<div class="content-ad"></div>

기본적으로 Next.js는 각 슬롯의 활성 상태(또는 하위 페이지)를 추적합니다. 그러나 슬롯 내에 렌더링된 콘텐츠는 다음과 같은 네비게이션 유형에 따라 달라집니다:

- 소프트 네비게이션: 클라이언트 측 네비게이션 중에는 Next.js가 부분 렌더링을 수행하여 슬롯 내의 하위 페이지를 변경하고 다른 슬롯의 활성 하위 페이지를 유지합니다. 현재 URL과 일치하지 않더라도.
- 하드 네비게이션: 전체 페이지 로드(브라우저 새로고침) 후에는 Next.js가 현재 URL과 일치하지 않는 슬롯에 대해 활성 상태를 결정할 수 없습니다. 대신, 일치하지 않는 슬롯에 대해 default.js 파일을 렌더링하거나 default.js가 없는 경우 404를 렌더링합니다.

> 알아두면 좋은 사실:
일치하지 않는 경로에 대한 404는 그 경로가 의도되지 않은 페이지에 대해 실수로 병렬 경로를 렌더링하지 않도록 도와줍니다.

### default.js

<div class="content-ad"></div>

초기 로드 또는 전체 페이지 다시로드 중에 일치하지 않는 슬롯에 대해 백업으로 렌더링 할 default.js 파일을 정의할 수 있습니다.

다음 폴더 구조를 고려해보세요. @team 슬롯은 /settings 페이지를 가지고 있지만 @analytics는 그렇지 않습니다.

![폴더 구조](/assets/img/2024-07-23-ParallelRoutes_2.png)

/settings로 이동할 때, @team 슬롯은 /settings 페이지를 렌더링하고, @analytics 슬롯을 유지한 상태에서 현재 활성화된 페이지를 유지합니다.

<div class="content-ad"></div>

새로고침 시, Next.js는 @analytics에 대한 default.js를 렌더링합니다. default.js가 없는 경우 404가 대신 렌더링됩니다.

또한, children이 암시적 슬롯인 경우에는 부모 페이지의 활성 상태를 복구할 수 없을 때 children에 대한 대체물을 렌더링하기 위해 default.js 파일을 만들어야 합니다.

### useSelectedLayoutSegment(s)

useSelectedLayoutSegment 및 useSelectedLayoutSegments는 parallelRoutesKey 매개변수를 받아들입니다. 이를 통해 슬롯 내에서 활성 라우트 세그먼트를 읽을 수 있습니다.

<div class="content-ad"></div>

```js
'클라이언트 사용'
 
import { useSelectedLayoutSegment } from 'next/navigation'
 
export default function Layout({ auth }: { auth: React.ReactNode }) {
  const loginSegment = useSelectedLayoutSegment('auth')
  // ...
}
```

사용자가 app/@auth/login(또는 URL 막대의 /login)으로 이동하면 loginSegment가 문자열 "login"과 동일합니다.

## 예시

### 조건부 라우트


<div class="content-ad"></div>

Parallel Routes를 사용하면 사용자 역할과 같은 특정 조건에 따라 라우트를 조건부로 렌더링할 수 있어요. 예를 들어, /admin 또는 /user 역할에 따라 다른 대시보드 페이지를 렌더링하려면:

![Parallel Routes](/assets/img/2024-07-23-ParallelRoutes_3.png)

```js
import { checkUserRole } from '@/lib/auth'
 
export default function Layout({
  user,
  admin,
}: {
  user: React.ReactNode
  admin: React.ReactNode
}) {
  const role = checkUserRole()
  return <>{role === 'admin' ? admin : user}</>
}
```

### 탭 그룹

<div class="content-ad"></div>

슬롯 내에 레이아웃을 추가하여 사용자가 해당 슬롯을 독립적으로 탐색할 수 있게 할 수 있습니다. 이는 탭을 만드는 데 유용합니다.

예를 들어, @analytics 슬롯에는 두 개의 하위 페이지인 /page-views와 /visitors가 있습니다.

![이미지](/assets/img/2024-07-23-ParallelRoutes_4.png)

@analytics 내에서 두 페이지 간에 탭을 공유하기 위해 레이아웃 파일을 만들어보세요:

<div class="content-ad"></div>

```js
import Link from 'next/link'
 
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <>
      <nav>
        <Link href="/page-views">페이지 조회수</Link>
        <Link href="/visitors">방문자</Link>
      </nav>
      <div>{children}</div>
    </>
  )
}
```

### 모달

병렬 루트는 가로채기 루트와 함께 사용하여 모달을 만들 수 있습니다. 이를 통해 다음과 같은 모달 생성 시 자주 발생하는 일반적인 문제를 해결할 수 있습니다:

- URL을 통해 모달 콘텐츠를 공유할 수 있습니다.
- 페이지가 새로 고쳐져도 모달을 닫지 않고 컨텍스트를 보존할 수 있습니다.
- 이전 루트로 이동하는 대신 뒤로 가기로 모달을 닫을 수 있습니다.
- 앞으로 가기로 모달을 다시 여는 것이 가능합니다.

<div class="content-ad"></div>

다음 UI 패턴을 고려해보세요. 사용자는 클라이언트 측 탐색을 사용하여 레이아웃에서 로그인 모달을 열거나 별도의 /login 페이지에 액세스할 수 있습니다:

![UI Pattern](/assets/img/2024-07-23-ParallelRoutes_5.png)

이 패턴을 구현하려면, 먼저 메인 로그인 페이지를 렌더링하는 /login route를 만드세요.

![Implementing Pattern](/assets/img/2024-07-23-ParallelRoutes_6.png)

<div class="content-ad"></div>

```js
import { 로그인 } from '@/app/ui/login'
 
export default function 페이지() {
  return <로그인 />
}
```

그런 다음 @auth 슬롯 내에서 null을 반환하는 default.js 파일을 추가하세요. 이렇게 함으로써 모달이 활성화되지 않은 경우 렌더링되지 않습니다.

```js
export default function 기본() {
  return null
}
```

@auth 슬롯 내에서 /(.)login 폴더를 업데이트하여 /login 경로를 가로채도록 설정하세요. `Modal` 컴포넌트 및 해당 하위 컴포넌트를 /(.)login/page.tsx 파일에 가져와주세요:

<div class="content-ad"></div>

```js
import { Modal } from '@/app/ui/modal'
import { Login } from '@/app/ui/login'

export default function Page() {
  return (
    <Modal>
      <Login />
    </Modal>
  )
}
```

> 좋은 정보:
경로를 가로채는 데 사용되는 규칙인(.), 파일 시스템 구조에 따라 다를 수 있습니다. 경로 가로채기 규칙을 참조하세요.
`Modal` 기능을 modal 콘텐츠인 `Login`으로 분리함으로써 모달 안의 양식 등이 서버 컴포넌트임을 보장할 수 있습니다. 자세한 내용은 서버 및 클라이언트 컴포넌트 교차에 대해 참조하세요.

#### 모달 열기

이제 Next.js 라우터를 사용하여 모달을 열고 닫을 수 있습니다. 이렇게 하면 모달이 열릴 때 URL이 올바르게 업데이트되고, 뒤로 가기 및 앞으로 가기할 때도 올바르게 작동합니다.

<div class="content-ad"></div>

모달을 열려면 @auth 슬롯을 부모 레이아웃에 prop으로 전달하고 자식 prop과 함께 렌더링합니다.

```js
import Link from 'next/link'
 
export default function Layout({
  auth,
  children,
}: {
  auth: React.ReactNode
  children: React.ReactNode
}) {
  return (
    <>
      <nav>
        <Link href="/login">모달 열기</Link>
      </nav>
      <div>{auth}</div>
      <div>{children}</div>
    </>
  )
}
```

사용자가 `Link`를 클릭하면 /login 페이지로 이동하는 대신 모달이 열립니다. 그러나 새로고침하거나 초기로드할 때는 /login으로 이동하여 메인 로그인 페이지로 이동합니다.

#### 모달 닫기

<div class="content-ad"></div>

모달을 닫으려면 `router.back()`을 호출하거나 Link 컴포넌트를 사용하면 됩니다.

```js
'use client'

import { useRouter } from 'next/navigation'

export function Modal({ children }: { children: React.ReactNode }) {
  const router = useRouter()

  return (
    <>
      <button
        onClick={() => {
          router.back()
        }}
      >
        모달 닫기
      </button>
      <div>{children}</div>
    </>
  )
}
```

@auth 슬롯을 더 이상 렌더링하지 않아야 하는 페이지로 이동할 때 Link 컴포넌트를 사용하면 null을 반환하는 캐치 올 라우트를 사용합니다.

```js
import Link from 'next/link'

export function Modal({ children }: { children: React.ReactNode }) {
  return (
    <>
      <Link href="/">모달 닫기</Link>
      <div>{children}</div>
    </>
  )
}
```

<div class="content-ad"></div>

```js
export default function CatchAll() {
  return null
}
```

> 좋아 알아두세요:
우리는 @auth 슬롯 내의 캐치 올 라우트를 사용하여 액티브 상태 및 내비게이션에서 설명된 동작으로 인해 모달을 닫습니다. 클라이언트 측에서 슬롯과 일치하지 않는 라우트로의 내비게이션은 계속 표시되므로, 모달을 닫기 위해 null을 반환하는 라우트와 슬롯을 일치시키어야 합니다.
다른 예시로는 갤러리에서 사진 모달을 열면서 전용 /photo/[id] 페이지도 가지고 있거나, 쇼핑 카트를 측면 모달에서 열 수도 있습니다.
인터셉트된 및 병렬 라우트가 있는 모달의 예시를 확인해보세요.

### 로딩 및 에러 UI

병렬 라우트는 독립적으로 스트리밍될 수 있어, 각 라우트에 대해 독립적인 에러 및 로딩 상태를 정의할 수 있습니다.

<div class="content-ad"></div>

안녕하세요! 테이블 태그를 마크다운 형식으로 변경해주세요.