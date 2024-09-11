---
title: "개발자 대부분 사용하지 않지만 반드시 사용해야하는 5가지 CSS 기능 "
description: ""
coverImage: "/assets/img/2024-09-10-Top5CSSFeaturesYoureNotUsingButShouldBe_0.png"
date: 2024-09-10 17:50
ogImage: 
  url: /assets/img/2024-09-10-Top5CSSFeaturesYoureNotUsingButShouldBe_0.png
tag: Tech
originalTitle: "Top 5 CSS Features Youre Not Using (But Should Be)"
link: "https://medium.com/gitconnected/top-5-css-features-youre-not-using-but-should-be-990dcdca9171"
isUpdated: true
updatedAt: 1726041459751
---



![CSS Image](/assets/img/2024-09-10-Top5CSSFeaturesYoureNotUsingButShouldBe_0.png)

CSS는 웹 사이트를 디자인하는 데 도움이 되는 도구입니다.

많은 개발자들이 CSS의 기본을 알고 있지만, 작업 시간을 절약하고 코드를 개선할 수 있는 특별한 기능과 숨겨진 보석들이 있습니다.

여기에는 당신이 사용하지 않을 수도 있는 5가지 CSS 기능이 나와 있습니다, 하지만 확실히 사용해야 할 것들입니다!


<div class="content-ad"></div>

# 1. 측면 비율

첫 번째로, 요소의 측면 비율을 정의하는 한 줄입니다. 이를 통해 설정된 너비에 대응하여 높이를 자동으로 조절할 수 있습니다(또는 그 반대로).

```js
.box {
  width: 90%;
  aspect-ratio: 16/9;
}
```

# 2. 강조 색상

<div class="content-ad"></div>

이 CSS 규칙은 링크 및 버튼과 같은 상호 작용 요소에 일관된 강조 색상을 적용합니다.

이것은 이 색상을 사용할 때마다 각 요소를 개별적으로 스타일링할 필요가 없어 사이트 전체적으로 일관성을 유지할 수 있게 해줍니다.

```js
body {
  accent-color: green;
}
```

# 3. 스무스 스크롤

<div class="content-ad"></div>

CSS 중에서 제가 가장 좋아하는 기능 중 하나에요.

이 기능은 페이지 전체에 대해 부드러운 스크롤을 활성화시켜 주는데, 앵커 링크로 이동할 때 사용자 경험을 향상시키죠.

아래 내용을 확인해보세요:

```js
html {
    scroll-behavior: smooth;
}
```

<div class="content-ad"></div>

# 4. 논리적 속성

margin-left, margin-right, margin-top 및 margin-bottom을 계속 사용에 지쳤나요?

대신 이렇게 해보세요!

```js
.box {
  margin-block: 5px 10px;    /* 5px 상단 여백, 10px 하단 여백 */
  margin-inline: 20px 30px;  /* 20px 왼쪽 여백, 30px 오른쪽 여백 */
}
```

<div class="content-ad"></div>

동일한 방법은 패딩에도 적용됩니다.

```js
.box {
  padding-block: 10px 20px; /* 위/아래 10px, 왼쪽 20px */
  padding-inline: 15px 25px; /* 왼쪽 15px, 오른쪽 25px */
}
```

이러한 속성들은 텍스트 방향(예: 왼쪽에서 오른쪽 또는 오른쪽에서 왼쪽)에 따라 자동으로 조정되어 코드가 보기 좋고 짧고 유연하며 국제화에 친화적이게 됩니다.

# 5. CSS 변수

<div class="content-ad"></div>

CSS 변수, 또는 사용자 정의 속성이라고도 하는 것은 스타일 시트 전체에서 사용할 수 있는 재사용 가능한 값(색상, 글꼴, 크기 등)을 정의할 수 있게 해줍니다.

이를 통해 일관성을 유지하고 프로젝트 크기가 큰 경우에 필요할 때 값들을 쉽게 업데이트할 수 있습니다.

```js
:root {
  --main-color: #3498db;
  --padding-size: 20px;
}

.box {
  background-color: var(--main-color);
  padding: var(--padding-size);
}
```

여기서 --main-color 또는 --padding-size를 한 곳에서 변경하면 이 변수를 사용하는 모든 요소가 일괄적으로 업데이트되어 사이트 전체에 적용됩니다.

<div class="content-ad"></div>

이러한 기능들은 실용적인 CSS의 본질을 담고 있어서, 일반적인 레이아웃 및 스타일링 문제에 대한 해결책을 최소한의 코드로 제공합니다. 이러한 코드를 매일의 작업 흐름에 통합하여, 웹 프로젝트의 미학과 기능성을 효율적이고 효과적으로 향상시킬 수 있습니다.

코딩을 즐기세요!