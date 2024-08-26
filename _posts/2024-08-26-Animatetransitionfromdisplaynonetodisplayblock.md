---
title: "display none에서 display block으로 전환하는 애니메이션效果를 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-Animatetransitionfromdisplaynonetodisplayblock_0.png"
date: 2024-08-26 17:24
ogImage: 
  url: /assets/img/2024-08-26-Animatetransitionfromdisplaynonetodisplayblock_0.png
tag: Tech
originalTitle: "Animate transition from display none to display block"
link: "https://medium.com/@centrodph/animate-transition-from-display-none-to-display-block-e71f0927ddea"
isUpdated: false
---



![image](/assets/img/2024-08-26-Animatetransitionfromdisplaynonetodisplayblock_0.png)

display: none에서 display: block으로 요소를 애니메이션하는 것은 CSS에서 오랫동안 어려운 과제였습니다. 전통적인 display 속성은 전환이 지원되지 않아 부드러운 애니메이션을 만들기 어렵게 만들었습니다. 그러나 @starting-style at-rule 및 다른 새로운 CSS 기능의 도입으로 이제 이러한 상태 간 부드러운 전환을 만들기 위한 강력한 도구를 사용할 수 있습니다.

[YouTube Link](https://www.youtube.com/watch?v=vmDEHAzj2XE)

# display: none의 문제점


<div class="content-ad"></div>

통상 요소를 display: none에서 display: block으로 애니메이팅하는 것은 불가능했었습니다. 그 이유는 다음과 같습니다:

- display 속성은 애니메이션을 지원하지 않습니다.
- 요소가 display: none 상태일 때, 문서 흐름에서 제거되므로 속성을 전환할 수 없습니다.

# 원활한 애니메이션을 위한 새로운 CSS 기능

크롬 116 및 117은 이산 프로퍼티에 대한 부드러운 애니메이션과 전환을 가능케 하는 네 가지 새로운 웹 플랫폼 기능을 도입했습니다:

<div class="content-ad"></div>

- 키프레임 타임라인에서 display 및 content-visibility 애니메이션 효과 지정 (Chrome 116+)
- transition-behavior 속성에서 allow-discrete 키워드 사용 가능 (Chrome 117+)
- 입력 애니메이션을 위한 @starting-style 규칙 (Chrome 117+)
- 애니메이션 중 상위 레이어 동작 제어를 위한 overlay 속성 (Chrome 117+)

이러한 기능들을 자세히 살펴보겠습니다.

# 키프레임에서 display 애니메이션

Chrome 116부터 키프레임 규칙에서 display 및 content-visibility를 사용할 수 있습니다. 여기 예시가 있습니다:

<div class="content-ad"></div>

```js
.card {
  animation: fade-out 0.5s forwards;
}
```

```js
@keyframes fade-out {
  100% {
    opacity: 0;
    display: none;
  }
}
```

이 애니메이션은 요소를 서서히 흐려지게 만들고 마지막에 display: none으로 설정합니다.

# transition-behavior 속성

<div class="content-ad"></div>

디스플레이와 같은 이산 속성을 전환하려면 transition-behavior 속성의 allow-discrete 모드를 사용하세요:

```js
.card {
  transition: opacity 0.25s, display 0.25s;
  transition-behavior: allow-discrete;
}
```

```js
.card.fade-out {
  opacity: 0;
  display: none;
}
```

# Entry 애니메이션을 위한 @starting-style Rule

<div class="content-ad"></div>

`@starting-style` 규칙을 사용하면 진입 애니메이션의 초기 상태를 정의할 수 있습니다:

```js
@starting-style {
  .item {
    opacity: 0;
    height: 0;
  }
}
```

```js
.item {
  height: 3rem;
  display: grid;
  overflow: hidden;
  transition: opacity 0.5s, transform 0.5s, height 0.5s, display 0.5s allow-discrete;
}
```

이 설정은 높이가 0이고 투명도가 0인 상태에서 마지막 상태로의 부드러운 진입 애니메이션을 생성합니다.

<div class="content-ad"></div>

# 최상위 레이어로 요소 애니메이션 적용하기

최상위 레이어를 사용하는 대화상자(dialogs)나 팝오버(popovers)와 같은 요소들에 대해 부드러운 애니메이션을 위해 @starting-style과 오버레이(overlay) 속성을 사용할 수 있습니다:

```js
@starting-style {
  dialog[open] {
    translate: 0 100vh;
  }
}
```

```js
dialog[open] {
  translate: 0 0;
}
dialog {
  transition: translate 0.7s ease-out, overlay 0.7s ease-out allow-discrete, display 0.7s ease-out allow-discrete;
  translate: 0 100vh;
}
```

<div class="content-ad"></div>

오버레이 속성은 요소가 다른 요소에 의해 잘림이나 가림을 받지 않고 애니메이션 중 상단 레이어에 유지되도록 보장합니다.

# CodePen 예시

다음은 이러한 기술을 보여주는 CodePen입니다:

CodePen: @starting-style을 사용하여 display: none으로부터 애니메이션화

<div class="content-ad"></div>

# 브라우저 지원

2024년 현재, 대부분의 최신 브라우저에서 Chrome, Edge 및 Firefox를 포함하여 이러한 기능이 지원됩니다. 그러나 최신 브라우저 지원 정보를 확인하려면 "Can I Use"를 확인하는 것이 좋습니다.

# 이전 브라우저를 위한 대체 방법

이러한 새로운 기능을 지원하지 않는 브라우저를 위해 JavaScript 기반의 대체 방법을 사용할 수 있습니다.

<div class="content-ad"></div>

```js
if (!CSS.supports('@starting-style {}')) {
  // 여기에 대체 애니메이션 논리 처리
}
```

# 결론

@starting-style, transition-behavior: allow-discrete 및 overlay 속성의 도입은 display: none에서 display: block으로 요소를 애니메이션화하는 오랜 문제에 우아한 해결책을 제공합니다. 이러한 기능을 사용하면 복잡한 JavaScript 우회를 필요로하지 않고 부드럽고 효율적인 전환을 가능하게 합니다.

# 참고문헌

<div class="content-ad"></div>

- CSS @starting-style — MDN Web Docs
- Display None에서 Block으로 애니메이션 효과 주기 — YouTube
- 사용 가능한가: @starting-style
- 부드러운 입장 및 퇴장 애니메이션을 위한 네 가지 새로운 CSS 기능 — Chrome 개발자
원문: https://centrodph.github.io/gerardo-perrucci/blog/css/animate-transition-from-display-none