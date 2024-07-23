---
title: "Nextjs 14에서 Edge와 Nodejs 런타임 비교 선택 가이드"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:23
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Edge and Node.js Runtimes"
link: "https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes"
---


# Edge와 Node.js 런타임

Next.js의 맥락에서 런타임이란 실행 중에 코드에서 사용 가능한 라이브러리, API 및 일반 기능 집합을 의미합니다.

서버에서는 애플리케이션 코드의 일부를 렌더링할 수 있는 두 가지 런타임이 있습니다:

- Node.js 런타임(기본값)은 Node.js API 및 해당 생태계에서 호환되는 패키지에 대한 액세스 권한이 있습니다.
- Edge 런타임은 Web API를 기반으로 합니다.

<div class="content-ad"></div>

## 런타임 차이점

런타임을 선택할 때 고려해야 할 사항이 많이 있습니다. 이 표는 주요 차이점을 한눈에 보여줍니다. 차이를 더 자세히 분석하고 싶다면 아래 섹션을 확인해보세요.


|                    | Node   | Serverless | Edge     |
|------------------- |------- |------------|----------|
| Cold Boot          | /      | Normal     | Low      |
| [HTTP Streaming](/docs/app/building-your-application/routing/loading-ui-and-streaming) | Yes    | Yes        | Yes      |
| IO                 | All    | All        | `fetch`  |
| Scalability        | /      | High       | Highest  |
| Security           | Normal | High       | High     |
| Latency            | Normal | Low        | Lowest   |
| npm Packages       | All    | All        | A smaller subset |
| [Static Rendering](/docs/app/building-your-application/rendering/server-components#static-rendering-default) | Yes | Yes        | No       |
| [Dynamic Rendering](/docs/app/building-your-application/rendering/server-components#dynamic-rendering)   | Yes | Yes        | Yes      |
| [Data Revalidation w/ `fetch`](/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#revalidating-data) | Yes | Yes     | Yes      |


### Edge Runtime

<div class="content-ad"></div>

Next.js에서는 경량 Edge 런타임이 사용 가능한 Node.js API의 하위 집합입니다.

Edge 런타임은 소량의 간단한 함수로 저렴한 대응 속도로 동적이고 개인화된 콘텐츠를 제공해야 하는 경우 이상적입니다. Edge 런타임의 속도는 자원의 최소한 사용으로부터 나오지만, 이는 많은 시나리오에서 제한적일 수 있습니다.

예를 들어, Vercel의 Edge 런타임에서 실행되는 코드는 1MB와 4MB 사이로 제한되어야 합니다. 이 한계에는 가져온 패키지, 글꼴, 파일이 포함되며, 배포 인프라에 따라 달라질 수 있습니다. 또한, Edge 런타임은 모든 Node.js API를 지원하지 않기 때문에 일부 npm 패키지가 작동하지 않을 수 있습니다. 예를 들어, "Module not found: Can't resolve 'fs'"와 같은 오류가 발생할 수 있습니다. 이러한 API나 패키지를 사용해야 하는 경우 Node.js 런타임을 사용하는 것을 권장합니다.

### Node.js 런타임

<div class="content-ad"></div>

Node.js 런타임을 사용하면 모든 Node.js API에 액세스할 수 있고, 그에 의존하는 모든 npm 패키지에 접근할 수 있습니다. 그러나 Edge 런타임을 사용하는 라우트보다 시작 속도가 느릴 수 있습니다.

Next.js 애플리케이션을 Node.js 서버에 배포하려면 인프라를 관리, 확장 및 구성해야 합니다. 또는 Vercel과 같은 서버리스 플랫폼에 Next.js 애플리케이션을 배포해도 됩니다. 해당 플랫폼이 이를 처리해줍니다.

### 서버리스 Node.js

서버리스는 Edge 런타임보다 더 복잡한 계산 부하를 처리할 수 있는 확장 가능한 솔루션이 필요할 때 이상적입니다. 예를 들어 Vercel의 서버리스 함수를 사용하면 가져온 패키지, 글꼴 및 파일을 포함한 전체 코드 크기가 50MB이 됩니다.

<div class="content-ad"></div>

The Edge를 사용하는 라우트와 비교했을 때의 단점은 Serverless Functions가 요청 처리를 시작하기 전에 부팅하는 데 수백 밀리초가 걸릴 수 있다는 것입니다. 사이트가 받는 트래픽 양에 따라, 이는 함수가 자주 "워밍 업"되지 않기 때문에 빈번한 발생이 될 수 있습니다.

## 예시

### 세그먼트 런타임 옵션

Next.js 애플리케이션에서 개별 라우트 세그먼트에 런타임을 지정할 수 있습니다. 이를 위해 runtime이라는 변수를 선언하고 내보내면 됩니다. 이 변수는 문자열이어야 하며 'nodejs' 또는 'edge' 런타임의 값이어야 합니다.

<div class="content-ad"></div>

다음 예제는 값이 `edge`인 런타임을 내보내는 페이지 경로 세그먼트를 보여줍니다:

```js
export const runtime = 'edge' // 'nodejs' (기본) | 'edge'
```

또한 레이아웃 수준에서 런타임을 정의할 수도 있습니다. 이는 레이아웃 아래의 모든 경로가 엣지 런타임에서 실행되도록 만듭니다:

```js
export const runtime = 'edge' // 'nodejs' (기본) | 'edge'
```

<div class="content-ad"></div>

세그먼트 런타임이 설정되지 않은 경우 기본적으로 Node.js 런타임이 사용됩니다. Node.js 런타임에서 변경할 계획이 없다면 런타임 옵션을 사용할 필요가 없습니다.

> Node.js Docs 및 Edge Docs를 참조하여 사용 가능한 API 전체 목록을 확인해주세요. 두 런타임은 배포 인프라에 따라 스트리밍도 지원할 수 있습니다.