---
title: "Nextjs 14에서 Cypress 설정하는 방법 초보자 가이드"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:50
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Setting up Cypress with Next.js"
link: "https://nextjs.org/docs/app/building-your-application/testing/cypress"
isUpdated: true
---




# 넥스트.자스와 함께 사이프레스 설정하기

사이프레스는 End-to-End (E2E) 및 컴포넌트 테스트에 사용되는 테스트 러너입니다. 이 페이지에서는 넥스트.자스와 함께 사이프레스를 설정하고 첫 번째 테스트를 작성하는 방법을 안내해 드립니다.

> 주의:
컴포넌트 테스트의 경우, 사이프레스는 현재 넥스트.자스 버전 14와 async 서버 컴포넌트를 지원하지 않습니다. 이 문제들은 추적 중입니다. 현재 컴포넌트 테스트는 넥스트.자스 버전 13에서 작동하며, async 서버 컴포넌트에 대해서는 E2E 테스트를 권장합니다.
사이프레스는 현재 TypeScript 버전 5와 moduleResolution: "bundler"를 지원하지 않습니다. 이 문제는 추적 중에 있습니다.

## 빠른 시작

<div class="content-ad"></div>

create-next-app을 사용하여 with-cypress 예제를 통해 빠르게 시작할 수 있어요.

```js
npx create-next-app@latest --example with-cypress with-cypress-app
```

## 수동 설정

수동으로 Cypress를 설정하려면, cypress를 개발 의존성으로 설치하세요:

<div class="content-ad"></div>

```js
npm install -D cypress
# 또는
yarn add -D cypress
# 또는
pnpm install -D cypress
```

package.json 스크립트 필드에 Cypress를 실행하는 명령어를 추가하세요:

```js
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open"
  }
}
```

첫 번째로 Cypress를 실행하여 Cypress 테스트 스위트를 엽니다:

<div class="content-ad"></div>

```js
npm run cypress:open
```

E2E 테스트 및/또는 컴포넌트 테스트를 구성할 수 있습니다. 이러한 옵션 중 하나를 선택하면 프로젝트에 cypress.config.js 파일과 cypress 폴더가 자동으로 생성됩니다.

## 첫 번째 Cypress E2E 테스트 작성 방법

cypress.config.js 파일이 다음 구성을 갖고 있는지 확인하세요.

<div class="content-ad"></div>

```js
import { defineConfig } from 'cypress'

export default defineConfig({
  e2e: {
    setupNodeEvents(on, config) {},
  },
})
```

```js
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    setupNodeEvents(on, config) {},
  },
})
```

그런 다음 두 개의 새로운 Next.js 파일을 생성하십시오:

```js
import Link from 'next/link'

export default function Page() {
  return (
    <div>
      <h1>Home</h1>
      <Link href="/about">About</Link>
    </div>
  )
}
```

<div class="content-ad"></div>

```js
import Link from 'next/link'
 
export default function Page() {
  return (
    <div>
      <h1>About</h1>
      <Link href="/">Home</Link>
    </div>
  )
}
```

내비게이션이 올바르게 작동하는지 확인하는 테스트를 추가해보세요:

```js
describe('Navigation', () => {
  it('should navigate to the about page', () => {
    // 처음에는 홈페이지에서 시작합니다
    cy.visit('http://localhost:3000/')
 
    // "about"이라는 href 속성을 포함하는 링크를 찾아 클릭합니다
    cy.get('a[href*="about"]').click()
 
    // 새로운 URL에 "/about"이 포함되어야 합니다
    cy.url().should('include', '/about')
 
    // 새 페이지에는 "About"이라는 제목이 있는 h1 요소가 있어야 합니다
    cy.get('h1').contains('About')
  })
})
```

### E2E 테스트 실행하기


<div class="content-ad"></div>

심심하지 않게 릴레이하면, Cypress는 사용자가 애플리케이션을 탐색하는 것을 시뮬레이션합니다. 이를 위해서는 Next.js 서버가 실행 중이어야 합니다. 생산 코드에 대한 테스트를 실행하여 애플리케이션이 어떻게 동작할지를 더 가깝게 반영할 것을 권장합니다.

Next.js 애플리케이션을 빌드하려면 npm run build && npm run start를 실행한 후, 다른 터미널 창에서 npm run cypress:open을 실행하여 Cypress를 시작하고 E2E 테스트 스위트를 실행하세요.

> 알아두면 좋은 사실:
cypress.config.js 구성 파일에 baseUrl: 'http://localhost:3000'을 추가하여 cy.visit("/") 대신 cy.visit("http://localhost:3000/")를 사용할 수 있습니다.
또는 start-server-and-test 패키지를 설치하여 Next.js 프로덕션 서버를 Cypress와 함께 실행할 수 있습니다. 설치 후 package.json 스크립트 필드에 "test": "start-server-and-test start http://localhost:3000 cypress"를 추가하세요. 새 변경 사항 이후 애플리케이션을 다시 빌드하는 것을 잊지 마세요.

## 첫 번째 Cypress 컴포넌트 테스트 만들기

<div class="content-ad"></div>

컴포넌트 테스트는 전체 응용 프로그램을 번들하거나 서버를 시작할 필요 없이 특정 컴포넌트를 빌드하고 마운트합니다.

Cypress 앱에서 Component Testing을 선택한 다음 프론트엔드 프레임워크로 Next.js를 선택하십시오. 프로젝트에 cypress/component 폴더가 생성되고 cypress.config.js 파일이 업데이트되어 컴포넌트 테스트를 활성화합니다.

cypress.config.js 파일이 다음 구성을 가지고 있는지 확인하세요:

```js
import { defineConfig } from 'cypress'

export default defineConfig({
  component: {
    devServer: {
      framework: 'next',
      bundler: 'webpack',
    },
  },
})
```

<div class="content-ad"></div>

```js
const { defineConfig } = require('cypress')
 
module.exports = defineConfig({
  component: {
    devServer: {
      framework: 'next',
      bundler: 'webpack',
    },
  },
})
```

이전 섹션에서와 같은 컴포넌트를 전제로 하여, 예상된 출력이 렌더링되는지 확인하는 테스트를 추가해보세요:

```js
import Page from '../../app/page'
 
describe('<Page />', () => {
  it('예상된 콘텐츠가 렌더링되고 표시되어야 합니다', () => {
    // Home 페이지에 대한 React 컴포넌트를 마운트합니다
    cy.mount(<Page />)
 
    // 새로운 페이지에 "Home"이라는 h1 태그가 있어야 합니다
    cy.get('h1').contains('Home')
 
    // 예상된 URL을 가진 링크가 존재하는지 확인합니다
    // 링크를 따라가는 것은 E2E 테스트에 더 적합합니다
    cy.get('a[href="/about"]').should('be.visible')
  })
})
```

> 알아두면 좋은 사항:
Cypress는 현재 async Server Components에 대한 컴포넌트 테스트를 지원하지 않습니다. E2E 테스트를 권장합니다.
컴포넌트 테스트는 Next.js 서버가 필요하지 않기 때문에, 서버를 필요로 하는 'Image'와 같은 기능은 기본적으로 작동하지 않을 수 있습니다.

<div class="content-ad"></div>

### 컴포넌트 테스트 실행

터미널에서 npm run cypress:open을 실행하여 Cypress를 시작하고 컴포넌트 테스트 스위트를 실행하세요.

## 지속적인 통합 (CI)

대화식 테스트 외에도 CI 환경에 더 적합한 cypress run 명령을 사용하여 Cypress를 실행할 수 있습니다.

<div class="content-ad"></div>

```json
{
  "scripts": {
    //...
    "e2e": "start-server-and-test dev http://localhost:3000 \"cypress open --e2e\"",
    "e2e:headless": "start-server-and-test dev http://localhost:3000 \"cypress run --e2e\"",
    "component": "cypress open --component",
    "component:headless": "cypress run --component"
  }
}
```

이 리소스를 통해 Cypress 및 지속적 통합에 대해 더 알아볼 수 있어요:

- Cypress 예제와 함께 Next.js
- Cypress 지속적 통합 문서
- Cypress GitHub Actions 가이드
- 공식 Cypress GitHub 액션
- Cypress Discord