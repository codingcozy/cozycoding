---
title: "Nextjs 14에서 사용하기 좋은 타사 라이브러리 TOP 10"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:42
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Third Party Libraries"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/third-party-libraries"
---


# 써드 파티 라이브러리

@next/third-parties는 Next.js 애플리케이션에서 인기있는 써드 파티 라이브러리를로드하는 성능 및 개발자 경험을 향상시키는 구성 요소 및 유틸리티 모음을 제공하는 라이브러리입니다.

@next/third-parties가 제공하는 모든 써드 파티 통합은 성능 및 사용 편의성을 최적화되었습니다.

## 시작하기

<div class="content-ad"></div>

시작하려면 @next/third-parties 라이브러리를 설치해주세요:

```js
npm install @next/third-parties@latest next@latest
```

@next/third-parties는 현재 활발히 개발 중인 실험적인 라이브러리입니다. 더 많은 타사 통합 기능을 추가하는 동안 최신 또는 캐너리 플래그와 함께 설치하는 것을 권장합니다.

## Google Third-Parties

<div class="content-ad"></div>

Google의 모든 지원되는 서드파티 라이브러리는 @next/third-parties/google에서 가져올 수 있습니다.

### Google Tag Manager

GoogleTagManager 구성 요소를 사용하여 Google Tag Manager 컨테이너를 페이지에 인스턴스화할 수 있습니다. 기본적으로 페이지에서 하이드레이션이 발생한 후 원래의 인라인 스크립트를 가져옵니다.

모든 경로에 Google Tag Manager를 로드하려면 루트 레이아웃에 구성 요소를 직접 포함하고 GTM 컨테이너 ID를 전달하세요:

<div class="content-ad"></div>

```js
import { GoogleTagManager } from '@next/third-parties/google'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <GoogleTagManager gtmId="GTM-XYZ" />
      <body>{children}</body>
    </html>
  )
}
```

Google Tag Manager를 특정 경로에 로드하려면 페이지 파일에 컴포넌트를 포함하십시오:

```js
import { GoogleTagManager } from '@next/third-parties/google'
 
export default function Page() {
  return <GoogleTagManager gtmId="GTM-XYZ" />
}
```

#### 이벤트 전송하기


<div class="content-ad"></div>

sendGTMEvent 함수는 dataLayer 객체를 사용하여 페이지에서 사용자 상호작용을 추적하는 데 사용할 수 있습니다. 이 함수가 작동하려면 `GoogleTagManager /` 구성 요소가 부모 레이아웃, 페이지 또는 컴포넌트 또는 동일한 파일에 직접 포함되어 있어야 합니다.

```js
'use client'

import { sendGTMEvent } from '@next/third-parties/google'

export function EventButton() {
  return (
    <div>
      <button
        onClick={() => sendGTMEvent({ event: 'buttonClicked', value: 'xyz' })}
      >
        Send Event
      </button>
    </div>
  )
}
```

태그 관리자 개발자 문서를 참조하여 함수로 전달할 수 있는 다른 변수 및 이벤트에 대해 알아보세요.

#### Options

<div class="content-ad"></div>

Google Tag Manager에 전달할 옵션들이 있어요. 전체 옵션 목록을 보려면 Google Tag Manager 문서를 읽어보세요.

### Google Analytics

GoogleAnalytics 구성 요소를 사용하여 Google Analytics 4를 페이지에 포함시킬 수 있어요. 기본적으로 페이지에서 수분 후에 원본 스크립트를 가져와요.

<div class="content-ad"></div>

> 권고: 애플리케이션에 이미 Google Tag Manager가 포함되어 있다면 별도의 Google Analytics 구성 대신에 직접 Google Analytics를 사용할 수 있습니다. Tag Manager와 gtag.js의 차이에 대해 자세히 알아보려면 문서를 참조해주세요.

모든 경로에 Google Analytics를 로드하려면 루트 레이아웃에 구성 요소를 직접 포함하고 측정 ID를 전달하세요:

```js
import { GoogleAnalytics } from '@next/third-parties/google'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <GoogleAnalytics gaId="G-XYZ" />
    </html>
  )
}
```

단일 경로에 Google Analytics를 로드하려면 해당 페이지 파일에 구성 요소를 포함하면 됩니다:

<div class="content-ad"></div>

```js
import { GoogleAnalytics } from '@next/third-parties/google'
 
export default function Page() {
  return <GoogleAnalytics gaId="G-XYZ" />
}
```

#### 이벤트 전송

sendGAEvent 함수를 사용하여 데이터 레이어 객체를 통해 이벤트를 보내 사용자 상호작용을 측정할 수 있습니다. 이 함수를 사용하려면 `GoogleAnalytics` 컴포넌트가 상위 레이아웃, 페이지 또는 컴포넌트에 포함되어 있거나 직접 같은 파일에 포함되어 있어야 합니다.

```js
'use client'

import { sendGAEvent } from '@next/third-parties/google'
 
export function EventButton() {
  return (
    <div>
      <button
        onClick={() => sendGAEvent({ event: 'buttonClicked', value: 'xyz' })}
      >
        Send Event
      </button>
    </div>
  )
}
```

<div class="content-ad"></div>

Google Analytics 개발자 문서를 참조하여
이벤트 매개변수에 대해 더 알아보세요.

#### 페이지뷰 추적

Google Analytics는 브라우저 히스토리 상태가 변경될 때 자동으로 페이지뷰를 추적합니다. 이는 Next.js 라우트 간의 클라이언트 측 이동이 어떤 구성 없이도 페이지뷰 데이터를 전송한다는 것을 의미합니다.

클라이언트 측 이동이 올바르게 측정되고 있는지 확인하려면, 관리자 패널에서 “향상된 측정” 속성이 활성화되어 있고 “브라우저 히스토리 이벤트에 기반한 페이지 변경” 확인란이 선택되어 있는지 확인하세요.

<div class="content-ad"></div>

> 참고: 페이지뷰 이벤트를 수동으로 보내기로 결정하면, 중복 데이터가 발생하지 않도록 기본 페이지뷰 측정을 비활성화해야 합니다. Google Analytics 개발자 문서를 참조하여 더 많은 정보를 알아보세요.

#### 옵션

`GoogleAnalytics` 컴포넌트에 전달할 옵션들입니다.


| 이름           | 타입      | 설명                                                                      |
|---------------|---------|------------------------------------------------------------------------|
| `gaId`        | 필수     | <a href="https://support.google.com/analytics/answer/12270356" rel="noopener noreferrer nofollow" target="_blank">측정 ID<span class="inline-flex"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24" style="color:currentColor;width:14px;height:14px"><path d="M7 17L17 7"></path><path d="M7 7h10v10"></path></svg></span></a>을 입력해주세요. 보통 <code>G-</code>로 시작됩니다. |
| `dataLayerName` | 선택사항   | 데이터 레이어의 이름입니다. 기본값은 <code>dataLayer</code>입니다.            |


<div class="content-ad"></div>

### Google Maps Embed

GoogleMapsEmbed 컴포넌트를 사용하면 페이지에 Google Maps Embed를 추가할 수 있습니다. 기본적으로 아래로 스크롤해서 보이는 부분 아래에 있는 embed를 lazy-load하는 loading 속성을 사용합니다.

```js
import { GoogleMapsEmbed } from '@next/third-parties/google'
 
export default function Page() {
  return (
    <GoogleMapsEmbed
      apiKey="XYZ"
      height={200}
      width="100%"
      mode="place"
      q="Brooklyn+Bridge,New+York,NY"
    />
  )
}
```

#### 옵션

<div class="content-ad"></div>

Google Maps Embed에 전달할 옵션입니다. 전체 옵션 목록을 보려면 Google Map Embed 문서를 참조하세요.

| 이름 | 유형 | 설명 |
|------|-------|------|
| `apiKey` | 필수 | API 키를 입력하세요. |
| `mode` | 필수 | <a href="https://developers.google.com/maps/documentation/embed/embedding-map#choosing_map_modes" rel="noopener noreferrer nofollow" target="_blank">지도 모드<span class="inline-flex"><svg class="with-icon_icon__MHUeb" data-testid="geist-icon" fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24" style="color:currentColor;width:14px;height:14px"><path d="M7 17L17 7"></path><path d="M7 7h10v10"></path></svg></span></a>를 선택하세요. |
| `height` | 선택사항 | 임베드의 높이. 기본값은 `auto`입니다. |
| `width` | 선택사항 | 임베드의 너비. 기본값은 `auto`입니다. |
| `style` | 선택사항 | iframe에 스타일을 전달하세요. |
| `allowfullscreen` | 선택사항 | 특정 지도 부분을 전체 화면으로 확대할 수 있도록 하는 속성입니다. |
| `loading` | 선택사항 | 기본값은 lazy입니다. 임베드가 화면 위쪽에 표시될 것을 알 경우 고려해야 합니다. |
| `q` | 선택사항 | 지도 마커 위치를 정의합니다. <em>지도 모드에 따라 필요할 수 있습니다.</em> |
| `center` | 선택사항 | 지도 보기의 중심을 정의합니다. |
| `zoom` | 선택사항 | 지도의 초기 확대 수준을 설정합니다. |
| `maptype` | 선택사항 | 로드할 지도 타일의 유형을 정의합니다. |
| `language` | 선택사항 | UI 요소와 지도 타일 레이블의 표시에 사용할 언어를 정의합니다. |
| `region` | 선택사항 | 지리적 민감성에 기초하여 표시할 적절한 테두리와 레이블을 정의합니다. |

### YouTube 임베드

YouTubeEmbed 구성 요소는 YouTube 임베드를 로드하고 표시하는 데 사용할 수 있습니다. 내부적으로 lite-youtube-embed를 사용하여 빠르게 로드됩니다.

<div class="content-ad"></div>

```js
import { YouTubeEmbed } from '@next/third-parties/google'

export default function Page() {
  return <YouTubeEmbed videoid="ogfYd705cRs" height={400} params="controls=0" />
}
```

#### Options

Name | Type | Description
--- | --- | ---
`videoid` | Required | YouTube video id.
`width` | Optional | Width of the video container. Defaults to `auto`
`height` | Optional | Height of the video container. Defaults to `auto`
`playlabel` | Optional | A visually hidden label for the play button for accessibility.
`params` | Optional | The video player params defined [here](https://developers.google.com/youtube/player_parameters#Parameters). <br> Params are passed as a query param string. <br> Eg: `params="controls=0&start=10&end=30"`
`style` | Optional | Used to apply styles to the video container.
