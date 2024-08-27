---
title: "CSS Positioning 완벽 정리"
description: ""
coverImage: "/assets/img/2024-08-21-CSSPositioningWhereExactlyDoYouThinkYoureGoing_0.png"
date: 2024-08-21 19:01
ogImage: 
  url: /assets/img/2024-08-21-CSSPositioningWhereExactlyDoYouThinkYoureGoing_0.png
tag: Tech
originalTitle: "CSS Positioning Where Exactly Do You Think Youre Going"
link: "https://dev.to/gdebojyoti/css-positioning-where-exactly-do-you-think-youre-going-2ec6"
isUpdated: true
updatedAt: 1724246033336
---


환영합니다! CSS 포지셔닝의 마법 같은 세계에 오신 것을 환영합니다. 여기서는 웹 요소의 건물주와 디자이너 역할을 할 수 있습니다! 버튼을 원하는 위치에 정확히 놓는 방법이나 헤더가 어디론가 헤매지 않도록 하는 방법에 대해 궁금했다면, 여기가 바로 그곳입니다. 위트있고 재미있는 CSS 포지셔닝의 기초를 알아보도록 하겠습니다.

![CSS Positioning](/assets/img/2024-08-21-CSSPositioningWhereExactlyDoYouThinkYoureGoing_0.png)

## 1. 정적 포지셔닝: 기본 설정

정적 포지셔닝은 TV의 기본 설정과 비슷합니다. 특별한 것은 없고, 기본적이고 직관적입니다. 기본적으로 모든 요소는 정적으로 포지셔닝되어 있으며, 이는 문서에서 예상대로 흘러가는 것을 의미합니다 - 하나씩 차례대로요.

<div class="content-ad"></div>

```js
div {
    position: static;
}
```

`position: static;`을 사용하면 브라우저에 해당 요소를 일반적인 위치에 배치하라고 알리는 것이죠. 이것은 정적 위치 지정의 "여기 아무 것도 없어"라고 이해하시면 됩니다.

## 2. 상대 위치 지정: 약간 어색하지만 매력적인 친구

상대 위치 지정은 항상 약간 어색하지만 매력적인 친구처럼 작동합니다. 이것은 페이지의 나머지 부분을 방해하지 않고 요소를 일반적인 위치를 기준으로 상대적으로 이동할 수 있게 해줍니다.

<div class="content-ad"></div>

```css
.relative-box {
    position: relative;
    top: 10px;
    left: 20px;
}
```

여기서 .relative-box는 원래 위치에서 아래쪽으로 10픽셀, 오른쪽으로 20픽셀 이동합니다. 주변 요소는 그대로 위치합니다.

## 3. 절대 위치 지정: 자유로운 탐험가

절대 위치 지정은 집단을 따르지 않고 모험을 즐기는 친구와 같습니다. 가장 가까운 "static이 아닌" 조상(즉, position: relative, absolute, fixed 또는 sticky가 지정된 조상)을 기준으로 요소를 원하는 곳에 위치시킬 수 있습니다.

<div class="content-ad"></div>

```js
<div class="parent">
    <div class="child"></div>
</div>
```

```js
.parent {
    position: relative;
}

.child {
    position: absolute;
    top: 30px;
    right: 10px;
}
```

이 예시에서 .child는 .parent에서 위에서 30픽셀, 오른쪽으로 10픽셀 떨어진 위치에 배치됩니다. 만약 .parent가 없다면, 가장 가까운 위치 지정 조상이나 뷰포트를 기준으로 배치됩니다.

## 4. 고정 위치 지정: 고집하는 정적

<div class="content-ad"></div>

고친 위치 지정은 움직이지 않는 좋아하는 의자를 가지고 있는 것과 같아요. 이것을 사용하면 요소를 뷰포트에 상대적으로 위치시켜 스크롤이 얼마나 많이 이동하든 항상 같은 위치에 두게 할 수 있어요.

```js
.floating-action-button {
    position: fixed;
    bottom: 20px;
    right: 20px;
}
```

position: fixed;를 사용하면 .floating-action-button은 뷰포트의 하단에서 20픽셀, 우측에서 20픽셀 떨어진 위치에 유지되어, 스크롤해도 같은 곳에 있게 됩니다. 이는 사이트의 헤더나 채팅 위젯, 또는 지속적인 호출-투-액션 버튼과 같은 고정 요소에 이상적이에요.

## 5. 스티키 위치 지정: 두 마리 토끼를 모두 잡는 법

<div class="content-ad"></div>

스티키 위치 설정은 항상 조명 속에서 부분적으로 나가 있는 친구가 있는 것과 같습니다. 이것은 상대적 위치와 고정 위치 사이의 혼합체로 생각할 수 있습니다. 위치 값이 sticky로 설정된 요소는 특정 지점을 스크롤할 때 해당 컨테이너 내에서 해당 위치에 고정되지만, 그렇지 않은 경우에는 상대적으로 위치된 요소처럼 동작합니다.

```css
.sticky-box {
    position: sticky;
    top: 0;
}
```

이 예제에서 .sticky-box는 특정 지점을 스크롤할 때 컨테이너의 맨 위에 고정됩니다. 계속 스크롤할 때, 컨테이너가 보일 때만 고정 상태로 유지되지만 컨테이너가 보이지 않게 되면 스티키 요소도 함께 스크롤됩니다. 이것은 페이지의 특정 섹션 내에서 계속 보이도록 유지하고 싶은 헤더, 내비게이션 메뉴 또는 사이드바에 적합합니다.

## Z-Index: The Overachiever

<div class="content-ad"></div>

아, z-index! 여기서 약간 경쟁적인 요소가 생깁니다. 요소가 겹치면 z-index가 어떤 것이 맨 위에 있을지를 결정합니다. 웹 요소에 대한 고교 인기 경쟁과 비슷하죠.

```js
.box1 {
    position: absolute;
    z-index: 10;
}

.box2 {
    position: absolute;
    z-index: 5;
}
```

이 경우에는 .box1이 .box2 위에 나타날 것입니다. 왜냐하면 z-index가 더 높기 때문이죠. 단지 기억하세요, z-index는 위치가 지정된 요소 (relative, absolute, fixed, 또는 sticky)에서만 작동합니다.

## 요약

<div class="content-ad"></div>

CSS 위치 지정은 퍼즐처럼 느껴질 수 있지만, 한 번 감을 잡으면 정확하고 멋진 요소를 배치할 수 있을 거예요. 각 위치 유형에는 웹 디자인에서 특별한 역할이 있어요. 시도해보세요. 그러면 웹 페이지가 완벽하게 보이게 만들 수 있는 방법을 알 수 있을 거예요.

즐거운 코딩 되세요!