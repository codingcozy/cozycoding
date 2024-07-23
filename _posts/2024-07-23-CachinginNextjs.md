---
title: "Nextjs 14에서 캐싱 최적화하는 방법 App Router 활용하기"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:24
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Caching in Next.js"
link: "https://nextjs.org/docs/app/building-your-application/caching"
---


# Next.js에서의 캐싱

Next.js는 렌더링 작업과 데이터 요청을 캐싱하여 응용 프로그램의 성능을 향상시키고 비용을 절감하는데 도움을 줍니다. 이 페이지는 Next.js 캐싱 메커니즘을 자세히 살펴보고, 구성하는 데 사용할 수 있는 API 및 상호 작용하는 방법에 대해 제공합니다.

> 알아 두면 좋아요: 이 페이지는 Next.js의 내부 작동 방식을 이해하는 데 도움이 되지만, Next.js를 사용하여 생산성을 높이는 데 필수적인 지식은 아닙니다. 대부분의 Next.js 캐싱 휴리스틱은 API 사용에 따라 결정되며, 최상의 성능을 위해 제로 또는 최소한의 구성을 사용합니다.

## 개요

<div class="content-ad"></div>

여기에는 다양한 캐싱 메커니즘 및 그 목적에 대한 고수준 개요가 있어요:

| 메커니즘 | 무엇에 사용되는가 | 어디에 사용되는가 | 목적 | 유지 기간 |
|---------|----------------------|--------------------|------|------------|
| [Request Memoization](#request-memoization) | 함수의 반환 값 | 서버 | React 컴포넌트 트리에서 데이터 재사용 | 각 요청 주기동안 |
| [Data Cache](#data-cache) | 데이터 | 서버 | 사용자 요청 및 배포 전체에서 데이터 저장 | 지속적 (재확인 가능) |
| [Full Route Cache](#full-route-cache) | HTML 및 RSC 페이로드 | 서버 | 렌더링 비용 절감 및 성능 향상 | 지속적 (재확인 가능) |
| [Router Cache](#router-cache) | RSC 페이로드 | 클라이언트 | 네비게이션 시 서버 요청 감소 | 사용자 세션 또는 시간 기반 |

기본적으로 Next.js는 성능 향상과 비용 절감을 위해 가능한 한 많이 캐시하게 됩니다. 이는 경로가 정적으로 렌더링되며 데이터 요청이 캐시되어 옵트 아웃하지 않는 한 데이터가 저장된다는 의미입니다. 다음 다이어그램은 기본 캐싱 동작을 보여줍니다: 라우트가 빌드 시 정적으로 렌더링되며 정적 경로가 처음 방문되는 경우입니다.

<img src="/assets/img/2024-07-23-CachinginNextjs_0.png" />

<div class="content-ad"></div>

경로가 정적으로 또는 동적으로 렌더링되는 경우, 데이터가 캐시되었는지 아닌지, 요청이 초기 방문인지 후속 탐색인지에 따라 캐싱 동작이 변경됩니다. 사용 사례에 따라 개별 경로 및 데이터 요청에 대한 캐싱 동작을 구성할 수 있습니다.

## 요청 메모이제이션

React는 동일한 URL 및 옵션을 가진 요청을 자동으로 메모이제이션하는 방식으로 fetch API를 확장합니다. 따라서 React 컴포넌트 트리의 여러 위치에서 동일한 데이터를 호출할 수 있지만 한 번만 실행할 수 있습니다.

<img src="/assets/img/2024-07-23-CachinginNextjs_1.png" />

<div class="content-ad"></div>

예를 들어, 같은 데이터를 경유지(예: 레이아웃, 페이지 및 여러 컴포넌트) 전반에 걸쳐 사용해야 할 경우, 데이터를 트리 상단에서 가져오고 컴포넌트 간에 props를 전달할 필요가 없습니다. 대신, 동일한 데이터에 대해 네트워크를 통해 여러 요청을 생성하는 성능 문제를 걱정하지 않고 필요한 컴포넌트에서 데이터를 가져올 수 있습니다.

```js
async function getItem() {
  // `fetch` 함수는 자동으로 메모이제이션이 되며 결과는 캐시됩니다
  const res = await fetch('https://.../item/1')
  return res.json()
}
 
// 이 함수는 두 번 호출되지만, 첫 번째 호출 때만 실행됩니다
const item = await getItem() // 캐시 미스
 
// 두 번째 호출은 전체 루트 내 어디에서든 일어날 수 있습니다
const item = await getItem() // 캐시 힛
```

요청 메모이제이션 작동 방식

![Caching in Next.js](/assets/img/2024-07-23-CachinginNextjs_2.png)

<div class="content-ad"></div>

- 라우트를 렌더링하는 동안 처음으로 특정 요청이 발생하면 결과가 메모리에 없어서 캐시 MISS가 될 것입니다.
- 따라서 함수가 실행되고, 데이터가 외부 소스에서 가져와지며, 결과가 메모리에 저장될 것입니다.
- 동일한 렌더 패스에서 해당 요청에 대한 이후 함수 호출은 캐시 HIT이 되어 함수를 실행하지 않고 메모리에서 데이터를 반환합니다.
- 라우트가 렌더링되고 렌더링 패스가 완료되면 메모리가 "재설정"되며 모든 요청 메모화 항목이 지워집니다.

> 알아두면 좋아요:
요청 메모화는 Next.js 기능이 아니라 React 기능입니다. 이것은 다름 캐싱 메커니즘들과 어떻게 상호작용하는지 보여주기 위해 여기에 포함되었습니다.
메모화는 fetch 요청의 GET 메서드에만 적용됩니다.
메모화는 React 컴포넌트 트리에만 적용되며, 즉:
generateMetadata, generateStaticParams, Layouts, Pages, 그리고 다른 서버 컴포넌트의 fetch 요청에 적용됩니다.
Route Handlers의 fetch 요청에는 적용되지 않습니다. 왜냐하면 그것들은 React 컴포넌트 트리의 일부가 아니기 때문입니다.
fetch가 적합하지 않은 경우 (예: 일부 데이터베이스 클라이언트, CMS 클라이언트 또는 GraphQL 클라이언트 등), 함수 메모화를 위해 React 캐시 함수를 사용할 수 있습니다.

### 기간

캐시는 서버 요청의 수명 동안 유지되며 React 컴포넌트 트리가 렌더링을 완료할 때까지 유지됩니다.

<div class="content-ad"></div>

### 재검증

메모이제이션은 서버 요청 간에 공유되지 않고 렌더링 중에만 적용되기 때문에 다시 유효성을 검사할 필요가 없습니다.

### 선택 사항 해제

메모이제이션은 페치 요청의 GET 메서드에만 적용되며, POST 및 DELETE와 같은 다른 메서드는 메모이제이션되지 않습니다. 이 기본 동작은 React의 최적화이며 우리는 이를 선택적으로 해제하는 것을 권장하지 않습니다.

<div class="content-ad"></div>

개별 요청을 관리하려면 AbortController의 signal 속성을 사용할 수 있습니다. 그러나 이는 요청을 메모리에 저장하지 않고 오히려 처리 중인 요청을 중단합니다.

```js
const { signal } = new AbortController()
fetch(url, { signal })
```

## 데이터 캐시

Next.js에는 내장된 데이터 캐시가 있습니다. 이는 데이터 가져오기의 결과를 수신 서버 요청 및 배포 사이에서 유지할 수 있도록 합니다. 이는 Next.js가 기본 fetch API를 확장하여 서버의 각 요청이 자체 영구 캐싱 의미론을 설정할 수 있도록 가능해집니다.

<div class="content-ad"></div>

> 좋은 정보입니다: 브라우저에서 fetch의 캐시 옵션은 요청이 브라우저의 HTTP 캐시와 상호 작용하는 방식을 나타내며, Next.js에서는 캐시 옵션이 서버 측 요청이 서버의 데이터 캐시와 상호 작용하는 방식을 나타냅니다.

기본적으로 fetch를 사용하는 데이터 요청은 캐시됩니다. 캐싱 동작을 구성하기 위해 fetch의 cache 및 next.revalidate 옵션을 사용할 수 있습니다.

데이터 캐시 동작 방법

![이미지](/assets/img/2024-07-23-CachinginNextjs_3.png)

<div class="content-ad"></div>

- 렌더링 중에 fetch 요청이 처음 호출될 때, Next.js는 캐시된 응답을 확인하기 위해 데이터 캐시를 확인합니다.
- 캐시된 응답이 발견되면 즉시 반환되고 메모이즈됩니다.
- 캐시된 응답이 발견되지 않으면 요청이 데이터 원본에 전달되어 결과가 데이터 캐시에 저장되고 메모이즈됩니다.
- 캐시되지 않은 데이터(' cache: `no-store` '와 같은)의 경우, 항상 데이터 원본에서 가져오고 메모이즈됩니다.
- 데이터가 캐시되었든 캐시되지 않았든, 요청은 항상 메모이즈되어 React 렌더링 중에 동일한 데이터에 대해 중복 요청을 하지 않습니다.

> 데이터 캐시와 요청 메모이제이션의 차이점
두 캐싱 메커니즘이 모두 성능을 향상시키는 데 도움이 되지만, 데이터 캐시는 들어오는 요청과 배포 사이에 지속되지만 메모이제이션은 요청 수명 동안에만 지속됩니다.
메모이제이션을 통해, 동일한 렌더링 중에 네트워크 경계를 통해 데이터 캐시 서버(예: CDN 또는 엣지 네트워크) 또는 데이터 원본(예: 데이터베이스 또는 CMS)로 건너가는 중복 요청 수를 줄일 수 있습니다. 데이터 캐시를 사용하면 원본 데이터 소스로의 요청 횟수를 줄일 수 있습니다.

### 지속 시간

데이터 캐시는 들어오는 요청과 배포 간에 지속되지만 재검증하거나 선택적으로 제외하지 않는 한 메모이즈는 요청 수명 동안에만 지속됩니다.

<div class="content-ad"></div>

### 다시 유효성 검사

캐시된 데이터를 두 가지 방법으로 다시 유효성을 검사할 수 있습니다:

- 시간 기반 유효성 검사: 일정 시간이 경과하고 새 요청이 들어왔을 때 데이터를 재검사합니다. 이는 드물게 변경되는 데이터에 유용하며 신선도가 크게 중요하지 않을 때 사용됩니다.
- 요청 기반 유효성 검사: 이벤트(예: 양식 제출)를 기반으로 데이터를 검사합니다. 요청 기반 유효성 검사는 한 번에 데이터 그룹을 다시 유효성 검사할 수 있도록 태그 또는 경로를 사용할 수 있습니다. 이는 가능한 빨리 최신 데이터가 표시되도록 보장하고 싶을 때 유용합니다(예: 헤들리스 CMS의 콘텐츠가 업데이트된 경우).

#### 시간 기반 유효성 검사

<div class="content-ad"></div>

특정 시간 간격마다 데이터를 재검증하려면, fetch의 next.revalidate 옵션을 사용하여 리소스의 캐시 수명(초 단위)을 설정할 수 있습니다.

```js
// 1시간마다 재검증
fetch('https://...', { next: { revalidate: 3600 } })
```

다른 방법으로는 Route Segment Config 옵션을 사용하여 세그먼트의 모든 fetch 요청을 구성하거나 fetch를 사용할 수 없는 경우에 대비할 수 있습니다.

타임 기반 재검증 작동 방식

<div class="content-ad"></div>

<img src="/assets/img/2024-07-23-CachinginNextjs_4.png" />

- revalidate가 포함된 fetch 요청이 처음 호출될 때, 데이터는 외부 데이터 소스에서 가져와서 데이터 캐시에 저장됩니다.
- 지정된 시간 간겯(예: 60초) 내에 호출된 모든 요청은 캐시된 데이터를 반환합니다.
- 시간 간격이 끝나면, 다음 요청은 여전히 캐시된 (지금은 오래된) 데이터를 반환합니다.
Next.js는 데이터를 백그라운드에서 재검증하도록 트리거합니다.
데이터가 성공적으로 가져온 후, Next.js는 최신 데이터로 데이터 캐시를 업데이트합니다.
백그라운드 재검증이 실패하면, 이전 데이터는 변경되지 않은 채 유지될 것입니다.
- Next.js는 데이터를 백그라운드에서 재검증하도록 트리거합니다.
- 데이터가 성공적으로 가져온 후, Next.js는 최신 데이터로 데이터 캐시를 업데이트합니다.
- 백그라운드 재검증이 실패하면, 이전 데이터는 변경되지 않은 채 유지될 것입니다.

이것은 stale-while-revalidate 동작과 유사합니다.

#### 요청에 따른 재검증

<div class="content-ad"></div>

데이터는 경로(revalidatePath) 또는 캐시 태그(revalidateTag)에 의해 필요할 때 다시 유효성을 검사할 수 있습니다.

어떻게 필요할 때 유효성을 다시 검사하는지

![이미지](/assets/img/2024-07-23-CachinginNextjs_5.png)

- 처음으로 요청이 발생할 때, 데이터는 외부 데이터 소스에서 가져와 데이터 캐시에 저장됩니다.
- 필요할 때 유효성을 다시 검사하면, 적절한 캐시 항목이 캐시에서 제거됩니다.
- 이는 시간 기반 유효성 검사와는 다릅니다. 시간 기반 유효성 검사는 신선한 데이터가 가져올 때까지 캐시에 낡은 데이터를 보관합니다.
- 다음 요청 시, 다시 캐시 MISS가 되고, 데이터는 외부 데이터 소스에서 가져와 데이터 캐시에 저장됩니다.

<div class="content-ad"></div>

### 캐싱 거부

개별 데이터 가져오기에 대해 캐싱을 거부하려면, 캐시 옵션을 no-store로 설정하면 됩니다. 이는 데이터가 fetch를 호출할 때마다 가져올 수 있음을 의미합니다.

```js
// 개별 `fetch` 요청에 대해 캐싱 거부
fetch(`https://...`, { cache: 'no-store' })
```

또는 라우트 세그먼트 구성 옵션을 사용하여 특정 라우트 세그먼트에 대해 캐싱 거부할 수도 있습니다. 이는 서드파티 라이브러리를 포함한 라우트 세그먼트 내의 모든 데이터 요청에 영향을 줍니다.

<div class="content-ad"></div>

```js
// 해당 경로 세그먼트의 모든 데이터 요청에 대한 캐싱을 비활성화합니다
export const dynamic = 'force-dynamic'
```

> 참고: 데이터 캐시는 현재 페이지/라우트에서만 사용할 수 있으며 미들웨어에서는 사용할 수 없습니다. 미들웨어 내부에서 수행된 모든 fetch는 기본적으로 캐시되지 않습니다.

> Vercel 데이터 캐시
만약 Next.js 애플리케이션이 Vercel에 배포되어 있다면, Vercel 특정 기능에 대한 더 나은 이해를 위해 Vercel 데이터 캐시 문서를 읽어보시기를 권장합니다.

## 전체 경로 캐시

<div class="content-ad"></div>

> 관련 용어:
자동 정적 최적화, 정적 사이트 생성 또는 정적 렌더링이라는 용어가 귀하의 애플리케이션의 경로를 생성하고 캐싱하는 프로세스를 가리키는 데 상호 교환적으로 사용될 수 있습니다.

Next.js는 빌드 시간에 자동으로 라우트를 렌더링하고 캐싱합니다. 이는 모든 요청에 대해 서버에서 렌더링하는 대신 캐시된 경로를 제공하여 페이지 로드 속도를 높일 수 있는 최적화입니다.

전체 경로 캐시 작업 방식을 이해하려면 React가 렌더링하는 방식과 Next.js가 결과를 캐싱하는 방법을 알아보는 것이 도움이 됩니다:

### 1. 서버에서의 React 렌더링

<div class="content-ad"></div>

서버에서 Next.js는 렌더링을 조정하기 위해 React의 API를 사용합니다. 렌더링 작업은 각 루트 세그먼트 및 Suspense 경계로 나뉩니다.

각 청크는 두 단계로 렌더링됩니다:

- React는 서버 컴포넌트를 스트리밍에 최적화된 특별한 데이터 형식인 React 서버 컴포넌트 페이로드로 렌더링합니다.
- Next.js는 React 서버 컴포넌트 페이로드와 클라이언트 컴포넌트 JavaScript 지시사항을 사용하여 서버에서 HTML을 렌더링합니다.

이는 모든 것이 렌더링되기 전에 작업을 캐싱하거나 응답을 보내기를 기다릴 필요가 없다는 의미입니다. 대신에 완료되는 대로 응답을 스트리밍할 수 있습니다.

<div class="content-ad"></div>

> 리액트 서버 구성 요소 페이로드는 렌더링된 리액트 서버 구성 요소 트리의 컴팩트 이진 표현입니다. 클라이언트에서 브라우저 DOM을 업데이트하는 데 사용됩니다. 리액트 서버 구성 요소 페이로드에는 다음이 포함되어 있습니다:
> - 서버 구성 요소의 렌더링 결과
> - 클라이언트 구성 요소가 렌더링되어야 하는 자리 표시자 및 해당 JavaScript 파일에 대한 참조
> - 서버 구성 요소에서 클라이언트 구성 요소로 전달된 모든 속성
> 더 자세한 정보는 서버 구성 요소 문서를 확인하세요.

### 2. 넥스트.js 서버 캐시 기능 (전체 경로 캐시)

![이미지](/assets/img/2024-07-23-CachinginNextjs_6.png)

넥스트.js의 기본 동작은 경로의 렌더링 결과(리액트 서버 구성 요소 페이로드 및 HTML)를 서버에 캐시하는 것입니다. 이는 정적으로 렌더링된 경로에 빌드 시간에 또는 다시 유효성을 검사할 때 적용됩니다.

<div class="content-ad"></div>

### 3. 클라이언트에서의 React Hydration과 Reconciliation

요청 시점에 클라이언트에서:

- HTML은 클라이언트 및 서버 구성 요소의 빠르고 대화형이 아닌 초기 미리보기를 즉시 표시하는 데 사용됩니다.
- React 서버 구성 요소 페이로드는 클라이언트 및 렌더링된 서버 구성 요소 트리를 조정하고 DOM을 업데이트하는 데 사용됩니다.
- JavaScript 지침은 클라이언트 구성 요소를 수분화하여 응용 프로그램을 상호 작용 가능하게 만드는 데 사용됩니다.

### 4. 클라이언트에서의 Next.js 캐싱 (루터 캐시)

<div class="content-ad"></div>

리액트 서버 컴포넌트 페이로드는 클라이언트 사이드 라우터 캐시에 저장됩니다. 이 캐시는 개별 경로 세그먼트로 분할되어 있습니다. 이 라우터 캐시는 이전에 방문한 경로를 저장하고 미래의 경로를 사전로딩하여 네비게이션 경험을 개선하는 데 사용됩니다.

### 5. 후속 네비게이션

후속 네비게이션 또는 사전로딩 중에 Next.js는 리액트 서버 컴포넌트 페이로드가 라우터 캐시에 저장되어 있는지 확인합니다. 그렇다면 새로운 요청을 서버에 보내지 않습니다.

캐시에 경로 세그먼트가 없다면, Next.js는 서버에서 리액트 서버 컴포넌트 페이로드를 가져와 클라이언트의 라우터 캐시를 채웁니다.

<div class="content-ad"></div>

### 정적 및 동적 렌더링

경로가 빌드 타임에 캐싱되는지 여부는 정적 또는 동적으로 렌더링되는지에 따라 달라집니다. 정적인 경로는 기본적으로 캐싱되지만 동적인 경로는 요청 시 렌더링되며 캐시되지 않습니다.

다음 다이어그램은 캐싱된 데이터와 캐싱되지 않은 데이터를 사용하여 정적으로 및 동적으로 렌더링된 경로 간의 차이를 보여줍니다:

![캐싱 다이어그램](/assets/img/2024-07-23-CachinginNextjs_7.png)

<div class="content-ad"></div>

정적 및 동적 렌더링에 대해 더 알아보세요.

### 기간

Full Route Cache는 기본적으로 영속적입니다. 이는 렌더링 결과가 사용자 요청 간에 캐싱된다는 것을 의미합니다.

### 무효화

<div class="content-ad"></div>

풀 라우트 캐시를 무효화하는 두 가지 방법이 있습니다:

- 데이터 재확인: 데이터 캐시를 재확인하면 서버에서 구성 요소를 다시 렌더링하여 새로운 렌더링 출력을 캐싱함으로써 라우터 캐시를 무효화합니다.
- 재배포: 데이터 캐시와 달리, 풀 라우트 캐시는 새로운 배포 시 지워집니다.

### 선택해제

풀 라우트 캐시를 선택해제하여, 즉, 모든 들어오는 요청에 대해 동적으로 구성 요소를 렌더링하는 방법은 다음과 같습니다:

<div class="content-ad"></div>

- 동적 기능 사용: 이 방법을 사용하면 전체 라우트 캐시에서 경로를 선택하고 요청 시 동적으로 렌더링합니다. 데이터 캐시는 여전히 사용할 수 있습니다.
- 동적 = `force-dynamic` 또는 revalidate = 0 라우트 세그먼트 구성 옵션 사용: 이 방법을 사용하면 전체 라우트 캐시와 데이터 캐시를 건너뛸 수 있습니다. 이는 컴포넌트가 서버로 전송되는 모든 요청마다 렌더링되고 데이터가 가져오는 것을 의미합니다. 라우터 캐시는 클라이언트 측 캐시이기 때문에 여전히 적용됩니다.
- 데이터 캐시 선택 해제: 캐시되지 않은 페치 요청이 있는 라우트를 선택하는 경우, 해당 요청은 전체 라우트 캐시에서 선택됩니다. 특정 페치 요청에 대한 데이터는 각 요청마다 가져올 것입니다. 캐싱에서 선택되지 않은 다른 페치 요청은 여전히 데이터 캐시에 캐싱됩니다. 이는 캐시된 데이터와 캐시되지 않은 데이터의 혼합을 가능하게 합니다.

## 라우터 캐시

> 관련 용어:
라우터 캐시가 클라이언트 측 캐시 또는 프리페치 캐시로 언급되는 것을 볼 수 있습니다. 프리페치 캐시는 프리페치된 라우트 세그먼트를 의미하며, 클라이언트 측 캐시는 방문한 세그먼트와 프리페치된 세그먼트를 모두 포함하는 전체 라우터 캐시를 의미합니다. 이 캐시는 특히 Next.js와 서버 컴포넌트를 위해 적용되며, 브라우저의 bfcache와는 다릅니다.
  
Next.js는 사용자 세션 기간 동안 개별 라우트 세그먼트로 분할된 React 서버 컴포넌트 페이로드를 저장하는 인메모리 클라이언트 측 캐시인 라우터 캐시를 가지고 있습니다.

<div class="content-ad"></div>

라우터 캐시 작동 방식

![이미지](/assets/img/2024-07-23-CachinginNextjs_8.png)

사용자가 라우트간을 이동함에 따라, Next.js는 방문한 라우트 세그먼트를 캐시하고 사용자가 이동할 가능성이 있는 라우트를 미리 가져옵니다 (그들의 뷰포트에 있는 `Link` 구성요소에 기반함).

이는 사용자에게 향상된 탐색 경험을 제공합니다.

<div class="content-ad"></div>

- 방문한 경로가 캐시되어 있어 빠른 역방향/정방향 탐색이 가능하고, 사전로드(pre-fetching) 및 부분 렌더링에 의해 새로운 경로로의 빠른 이동이 가능합니다.
- 네비게이션 간 전체 페이지 새로고침이 없으며, React 상태와 브라우저 상태가 유지됩니다.

> 라우터 캐시와 전체 경로 캐시의 차이:
라우터 캐시는 사용자 세션 동안 브라우저 내에서 React 서버 컴포넌트 페이로드를 임시로 저장하는 반면, 전체 경로 캐시는 React 서버 컴포넌트 페이로드와 HTML을 서버에 영구적으로 저장하여 여러 사용자 요청에 걸쳐 유지합니다.
전체 경로 캐시는 정적으로 렌더링된 경로만 캐시하지만, 라우터 캐시는 정적 및 동적으로 렌더링된 경로 양쪽에 모두 적용됩니다.

### 지속 시간

캐시는 브라우저의 임시 메모리에 저장됩니다. 라우터 캐시가 지속되는 기간은 두 가지 요소에 dependent합니다:

<div class="content-ad"></div>

- 세션: 캐시는 탐색 간 유지됩니다. 그러나 페이지 새로고침 시에는 지워집니다.
- 자동 무효화 기간: 개별 세그먼트의 캐시는 특정 시간 이후 자동으로 무효화됩니다. 지속 시간은 리소스의 사전 가져온 방식에 따라 달라집니다:
기본 사전 가져오기(prefetch='null' 또는 미지정): 30초
완전한 사전 가져오기(prefetch='true' 또는 router.prefetch): 5분
- 기본 사전 가져오기(prefetch='null' 또는 미지정): 30초
- 완전한 사전 가져오기(prefetch='true' 또는 router.prefetch): 5분

페이지 새로고침은 모든 캐시된 세그먼트를 지우지만, 자동 무효화 기간은 개별 세그먼트에만 영향을 미칩니다. 해당 세그먼트가 사전 가져온 시간부터 시간이 경과될 때까지입니다.

> 참고: v14.2.0-canary.53부터 이러한 값들을 설정할 수 있는 실험적 지원이 제공됩니다.

### 무효화

<div class="content-ad"></div>

라우터 캐시를 무효화하는 두 가지 방법이 있습니다:

- 서버 액션에서:
(revalidatePath)를 통해 경로별로 데이터를 필요에 따라 다시 유효화하거나 (revalidateTag)를 통해 캐시 태그별로 데이터를 다시 유효화할 수 있습니다.
쿠키 설정(cookies.set) 또는 쿠키 삭제(cookies.delete)를 사용하여 라우터 캐시를 무효화할 수 있습니다. 쿠키를 사용하는 라우트가 오래되지 않도록 하기 위해서입니다 (예: 인증).
- (revalidatePath)를 통해 경로별로 데이터를 필요에 따라 다시 유효화하거나 (revalidateTag)를 통해 캐시 태그별로 데이터를 다시 유효화할 수 있습니다.
- router.refresh를 호출하면 라우터 캐시를 무효화하고 현재 경로에 대해 서버로 새로운 요청을 보냅니다.

### 선택적으로 비활성화하기

라우터 캐시에서 제외하는 것은 불가능합니다. 그러나 router.refresh, revalidatePath 또는 revalidateTag를 호출하여 무효화할 수 있습니다 (위 참조). 이렇게 하면 캐시가 지워지고 서버에 새로운 요청이 보내져 최신 데이터가 표시됩니다.

<div class="content-ad"></div>

`Link` 구성 요소의 prefetch 속성을 false로 설정하여 prefetching을 선택적으로 사용하지 않을 수도 있습니다. 그러나 이로 인해 탭 바 또는 앞뒤 네비게이션과 같은 중첩 세그먼트 간의 즉각적인 이동을 허용하기 위해 경로 세그먼트가 30초 동안 임시로 저장됩니다. 방문한 경로는 여전히 캐시됩니다.

## 캐시 상호작용

다양한 캐싱 메커니즘을 구성할 때 다른 캐싱 메커니즘들이 서로 상호작용하는 방식을 이해하는 것이 중요합니다:

### 데이터 캐시 및 전체 경로 캐시

<div class="content-ad"></div>

- 데이터 캐시를 재확인하거나 선택 해제하면 렌더 출력이 데이터에 따라 달라지므로 전체 라우트 캐시가 재설정됩니다.
- 전체 라우트 캐시의 유효성을 손상하거나 선택 해제하면 데이터 캐시에는 영향을 미치지 않습니다. 캐시된 데이터 및 캐시되지 않은 데이터를 모두 사용하는 라우트를 동적으로 렌더링할 수 있습니다. 페이지 대부분이 캐시된 데이터를 사용하지만 요청 시 가져와야 하는 데이터에 의존하는 몇 가지 구성 요소가 있는 경우 유용합니다. 모든 데이터를 다시 가져오는 성능 영향을 걱정하지 않고 동적으로 렌더링할 수 있습니다.

### 데이터 캐시 및 클라이언트 측 라우터 캐시

- 라우트 핸들러에서 데이터 캐시를 다시 확인하면 라우트 핸들러가 특정 라우트에 바인딩되지 않기 때문에 즉시 라우터 캐시를 무효화하지 않습니다. 즉, 라우터 캐시는 강제 새로고침이나 자동 무효화 기간이 경과할 때까지 이전 페이로드를 계속 제공합니다.
- 데이터 캐시 및 라우터 캐시를 즉시 무효화하려면 Server Action에서 revalidatePath 또는 revalidateTag를 사용할 수 있습니다.

## API

<div class="content-ad"></div>

다음 표는 다양한 Next.js API가 캐싱에 어떻게 영향을 미치는지 개요를 제공합니다:

| API                                     | Router Cache       | Full Route Cache   | Data Cache         | React Cache        |
| --------------------------------------- | ------------------ | ------------------ | ------------------ | ------------------ |
| [`<Link prefetch>`](#link)               | Cache              |                    |                    |                    |
| [`router.prefetch`](#routerprefetch)     | Cache              |                    |                    |                    |
| [`router.refresh`](#routerrefresh)       | Revalidate         |                    |                    |                    |
| [`fetch`](#fetch)                        |                    |                    | Cache              | Cache              |
| [`fetch options.cache`](#fetch-optionscache) |                    |                    | Cache or Opt out    |                    |
| [`fetch options.next.revalidate`](#fetch-optionsnextrevalidate) |                    | Revalidate         | Revalidate         |                    |
| [`fetch options.next.tags`](#fetch-optionsnexttags-and-revalidatetag) |                    | Cache              | Cache              |                    |
| [`revalidateTag`](#fetch-optionsnexttags-and-revalidatetag) | Revalidate (Server Action) | Revalidate         | Revalidate         |                    |
| [`revalidatePath`](#revalidatepath)       | Revalidate (Server Action) | Revalidate         | Revalidate         |                    |
| [`const revalidate`](#segment-config-options) |                    | Revalidate or Opt out | Revalidate or Opt out |                    |
| [`const dynamic`](#segment-config-options) |                    | Cache or Opt out   | Cache or Opt out    |                    |
| [`cookies`](#cookies)                    | Revalidate (Server Action) | Opt out            |                    |                    |
| [`headers`, `searchParams`](#dynamic-functions) |                    | Opt out            |                    |                    |
| [`generateStaticParams`](#generatestaticparams) |                    | Cache              |                    |                    |
| [`React.cache`](#react-cache-function)    |                    |                    |                    | Cache              |
| [`unstable_cache`](/docs/app/api-reference/functions/unstable_cache) |                    |                    |                    |                    |

### `Link`

기본적으로 `Link` 컴포넌트는 Full Route Cache에서 경로를 자동으로 프리페치하며 React 서버 컴포넌트 페이로드를 Router Cache에 추가합니다.

<div class="content-ad"></div>

프리패칭을 비활성화하려면 prefetch 속성을 false로 설정할 수 있습니다. 그러나 이렇게 하더라도 캐시를 영구적으로 건너뛰지는 않습니다. 사용자가 경로를 방문할 때 해당 라우트 세그먼트는 여전히 클라이언트 측에 캐시됩니다.

`Link` 컴포넌트에 대해 더 알아보세요.

### router.prefetch

useRouter 훅의 prefetch 옵션을 사용하여 라우트를 수동으로 프리패치할 수 있습니다. 이렇게 하면 React Server Component 페이로드가 라우터 캐시에 추가됩니다.

<div class="content-ad"></div>

useRouter 훅 API 레퍼런스를 확인해보세요.

### router.refresh

useRouter 훅의 refresh 옵션은 라우트를 수동으로 새로 고칠 때 사용할 수 있습니다. 이는 완전히 Router Cache를 지우고 현재 라우트에 대해 서버로 새 요청을 보냅니다. refresh는 Data나 Full Route Cache에는 영향을 미치지 않습니다.

렌더링된 결과는 클라이언트에서 React 상태와 브라우저 상태를 보존하면서 조정될 것입니다.

<div class="content-ad"></div>

useRouter 훅 API 참조를 확인해보세요.

### fetch

fetch로 반환된 데이터는 자동으로 데이터 캐시에 저장됩니다.

```js
// 기본적으로 캐시됩니다. `force-cache`는 기본 옵션이며 생략할 수 있습니다.
fetch(`https://...`, { cache: 'force-cache' })
```

<div class="content-ad"></div>

더 많은 옵션을 위해 fetch API 참조를 확인해보세요.

### fetch options.cache

데이터 캐싱에서 개별 fetch 요청을 건너뛰려면 캐시 옵션을 'no-store'로 설정하세요:

```js
// 캐싱 건너뛰기
fetch(`https://...`, { cache: 'no-store' })
```

<div class="content-ad"></div>

데이터에 따라 렌더링 출력이 달라지기 때문에 cache: 'no-store'를 사용하면 fetch 요청이 사용되는 라우트의 전체 루트 캐시를 건너뛸 수도 있습니다. 이는 라우트가 매 요청마다 동적으로 렌더링되지만, 동일한 라우트에서 여전히 다른 캐시된 데이터 요청을 할 수 있다는 의미입니다.

더 많은 옵션을 원한다면 fetch API 참조를 확인해보세요.

### fetch 옵션.next.revalidate

개별 fetch 요청의 다시 유효화 기간(초)을 설정하기 위해 fetch의 next.revalidate 옵션을 사용할 수 있습니다. 이를 통해 데이터 캐시와 이에 따른 전체 루트 캐시를 다시 유효화할 수 있습니다. 새로운 데이터가 가져와지고 컴포넌트가 서버에서 다시 렌더링될 것입니다. 

<div class="content-ad"></div>

```js
// 1시간 후에 Revalidate
fetch(`https://...`, { next: { revalidate: 3600 } })
```

추가 옵션 정보는 fetch API 레퍼런스에서 확인할 수 있습니다.

### fetch 옵션.next.tags 및 revalidateTag

Next.js는 세밀한 데이터 캐싱과 다시 유효화를 위한 캐시 태깅 시스템을 가지고 있습니다.


<div class="content-ad"></div>

- fetch 나 unstable_cache를 사용할 때, 캐시 엔트리에 하나 이상의 태그를 붙일 수 있어요.
- 그러면 revalidateTag를 호출하여 해당 태그와 연관된 캐시 엔트리를 삭제할 수 있어요.

예를 들어, 데이터를 가져올 때 태그를 설정할 수 있어요:

```js
// 태그와 함께 데이터 캐시
fetch(`https://...`, { next: { tags: ['a', 'b', 'c'] } })
```

그런 다음 태그와 함께 revalidateTag를 호출하여 캐시 엔트리를 삭제할 수 있어요:

<div class="content-ad"></div>

```js
// 특정 태그가 있는 항목을 다시 유효성 검사합니다.
revalidateTag('a')
```

revalidateTag를 사용할 수 있는 두 군데가 있습니다. 당신이 이루고자 하는 목표에 따라 다릅니다:

- Route Handlers - 타사 이벤트 (예: 웹훅)의 응답 데이터를 다시 유효성 검사합니다. 이는 라우터 캐시를 즉시 무효화하지 않습니다. 왜냐하면 라우터 핸들러가 특정 경로에 묶여 있지 않기 때문입니다.
- Server Actions - 사용자 조작 (예: 폼 제출) 이후 데이터를 다시 유효성 검사합니다. 이는 연관된 경로의 라우터 캐시를 무효화합니다.

### revalidatePath


<div class="content-ad"></div>

`revalidatePath` 함수를 사용하면 특정 경로 아래의 라우트 세그먼트를 수동으로 다시 유효성 검사하고 다시 렌더링할 수 있습니다. `revalidatePath` 메소드를 호출하면 데이터 캐시를 다시 유효성 검사하고, 이로써 전체 라우트 캐시가 무효화됩니다.

```js
revalidatePath('/')
```

달성하려는 목표에 따라 `revalidatePath`를 사용할 수 있는 두 가지 장소가 있습니다:

- 라우트 핸들러 - 제삼자 이벤트(예: 웹훅)에 대한 응답으로 데이터를 다시 유효성 검사하려는 경우.
- 서버 액션 - 사용자 상호작용(예: 폼 제출, 버튼 클릭) 후 데이터를 다시 유효성 검사하려는 경우.

<div class="content-ad"></div>

더 많은 정보를 보려면 revalidatePath API 참조를 확인하세요.

> revalidatePath vs. router.refresh:
router.refresh를 호출하면 Router 캐시가 지워지고 데이터 캐시나 전체 루트 캐시를 무효화시키지 않고 서버에서 루트 세그먼트를 다시 렌더링합니다. 
차이점은 revalidatePath가 데이터 캐시와 전체 루트 캐시를 삭제하는 반면 router.refresh()는 데이터 캐시와 전체 루트 캐시를 변경하지 않으며 클라이언트 측 API이기 때문입니다.

### 동적 함수

쿠키, 헤더와 같은 동적 함수 및 Pages의 searchParams 속성은 런타임 요청 정보에 따라 변경됩니다. 
이러한 함수를 사용하면 루트가 전체 루트 캐시에서 제외되어 동적으로 렌더링됩니다.

<div class="content-ad"></div>

#### 쿠키

쿠키를 사용하여 cookies.set이나 cookies.delete를 서버 동작에서 사용하면 라우터 캐시를 무효화하여 쿠키를 사용하는 경로가 더 이상 유효하지 않게 되는 것을 방지합니다 (예: 인증 변경을 반영).

쿠키 API 참조를 참고하세요.

### 세그먼트 구성 옵션

<div class="content-ad"></div>

경로 세그먼트 구성 옵션은 기본 경로 세그먼트를 재정의하거나 fetch API를 사용할 수 없는 경우(예: 데이터베이스 클라이언트 또는 제3자 라이브러리)에 사용할 수 있습니다.

다음 경로 세그먼트 구성 옵션은 데이터 캐시 및 전체 경로 캐시에서 제외됩니다:

- const dynamic = `force-dynamic`
- const revalidate = 0

더 많은 옵션을 보려면 Route Segment Config 문서를 참조하십시오.

<div class="content-ad"></div>

### generateStaticParams

동적 세그먼트 (예: app/blog/[slug]/page.js)의 경우, generateStaticParams에서 제공된 경로는 빌드 시 전체 루트 캐시에 캐시됩니다. 요청 시에 Next.js는 처음 방문할 때 빌드 시 알려지지 않았던 경로도 캐시합니다.

요청 시 캐싱을 비활성화하려면 라우트 세그먼트에서 export const dynamicParams = false 옵션을 사용하면 됩니다. 이 구성 옵션을 사용하면 generateStaticParams에서 제공된 경로만 제공되며, 다른 경로는 404 오류가 발생하거나 일치합니다 (캐치올 라우트의 경우).

generateStaticParams API 참조를 참조하세요.

<div class="content-ad"></div>

### React 캐시 함수

React 캐시 함수를 사용하면 함수의 반환 값을 메모이즈하여 동일한 함수를 여러 번 호출하더라도 한 번만 실행할 수 있습니다.

페치 요청은 자동으로 메모이즈되므로 React 캐시로 랩핑할 필요가 없습니다. 그러나 페치 API가 적합하지 않은 경우에 데이터 요청을 수동으로 메모이즈하기 위해 캐시를 사용할 수 있습니다. 예를 들어, 일부 데이터베이스 클라이언트, CMS 클라이언트 또는 GraphQL 클라이언트 등이 해당됩니다.

```js
import { cache } from 'react'
import db from '@/lib/db'
 
export const getItem = cache(async (id: string) => {
  const item = await db.item.findUnique({ id })
  return item
})
```