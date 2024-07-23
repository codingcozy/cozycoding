---
title: "Nextjs 14에서 메모리 사용량 최적화하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:43
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Memory Usage"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/memory-usage"
---


# 메모리 사용량

애플리케이션이 커지고 더 많은 기능을 갖게 되면 로컬 개발이나 프로덕션 빌드를 생성할 때 더 많은 리소스를 필요로 할 수 있습니다.

Next.js에서 메모리를 최적화하고 일반적인 메모리 문제를 해결하기 위한 몇 가지 전략과 기술을 살펴봅시다.

## 의존성 개수 줄이기

<div class="content-ad"></div>

의존성이 많은 애플리케이션은 더 많은 메모리를 사용할 수 있습니다.

번들 분석기를 사용하면 애플리케이션 내의 큰 의존성을 조사하여 성능 및 메모리 사용량을 향상시키기 위해 제거할 수 있는 항목을 확인할 수 있습니다.

## --experimental-debug-memory-usage 플래그를 사용하여 다음 빌드 실행하기

14.2.0부터 --experimental-debug-memory-usage를 사용하여 다음 빌드를 실행할 수 있습니다. 이 모드에서 Next.js는 힙 사용량 및 가비지 컬렉션 통계와 같이 빌드 중에 지속적으로 메모리 사용에 관한 정보를 출력합니다. 메모리 사용량이 구성된 한도에 접근하면 힙 스냅샷이 자동으로 촬영됩니다.

<div class="content-ad"></div>

> 좋은 정보입니다: 이 기능은 사용자 정의 webpack 구성이 없는 한 자동으로 활성화되는 Webpack 빌드 워커 옵션과 호환되지 않습니다.

## 힙 프로파일 기록

메모리 문제를 찾기 위해 Node.js에서 힙 프로파일을 기록하고 Chrome DevTools에서로드하여 메모리 누수의 잠재적 원인을 식별할 수 있습니다.

터미널에서 Next.js 빌드를 시작할 때 Node.js에 --heap-prof 플래그를 전달하세요.

<div class="content-ad"></div>

```js
node --heap-prof node_modules/next/dist/bin/next build
```

빌드가 마무리되면 Node.js에 의해 .heapprofile 파일이 생성됩니다.

Chrome 개발자 도구에서는 Memory 탭을 열고 "프로필 로드" 버튼을 클릭하여 파일을 시각화할 수 있습니다.

## 힙 스냅샷 분석


<div class="content-ad"></div>

애플리케이션의 메모리 사용량을 분석하려면 Inspector 도구를 사용할 수 있어요.

다음 빌드 또는 다음 개발 명령을 실행할 때, 명령어의 시작 부분에 NODE_OPTIONS=--inspect를 추가하세요. 이렇게 하면 기본 포트에서 Inspector 에이전트가 노출됩니다. 사용자 코드가 시작되기 전에 중단하려면 --inspect-brk를 전달할 수도 있어요. 프로세스가 실행되는 동안 Chrome DevTools 같은 도구를 사용하여 디버깅 포트에 연결하여 힙 스냅샷을 기록하고 분석하여 어떤 메모리가 보존되는지 확인할 수 있어요.

14.2.0부터는 --experimental-debug-memory-usage 플래그를 사용하여 next build를 실행하여 힙 스냅샷을 쉽게 촬영할 수 있어요.

이 모드에서 실행 중일 때, 프로세스에 언제든지 SIGUSR2 신호를 보낼 수 있고 프로세스는 힙 스냅샷을 촬영할 거에요.

<div class="content-ad"></div>

힙 스냅샷은 Next.js 애플리케이션의 프로젝트 루트에 저장되며 Chrome DevTools와 같은 힙 분석 도구에서 메모리 유지 상태를 확인할 수 있습니다. 이 모드는 아직 Webpack 빌드 워커와 호환되지 않습니다.

자세한 내용은 힙 스냅샷 기록 및 분석 방법을 확인하세요.

## Webpack 빌드 워커

Webpack 빌드 워커를 사용하면 별도의 Node.js 워커 내에서 Webpack 컴파일을 실행하여 빌드 중에 응용 프로그램의 메모리 사용량을 줄일 수 있습니다.

<div class="content-ad"></div>

이 옵션은 v14.1.0부터 사용자의 애플리케이션이 사용자 정의 Webpack 구성을 가지고 있지 않은 경우 기본적으로 활성화됩니다.

이전 버전의 Next.js를 사용하거나 사용자 정의 Webpack 구성이 있는 경우 next.config.js 내에서 experimental.webpackBuildWorker: true로 설정하여 이 옵션을 활성화할 수 있습니다.

> 알아두세요: 이 기능은 모든 사용자 정의 Webpack 플러그인과 호환되지 않을 수 있습니다.

## Webpack 캐시 비활성화

<div class="content-ad"></div>

웹팩 캐시는 빌드 속도를 향상시키기 위해 생성된 웹팩 모듈을 메모리와/또는 디스크에 저장합니다. 이는 성능 향상에 도움이 될 수 있지만, 캐시된 데이터를 저장하기 위해 응용 프로그램의 메모리 사용량이 늘어날 수도 있습니다.

이 동작을 비활성화하려면 애플리케이션에 사용자 지정 웹팩 구성을 추가하면 됩니다:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  webpack: (
    config,
    { buildId, dev, isServer, defaultLoaders, nextRuntime, webpack }
  ) => {
    if (cfg.cache && !dev) {
      cfg.cache = Object.freeze({
        type: 'memory',
      })
      cfg.cache.maxMemoryGenerations = 0
    }
    // 중요: 수정된 구성을 반환합니다.
    return config
  },
}
 
export default nextConfig
```

## 소스 맵 비활성화

<div class="content-ad"></div>

빌드 프로세스 중에 소스 맵을 생성하는 것은 추가 메모리를 사용합니다.

Next.js 구성에 productionBrowserSourceMaps: false와 experimental.serverSourceMaps: false를 추가하여 소스 맵 생성을 비활성화할 수 있습니다.

> 알아 두면 좋은 점: 일부 플러그인은 소스 맵을 활성화할 수 있으며 비활성화하려면 사용자 정의 구성이 필요할 수 있습니다.

## Edge 메모리 문제

<div class="content-ad"></div>

Next.js v14.1.3는 Edge 런타임을 사용할 때 발생하는 메모리 문제를 해결했어요. 문제를 해결하는 데 이 버전(또는 그 이후 버전)을 업데이트해보세요.