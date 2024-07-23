---
title: "Nextjs 14와 Nextjs App Router를 이용한 2024 국제화i18n 가이드"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:16
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Internationalization"
link: "https://nextjs.org/docs/app/building-your-application/routing/internationalization"
---


# 국제화

Next.js를 사용하면 여러 언어를 지원하기 위해 라우팅 및 콘텐츠 렌더링을 구성할 수 있습니다. 사이트를 다양한 로케일에 적응시키는 것은 번역된 콘텐츠(로컬라이제이션)와 국제화된 경로를 포함합니다.

## 용어

- 로캘(Locale): 언어와 형식화 기본 설정에 대한 식별자입니다. 일반적으로 사용자의 선호 언어와 지역을 포함합니다.
- en-US: 미국에서 사용되는 영어
- nl-NL: 네덜란드에서 사용되는 네덜란드어
- nl: 네덜란드어, 특정 지역 없음
- en-US: 미국에서 사용되는 영어
- nl-NL: 네덜란드에서 사용되는 네덜란드어
- nl: 네덜란드어, 특정 지역 없음

<div class="content-ad"></div>

## 라우팅 개요

브라우저에서 사용자의 언어 환경 설정을 사용하여 사용할 지역 설정을 선택하는 것이 좋습니다. 선호하는 언어를 변경하면 응용 프로그램으로 들어오는 Accept-Language 헤더가 수정됩니다.

예를 들어, 다음 라이브러리를 사용하면 Headers, 지원할 지역 설정 및 기본 지역 설정에 기반하여 선택할 지역 설정을 확인할 수 있습니다.

```js
import { match } from '@formatjs/intl-localematcher'
import Negotiator from 'negotiator'
 
let headers = { 'accept-language': 'en-US,en;q=0.5' }
let languages = new Negotiator({ headers }).languages()
let locales = ['en-US', 'nl-NL', 'nl']
let defaultLocale = 'en-US'
 
match(languages, locales, defaultLocale) // -> 'en-US'
```

<div class="content-ad"></div>

라우팅은 하위 경로 (/fr/products) 또는 도메인 (my-site.fr/products)을 통해 국제화될 수 있습니다. 이 정보를 사용하여 미들웨어 내에서 로케일에 기반하여 사용자를 리디렉션할 수 있습니다.

```js
import { NextResponse } from "next/server";
 
let locales = ['en-US', 'nl-NL', 'nl']
 
// 선호하는 로케일 가져오기, 위와 유사하게 또는 라이브러리를 사용하여
function getLocale(request) { ... }
 
export function middleware(request) {
  // 경로에 지원되는 로케일이 있는지 확인
  const { pathname } = request.nextUrl
  const pathnameHasLocale = locales.some(
    (locale) => pathname.startsWith(`/${locale}/`) || pathname === `/${locale}`
  )
 
  if (pathnameHasLocale) return
 
  // 로케일이 없으면 리디렉션
  const locale = getLocale(request)
  request.nextUrl.pathname = `/${locale}${pathname}`
  // 예: 들어오는 요청이 /products인 경우
  // 새로운 URL은 이제 /en-US/products가 됩니다
  return NextResponse.redirect(request.nextUrl)
}
 
export const config = {
  matcher: [
    // 모든 내부 경로 (_next)는 건너뛰기
    '/((?!_next).*)',
    // 옵션: 루트 (/) URL에서만 실행
    // '/'
  ],
}
```

마지막으로, app/ 내의 모든 특수 파일이 app/[lang] 아래에 중첩되도록 확인하십시오. 이렇게 하면 Next.js 라우터가 루트에서 다른 로케일을 동적으로 처리하고 모든 레이아웃과 페이지에 lang 매개변수를 전달할 수 있습니다. 예를 들어:

```js
// 현재 로케일에 액세스할 수 있습니다
// 예: /en-US/products -> `lang`은 "en-US"입니다
export default async function Page({ params: { lang } }) {
  return ...
}
```

<div class="content-ad"></div>

루트 레이아웃은 새 폴더에 중첩될 수도 있습니다 (예: app/[lang]/layout.js).

## 지역화

사용자의 선호 지역에 따라 표시된 콘텐츠를 변경하는 것, 즉 지역화는 Next.js에만 해당되는 것은 아닙니다. 아래에 설명된 패턴은 모든 웹 애플리케이션에서 동일하게 작동할 것입니다.

예를 들어, 애플리케이션 내에서 영어 및 네덜란드어 콘텐츠를 지원하고 싶다고 가정해 봅시다. 우리는 두 개의 다른 "사전"을 유지할 수 있습니다. 이는 키에서 로컬라이즈된 문자열로의 매핑을 제공하는 객체들입니다. 예를 들면:

<div class="content-ad"></div>

```js
{
  "products": {
    "cart": "장바구니에 추가"
  }
}
```

```js
{
  "products": {
    "cart": "장바구니에 추가"
  }
}
```

그런 다음 요청된 지역에 대한 번역을로드하기 위해 getDictionary 함수를 만들 수 있습니다.

```js
import 'server-only'

const dictionaries = {
  en: () => import('./dictionaries/en.json').then((module) => module.default),
  nl: () => import('./dictionaries/nl.json').then((module) => module.default),
}

export const getDictionary = async (locale) => dictionaries[locale]()
```

<div class="content-ad"></div>

현재 선택된 언어에 따라 레이아웃이나 페이지 안에 있는 사전을 가져올 수 있습니다.

```js
import { getDictionary } from './dictionaries'

export default async function Page({ params: { lang } }) {
  const dict = await getDictionary(lang) // en
  return <button>{dict.products.cart}</button> // 카트에 추가
}
```

app/ 디렉토리에 있는 모든 레이아웃과 페이지가 서버 구성 요소로 기본 설정되어 있기 때문에 번역 파일의 크기가 클라이언트 측 JavaScript 번들 크기에 영향을 미치는 것에 대해 걱정할 필요가 없습니다. 이 코드는 서버에서만 실행되며 최종 HTML만 브라우저로 전송됩니다.

## 정적 생성

<div class="content-ad"></div>

특정 로캘 세트에 대한 정적 경로를 생성하려면 페이지나 레이아웃과 함께 generateStaticParams를 사용할 수 있습니다. 이것은 전역적으로 사용할 수 있으며, 예를 들어 루트 레이아웃에서 다음과 같이 사용할 수 있습니다:

```js
export async function generateStaticParams() {
  return [{ lang: 'en-US' }, { lang: 'de' }]
}
 
export default function Root({ children, params }) {
  return (
    <html lang={params.lang}>
      <body>{children}</body>
    </html>
  )
}
```

## 자원

- 최소 i18n 라우팅과 번역
- next-intl
- next-international
- next-i18n-router