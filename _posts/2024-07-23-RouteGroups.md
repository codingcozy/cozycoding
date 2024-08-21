---
title: "Nextjs 14에서 Route Groups 사용하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:09
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Route Groups"
link: "https://nextjs.org/docs/app/building-your-application/routing/route-groups"
isUpdated: true
---




# 라우트 그룹

앱 디렉토리에서 중첩된 폴더는 일반적으로 URL 경로에 매핑됩니다. 그러나 라우트 그룹으로 폴더를 표시하여 해당 폴더가 라우트의 URL 경로에 포함되지 않도록 할 수 있습니다.

이를 사용하면 라우트 세그먼트와 프로젝트 파일을 URL 경로 구조에 영향을 주지 않고 논리적 그룹으로 구성할 수 있습니다.

라우트 그룹은 다음과 같은 경우에 유용합니다:

<div class="content-ad"></div>

- 루트를 사이트 섹션, 취지 또는 팀별로 그룹화합니다.
- 동일한 루트 세그먼트 레벨에서 중첩 레이아웃을 활성화합니다:
동일한 세그먼트에 여러 중첩 레이아웃 생성, 여러 루트 레이아웃 포함
일반 세그먼트의 일부 루트에 레이아웃 추가
- 동일한 세그먼트에서 여러 중첩 레이아웃을 생성합니다. 여러 루트 레이아웃을 포함합니다.
- 일반 세그먼트의 일부 루트에 레이아웃 추가합니다.

## 컨벤션

폴더명을 괄호로 묶어 루트 그룹을 만들 수 있습니다: (폴더명)

## 예제

<div class="content-ad"></div>

### URL 경로에 영향을 주지 않고 라우트 구성하기

관련 있는 라우트들을 함께 모아두기 위해 그룹을 만들어 URL에 영향을 주지 않고 라우트를 구성할 수 있습니다. 괄호 안에 있는 폴더들은 URL에서 제외됩니다 (예: (marketing) 또는 (shop)).

![라우트 그룹](/assets/img/2024-07-23-RouteGroups_0.png)

(marketing) 및 (shop) 안의 라우트들은 동일한 URL 계층을 공유하더라도, 각 그룹에 해당하는 레이아웃을 layout.js 파일을 각 폴더 안에 추가하여 만들 수 있습니다.

<div class="content-ad"></div>

![RouteGroups_1](/assets/img/2024-07-23-RouteGroups_1.png)

### 레이아웃에 특정 세그먼트 선택하기

특정 경로를 레이아웃에 선택하려면 새 경로 그룹(예: (shop))을 만들고 동일한 레이아웃을 공유하는 경로를 해당 그룹으로 이동하십시오 (예: account 및 cart). 그룹 밖의 경로는 해당 레이아웃을 공유하지 않습니다 (예: checkout).

![RouteGroups_2](/assets/img/2024-07-23-RouteGroups_2.png)

<div class="content-ad"></div>

### 여러 루트 레이아웃 만들기

여러 루트 레이아웃을 만들려면 최상위 레이아웃.js 파일을 제거하고 각 라우트 그룹 내에 레이아웃.js 파일을 추가하면 됩니다. 이는 응용 프로그램을 완전히 다른 UI나 경험을 갖는 섹션으로 분할하는 데 유용합니다. `html` 및 `body` 태그를 각 루트 레이아웃에 추가해야 합니다.

<img src="/assets/img/2024-07-23-RouteGroups_3.png" />

위 예시에서 (마케팅)과 (상점)은 각각 고유한 루트 레이아웃을 갖습니다.

<div class="content-ad"></div>

> 알아두면 좋아요:
라우트 그룹의 이름 짓기는 조직화를 위한 것 이외에는 별다른 의미가 없습니다. URL 경로에 영향을 미치지 않습니다.
라우트 그룹을 포함하는 라우트는 다른 라우트와 같은 URL 경로로 해결되어서는 안 됩니다. 예를 들어, 라우트 그룹이 URL 구조에 영향을 주지 않기 때문에 (마케팅)/about/page.js와 (쇼핑)/about/page.js는 둘 다 /about로 해석되어 에러를 발생시킵니다.
최상위 레이아웃 파일인 layout.js 없이 여러 루트 레이아웃을 사용하는 경우, 홈 페이지.js 파일은 라우트 그룹 중 하나에 정의되어야 합니다. 예를 들어: app/(마케팅)/page.js.
여러 루트 레이아웃을 거쳐 이동하면 전체 페이지가 다시로드됩니다(클라이언트 측 탐색과는 대조적). 예를 들어, app/(쇼핑)/layout.js를 사용하는 /cart에서 app/(마케팅)/layout.js를 사용하는 /blog로 이동하면 전체 페이지가 다시로드됩니다. 이는 여러 루트 레이아웃에만 적용됩니다.