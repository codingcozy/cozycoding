---
title: "Nextjs 14 클라이언트 컴포넌트 제대로 이해하기"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:21
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Client Components"
link: "https://nextjs.org/docs/app/building-your-application/rendering/client-components"
isUpdated: true
---




# 클라이언트 구성 요소

클라이언트 구성 요소를 사용하면 서버에서 사전 렌더링된 상호 작용형 UI를 작성하고 클라이언트 JavaScript를 사용하여 브라우저에서 실행할 수 있습니다.

이 페이지에서는 클라이언트 구성 요소가 작동하는 방식, 렌더링되는 방식 및 사용 사례에 대해 알아볼 것입니다.

## 클라이언트 렌더링의 장점

<div class="content-ad"></div>

클라이언트에서 렌더링 작업을 수행하는 것에는 몇 가지 이점이 있습니다:

- 상호 작용성: 클라이언트 구성요소는 상태, 효과 및 이벤트 리스너를 사용할 수 있어 사용자에게 즉각적인 피드백을 제공하고 UI를 업데이트할 수 있습니다.
- 브라우저 API: 클라이언트 구성요소는 지리적 위치 또는 로컬저장소와 같은 브라우저 API에 액세스할 수 있습니다.

## Next.js에서 클라이언트 구성요소 사용하기

클라이언트 구성요소를 사용하려면 파일 상단에 React "use client" 지시문을 추가하여 가져오기 위에 위치시키면 됩니다.

<div class="content-ad"></div>

"use client"은 서버 및 클라이언트 컴포넌트 모듈 간의 경계를 선언하는 데 사용됩니다. 이것은 파일에서 "use client"를 정의함으로써 해당 파일로 가져온 다른 모든 모듈, 포함하여 하위 컴포넌트가 클라이언트 번들의 일부로 간주된다는 것을 의미합니다.

```js
import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  )
}
```

아래 다이어그램은 onClick 및 useState를 중첩 컴포넌트(toggle.js)에서 사용할 때 "use client" 지시문이 정의되지 않으면 오류가 발생한다는 것을 보여줍니다. 이것은 애플리케이션 라우터의 모든 컴포넌트가 기본적으로 서버 컴포넌트이며 이러한 API가 사용 불가능한 경우이기 때문입니다. toggle.js에서 "use client" 지시문을 정의함으로써 React에게 이 API가 사용 가능한 클라이언트 경계로 진입하라고 알릴 수 있습니다.

![Client Components](/assets/img/2024-07-23-ClientComponents_0.png)

<div class="content-ad"></div>

> 다중 사용 클라이언트 엔트리 포인트 정의:
React 컴포넌트 트리에 다중 "사용 클라이언트" 엔트리 포인트를 정의할 수 있습니다. 이를 통해 응용 프로그램을 여러 클라이언트 번들로 분할할 수 있습니다.
그러나 "사용 클라이언트"는 클라이언트에서 렌더링해야 하는 모든 컴포넌트에 정의할 필요가 없습니다. 경계를 정의한 후에는 해당 경계로 가져온 모든 하위 컴포넌트와 모듈이 클라이언트 번들의 일부로 간주됩니다.

## 클라이언트 컴포넌트는 어떻게 렌더링되나요?

Next.js에서 클라이언트 컴포넌트는 전체 페이지 로드(응용프로그램의 초기 방문 또는 브라우저 새로고침에 의한 페이지 다시로드)인 경우나 후속 탐색인 경우에 따라 다르게 렌더링됩니다.

### 전체 페이지 로드

<div class="content-ad"></div>

첫 페이지로드를 최적화하기 위해 Next.js는 클라이언트 및 서버 컴포넌트에 대한 서버 측 정적 HTML 미리보기를 렌더링하기 위해 React의 API를 사용합니다. 이는 사용자가 애플리케이션을 처음 방문할 때 클라이언트가 클라이언트 컴포넌트 JavaScript 번들을 다운로드, 구문 분석 및 실행할 필요 없이 페이지의 내용을 즉시 볼 수 있다는 것을 의미합니다.

서버에서는:

- React는 서버 컴포넌트를 React 서버 컴포넌트 페이로드(RSC 페이로드)라고 하는 특별한 데이터 형식으로 렌더링하며 클라이언트 컴포넌트에 대한 참조를 포함합니다.
- Next.js는 RSC 페이로드 및 클라이언트 컴포넌트 JavaScript 지침을 사용하여 서버에서 라우트에 대한 HTML을 렌더링합니다.

그런 다음 클라이언트에서:

<div class="content-ad"></div>

- HTML은 경로의 빠르고 상호 작용이 없는 초기 미리 보기를 즉시 표시하는 데 사용됩니다.
- React 서버 구성 요소 페이로드는 클라이언트 및 서버 구성 요소 트리를 조화시키고 DOM을 업데이트하는 데 사용됩니다.
- JavaScript 명령은 클라이언트 구성 요소를 수분화시키고 UI를 상호 작용적으로 만드는 데 사용됩니다.

> 수분화는 무엇인가요?
수분화란 정적 HTML을 상호 작용적으로 만들기 위해 DOM에 이벤트 리스너를 부착하는 프로세스입니다. 수분화는 벡그라운드에서 hydrateRoot React API로 수행됩니다.

### 후속 탐색

후속 탐색에서는 클라이언트 구성 요소가 서버에서 렌더링된 HTML 없이 완전히 클라이언트에서 렌더링됩니다.

<div class="content-ad"></div>

이것은 클라이언트 구성 요소 JavaScript 번들이 다운로드되고 구문 분석된다는 것을 의미합니다. 번들이 준비되면 React는 RSC 페이로드를 사용하여 클라이언트 및 서버 구성 요소 트리를 조화시키고 DOM을 업데이트합니다.

## 서버 환경으로 돌아가기

가끔 "use client" 경계를 선언한 후에는 서버 환경으로 돌아가고 싶을 수 있습니다. 예를 들어 클라이언트 번들 크기를 줄이거나 서버에서 데이터를 가져오거나 서버에서만 사용 가능한 API를 사용하고 싶을 수 있습니다.

Client 구성 요소 내에 이론적으로 중첩된 서버 환경의 코드를 유지할 수 있습니다. Client 및 Server 구성 요소와 Server 액션을 교차로 배치하여 Server 환경에서 코드를 유지할 수 있습니다. 자세한 내용은 "구성 패턴" 페이지를 참조하세요.