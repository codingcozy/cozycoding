---
title: " CSS가 일반 흐름에서 수직 정렬을 지원하기 시작했습니다"
description: ""
coverImage: "/assets/img/2024-09-10-CSSNowSupportsVerticalAlignmentInNormalFlow_0.png"
date: 2024-09-10 18:29
ogImage: 
  url: /assets/img/2024-09-10-CSSNowSupportsVerticalAlignmentInNormalFlow_0.png
tag: Tech
originalTitle: " CSS Now Supports Vertical Alignment In Normal Flow"
link: "https://medium.com/@tomaszs2/css-now-supports-vertical-alignment-in-normal-flow-13a85f06adb1"
isUpdated: false
---


# 드디어 많은 년 뒤에 CSS 하나로 요소를 수직으로 가운데 정렬할 수 있습니다!

물론 CSS는 많은 년 동안 수직 정렬을 지원해 왔습니다. 이를 위한 다양한 방법이 있습니다. 예를 들어, 위와 아래 여백에 모두 auto를 설정할 수 있습니다. 이렇게 하면 요소가 가운데 정렬됩니다.

다른 방법으로는 position: absolute, top 50% 및 transform: translate(0, -50%)을 사용하는 방법이 있습니다. 또한 position을 50%로 설정하고 margin을 사용하여 요소를 위로 이동시키는 방법도 있어서 가운데 정렬할 수 있습니다.

하지만 이러한 방법들은 모두 해킹 방법일 뿐입니다. 정직하게 말하면, 우리가 일반적인 방법을 사용할 수 있다면 이렇게 해서는 안 됩니다.

<div class="content-ad"></div>

사실 많은 해 동안 사용 가능한 일반적인 방법이 있습니다:

```js
display: flex;
align-items: center;
```

<img src="/assets/img/2024-09-10-CSSNowSupportsVerticalAlignmentInNormalFlow_0.png" />

이 두 속성을 사용하면 요소가 수직으로 가운데 정렬됩니다.

<div class="content-ad"></div>

하지만 그 단점은 일반 흐름에서 플렉스로 흐름을 변경해야 한다는 것입니다. 왜 이렇게 해야 했을까요? 오랜 시간 동안 모든 브라우저가 일반 흐름에서 동일한 개념을 지원하지 않았기 때문입니다. 이렇게:

```js
align-content: center; #이름을 주목해주세요!
```

모든 곳에서 작동하지 않았습니다. 데스크탑 브라우저가 이를 더 빨리 지원했고, 2024년 3월에는 대다수의 브라우저(브레이브, 파이어폭스, 오페라, 크로-사파-엣지)가 수직 정렬을 지원했습니다.

하지만 2024년 7월 말까지는 모바일 브라우저인 Chrome for Android, Android Browser, 그리고 Firefox for Android도 이에 동참했습니다. 현재 모든 브라우저에서 표준으로 지원되지는 않지만요:

<div class="content-ad"></div>

<img src="/assets/img/2024-09-10-CSSNowSupportsVerticalAlignmentInNormalFlow_1.png" />

전 세계 사용자 중 81.55%를 커버하는 정상 흐름(normal flow)에서 align-content가 화면 흐름(display-flow) 변형의 대안으로 고려될 수 있습니다.

사용법은 간단합니다. 하나의 속성만 정의하면 됩니다:

<img src="/assets/img/2024-09-10-CSSNowSupportsVerticalAlignmentInNormalFlow_2.png" />

<div class="content-ad"></div>

좋은 세로 정렬을 얻으려면:


<img src="/assets/img/2024-09-10-CSSNowSupportsVerticalAlignmentInNormalFlow_3.png" />


아마도 달에 착륙하는 것만큼 혁명적이지는 않지만, 우리가 얻을 수 있는 것을 받아들여야 할 2024년입니다. 하나의 프로퍼티를 사용하는 것이 유용할 수 있으며 기업 환경이 새로운 프로퍼티를 사용하여 코드베이스를 정렬하기 시작하는 것을 기대합니다. 아마 브라우저에서의 지원을 구현하는 것만큼 많은 시간이 걸리지는 않기를 희망합니다 :)

코딩 뉴스를 읽고 싶다면 구독, 팔로우, 좋아요, 박수 및 공유해주세요!