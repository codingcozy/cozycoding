---
title: "Nextjs 14에서 Jest 설정하는 방법 최신 가이드 및 팁"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:49
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Setting up Jest with Next.js"
link: "https://nextjs.org/docs/app/building-your-application/testing/jest"
isUpdated: true
---




# Next.js에서 Jest 설정하기

Jest와 React Testing Library는 주로 Unit 테스트 및 스냅샷 테스트에 함께 사용됩니다. 이 안내서에서는 Next.js에서 Jest를 설정하고 첫 번째 테스트를 작성하는 방법을 안내합니다.

> 참고: 비동기 서버 컴포넌트는 React 생태계에서 새롭기 때문에 Jest는 현재 지원하지 않습니다. 동기 서버 및 클라이언트 컴포넌트에 대한 단위 테스트를 실행할 수는 있지만, 비동기 컴포넌트에 대해서는 E2E 테스트를 권장합니다.

## 빠른 시작

<div class="content-ad"></div>

빠르게 시작하려면 Next.js의 with-jest 예제와 함께 create-next-app을 사용할 수 있어요:

```js
npx create-next-app@latest --example with-jest with-jest-app
```

## 수동 설정

Next.js 12가 출시되면서 이제 Jest를 위한 기본 구성이 내장되어 있어요.

<div class="content-ad"></div>

Jest를 설정하려면 다음 패키지를 설치하세요:

```js
npm install -D jest jest-environment-jsdom @testing-library/react @testing-library/jest-dom
# or
yarn add -D jest jest-environment-jsdom @testing-library/react @testing-library/jest-dom
# or
pnpm install -D jest jest-environment-jsdom @testing-library/react @testing-library/jest-dom
```

다음 명령어를 실행하여 기본 Jest 구성 파일을 생성하세요:

```js
npm init jest@latest
# or
yarn create jest@latest
# or
pnpm create jest@latest
```

<div class="content-ad"></div>

프로젝트를 설정하는 일련의 프롬프트를 따라 Jest를 설정하는 과정을 안내합니다. 이 과정에서 jest.config.ts|js 파일을 자동으로 생성할 것입니다.

next/jest를 사용하도록 구성 파일을 업데이트하세요. 이 변환기에는 Next.js와 함께 Jest가 작동하도록 필요한 모든 구성 옵션이 포함되어 있습니다:

```js
import type { Config } from 'jest'
import nextJest from 'next/jest.js'
 
const createJestConfig = nextJest({
  // 테스트 환경에서 next.config.js 및 .env 파일을 로드하기 위해 Next.js 앱의 경로를 제공하세요
  dir: './',
})
 
// Jest에 전달될 사용자 지정 구성을 추가하세요
const config: Config = {
  coverageProvider: 'v8',
  testEnvironment: 'jsdom',
  // 각 테스트가 실행되기 전에 추가 설정 옵션을 추가하세요
  // setupFilesAfterEnv: ['<rootDir>/jest.setup.ts'],
}
 
// next/jest가 Next.js 구성을 비동기로 로드할 수 있도록 createJestConfig가 이 방식으로 내보내기됩니다.
export default createJestConfig(config)
```

내부적으로 next/jest는 자동으로 Jest를 구성하여 다음을 포함합니다:

<div class="content-ad"></div>

- Next.js 컴파일러를 사용하여 transform 설정하기
- .css, .module.css 및 scss 변형 파일, 이미지 임포트 및 next/font 자동 모킹 스타일시트 설정하기
- .env 파일(및 모든 변형 파일)을 process.env에 로드하기
- 테스트 생성 및 변형에서 node_modules 무시하기
- 테스트 생성에서 .next 파일을 무시하기
- SWC 변형을 활성화하는 플래그를 가지고 있는 next.config.js 파일 로드하기

> 참고: 환경 변수를 직접 테스트하려면 별도의 설정 스크립트 또는 jest.config.ts 파일에 수동으로 로드하십시오. 더 많은 정보는 "테스트 환경 변수"를 참조하십시오.

## 선택 사항: 절대 임포트 및 모듈 경로 별칭 처리

프로젝트에서 모듈 경로 별칭을 사용하는 경우 Jest를 구성하여 jsconfig.json 파일의 paths 옵션과 jest.config.js 파일의 moduleNameMapper 옵션을 일치시켜 임포트를 해결해야 합니다. 예를 들어:

<div class="content-ad"></div>

```js
{
  "compilerOptions": {
    "module": "esnext",
    "moduleResolution": "bundler",
    "baseUrl": "./",
    "paths": {
      "@/components/*": ["components/*"]
    }
  }
}
```

```js
moduleNameMapper: {
  // ...
  '^@/components/(.*)$': '<rootDir>/components/$1',
}
```

## Optional: Extend Jest with custom matchers

@testing-library/jest-dom에는 .toBeInTheDocument()과 같이 편리한 사용자 정의 매처들이 포함되어 있어 테스트 작성이 더 쉬워집니다. 테스트 파일마다 사용자 정의 매처를 가져오기 위해 Jest 구성 파일에 다음 옵션을 추가할 수 있습니다.

<div class="content-ad"></div>

```js
setupFilesAfterEnv: ['<rootDir>/jest.setup.ts']
```

그런 다음, jest.setup.ts 파일 내부에 다음 import를 추가해주세요:

```js
import '@testing-library/jest-dom'
```

> 참고: v6.0에서 extend-expect는 제거되었으므로, 버전 6 이전에 @testing-library/jest-dom을 사용 중이라면 @testing-library/jest-dom/extend-expect를 import해야 합니다.

<div class="content-ad"></div>

만일 각 테스트 전에 더 많은 설정 옵션을 추가해야 한다면 jest.setup.js 파일에 추가할 수 있어요.

## package.json에 테스트 스크립트 추가하기:

마지막으로, package.json 파일에 Jest 테스트 스크립트를 추가해주세요:

```js
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
  }
}
```

<div class="content-ad"></div>

jest --watch 명령어를 사용하면 파일이 변경될 때마다 테스트가 다시 실행됩니다. 더 많은 Jest CLI 옵션을 원하신다면 Jest 문서를 참조해주세요.

### 첫 번째 테스트 생성하기:

프로젝트가 테스트를 실행할 준비가 되었습니다. 프로젝트의 루트 디렉토리에 __tests__라는 폴더를 만드세요.

예를 들어, `Page /` 컴포넌트가 제목을 성공적으로 렌더링하는지 확인하는 테스트를 추가할 수 있습니다:

<div class="content-ad"></div>

```js
import Link from 'next/link'
 
export default function Home() {
  return (
    <div>
      <h1>Home</h1>
      <Link href="/about">About</Link>
    </div>
  )
}
```

```js
import '@testing-library/jest-dom'
import { render, screen } from '@testing-library/react'
import Page from '../app/page'
 
describe('Page', () => {
  it('renders a heading', () => {
    render(<Page />)
 
    const heading = screen.getByRole('heading', { level: 1 })
 
    expect(heading).toBeInTheDocument()
  })
}
```

선택 사항으로 컴포넌트에 예기치 않은 변경 사항을 추적하기 위한 스냅샷 테스트를 추가해보세요:

```js
import { render } from '@testing-library/react'
import Page from '../app/page'
 
it('renders homepage unchanged', () => {
  const { container } = render(<Page />)
  expect(container).toMatchSnapshot()
}
```

<div class="content-ad"></div>

## 테스트 실행하기

다음 명령어를 사용하여 테스트를 실행하세요:

```js
npm run test
# 또는
yarn test
# 또는
pnpm test
```

## 추가 자료

<div class="content-ad"></div>

더 읽을 자료를 찾아보시면 도움이 될 것 같아요:

- Next.js와 Jest 예제
- Jest 문서
- React Testing Library 문서
- Testing Playground
- 요소를 일치시키기 위해 좋은 테스트 방법을 사용해보세요.