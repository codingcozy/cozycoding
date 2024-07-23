---
title: "Nextjs 14에서 TypeScript를 활용한 프로젝트 설정 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:44
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "TypeScript"
link: "https://nextjs.org/docs/app/building-your-application/configuring/typescript"
---


# TypeScript

Next.js는 React 애플리케이션을 만들기 위한 TypeScript 우선 개발 환경을 제공합니다.

필요한 패키지를 자동으로 설치하고 적절한 설정을 구성하는 내장 TypeScript 지원이 제공됩니다.

또한 편집기용 TypeScript 플러그인도 함께 제공됩니다.

<div class="content-ad"></div>

> 🎥 시청하세요: 내장된 TypeScript 플러그인에 대해 알아보세요 → YouTube (3분)

## 새 프로젝트

create-next-app은 이제 TypeScript를 기본으로 제공합니다.

```js
npx create-next-app@latest
```

<div class="content-ad"></div>

## 기존 프로젝트

프로젝트에 TypeScript를 추가하려면 파일 확장자를 .ts / .tsx로 변경하십시오. 그런 다음 next dev 및 next build를 실행하여 필요한 종속성을 자동으로 설치하고 권장 구성 옵션을 포함한 tsconfig.json 파일을 추가하십시오.

이미 jsconfig.json 파일이 있는 경우 기존 jsconfig.json에서 paths 컴파일러 옵션을 새로운 tsconfig.json 파일로 복사한 다음 기존 jsconfig.json 파일을 삭제하십시오.

## TypeScript 플러그인

<div class="content-ad"></div>

Next.js에는 커스텀 TypeScript 플러그인과 타입 체커가 포함되어 있습니다. 이를 통해 VSCode 및 다른 코드 편집기에서 고급 타입 체크와 자동 완성을 사용할 수 있습니다.

VS Code에서 플러그인을 활성화하려면 다음을 따르세요:

- 명령 팔레트를 엽니다 (Ctrl/⌘ + Shift + P)
- "TypeScript: Select TypeScript Version"을 검색합니다
- "Use Workspace Version"을 선택합니다

![이미지](/assets/img/2024-07-23-TypeScript_0.png)

<div class="content-ad"></div>

파일을 수정할 때마다 맞춤 플러그인이 활성화됩니다. 다음 빌드를 실행할 때는 맞춤 타입 체커가 사용됩니다.

### 플러그인 기능

TypeScript 플러그인은 다음과 같은 기능을 제공할 수 있습니다:

- 세그먼트 구성 옵션에 잘못된 값이 전달되었을 때 경고 표시
- 사용 가능한 옵션 및 문서 제시
- use client 지시문이 올바르게 사용되었는지 확인
- useState와 같은 클라이언트 후크가 Client 구성 요소에서만 사용되도록 보장

<div class="content-ad"></div>

> 좋은 정보: 앞으로 더 많은 기능이 추가될 예정입니다.

## 최소 TypeScript 버전

적어도 TypeScript v4.5.2 이상을 사용하는 것이 좋습니다. 이를 통해 import 이름에 대한 유형 수정자와 성능 향상과 같은 구문 기능을 얻을 수 있습니다.

## 정적으로 타입 지정된 링크

<div class="content-ad"></div>

Next.js는 next/link을 사용할 때 오타 및 다른 오류를 방지하기 위해 정적으로 링크의 유형을 지정할 수 있습니다. 이는 페이지 간 탐색 시 유형 안전성이 향상됩니다.

이 기능을 활성화하려면 experimental.typedRoutes를 활성화하고 프로젝트가 TypeScript를 사용하고 있어야 합니다.

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    typedRoutes: true,
  },
}
 
module.exports = nextConfig
```

Next.js는 .next/types에 모든 기존 경로에 대한 정보를 포함하는 링크 정의를 생성하며, TypeScript는 이를 사용하여 편집기에서 잘못된 링크에 대한 피드백을 제공할 수 있습니다.

<div class="content-ad"></div>

현재 실험적인 지원은 동적 세그먼트를 포함한 모든 문자열 리터럴을 포함합니다. 리터럴이 아닌 문자열의 경우 href를 Route로 수동으로 캐스팅해야 합니다:

```js
import type { Route } from 'next';
import Link from 'next/link'

// href가 유효한 라우트인 경우 TypeScript 오류가 발생하지 않음
<Link href="/about" />
<Link href="/blog/nextjs" />
<Link href={`/blog/${slug}`} />
<Link href={('/blog' + slug) as Route} />

// href가 유효한 라우트가 아닌 경우 TypeScript 오류 발생
<Link href="/aboot" />
```

next/link을 감싸는 사용자 지정 컴포넌트에서 href를 받으려면 제네릭을 사용하세요:

```js
import type { Route } from 'next'
import  Link from 'next/link'

function Card<T extends string>({ href }: { href: Route<T> | URL }) {
  return (
    <Link href={href}>
      <div>나의 카드</div>
    </Link>
  )
}
```

<div class="content-ad"></div>

> 어떻게 작동합니까?
'next dev' 또는 'next build'를 실행할 때, Next.js는 애플리케이션의 모든 기존 경로에 대한 정보를 포함하는 .next 폴더 안에 숨겨진 .d.ts 파일을 생성합니다 (링크의 href 유형으로 유효한 모든 경로). 이 .d.ts 파일은 tsconfig.json에 포함되며 TypeScript 컴파일러는 해당 .d.ts 파일을 확인하고 편집기에서 잘못된 링크에 대한 피드백을 제공합니다.

## 종단간 타입 안전성

Next.js App Router는 향상된 타입 안전성을 제공합니다. 이에는 다음 내용이 포함됩니다:

- 데이터 직렬화 없음: 서버에서 직접 컴포넌트, 레이아웃 및 페이지에서 데이터를 가져올 수 있습니다. 이 데이터는 React에서 사용하기 위해 (문자열로 변환되지 않은) 직렬화되어야 하는 필요가 없습니다. 대신, 앱은 기본적으로 Server Components를 사용하므로 추가적인 단계 없이 Date, Map, Set 등의 값들을 사용할 수 있습니다. 이전에는 Next.js 특정 유형으로 서버와 클라이언트 간 경계를 수동으로 지정해야 했습니다.
- 컴포넌트 간 데이터 흐름의 간소화: _app을 루트 레이아웃으로 대체함으로써, 이제는 컴포넌트와 페이지 간 데이터 흐름을 시각화하기가 더욱 쉬워졌습니다. 이전에는 개별 페이지와 _app 간에 데이터가 흐르는 것이 타입을 지정하는 데 어려울 수 있고 혼란스러운 버그를 유발할 수 있었습니다. App Router에서 공동 데이터 가져오기를 수행하기 때문에 이제 이 문제가 발생하지 않습니다.

<div class="content-ad"></div>

Next.js에서 데이터 가져오기는 데이터베이스나 콘텐츠 제공 업체 선택에 대해 구체적으로 되지 않으면서 최대한 엔드 투 엔드 유형 안전성을 제공합니다.

우리는 일반 TypeScript에서 예상할 수 있는 대로 응답 데이터의 유형을 지정할 수 있습니다. 예를 들어:

```js
async function getData() {
  const res = await fetch('https://api.example.com/...')
  // 반환값은 직렬화되지 않습니다
  // Date, Map, Set 등을 반환할 수 있습니다
  return res.json()
}
 
export default async function Page() {
  const name = await getData()
 
  return '...'
}
```

완전한 엔드 투 엔드 유형 안전성을 위해서는 데이터베이스나 콘텐츠 제공 업체가 TypeScript를 지원해야 합니다. 이는 ORM 또는 타입 안전한 쿼리 빌더를 사용하여 구현할 수 있습니다.

<div class="content-ad"></div>

## Async Server Component TypeScript 에러

TypeScript로 async Server Component를 사용하려면 TypeScript 5.1.3 이상 및 @types/react 18.2.8 이상을 사용해야 합니다.

이전 버전의 TypeScript를 사용하고 있는 경우 `Promise`Element`` is not a valid JSX element type 오류가 발생할 수 있습니다. 최신 버전의 TypeScript와 @types/react로 업데이트하면 이 문제가 해결될 것입니다.

## 서버 및 클라이언트 컴포넌트 간 데이터 전달

<div class="content-ad"></div>

서버와 클라이언트 컴포넌트 간에 데이터를 props를 통해 전달할 때, 브라우저에서 사용하기 위해 데이터는 여전히 직렬화(문자열로 변환)됩니다. 그러나 특별한 타입이 필요하지는 않습니다. 이는 다른 컴포넌트 간에 props를 전달하는 것과 동일하게 타입이 지정됩니다.

또한, 렌더링되지 않은 데이터는 서버와 클라이언트 간에 전달되지 않기 때문에 직렬화해야 하는 코드가 적어집니다. 이는 Server Components를 지원함으로써 현재만 가능합니다.

## 경로 별칭과 baseUrl

Next.js는 자동으로 tsconfig.json의 "paths"와 "baseUrl" 옵션을 지원합니다.

<div class="content-ad"></div>

모듈 경로 별칭 문서에서 이 기능에 대해 더 자세히 알아볼 수 있어요.

## next.config.js 타입 체크

next.config.js 파일은 Babel이나 TypeScript에 의해 구문 분석되지 않기 때문에 JavaScript 파일이어야 합니다. 하지만 JSDoc을 사용하여 IDE에서 일부 타입 체크를 추가할 수 있어요:

```js
// @ts-check

/**
 * @type {import('next').NextConfig}
 **/
const nextConfig = {
  /* 구성 옵션을 여기에 추가하세요 */
}

module.exports = nextConfig
```

<div class="content-ad"></div>

## 점진적 타입 확인

v10.2.1부터 Next.js에서는 tsconfig.json에서 활성화된 경우 증분 방식의 타입 확인을 지원합니다. 이를 통해 대형 애플리케이션에서의 타입 확인 속도를 높일 수 있습니다.

## TypeScript 오류 무시하기

Next.js는 프로젝트에 TypeScript 오류가 있을 때 프로덕션 빌드(next build)를 실패시킵니다.

<div class="content-ad"></div>

만약 Next.js가 애플리케이션이 에러를 가지고 있을 때에도 위험하지 않은 프로덕션 코드를 생성하도록 하는 것을 원하시면, 기본 타입 체크 단계를 비활성화할 수 있습니다.

비활성화할 경우, 빌드 또는 배포 과정 중 타입 체크를 실행하는지 확인해야 하며, 그렇지 않으면 매우 위험할 수 있습니다.

next.config.js를 열어서 typescript 구성에서 ignoreBuildErrors 옵션을 활성화하세요:

```js
module.exports = {
  typescript: {
    // !! 경고 !!
    // 프로젝트에 타입 에러가 있더라도 프로덕션 빌드가 성공적으로 완료되도록 하는 것에 주의하세요.
    // !! 경고 !!
    ignoreBuildErrors: true,
  },
}
```

<div class="content-ad"></div>

## 사용자 정의 유형 선언

사용자 정의 유형을 선언해야 할 때 next-env.d.ts 파일을 수정하고 싶을 수 있습니다. 그러나 이 파일은 자동으로 생성되므로 수행한 모든 변경 사항이 덮어씌워집니다. 대신, 새 파일을 생성하여 new-types.d.ts라고 하겠습니다. 그리고 이를 tsconfig.json 파일에서 참조해야 합니다:

```js
{
  "compilerOptions": {
    "skipLibCheck": true
    //...생략...
  },
  "include": [
    "new-types.d.ts",
    "next-env.d.ts",
    ".next/types/**/*.ts",
    "**/*.ts",
    "**/*.tsx"
  ],
  "exclude": ["node_modules"]
}
```

## 버전 변경

<div class="content-ad"></div>

<table class="w-full table-auto">
<thead>
<tr>
<th>버전</th>
<th>변경 내용</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>v13.2.0</code></td>
<td>정적 타입 링크가 베타로 제공됩니다.</td>
</tr>
<tr>
<td><code>v12.0.0</code></td>
<td><a href="/docs/architecture/nextjs-compiler">SWC</a>이(가) 더 빠른 빌드를 위해 TypeScript 및 TSX를 컴파일하는 데 기본적으로 사용됩니다.</td>
</tr>
<tr>
<td><code>v10.2.1</code></td>
<td><a href="https://www.typescriptlang.org/tsconfig#incremental" rel="noopener noreferrer nofollow" target="_blank">증분 형 검사<span class="inline-flex"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24" style="color:currentColor;width:14px;height:14px"><path d="M7 17L17 7"></path><path d="M7 7h10v10"></path></svg></span></a>를 <code>tsconfig.json</code>에서 활성화하면 추가 지원이 제공됩니다.</td>
</tr>
</tbody>
</table>