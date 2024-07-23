---
title: "Nextjs 14에서 Markdown과 MDX를 사용하여 동적 콘텐츠 관리하기"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:47
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Markdown and MDX"
link: "https://nextjs.org/docs/app/building-your-application/configuring/mdx"
---


# 마크다운과 MDX

마크다운은 텍스트를 서식 지정하는 데 사용되는 가벼운 마크업 언어입니다. 일반 텍스트 구문을 사용하여 작성하고 이를 구조적으로 유효한 HTML로 변환할 수 있습니다. 보통 웹사이트나 블로그 콘텐츠를 작성하는 데 사용됩니다.

당신이 작성한 내용...

```js
I **love** using [Next.js](https://nextjs.org/)
```

<div class="content-ad"></div>

```js
<p>I <strong>love</strong> using <a href="https://nextjs.org/">Next.js</a></p>
```

MDX는 JSX를 마크다운 파일에 직접 작성할 수 있는 마크다운의 상위 집합입니다. 이는 동적 상호 작용을 추가하고 콘텐츠 내에서 리액트 컴포넌트를 포함시키는 강력한 방법입니다.

Next.js는 로컬 MDX 콘텐츠와 서버에서 동적으로 가져온 원격 MDX 파일을 모두 지원할 수 있습니다. Next.js 플러그인은 마크다운과 리액트 컴포넌트를 HTML로 변환하는 처리를 담당하며, 앱 라우터의 기본인 서버 컴포넌트에서도 사용할 수 있습니다.

<div class="content-ad"></div>

## @next/mdx

@next/mdx 패키지는 Next.js를 설정하여 markdown 및 MDX를 처리할 수 있도록합니다. 이는 로컬 파일에서 데이터를 가져와 .mdx 확장자가있는 페이지를 만들 수 있게 해줍니다. 이를 /pages 또는 /app 디렉토리에서 직접 사용할 수 있습니다.

우리 함께 Next.js에서 MDX를 구성하고 사용하는 방법을 살펴보겠습니다.

## 시작하기

<div class="content-ad"></div>

MDX를 렌더링하는 데 필요한 패키지를 설치해주세요:

```js
npm install @next/mdx @mdx-js/loader @mdx-js/react @types/mdx
```

애플리케이션 루트(src/ 또는 app/의 상위 폴더)에 mdx-components.tsx 파일을 만들어주세요:

> 참고: App Router를 사용하려면 mdx-components.tsx 파일이 필요하며, 이 파일이 없으면 MDX가 작동하지 않습니다.

<div class="content-ad"></div>

```js
import type { MDXComponents } from 'mdx/types'

export function useMDXComponents(components: MDXComponents): MDXComponents {
  return {
    ...components,
  }
}
```

프로젝트 루트의 next.config.js 파일을 업데이트하여 MDX를 사용하도록 구성하십시오:

```js
const withMDX = require('@next/mdx')()

/** @type {import('next').NextConfig} */
const nextConfig = {
  // `pageExtensions`를 구성하여 MDX 파일을 포함하도록 설정합니다
  pageExtensions: ['js', 'jsx', 'mdx', 'ts', 'tsx'],
  // 필요에 따라 다른 Next.js 구성을 추가할 수 있습니다
}

module.exports = withMDX(nextConfig)
```

그런 다음, /app 디렉토리 내에 새로운 MDX 페이지를 생성하십시오:

<div class="content-ad"></div>

```js
  your-project
  ├── app
  │   └── my-mdx-page
  │       └── page.mdx
  └── package.json
```

이제 마크다운을 사용하여 MDX 페이지 내에서 직접 React 컴포넌트를 가져올 수 있습니다:

```js
import { MyComponent } from 'my-components'

# 내 MDX 페이지에 오신 것을 환영합니다!

이것은 **굵은** 및 _이탤릭체_ 텍스트입니다.

이것은 마크다운 목록입니다:

- 하나
- 둘
- 셋

React 컴포넌트를 확인해보세요:

<MyComponent />
```

/my-mdx-page 경로로 이동하면 렌더링된 MDX가 표시됩니다.

<div class="content-ad"></div>

## 원격 MDX

만약 마크다운 또는 MDX 파일 또는 컨텐츠가 다른 곳에 저장되어 있다면, 서버에서 동적으로 가져올 수 있습니다. 이는 별도의 로컬 폴더, CMS, 데이터베이스 또는 다른 곳에 저장된 컨텐츠에 유용합니다. 이용에 많은 커뮤니티 패키지는 next-mdx-remote가 있습니다.

> 알아두면 좋은 점: 조심히 진행해주세요. MDX는 JavaScript로 컴파일되고 서버에서 실행됩니다. 신뢰할 수 있는 소스에서만 MDX 컨텐츠를 가져와야 합니다. 그렇지 않으면 원격 코드 실행(RCE)로 이어질 수 있습니다.

다음 예제에서는 next-mdx-remote를 사용합니다:

<div class="content-ad"></div>

```javascript
import { MDXRemote } from 'next-mdx-remote/rsc'
 
export default async function RemoteMdxPage() {
  // MDX 텍스트 - 로컬 파일, 데이터베이스, CMS, fetch 등에서 가져올 수 있습니다...
  const res = await fetch('https://...')
  const markdown = await res.text()
  return <MDXRemote source={markdown} />
}
```

/my-mdx-page-remote 경로로 이동하면 렌더링된 MDX가 표시됩니다.

## 레이아웃

MDX 페이지 전체에 대한 레이아웃을 공유하려면 App Router의 내장 레이아웃 지원을 사용할 수 있습니다.


<div class="content-ad"></div>

```js
export default function MdxLayout({ children }: { children: React.ReactNode }) {
  // 여기서 공유 레이아웃이나 스타일을 작성할 수 있어요
  return <div style={ color: 'blue' }>{children}</div>
}
```

## Remark과 Rehype 플러그인

MDX 콘텐츠를 변환하기 위해 remark와 rehype 플러그인을 선택적으로 제공할 수 있어요.

예를 들어, remark-gfm을 사용하여 GitHub Flavored Markdown을 지원할 수 있어요.

<div class="content-ad"></div>

`table` 태그를 Markdown 형식으로 변경하세요.


Since the remark and rehype ecosystem is ESM only, you`ll need to use next.config.mjs as the configuration file.

```js
import remarkGfm from 'remark-gfm'
import createMDX from '@next/mdx'

/** @type {import('next').NextConfig} */
const nextConfig = {
  // Configure `pageExtensions`` to include MDX files
  pageExtensions: ['js', 'jsx', 'mdx', 'ts', 'tsx'],
  // Optionally, add any other Next.js config below
}

const withMDX = createMDX({
  // Add markdown plugins here, as desired
  options: {
    remarkPlugins: [remarkGfm],
    rehypePlugins: [],
  },
})

// Wrap MDX and Next.js config with each other
export default withMDX(nextConfig)
```

## Frontmatter

Frontmatter is a YAML like key/value pairing that can be used to store data about a page. @next/mdx does not support frontmatter by default, though there are many solutions for adding frontmatter to your MDX content, such as:


<div class="content-ad"></div>

- remark-frontmatter
- remark-mdx-frontmatter
- gray-matter
.

@next/mdx를 사용하여 페이지 메타데이터에 액세스하려면 .mdx 파일 내에서 metadata 객체를 내보낼 수 있습니다:

```js
export const metadata = {
  author: 'John Doe',
}

# 내 MDX 페이지
```

## 사용자 정의 요소

<div class="content-ad"></div>

마크다운을 사용하는 기쁜 점 중 하나는 기본 HTML 요소로 매핑되어 빠르고 직관적으로 쓸 수 있다는 것입니다:

```js
이것은 마크다운에서의 리스트입니다:
 
- 하나
- 둘
- 셋
```

위의 마크다운은 다음과 같은 HTML을 생성합니다:

```js
<p>이것은 마크다운에서의 리스트입니다:</p>
 
<ul>
  <li>하나</li>
  <li>둘</li>
  <li>셋</li>
</ul>
```

<div class="content-ad"></div>

무슨 종류의 웹사이트나 애플리케이션을 만들 때 자체 요소에 스타일을 적용하고 싶을 때, 단축 코드를 전달할 수 있어요. 이것들은 HTML 요소에 매핑되는 자체적인 사용자 정의 컴포넌트입니다.

이를 위해, 애플리케이션 루트에 있는 mdx-components.tsx 파일을 열고 사용자 정의 요소를 추가하세요:

```js
import type { MDXComponents } from 'mdx/types'
import Image, { ImageProps } from 'next/image'

// 이 파일을 사용하면 MDX 파일에서 사용할 사용자 정의 React 컴포넌트를 제공할 수 있어요.
// 필요한 경우 다른 라이브러리의 컴포넌트를 가져와 사용할 수 있고,
// 인라인 스타일이나 기타 것들을 포함해서 필요한 React 컴포넌트를 가져와 사용 가능합니다.

export function useMDXComponents(components: MDXComponents): MDXComponents {
  return {
    // 내장된 컴포넌트를 사용자 정의하여 스타일을 추가하는 등의 작업이 가능합니다.
    h1: ({ children }) => <h1 style={{ fontSize: '100px' }}>{children}</h1>,
    img: (props) => (
      <Image
        sizes="100vw"
        style={{ width: '100%', height: 'auto' }}
        {...(props as ImageProps)}
      />),
    ...components,
  }
}
```

## 심층분석: 마크다운을 HTML로 어떻게 변환하나요?

<div class="content-ad"></div>

리액트는 마크다운을 네이티브로 이해하지 못합니다. 먼저 마크다운 플레인 텍스트를 HTML로 변환해야 합니다. 이 작업은 remark와 rehype를 사용하여 수행할 수 있습니다.

remark는 마크다운 주변의 도구들의 생태계입니다. rehype는 HTML에서 동일한 일을 합니다. 예를 들어, 다음 코드 스니펫은 마크다운을 HTML로 변환합니다:

```js
import { unified } from 'unified'
import remarkParse from 'remark-parse'
import remarkRehype from 'remark-rehype'
import rehypeSanitize from 'rehype-sanitize'
import rehypeStringify from 'rehype-stringify'

main()

async function main() {
  const file = await unified()
    .use(remarkParse) // 마크다운 AST로 변환
    .use(remarkRehype) // HTML AST로 변환
    .use(rehypeSanitize) // HTML 입력을 정리
    .use(rehypeStringify) // AST를 직렬화된 HTML로 변환
    .process('Hello, Next.js!')

  console.log(String(file)) // <p>Hello, Next.js!</p>
}
```

remark와 rehype 에코시스템에는 문법 강조 표시를 위한 플러그인부터 제목 링크, 목차 생성 등 다양한 플러그인이 포함되어 있습니다.

<div class="content-ad"></div>

위에서 보여준대로 @next/mdx를 사용할 때는 직접적으로 remark나 rehype을 사용할 필요가 없습니다. 이것은 대신 여러분을 위해 처리됩니다. @next/mdx 패키지가 무엇을 하는지 더 깊이 이해하기 위해 여기에 설명하고 있습니다.

## 러스트 기반 MDX 컴파일러 사용 (실험적)

Next.js는 러스트로 작성된 새로운 MDX 컴파일러를 지원합니다. 이 컴파일러는 아직 실험 단계이며 프로덕션 환경에서 권장되지 않습니다. 새로운 컴파일러를 사용하려면 next.config.js를 구성해야 합니다. 이를 withMDX에 전달할 때 다음과 같이 설정하세요:

```js
module.exports = withMDX({
  experimental: {
    mdxRs: true,
  },
})
```

<div class="content-ad"></div>

## 도움이 되는 링크

- MDX
- @next/mdx
- remark
- rehype
- Markdoc