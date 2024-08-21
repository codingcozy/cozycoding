---
title: "Nextjs 14 절대 경로 임포트와 모듈 경로 별칭 설정하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:46
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Absolute Imports and Module Path Aliases"
link: "https://nextjs.org/docs/app/building-your-application/configuring/absolute-imports-and-module-aliases"
isUpdated: true
---




# 절대 경로 및 모듈 경로 별칭

Next.js에는 tsconfig.json과 jsconfig.json 파일의 "paths" 및 "baseUrl" 옵션을 내장 지원합니다.

이러한 옵션을 사용하면 프로젝트 디렉토리를 절대 경로에 별칭으로 지정하여 모듈을 더 쉽게 가져올 수 있습니다. 예를 들어:

```js
// 이전
import { Button } from '../../../components/button'

// 이후
import { Button } from '@/components/button'
```

<div class="content-ad"></div>

> 좋은 정보입니다: create-next-app이 이러한 옵션을 설정하도록 요청할 것입니다.

## 절대 Imports

baseUrl 구성 옵션을 사용하면 프로젝트의 루트에서 직접 가져올 수 있습니다.

이 구성의 예시:

<div class="content-ad"></div>

```json
{
  "compilerOptions": {
    "baseUrl": "."
  }
}
```

```jsx
export default function Button() {
  return <button>Click me</button>
}
```

```jsx
import Button from 'components/button'
 
export default function HomePage() {
  return (
    <>
      <h1>Hello World</h1>
      <Button />
    </>
  )
}
```

## Module Aliases

<div class="content-ad"></div>

baseUrl 경로를 설정하는 것 외에 "paths" 옵션을 사용하여 모듈 경로를 "별칭"으로 설정할 수도 있습니다.

예를 들어, 다음 구성은 @/components/*를 components/*로 매핑합니다:

```js
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
    "@/components/*": ["components/*"]
    }
  }
}
```

```js
export default function Button() {
  return <button>Click me</button>
}
```

<div class="content-ad"></div>

```jsx
import Button from '@/components/button'

export default function HomePage() {
  return (
    <>
      <h1>Hello World</h1>
      <Button />
    </>
  )
}
```

각 "경로"는 baseUrl 위치를 기준으로 상대 경로입니다. 예를 들면:

```json
// tsconfig.json 또는 jsconfig.json
{
  "compilerOptions": {
    "baseUrl": "src/",
    "paths": {
    "@/styles/*": ["styles/*"],
    "@/components/*": ["components/*"]
  }
}
```

```jsx
// pages/index.js
import Button from '@/components/button'
import '@/styles/styles.css'
import Helper from 'utils/helper'

export default function HomePage() {
  return (
    <Helper>
      <h1>Hello World</h1>
      <Button />
    </Helper>
  )
}
```