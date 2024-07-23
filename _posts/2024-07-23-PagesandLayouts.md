---
title: "Nextjs 14 Pages와 Layouts 관리하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:04
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Pages and Layouts"
link: "https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts"
---


# 페이지와 레이아웃

> 계속하기 전에 라우팅 기본 사항 및 라우트 정의 페이지를 읽어보는 것을 권장합니다.

layout.js, page.js 및 template.js와 같은 특별한 파일을 사용하면 경로에 대한 UI를 만들 수 있습니다. 이 페이지에서는 이러한 특별한 파일을 어떻게 그리고 언제 사용해야 하는지에 대한 안내를 제공합니다.

## 페이지

<div class="content-ad"></div>

페이지는 경로별로 고유한 UI입니다. 페이지는 page.js 파일에서 컴포넌트를 내보내어 정의할 수 있습니다.

예를 들어, index 페이지를 만들려면 앱 디렉토리에 page.js 파일을 추가하십시오:

![이미지](/assets/img/2024-07-23-PagesandLayouts_0.png)

```js
// `app/page.tsx`는 `/` URL의 UI입니다
export default function Page() {
  return <h1>안녕하세요, 홈페이지!</h1>
}
```

<div class="content-ad"></div>

그럼 페이지를 더 만들려면 새 폴더를 만들고 그 안에 page.js 파일을 추가하면 됩니다. 예를 들어, /dashboard 경로용 페이지를 만들기 위해 dashboard라는 새 폴더를 만들고 그 안에 page.js 파일을 추가하세요:

```javascript
// `app/dashboard/page.tsx`는 `/dashboard` URL의 UI이며
export default function Page() {
  return <h1>Hello, Dashboard Page!</h1>
}
```

> 알아 두면 좋은 사항:
.js, .jsx 또는 .tsx 파일 확장자를 페이지에 사용할 수 있습니다.
페이지는 항상 라우트 하위 트리의 잎입니다.
라우트 세그먼트를 공개적으로 접근 가능하게 만들려면 page.js 파일이 필요합니다.
기본적으로 페이지는 서버 구성 요소(Server Components)이지만 클라이언트 구성 요소(Client Component)로 설정할 수도 있습니다.
페이지는 데이터를 가져올 수 있습니다. 자세한 정보는 데이터 가져오기 섹션을 참조하세요.

## 레이아웃(Layouts)

<div class="content-ad"></div>

여러 경로 간에 공유되는 UI 레이아웃이란 것이 있어요. 이동할 때 UI 레이아웃은 상태를 보존하고 상호 작용 가능한 상태를 유지하며 다시 렌더링되지 않아요. 레이아웃은 중첩될 수도 있어요.

레이아웃은 layout.js 파일에서 React 컴포넌트를 기본적으로 내보내어 정의할 수 있어요. 해당 컴포넌트는 렌더링 중에 하위 레이아웃(있을 경우) 또는 페이지로 채워지는 children 속성을 받아들여야 해요.

예를 들어, 레이아웃은 /dashboard 및 /dashboard/settings 페이지와 공유될 거예요:

![사진](/assets/img/2024-07-23-PagesandLayouts_1.png)

<div class="content-ad"></div>

```js
export default function DashboardLayout({
  children, // 페이지 또는 중첩된 레이아웃입니다
}: {
  children: React.ReactNode
}) {
  return (
    <section>
      {/* 공유 UI를 여기에 포함하십시오 예: 헤더 또는 사이드바 */}
      <nav></nav>
 
      {children}
    </section>
  )
}
```

### 루트 레이아웃 (필수)

루트 레이아웃은 앱 디렉토리의 최상위에서 정의되며 모든 경로에 적용됩니다. 이 레이아웃은 필수이며 서버에서 반환된 초기 HTML을 수정할 수 있도록 html과 body 태그를 포함해야 합니다.

```js
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        {/* 레이아웃 UI */}
        <main>{children}</main>
      </body>
    </html>
  )
}
```

<div class="content-ad"></div>

### 레이아웃 중첩

기본적으로 폴더 계층 구조에서 레이아웃은 중첩되어 있습니다. 즉, 자식 레이아웃을 children 속성을 통해 감쌀 수 있습니다. layout.js 파일을 특정 라우트 세그먼트(폴더) 안에 추가하여 레이아웃을 중첩할 수 있습니다.

예를 들어 /dashboard 라우트에 대한 레이아웃을 만들려면 dashboard 폴더 안에 새로운 layout.js 파일을 추가하면 됩니다:

![이미지](/assets/img/2024-07-23-PagesandLayouts_2.png)

<div class="content-ad"></div>

```js
export default function 대시보드_레이아웃({
  children,
}: {
  children: React.ReactNode
}) {
  return <section>{children}</section>
}
```

위의 두 레이아웃을 결합한다면, 루트 레이아웃(app/layout.js)이 대시보드 레이아웃(app/dashboard/layout.js)을 둘러싸고, 대시보드 레이아웃이 app/dashboard/* 안의 라우트 세그먼트를 둘러싸게 될 것입니다.

이 두 레이아웃은 다음과 같이 중첩될 것입니다:

<img src="/assets/img/2024-07-23-PagesandLayouts_3.png" /> 


<div class="content-ad"></div>

> 좋아 알아두기:
.js, .jsx 또는 .tsx 파일 확장자를 사용하여 레이아웃을 만들 수 있어요.
루트 레이아웃만 `html`과 `body` 태그를 포함해야 해요.
동일한 폴더에 layout.js 및 page.js 파일이 정의되어 있을 때, 레이아웃이 페이지를 감싸요.
레이아웃은 기본적으로 서버 구성요소이지만 클라이언트 구성요소로 설정할 수도 있어요.
레이아웃은 데이터도 가져올 수 있어요. 더 많은 정보는 데이터 가져오기 섹션을 확인해주세요.
부모 레이아웃과 해당 자식 간에 데이터를 전달하는 것은 불가능할 거예요. 그러나 같은 데이터를 경로에서 여러 번 가져올 수 있고, React가 자동으로 중복 요청을 처리하여 성능에 영향을 주지 않아요.
레이아웃은 자신 아래의 경로 세그먼트에 액세스할 수 없어요. 모든 경로 세그먼트에 액세스하려면 클라이언트 구성요소에서 useSelectedLayoutSegment 또는 useSelectedLayoutSegments를 사용할 수 있어요.
Route Groups를 사용하여 공유 레이아웃에 특정 경로 세그먼트를 선택적으로 추가하거나 제외할 수 있어요.
Route Groups를 사용하여 여러 루트 레이아웃을 생성할 수 있어요. 여기에서 예제를 확인하세요.
페이지 디렉토리에서 마이그레이션: 루트 레이아웃이 _app.js 및 _document.js 파일을 대체해요. 마이그레이션 가이드를 확인해주세요.

## 템플릿

템플릿은 각 각의 자식 레이아웃이나 페이지를 감싸는 레이아웃과 유사해요. 경로 간에 지속되고 상태가 유지되는 레이아웃과는 달리, 템플릿은 이동 시 각각의 자식에 대해 새 인스턴스를 생성해요. 이는 일반 레이아웃을 공유하는 경로 간에 사용자가 탐색할 때 해당 컴포넌트의 새 인스턴스가 마운트되고 DOM 요소가 다시 생성되며 상태가 유지되지 않고 효과가 다시 동기화된다는 의미에요.

그 특정 동작이 필요한 경우가 있을 수 있고, 그럴 때는 레이아웃보다는 템플릿이 더 적합할 수 있어요. 예를 들어:

<div class="content-ad"></div>

- useEffect에 의존하는 기능(예: 페이지 뷰 로깅) 및 useState에 의존하는 기능(예: 페이지별 피드백 양식).
- 기본 프레임워크 동작을 변경하기 위해 사용됩니다. 예를 들어, 레이아웃 내에 있는 Suspense Boundaries는 레이아웃이 처음로드될 때만 대기 중 표시하고 페이지를 전환할 때에는 표시하지 않습니다. 템플릿의 경우, 대기 중이 모든 탐색마다 표시됩니다.

템플릿은 template.js 파일에서 기본 React 컴포넌트를 내보내어 정의할 수 있습니다. 컴포넌트는 children 속성을 받아야 합니다.

![image](/assets/img/2024-07-23-PagesandLayouts_4.png)

```jsx
export default function Template({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>
}
```

<div class="content-ad"></div>

중첩 관련해서, template.js는 레이아웃과 그 자식들 사이에 렌더링됩니다. 여기에 간단한 출력이 있어요:

```js
<Layout>
  {/* 템플릿이 고유한 키를 갖도록 주목하세요. */}
  <Template key={routeParam}>{children}</Template>
</Layout>
```

## 메타데이터

앱 디렉터리에서 Metadata API를 사용하여 제목과 메타처럼 `head` HTML 요소를 수정할 수 있어요.

<div class="content-ad"></div>

메타데이터는 layout.js 또는 page.js 파일에서 메타데이터 객체를 내보내거나 generateMetadata 함수를 정의하여 정의할 수 있습니다.

```js
import { Metadata } from 'next'
 
export const metadata: Metadata = {
  title: 'Next.js',
}
 
export default function Page() {
  return '...'
}
```

> 알아두면 좋아요: `head` 및 `title`, `meta`와 같은 `head` 태그를 루트 레이아웃에 직접 추가하면 안 됩니다. 대신 Metadata API를 사용해야 합니다. Metadata API는 `head` 요소의 고급 요구 사항을 자동으로 처리하여 스트리밍 및 de-duplicating합니다.

API 참조에서 사용 가능한 메타데이터 옵션에 대해 자세히 알아보세요.