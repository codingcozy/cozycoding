---
title: "Nextjs 14로 Playwright 설정하는 방법 쉬운 가이드"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:50
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Setting up Playwright with Next.js"
link: "https://nextjs.org/docs/app/building-your-application/testing/playwright"
isUpdated: true
---




# Next.js와 함께 Playwright 설정하기

Playwright는 Chromium, Firefox 및 WebKit을 단일 API로 자동화할 수 있는 테스트 프레임워크입니다. 이를 사용하여 End-to-End (E2E) 테스트를 작성할 수 있습니다. 이 안내서에서는 Next.js와 Playwright를 설정하고 첫 번째 테스트를 작성하는 방법을 안내해드립니다.

## 빠른 시작

가장 빠른 시작 방법은 create-next-app에서 with-playwright 예제를 사용하는 것입니다. 이렇게 하면 Playwright가 구성된 Next.js 프로젝트가 생성됩니다.

<div class="content-ad"></div>


```js
npx create-next-app@latest --example with-playwright with-playwright-app
```

## 수동 설정

Playwright를 설치하려면 다음 명령을 실행하세요:

```js
npm init playwright
# 또는
yarn create playwright
# 또는
pnpm create playwright
```

<div class="content-ad"></div>

이 프로젝트에 Playwright를 설정하고 구성하기 위한 일련의 프롬프트를 통해 안내 받게 될 거에요. 이 과정에는 playwright.config.ts 파일 추가도 포함돼요. 자세한 단계별 안내는 Playwright 설치 가이드를 참조해주세요.

## 첫 번째 Playwright E2E 테스트 생성하기

두 개의 새로운 Next.js 페이지를 생성해보세요:

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

이제 네비게이션이 올바르게 작동하는지 확인하는 테스트를 추가해보세요:

```js
import { test, expect } from '@playwright/test'
 
test('어바웃 페이지로 이동해야 합니다', async ({ page }) => {
  // 인덱스 페이지에서 시작 (baseUrl은 playwright.config.ts에 webServer를 통해 설정됨)
  await page.goto('http://localhost:3000/')
  // '어바웃'이라는 텍스트를 가진 요소를 찾아 클릭합니다
  await page.click('text=About')
  // 새 URL은 "/about"이어야 합니다 (baseUrl이 사용됨)
  await expect(page).toHaveURL('http://localhost:3000/about')
  // 새 페이지에 "About"이라는 제목이 포함되어 있어야 합니다
  await expect(page.locator('h1')).toContainText('About')
}
```

> 알아두면 좋은 팁:
playwright.config.ts 구성 파일에 "baseURL": "http://localhost:3000"을 추가하면 page.goto("/") 대신 page.goto("http://localhost:3000/")를 사용할 수 있습니다.


<div class="content-ad"></div>

### Playwright 테스트 실행하기

Playwright는 Chromium, Firefox 및 Webkit 세 가지 브라우저를 사용하여 사용자가 응용 프로그램을 탐색하는 것을 시뮬레이션합니다. 이를 위해서 Next.js 서버가 실행 중이어야 합니다. 응용 프로그램이 실제로 동작하는 방식과 더 유사하도록 테스트를 실행하기 위해 제작 코드에 대한 테스트를 권장합니다.

먼저 npm run build 및 npm run start를 실행한 다음, 다른 터미널 창에서 npx playwright test를 실행하여 Playwright 테스트를 수행하세요.

> 참고: 다른 방법으로 Playwright가 개발 서버를 시작하고 완전히 사용 가능할 때까지 대기하게 하는 webServer 기능을 사용할 수도 있습니다.

<div class="content-ad"></div>

### 지속적 통합(CI)에서 Playwright 사용하기

Playwright는 기본적으로 헤드리스 모드에서 테스트를 실행합니다. 모든 Playwright 종속성을 설치하려면 npx playwright install-deps를 실행하세요.

다음 자료에서 Playwright 및 지속적 통합에 대해 더 알아볼 수 있습니다:

- Next.js와 Playwright 예제
- 지속적 통합 제공업체에서의 Playwright 사용
- Playwright Discord