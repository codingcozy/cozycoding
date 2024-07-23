---
title: "Nextjs 14에서 Vitest 설정하는 방법 처음부터 차근차근 배우기"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:49
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Setting up Vitest with Next.js"
link: "https://nextjs.org/docs/app/building-your-application/testing/vitest"
---


# Next.js에서 Vitest 설정하기

Vite와 React Testing Library는 종종 함께 사용되는데요. 유닛 테스트를 위해 Vitest를 Next.js와 함께 설정하는 방법과 첫 번째 테스트를 작성하는 방법을 안내해 드릴 거에요.

> 좋아요: 비동기 서버 컴포넌트가 React 생태계에 새롭게 추가되었기 때문에 Vitest는 현재 해당 컴포넌트를 지원하지 않습니다. 동기적인 서버 및 클라이언트 컴포넌트에 대한 단위 테스트는 여전히 수행할 수 있지만, 비동기 컴포넌트에 대해서는 E2E 테스트를 추천합니다.

## 빠른 시작하기

<div class="content-ad"></div>

빠르게 시작할 수 있는 Next.js with-vitest 예제를 사용하여 create-next-app을 실행할 수 있습니다:

```js
npx create-next-app@latest --example with-vitest with-vitest-app
```

## 수동 설정

Vitest를 수동으로 설정하려면 vitest와 다음 패키지들을 dev dependencies로 설치하세요:

<div class="content-ad"></div>

```json
npm install -D vitest @vitejs/plugin-react jsdom @testing-library/react
# 또는
yarn add -D vitest @vitejs/plugin-react jsdom @testing-library/react
# 또는
pnpm install -D vitest @vitejs/plugin-react jsdom @testing-library/react
# 또는
bun add -D vitest @vitejs/plugin-react jsdom @testing-library/react
```

프로젝트의 루트에 vitest.config.ts|js 파일을 만들고 다음 옵션을 추가해주세요:

```js
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
  },
})
```

Vitest 구성에 대한 자세한 정보는 Vitest 구성 문서를 참조해주세요.

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경하세요.

<div class="content-ad"></div>

모든 것이 제대로 작동하는지 확인하기 위해 `Page /` 컴포넌트가 제목을 성공적으로 렌더링하는지 확인하는 테스트를 만들어보세요:

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

```js
import { expect, test } from 'vitest'
import { render, screen } from '@testing-library/react'
import Page from '../app/page'

test('Page', () => {
  render(<Page />)
  expect(screen.getByRole('heading', { level: 1, name: 'Home' })).toBeDefined()
})
```

> 참고: 위의 예시는 일반적인 __tests__ 규칙을 사용했지만, 테스트 파일은 앱 라우터 내에도 함께 배치될 수 있습니다.

<div class="content-ad"></div>

## 테스트 실행하기

다음 명령어를 실행하여 테스트를 수행하세요:

```js
npm run test
# 또는
yarn test
# 또는
pnpm test
# 또는
bun test
```

## 추가 자료

<div class="content-ad"></div>

아래 자료들이 도움이 될 수 있어요:

- Next.js with Vitest 예제
- Vitest 문서
- React Testing Library 문서