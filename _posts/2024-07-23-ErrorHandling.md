---
title: "Nextjs 14에서 에러 핸들링 방법 완벽 가이드"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:07
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Error Handling"
link: "https://nextjs.org/docs/app/building-your-application/routing/error-handling"
---


# 오류 처리

에러.js 파일 규칙을 따르면 중첩된 라우트에서 예기치 않은 런타임 오류를 우아하게 처리할 수 있습니다.

- React 에러 경계를 자동으로 랩핑하여 라우트 세그먼트와 그 하위 자식들을 처리하세요.
- 파일 시스템 계층 구조를 사용하여 특정 세그먼트에 맞는 오류 UI를 생성하세요.
- 영향받는 세그먼트에만 오류를 격리시키면서 나머지 애플리케이션은 정상적으로 작동합니다.
- 전체 페이지 다시로드 없이 오류에서 복구를 시도하는 기능을 추가하세요.

오류 UI를 만들려면 라우트 세그먼트 내에 error.js 파일을 추가하고 React 컴포넌트를 내보내세요.

<div class="content-ad"></div>

<table> 태그를 Markdown 형식으로 변환하세요.

![Error Handling Image](/assets/img/2024-07-23-ErrorHandling_0.png)

```js
'use client' // 에러 컴포넌트는 Client Components여야 합니다

import { useEffect } from 'react'

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    // 에러를 에러 보고 서비스에 기록합니다
    console.error(error)
  }, [error])

  return (
    <div>
      <h2>문제가 발생했습니다!</h2>
      <button
        onClick={
          // 세그먼트를 다시 렌더링하여 복구를 시도합니다
          () => reset()
        }
      >
        다시 시도
      </button>
    </div>
  )
}
```

### error.js 작동 방식

![Error Handling Image](/assets/img/2024-07-23-ErrorHandling_1.png)

<div class="content-ad"></div>

- error.js는 중첩된 하위 세그먼트나 page.js 컴포넌트를 감싸는 React 에러 경계를 자동으로 생성합니다.
- error.js 파일에서 내보낸 React 컴포넌트가 백업 컴포넌트로 사용됩니다.
- 에러 경계 내에서 에러가 발생하면 해당 에러를 포착하고 백업 컴포넌트가 렌더링됩니다.
- 백업 에러 컴포넌트가 활성화된 경우, 에러 경계 위의 레이아웃은 상태를 유지하고 상호 작용을 유지하며, 에러 컴포넌트는 에러에서 복구할 수 있는 기능을 표시할 수 있습니다.

### 에러에서 복구하기

에러의 원인은 가끔 일시적일 수 있습니다. 이러한 경우에는 단순히 다시 시도하면 문제가 해결될 수 있습니다.

에러 컴포넌트는 reset() 함수를 사용하여 사용자에게 에러에서 복구를 시도하도록 시도할 수 있습니다. 이 함수를 실행하면 에러 경계의 내용을 다시 렌더링하려고 시도합니다. 성공하면 백업 에러 컴포넌트가 다시 렌더링 결과로 대체됩니다.

<div class="content-ad"></div>

```js
'사용자'
 
export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <div>
      <h2>문제가 발생했습니다!</h2>
      <button onClick={() => reset()}>다시 시도</button>
    </div>
  )
}
```

### 중첩 라우트

특별한 파일을 통해 생성된 React 구성 요소는 특정 중첩 계층 구조에 렌더링됩니다.

예를 들어, layout.js와 error.js 파일을 모두 포함하는 두 세그먼트로 구성된 중첩 라우트는 다음과 같은 간소화된 구성 요소 계층에 렌더링됩니다:

<div class="content-ad"></div>


![Error Handling](/assets/img/2024-07-23-ErrorHandling_2.png)

중첩 컴포넌트 계층 구조는 중첩된 경로 전체에서 error.js 파일의 동작에 영향을 미칩니다:

- 오류는 가장 가까운 부모 오류 경계로 전파됩니다. 이는 error.js 파일이 모든 중첩된 하위 세그먼트의 오류를 처리한다는 것을 의미합니다. error.js 파일을 경로의 중첩된 폴더의 다른 수준에 배치함으로써 더 또는 덜 세분화된 오류 UI를 달성할 수 있습니다.
- 오류가 발생한 레이아웃.js 컴포넌트에서 throw된 오류를 error.js 경계가 처리하지 않습니다. 그 이유는 오류 경계가 그 레이아웃의 컴포넌트 내에 중첩되어 있기 때문입니다.

### 레이아웃에서 오류 처리


<div class="content-ad"></div>

에러.js 경계는 동일 세그먼트의 layout.js 또는 template.js 구성 요소에서 발생한 에러를 잡지 못합니다. 이 계층 구조는 에러가 발생할 때 동일한 라우트 간에 공유되는 중요한 UI(예: 내비게이션)가 보이고 기능적으로 유지되도록 합니다.

특정 레이아웃이나 템플릿 내에서 에러를 처리하려면 레이아웃의 상위 세그먼트에 에러.js 파일을 배치하십시오.

루트 레이아웃이나 템플릿에서 에러를 처리하려면 global-error.js라는 에러.js의 변형을 사용하십시오.

### 루트 레이아웃에서 에러 처리하기

<div class="content-ad"></div>

루트 앱/error.js 경계는 루트 앱/layout.js나 app/template.js 컴포넌트에서 발생한 오류를 잡지 못합니다.

이러한 루트 컴포넌트에서 오류를 명시적으로 처리하려면 루트 앱 디렉토리에 있는 error.js의 변형인 app/global-error.js를 사용하세요.

루트 error.js와는 달리 global-error.js 오류 경계는 전체 애플리케이션을 감싸며, 활성화되는 경우 루트 레이아웃을 대체하는 여분 컴포넌트를 갖습니다. 따라서 global-error.js는 자체 `html` 및 `body` 태그를 정의해야 합니다.

global-error.js는 가장 세분화된 오류 UI이며 전체 애플리케이션에 대한 "캐치 올" 오류 처리로 간주될 수 있습니다. 루트 컴포넌트가 일반적으로 덜 동적이기 때문에 자주 트리거되지는 않을 것이며, 대부분의 오류는 다른 error.js 경계에서 잡을 것입니다.

<div class="content-ad"></div>

그래도 global-error.js가 정의되어있더라도, 루트 에러.js를 정의하는 것이 좋습니다. 루트 에러.js에는 대체 컴포넌트가 렌더링되며, 전역으로 공유되는 UI 및 브랜딩이 포함된 루트 레이아웃에서 표시됩니다.

```js
'use client'
 
export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <html>
      <body>
        <h2>에러가 발생했습니다!</h2>
        <button onClick={() => reset()}>다시 시도</button>
      </body>
    </html>
  )
}
```

> 유의사항:
global-error.js는 프로덕션 환경에서만 활성화됩니다. 개발 환경에서는 오류 오버레이가 대신 표시됩니다.

### 서버 오류 처리

<div class="content-ad"></div>

서버 컴포넌트 내에서 오류가 발생하면, Next.js에서는 Error 객체를 가장 가까운 error.js 파일로 오류 속성으로 전달합니다. (프로덕션 환경에서는 민감한 오류 정보가 제거됩니다.)

#### 민감한 오류 정보 보호

프로덕션 환경에서, 클라이언트로 전달되는 Error 객체에는 일반 메시지와 다이제스트 속성만 포함됩니다.

이것은 오류에 포함된 잠재적으로 민감한 세부 정보가 클라이언트로 노출되는 것을 방지하기 위한 보안 조치입니다.

<div class="content-ad"></div>

개발 중에는 클라이언트로 전송된 오류 객체가 직렬화되어 해당 오류의 메시지를 포함하므로 디버깅이 좀 더 쉽습니다.