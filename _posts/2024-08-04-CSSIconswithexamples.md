---
title: "예제와 함께 알아보는 CSS 아이콘 사용법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-04 19:36
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "CSS Icons with examples"
link: "https://dev.to/wasifali/css-icons-with-examples-5cjm"
---


## CSS 아이콘

아이콘은 아이콘 라이브러리를 사용하여 우리의 HTML 페이지에 쉽게 추가할 수 있습니다.

## 아이콘 추가 방법

HTML 페이지에 아이콘을 추가하는 가장 간단한 방법은 Font Awesome와 같은 아이콘 라이브러리를 사용하는 것입니다.
지정된 아이콘 클래스의 이름을 인라인 HTML 요소(예: `i` 또는 `span`)에 추가하면 됩니다.
CSS 아이콘은 PNG 또는 SVG와 같은 전통적인 이미지 형식이 아닌 CSS(계층식 스타일 시트)를 사용하여 생성된
기호 또는 그래픽 표현입니다.
이들은 종종 이미지 파일에 의존하지 않고 웹 디자인에 시각적 요소를 추가하는 데 사용됩니다.
CSS 아이콘을 만드는 몇 가지 일반적인 방법이 있습니다:

<div class="content-ad"></div>

## 폰트 아이콘:

이것들은 Font Awesome, Material Icons 또는 Ion 아이콘과 같은 특별한 아이콘 폰트로부터 만들어진 아이콘들입니다. 이러한 폰트는 CSS로 스타일을 지정할 수 있는 일련의 글리프(기호)를 포함하고 있습니다.

## 예시

예를 들어, 당신은 .fa-heart와 같은 클래스를 사용하여 하트 아이콘을 HTML에 추가한 뒤 CSS 속성으로 스타일을 지정할 수 있습니다.

<div class="content-ad"></div>

## CSS 모양:

아이콘은 `div`, `span` 등의 HTML 요소를 CSS 속성인 border, border-radius, background 및 transform과 같은 CSS 속성으로 스타일링하여 순수 CSS를 사용하여 만들 수 있습니다. 이 방법은 주로 간단한 기하학적 모양이나 사용자 정의 디자인에 사용됩니다. CSS 아이콘은 확장 가능성, 쉬운 사용자 정의 가능성 및 이미지에 비해 파일 크기가 작을 수도 있다는 장점을 제공합니다. 웹 디자인에 시각적 요소를 추가하는 다재다능하고 효율적인 방법이 될 수 있습니다.

## Font Awesome

프로젝트에 Font Awesome를 포함하려면:
다음 라인을 HTML의 `head`에 추가하세요:

<div class="content-ad"></div>

```js
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
```

## 아이콘 사용하기

아이콘을 사용하려면 적절한 클래스를 가진 `i` 또는 `span` 요소를 추가하세요:

```js
<i class="fas fa-camera"></i>
```

<div class="content-ad"></div>

## Material Icons

프로젝트에 Material Icons를 포함하세요:
다음 코드를 추가해주세요.

```js
<link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
```

## 아이콘 사용하기

<div class="content-ad"></div>

아이콘을 사용하려면 아래와 같이 `i` 요소를 추가하세요. `i` 요소 안에는 material-icons 클래스와 아이콘 이름이 포함되어야 합니다:

```js
<i class="material-icons">camera_alt</i>
```

## 2. 사용자 지정 아이콘을 위한 CSS 사용

CSS를 사용하여 고유한 아이콘을 만들 수도 있습니다. 다음은 순수 CSS를 사용한 간단한 예시입니다:

<div class="content-ad"></div>

## 기본 HTML 구조 만들기

```js
<div class="icon-star"></div>
```

## CSS를 추가하여 아이콘 스타일링하기

```js
.icon-star {
    display: inline-block;
    width: 0;
    height: 0;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-bottom: 100px solid gold;
    position: relative;
    transform: rotate(35deg);
}


.icon-star:before {
    content: '';
    position: absolute;
    top: 0;
    left: -50px;
    width: 0;
    height: 0;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-bottom: 100px solid gold;
    transform: rotate(-70deg);
}
```

<div class="content-ad"></div>

이 CSS 조각은 테두리와 위치 설정을 사용하여 간단한 별 모양을 만듭니다.

## 3. SVG 아이콘

고품질 아이콘을 위해 SVG를 사용할 수도 있습니다:

## 인라인 SVG

<div class="content-ad"></div>

```js
<svg width="24" height="24" viewBox="0 0 24 24">
  <path d="M12 2L2 7v10l10 5 10-5V7z"/>
</svg>
```

## 배경 이미지로 SVG 사용하기

```js
.icon {
    width: 24px;
    height: 24px;
    background: url('data:image/svg+xml;base64,...') no-repeat center center;
    background-size: contain;
}
```

## 팁


<div class="content-ad"></div>

크기와 색상: 폰트 아이콘 및 인라인 SVG의 경우, 폰트 크기 또는 너비와 높이 속성으로 크기를 조절하고, 색상은 색상 또는 SVG의 경우 채우기로 변경할 수 있습니다.
접근성: 필요에 따라 설명 텍스트나 aria 속성을 추가하여 항상 접근성을 고려해주세요.
프로젝트에 가장 적합한 방법을 찾기 위해 자유롭게 실험하고 다양한 방법을 혼합해 사용해보세요!