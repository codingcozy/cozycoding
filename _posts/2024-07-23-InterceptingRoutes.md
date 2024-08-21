---
title: "Nextjs 14에서 라우트를 가로채는 방법 앱 라우터 활용 안내"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:13
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Intercepting Routes"
link: "https://nextjs.org/docs/app/building-your-application/routing/intercepting-routes"
isUpdated: true
---




# 경로 가로채기

경로를 가로채면 현재 레이아웃 내에서 애플리케이션의 다른 부분에서 경로를 로드할 수 있습니다. 이러한 라우팅 패러다임은 사용자가 다른 컨텍스트로 전환하지 않고도 경로의 내용을 표시하고 싶을 때 유용합니다.

예를 들어, 피드에서 사진을 클릭할 때 사진을 모달로 표시하여 피드 위에 겹쳐서 보여줄 수 있습니다. 이 경우 Next.js는 /photo/123 경로를 가로채고 URL을 가린 후 /feed 위에 겹쳐 표시합니다.

![Intercepting Routes](/assets/img/2024-07-23-InterceptingRoutes_0.png)

<div class="content-ad"></div>

그러나 공유 가능한 URL을 클릭하거나 페이지를 새로고침하여 사진으로 이동할 때, 모달이 아닌 전체 사진 페이지가 렌더링되어야 합니다. Route 가로채기는 발생하지 않아야 합니다.

![이미지](/assets/img/2024-07-23-InterceptingRoutes_1.png)

## 규칙

Route를 가로채는 규칙은 (..) 규칙을 사용하여 정의할 수 있습니다. 이는 상대 경로 규칙 ../와 유사하지만 세그먼트를 위한 것입니다.

<div class="content-ad"></div>

마크다운 형식으로 테이블 태그를 변경하실 수 있습니다.

- `(.)` 동일 레벨 세그먼트와 일치
- `(..)` 한 단계 위 세그먼트와 일치
- `(..)(..)` 두 단계 위 세그먼트와 일치
- `(...)` 루트 앱 디렉토리에서 세그먼트와 일치

예를 들어, 피드 세그먼트 내부에서 (..)photo 디렉토리를 만들어 사진 세그먼트를 가로챌 수 있습니다.

<img src="/assets/img/2024-07-23-InterceptingRoutes_2.png" />

<div class="content-ad"></div>

> 참고: (..) 규칙은 파일 시스템이 아닌 route 세그먼트를 기반으로 합니다.

## 예시

### 모달

Route 가로채기는 병렬 Route와 함께 사용하여 모달을 생성할 수 있습니다. 이를 통해 모달을 생성할 때 발생하는 공통적인 문제를 해결할 수 있습니다.

<div class="content-ad"></div>

- 모달 콘텐츠를 URL을 통해 공유할 수 있도록 만들기.
- 페이지 새로 고침 시 모달을 닫지 않고 컨텍스트 유지하기.
- 뒤로 이동할 때 모달 닫기 대신 이전 경로로 이동하기.
- 앞으로 이동할 때 모달 다시 열기.

사용자가 갤러리에서 클라이언트 측 네비게이션을 사용하여 사진 모달을 열거나 공유 가능한 URL을 통해 사진 페이지로 직접 이동할 수 있는 다음 UI 패턴을 고려해보세요:


![이미지](/assets/img/2024-07-23-InterceptingRoutes_3.png)


위 예시에서 사진 세그먼트로의 경로는 @modal이 슬롯이고 세그먼트가 아니기 때문에 (..) 매처를 사용할 수 있습니다. 이는 사진 라우트가 파일 시스템 레벨이 두 단계 더 높지만 실제로 세그먼트 레벨이 한 단계 높다는 것을 의미합니다.

<div class="content-ad"></div>

병렬 경로에 대한 단계별 예제는 문서를 참조하거나 이미지 갤러리 예제를 확인해보세요.

> 참고사항:
다른 예는 상단 네비게이션 바에서 로그인 모달을 열면서 동시에 /login 페이지를 가지고 있거나, 쇼핑 카트를 측면 모달에서 열 때의 예가 될 수 있습니다.