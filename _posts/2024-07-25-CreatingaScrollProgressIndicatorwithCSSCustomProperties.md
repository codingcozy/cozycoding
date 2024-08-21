---
title: "CSS 사용자 정의 속성을 사용한 스크롤 진행률 표시기 만들기"
description: ""
coverImage: "/assets/img/2024-07-25-CreatingaScrollProgressIndicatorwithCSSCustomProperties_0.png"
date: 2024-07-25 11:44
ogImage: 
  url: /assets/img/2024-07-25-CreatingaScrollProgressIndicatorwithCSSCustomProperties_0.png
tag: Tech
originalTitle: "Creating a Scroll Progress Indicator with CSS Custom Properties"
link: "https://medium.com/@rahajason/creating-a-scroll-progress-indicator-with-css-custom-properties-aa1d31964741"
isUpdated: true
---




<img src="/assets/img/2024-07-25-CreatingaScrollProgressIndicatorwithCSSCustomProperties_0.png" />

이 튜토리얼에서는 CSS 사용자 지정 속성, 애니메이션 및 카운터를 사용하여 동적 스크롤 진행률 표시기를 만드는 방법을 안내합니다. 이를 통해 웹 페이지의 스크롤 진행률을 백분율로 표시할 수 있습니다. 이 튜토리얼을 마치면 이러한 현대적인 CSS 기능을 사용하여 매력적이고 정보를 제공하는 사용자 인터페이스 요소를 만드는 방법을 이해하게 됩니다.

# 단계 1: 사용자 지정 속성 정의

먼저, 스크롤 진행률을 추적할 사용자 지정 속성을 정의해야 합니다.

<div class="content-ad"></div>

```js
@property --s {
  syntax: '<integer>';
  inherits: true;
  initial-value: 0; 
}
```

위 코드는 다음과 같은 속성을 갖는 사용자 정의 속성 --s를 정의합니다:

- syntax: ``integer``; 속성 값이 정수인지를 확인합니다.
- inherits: true; 자식 요소에서 상속되도록 합니다.
- initial-value: 0; --s의 초기 값이 0으로 설정됩니다.

# Step 2: 루트 애니메이션 생성하기

<div class="content-ad"></div>

다음으로, 페이지를 스크롤할 때 루트 요소에 애니메이션을 설정하여 --s 속성을 업데이트할 것입니다.

```js
:root {
  animation: scroll 1s linear;
  animation-timeline: scroll();
}
```

여기에서:

- animation 속성은 1초 동안 선형 방식으로 실행되는 scroll이라는 애니메이션을 적용합니다.
- animation-timeline: scroll();은 애니메이션을 스크롤 타임라인과 연관시키지만, 이것은 실험적인 기능이므로 모든 브라우저에서 지원되지 않을 수 있습니다.

<div class="content-ad"></div>

# 단계 3: 애니메이션을 위한 키프레임 정의

스크롤 애니메이션에 대한 키프레임을 정의하여 --s 속성을 업데이트합니다.

```js
@keyframes scroll {
  to {
    --s: 100;
  }
}
```

이 애니메이션은 --s 속성을 0부터 100까지 증가시킬 것입니다.

<div class="content-ad"></div>

# 단계 4: 스크롤 진행률 인디케이터 스타일링

우리는 body:before 가상 요소를 사용하여 스크롤 진행률을 표시할 것입니다.

```css
body:before {
  content: "글의 %c 번을 읽었습니다.";
  counter-reset: s var(--s);
  font-size: 50px;
  font-family: system-ui, sans-serif;
  font-weight: 900;
  white-space: pre;
  text-align: center;
  position: fixed;
  inset: 0;
  width: fit-content;
  height: fit-content;
  margin: auto;
}
```

위 스타일링은 다음을 수행합니다:

<div class="content-ad"></div>

- 내용: "스크롤 진행률\A " counter(s) "%"; 해당 내용은 s 카운터 값과 퍼센트 기호를 포함하여 "스크롤 진행률" 텍스트를 표시합니다. \A는 줄 바꿈을 생성합니다.
- counter-reset: s var(--s); s 카운터를 --s 속성의 값으로 재설정합니다.
- 나머지 속성은 텍스트를 스타일링합니다 (글꼴 크기, 글꼴, 글꼴 무게), 올바르게 표시되도록합니다 (white-space: pre), 그리고 화면 중앙에 위치하도록 설정합니다 (position, inset, width, height, margin).

# 단계 5: 페이지를 스크롤할 수 있도록 확인

마지막으로, 페이지가 스크롤할 내용이 충분한지 확인해야 합니다.

```js
body {
  min-height: 500vh;
}
```

<div class="content-ad"></div>

바디의 최소 높이를 뷰포트 높이의 300%로 설정하여 페이지를 스크롤할 만큼 충분히 길게 만듭니다.

모든 CSS 코드를 함께 정리했습니다:

```js
@property --s {
  syntax: '<integer>';
  inherits: true;
  initial-value: 0; 
}

:root {
  animation: scroll 1s linear;
  animation-timeline: scroll();
}

@keyframes scroll {
  to {
    --s: 100;
  }
}

body:before {
  content: "You readed \A" counter(s) "% of the post";
  counter-reset: s var(--s);
  font-size: 50px;
  font-family: system-ui, sans-serif;
  font-weight: 900;
  white-space: pre;
  text-align: center;
  position: fixed;
  inset: 0;
  width: fit-content;
  height: fit-content;
  margin: auto;
}

body {
  min-height: 500vh;
}
```

이 튜토리얼을 따라하면 CSS 사용자 지정 속성, 애니메이션 및 카운터를 사용하여 스크롤 진행률 표시기를 만들었습니다. 이 기능은 사용자 경험을 향상시켜 콘텐츠를 얼마나 보았는지와 얼마나 남았는지를 시각적으로 표시해줍니다. 이러한 기술을 활용하여 더 매력적이고 상호작용적인 웹 페이지를 만들어보세요. 그리고 모두 자바스크립트 없이 가능합니다.

<div class="content-ad"></div>

데모와 저장소

🙏 읽어 주셔서 감사합니다