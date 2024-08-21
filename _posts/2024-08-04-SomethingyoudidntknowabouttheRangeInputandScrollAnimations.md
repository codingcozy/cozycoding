---
title: "Range Input과 스크롤 애니메이션 개념 정리"
description: ""
coverImage: "/assets/img/2024-08-04-SomethingyoudidntknowabouttheRangeInputandScrollAnimations_0.png"
date: 2024-08-04 20:06
ogImage: 
  url: /assets/img/2024-08-04-SomethingyoudidntknowabouttheRangeInputandScrollAnimations_0.png
tag: Tech
originalTitle: "Something you didnt know about the Range Input and Scroll Animations"
link: "https://medium.com/@levchenkod/something-you-didnt-know-about-the-range-input-and-scroll-animations-20c21540e8c3"
isUpdated: true
updatedAt: 1724246216183
---



![image](/assets/img/2024-08-04-SomethingyoudidntknowabouttheRangeInputandScrollAnimations_0.png)

CSS-only 스크롤 애니메이션은 정말 놀라운 기능 중 하나입니다. 저는 웹이 애니메이션으로 폭발할 것이라고 믿는 이유 중 하나입니다. 스크롤 애니메이션의 정신적 모델이 이해하기 어렵더라도, CSS 변수를 Range Input 값에 연결할 수 있다는 것을 알게 되면 어떤 JavaScript도 필요하지 않습니다!

다음은 데모입니다:

Range Input을 사용하여 숫자 값을 지원하는 모든 스타일 속성을 제어할 수 있습니다. 따라서 가능성은 무궁무진합니다.


<div class="content-ad"></div>

# 작동 방식

Range Input의 Thumb에 ::-webkit-slider-thumb 의사 요소를 통해 view-timeline 속성을 적용하고 Thumb의 위치를 CSS 변수로 변환하는 것이 가능합니다.

# 단계별

## 0. 우리의 레이아웃

<div class="content-ad"></div>

```js
<div class="wrapper">
  <input type="range" />
  <h1>변경 범위 변경</h1>
</div>
```

## 1. CSS에서 숫자 변수를 만들어보세요.

값은 0부터 1 사이의 어떤 숫자든 될 수 있습니다.

```js
@property — padding {
 syntax: "<number>";
 inherits: true;
 initial-value: 1;
}
```

<div class="content-ad"></div>

## 2. wrapper에 대한 타임라인 규칙 정의

```js
.wrapper {
    timeline-scope: --padding-timeline;
    animation: 1초 paddingKeyframes linear;
    animation-timeline: --padding-timeline;
    animation-range: entry 100% exit 0%;
}
```

## 3. Thumb에 view-timeline 추가

```js
input[type="range"]::-webkit-slider-thumb {
    view-timeline: --padding-timeline x;
}
```

<div class="content-ad"></div>

## 4. 결과 확인

### 주의사항*

처음에 입력한 'Range Input value*'는 정확하지 않을 수 있습니다. 이 값은 실제로 Thumb(위치 지정자)의 위치가 Scroll Container(스크롤러)에 상대적인 값으로 변환됩니다. 정확하지 않은 결과가 나타날 경우, 이것이 그 이유일 수 있습니다.

### 지원

<div class="content-ad"></div>

스크롤 방식으로 애니메이션을 사용하는 기능은 여전히 "편집자 초안"으로 분류되어 있어 널리 지원되는 브라우저가 없으며 어떠한 방식으로든 제품에 사용할 준비가 되어 있지 않습니다.

# 디버깅

이는 브라우저 지원이 아닌 개발 도구도 필요로 합니다. 저는 2024년 8월에 작성 중이므로 캐스팅된 값을 디버깅하는 방법이 JS 없이는 없습니다. 그러므로 다음과 같이 사용해야 합니다:

```js
getComputedStyle(document.body).getPropertyValue('--myVar')
```

<div class="content-ad"></div>

# 결론

범위 입력이 이러한 유형의 값 캐스팅에 독점적으로 권한을 갖고 있는 것 같지만, 이미 흥미로운 개선 사항입니다. 현재 실제 시나리오에서 어디에 사용될 수 있는지 예측하기 어렵지만, 확실히 실험해볼 가치가 있습니다!

# 리소스