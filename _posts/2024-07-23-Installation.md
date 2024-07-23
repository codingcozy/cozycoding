---
title: "Nextjs 14 설치 가이드"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:00
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Installation"
link: "https://nextjs.org/docs/getting-started/installation"
---


# 설치

시스템 요구 사항:

- Node.js 18.17
또는 그 이상 버전.
- macOS, Windows (WSL 포함), 그리고 Linux를 지원합니다.

## 자동 설치

<div class="content-ad"></div>

Next.js 앱을 만들 때 create-next-app을 사용하는 것을 추천드립니다. 이 도구를 이용하면 모든 설정을 자동으로 해줍니다. 프로젝트를 만들려면 다음 명령을 실행하세요:

```js
npx create-next-app@latest
```

설치 시 다음과 같은 프롬프트가 나타납니다:

```js
프로젝트 이름은 무엇으로 하시겠습니까? my-app
TypeScript를 사용하시겠습니까? No / Yes
ESLint를 사용하시겠습니까? No / Yes
Tailwind CSS를 사용하시겠습니까? No / Yes
`src/` 디렉토리를 사용하시겠습니까? No / Yes
App Router를 사용하시겠습니까? (권장) No / Yes
기본 import alias인 @/*를 사용자 정의하시겠습니까? No / Yes
구성할 import alias는 무엇입니까? @/*
```

<div class="content-ad"></div>

프롬프트를 따르면 create-next-app은 프로젝트 이름으로 폴더를 생성하고 필요한 종속성을 설치합니다.

Next.js를 처음 사용하시는 경우 애플리케이션 내 모든 가능한 파일과 폴더에 대한 개요를 보려면 프로젝트 구조 문서를 참조하십시오.

> 알아 두면 좋은 사실:
Next.js는 이제 TypeScript, ESLint 및 기본적으로 Tailwind CSS 구성을 함께 제공합니다.
필요에 따라 프로젝트의 루트에 src 디렉토리를 사용하여 애플리케이션 코드와 구성 파일을 분리할 수 있습니다.

## 수동 설치

<div class="content-ad"></div>

새로운 Next.js 앱을 수동으로 만들려면 필요한 패키지를 설치하세요:

```js
npm install next@latest react@latest react-dom@latest
```

package.json 파일을 열고 다음 스크립트를 추가하세요:

```js
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  }
}
```

<div class="content-ad"></div>

이 스크립트들은 애플리케이션 개발의 다양한 단계를 참조합니다:

- dev: Next.js를 개발 모드로 시작하는 데 사용됩니다.
- build: 어플리케이션을 프로덕션 용도로 빌드하는 데 사용됩니다.
- start: Next.js 프로덕션 서버를 시작하는 데 사용됩니다.
- lint: Next.js의 내장 ESLint 구성을 설정하는 데 사용됩니다.

### 디렉토리 생성

Next.js는 파일 시스템 라우팅을 사용하므로 애플리케이션의 경로는 파일 구조에 따라 결정됩니다.

<div class="content-ad"></div>

#### 앱 디렉토리

새로운 애플리케이션을 만들 때는 App Router를 사용하는 것을 권장합니다. 이 라우터를 사용하면 React의 최신 기능을 활용할 수 있으며, 커뮤니티 피드백을 기반으로 한 Pages Router의 진화 버전입니다.

앱/ 폴더를 생성한 후 layout.tsx와 page.tsx 파일을 추가하세요. 이 파일들은 사용자가 애플리케이션의 루트(/)를 방문했을 때 렌더링됩니다.

![Installation_0](/assets/img/2024-07-23-Installation_0.png)

<div class="content-ad"></div>

app/layout.tsx 파일 안에 `html` 및 `body` 태그가 필요한 root 레이아웃을 만들어주세요:

```js
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

마지막으로, 초기 콘텐츠가 포함된 home 페이지인 app/page.tsx 파일을 생성해주세요:

```js
export default function Page() {
  return <h1>Hello, Next.js!</h1>
}
```

<div class="content-ad"></div>

> 좋은 정보: 만약 layout.tsx 파일을 만드는 것을 잊는다면, Next.js는 개발 서버를 실행할 때 이 파일을 자동으로 생성해 줍니다.

App Router 사용에 대해 더 알아보세요.

#### 페이지 디렉토리 (선택 사항)

App Router 대신 Pages Router를 사용하고 싶다면, 프로젝트 루트에 pages/ 디렉토리를 생성할 수 있습니다.

<div class="content-ad"></div>

그럼, 페이지 폴더 안에 index.tsx 파일을 추가해보세요. 이것이 홈페이지(/)가 될 것입니다:

```js
export default function Page() {
  return <h1>Hello, Next.js!</h1>
}
```

그 다음, 전역 레이아웃을 정의하기 위해 페이지 폴더 안에 _app.tsx 파일을 추가하세요. 사용자 정의 App 파일에 대해 더 알아보세요.

```js
import type { AppProps } from 'next/app'

export default function App({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />
}
```

<div class="content-ad"></div>

마지막으로, 서버로부터의 초기 응답을 제어하기 위해 페이지 내부에 _document.tsx 파일을 추가하세요. 사용자 정의 Document 파일에 대해 더 알아보세요.

```js
import { Html, Head, Main, NextScript } from 'next/document'
 
export default function Document() {
  return (
    <Html>
      <Head />
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  )
}
```

페이지 라우터 사용에 대해 더 알아보세요.

> 알아두면 좋은 사실: 같은 프로젝트에서 두 라우터를 모두 사용할 수 있지만, 앱 내의 라우트가 페이지보다 우선됩니다. 혼란을 피하려면 새 프로젝트에서는 한 가지 라우터만 사용하는 것이 좋습니다.

<div class="content-ad"></div>

#### 공개 폴더 (선택 사항)

정적 자산인 이미지, 폰트 등을 저장하기 위한 공개 폴더를 만들어주세요. 공개 디렉토리 안의 파일은 기본 URL(/)을 기준으로 코드에서 참조할 수 있습니다.

## 개발 서버 실행

- npm run dev 명령어를 실행하여 개발 서버를 시작해주세요.
- 애플리케이션을 확인하려면 http://localhost:3000에 방문해주세요.
- app/page.tsx (또는 pages/index.tsx) 파일을 편집하고 브라우저에서 결과를 확인하기 위해 저장해주세요.