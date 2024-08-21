---
title: "Nextjs 14에서 동적 라우팅 설정 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:11
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Dynamic Routes"
link: "https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes"
isUpdated: true
---




# 동적 라우트

정확한 세그먼트 이름을 미리 알 수 없고 동적 데이터에서 라우트를 생성하고 싶을 때, 요청 시에 채워지거나 빌드 시에 미리 렌더링된 동적 세그먼트를 사용할 수 있습니다.

## 관례

폴더 이름을 대괄호로 감싸면 동적 세그먼트를 만들 수 있습니다: [폴더이름]. 예를 들어, [id] 또는 [slug]와 같이 사용할 수 있습니다.

<div class="content-ad"></div>

동적 세그먼트는 레이아웃, 페이지, 라우트 및 generateMetadata 함수에 params 속성으로 전달됩니다.

## 예시

예를 들어, 블로그의 경우 블로그 게시물을 위한 동적 세그먼트인 [slug]가 포함된 route app/blog/[slug]/page.js를 사용할 수 있습니다.

```js
export default function Page({ params }: { params: { slug: string } }) {
  return <div>My Post: {params.slug}</div>
}
```

<div class="content-ad"></div>


| Route                   | Example URL | `params`           |
|-------------------------|-------------|--------------------|
| `app/blog/[slug]/page.js` | `/blog/a`   | `{ slug: 'a' }`    |
| `app/blog/[slug]/page.js` | `/blog/b`   | `{ slug: 'b' }`    |
| `app/blog/[slug]/page.js` | `/blog/c`   | `{ slug: 'c' }`    |

See the `generateStaticParams()` page to learn how to generate the params for the segment.

> Good to know: Dynamic Segments are equivalent to Dynamic Routes in the pages directory.

## Generating Static Params


<div class="content-ad"></div>

```js
generateStaticParams 함수는 동적 라우트 세그먼트와 조합하여 빌드 시간에 루트를 정적으로 생성할 수 있습니다. 이를 통해 요청 시간이 아니라 미리 빌드할 수 있습니다.

export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())
 
  return posts.map((post) => ({
    slug: post.slug,
  }))
}

generateStaticParams 함수의 주요 이점은 데이터를 스마트하게 검색한다는 것입니다. generateStaticParams 함수 내에서 fetch 요청을 사용하여 콘텐츠를 가져오면 요청이 자동으로 메모이제이션됩니다. 이는 동일한 인수로 여러 generateStaticParams, 레이아웃 및 페이지에서 요청이 이루어질 때 한 번만 요청되므로 빌드 시간이 줄어듭니다.

페이지 디렉토리에서 마이그레이션하는 경우 마이그레이션 가이드를 사용하십시오.
```

<div class="content-ad"></div>

더 많은 정보 및 고급 사용 예시에 대한 generateStaticParams 서버 함수 설명서를 참조하세요.

## Catch-all Segments

동적 세그먼트를 확장하여 대괄호 안에 마침표 세 개(ellipsis)를 추가하여 이후의 모든 세그먼트를 캐치할 수 있습니다 [...폴더명].

예를 들어, app/shop/[...slug]/page.js는 /shop/clothes뿐만 아니라 /shop/clothes/tops, /shop/clothes/tops/t-shirts 등과 같이 매치됩니다.

<div class="content-ad"></div>

표를 다음과 같이 Markdown 형식으로 변경해보세요.


| Route                      | Example URL     | `params`            |
|----------------------------|-----------------|---------------------|
| `app/shop/[...slug]/page.js`| `/shop/a`       | `{ slug: ['a'] }`   |
| `app/shop/[...slug]/page.js`| `/shop/a/b`     | `{ slug: ['a', 'b'] }` |
| `app/shop/[...slug]/page.js`| `/shop/a/b/c`   | `{ slug: ['a', 'b', 'c'] }` |

## Optional Catch-all Segments

Catch-all Segments는 파라미터를 이중 대괄호로 포함시켜 선택사항으로 만들 수 있습니다: `[[...folderName]]`.

예를 들어, `app/shop/[[...slug]]/page.js`는 `/shop`뿐만 아니라 `/shop/clothes`, `/shop/clothes/tops`, `/shop/clothes/tops/t-shirts`도 일치시킵니다.


<div class="content-ad"></div>

선택적 캐치올 세그먼트와 캐치올 세그먼트의 차이는 선택적으로 사용할 수 있는 경우, 매개변수 없이도 일치하는 라우트입니다 (/shop은 위의 예제에서 일치합니다).

Route         | Example URL | `params`
--------------|-------------|---------
`app/shop/[[...slug]]/page.js` | `/shop` | `{}`
`app/shop/[[...slug]]/page.js` | `/shop/a` | `{ slug: ['a'] }`
`app/shop/[[...slug]]/page.js` | `/shop/a/b` | `{ slug: ['a', 'b'] }`
`app/shop/[[...slug]]/page.js` | `/shop/a/b/c` | `{ slug: ['a', 'b', 'c'] }`

## TypeScript

TypeScript를 사용할 때, 구성된 라우트 세그먼트에 따라 매개변수에 대한 유형을 추가할 수 있습니다.

<div class="content-ad"></div>

```javascript
export default function Page({ params }: { params: { slug: string } }) {
  return <h1>My Page</h1>
}
```

- Route: `app/blog/[slug]/page.js`
  - `params` Type Definition: `{ slug: string }`

- Route: `app/shop/[...slug]/page.js`
  - `params` Type Definition: `{ slug: string[] }`

- Route: `app/shop/[[...slug]]/page.js`
  - `params` Type Definition: `{ slug?: string[] }`

- Route: `app/[categoryId]/[itemId]/page.js`
  - `params` Type Definition: `{ categoryId: string, itemId: string }`

> 좋은 정보: 이러한 작업은 앞으로 TypeScript 플러그인에서 자동으로 처리될 수 있습니다.