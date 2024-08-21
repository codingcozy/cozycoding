---
title: "2024년 Nextjs 14에서 OpenTelemetry를 활용하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:41
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "OpenTelemetry"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/open-telemetry"
isUpdated: true
---




# OpenTelemetry

> 좋은 정보: 이 기능은 실험적인 기능입니다. 다음 단계에서 experimental.instrumentationHook = true;을 제공하여 명시적으로 수락해야 합니다.

관측 가능성은 Next.js 앱의 동작과 성능을 이해하고 최적화하는 데 중요합니다.

응용 프로그램이 더 복잡해지면 발생할 수 있는 문제를 식별하고 진단하기가 점점 어려워집니다. 관측 가능성 도구인 로깅 및 메트릭과 같은 도구를 활용하여 개발자는 응용 프로그램의 동작에 대한 통찰력을 얻고 최적화할 수 있는 영역을 식별할 수 있습니다. 관측 가능성을 통해 개발자는 문제가 복잡해지기 전에 문제를 사전에 해결하고 사용자 경험을 개선할 수 있습니다. 따라서 Next.js 애플리케이션에서 관측 가능성을 사용하여 성능을 향상시키고 리소스를 최적화하며 사용자 경험을 향상시키는 것이 매우 권장됩니다.

<div class="content-ad"></div>

앱을 계측하기 위해 OpenTelemetry을 사용하는 것을 추천합니다. 이는 앱을 계측하기 위한 플랫폼에 중립적인 방식으로, 코드를 변경하지 않고도 관찰성 제공자를 변경할 수 있게 해줍니다. OpenTelemetry에 대한 자세한 정보는 공식 OpenTelemetry 문서를 참조해주세요.

이 문서는 Span, Trace 또는 Exporter와 같은 용어를 사용하며, 모두 OpenTelemetry Observability Primer에서 찾을 수 있습니다.

Next.js는 OpenTelemetry 계측을 기본 제공하므로 이미 Next.js 자체가 계측되어 있습니다. OpenTelemetry을 활성화하면 getStaticProps와 같은 코드를 자동으로 유용한 속성이 포함된 spans 안에 래핑할 것입니다.

## 시작하기

<div class="content-ad"></div>

OpenTelemetry은 확장 가능하지만 올바르게 설정하는 것은 다소 많은 작업일 수 있습니다. 그래서 우리는 빠르게 시작할 수 있도록 도와주는 패키지 @vercel/otel를 준비했습니다.

### @vercel/otel 사용하기

시작하려면 @vercel/otel을 설치해야 합니다:

```js
npm install @vercel/otel
```

<div class="content-ad"></div>

다음으로, 프로젝트의 루트 디렉토리에 사용자 정의 instrumentation.ts (또는 .js) 파일을 만들어 주세요 (또는 src 폴더 내에서 사용 중이라면 해당 폴더 안에 만들어 주세요):

```js
import { registerOTel } from '@vercel/otel'
 
export function register() {
  registerOTel({ serviceName: 'next-app' })
}
```

추가 구성 옵션에 대한 정보는 @vercel/otel 문서를 참조하세요.

> 알아두면 좋은 사항
instrumentation 파일은 프로젝트의 루트에 있어야하며 앱이나 페이지 디렉토리 안에 있으면 안 됩니다. src 폴더를 사용하는 경우에는 파일을 src 폴더 안에 페이지와 앱과 함께 배치해야 합니다.
suffix를 추가하기 위해 pageExtensions 구성 옵션을 사용하는 경우, instrumentation 파일 이름도 해당 suffix에 맞게 업데이트해야 합니다.
with-opentelemetry 예시를 활용할 수 있는 기본 예제를 만들어 두었습니다.

<div class="content-ad"></div>

### 수동 OpenTelemetry 설정

@vercel/otel 패키지는 많은 구성 옵션을 제공하며 대부분의 일반적인 사용 사례를 다룰 수 있습니다. 그러나 여러분의 요구에 맞지 않는 경우 OpenTelemetry를 수동으로 구성할 수 있습니다.

먼저 OpenTelemetry 패키지를 설치해야 합니다:

```js
npm install @opentelemetry/sdk-node @opentelemetry/resources @opentelemetry/semantic-conventions @opentelemetry/sdk-trace-node @opentelemetry/exporter-trace-otlp-http
```

<div class="content-ad"></div>

이제 instrumentation.ts 파일에서 NodeSDK를 초기화할 수 있습니다. @vercel/otel과 다르게 NodeSDK는 edge runtime과 호환되지 않으므로 process.env.NEXT_RUNTIME === 'nodejs' 일 때에만 import하도록 해야 합니다. node를 사용할 때만 조건적으로 import할 수 있도록 새 파일인 instrumentation.node.ts를 생성하는 것을 추천합니다:

```js
export async function register() {
  if (process.env.NEXT_RUNTIME === 'nodejs') {
    await import('./instrumentation.node.ts')
  }
}
```

```js
import { NodeSDK } from '@opentelemetry/sdk-node'
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-http'
import { Resource } from '@opentelemetry/resources'
import { SEMRESATTRS_SERVICE_NAME } from '@opentelemetry/semantic-conventions'
import { SimpleSpanProcessor } from '@opentelemetry/sdk-trace-node'
 
const sdk = new NodeSDK({
  resource: new Resource({
    [SEMRESATTRS_SERVICE_NAME]: 'next-app',
  }),
  spanProcessor: new SimpleSpanProcessor(new OTLPTraceExporter()),
})
sdk.start()
```

이렇게 하면 @vercel/otel을 사용하는 것과 동일하지만, @vercel/otel에서 노출되지 않는 기능을 수정하거나 확장할 수 있습니다. edge runtime 지원이 필요하다면 @vercel/otel을 사용해야 합니다.

<div class="content-ad"></div>

## OpenTelemetry 계측 도구 테스트

로컬에서 OpenTelemetry 추적을 테스트하려면 호환되는 백엔드를 사용하는 OpenTelemetry 수집기가 필요합니다. 저희의 OpenTelemetry 개발 환경을 사용하는 것을 권장합니다.

모든 것이 잘 작동하면 루트 서버 스팬이 GET /requested/pathname로 표시되는 것을 볼 수 있어야 합니다. 해당 추적에서 발생하는 다른 모든 스팬은 해당 스팬 하위에 중첩됩니다.

Next.js는 기본으로 발생하는 것보다 더 많은 스팬을 추적합니다. 더 많은 스팬을 보려면 NEXT_OTEL_VERBOSE=1을 설정해야 합니다.

<div class="content-ad"></div>

## 배포

### OpenTelemetry Collector 사용

OpenTelemetry Collector로 배포하는 경우 @vercel/otel을 사용할 수 있습니다. Vercel 및 자체 호스팅 시 모두 작동합니다.

#### Vercel에 배포하기

<div class="content-ad"></div>

우리는 오픈텔레미트리가 Vercel에서 쉽게 작동되도록 보장했어요.

[Vercel 문서](https://vercel.com/docs)
를 따라서 프로젝트를 관찰 공급 업체에 연결해주세요.

#### 자체 호스팅

다른 플랫폼으로 배포하는 것도 간단해요. 당신의 Next.js 앱에서 텔레미트리 데이터를 받아들이고 처리할 오픈텔레미트리 수집기를 구동해야 해요.

<div class="content-ad"></div>

이를 위해 OpenTelemetry Collector 시작 안내서를 따르세요. 해당 안내서를 통해 수집기를 설정하고 Next.js 앱에서 데이터를 수신할 수 있도록 구성하는 방법을 안내받을 수 있습니다.

수집기를 설정하고 실행한 후에는 각 플랫폼의 배포 안내를 따라 Next.js 앱을 선택한 플랫폼에 배포할 수 있습니다.

### 사용자 지정 내보내기 도구

OpenTelemetry Collector를 사용할 필요가 없습니다. @vercel/otel 또는 수동 OpenTelemetry 구성을 사용하여 사용자 지정 OpenTelemetry 내보내기 도구를 사용할 수 있습니다.

<div class="content-ad"></div>

## 사용자 지정 Span

OpenTelemetry API를 사용하여 사용자 지정 Span을 추가할 수 있습니다.

```js
npm install @opentelemetry/api
```

다음 예시는 GitHub star를 가져와서 가져온 결과를 추적하기 위해 사용자 정의 fetchGithubStars Span을 추가하는 함수를 보여줍니다.

<div class="content-ad"></div>

```js
import { trace } from '@opentelemetry/api'

export async function fetchGithubStars() {
  return await trace
    .getTracer('nextjs-example')
    .startActiveSpan('fetchGithubStars', async (span) => {
      try {
        return await getValue()
      } finally {
        span.end()
      }
    })
}
```

register 함수는 새로운 환경에서 코드가 실행되기 전에 실행됩니다. 새로운 span을 만들기 시작하면 내보낸 추적에 정확히 추가되어야 합니다.

## Next.js의 기본 Spans

Next.js는 여러분이 응용프로그램 성능에 유용한 통찰을 제공하기 위해 몇 가지 span을 자동으로 instrument합니다.


<div class="content-ad"></div>

속성들은 OpenTelemetry의 의미론적 규칙을 따릅니다. 그리고 다음 네임스페이스 아래 사용자 정의 속성을 추가합니다:

- next.span_name - 스팬 이름을 복제합니다.
- next.span_type - 각 스팬 유형은 고유한 식별자를 가집니다.
- next.route - 요청의 경로 패턴 (예: / [param] /user).
- next.rsc (true/false) - 요청이 RSC 요청인지 여부, prefetch와 같은 요청을 나타냅니다.
- next.page
앱 라우터에서 사용하는 내부 값입니다.
특별한 파일 경로(예: page.ts, layout.ts, loading.ts 및 기타)로의 경로로 생각할 수 있습니다.
next.route와 쌍을 이룰 때에만 고유 식별자로 사용할 수 있습니다. 왜냐하면 /(groupA)/layout.ts와 /(groupB)/layout.ts 둘 다를 식별할 수 있기 때문입니다. 

### [http.method] [next.route]

- next.span_type: BaseServer.handleRequest

<div class="content-ad"></div>

이 span은 Next.js 애플리케이션으로 들어오는 각 요청의 루트 span을 나타냅니다. 요청의 HTTP 메서드, 경로, 대상 및 상태 코드를 추적합니다.

속성:

- 일반적인 HTTP 속성
  - http.method
  - http.status_code
- 서버 HTTP 속성
  - http.route
  - http.target
- next.span_name
- next.span_type
- next.route

### 렌더 경로 (앱) [next.route]

<div class="content-ad"></div>

- next.span_type: AppRender.getBodyResult.

이 스팬은 앱 라우터에서 라우트를 렌더링하는 과정을 나타냅니다.

속성:

- next.span_name
- next.span_type
- next.route

<div class="content-ad"></div>

### fetch [http.method] [http.url]

- next.span_type: AppRender.fetch

이 span은 코드에서 실행된 fetch 요청을 나타냅니다.

속성:

<div class="content-ad"></div>

- 공통 HTTP 속성
  - http.method
- 클라이언트 HTTP 속성
  - http.url
  - net.peer.name
  - net.peer.port (지정된 경우에만)
  - http.url
  - net.peer.name
  - net.peer.port (지정된 경우에만)
  - next.span_name
  - next.span_type

이 span은 환경 변수에서 NEXT_OTEL_FETCH_DISABLED=1로 설정하여 끌 수 있습니다. 사용자 지정 fetch instrumentation 라이브러리를 사용하고 싶을 때 유용합니다.

### 실행 중인 API 경로 (앱) [next.route]

- next.span_type: AppRouteRouteHandlers.runHandler.

<div class="content-ad"></div>

이 span은 앱 라우터에서 API 라우트 핸들러의 실행을 나타냅니다.

속성:

- next.span_name
- next.span_type
- next.route

### getServerSideProps [next.route]

<div class="content-ad"></div>

- 다음.span_type: Render.getServerSideProps.

이 span은 특정 경로에 대한 getServerSideProps의 실행을 나타냅니다.

속성:

- 다음.span_name
- 다음.span_type
- 다음.route

<div class="content-ad"></div>

### getStaticProps [next.route]

- next.span_type: Render.getStaticProps.

해당 span은 특정 route에 대해 getStaticProps의 실행을 나타냅니다.

속성:

<div class="content-ad"></div>

- next.span_name
- next.span_type
- next.route

### 렌더 루트 (페이지) [next.route]

- next.span_type: Render.renderDocument.

이 스팬은 특정 경로에 대한 문서 렌더링 프로세스를 나타냅니다.

<div class="content-ad"></div>

특성:

- next.span_name
- next.span_type
- next.route

### generateMetadata [next.page]

- next.span_type: ResolveMetadata.generateMetadata.

<div class="content-ad"></div>

이 span은 특정 페이지에 대한 메타데이터를 생성하는 과정을 나타냅니다 (단일 경로에는 이러한 span이 여러 개 포함될 수 있음).

속성:

- next.span_name
- next.span_type
- next.page

### 페이지 구성요소 해결

<div class="content-ad"></div>

- next.span_type: NextNodeServer.findPageComponents.

이 span은 특정 페이지의 페이지 구성 요소를 해결하는 과정을 나타냅니다.

속성:

- next.span_name
- next.span_type
- next.route

<div class="content-ad"></div>

### 세그먼트 모듈 해결

- next.span_type: NextNodeServer.getLayoutOrPageModule.

이 span은 레이아웃이나 페이지를 위한 코드 모듈을 로딩하는 것을 나타냅니다.

속성:

<div class="content-ad"></div>

- next.span_name
- next.span_type
- next.segment

### 응답 시작

- next.span_type: NextNodeServer.startResponse.

이 길이가 0인 span은 응답의 첫 번째 바이트가 보내졌을 때의 시간을 나타냅니다.