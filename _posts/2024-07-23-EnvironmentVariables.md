---
title: "환경 변수 쉽게 관리하기 Nextjs 14 App Router 가이드"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:46
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Environment Variables"
link: "https://nextjs.org/docs/app/building-your-application/configuring/environment-variables"
isUpdated: true
---




# 환경 변수

Next.js는 내장된 환경 변수 지원으로 다음을 수행할 수 있게 해줍니다:

- .env.local을 사용하여 환경 변수를 로드합니다.
- NEXT_PUBLIC_ 접두사를 사용하여 브라우저에 환경 변수를 번들합니다.

## 환경 변수 로드하기

<div class="content-ad"></div>

Next.js는 .env.local에서 환경 변수를 process.env로 로드할 수 있도록 내장 지원이 있습니다.

```js
DB_HOST=localhost
DB_USER=myuser
DB_PASS=mypassword
```

> 참고: Next.js는 .env* 파일에서 여러 줄 변수를 지원합니다.
# .env.local

# 줄 바꿈으로 작성할 수 있습니다.
PRIVATE_KEY="-----BEGIN RSA PRIVATE KEY-----
...
Kh9NV...
...
-----END DSA PRIVATE KEY-----"
 
# 혹은 이스케이프 문자 `\n`을 사용하여 작성할 수 있습니다.
PRIVATE_KEY="-----BEGIN RSA PRIVATE KEY-----\nKh9NV...\n-----END DSA PRIVATE KEY-----\n"

> 참고: /src 폴더를 사용하는 경우, Next.js는 .env 파일을 부모 폴더에서만 로드하며 /src 폴더에서는 로드하지 않습니다. 이는 process.env.DB_HOST, process.env.DB_USER, process.env.DB_PASS를 Node.js 환경으로 자동으로 로드하여 Route Handlers에서 사용할 수 있도록 합니다.

<div class="content-ad"></div>

예를 들어:

```js
export async function GET() {
  const db = await myDB.connect({
    host: process.env.DB_HOST,
    username: process.env.DB_USER,
    password: process.env.DB_PASS,
  })
  // ...
}
```

### 다른 변수 참조

Next.js는 .env* 파일 내부에서 $를 사용하여 다른 변수를 참조하는 변수를 자동으로 확장합니다. 이를 통해 다른 시크릿을 참조할 수 있습니다. 예를 들어:

<div class="content-ad"></div>

```js
TWITTER_USER=nextjs
TWITTER_URL=https://twitter.com/$TWITTER_USER
```

위 예시에서, process.env.TWITTER_URL은 https://twitter.com/nextjs로 설정됩니다.

> 참고: 만약 변수를 값으로 사용할 때 $가 포함된 경우, \$로 escape 처리해야 합니다.

## 브라우저용 환경 변수 번들링

<div class="content-ad"></div>

NEXT_PUBLIC_ 환경 변수는 Node.js 환경에서만 사용할 수 있습니다. 즉, 브라우저(클라이언트)에서는 사용할 수 없습니다. 

브라우저에서 환경 변수의 값을 사용하려면, Next.js가 값이 인라인 처리되어 클라이언트에 전달되는 js 번들에 삽입되도록 지정해야 합니다. 이렇게 하려면 변수 이름 앞에 NEXT_PUBLIC_를 붙이면 됩니다. 예를 들어:

```js
NEXT_PUBLIC_ANALYTICS_ID=abcdefghijk
```

위와 같이 하면 Next.js가 Node.js 환경에서 process.env.NEXT_PUBLIC_ANALYTICS_ID를 해당 값을 사용하여 대체하도록 지시하여 코드 어디에서나 사용할 수 있게 됩니다. 이렇게 하면 브라우저로 전송되는 JavaScript에 삽입됩니다.

<div class="content-ad"></div>

> 참고: 앱을 빌드한 후에는 이 환경 변수에 대한 변경에 응답하지 않습니다. 예를 들어, Heroku 파이프라인을 사용하여 한 환경에서 빌드된 슬러그를 다른 환경으로 확산하거나 도커 이미지를 여러 환경에 배포하는 경우 모든 NEXT_PUBLIC_ 변수는 빌드 시 평가된 값으로 고정됩니다. 이 값들은 프로젝트를 빌드할 때 적절히 설정해야 합니다. 런타임 환경 값에 액세스해야 하는 경우에는, 클라이언트에게 이 값을 제공하기 위해 자체 API를 설정해야 합니다(요청 시 또는 초기화 중에).

```js
import setupAnalyticsService from '../lib/my-analytics-service'

// 'NEXT_PUBLIC_ANALYTICS_ID'를 여기서 사용할 수 있습니다. 'NEXT_PUBLIC_'로 접두사가 붙어 있기 때문입니다.
// 빌드할 때 `setupAnalyticsService('abcdefghijk')`로 변환됩니다.
setupAnalyticsService(process.env.NEXT_PUBLIC_ANALYTICS_ID)

function HomePage() {
  return <h1>Hello World</h1>
}

export default HomePage
```

동적 조회는 인라인화되지 않습니다. 다음과 같이:

```js
// 변수를 사용하므로 이 부분은 인라인화되지 않습니다
const varName = 'NEXT_PUBLIC_ANALYTICS_ID'
setupAnalyticsService(process.env[varName])

// 변수를 사용하므로 이 부분은 인라인화되지 않습니다
const env = process.env
setupAnalyticsService(env.NEXT_PUBLIC_ANALYTICS_ID)
```

<div class="content-ad"></div>

### 런타임 환경 변수

Next.js는 빌드 시간 및 런타임 환경 변수를 모두 지원할 수 있습니다.

기본적으로 환경 변수는 서버에서만 사용 가능합니다. 브라우저에 환경 변수를 노출시키려면 NEXT_PUBLIC_로 접두사를 붙여야 합니다. 그러나 이러한 공개 환경 변수는 다음 빌드 중에 JavaScript 번들에 인라인될 것입니다.

런타임 환경 변수를 읽기 위해서는 getServerSideProps를 사용하거나 App Router를 증분적으로 채택하는 것이 좋습니다. App Router를 사용하면 동적 렌더링 중 서버에서 환경 변수를 안전하게 읽을 수 있습니다. 이를 통해 서로 다른 값이 있는 여러 환경을 거쳐 홍보할 수 있는 단일 Docker 이미지를 사용할 수 있습니다.

<div class="content-ad"></div>

```js
import { unstable_noStore as noStore } from 'next/cache'
 
export default function Component() {
  noStore()
  // cookies(), headers(), and other dynamic functions
  // will also opt into dynamic rendering, meaning
  // this env variable is evaluated at runtime
  const value = process.env.MY_VALUE
  // ...
}
```

알아두면 좋은 사실:

- register 함수를 사용하여 서버 시작 시 코드를 실행할 수 있습니다.
- 단독 출력 모드와 호환되지 않는 runtimeConfig 옵션의 사용을 권장하지 않습니다. 대신 App Router를 점진적으로 도입하는 것이 좋습니다.

## 기본 환경 변수

<div class="content-ad"></div>

보통 .env.local 파일 하나만 필요합니다. 그러나 때로는 개발 (next dev) 또는 프로덕션 (next start) 환경에 몇 가지 기본값을 추가하고 싶을 수도 있습니다.

Next.js에서는 .env (모든 환경), .env.development (개발 환경) 및 .env.production (프로덕션 환경) 등에서 기본값을 설정할 수 있습니다.

.env.local은 항상 설정된 기본값을 덮어씁니다.

> 알아두면 좋은 점: .env, .env.development 및 .env.production 파일은 기본값을 정의하므로 저장소에 포함되어야 합니다. .env*.local 파일은 무시되어야 하는 파일이므로 .gitignore에 추가해야 합니다. .env.local은 비밀 정보를 저장할 수 있는 곳입니다.

<div class="content-ad"></div>

## Vercel에서의 환경 변수

Next.js 애플리케이션을 Vercel에 배포할 때, 프로젝트 설정에서 환경 변수를 구성할 수 있습니다.

개발 및 배포에 사용되는 모든 환경 변수는 해당 위치에서 구성해야 합니다. 개발에 사용되는 환경 변수도 구성하실 수 있습니다. 이후 로컬 기기로 내려받을 수 있습니다.

개발 환경 변수를 구성했다면, 아래 명령어를 사용하여 로컬 기기에서 사용할 `.env.local`에 가져올 수 있습니다.

<div class="content-ad"></div>

```js
vercel env pull .env.local
```

> 좋은 정보: Next.js 애플리케이션을 Vercel에 배포할 때 .env* 파일에 있는 환경 변수는 Edge Runtime에서 사용할 수 없습니다. 환경 변수의 이름이 NEXT_PUBLIC_로 시작하지 않으면 안 됩니다. 모든 환경 변수를 사용할 수 있도록 하는 편이 좋습니다. Project Settings 에서 환경 변수를 관리하는 것이 좋습니다.

## 환경 변수 테스트

개발 및 프로덕션 환경 외에도 3번째 옵션이 있습니다. test. 개발 또는 프로덕션 환경에 기본값을 설정할 수 있는 것과 같은 방식으로 테스트 환경용 .env.test 파일에 동일한 작업을 수행할 수 있습니다 (이 경우 이전 두 가지보다는 덜 흔합니다). Next.js는 테스트 환경에서 .env.development 또는 .env.production으로부터 환경 변수를로드하지 않습니다.

<div class="content-ad"></div>

이 내용은 jest나 cypress와 같은 도구를 사용하여 특정 환경 변수를 테스트 목적으로 설정해야 할 때 유용합니다. NODE_ENV가 test로 설정된 경우에는 테스트 기본값이 로드됩니다. 그러나 테스팅 도구가 이를 대신 다루어 주기 때문에 일반적으로 직접 설정할 필요는 없습니다.

테스트 환경과 개발 및 프로덕션 사이에 주의할 점이 있습니다: .env.local은 로드되지 않습니다. 모든 사용자에게 동일한 결과를 제공하는 것이 테스트 목적이기 때문에 .env.local을 무시하고 동일한 환경 기본값을 사용하도록 설정되어 있습니다 (기본 설정을 재정의하려는 것은 아니기 때문).

> 참고: 기본 환경 변수와 마찬가지로 .env.test 파일은 저장소에 포함되어야 하지만 .env.test.local은 포함되지 않아야 합니다. 왜냐하면 .env*.local 파일은 .gitignore를 통해 무시되도록 설정되어 있기 때문입니다.

유닛 테스트를 실행하는 동안 @next/env 패키지의 loadEnvConfig 함수를 활용하여 Next.js가 환경 변수를 로드하는 방법과 동일하게 환경 변수를 로드할 수 있습니다.

<div class="content-ad"></div>

```js
// 아래 내용은 Jest 전역 설정 파일이나 테스트 설정 파일과 같이 사용할 수 있습니다.
import { loadEnvConfig } from '@next/env'

export default async () => {
  const projectDir = process.cwd()
  loadEnvConfig(projectDir)
}
```

## 환경 변수 로드 순서

환경 변수는 다음 위치에서 찾아집니다. 변수를 찾으면 중지됩니다.

- process.env
- .env.$(NODE_ENV).local
- .env.local (NODE_ENV가 test일 때는 확인되지 않음.)
- .env.$(NODE_ENV)
- .env


<div class="content-ad"></div>

예를 들어, NODE_ENV가 development로 설정되어 있고 .env.development.local과 .env 두 파일에 변수를 정의했다면, .env.development.local 파일의 값이 사용됩니다.

> 알아두면 좋은 사실: NODE_ENV에 허용된 값은 production, development 및 test입니다.

## 알아두면 좋은 사실

- 만약 /src 디렉토리를 사용 중이라면, .env.* 파일들은 프로젝트의 루트에 있어야 합니다.
- 환경 변수 NODE_ENV가 할당되지 않았을 경우, Next.js는 next dev 명령을 실행할 때 자동으로 development를 할당하거나 다른 모든 명령에 대해서는 production을 할당합니다.

<div class="content-ad"></div>

## 버전 기록

| 버전   | 변경 내용                                |
|--------|-----------------------------------------|
| `v9.4.0` | `.env` 및 `NEXT_PUBLIC_` 지원이 도입되었습니다. |