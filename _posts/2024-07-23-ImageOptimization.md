---
title: "Nextjs 14에서 이미지 최적화하는 방법 최신 App Router 기능 활용하기"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:35
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Image Optimization"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/images"
---


# 이미지 최적화

웹 알마나크에 따르면 이미지는 일반 웹사이트 페이지 무게의 상당 부분을 차지하며 웹사이트의 LCP 성능에 상당한 영향을 미칠 수 있습니다.

Next.js 이미지 구성 요소는 자동 이미지 최적화 기능을 갖춘 HTML `img` 요소를 확장합니다.

- 크기 최적화: 각 기기에 맞게 올바른 크기의 이미지를 자동으로 제공하며 WebP 및 AVIF와 같은 현대 이미지 형식을 사용합니다.
- 시각적 안정성: 이미지를 로딩할 때 레이아웃 이동을 자동으로 방지합니다.
- 빠른 페이지 로드: 네이티브 브라우저 지연 로딩을 사용하여 뷰포트에 이미지가 들어올 때만 이미지를로딩하며 옵션 블러 업 플레이스홀더를 사용합니다.
- 자산 유연성: 원격 서버에 저장된 이미지도 필요에 따라 이미지 크기 조정을 지원합니다.

<div class="content-ad"></div>

> 🎥 시청: next/image 사용 방법에 대해 더 알아보세요 → YouTube (9분)
.

## 사용법

```js
import Image from 'next/image'
```

그런 다음 이미지의 src를 정의할 수 있습니다 (로컬 또는 원격).

<div class="content-ad"></div>

### 로컬 이미지

로컬 이미지를 사용하려면 .jpg, .png 또는 .webp 이미지 파일을 가져와야 합니다.

Next.js는 가져온 파일을 기반으로 이미지의 너비와 높이를 자동으로 결정합니다. 이 값들은 이미지가 로딩되는 동안 Cumulative Layout Shift를 방지하는 데 사용됩니다.

```js
import Image from 'next/image'
import profilePic from './me.png'
 
export default function Page() {
  return (
    <Image
      src={profilePic}
      alt="저자의 사진"
      // width={500} 자동 제공
      // height={500} 자동 제공
      // blurDataURL="data:..." 자동 제공
      // placeholder="blur" // 로딩 중에 선택적으로 blur-up 가능
    />
  )
}
```

<div class="content-ad"></div>

> 경고: 동적으로 await import() 또는 require()을 사용할 수 없습니다. import는 정적이어야 하므로 빌드 시 분석할 수 있어야 합니다.

### 원격 이미지

원격 이미지를 사용하려면 src 속성은 URL 문자열이어야 합니다.

Next.js는 빌드 프로세스 중에 원격 파일에 액세스할 수 없으므로 너비, 높이 및 선택적인 blurDataURL props를 수동으로 제공해야 합니다.

<div class="content-ad"></div>

width 및 height 속성은 올바른 이미지 종횡비를 추측하고 이미지 로딩에서 레이아웃 이동을 방지하는 데 사용됩니다. width 및 height는 이미지 파일의 렌더링 크기를 결정하지 않습니다. 이미지 크기 조정에 대해 자세히 알아보세요.

```js
import Image from 'next/image'
 
export default function Page() {
  return (
    <Image
      src="https://s3.amazonaws.com/my-bucket/profile.png"
      alt="Picture of the author"
      width={500}
      height={500}
    />
  )
}
```

이미지를 안전하게 최적화하기 위해, next.config.js에서 지원되는 URL 패턴 목록을 정의하세요. 잘못된 사용을 방지하기 위해 가능한 구체적이세요. 예를 들어, 다음 구성은 특정 AWS S3 버킷의 이미지만 허용합니다:

```js
module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 's3.amazonaws.com',
        port: '',
        pathname: '/my-bucket/**',
      },
    ],
  },
}
```

<div class="content-ad"></div>

remotePatterns 구성에 대해 더 알아보세요. 이미지 src에 상대 URL을 사용하려면 로더를 사용하세요.

### 도메인

가끔 원격 이미지를 최적화할 수 있지만 내장 Next.js 이미지 최적화 API를 사용하길 원할 수 있습니다. 이를 위해 로더를 기본 설정으로 두고 Image src 속성에 절대 URL을 입력하세요.

악의적인 사용자로부터 응용 프로그램을 보호하려면 next/image 구성요소와 함께 사용할 원격 호스트 이름 목록을 정의해야 합니다.

<div class="content-ad"></div>

> remotePatterns 구성에 대해 더 알아보세요.

### 로더

이전 예시에서 볼 수 있듯이, 지역 이미지를 위해 부분 URL("/me.png")이 제공됩니다. 이는 로더 아키텍처 덕분에 가능합니다.

로더는 이미지 URL을 생성하는 함수입니다. 이는 제공된 src를 수정하고, 이미지를 다양한 크기로 요청할 수 있는 여러 URL을 생성합니다. 이러한 다양한 URL은 자동 srcset 생성에 사용되어 방문자들이 화면 크기에 맞는 이미지를 받아볼 수 있도록 합니다.

<div class="content-ad"></div>

Next.js 애플리케이션의 기본 로더는 내장된 이미지 최적화 API를 사용하여 웹 어디서나 이미지를 최적화한 다음 Next.js 웹 서버에서 직접 제공합니다. 이미지를 CDN이나 이미지 서버에서 직접 제공하려면 몇 줄의 JavaScript로 자체 로더 함수를 작성할 수 있습니다.

각 이미지에 로더 속성을 사용하여 이미지별로 로더를 정의하거나 loaderFile 구성으로 애플리케이션 수준에서 로더를 정의할 수 있습니다.

## 우선순위

각 페이지마다 Largest Contentful Paint (LCP) 요소가 될 이미지에 우선순위 속성을 추가해야 합니다. 이렇게 하면 Next.js가 이미지를로드하기 위해 특별히 우선순위를 부여할 수 있어서 (예: preload 태그 또는 우선 순위 힌트를 통해) LCP에 의미 있는 향상을 가져올 수 있습니다.

<div class="content-ad"></div>

LCP 요소는 일반적으로 페이지 뷰포트 내에서 가장 큰 이미지 또는 텍스트 블록입니다. 다음으로 개발 서버를 실행하면, LCP 요소가 `priority` 속성 없이 `Image`인 경우 콘솔 경고가 표시됩니다.

LCP 이미지를 식별한 후, 다음과 같이 속성을 추가할 수 있습니다:

```js
import Image from 'next/image'
import profilePic from '../public/me.png'
 
export default function Page() {
  return <Image src={profilePic} alt="Picture of the author" priority />
}
```

`next/image` 컴포넌트 문서에서 `priority`에 대해 더 자세히 알아보세요.

<div class="content-ad"></div>

## 이미지 사이즈 조정

이미지가 성능을 떨어뜨리는 가장 흔한 방법 중 하나는 이미지가 로드되는 동안 페이지에서 다른 요소들을 밀어내어 레이아웃을 변경시키는 레이아웃 이동으로 인한 것입니다. 이 성능 문제는 사용자들에게 너무나 거슬리는 문제로, 이에 대한 코어 웹 핵심요소인 Cumulative Layout Shift가 따로 존재합니다. 이미지 기반의 레이아웃 이동을 피하는 방법은 항상 이미지 크기를 지정하는 것입니다. 이를 통해 브라우저가 이미지를 로드하기 전에 정확한 공간을 예약할 수 있게 됩니다.

다음/이미지가 좋은 성능 결과를 보장하도록 설계되었기 때문에 레이아웃 이동에 기여하는 방식으로 사용할 수 없으며, 다음 중 하나의 방법으로 사이즈를 조정해야 합니다:

- 정적 임포트를 사용하여 자동으로
- 너비와 높이 속성을 포함하여 명확하게
- 부모 요소를 채우도록 하는 fill을 사용하여 암시적으로

<div class="content-ad"></div>

> 이미지의 크기를 모른다면 어떻게 해야 할까요?
이미지의 크기를 알 수 없는 소스에서 이미지에 액세스하는 경우, 다음과 같은 몇 가지 방법이 있습니다:
fill 속성 사용
fill 속성을 사용하면 이미지가 부모 요소의 크기에 맞게 조절됩니다. 이미지의 부모 요소에 페이지에서 공간을 확보하도록 CSS를 사용하고, sizes 속성을 사용하여 미디어 쿼리의 중단점에 맞추세요. 또한 object-fit 속성을 사용하여 fill, contain 또는 cover 및 object-position 속성을 사용하여 이미지가 그 공간을 어떻게 채우는지 정의할 수 있습니다.
이미지 정규화
제어 가능한 소스에서 이미지를 제공하는 경우 이미지 파이프 라인을 수정하여 이미지를 특정 크기로 정규화하는 것을 고려해보세요.
API 호출 수정
응용 프로그램이 API 호출을 사용하여 이미지 URL을 검색하는 경우 (예: CMS에), API 호출을 수정하여 이미지 차원을 URL과 함께 반환할 수도 있습니다.

제안된 방법 중 어느 것도 이미지 크기 조정에 작동하지 않는 경우, next/image 구성 요소는 페이지에서 표준 img 요소와 함께 잘 작동하도록 설계되었습니다.

## 스타일링

이미지 구성 요소를 스타일링하는 방법은 일반 img 요소를 스타일링하는 것과 유사하지만 몇 가지 지침을 염두에 두는 것이 좋습니다:

<div class="content-ad"></div>

- className이나 style을 사용하세요. styled-jsx 대신에 className prop을 사용하는 것을 추천합니다. 이것은 가져온 CSS 모듈이나 전역 스타일시트 등이 될 수 있습니다.
- inline 스타일을 할당하기 위해 style prop을 사용할 수도 있습니다.
- styled-jsx는 현재 컴포넌트에 스코프가 지정되어 있기 때문에 사용할 수 없습니다 (글로벌로 스타일을 지정하지 않는 한).
- fill을 사용할 때는 부모 요소가 position: relative여야 합니다. 이미지 요소를 그 레이아웃 모드에서 올바르게 렌더링하기 위해 필요합니다.
- fill을 사용할 때는 부모 요소가 display: block이어야 합니다. 이는 `div` 요소의 기본 설정이지만 다른 방식으로 지정해주어야 합니다.

## 예시

### Responsive

<img src="/assets/img/2024-07-23-ImageOptimization_0.png" />

<div class="content-ad"></div>

```js
import Image from 'next/image'
import mountains from '../public/mountains.jpg'

export default function Responsive() {
  return (
    <div style={ display: 'flex', flexDirection: 'column' }>
      <Image
        alt="Mountains"
        // 이미지를 가져오면 자동으로 너비와 높이를 설정합니다.
        src={mountains}
        sizes="100vw"
        // 이미지를 전체 너비로 표시합니다.
        style={
          width: '100%',
          height: 'auto',
        }
      />
    </div>
  )
}
```

### 컨테이너 채우기

<img src="/assets/img/2024-07-23-ImageOptimization_1.png" />

```js
import Image from 'next/image'
import mountains from '../public/mountains.jpg'

export default function Fill() {
  return (
    <div
      style={
        display: 'grid',
        gridGap: '8px',
        gridTemplateColumns: 'repeat(auto-fit, minmax(400px, auto))',
      }
    >
      <div style={ position: 'relative', height: '400px' }>
        <Image
          alt="Mountains"
          src={mountains}
          fill
          sizes="(min-width: 808px) 50vw, 100vw"
          style={
            objectFit: 'cover', // cover, contain, none
          }
        />
      </div>
      {/* 그리드 안에 더 많은 이미지가 있습니다... */}
    </div>
  )
}
```

<div class="content-ad"></div>

### 배경 이미지

![이미지](/assets/img/2024-07-23-ImageOptimization_2.png)

```js
import Image from 'next/image'
import mountains from '../public/mountains.jpg'

export default function Background() {
  return (
    <Image
      alt="Mountains"
      src={mountains}
      placeholder="blur"
      quality={100}
      fill
      sizes="100vw"
      style={{
        objectFit: 'cover',
      }}
    />
  )
}
```

이미지 컴포넌트를 다양한 스타일로 사용하는 예제는 [이미지 컴포넌트 데모](링크)를 참조하세요.

<div class="content-ad"></div>

## 기타 속성

다음/image 컴포넌트에서 사용 가능한 모든 속성을 확인해보세요.

## 구성

다음/image 컴포넌트와 Next.js 이미지 최적화 API는 next.config.js 파일에서 구성할 수 있습니다. 이러한 구성을 사용하면 원격 이미지를 활성화하거나 사용자 정의 이미지 브레이크포인트를 정의하거나 캐싱 동작을 변경하는 등의 작업을 수행할 수 있습니다.

<div class="content-ad"></div>

더 많은 정보를 얻으려면 이미지 구성 문서 전체를 읽어보세요.