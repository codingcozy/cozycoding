---
title: "2024년 Nextjs 14로 비디오 최적화하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:36
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Video Optimization"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/videos"
isUpdated: true
---




# 영상 최적화

이 페이지는 Next.js 애플리케이션에서 비디오를 사용하는 방법을 설명하며, 성능에 영향을 미치지 않고 비디오 파일을 저장하고 표시하는 방법을 보여줍니다.

## `video` 및 `iframe` 사용하기

비디오는 직접 비디오 파일에 대한 HTML `video` 태그와 외부 플랫폼 호스팅 비디오에 대한 `iframe`을 사용하여 페이지에 삽입할 수 있습니다.

<div class="content-ad"></div>

### `비디오`

HTML `video` 태그를 사용하면 자체 호스팅된 또는 직접 제공되는 비디오 콘텐츠를 임베드하여 재생 및 외관을 완전히 제어할 수 있습니다.

```js
export function Video() {
  return (
    <video width="320" height="240" controls preload="none">
      <source src="/path/to/video.mp4" type="video/mp4" />
      <track
        src="/path/to/captions.vtt"
        kind="subtitles"
        srcLang="en"
        label="English"
      />
      브라우저가 비디오 태그를 지원하지 않습니다.
    </video>
  )
}
```

### 일반적인 `video` 태그 속성

<div class="content-ad"></div>


| Attribute    | Description                                                     | Example Value                  |
|--------------|-----------------------------------------------------------------|--------------------------------|
| `src`        | 비디오 파일의 원본을 지정합니다.                                | `<video src="/path/to/video.mp4" />` |
| `width`      | 비디오 플레이어의 너비를 설정합니다.                             | `<video width="320" />`        |
| `height`     | 비디오 플레이어의 높이를 설정합니다.                             | `<video height="240" />`       |
| `controls`   | 존재하는 경우 기본 재생 컨트롤 세트를 표시합니다.              | `<video controls />`           |
| `autoPlay`   | 페이지 로드시 비디오를 자동으로 재생합니다. 주의: 자동 재생 규정은 브라우저마다 다릅니다. | `<video autoPlay />`           |
| `loop`       | 비디오 재생을 반복합니다.                                        | `<video loop />`               |
| `muted`      | 기본적으로 오디오를 음소거합니다. 자주 `autoPlay`와 함께 사용됩니다. | `<video muted />`              |
| `preload`    | 비디오의 사전로드 방식을 지정합니다. 값: `none`, `metadata`, `auto`. | `<video preload="none" />`     |
| `playsInline`| iOS 기기에서 인라인 재생을 활성화합니다. iOS Safari에서 자동 재생이 작동하기 위해 종종 필요합니다. | `<video playsInline />`        |


> 알아두면 좋은 팁: `autoPlay` 속성을 사용할 때 대부분의 브라우저에서 비디오가 자동으로 재생되도록 보장하기 위해 `muted` 속성을 함께 포함하는 것이 중요하며 iOS 기기와 호환성을 위해 `playsInline` 속성이 필요합니다.

비디오 속성에 대한 포괄적인 목록은 MDN 문서를 참조하세요.

### 비디오 사용 가이드


<div class="content-ad"></div>

- 대안 콘텐츠: `video` 태그를 사용할 때, 비디오 재생을 지원하지 않는 브라우저를 위해 태그 내에 대체 콘텐츠를 포함하세요.
- 자막 또는 캡션: 청각 장애가 있는 사용자를 위해 자막 또는 캡션을 포함하세요. `video` 요소에 `track` 태그를 활용하여 캡션 파일 소스를 지정하세요.
- 접근성 있는 컨트롤: 키보드 탐색 및 스크린 리더 호환성을 위해 표준 HTML5 비디오 컨트롤을 권장합니다. 고급 기능이 필요한 경우, react-player나 video.js와 같은 서드파티 플레이어를 고려해보세요. 이들은 접근성 있는 컨트롤과 일관된 브라우저 경험을 제공합니다.

### `iframe`

HTML `iframe` 태그를 사용하면 YouTube나 Vimeo와 같은 외부 플랫폼에서 비디오를 임베드할 수 있습니다.

```js
export default function Page() {
  return (
    <iframe
      src="https://www.youtube.com/watch?v=gfU1iZnjRZM"
      frameborder="0"
      allowfullscreen
    />
  )
}
```

<div class="content-ad"></div>

### 공통 `iframe` 태그 속성

| 속성 | 설명 | 예시 값 |
|---|---|---|
| `src` | 삽입할 페이지의 URL입니다. | `<iframe src="https://example.com" />` |
| `width` | iframe의 너비를 설정합니다. | `<iframe width="500" />` |
| `height` | iframe의 높이를 설정합니다. | `<iframe height="300" />` |
| `frameborder` | iframe 주변에 테두리를 표시할지 여부를 지정합니다. | `<iframe frameborder="0" />` |
| `allowfullscreen` | iframe 콘텐츠를 전체 화면 모드로 표시할 수 있도록 합니다. | `<iframe allowfullscreen />` |
| `sandbox` | iframe 내부 콘텐츠에 추가 제한을 활성화합니다. | `<iframe sandbox />` |
| `loading` | 로딩 동작을 최적화합니다(예: 지연 로딩). | `<iframe loading="lazy" />` |
| `title` | 접근성을 지원하기 위해 iframe에 제목을 제공합니다. | `<iframe title="설명" />` |

iframe 속성에 대한 상세한 목록은 MDN 문서를 참조해 주세요.

### 동영상 삽입 방법 선택하기

<div class="content-ad"></div>

Next.js 애플리케이션에서 비디오를 삽입하는 두 가지 방법이 있습니다:

- 직접 호스트하거나 직접 비디오 파일: `video` 태그를 사용하여 자체 호스트된 비디오를 삽입하면 플레이어의 기능과 외관에 대한 자세한 제어가 필요한 시나리오에 적합합니다. Next.js 내에서 이 통합 방법을 사용하면 비디오 콘텐츠를 사용자 정의하고 제어할 수 있습니다.
- 비디오 호스팅 서비스 사용(YouTube, Vimeo 등): YouTube 또는 Vimeo와 같은 비디오 호스팅 서비스를 사용하는 경우 `iframe` 태그를 사용하여 그들의 iframe 기반 플레이어를 삽입할 수 있습니다. 이 방법은 플레이어에 대한 일부 제어를 제한하지만 이러한 플랫폼이 제공하는 사용 편의성과 기능을 제공합니다.

귀하의 응용 프로그램 요구 사항과 전달하려는 사용자 경험과 일치하는 삽입 방법을 선택하십시오.

### 외부 호스트된 비디오 삽입

<div class="content-ad"></div>

외부 플랫폼에서 비디오를 임베드하기 위해, Next.js를 사용하여 비디오 정보를 가져오고 로딩 중에 대체 상태를 처리하기 위해 React Suspense를 사용할 수 있습니다.

1. 비디오 임베딩을 위한 서버 컴포넌트 생성

첫 번째 단계는 서버 컴포넌트를 생성하는 것입니다.
비디오를 임베드하기 위한 적절한 iframe을 생성하는 이 컴포넌트는 비디오의 소스 URL을 가져와서 iframe을 렌더링합니다.

```js
export default async function VideoComponent() {
  const src = await getVideoSrc()
 
  return <iframe src={src} frameborder="0" allowfullscreen />
}
```

<div class="content-ad"></div>

2. React Suspense를 사용하여 비디오 컴포넌트 스트리밍하기

비디오를 임베드할 Server Component를 만든 후 다음 단계는 React Suspense를 사용하여 컴포넌트를 스트리밍하는 것입니다.

```js
import { Suspense } from 'react'
import VideoComponent from '../ui/VideoComponent.jsx'

export default function Page() {
  return (
    <section>
      <Suspense fallback={<p>Loading video...</p>}>
        <VideoComponent />
      </Suspense>
      {/* 페이지의 다른 컨텐츠 */}
    </section>
  )
}
```

> 알아두면 좋은 점: 외부 플랫폼에서 비디오를 임베드할 때 다음과 같은 모베스트 프랙티스를 고려하세요:
비디오 임베드가 반응형이 되도록합니다. CSS를 사용하여 iframe이나 비디오 플레이어가 다양한 화면 크기에 적응하도록 합니다.
특히 데이터 사용량이 제한된 사용자를 위해 네트워크 상황에 따라 비디오를 로딩하는 전략을 구현합니다.

<div class="content-ad"></div>

이 접근 방식은 페이지가 차단되는 것을 방지하여 더 나은 사용자 경험을 제공합니다. 이는 사용자가 비디오 구성요소가 스트리밍되는 동안 페이지와 상호 작용할 수 있음을 의미합니다.

더 매력적이고 유익한 로딩 경험을 위해, 대체 UI로 로딩 스켈레톤을 사용하는 것을 고려해보세요. 따라서 단순한 로딩 메시지를 표시하는 대신, 비디오 플레이어와 유사한 외형을 가진 스켈레톤을 보여줄 수 있습니다.

```js
import { Suspense } from 'react'
import VideoComponent from '../ui/VideoComponent.jsx'
import VideoSkeleton from '../ui/VideoSkeleton.jsx'

export default function Page() {
  return (
    <section>
      <Suspense fallback={<VideoSkeleton />}>
        <VideoComponent />
      </Suspense>
      {/* 페이지의 다른 내용 */}
    </section>
  )
}
```

## 자체 호스팅된 비디오

<div class="content-ad"></div>

비디오를 자체 호스팅하는 것은 여러 가지 이유로 선호될 수 있습니다:

- 완전한 제어와 독립성: 자체 호스팅을 통해 비디오 콘텐츠에 대한 직접적인 관리가 가능하며, 재생부터 외관까지 완전한 소유 및 제어를 보장하여 외부 플랫폼 제약에서 자유롭게 할 수 있습니다.
- 특정 요구 사항에 대한 사용자 정의: 동적 배경 비디오와 같은 고유 요구 사항에 적합하며, 디자인과 기능적 요구에 부합하도록 맞춤형 사용자 정의가 가능합니다.
- 성능 및 확장성 고려 사항: 점점 증가하는 트래픽 및 콘텐츠 크기를 효과적으로 지원하기 위해 고성능 및 확장 가능한 저장 솔루션을 선택하세요.
- 비용 및 통합: 저장 및 대역폭 비용을 Next.js 프레임워크 및 더 넓은 기술 생태계에 쉽게 통합할 필요성과 균형을 맞추세요.

### 비디오 호스팅에 Vercel Blob 사용하기

Vercel Blob은 비디오를 호스팅하는 효율적인 방법을 제공하며, Next.js와 잘 작동하는 확장 가능한 클라우드 저장 솔루션을 제공합니다. Vercel Blob를 사용하여 비디오를 호스팅하는 방법은 다음과 같습니다:

<div class="content-ad"></div>

1. Vercel Blob에 비디오를 업로드하기

Vercel 대시보드에서 "Storage" 탭으로 이동하고 Vercel Blob 저장소를 선택합니다. Blob 테이블의 오른쪽 상단에 "업로드" 버튼을 찾아 클릭합니다. 그런 다음, 업로드하려는 비디오 파일을 선택합니다. 업로드가 완료되면 비디오 파일이 Blob 테이블에 나타납니다.

또는 서버 액션을 사용하여 비디오를 업로드할 수도 있습니다. 자세한 지침은 서버 측 업로드에 대한 Vercel 문서를 참조하십시오. Vercel은 클라이언트 측 업로드도 지원합니다. 이 방법은 특정 사용 사례에 적합할 수 있습니다.

2. Next.js에서 비디오 표시하기

<div class="content-ad"></div>

영상을 업로드하고 저장한 후에는 Next.js 애플리케이션에서 해당 영상을 표시할 수 있습니다. 아래는 `video` 태그와 React Suspense를 사용하여 이를 수행하는 예시입니다:

```js
import { Suspense } from 'react'
import { list } from '@vercel/blob'

export default function Page() {
  return (
    <Suspense fallback={<p>Loading video...</p>}>
      <VideoComponent fileName="my-video.mp4" />
    </Suspense>
  )
}

async function VideoComponent({ fileName }) {
  const { blobs } = await list({
    prefix: fileName,
    limit: 1,
  })
  const { url } = blobs[0]

  return (
    <video controls preload="none" aria-label="Video player">
      <source src={url} type="video/mp4" />
      Your browser does not support the video tag.
    </video>
  )
}
```

이 방법을 사용하면 페이지는 영상의 @vercel/blob URL을 활용하여 VideoComponent를 통해 영상을 표시합니다. React Suspense를 사용하여 영상 URL을 가져오고 영상을 표시할 준비가 될 때까지 fallback을 보여줍니다.

### 영상에 자막 추가하기

<div class="content-ad"></div>

영상에 자막이 있는 경우, `video` 태그 내부에 `track` 요소를 사용하여 쉽게 추가할 수 있습니다. 영상 파일과 비슷한 방식으로 Vercel Blob에서 자막 파일을 가져올 수 있습니다. 다음은 `VideoComponent`를 업데이트하여 자막을 포함하는 방법입니다.

```js
async function VideoComponent({ fileName }) {
  const { blobs } = await list({
    prefix: fileName,
    limit: 2
  });
  const { url } = blobs[0];
  const { url: captionsUrl } = blobs[1];
 
  return (
    <video controls preload="none" aria-label="Video player">
      <source src={url} type="video/mp4" />
      <track
        src={captionsUrl}
        kind="subtitles"
        srcLang="en"
        label="English">
      Your browser does not support the video tag.
    </video>
  );
};
```

이 접근 방식을 따르면 영상을 효과적으로 자체 호스팅하여 Next.js 애플리케이션에 통합할 수 있습니다.

## 자료 출처

<div class="content-ad"></div>

비디오 최적화와 관련된 더 많은 정보와 모범 사례를 학습하려면 다음 자료들을 참고해 주세요:

- 비디오 형식 및 코덱 이해: MP4와 WebM과 같은 올바른 형식 및 코덱을 선택하여 비디오 요구 사항에 맞춰 사용하세요. 자세한 내용은 Mozilla의 비디오 코덱 안내서를 참조하세요.

- 비디오 압축: FFmpeg와 같은 도구를 사용하여 효율적으로 비디오를 압축하고 품질과 파일 크기 사이의 균형을 맞추세요. 압축 기술에 대해 자세히 알아보려면 FFmpeg의 공식 웹사이트를 방문하세요.

- 해상도와 비트율 조정: 모바일 기기에는 낮은 설정을 사용하여 시청 플랫폼에 맞게 해상도와 비트율을 조정하세요.

- 콘텐츠 전달 네트워크(CDN): CDN을 활용하여 비디오 전송 속도를 향상시키고 고트래픽을 관리하세요. Vercel Blob와 같은 일부 저장 솔루션을 사용할 때는 CDN 기능이 자동으로 처리됩니다. CDN 및 그 이점에 대해 자세히 알아보세요.

Next.js 프로젝트에 비디오를 통합하기 위해 다음 비디오 스트리밍 플랫폼을 탐색해보세요:

### 오픈 소스 다음-비디오 컴포넌트

<div class="content-ad"></div>

- 다양한 호스팅 서비스인 Vercel Blob, S3, Backblaze 및 Mux와 호환되는 Next.js용 `Video` 컴포넌트를 제공합니다.
- 서로 다른 호스팅 서비스와 함께 next-video.dev를 사용하는 방법에 대한 상세한 문서가 제공됩니다.

### Cloudinary 통합

- Cloudinary를 Next.js와 함께 사용하는 공식 문서 및 통합 가이드가 제공됩니다.
- 드롭인 비디오 지원을 위한 `CldVideoPlayer` 컴포넌트가 포함되어 있습니다.
- Cloudinary를 Next.js와 통합하는 예제 및 적응형 비트레이트 스트리밍이 있습니다.
- 다른 Cloudinary 라이브러리 중에는 Node.js SDK도 있습니다.

### Mux Video API

<div class="content-ad"></div>

- Mux는 Mux와 Next.js를 사용하여 비디오 코스를 만드는 데 사용할 수 있는 스타터 템플릿을 제공합니다.
- Next.js 애플리케이션에 고성능 비디오를 임베드하는 Mux의 권장 사항에 대해 알아보세요.
- Mux와 Next.js를 활용한 예제 프로젝트를 살펴보세요.

### Fastly

- Next.js에 Video on Demand 및 스트리밍 미디어를 위한 Fastly 솔루션을 통합하는 방법에 대해 알아보세요.