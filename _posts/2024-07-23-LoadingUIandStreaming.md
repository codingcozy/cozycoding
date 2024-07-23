---
title: "Nextjs 14에서 App Router를 이용한 로딩 UI와 스트리밍 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:06
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Loading UI and Streaming"
link: "https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming"
---


# UI 및 스트리밍 로딩

특별한 파일 loading.js를 사용하면 React Suspense와 함께 의미 있는 로딩 UI를 쉽게 만들 수 있습니다. 이 규칙을 사용하면 라우트 세그먼트의 내용을 로드하는 동안 서버에서 즉시 로딩 상태를 보여줄 수 있습니다. 렌더링이 완료되면 새로운 콘텐츠가 자동으로 교체됩니다.

![로딩 UI 및 스트리밍](/assets/img/2024-07-23-LoadingUIandStreaming_0.png)

## 즉시 로딩 상태

<div class="content-ad"></div>

인스턴트 로딩 상태는 네비게이션 직후에 즉시 표시되는 대비 UI입니다. 스켈레톤 및 스피너와 같은 로딩 표시기 또는 향후 화면 중 일부인 커버 사진, 제목 등을 미리 렌더링할 수 있습니다. 이를 통해 사용자가 앱이 응답 중임을 이해하고 더 나은 사용자 경험을 제공할 수 있습니다.

폴더 내부에 loading.js 파일을 추가하여 로딩 상태를 만들어보세요.

```js
export default function Loading() {
  // 로딩 내부에 스켈레톤을 비롯한 모든 UI를 추가할 수 있습니다.
  return <LoadingSkeleton />
}
```

<div class="content-ad"></div>

동일한 폴더에 있는 loading.js가 layout.js 안에 중첩될 것입니다. 이 파일은 자동으로 page.js 파일과 하단의 모든 하위 항목을 `Suspense` 경계로 둘러싸게 됩니다.

![이미지](/assets/img/2024-07-23-LoadingUIandStreaming_2.png)

> 알아 두면 좋은 점:
- 서버 중심의 경로 지정을 사용해도 탐색은 즉시 이루어집니다.
- 탐색은 중단 가능하며, 새로운 경로로 전환하기 전에 경로 내용을 완전히 로드할 필요가 없습니다.
- 공유 레이아웃은 새 경로 세그먼트를 로드하는 동안에도 상호 작용이 가능합니다.

> 권장 사항: Next.js가 이 기능을 최적화하기 때문에 route 세그먼트(레이아웃 및 페이지)에 대해 loading.js 규칙을 사용하세요.

<div class="content-ad"></div>

## 스트리밍과 Suspense

loading.js 외에도, 자체 UI 컴포넌트를 위해 Suspense 경계를 수동으로 생성할 수 있습니다. 앱 라우터는 Node.js 및 Edge 런타임을 위한 스트리밍을 Suspense와 함께 지원합니다.

> 알아두면 좋은 사항:
일부 브라우저는 스트리밍 응답을 버퍼링합니다. 1024바이트가 넘어가야만 스트리밍 응답이 표시될 수 있습니다. 이는 일반적으로 "안녕, 세계" 애플리케이션에만 영향을 미치며, 실제 애플리케이션에는 영향을 미치지 않습니다.

### 스트리밍이란 무엇인가요?

<div class="content-ad"></div>

React 및 Next.js에서 스트리밍이 작동하는 방법을 이해하려면 서버 측 렌더링(SSR) 및 그 제한 사항을 이해하는 것이 도움이 됩니다.

SSR을 사용하면 사용자가 페이지를 볼 수 있고 상호 작용할 수 있기 전에 완료해야 할 일련의 단계가 있습니다:

- 먼저, 특정 페이지의 모든 데이터가 서버에서 가져와집니다.
- 그런 다음 서버에서 페이지의 HTML을 렌더링합니다.
- 페이지의 HTML, CSS 및 JavaScript가 클라이언트로 전송됩니다.
- 생성된 HTML 및 CSS를 사용하여 비대화식 사용자 인터페이스가 표시됩니다.
- 마지막으로 React가 사용자 인터페이스를 상호 작용할 수 있도록 만들기 위해 적용됩니다.

<img src="/assets/img/2024-07-23-LoadingUIandStreaming_3.png" />

<div class="content-ad"></div>

이러한 단계들은 순차적이고 블로킹되어 있습니다. 이는 서버가 페이지의 HTML을 렌더링할 때 모든 데이터를 가져온 후에 가능합니다. 또한 클라이언트에서 React는 페이지의 모든 구성 요소가 다운로드된 후에 UI를 주입할 수 있습니다.

React 및 Next.js를 사용한 SSR은 사용자에게 상호 작용이 불가능한 페이지를 가능한 빨리 표시하여 지각된 로딩 성능을 향상시킵니다.

<img src="/assets/img/2024-07-23-LoadingUIandStreaming_4.png" />

그러나 사용자에게 페이지를 표시하기 전에 서버에서 모든 데이터 가져오기가 완료되어야 하므로 여전히 느릴 수 있습니다.

<div class="content-ad"></div>

스트리밍을 통해 페이지의 HTML을 작은 조각으로 나누고 서버에서 클라이언트로 점진적으로 보낼 수 있습니다.

![이미지](/assets/img/2024-07-23-LoadingUIandStreaming_5.png)

이를 통해 페이지의 일부가 더 빨리 표시되어 모든 데이터가 로드될 때까지 기다릴 필요 없이 UI가 렌더링될 수 있습니다.

스트리밍은 React의 컴포넌트 모델과 잘 맞습니다. 각 컴포넌트는 하나의 조각으로 생각할 수 있습니다. 우선 순위가 높은 컴포넌트(예: 제품 정보)나 데이터에 의존하지 않는 컴포넌트(예: 레이아웃)를 먼저 보낼 수 있고 React는 조기 수화를 시작할 수 있습니다. 우선 순위가 낮은 컴포넌트(예: 리뷰, 관련 제품)는 데이터를 가져온 후 동일한 서버 요청에서 보내질 수 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-23-LoadingUIandStreaming_6.png" />

스트리밍은 페이지 렌더링을 차단하는 긴 데이터 요청을 방지하고 Time To First Byte (TTFB) 및 First Contentful Paint (FCP)을 줄일 수 있어 특히 유용합니다. 또한 느린 기기에서 특히 Time to Interactive (TTI)를 개선하는 데 도움이 됩니다.

### 예시

`Suspense`는 비동기 작업(예: 데이터 가져오기)을 수행하는 구성 요소를 래핑하여 과정이 진행되는 동안 대체 UI(예: 스켈레톤, 스피너)를 표시하고 동작이 완료되면 구성 요소를 교체하는 방식으로 작동합니다.

<div class="content-ad"></div>

```js
import { Suspense } from 'react'
import { PostFeed, Weather } from './Components'
 
export default function Posts() {
  return (
    <section>
      <Suspense fallback={<p>Loading feed...</p>}>
        <PostFeed />
      </Suspense>
      <Suspense fallback={<p>Loading weather...</p>}>
        <Weather />
      </Suspense>
    </section>
  )
}
```

Suspense를 사용하면 다음과 같은 이점을 얻을 수 있습니다:

- Streaming Server Rendering - 서버에서 클라이언트로 HTML을 점진적으로 렌더링합니다.
- Selective Hydration - React는 사용자 상호작용에 따라 먼저 어떤 컴포넌트를 상호작용 가능하게 만들지 우선순위를 정합니다.

더 많은 Suspense 예제와 사용 사례는 React 문서를 참조해주세요.


<div class="content-ad"></div>

### SEO

- Next.js는 generateMetadata 내부의 데이터 가져오기가 완료될 때까지 클라이언트로 UI를 스트리밍하는 것을 기다립니다. 이는 스트리밍된 응답의 첫 부분이 `head` 태그를 포함하도록 보장합니다.
- 스트리밍이 서버 측 렌더링되기 때문에 SEO에 영향을 미치지 않습니다. Google의 Rich Results Test 도구를 사용하여 페이지가 Google의 웹 크롤러에게 어떻게 표시되는지 확인하고 직렬화된 HTML을 볼 수 있습니다.

### 상태 코드

스트리밍 중에는 요청이 성공적이었음을 나타내는 200 상태 코드가 반환됩니다.

<div class="content-ad"></div>

서버는 여전히 스트리밍된 내용 그 자체 내에서 오류 또는 문제를 클라이언트에게 전할 수 있습니다. 예를 들어, 리디렉션 또는 찾을 수 없음을 사용할 때입니다. 응답 헤더가 이미 클라이언트에게 전송되었기 때문에 응답의 상태 코드를 업데이트할 수 없습니다. 이는 SEO에 영향을 미치지 않습니다.