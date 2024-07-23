---
title: "Nextjs 14에서 Font Optimization 쉽게 하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:37
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Font Optimization"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/fonts"
---


# 폰트 최적화

next/font는 자동으로 당신의 폰트(사용자 정의 폰트 포함)를 최적화하고 개인 정보 보호와 성능 향상을 위해 외부 네트워크 요청을 제거합니다.

> 🎥 시청: next/font 사용에 대해 더 알아보기 → YouTube (6분)
.

next/font에는 글꼴 파일을 위한 내장 자체 호스팅이 포함되어 있습니다. 이는 사용된 CSS size-adjust 속성 덕분에 레이아웃 변화 없이 웹 폰트를 최적으로 로드할 수 있다는 뜻입니다.

<div class="content-ad"></div>

이 새로운 폰트 시스템은 모든 Google Font를 편리하게 사용할 수 있도록 해주며 성능과 개인 정보 보호를 고려합니다. CSS 및 폰트 파일은 빌드 시간에 다운로드되어 다른 정적 자산과 함께 자체 호스팅됩니다. 브라우저가 Google로 요청을 보내지 않습니다.

## Google Fonts

Google Font를 자동으로 자체 호스팅합니다. 폰트는 배포에 포함되어 배포 도메인에서 제공됩니다. 브라우저가 Google로 요청을 보내지 않습니다.

next/font/google에서 사용할 폰트를 가져와서 함수로 사용하여 시작할 수 있습니다. 최상의 성능과 유연성을 위해 변수 폰트를 사용하는 것을 권장합니다.

<div class="content-ad"></div>

```js
import { Inter } from 'next/font/google'

// 만약 변수 글꼴을 로드한다면 폰트 무게를 지정할 필요가 없습니다.
const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
})

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

만약 변수 글꼴을 사용할 수 없다면, 폰트 무게를 지정해야 합니다.

```js
import { Roboto } from 'next/font/google'
 
const roboto = Roboto({
  weight: '400',
  subsets: ['latin'],
  display: 'swap',
})
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={roboto.className}>
      <body>{children}</body>
    </html>
  )
}
```

여러 무게와/또는 스타일을 지정하려면 배열을 사용할 수 있습니다:

<div class="content-ad"></div>

```js
const roboto = Roboto({
  weight: ['400', '700'],
  style: ['normal', 'italic'],
  subsets: ['latin'],
  display: 'swap',
})
```

> 참고: 여러 단어로 된 글꼴 이름은 밑줄(_)을 사용합니다. 예: Roboto Mono는 Roboto_Mono으로 가져와야 합니다.

### 하위 집합 지정

Google 글꼴은 자동으로 하위 집합으로 분할됩니다. 이렇게 하면 글꼴 파일의 크기가 줄어들고 성능이 향상됩니다. 미리로드할 하위 집합을 정의해야 합니다. preload를 true로 설정하면서 어떤 하위 집합도 지정하지 않으면 경고가 발생합니다.

<div class="content-ad"></div>

다음과 같이 함수 호출에 추가하면 가능합니다:

```js
const inter = Inter({ subsets: ['latin'] })
```

더 많은 정보를 보려면 폰트 API 참조를 확인해주세요.

### 여러 글꼴 사용하기

<div class="content-ad"></div>

어플리케이션에서 여러 폰트를 가져와 사용할 수 있어요. 두 가지 방법을 사용할 수 있어요.

첫 번째 방법은 유틸리티 함수를 만들어서 폰트를 내보내고 가져와서 필요한 곳에 클래스 이름을 적용하는 것이에요. 이를 통해 렌더링할 때 폰트가 사전로드되도록 할 수 있어요:

```js
import { Inter, Roboto_Mono } from 'next/font/google'
 
export const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
})
 
export const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  display: 'swap',
})
```

```js
import { inter } from './fonts'
 
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={inter.className}>
      <body>
        <div>{children}</div>
      </body>
    </html>
  )
}
```

<div class="content-ad"></div>

```js
import { roboto_mono } from './fonts'

export default function Page() {
  return (
    <>
      <h1 className={roboto_mono.className}>나의 페이지</h1>
    </>
  )
}
```

위 예제에서는 Inter가 전역적으로 적용되며, Roboto Mono는 필요에 따라 가져와 적용할 수 있습니다.

또는 CSS 변수를 만들고 선호하는 CSS 솔루션과 함께 사용할 수 있습니다:

```js
import { Inter, Roboto_Mono } from 'next/font/google'
import styles from './global.css'

const inter = Inter({
  subsets: ['latin'],
  variable: '--font-inter',
  display: 'swap',
})

const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  variable: '--font-roboto-mono',
  display: 'swap',
})

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={`${inter.variable} ${roboto_mono.variable}`}>
      <body>
        <h1>나의 앱</h1>
        <div>{children}</div>
      </body>
    </html>
  )
}
```

<div class="content-ad"></div>

```css
html {
  font-family: var(--font-inter);
}

h1 {
  font-family: var(--font-roboto-mono);
}
```

위 예시에서 Inter는 전역적으로 적용되며, `h1` 태그는 Roboto Mono로 스타일이 지정됩니다.

> 권장 사항: 클라이언트가 다운로드해야 하는 추가 자원이므로 각 새로운 글꼴을 신중하게 사용하십시오.

## 로컬 글꼴

<div class="content-ad"></div>

다음으로, next/font/local을 가져와 로컬 폰트 파일의 src를 지정하세요. 가장 좋은 성능과 유연성을 위해 변수 폰트를 사용하는 것이 좋습니다.

```js
import localFont from 'next/font/local'

// 폰트 파일은 `app` 내부에 함께 배치할 수 있습니다
const myFont = localFont({
  src: './my-font.woff2',
  display: 'swap',
})

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={myFont.className}>
      <body>{children}</body>
    </html>
  )
}
```

동일한 글꼴군에 대해 여러 파일을 사용하려면 src를 배열로 지정할 수 있습니다:

```js
const roboto = localFont({
  src: [
    {
      path: './Roboto-Regular.woff2',
      weight: '400',
      style: 'normal',
    },
    {
      path: './Roboto-Italic.woff2',
      weight: '400',
      style: 'italic',
    },
    {
      path: './Roboto-Bold.woff2',
      weight: '700',
      style: 'normal',
    },
    {
      path: './Roboto-BoldItalic.woff2',
      weight: '700',
      style: 'italic',
    },
  ],
})
```

<div class="content-ad"></div>

폰트 API 참조를 확인해보세요.

## Tailwind CSS 사용 시

next/font는 Tailwind CSS에서 CSS 변수를 통해 사용할 수 있습니다.

아래 예시에서는 next/font/google의 Inter 폰트를 사용합니다 (Google이나 로컬 폰트 중 아무 폰트를 사용할 수 있습니다). 폰트를 로드할 때 변수 옵션을 사용하여 CSS 변수 이름을 정의하고 그것을 inter에 할당하세요. 그런 다음, inter.variable을 사용하여 CSS 변수를 HTML 문서에 추가하세요.

<div class="content-ad"></div>

```js
import { Inter, Roboto_Mono } from 'next/font/google'

const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-inter',
})

const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-roboto-mono',
})

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={`${inter.variable} ${roboto_mono.variable}`}>
      <body>{children}</body>
    </html>
  )
}
```

마지막으로, Tailwind CSS 구성에 CSS 변수를 추가하세요:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx}',
    './components/**/*.{js,ts,jsx,tsx}',
    './app/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {
      fontFamily: {
        sans: ['var(--font-inter)'],
        mono: ['var(--font-roboto-mono)'],
      },
    },
  },
  plugins: [],
}
```

이제 font-sans 및 font-mono 유틸리티 클래스를 사용하여 요소에 글꼴을 적용할 수 있습니다.


<div class="content-ad"></div>

## 사전로드

웹 사이트의 페이지에서 글꼴 함수를 호출할 때, 해당 글꼴은 전역적으로 사용 가능하게 사전로드되지 않습니다. 대신 사용된 파일 유형에 따라 관련 경로에만 사전로드됩니다:

- 고유 페이지인 경우 해당 페이지의 고유 경로에 사전로드됩니다.
- 레이아웃인 경우 해당 레이아웃으로 둘러싸인 모든 경로에 사전로드됩니다.
- 루트 레이아웃인 경우 모든 경로에 사전로드됩니다.

## 글꼴 재사용

<div class="content-ad"></div>

매번 로컬 폰트 또는 구글 폰트 함수를 호출할 때, 해당 폰트는 애플리케이션에서 하나의 인스턴스로 호스팅됩니다. 따라서 동일한 폰트 함수를 여러 파일에서 불러오면 동일한 폰트의 여러 인스턴스가 호스팅됩니다. 이런 상황에서는 다음을 권장합니다:

- 폰트 로더 함수를 공유 파일 하나에 호출합니다.
- 상수로 내보냅니다.
- 이 폰트를 사용하려는 각 파일에서 해당 상수를 가져옵니다.