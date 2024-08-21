---
title: "Nextjs 14에서 Content Security Policy 설정하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:48
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Content Security Policy"
link: "https://nextjs.org/docs/app/building-your-application/configuring/content-security-policy"
isUpdated: true
---




# 콘텐츠 보안 정책

콘텐츠 보안 정책(CSP)는 다양한 보안 위협으로부터 Next.js 애플리케이션을 보호하는 데 중요합니다. 크로스사이트 스크립팅(XSS), 클릭재킹, 기타 코드 삽입 공격과 같은 보안 위협으로부터 보호할 수 있습니다.

CSP를 사용하면 개발자가 콘텐츠 소스, 스크립트, 스타일시트, 이미지, 폰트, 객체, 미디어(오디오, 비디오), 아이프레임 등에 대해 허용되는 출처를 명시할 수 있습니다.

## 난스

<div class="content-ad"></div>

한 번만 사용하는 용도로 생성된 고유한 무작위 문자열인 논스(nonce)는 CSP와 함께 사용되어 특정 인라인 스크립트나 스타일이 엄격한 CSP 지시문을 우회하고 실행되도록 선택적으로 허용됩니다.

### 논스를 사용하는 이유는?

CSP는 악의적인 스크립트를 차단하기 위해 설계되었지만, 일부 상황에서는 인라인 스크립트가 필요한 경우가 있습니다. 이러한 경우 논스를 사용하면 올바른 논스가 있는 경우에만 이러한 스크립트가 실행되도록 허용할 수 있습니다.

### Middleware를 사용하여 논스 추가하기

<div class="content-ad"></div>

미들웨어를 사용하면 페이지가 렌더링되기 전에 헤더를 추가하고 논스를 생성할 수 있습니다.

페이지를 볼 때 마다 새로운 논스를 생성해야 합니다. 이는 동적 렌더링을 사용하여 논스를 추가해야 한다는 것을 의미합니다.

예를 들어:

```js
import { NextRequest, NextResponse } from 'next/server'
 
export function middleware(request: NextRequest) {
  const nonce = Buffer.from(crypto.randomUUID()).toString('base64')
  const cspHeader = `
    default-src 'self';
    script-src 'self' 'nonce-${nonce}' 'strict-dynamic';
    style-src 'self' 'nonce-${nonce}';
    img-src 'self' blob: data:;
    font-src 'self';
    object-src 'none';
    base-uri 'self';
    form-action 'self';
    frame-ancestors 'none';
    upgrade-insecure-requests;
`
  // 줄 바꿈 문자 및 공백을 대체
  const contentSecurityPolicyHeaderValue = cspHeader
    .replace(/\s{2,}/g, ' ')
    .trim()
 
  const requestHeaders = new Headers(request.headers)
  requestHeaders.set('x-nonce', nonce)
 
  requestHeaders.set(
    'Content-Security-Policy',
    contentSecurityPolicyHeaderValue
  )
 
  const response = NextResponse.next({
    request: {
      headers: requestHeaders,
    },
  })
  response.headers.set(
    'Content-Security-Policy',
    contentSecurityPolicyHeaderValue
  )
 
  return response
}
```

<div class="content-ad"></div>

기본적으로 미들웨어는 모든 요청에서 실행됩니다. Matcher를 사용하여 특정 경로에서만 미들웨어가 실행되도록 필터링할 수 있습니다.

(prefetches와 static 자산을 무시하는 것이 좋습니다. 이들은 CSP 헤더가 필요하지 않습니다.)

```js
export const config = {
  matcher: [
    /*
     * API 경로인 경우를 제외한 모든 요청 경로와 일치:
     * - api (API routes)
     * - _next/static (정적 파일)
     * - _next/image (이미지 최적화 파일)
     * - favicon.ico (파비콘 파일)
     */
    {
      source: '/((?!api|_next/static|_next/image|favicon.ico).*)',
      missing: [
        { type: 'header', key: 'next-router-prefetch' },
        { type: 'header', key: 'purpose', value: 'prefetch' },
      ],
    },
  ],
}
```

### 논스(Nonce) 읽기

<div class="content-ad"></div>

서버 구성 요소에서 헤더를 사용하여 nonce를 읽어올 수 있습니다:

```js
import { headers } from 'next/headers'
import Script from 'next/script'

export default function Page() {
  const nonce = headers().get('x-nonce')

  return (
    <Script
      src="https://www.googletagmanager.com/gtag/js"
      strategy="afterInteractive"
      nonce={nonce}
    />
  )
}
```

## Nonces가 없는 경우

nonce가 필요하지 않은 애플리케이션의 경우, `next.config.js` 파일에서 CSP 헤더를 직접 설정할 수 있습니다.

<div class="content-ad"></div>

```js
const cspHeader = `
    default-src 'self';
    script-src 'self' 'unsafe-eval' 'unsafe-inline';
    style-src 'self' 'unsafe-inline';
    img-src 'self' blob: data:;
    font-src 'self';
    object-src 'none';
    base-uri 'self';
    form-action 'self';
    frame-ancestors 'none';
    upgrade-insecure-requests;
`
 
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          {
            key: 'Content-Security-Policy',
            value: cspHeader.replace(/\n/g, ''),
          },
        ],
      },
    ]
  },
}
```

## 버전 이력

Next.js의 v13.4.20+를 사용하는 것을 권장합니다. 이를 통해 nonce를 제대로 처리하고 적용할 수 있습니다.
