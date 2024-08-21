---
title: "프론트엔드 성능 최적화 2023 최신 기법과 베스트 프랙티스"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-23 21:13
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Optimizing Front-End Performance Techniques and Best Practices"
link: "https://medium.com/@kasata/optimizing-front-end-performance-techniques-and-best-practices-2e56b8137200"
isUpdated: true
---




현재의 빠르게 변화하는 디지털 세계에서는 프론트엔드 성능을 최적화하여 사용자 경험을 향상시키고 더 빠른 로드 시간을 보장하는 것이 중요해졌습니다. 최적화되지 않은 웹사이트는 이탈률이 높아지고 전환율이 낮아질 수 있습니다. 본 문서에서는 프론트엔드 성능을 향상시키고 원활한 사용자 경험을 제공하기 위한 다양한 기술과 모범 사례에 대해 다루고 있습니다.

### 1. HTTP 요청 최소화

웹페이지의 각 요소(이미지, 스크립트, 스타일시트 등)마다 HTTP 요청이 이루어집니다. 이러한 요청 수를 줄이는 것은 로드 시간을 크게 단축할 수 있습니다. 파일을 결합하거나 CSS 스프라이트를 사용하고 데이터 URL을 활용하여 이미지를 포함시키는 등 요청 수를 줄이는 방법을 고려해보세요.

### 2. 이미지 최적화

<div class="content-ad"></div>

웹 페이지에서 이미지는 종종 가장 큰 파일이며 최적화할 수 있는 가장 좋은 후보입니다. 웹P와 AVIF와 같은 현대 이미지 형식을 사용하여 품질을 저하시키지 않으면서 파일 크기를 줄이세요. `picture` 요소와 srcset 속성을 사용하여 다양한 기기에 맞는 이미지 크기를 제공하여 반응형 이미지를 활용해보세요.

## 3. 콘텐츠 전달 네트워크(CDN) 사용

CDN은 전 세계에 위치한 서버에 웹사이트 사본을 저장합니다. 사용자에게 가장 가까운 서버에서 콘텐츠를 제공함으로써 CDN은 대역폭을 크게 줄이고 로드 시간을 개선할 수 있습니다. 인기있는 CDN 제공업체로는 Cloudflare, Akamai, Amazon CloudFront 등이 있습니다.

## 4. 지연 로딩 구현

<div class="content-ad"></div>

Lazy loading은 비핵심 자원을 필요할 때까지 늦추는 것을 말합니다. 예를 들어, 아래쪽으로 스크롤해야 보이는 이미지는 초기 로드 시간을 개선하기 위해 lazy loading을 적용할 수 있습니다. Lazy loading을 구현하면 페이지의 초기 로드 크기가 크게 축소되고 성능이 향상될 수 있습니다.

## 5. 최소화 및 압축

최소화는 코드에서 공백 및 주석과 같은 불필요한 문자를 제거하고 기능에는 영향을 미치지 않도록하는 작업을 말합니다. UglifyJS 및 CSSNano와 같은 도구를 사용하여 JavaScript 및 CSS 파일을 최소화할 수 있습니다. 또한 서버에서 HTML, CSS 및 JavaScript 파일의 크기를 줄이기 위해 GZIP 또는 Brotli 압축을 활성화할 수 있습니다.

## 6. CSS와 JavaScript 최적화

<div class="content-ad"></div>

CSS와 JavaScript 파일을 최대한 효율적으로 유지하세요. 사용되지 않는 CSS를 제거하고, 본문 내용이 로드된 후에 비핵심 JavaScript를 지연시키는 것을 고려해 보세요. PurifyCSS와 TreeShaking과 같은 도구를 사용하여 사용되지 않는 코드를 식별하고 제거하는 데 도움을 줄 수 있습니다.

## 7. 브라우저 캐싱 활용

브라우저 캐싱은 이미지, CSS, JavaScript와 같은 정적 파일을 사용자의 기기에 저장하여 사용자가 사이트를 처음 방문할 때 발생합니다. 사이트를 다시 방문할 때 브라우저가 서버에서 파일을 다시 요청하는 대신 로컬 저장소에서 파일을 로드하므로 사이트 방문이 더 빨라질 수 있습니다. 적절한 캐싱 헤더를 구성하여 브라우저 캐싱을 효율적으로 활용하세요.

## 결론

<div class="content-ad"></div>

프런트엔드 성능 최적화는 계속되는 프로세스로 다양한 기술과 모범 사례가 필요합니다. HTTP 요청을 최소화하고, 이미지를 최적화하며, CDN을 사용하고, 레이지 로딩을 구현하며, 파일을 최소화하고 압축하며, CSS와 JavaScript를 최적화하고, 브라우저 캐싱을 활용함으로써 웹사이트의 성능을 크게 향상시킬 수 있습니다. 빠르고 반응성 좋은 웹사이트는 사용자 경험을 향상시키는 데 도움이 되며, SEO 순위와 전환율을 높일 수 있습니다.