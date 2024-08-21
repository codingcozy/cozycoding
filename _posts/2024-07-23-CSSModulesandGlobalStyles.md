---
title: "Nextjs 14 CSS 모듈과 글로벌 스타일 활용 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:30
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "CSS Modules and Global Styles"
link: "https://nextjs.org/docs/app/building-your-application/styling/css-modules"
isUpdated: true
---




# CSS 모듈 및 글로벌 스타일

Next.js는 다음과 같은 스타일시트 유형을 지원합니다:

- CSS 모듈
- 글로벌 스타일
- 외부 스타일시트

## CSS 모듈

<div class="content-ad"></div>

Next.js에는 .module.css 확장자를 사용하여 CSS 모듈을 내장 지원하고 있어요.

CSS 모듈은 자동으로 고유한 클래스 이름을 생성하여 CSS를 지역적으로 스코프화합니다. 이를 통해 서로 다른 파일에서 동일한 클래스 이름을 사용할 수 있고 충돌에 대해 걱정할 필요가 없어요. 이러한 동작은 CSS 모듈을 구성 요소 수준의 CSS를 포함하는 이상적인 방법으로 만듭니다.

## 예시

CSS 모듈은 앱 디렉토리 내의 모든 파일로 가져올 수 있어요.

<div class="content-ad"></div>

```js
import styles from './styles.module.css'
 
export default function 대시보드_레이아웃({
  children,
}: {
  children: React.ReactNode
}) {
  return <section className={styles.dashboard}>{children}</section>
}
```

```js
.dashboard {
  padding: 24px;
}
```

CSS 모듈은 .module.css 및 .module.sass 확장자를 가진 파일에만 활성화됩니다.

프로덕션 환경에서는 모든 CSS 모듈 파일이 자동으로 많은 압축된 CSS 파일로 연결됩니다. 이 CSS 파일들은 응용 프로그램에서 핫 실행 경로를 나타내며, 응용 프로그램을 렌더링하는 데 필요한 최소한의 CSS가로드되도록 보장합니다.

<div class="content-ad"></div>

## 전역 스타일

전역 스타일은 앱 디렉토리 내의 모든 레이아웃, 페이지 또는 컴포넌트로 가져올 수 있습니다.

> 참고: 페이지 디렉토리와 다른 점은 _app.js 파일 내에서만 전역 스타일을 가져올 수 있다는 점입니다.

예를 들어, app/global.css라는 스타일 시트를 고려해보겠습니다:

<div class="content-ad"></div>

```js
body {
  padding: 20px 20px 60px;
  max-width: 680px;
  margin: 0 auto;
}
```

루트 레이아웃 안에서 (app/layout.js), 전역 스타일 시트인 global.css를 가져와 애플리케이션의 모든 경로에 스타일을 적용하세요:

```js
// 이 스타일은 애플리케이션의 모든 경로에 적용됩니다
import './global.css'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

## 외부 스타일시트


<div class="content-ad"></div>

외부 패키지에서 발행된 스타일시트는 앱 디렉토리 어디서든 가져올 수 있습니다. 즉, 동일한 위치에 있는 컴포넌트를 포함합니다:

```js
import 'bootstrap/dist/css/bootstrap.css'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body className="container">{children}</body>
    </html>
  )
}
```

> 알아두기: 외부 스타일시트는 npm 패키지에서 직접 가져와야 하거나 코드베이스와 함께 위치시켜 다운로드해야 합니다. `link rel="stylesheet"`을 사용할 수 없습니다.

## 순서와 병합

<div class="content-ad"></div>

Next.js는 프로덕션 빌드 중에 CSS를 최적화하여 자동으로 스타일시트를 청크(병합)합니다. CSS 순서는 애플리케이션 코드로 스타일시트를 가져오는 순서에 따라 결정됩니다.

예를 들어, `BaseButton`이 `Page`에서 첫 번째로 가져오기 때문에 base-button.module.css가 page.module.css보다 우선하여 정렬됩니다:

```js
import styles from './base-button.module.css'

export function BaseButton() {
  return <button className={styles.primary} />
}
```

```js
import { BaseButton } from './base-button'
import styles from './page.module.css'

export function Page() {
  return <BaseButton className={styles.primary} />
}
```

<div class="content-ad"></div>

예측 가능한 순서를 유지하기 위해 다음을 권장합니다:

- CSS 파일은 한 번에 한 JS/TS 파일에서만 import합니다.
전역 클래스 이름을 사용하는 경우, 전역 스타일을 동일한 파일에 원하는 순서로 import하세요.
- CSS 모듈을 전역 스타일보다 선호합니다.
CSS 모듈에 일관된 이름 지정 규칙을 사용하세요. 예를 들어, `name`.module.css 대신 `name`.tsx를 사용하세요.
- 공유 스타일을 별도의 공유 컴포넌트로 추출합니다.
- Tailwind를 사용하는 경우, 파일 상단에 스타일시트를 import하세요. Root Layout에서 하는 것을 권장합니다.

> 유용한 정보: CSS 순서는 개발 모드에서는 다르게 작동합니다. 제품 빌드에서 최종 CSS 순서를 확인하려면 미리 보기 배포를 확인하세요.

## 추가 기능

<div class="content-ad"></div>

Next.js에는 스타일을 추가하는 작성 경험을 개선하기 위한 추가 기능이 포함되어 있습니다:

- next dev로 로컬에서 실행할 때, 로컬 스타일시트(전역 또는 CSS 모듈)는 Fast Refresh를 활용하여 편집을 저장할 때 즉시 변경 내용을 반영합니다.
- next build로 프로덕션용으로 빌드할 때, CSS 파일은 더 적은 수의 압축된 .css 파일로 번들링되어 스타일을 받아오기 위한 네트워크 요청 수를 줄입니다.
- JavaScript를 비활성화하면 스타일은 여전히 프로덕션 빌드(next start)에서 로드됩니다. 그러나 Fast Refresh를 사용하려면 JavaScript를 비활성화해도 되지 않습니다.