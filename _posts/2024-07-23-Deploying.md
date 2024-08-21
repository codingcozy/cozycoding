---
title: "Nextjs 14 배포하는 방법 완벽 가이드"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:53
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Deploying"
link: "https://nextjs.org/docs/app/building-your-application/deploying"
isUpdated: true
---




# 배포하기

축하합니다, 이제 프로덕션 환경으로 출시할 시간이에요.

Vercel을 사용하여 관리되는 Next.js를 배포하거나, Node.js 서버에 직접 호스팅하거나, Docker 이미지나 정적 HTML 파일에 배포할 수도 있어요. next start를 사용하여 배포할 때는 모든 Next.js 기능이 지원됩니다.

## 프로덕션 빌드

<div class="content-ad"></div>

다음 빌드를 실행하면 애플리케이션의 최적화된 버전이 생성됩니다. HTML, CSS 및 JavaScript 파일이 페이지를 기반으로 생성됩니다. JavaScript는 컴파일되며 브라우저 번들은 최적화되어 모든 최신 브라우저를 지원하는 데 도움이 되는 Next.js 컴파일러를 사용합니다.

Next.js는 관리 및 자체 호스팅 Next.js에서 사용하는 표준 배포 출력을 생성합니다. 이를 통해 배포 방법에 관계없이 모든 기능이 지원되도록 보장합니다. 다음 주요 버전에서 이 출력을 빌드 출력 API 사양으로 변환할 것입니다.

## Vercel을 사용한 Next.js 관리형

Vercel은 Next.js의 창조자이자 유지보수자로서 Next.js 애플리케이션을 위한 관리되는 인프라 및 개발자 환경 플랫폼을 제공합니다.

<div class="content-ad"></div>

Vercel을 배포하는 것은 설정 없이 가능하며 전 세계적으로 확장 가능성, 가용성 및 성능을 향상시키는 추가 기능을 제공합니다. 그러나 자체 호스팅할 때도 모든 Next.js 기능을 지원합니다.

Vercel에서 Next.js에 대해 더 알아보기
또는 무료로 템플릿을 배포하여
시도해보세요.

## 자체 호스트

Next.js를 자체 호스팅하는 방법은 세 가지가 있습니다:

<div class="content-ad"></div>

- Node.js 서버
- Docker 컨테이너
- 정적 익스포트

### Node.js 서버

Next.js는 Node.js를 지원하는 호스팅 프로바이더에 배포할 수 있습니다. package.json에 "build" 및 "start" 스크립트가 있는지 확인해주세요:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  }
}
```

<div class="content-ad"></div>

그런 다음 애플리케이션을 빌드하려면 npm run build를 실행하세요. 마지막으로, Node.js 서버를 시작하려면 npm run start를 실행하세요. 이 서버는 모든 Next.js 기능을 지원합니다.

### 도커 이미지

Next.js를 지원하는 모든 호스팅 제공업체에 배포할 수 있습니다. 쿠버네티스와 같은 컨테이너 오케스트레이터에 배포하거나 클라우드 제공업체의 컨테이너 내에서 실행할 때 이 접근 방식을 사용할 수 있습니다.

- 내 컴퓨터에 Docker를 설치하세요
- 저희 예제(또는 멀티 환경 예제)를 복제하세요
- 컨테이너를 빌드하세요: docker build -t nextjs-docker .
- 컨테이너를 실행하세요: docker run -p 3000:3000 nextjs-docker

<div class="content-ad"></div>

다음.js를 통해 도커를 지원하면 모든 다음.js 기능을 사용할 수 있습니다.

### 정적 HTML 내보내기

다음.js를 통해 정적 사이트 또는 Single-Page Application (SPA)으로 시작하고 나중에 선택적으로 서버를 필요로하는 기능을 사용하도록 업그레이드할 수 있습니다.

다음.js가 이 정적 내보내기를 지원하기 때문에 HTML/CSS/JS 정적 자산을 제공할 수 있는 모든 웹 서버에 배포하고 호스팅할 수 있습니다. 이러한 도구에는 AWS S3, Nginx, 또는 Apache가 포함됩니다.

<div class="content-ad"></div>

정적 익스포트로 실행하면 서버가 필요한 Next.js 기능을 지원하지 않습니다. 자세한 내용은 확인해주세요.

> 알아두면 좋은 사실:
서버 구성 요소는 정적 익스포트로 지원됩니다.

## 기능

### 이미지 최적화

<div class="content-ad"></div>

이미지 최적화는 next/image를 통해 자체 호스팅되며, next start로 배포할 때 구성 없이 작동합니다. 이미지를 최적화하는 별도의 서비스를 선호한다면 이미지 로더를 구성할 수도 있습니다.

이미지 최적화는 next.config.js에서 사용자 정의 이미지 로더를 정의하여 정적 익스포트와 함께 사용할 수 있습니다. 이미지는 빌드하는 동안이 아닌 실행 시에 최적화됨에 유의해주세요.

> 알고 있으면 좋은 사항:
자체 호스팅 시에는 제품 환경에서 성능을 높이기 위해 프로젝트 디렉토리에서 npm install sharp 명령을 실행하여 sharp를 설치하는 것이 좋습니다. Linux 플랫폼에서는 sharp가 과도한 메모리 사용을 방지하기 위해 추가 구성이 필요할 수 있습니다.
최적화된 이미지의 캐싱 동작 및 TTL(TimetoLive) 구성 방법에 대해 더 알아보세요.
또한 이미지 최적화를 비활성화하고도 next/image의 다른 혜택을 유지하고 싶다면 가능합니다. 예를 들어, 이미지를 별도로 최적화하는 경우입니다.

### 미들웨어

<div class="content-ad"></div>

미들웨어는 `next start`를 사용하여 배포할 때 구성 없이도 자체 호스트에서 작동합니다. 들어오는 요청에 대한 액세스가 필요하기 때문에 정적 익스포트를 사용할 때는 지원되지 않습니다.

미들웨어는 모든 가능한 Node.js API의 하위 집합인 런타임을 사용하여 낮은 대기 시간을 보장하기 위해 앱의 각 라우트나 에셋 앞에서 실행될 수 있습니다. 이 런타임은 "엣지"에서 실행할 필요가 없으며 단일 지역 서버에서 작동합니다. 여러 지역에서 미들웨어를 실행하려면 추가 구성 및 인프라가 필요합니다.

만약 모든 Node.js API를 필요로 하는 로직을 추가하려거나 외부 패키지를 사용하려 한다면, 해당 로직을 서버 컴포넌트로 이동시킬 수 있을 것입니다. 예를 들어, 헤더 확인 및 리디렉션과 같은 작업을 수행할 수 있습니다. 또한, `next.config.js`를 통해 헤더, 쿠키 또는 쿼리 매개변수를 사용하여 리디렉트하거나 리라이트할 수도 있습니다. 그런 방법도 통하지 않는다면, 사용자 정의 서버를 사용할 수도 있습니다.

### 환경 변수

<div class="content-ad"></div>

Next.js는 빌드 시간 및 런타임 환경 변수를 모두 지원할 수 있습니다.

기본적으로 환경 변수는 서버에서만 사용할 수 있습니다. 브라우저에 환경 변수를 노출하려면 NEXT_PUBLIC_로 접두사를 붙여야 합니다. 그러나 이러한 공개 환경 변수는 다음 빌드 중에 JavaScript 번들에 인라인으로 포함됩니다.

런타임 환경 변수를 읽으려면 getServerSideProps를 사용하거나 App Router를 점진적으로 적용하는 것을 권장합니다. App Router를 사용하면 동적 렌더링 중에 서버에서 환경 변수를 안전하게 읽을 수 있습니다. 이를 통해 여러 환경을 위한 다른 값으로 이미지를 확장할 수 있는 단일 Docker 이미지를 사용할 수 있게 됩니다.

```js
import { unstable_noStore as noStore } from 'next/cache';
 
export default function Component() {
  noStore();
  // cookies(), headers(), 및 기타 동적 함수
  // 동적 렌더링에 참여하여, 이 환경 변수는
  // 런타임에 평가됩니다
  const value = process.env.MY_VALUE
  ...
}
```

<div class="content-ad"></div>

> 알아 두면 좋아요:
서버 시작 시 register 함수를 사용하여 코드를 실행할 수 있습니다.
runtimeConfig 옵션을 사용하는 것을 권장하지 않습니다. 이 옵션은 독립적인 출력 모드와 호환되지 않습니다. 대신 앱 라우터를 점진적으로 채택하는 것을 권장합니다.

### 캐싱 및 ISR

Next.js는 응답, 생성된 정적 페이지, 빌드 출력 및 이미지, 글꼴 및 스크립트와 같은 기타 정적 에셋을 캐시할 수 있습니다.

페이지를 캐시하고 재검증하는 것(증분 정적 재생성(Incremental Static Regeneration, ISR) 또는 앱 라우터의 새로운 기능을 사용하는 경우)은 동일한 공유 캐시를 사용합니다. 기본적으로 이 캐시는 Next.js 서버의 파일 시스템(디스크)에 저장됩니다. 이것은 Pages와 App Router를 모두 사용하여 자체 호스팅할 때 자동으로 작동합니다.

<div class="content-ad"></div>

Next.js 캐시 위치를 설정하여 캐시된 페이지 및 데이터를 영구 저장소에 유지하거나 Next.js 애플리케이션의 여러 컨테이너 또는 인스턴스 간에 캐시를 공유하려면 설정할 수 있습니다.

#### 자동 캐싱

- Next.js는 불변한 리소스에 대해 Cache-Control 헤더를 public, max-age=31536000, immutable로 설정합니다. 이 값은 덮어쓸 수 없습니다. 이 불변 파일에는 파일 이름에 SHA 해시가 포함되어 있어 영구적으로 안전하게 캐시될 수 있습니다. 예를 들어, Static Image Imports. 이미지의 TTL을 설정할 수 있습니다.
- 증분 정적 재생성(ISR)은 s-maxage: `getStaticProps에서 revalidate`로 Cache-Control 헤더를 설정하며, stale-while-revalidate로 정의된 재검증 시간이 있습니다. revalidate: false로 설정하면 기본적으로 1년간 캐시됩니다.
- 동적으로 렌더링되는 페이지는 사용자별 데이터가 캐시되지 않도록 private, no-cache, no-store, max-age=0, must-revalidate로 Cache-Control 헤더를 설정합니다. 이는 App Router 및 Pages Router에 모두 적용됩니다. Draft Mode도 포함됩니다.

#### 정적 자산

<div class="content-ad"></div>

만약 다른 도메인이나 CDN에 정적 에셋을 호스팅하려면 next.config.js 파일에서 assetPrefix 설정을 사용할 수 있습니다. Next.js는 JavaScript 또는 CSS 파일을 검색할 때 이 asset 접두사를 사용할 것입니다. 에셋을 다른 도메인으로 분리하는 것은 DNS 및 TLS 해결에 추가 시간이 소요되는 단점이 있습니다.

assetPrefix에 대해 더 알아보기.

#### 캐싱 구성

기본적으로 생성된 캐시 에셋은 메모리(기본값은 50mb)와 디스크에 저장됩니다. 만약 Kubernetes와 같은 컨테이너 오케스트레이션 플랫폼을 사용하여 Next.js를 호스팅하고 있다면, 각 pod에는 캐시의 복사본이 있을 것입니다. 기본적으로 캐시가 팟 간에 공유되지 않기 때문에 오래된 데이터가 표시되는 것을 방지하기 위해 Next.js 캐시를 구성하여 캐시 핸들러를 제공하고 메모리 캐싱을 비활성화할 수 있습니다.

<div class="content-ad"></div>

셀프 호스팅 시 ISR/Data Cache 위치를 구성하려면 next.config.js 파일에서 사용자 지정 핸들러를 구성할 수 있습니다:

```js
module.exports = {
  cacheHandler: require.resolve('./cache-handler.js'),
  cacheMaxMemorySize: 0, // 기본 인메모리 캐싱 비활성화
}
```

그런 다음 프로젝트 루트에 cache-handler.js 파일을 만들어보세요. 아래는 예시입니다:

```js
const cache = new Map()

module.exports = class CacheHandler {
  constructor(options) {
    this.options = options
  }

  async get(key) {
    // 지속 가능한 저장소와 같이 어디에서든 저장되어 있을 수 있습니다
    return cache.get(key)
  }

  async set(key, data, ctx) {
    // 지속 가능한 저장소와 같이 어디에서든 저장되어 있을 수 있습니다
    cache.set(key, {
      value: data,
      lastModified: Date.now(),
      tags: ctx.tags,
    })
  }

  async revalidateTag(tag) {
    // 캐시 내 모든 항목을 반복
    for (let [key, value] of cache) {
      // 값의 태그에 지정된 태그가 포함되어 있다면 해당 항목 삭제
      if (value.tags.includes(tag)) {
        cache.delete(key)
      }
    }
  }
}
```

<div class="content-ad"></div>

사용자 지정 캐시 핸들러를 사용하면 Next.js 애플리케이션을 호스팅하는 모든 팟 간 일관성을 유지할 수 있습니다. 예를 들어, 캐시된 값들을 Redis나 AWS S3와 같은 곳에 저장할 수 있습니다.

> 알아두면 좋은 점:
revalidatePath는 캐시 태그 위에 편의성 레이어입니다. revalidatePath를 호출하면 제공된 페이지에 대한 특별한 기본 태그로 revalidateTag 함수를 호출합니다.

### 빌드 캐시

Next.js는 다음 빌드 중에 애플리케이션의 버전을 식별하기 위해 ID를 생성합니다. 동일한 빌드가 사용되고 여러 컨테이너가 시작될 수 있어야 합니다.

<div class="content-ad"></div>

만약 환경의 각 단계마다 재구축을 하려면, 컨테이너 간에 사용할 일관된 빌드 ID를 생성해야 합니다. next.config.js 파일에서 generateBuildId 명령어를 사용하세요:

```js
module.exports = {
  generateBuildId: async () => {
    // 최신 git 해시를 사용하여 아무 값이나 반환할 수 있습니다
    return process.env.GIT_HASH
  },
}
```

### 버전 스큐

Next.js는 대부분의 버전 스큐 상황을 자동으로 완화하고 감지될 때 새로운 에셋을 검색하기 위해 애플리케이션을 자동으로 다시로드합니다. 예를 들어, 배포 ID가 불일치하는 경우 페이지 간 이동은 사전 로드된 값 대신 강력한 네비게이션을 수행할 것입니다.

<div class="content-ad"></div>

응용 프로그램을 다시로드할 때 페이지 이동 사이에 유지되지 않도록 설계되지 않은 경우 응용 프로그램 상태 손실이 발생할 수 있습니다. 예를 들어 URL 상태나 로컬 저장소를 사용하면 페이지 새로 고침 후에도 상태가 유지됩니다. 그러나 useState와 같은 컴포넌트 상태는 이러한 이동에서 손실됩니다.

Vercel은 이전 버전의 자산 및 기능이 기존 클라이언트에 여전히 제공되도록 하는 Next.js 애플리케이션에 대한 추가적인 스쿠 보호 기능을 제공합니다. 이는 새 버전이 배포된 후에도 이전 버전의 자산과 기능이 이전 클라이언트에 대해 여전히 이용 가능하도록 하는 것입니다.

next.config.js 파일에서 deploymentId 속성을 수동으로 구성하여 각 요청이 ?dpl 쿼리 문자열이나 x-deployment-id 헤더를 사용하도록 보장할 수 있습니다.

### 스트리밍 및 서스펜스

<div class="content-ad"></div>

Next.js 앱 라우터는 자체 호스팅할 때 스트리밍 응답을 지원합니다. Nginx나 유사한 프록시를 사용하는 경우 스트리밍을 활성화하려면 버퍼링을 비활성화하도록 구성해야 합니다.

예를 들어, Nginx에서 X-Accel-Buffering을 no로 설정하여 버퍼링을 비활성화할 수 있습니다:

```js
module.exports = {
  async headers() {
    return [
      {
        source: '/:path*{/}?',
        headers: [
          {
            key: 'X-Accel-Buffering',
            value: 'no',
          },
        ],
      },
    ]
  },
}
```