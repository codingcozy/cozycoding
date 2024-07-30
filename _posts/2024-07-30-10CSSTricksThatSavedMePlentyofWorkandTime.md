---
title: "나에게 많은 시간과 노력을 절약해 준 10가지 CSS 팁"
description: ""
coverImage: "/assets/img/2024-07-30-10CSSTricksThatSavedMePlentyofWorkandTime_0.png"
date: 2024-07-30 16:42
ogImage: 
  url: /assets/img/2024-07-30-10CSSTricksThatSavedMePlentyofWorkandTime_0.png
tag: Tech
originalTitle: "10 CSS Tricks That Saved Me Plenty of Work and Time"
link: "https://medium.com/@arnoldgunter/10-css-tricks-that-saved-me-plenty-of-work-and-time-3698e69d7bac"
---


![이미지](/assets/img/2024-07-30-10CSSTricksThatSavedMePlentyofWorkandTime_0.png)

나는 첫 번째 큰 웹 사이트 중 하나에서 레이아웃을 딱 맞추는 데 얼마나 막혔는지 기억한다.

CSS를 조정하는 데 시간을 보냈다.

그런 다음 여러 블로그와 포럼에서 알려지지 않은 CSS 트릭을 우연히 발견했고, 모든 것이 훨씬 쉬워졌다.

<div class="content-ad"></div>

오늘은 소중한 시간을 아시는 만큼 이것들을 공유해 드리고 싶어요. 이러한 팁들은 여러분의 작업 시간을 절약할 뿐만 아니라 프로젝트를 빛나게 만들어 줄 거에요.

# 1. place-items 속성을 사용한 요소 가운데 정렬

CSS 그리드의 이 속성은 요소를 수평 및 수직으로 쉽게 가운데에 정렬할 수 있게 해 줘요.

```js
.box {
  display: grid;
  place-items: center;
}
```

<div class="content-ad"></div>

# 2. 반응형 타이포그래피 생성

폰트 크기에 대한 미디어 쿼리를 잊어버리세요.

clamp()를 사용하여 반응형 타이포그래피를 설정하세요. 이 함수를 사용하면 폰트 크기에 대한 범위를 설정할 수 있어서 뷰포트와 완벽하게 조화를 이룹니다.

```js
h1 {
  font-size: clamp(1.5rem, 2.5vw, 3rem);
}
```

<div class="content-ad"></div>

첫 번째 값은 최솟값을, 두 번째 값은 변수를, 세 번째 값은 최대 크기를 나타냅니다.

## 3. 자바스크립트 없이 부드러운 스크롤

단 한 줄의 CSS로 웹사이트에 부드러운 스크롤을 추가하세요.

특히 긴 웹사이트의 앵커 링크에 대한 사용자 경험을 향상시킵니다.

<div class="content-ad"></div>

```js
html {
  scroll-behavior: smooth;
}
```

# 4. 사용자 정의 체크박스 및 라디오 버튼

기본 체크박스와 라디오 버튼은 지루할 수 있습니다.

외관: 없음;을 사용하여 쉽게 스타일을 줄 수 있습니다.

<div class="content-ad"></div>


```js
input[type="checkbox"] {
  appearance: none;
  width: 20px;
  height: 20px;
  background: white;
  border: 2px solid #444;
  border-radius: 5px;
}

input[type="checkbox"]:checked {
  background: #444;
}
```

# 5. Creating Matching Color Shadows

Match your element’s shadow with its color using the currentColor keyword.

```js
.button {
  color: #4a90e2;
  box-shadow: 0 4px 8px currentColor;
}
```

<div class="content-ad"></div>

# 6. 고유한 디자인을 위한 변수 폰트

변수 폰트를 사용하여 글꼴을 변경하지 않고도 조정할 수 있는 고유한 타이포그래피 스타일을 만들어보세요.

```js
@font-face {
  font-family: 'VariableFont';
  src: url('variable-font의-경로.woff2') format('woff2-variations');
  font-weight: 100 900;
}

h1 {
  font-family: 'VariableFont';
  font-weight: 300;
}
```

# 7. 그라데이션 테두리 만들기

<div class="content-ad"></div>

gradient borders를 사용하여 화려한 테두리 효과를 얻어보세요.

```css
.button {
  border: 5px solid;
  border-image-source: linear-gradient(to right, #f06, #4a90e2);
  border-image-slice: 1;
}
```

## 8. 반응형 이미지 및 비디오용 종횡비

이미지와 비디오가 다양한 화면 크기에서도 종횡비를 유지하도록 하려면 aspect-ratio 속성을 사용하세요.

<div class="content-ad"></div>

가장 많이 사용되는 속성 중 하나일 것 같아요 ;-).

```js
.img-container {
  aspect-ratio: 16 / 9;
  width: 100%;
  background: url('image.jpg') no-repeat center center;
  background-size: cover;
}
```

# 9. 이미지 위에 텍스트 오버레이하기

position: relative;와 position: absolute;를 사용하여 이미지 위에 텍스트를 쉽게 오버레이할 수 있어요.

<div class="content-ad"></div>

```css
.image-container {
  position: relative;
  width: 100%;
}

.overlay-text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  background-color: rgba(0, 0, 0, 0.5);
  padding: 10px;
}
```

# 10. 복잡한 레이아웃을 위한 CSS Grid

CSS Grid를 사용하여 손쉽게 복잡한 레이아웃을 생성할 수 있습니다.

grid-template-areas를 사용하여 레이아웃 구조를 정의하세요.

<div class="content-ad"></div>

```js
.container {
  display: grid;
  grid-template-areas: 
    'header header'
    'sidebar main'
    'footer footer';
  grid-gap: 10px;
}

.header {
  grid-area: header;
}

.sidebar {
  grid-area: sidebar;
}

.main {
  grid-area: main;
}

.footer {
  grid-area: footer;
}
```

# 결론

이 CSS 트릭은 많은 시간과 노력을 절약해 주었고, 독자님에게도 동일한 도움이 될 것을 바라겠습니다.

이 기술들을 활용하여 웹 프로젝트를 향상시키고, 더욱 효율적이고 시각적으로 매력적인 결과물로 만들어 보세요.


<div class="content-ad"></div>

한번 시도해보세요! 생산성이 높아질 거에요!

코딩 즐기세요!