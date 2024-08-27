---
title: "반응형 웹사이트를 위한 100vh 높이 문제 해결 방법"
description: ""
coverImage: "/assets/img/2024-08-26-HowtoFixthe100vhHeightIssueforMobileResponsiveness_0.png"
date: 2024-08-26 17:09
ogImage: 
  url: /assets/img/2024-08-26-HowtoFixthe100vhHeightIssueforMobileResponsiveness_0.png
tag: Tech
originalTitle: "How to Fix the 100vh Height Issue for Mobile Responsiveness"
link: "https://medium.com/@tilankamihingu2018/solving-the-100vh-height-issue-in-mobile-responsiveness-a-practical-guide-2b74a678fb0f"
isUpdated: true
updatedAt: 1724743504556
---


<img src="/assets/img/2024-08-26-HowtoFixthe100vhHeightIssueforMobileResponsiveness_0.png" />

# 소개

모든 기기에서 매끄럽게 작동하는 반응형 레이아웃을 만드는 것은 현대 웹 개발의 중요한 부분입니다. 그러나 개발자들이 직면하는 일반적인 문제 중 하나는 모바일 기기에서 전체 높이 섹션에 100vh를 사용하는 것입니다. 이는 모바일 브라우저 UI 요소의 동적 성격 때문에 레이아웃의 일부 부분인 액션 바 등이 숨겨질 수 있는 문제로 이어질 수 있습니다. 이 블로그 포스트에서는 이 문제에 대한 제 경험과 프로젝트에서 사용할 수 있는 효과적인 해결책을 소개하겠습니다.

# 모바일 기기에서 100vh의 문제

<div class="content-ad"></div>

웹 레이아웃을 구축할 때 보통 100vh를 사용하여 섹션이 뷰포트의 전체 높이에 걸쳐 나타나게 합니다. 이는 데스크톱에서 잘 작동하지만 모바일 기기에서 문제를 일으킬 수 있습니다. 여기에 대한 이유는 다음과 같습니다:

- 모바일 브라우저는 주소 표시줄이나 도구 모음과 같은 동적 UI 요소를 가지고 있어 사용자가 페이지와 상호 작용함에 따라 보여지는 영역이 변경됩니다.
- 100vh를 사용하면 이러한 UI 요소가 계산에 포함되어 콘텐츠의 일부(예: 하단 작업 표시줄)가 보이지 않거나 예기치 않은 스크롤링을 유발할 수 있습니다.
- 이 문제는 iPhone, iPad 및 다른 기기에서 특히 눈에 띄게 나타납니다. 브라우저 UI가 항상 보이지 않는 경우가 있기 때문입니다.

# 실제 시나리오

저의 프로젝트 중 하나에서 상단 내비게이션 바, 가운데 컨텐츠 뷰 및 "다음" 또는 "취소"와 같은 버튼이 있는 하단 작업 표시줄을 포함한 레이아웃이 있었습니다. 마주한 문제는 이 하단 작업 표시줄이 때로는 모바일 기기에서 가려지는 경우가 있었는데, 특히 브라우저 UI가 보이는 경우에 그랬습니다. 이는 사용자들이 스크롤하지 않으면 작업 버튼을 보거나 사용할 수 없어 불편한 사용자 경험을 초래했습니다.

<div class="content-ad"></div>

# 이전 방법: 100vh와 calc()를 사용한 접근

처음에는 calc() 함수를 사용하여 중간 콘텐츠 섹션의 높이를 계산했습니다. 이 아이디어는 상단 네비게이션 바와 하단 액션 바를 고려한 후 남은 공간으로 중간 콘텐츠 높이를 설정하는 것이었습니다:

```css
.middle-content {
    height: calc(100vh - (var(--top-nav-height) + var(--bottom-nav-height)));
}
```

이 방법은 논리적으로 보였지만, 모바일 기기에서 실패했습니다. 이는 브라우저 UI(예: 주소 표시줄)로 인해 가시적인 뷰포트 높이가 100vh보다 작았기 때문이었습니다. 결과적으로, 하단 액션 바가 종종 화면 밖으로 밀려나가는 문제가 발생했습니다.

<div class="content-ad"></div>

# 해결책 1: SVH (Small Viewport Height) 사용

![이미지](/assets/img/2024-08-26-HowtoFixthe100vhHeightIssueforMobileResponsiveness_1.png)

다양한 해결책을 조사한 후, 100vh 대신 SVH (작은 뷰포트 높이)를 사용하여 문제가 해결되었음을 발견했습니다:

```css
.middle-content {
    height: calc(100svh - (var(--top-nav-height) + var(--bottom-nav-height)));
}
```

<div class="content-ad"></div>

# svh가 작동하는 이유:

- svh는 주소 표시줄과 같이 항상 표시되지 않는 모바일 브라우저 UI 요소가 차지하는 높이를 제외합니다.
- 이는 모바일 기기에 대해 더 신뢰할 수 있는 단위로 만들어줍니다. 내용이 표시 영역 내에 들어가게 하고 하단 작업 표시줄에 접근할 수 있게 합니다.

# 해결책 2: Flexbox를 사용한 더 견고한 접근 방식

![이미지](/assets/img/2024-08-26-HowtoFixthe100vhHeightIssueforMobileResponsiveness_2.png)

<div class="content-ad"></div>

svh가 빠른 해결책을 제공하는 동안, 특히 새 프로젝트에는 flexbox로 레이아웃을 생성하는 것이 더 나은 장기적인 해결책이라고 생각합니다. 이 접근 방식은 고정 높이를 완전히 피하고 레이아웃이 동적으로 조정되도록합니다:


.container {
  display: flex;
  flex-direction: column;
  height: 100vh; 또는 height: 100svh; (정밀도용)
}

.top-nav {
  padding: 16px;
}

.middle-content {
  flex-grow: 1;
  overflow-y: auto;
}


<div class="content-ad"></div>


.bottom-action-bar {
    padding: 16px;
}

## 왜 Flexbox가 최상의 솔루션인가요?

- 동적이고 적응형: 중간 콘텐츠에 flex-grow: 1을 사용하여 상하 섹션 사이의 사용 가능한 공간을 자동으로 차지하며 뷰포트 크기나 방향 변경에 적응합니다.
- 높이 계산 회피: 100vh, svh 또는 dvh와 같은 뷰포트 높이 단위를 사용할 필요가 없습니다. 이로 인해 레이아웃 문제가 발생할 수 있는데, Flexbox가 간겨합니다.
- 더 간단하고 유지보수가 쉬운 CSS: Flexbox는 CSS를 간소화하여 이해하고 유지하기 쉽게 만들어 레이아웃 버그의 위험을 줄입니다.

## 결론


<div class="content-ad"></div>

100vh 높이 문제를 모바일 반응성에서 처리하는 것은 도전적일 수 있지만, 올바른 방법으로 접근하면 모든 기기에서 원활하게 작동하는 레이아웃을 만들 수 있습니다. 기존 프로젝트의 경우 svh를 사용하여 콘텐츠가 보이도록 빠른 해결책을 제공할 수 있습니다. 그러나 새로운 프로젝트의 경우, 플렉스박스 기반 레이아웃을 채택하는 것이 가장 견고하고 확장 가능한 솔루션으로, 모든 기기에서 일관되고 예측 가능한 작동을 보장할 수 있습니다.