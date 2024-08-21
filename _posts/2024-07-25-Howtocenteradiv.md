---
title: "div 중앙 정렬하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-25 11:42
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "How to center a div"
link: "https://dev.to/neonx26/how-to-center-a-div-12ai"
isUpdated: true
---




여기에 각 CSS 기술을 사용하여 `div`를 가운데 정렬하는 방법을 설명하겠습니다.

## 1. Flexbox를 사용하여 div 가운데 정렬하기 :

코드 :

```js
<div class="container">
    <div class="centered-div">가운데 정렬된 콘텐츠</div>
</div>
```

<div class="content-ad"></div>

```js
.container 
{
  display: flex; /* 플렉스박스 레이아웃을 사용합니다 */
  justify-content: center; /* 수평으로 가운데 정렬됩니다 */
  align-items: center; /* 수직으로 가운데 정렬됩니다 */
  height: 100vh; /* 전체 뷰포트 높이를 차지합니다 */
}
```

설명:

- Flexbox는 컨테이너 내 항목들 간 간단한 정렬과 공간 분배를 가능하게 하는 레이아웃 모델입니다.
- justify-content: center는 컨테이너 내 항목들을 수평으로 가운데 정렬합니다.
- align-items: center는 항목들을 수직으로 가운데 정렬합니다.
- height: 100vh는 컨테이너가 전체 뷰포트 높이를 차지하도록 설정합니다.

## 2. 그리드 사용하기


<div class="content-ad"></div>

코드 :

```js
.container {
  display: grid; /* CSS Grid 레이아웃을 사용합니다 */
  place-items: center; /* 수평 및 수직 가운데 정렬 */
  height: 100vh; /* 뷰포트 전체 높이 */
}
```

설명:

- CSS Grid는 복잡한 레이아웃을 쉽게 만들 수 있는 방법을 제공합니다.
- place-items: center는 그리드 컨테이너 내에서 항목들을 수직 및 수평으로 가운데 정렬하는 축약형입니다.

<div class="content-ad"></div>

## 3. Margin Auto 사용하기 (블록 요소)

코드:

```js
.box {
  width: 300px; /* 고정 너비 */
  height: 200px; /* 고정 높이 */
  margin: 0 auto; /* 수평 가운데 정렬 */
}
```

설명:

<div class="content-ad"></div>

- 블록 요소(예: `div`)의 경우, 왼쪽과 오른쪽에 `margin: auto`를 설정하면 요소가 부모 컨테이너 내에서 자동으로 가운데 정렬됩니다. 이때 요소의 너비가 정의되어 있어야 합니다.

## 4. 절대 위치 사용

코드:

```js
.container {
    position: relative;
    height: 100vh;
}
.centered-div {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

<div class="content-ad"></div>

설명:

- position: absolute을 사용하면 상자가 일반 문서 흐름에서 벗어나 가장 가까운 위치 지정된 조상에 상대적으로 배치됩니다 (이 경우에는 컨테이너).
- top 및 left 속성은 상자를 가운데로 배치하며, transform: translate(-50%, -50%)은 상자의 크기를 고려하여 다시 이동시켜 실제로 상자를 중앙에 배치합니다.

## 5. 텍스트 정렬 사용하기 (인라인 요소용)

코드:

<div class="content-ad"></div>

```js
<div class="container">
    <div class="centered-div">가운데 정렬된 콘텐츠</div>
</div>
```

```js
.container {
  text-align: center; /* 인라인 요소들을 수평으로 가운데 정렬합니다 */
}

.centered-div {
  display: inline-block; /* text-align가 작동하기 위해 인라인으로 설정되어야 함 */
  width: 300px; /* 고정 너비 */
  height: 200px; /* 고정 높이 */
}
```

설명:

- 부모 컨테이너에 text-align: center를 설정하면 내부의 인라인 또는 인라인 블록 요소를 가운데 정렬할 수 있습니다.
- display: inline-block 속성은 블록 요소를 인라인 요소로 취급하게 하여 text-align 속성을 반응적으로 만듭니다.

<div class="content-ad"></div>

## 요약

- Flexbox와 Grid는 요소를 가운데 정렬하고 레이아웃을 관리하는 강력한 옵션을 제공하는 현대적인 CSS 레이아웃 기술입니다.
- Margin auto는 고정폭 블록 요소에 적합한 간단한 방법입니다.
- 절대 위치 지정은 요소를 부모 요소를 기준으로 정확하게 제어해야 할 때 유용합니다.
- 텍스트 정렬은 인라인 요소에 적용되며 가운데 정렬을 위한 간편한 옵션입니다.

레이아웃 요구에 가장 잘 맞는 방법을 자유롭게 선택해 주세요!