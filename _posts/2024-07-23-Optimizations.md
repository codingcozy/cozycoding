---
title: "Nextjs 14 성능 최적화를 위한 팁과 트릭 5가지"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:34
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Optimizations"
link: "https://nextjs.org/docs/app/building-your-application/optimizing"
isUpdated: true
---




# 최적화

Next.js에는 응용 프로그램의 속도 및 핵심 웹 척도를 향상시키기 위해 설계된 다양한 내장 최적화 기능이 제공됩니다. 이 안내서는 사용자 경험을 향상시키는 데 활용할 수 있는 최적화에 대해 다룰 것입니다.

## 내장 구성 요소

내장 구성 요소는 일반 UI 최적화를 구현하는 복잡성을 추상화합니다. 이러한 구성 요소는 다음과 같습니다:

<div class="content-ad"></div>

- 이미지: 기본 `img` 요소를 기반으로 제작되었습니다. 이미지 구성 요소는 이미지를 지연로딩하여 성능을 최적화하며, 기기 크기에 따라 이미지를 자동으로 조정합니다.
- 링크: 기본 `a` 태그를 기반으로 제작되었습니다. 링크 구성 요소는 페이지를 미리 가져와 빠르고 부드러운 페이지 전환이 가능하도록 합니다.
- 스크립트: 기본 `script` 태그를 기반으로 제작되었습니다. 스크립트 구성 요소는 제3자 스크립트의 로딩과 실행을 제어할 수 있습니다.

## 메타데이터

메타데이터는 검색 엔진이 콘텐츠를 더 잘 이해하도록 도와주어 SEO를 향상시킬 수 있으며, 소셜 미디어에서 콘텐츠가 어떻게 표시되는지를 사용자 정의할 수 있어 여러 플랫폼에서 더 매끄럽고 일관된 사용자 경험을 만들 수 있습니다.

Next.js의 메타데이터 API를 사용하면 페이지의 `head` 요소를 수정할 수 있습니다. 메타데이터는 두 가지 방법으로 구성할 수 있습니다.

<div class="content-ad"></div>

- Config-based Metadata: 레이아웃.js 또는 페이지.js 파일에 정적 메타데이터 객체 또는 동적 generateMetadata 함수를 내보냅니다.
- File-based Metadata: 라우트 세그먼트에 정적 또는 동적으로 생성된 특수 파일을 추가합니다.

또한, imageResponse 생성자를 사용하여 JSX 및 CSS로 동적 오픈 그래프 이미지를 생성할 수 있습니다.

## 정적 에셋

Next.js의 /public 폴더를 사용하여 이미지, 폰트 및 기타 파일과 같은 정적 에셋을 제공할 수 있습니다. /public 내부의 파일은 CDN 제공업체에 의해 캐시될 수 있어 효율적으로 전달될 수 있습니다.

<div class="content-ad"></div>

## 분석 및 모니터링

대규모 애플리케이션의 경우 Next.js는 인기 있는 분석 및 모니터링 도구와 통합하여 애플리케이션이 어떻게 수행되고 있는지 이해하는 데 도움을 줍니다. OpenTelemetry 및 Instrumentation 가이드에서 자세히 알아보세요.