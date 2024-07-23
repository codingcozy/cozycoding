---
title: "Nextjs 14 Bundle Analyzer로 최적화하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:39
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Bundle Analyzer"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/bundle-analyzer"
---


# 번들 분석기

`@next/bundle-analyzer`은 Next.js용 플러그인으로, JavaScript 모듈의 크기를 관리하는 데 도움을 줍니다. 각 모듈과 그 의존성의 크기에 대한 시각적 보고서를 생성합니다. 이 정보를 사용하여 큰 의존성을 제거하거나 코드를 분할하거나 필요할 때만 일부 부분을로드하여 클라이언트로 전달되는 데이터 양을 줄일 수 있습니다.

## 설치

다음 명령을 실행하여 플러그인을 설치합니다:

<div class="content-ad"></div>

```js
npm i @next/bundle-analyzer
# 또는
yarn add @next/bundle-analyzer
# 또는
pnpm add @next/bundle-analyzer
```

그런 다음, bundle analyzer의 설정을 당신의 next.config.js에 추가하세요.

```js
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
})
 
/** @type {import('next').NextConfig} */
const nextConfig = {}
 
module.exports = withBundleAnalyzer(nextConfig)
```

## 번들 분석하기

<div class="content-ad"></div>

다음 명령어를 실행하여 번들을 분석하세요:

```js
ANALYZE=true npm run build
# 또는
ANALYZE=true yarn build
# 또는
ANALYZE=true pnpm build
```

보고서는 브라우저에서 세 개의 새 탭이 열리며 확인할 수 있습니다. 개발 중이거나 사이트를 배포하기 전에 정기적으로 이 작업을 수행하여 큰 번들을 더 빨리 식별하고 애플리케이션을 효율적으로 설계하는 데 도움이 될 수 있습니다.