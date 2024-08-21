---
title: "Nextjs 14 앱 라우터에서 경로 정의하는 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:04
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Defining Routes"
link: "https://nextjs.org/docs/app/building-your-application/routing/defining-routes"
isUpdated: true
---




# 루트 정의하기

> 계속하기 전에 라우팅 기본 사항 페이지를 읽어보는 것을 권장합니다.

이 페이지에서는 Next.js 애플리케이션에 루트를 정의하고 구성하는 방법을 안내합니다.

## 루트 생성하기

<div class="content-ad"></div>

Next.js는 폴더를 사용하여 route를 정의하는 파일 시스템 기반의 라우터를 사용합니다.

각 폴더는 URL 세그먼트에 매핑되는 route 세그먼트를 나타냅니다. 중첩된 route를 만들려면 폴더를 서로 중첩할 수 있습니다.


![Route Example](/assets/img/2024-07-23-DefiningRoutes_0.png)


특별한 page.js 파일을 사용하여 route 세그먼트를 공개적으로 접근할 수 있습니다.

<div class="content-ad"></div>


![image](/assets/img/2024-07-23-DefiningRoutes_1.png)

이 예시에서 /dashboard/analytics URL 경로는 대응하는 page.js 파일이 없기 때문에 공개적으로 접근할 수 없습니다. 이 폴더는 컴포넌트, 스타일시트, 이미지 또는 다른 관련 파일을 저장하는 데 사용될 수 있습니다.

> 알아두면 좋은 점: .js, .jsx 또는 .tsx 파일 확장자를 사용하여 특별 파일을 만들 수 있습니다.

## UI 생성


<div class="content-ad"></div>

특별한 파일 규칙을 사용하여 각 경로 세그먼트에 대한 UI를 생성합니다. 가장 일반적인 것은 경로별로 고유한 UI를 보여주는 페이지와 여러 경로에 걸쳐 공유되는 UI를 보여주는 레이아웃입니다.

예를 들어, 첫 번째 페이지를 만들기 위해 앱 디렉토리 안에 page.js 파일을 추가하고 React 컴포넌트를 export합니다:

```js
export default function Page() {
  return <h1>Hello, Next.js!</h1>
}
```