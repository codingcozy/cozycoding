---
title: "Nextjs 14에서 Instrumentation 쉽게 구현하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:40
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Instrumentation"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/instrumentation"
isUpdated: true
---




# Instrumentation

Instrumentation은 코드를 사용하여 감시 및 로깅 도구를 응용 프로그램에 통합하는 과정입니다. 이를 통해 응용 프로그램의 성능 및 동작을 추적하고 본격적인 문제 해결을 할 수 있습니다.

## 관행

Instrumentation을 설정하려면 프로젝트의 루트 디렉토리에 instrumentation.ts|js 파일을 생성하십시오 (또는 src 폴더 내에서 사용 중인 경우 해당 폴더 내에).

<div class="content-ad"></div>

해당 파일에는 register 함수를 내보내십시오. 이 함수는 새로운 Next.js 서버 인스턴스가 초기화될 때 한 번 호출됩니다.

예를 들어, Next.js를 OpenTelemetry 및 @vercel/otel로 사용하려면:

```js
import { registerOTel } from '@vercel/otel'
 
export function register() {
  registerOTel('next-app')
}
```

Next.js와 OpenTelemetry 예시에서 전체 구현을 확인할 수 있습니다.

<div class="content-ad"></div>

> 좋은 정보입니다
이 기능은 실험적입니다. 사용하려면 experimental.instrumentationHook = true;를 당신의 next.config.js에 정의하여 명시적으로 사용해야 합니다.
계측 파일은 프로젝트의 루트에 있어야 하며 앱이나 페이지 디렉토리 안에 있으면 안 됩니다. src 폴더를 사용하고 있다면 파일을 src 폴더 안에 pages와 app과 함께 배치해주어야 합니다.
pageExtensions 구성 옵션을 사용하여 접두사를 추가하는 경우 계측 파일 이름도 일치하도록 업데이트해야 합니다.

## 예시

### 부수효과가 있는 파일 가져오기

가끔 코드에서 파일을 가져오는 것이 유용할 수 있습니다. 예를 들어 전역 변수 집합을 정의하는 파일을 가져와서 코드에서 명시적으로 사용하지 않고 파일을 가져올 수 있습니다. 여전히 패키지가 선언한 전역 변수에 액세스할 수 있을 것입니다.

<div class="content-ad"></div>

JavaScript import 구문을 사용하여 파일을 가져오는 것을 권장합니다. 다음 예시는 레지스터 함수에서 import의 기본적인 사용법을 보여줍니다:

```js
export async function register() {
  await import('package-with-side-effect')
}
```

> 알아두면 좋은 점:
파일을 파일 상단이 아닌 레지스터 함수 내에서 가져오는 것을 권장합니다. 이렇게 하면 코드 내에서 모든 사이드 이펙트를 한 곳에 모을 수 있고, 파일 상단에서 전역으로 가져오는 것으로 인한 의도치 않은 결과를 피할 수 있습니다.

### 런타임별 코드 가져오기

<div class="content-ad"></div>

Next.js는 모든 환경에서 register를 호출하기 때문에 특정 런타임(예: Edge 또는 Node.js)을 지원하지 않는 코드를 조건부로 가져와야 합니다. 현재 환경을 확인하려면 NEXT_RUNTIME 환경 변수를 사용할 수 있습니다:

```js
export async function register() {
  if (process.env.NEXT_RUNTIME === 'nodejs') {
    await import('./instrumentation-node')
  }
 
  if (process.env.NEXT_RUNTIME === 'edge') {
    await import('./instrumentation-edge')
  }
}
```