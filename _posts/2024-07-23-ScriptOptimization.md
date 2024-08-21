---
title: "Nextjs 14에서 스크립트 최적화하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:39
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Script Optimization"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/scripts"
isUpdated: true
---




# 스크립트 최적화

### 레이아웃 스크립트

여러 경로에서 서드 파티 스크립트를로드하려면 next/script를 가져와 레이아웃 구성 요소에 직접 스크립트를 포함하십시오:

```js
import Script from 'next/script'

export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <>
      <section>{children}</section>
      <Script src="https://example.com/script.js" />
    </>
  )
}
```

<div class="content-ad"></div>

세 번째 파티 스크립트는 사용자가 폴더 경로(예: 대시보드/페이지.js) 또는 임의의 중첩된 경로(예: 대시보드/설정/페이지.js)에 액세스 할 때 가져옵니다. Next.js는 사용자가 동일한 레이아웃에서 여러 경로 사이를 탐색해도 한 번만 스크립트가로드되도록 보장합니다.

### 어플리케이션 스크립트

모든 경로에 대한 세 번째 파티 스크립트를로드하려면 next/script를 가져 와서 스크립트를 직접 루트 레이아웃에 포함하십시오:

```js
import Script from 'next/script'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <Script src="https://example.com/script.js" />
    </html>
  )
}
```

<div class="content-ad"></div>

이 스크립트는 귀하의 애플리케이션에서 어떤 경로에 액세스할 때 로드되고 실행됩니다. Next.js는 사용자가 여러 페이지 사이를 이동하더라도 한 번만 스크립트가 로드되도록 보장합니다.

> 권장사항: 성능에 불필요한 영향을 최소화하기 위해, 기본적으로 서드파티 스크립트를 특정 페이지나 레이아웃에만 포함하는 것이 좋습니다.

### 전략

기본 동작을 통해 next/script를 사용하면 어떤 페이지나 레이아웃에서도 서드파티 스크립트를 로드할 수 있지만, strategy 속성을 사용하여 로딩 동작을 세밀하게 조정할 수 있습니다.

<div class="content-ad"></div>

- beforeInteractive: 모든 Next.js 코드 및 페이지 수화가 발생하기 전에 스크립트를 로드합니다.
- afterInteractive: (기본값) 일부 페이지 수화가 발생한 후에 스크립트를 조기에 로드합니다.
- lazyOnload: 브라우저가 유휴 상태일 때 스크립트를 나중에 로드합니다.
- worker: (실험적) 웹 워커에서 스크립트를 로드합니다.

각 전략과 사용 사례에 대해 자세히 알아보려면 next/script API 참조 문서를 참조하세요.

### 웹 워커로 스크립트 오프로딩 (실험적)

> 주의: 워커 전략은 아직 안정화되지 않았으며, 앱 디렉토리와 아직 호환되지 않습니다. 주의해서 사용하세요.

<div class="content-ad"></div>

Worker 전략을 사용하는 스크립트는 Partytown과 함께 웹 워커에서 오프로드되어 실행됩니다. 이를 통해 메인 스레드를 앱 코드의 나머지에 전념하여 사이트의 성능을 향상시킬 수 있습니다.

이 전략은 아직 실험적이며 next.config.js에서 nextScriptWorkers 플래그를 활성화해야만 사용할 수 있습니다:

```js
module.exports = {
  experimental: {
    nextScriptWorkers: true,
  },
}
```

그런 다음, 다음을 실행하세요(next.config.js에서 npm run dev 또는 yarn dev) Next.js가 설정을 마무리하기 위해 필요한 패키지 설치를 안내해 줄 것입니다:

<div class="content-ad"></div>

```js
npm run dev
```

이렇게 지시 사항을 볼 수 있을 거예요: npm install @builder.io/partytown를 실행하여 Partytown을 설치해주세요.

설정이 완료되면 strategy="worker"를 정의하면 Partytown이 자동으로 애플리케이션에 로드되고 스크립트를 웹 워커로 오프로드합니다.

```js
import Script from 'next/script'
 
export default function Home() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="worker" />
    </>
  )
}
```

<div class="content-ad"></div>

웹 워커에서 서드파티 스크립트를 로드할 때 고려해야 할 여러 가지 트레이드오프가 있습니다. 자세한 정보는 Partytown의 트레이드오프 문서를 참조하십시오.

### 인라인 스크립트

외부 파일에서 로드되지 않은 인라인 스크립트는 Script 구성 요소에서도 지원됩니다. 중괄호 안에 JavaScript를 배치하여 작성할 수 있습니다:

```js
<Script id="show-banner">
  {`document.getElementById('banner').classList.remove('hidden')`}
</Script>
```

<div class="content-ad"></div>

```js
<Script
  id="show-banner"
  dangerouslySetInnerHTML={{
    __html: `document.getElementById('banner').classList.remove('hidden')`,
  }}
/>
```

> 주의: Next.js가 스크립트를 추적하고 최적화하기 위해 인라인 스크립트에 id 속성을 할당해야 합니다.

### 추가 코드 실행하기

<div class="content-ad"></div>

이벤트 핸들러를 사용하여 스크립트 구성 요소와 함께 특정 이벤트가 발생한 후 추가 코드를 실행할 수 있어요:

- onLoad: 스크립트가 로드를 완료한 후에 코드를 실행합니다.
- onReady: 스크립트가 로드를 완료한 후와 컴포넌트가 마운트될 때마다 코드를 실행합니다.
- onError: 스크립트 로드에 실패할 때 코드를 실행합니다.

이러한 핸들러는 "use client"이 코드의 첫 줄로 정의된 Client Component 내에서 next/script가 가져와 사용될 때만 작동합니다:

```js
'use client'

import Script from 'next/script'

export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        onLoad={() => {
          console.log('스크립트가 로드되었습니다')
        }
      />
    </>
  )
}
```

<div class="content-ad"></div>

다음/script API 참조를 참고하여 각 이벤트 핸들러에 대해 자세히 알아보고 예시를 확인할 수 있습니다.

### 추가 속성

스크립트 구성 요소에서 사용되지 않는 많은 DOM 속성이 있습니다. 예를 들어 nonce나 사용자 지정 데이터 속성 등이 있습니다. 추가 속성이 있는 경우 최종적으로 HTML에 포함될 최적화된 `script` 요소로 자동으로 전달됩니다.

```js
import Script from 'next/script'

export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        id="example-script"
        nonce="XUENAJFW"
        data-test="script"
      />
    </>
  )
}
```