---
title: "Nextjs 14 드래프트 모드 더 효율적인 개발을 위한 비법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:48
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Draft Mode"
link: "https://nextjs.org/docs/app/building-your-application/configuring/draft-mode"
isUpdated: true
---




# 초안 모드

정적 렌더링은 페이지가 헤드리스 CMS에서 데이터를 가져올 때 유용합니다. 그러나 헤드리스 CMS에서 초안을 작성하고 즉시 해당 초안을 페이지에서 볼 수 있는 것이 이상적이지 않을 때가 있습니다. 이 경우 Next.js에게 이러한 페이지를 빌드 시간이 아닌 요청 시간에 렌더링하도록 하고, 발행된 콘텐츠 대신 초안 콘텐츠를 가져오도록 원할 것입니다. 이 특정 케이스에 대해서만 Next.js가 동적 렌더링으로 전환하도록 원할 것입니다.

Next.js에는 이 문제를 해결하는 Draft Mode라는 기능이 있습니다. 이를 사용하는 방법에 대한 지침은 다음과 같습니다.

## 단계 1: 라우트 핸들러 생성 및 접근하기

<div class="content-ad"></div>

먼저 Route Handler를 작성하세요. 이름은 무엇이든 상관없습니다 - 예: app/api/draft/route.ts

다음으로 next/headers에서 draftMode를 가져 와 enable() 메서드를 호출하세요.

```js
// route handler enabling draft mode
import { draftMode } from 'next/headers'
 
export async function GET(request: Request) {
  draftMode().enable()
  return new Response('Draft mode is enabled')
}
```

이렇게 하면 임시 모드를 활성화하는 쿠키가 설정됩니다. 이 쿠키를 포함한 후속 요청은 정적으로 생성된 페이지의 동작을 변경하는 Draft Mode를 트리거합니다(나중에 자세히 설명합니다).

<div class="content-ad"></div>

수동으로 확인하려면 /api/draft를 방문하고 브라우저의 개발자 도구를 확인하세요. __prerender_bypass이라는 쿠키가 포함된 Set-Cookie 응답 헤더를 참고하실 수 있습니다.

### 헤드리스 CMS에서 안전하게 접근하는 방법

실제로는 이 라우트 핸들러를 헤드리스 CMS에서 안전하게 호출하고 싶을 것입니다. 사용 중인 헤드리스 CMS에 따라 구체적인 단계는 달라질 수 있지만, 일반적으로 취할 수 있는 몇 가지 단계가 있습니다.

이러한 단계들은 사용 중인 헤드리스 CMS에서 사용자 정의 드래프트 URL을 설정하는 것을 지원할 때 가정됩니다. 그렇지 않은 경우에도 이 방법을 사용하여 드래프트 URL을 안전하게 보호할 수 있지만, 드래프트 URL을 직접 만들고 액세스해야 할 것입니다.

<div class="content-ad"></div>

먼저, 선택한 토큰 생성기를 사용하여 비밀 토큰 문자열을 생성해야 합니다. 이 비밀은 Next.js 앱과 헤드리스 CMS만 알게 될 것입니다. 이 비밀은 CMS에 액세스할 수 없는 사람들이 초안 URL에 액세스하는 것을 방지합니다.

두 번째로, 헤드리스 CMS가 사용자 정의 초안 URL을 설정하는 것을 지원하는 경우, 다음과 같이 초안 URL을 지정하십시오. 이는 Route Handler가 app/api/draft/route.ts에 위치해 있다고 가정합니다.

```js
https://<your-site>/api/draft?secret=<token>&slug=<path>
```

- `your-site`는 배포 도메인이어야 합니다.
- `token`은 생성한 비밀 토큰으로 대체해야 합니다.
- `path`는 볼 페이지의 경로여야 합니다. /posts/foo를 보려면 &slug=/posts/foo를 사용해야 합니다.

<div class="content-ad"></div>

당신의 헤드리스 CMS는 초안 URL에 변수를 포함할 수 있도록 해줄 수 있습니다. 이렇게 하면 `path`를 CMS 데이터를 기반으로 동적으로 설정할 수 있습니다: &slug=/posts/'entry.fields.slug'

마지막으로, 라우트 핸들러에서:

- secret이 일치하는지 확인하고 slug 매개변수가 있는지 확인하세요 (없으면 요청이 실패해야 합니다).
- draftMode.enable()을 호출하여 쿠키를 설정합니다.
- 그런 다음 브라우저를 slug로 지정된 경로로 리디렉션합니다.

```js
// secret 및 slug를 사용한 라우트 핸들러
import { draftMode } from 'next/headers'
import { redirect } from 'next/navigation'
 
export async function GET(request: Request) {
  // 쿼리 문자열 매개변수 구문 분석
  const { searchParams } = new URL(request.url)
  const secret = searchParams.get('secret')
  const slug = searchParams.get('slug')
 
  // secret 및 다음 매개변수 확인
  // 이 secret은 이 라우트 핸들러와 CMS에서만 알려져야 합니다
  if (secret !== 'MY_SECRET_TOKEN' || !slug) {
    return new Response('잘못된 토큰', { status: 401 })
  }
 
  // 제공된 `slug`가 존재하는지 확인하기 위해 헤드리스 CMS를 가져옵니다
  // getPostBySlug는 헤드리스 CMS로 필요한 검색 로직을 구현할 것입니다
  const post = await getPostBySlug(slug)
 
  // slug가 존재하지 않으면 초안 모드를 활성화하지 못하도록 합니다
  if (!post) {
    return new Response('잘못된 slug', { status: 401 })
  }
 
  // 쿠키를 설정하여 초안 모드를 활성화합니다
  draftMode().enable()
 
  // 가져온 게시물의 경로로 리디렉션합니다
  // searchParams.slug로 리디렉션하지 않는 이유는 열린 리디렉션 취약점을 초래할 수 있기 때문입니다
  redirect(post.slug)
}
```

<div class="content-ad"></div>

성공하면, 브라우저가 원하는 경로로 이동하고 임시 모드 쿠키를 사용합니다.

## 단계 2: 페이지 업데이트

다음 단계는 페이지를 업데이트하여 draftMode().isEnabled의 값을 확인하는 것입니다.

쿠키가 설정된 페이지를 요청하면, 데이터가 빌드 시간이 아닌 요청 시간에 가져옵니다.

<div class="content-ad"></div>

또한, isEnabled의 값은 true가 될 것입니다.

```js
// 데이터를 가져오는 페이지
import { draftMode } from 'next/headers'

async function getData() {
  const { isEnabled } = draftMode()

  const url = isEnabled
    ? 'https://draft.example.com'
    : 'https://production.example.com'

  const res = await fetch(url)

  return res.json()
}

export default async function Page() {
  const { title, desc } = await getData()

  return (
    <main>
      <h1>{title}</h1>
      <p>{desc}</p>
    </main>
  )
}
```

마치! headless CMS에서 비발행 글의 더티 URL 핸들러(비밀 및 슬러그 포함)에 액세스하거나 수동으로 액세스하면, 이제 더티 콘텐츠를 확인할 수 있어야 합니다. 그리고 발행하지 않은 글을 업데이트하고 저장하지 않는 경우, 더티를 확인할 수 있어야 합니다.

headless CMS에서 이를 더티 URL로 설정하거나 수동으로 액세스하면, 더티를 확인할 수 있어야 합니다.

<div class="content-ad"></div>


https://<your-site>/api/draft?secret=<token>&slug=<path>


## 자세한 내용

### 드래프트 모드 쿠키 지우기

기본적으로 브라우저를 닫을 때 드래프트 모드 세션이 종료됩니다.


<div class="content-ad"></div>

Draft Mode 쿠키를 수동으로 지우려면 draftMode().disable()를 호출하는 Route Handler를 만들어주세요:

```js
import { draftMode } from 'next/headers'
 
export async function GET(request: Request) {
  draftMode().disable()
  return new Response('Draft mode is disabled')
}
```

그런 다음 /api/disable-draft로 요청을 보내어 Route Handler를 호출하세요. next/link를 사용하여이 경로를 호출하는 경우에는 prefetch='false'를 전달하여 미리보기(prefetch) 시 실수로 쿠키를 삭제하는 것을 방지해야합니다.

### Next 빌드마다 고유함

<div class="content-ad"></div>

매번 다음 빌드를 실행할 때마다 새로운 바이패스 쿠키 값이 생성됩니다.

이렇게 함으로써 바이패스 쿠키를 추측할 수 없게 됩니다.

> 알아 두면 좋은 점: 로컬에서 HTTP를 통해 드래프트 모드를 테스트하려면 브라우저가 타사 쿠키 및 로컬 저장소 액세스를 허용해야 합니다.