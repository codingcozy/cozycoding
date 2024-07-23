---
title: "Nextjs 14 App Router의 기초 이해하기"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:03
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Routing Fundamentals"
link: "https://nextjs.org/docs/app/building-your-application/routing"
---


# 라우팅 기초

모든 응용 프로그램의 뼈대는 라우팅입니다. 이 페이지에서는 웹의 라우팅의 기본 개념과 Next.js에서 라우팅을 다루는 방법에 대해 소개하겠습니다.

## 용어

먼저 문서 전반에 사용되는 다음 용어들을 볼 수 있습니다. 여기에 간단한 참고자료가 있습니다:

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-23-RoutingFundamentals_0.png)

- 트리(Tree): 계층 구조를 시각적으로 표시하는 규칙입니다. 예를 들어, 부모 및 자식 컴포넌트가 있는 컴포넌트 트리, 폴더 구조 등.
- 서브트리(Subtree): 새 루트(첫 번째)에서 시작하여 잎(마지막)까지의 트리 일부입니다.
- 루트(Root): 트리나 서브트리에서 첫 번째 노드로, 루트 레이아웃과 같은 것입니다.
- 잎(Leaf): 자식이 없는 서브트리의 노드로, URL 경로의 마지막 세그먼트와 같습니다.

![이미지](/assets/img/2024-07-23-RoutingFundamentals_1.png)

- URL 세그먼트(URL Segment): 슬래시로 구분된 URL 경로의 일부입니다.
- URL 경로(URL Path): 도메인 이후의 URL 일부로, 세그먼트로 구성됩니다.

<div class="content-ad"></div>

## 앱 라우터

Next.js의 13버전에서 새로운 앱 라우터가 소개되었는데, React Server Components를 기반으로 하며 공유 레이아웃, 중첩 라우팅, 로딩 상태, 오류 처리 등을 지원합니다.

앱 라우터는 새로운 app 디렉터리에서 작동합니다. 앱 디렉터리는 페이지 디렉터리와 함께 작동하여 점진적 적용이 가능합니다. 이를 통해 애플리케이션의 일부 라우트를 새로운 동작으로 전환하면서 이전 동작을 유지할 수 있습니다. 만약 애플리케이션이 페이지 디렉터리를 사용한다면, 페이지 라우터 문서도 참고해 주세요.

> 알아두면 좋은 점: 앱 라우터는 페이지 라우터보다 우선순위를 가집니다. 디렉터리 간 라우트는 동일한 URL 경로로 해석되어서는 안 되며, 충돌을 방지하기 위해 런타임 오류가 발생할 것입니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-23-RoutingFundamentals_2.png)

기본적으로 앱 내의 컴포넌트는 React Server Components입니다. 이는 성능 최적화로, 쉽게 채용할 수 있으며 Client Components를 사용할 수도 있습니다.

> 권장 사항: Server Components에 익숙하지 않은 경우 Server 페이지를 확인해보세요.

## 폴더 및 파일의 역할


<div class="content-ad"></div>

Next.js는 파일 시스템 기반 라우터를 사용합니다:

- 폴더를 사용하여 경로를 정의합니다. 경로는 최종 leaf 폴더가 있는 파일 시스템 계층 구조의 중첩 폴더 경로입니다. 해당 폴더에는 page.js 파일이 포함됩니다. 라우트 정의를 참조하세요.
- 파일은 라우트 세그먼트에 대해 표시되는 UI를 작성하는 데 사용됩니다. 특수 파일을 참조하세요.

## 라우트 세그먼트

라우트 내의 각 폴더는 라우트 세그먼트를 나타냅니다. 각 라우트 세그먼트는 URL 경로의 해당 세그먼트에 매핑됩니다.

<div class="content-ad"></div>


아래는 Markdown 형식으로 수정된 텍스트입니다.

![이미지](/assets/img/2024-07-23-RoutingFundamentals_3.png)

## 중첩된 라우트

중첩된 라우트를 만들려면 서로 다른 폴더를 중첩시킬 수 있습니다. 예를 들어, 앱 디렉토리에 두 개의 새 폴더를 중첩하여 새로운 /dashboard/settings 루트를 추가할 수 있습니다.

/dashboard/settings 루트는 세 개의 세그먼트로 구성됩니다:


<div class="content-ad"></div>

- / (Root segment)
- dashboard (Segment)
- settings (Leaf segment)

## 파일 규칙

Next.js는 중첩된 경로에서 특정 동작을 가진 UI를 생성하기 위한 특별한 파일 세트를 제공합니다:

| 파일 | 설명 |
| --- | --- |
| [layout](/docs/app/building-your-application/routing/pages-and-layouts#layouts) | 세그먼트 및 해당 하위 요소들에 대한 공유 UI |
| [page](/docs/app/building-your-application/routing/pages-and-layouts#pages) | 경로의 고유한 UI 및 경로를 공개적으로 접근 가능하게 함 |
| [loading](/docs/app/building-your-application/routing/loading-ui-and-streaming) | 세그먼트 및 해당 하위 요소들에 대한 로딩 UI |
| [not-found](/docs/app/api-reference/file-conventions/not-found) | 세그먼트 및 해당 하위 요소들에 대한 찾을 수 없음 UI |
| [error](/docs/app/building-your-application/routing/error-handling) | 세그먼트 및 해당 하위 요소들에 대한 오류 UI |
| [global-error](/docs/app/building-your-application/routing/error-handling) | 전역 오류 UI |
| [route](/docs/app/building-your-application/routing/route-handlers) | 서버 측 API 엔드포인트 |
| [template](/docs/app/building-your-application/routing/pages-and-layouts#templates) | 특수화된 다시 렌더링된 레이아웃 UI |
| [default](/docs/app/api-reference/file-conventions/default) | [병렬 라우트](/docs/app/building-your-application/routing/parallel-routes)를 위한 대체 UI |

<div class="content-ad"></div>

> 유용한 정보: .js, .jsx 또는 .tsx 파일 확장자는 특별한 파일에 사용할 수 있습니다.

## 컴포넌트 계층 구조

경로 세그먼트의 특별한 파일에 정의된 React 컴포넌트는 다음과 같은 특정한 계층 구조로 렌더링됩니다:

- layout.js
- template.js
- error.js (React 에러 경계)
- loading.js (React suspense 경계)
- not-found.js (React 에러 경계)
- page.js 또는 중첩 레이아웃.js

<div class="content-ad"></div>


![routingfundamentals](/assets/img/2024-07-23-RoutingFundamentals_4.png)

In a nested route, the components of a segment will be nested inside the components of its parent segment.

![routingfundamentals](/assets/img/2024-07-23-RoutingFundamentals_5.png)

## Colocation


<div class="content-ad"></div>

특별 파일 외에도 앱 디렉토리 안의 폴더에 자체 파일(예: 컴포넌트, 스타일, 테스트 등)을 함께 둘 수 있습니다.

폴더는 경로를 정의하지만 page.js 또는 route.js에서 반환된 내용만 공개적으로 접근할 수 있습니다.

![Routing Fundamentals](/assets/img/2024-07-23-RoutingFundamentals_6.png)

프로젝트 조직 및 동일 위치에 관해 더 알아보세요.

<div class="content-ad"></div>

## 고급 라우팅 패턴

App Router는 더 고급 라우팅 패턴을 구현하는 데 도움이 되는 규칙 세트를 제공합니다. 이러한 패턴에는 아래와 같은 것들이 포함됩니다:

- 병렬 루트: 하나의 보기에서 두 개 이상의 페이지를 동시에 표시하도록 허용하여 독립적으로 탐색할 수 있습니다. 이를 사용하여 각각의 하위 탐색이 있는 분할 뷰(대시보드와 같은 것)를 만들 수 있습니다.
- 루트 가로채기: 루트를 가로채어 다른 루트의 컨텍스트에서 표시할 수 있습니다. 현재 페이지의 컨텍스트를 유지하는 것이 중요할 때 사용할 수 있습니다. 예를 들어, 한 작업을 편집하면서 모든 작업을 볼 수 있거나 피드에서 사진을 확대하는 경우 등이 있습니다.

이러한 패턴을 사용하면 더 풍부하고 복잡한 UI를 구축할 수 있으며, 소규모 팀이나 개발자들이 구현하는 데 역사적으로 복잡한 기능을 보편화할 수 있습니다.

<div class="content-ad"></div>

## 다음 단계

지금까지 Next.js에서 라우팅의 기본 원리를 이해했으니, 아래 링크를 따라 첫 번째 경로를 만들어 보세요: