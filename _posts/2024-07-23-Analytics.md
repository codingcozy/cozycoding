---
title: "Nextjs 14에서 App Router로 웹사이트 분석 도구 구축하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:40
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Analytics"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/analytics"
---


# 분석

Next.js에는 성능 메트릭을 측정하고 보고하는 데 내장 지원이 있습니다. useReportWebVitals 훅을 사용하여 직접 보고를 관리하거나 대안으로 Vercel은 자동으로 데이터를 수집하여 시각화해주는 관리형 서비스를 제공합니다.

## 직접 만들기

```js
'use client'
 
import { useReportWebVitals } from 'next/web-vitals'
 
export function WebVitals() {
  useReportWebVitals((metric) => {
    console.log(metric)
  })
}
```

<div class="content-ad"></div>

```js
import { WebVitals } from './_components/web-vitals'
 
export default function Layout({ children }) {
  return (
    <html>
      <body>
        <WebVitals />
        {children}
      </body>
    </html>
  )
}
```

> useReportWebVitals 훅을 사용하려면 "use client" 지시문이 필요합니다. 가장 효율적인 접근법은 루트 레이아웃이 가져오는 별도의 컴포넌트를 만드는 것입니다. 이를 통해 클라이언트 경계를 WebVitals 컴포넌트로만 제한할 수 있습니다.

더 많은 정보는 API 참조를 확인하세요.

## 웹 비탈스

<div class="content-ad"></div>

웹 비탈스(Web Vitals)는 웹 페이지의 사용자 경험을 포착하려는 유용한 지표 모음입니다. 다음과 같은 웹 비탈스가 모두 포함되어 있습니다:

- Time to First Byte (TTFB)
- First Contentful Paint (FCP)
- Largest Contentful Paint (LCP)
- First Input Delay (FID)
- Cumulative Layout Shift (CLS)
- Interaction to Next Paint (INP)

이러한 지표 결과를 모두 name 속성을 사용하여 처리할 수 있습니다. 아래는 Markdown 포맷으로 테이블 태그를 변경한 내용입니다.

```js
'use client'

import { useReportWebVitals } from 'next/web-vitals'

export function WebVitals() {
  useReportWebVitals((metric) => {
    switch (metric.name) {
      case 'FCP': {
        // FCP 결과 처리
      }
      case 'LCP': {
        // LCP 결과 처리
      }
      // ...
    }
  })
}
```

<div class="content-ad"></div>

## 외부 시스템으로 결과 전송하기

귀하의 사이트에서 실제 사용자 성능을 측정하고 추적하기 위해 어떤 엔드포인트에든 결과를 전송할 수 있습니다. 예를 들어:

```js
useReportWebVitals((metric) => {
  const body = JSON.stringify(metric)
  const url = 'https://example.com/analytics'
 
  // `navigator.sendBeacon()`을 사용할 수 있다면 사용하고, 그렇지 않으면 `fetch()`를 사용합니다.
  if (navigator.sendBeacon) {
    navigator.sendBeacon(url, body)
  } else {
    fetch(url, { body, method: 'POST', keepalive: true })
  }
})
```

> 알아두면 좋은 점: Google Analytics를 사용하는 경우 id 값을 사용하면 수동으로 메트릭 분포를 만들 수 있습니다 (백분위수 계산 등).

<div class="content-ad"></div>

```js
useReportWebVitals((metric) => {
  // 이 예제와 같이 Google Analytics를 초기화했다면 `window.gtag`을 사용하세요:
  // https://github.com/vercel/next.js/blob/canary/examples/with-google-analytics/pages/_app.js
  window.gtag('event', metric.name, {
    value: Math.round(
      metric.name === 'CLS' ? metric.value * 1000 : metric.value
    ), // 값은 정수여야 합니다.
    event_label: metric.id, // 현재 페이지 로드에 고유한 ID
    non_interaction: true, // 이벤트가 이탈률에 영향을 미치지 않음
  });
});
```

Google Analytics에 결과를 보내는 방법에 대해 더 읽어보기.