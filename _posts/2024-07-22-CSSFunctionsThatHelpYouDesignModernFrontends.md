---
title: "최신 프론트엔드 디자인을 위한 CSS 함수들"
description: ""
coverImage: "/assets/img/2024-07-22-CSSFunctionsThatHelpYouDesignModernFrontends_0.png"
date: 2024-07-22 11:21
ogImage: 
  url: /assets/img/2024-07-22-CSSFunctionsThatHelpYouDesignModernFrontends_0.png
tag: Tech
originalTitle: "CSS Functions That Help You Design Modern Frontends"
link: "https://medium.com/gitconnected/css-functions-that-help-you-design-modern-frontends-5ba7f4eaf018"
isUpdated: true
---




![2024-07-22-CSSFunctionsThatHelpYouDesignModernFrontends_0](/assets/img/2024-07-22-CSSFunctionsThatHelpYouDesignModernFrontends_0.png)

CSS 스타일링 언어는 초기에 시맨틱 HTML 구조의 색상을 설정하고 위치를 조정하며 텍스트 스타일을 지정하는 데 사용되었습니다. CSS는 많은 사용자 중심 및 개발자 친화적 기능을 통해 현대적이고 매력적인 웹 페이지를 생산적으로 디자인하는 데 이용됩니다. 최근 CSS 표준은 내장된 JavaScript 없이 DOM 애니메이션, 반응형 디자인 기능, 이미지 효과, 그라데이션 효과 등 다양한 개발자 생산성 중심 기능을 제공합니다.

전통적인 CSS 속성은 대개 고정된 값만을 허용했기 때문에, 개발자들은 동적 값이 필요한 상황에 대처하기 위해 다양한 대체 방법을 사용해야 했습니다. 또한 어떤 상황에서는 단일 고정 값을 사용하여 단일 CSS 속성에 대해 여러 값으로 표현하는 것이 충분하지 않을 수도 있습니다. 즉, RGB 값을 사용하여 색상을 정의하는 것과 같은 경우입니다. 현재 CSS 함수는 이러한 개발자 요구 사항을 생산적으로 충족시키기 위해 거의 모든 곳에 존재합니다. 미래의 CSS 디자인 초안은 계속해서 CSS 함수를 추가하여 개발자를 지원하고 있습니다.

이 글에서는 미래적이고 고품질 및 매력적인 웹 페이지를 디자인하는 데 사용할 수 있는 여러 CSS 함수를 설명하겠습니다. 이미 rgb, url과 같은 CSS 함수를 사용하고 있을 수 있지만, 다음 CSS 함수도 숙달해서 최신 CSS 스타일시트를 작성하는 CSS 전문가가 되어보세요!

<div class="content-ad"></div>

# calc() 함수를 사용하여 기본 산술 연산 수행하기

일반적으로 CSS 속성 값은 단위가 있는 (또는 없는) 고정된 속성 값을 허용하기 때문에 기본 산술 연산을 거기서 사용할 수 없습니다. calc 함수를 사용하면 CSS 원자형 데이터 유형으로 산술 연산을 수행할 수 있습니다. 컨테이너의 높이를 동적으로 조절해야하지만 푸터 메뉴로 40px를 줄여야한다고 가정해보세요. 높이를 계산하기 위해 calc 함수를 사용할 수 있습니다:

```js
<div class="container"></div>
```

```js
body {
  margin: 0px;
  padding: 0px;
  height: 100vh;
}

.container {
  width: 100%;
  height: calc(100% - 40px);
  background: #ccc;
}
```

<div class="content-ad"></div>

위의 코드 스니펫은 뷰포트 높이에 맞춰 컨테이너 높이를 늘리지만 푸터 메뉴를 위한 40픽셀 하단 갭을 가지도록합니다. 미리보기는 다음과 같습니다:

![예시 이미지](https://miro.medium.com/v2/resize:fit:1138/1*Sd4468xkHaUrOCUbE8ia0w.gif)

calc를 다른 내장된 CSS 함수 내에서 사용할 수 있으며, calc 내에서 다른 CSS 함수를 사용할 수도 있습니다. 또한, calc 산술 함수를 사용하여 다소 복잡한 계산을 구성할 수 있습니다.

다음 CSS 정의를 살펴보세요:

<div class="content-ad"></div>

```js
:root {
  --color-ratio: 10;
}

.container {
  width: 100%;
  padding: 12px;
  box-sizing: border-box;
  height: calc(100% - 40px);
  font-size: calc(10px + 1.5vw + 1.5vh);
  color: hsl(calc(360deg / var(--color-ratio)), 50%, 30%);
  overflow: hidden;
  background: #ccc;
}
```

여기서 뷰포트 크기에 따라 동적으로 글꼴 크기를 설정하고, 런타임에 CSS 변수에 따라 컨테이너 글꼴 색상을 계산합니다.

<img src="https://miro.medium.com/v2/resize:fit:1138/1*QNVAKGqq_IV62wH35ihzcQ.gif" />

기본 산술은 모든 프로그래머가 알아야 하는 개념입니다. 컴퓨터 기반의 산술 기법은 일반적으로 컴퓨터 알고리즘에 속하며, 모든 프로그래머가 초기 컴퓨터 과학 교육에서 따르는 분야입니다. 몇 가지 JavaScript 알고리즘 인터뷰 질문을 배우고 컴퓨터 알고리즘 지식을 향상시키세요. 아래 이야기를 통해 더 많이 알아보세요.

<div class="content-ad"></div>

# CSS 필터 함수 사용하기

웹 페이지 사용자와의 상호 작용을 개선하기 위해 종종 DOM 요소에 다양한 시각 효과를 적용해야 합니다. 예를 들어 사용자가 마우스를 가리킬 때 텍스트나 배경 색상을 변경하는 등의 전통적인 색상 기반 DOM 효과 외에도 CSS 필터를 사용할 수 있습니다. CSS 필터는 DOM 요소와 함께 작동하며 전환 또는 애니메이션과 함께 사용할 수 있어 네이티브 CSS 필터로 매력적인 이미지 효과를 구현할 수 있습니다. 여러 필터 함수를 사용하여 filter 속성을 통해 CSS 필터를 적용할 수 있습니다.

먼저 간단한 DOM 요소에 필터를 적용해 보겠습니다:

```js
button {
  padding: 10px;
  font-size: 20px;
  border: none;
  border-radius: 4px;
  background-color: #5e94ff;
  transition: filter 0.2s linear;
}

button:active {
  filter: blur(2px);
}
```

<div class="content-ad"></div>

위의 CSS 정의는 `button` 요소의 활성 상태에 흐림 효과를 렌더링합니다. blur CSS 필터 함수를 사용했습니다:

![blur effect example](https://miro.medium.com/v2/resize:fit:1094/1*7nSZMGlyvxmlp1yLgNAU6g.gif)

이것은 간단한 데모이지만 CSS 필터를 오버레이, 대화 상자 등과 함께 사용하여 웹 페이지에 미래적인 느낌을 줄 수 있습니다.

이미지 요소와 함께 CSS 필터를 사용하는 것은 개발자들 사이에서 인기가 있습니다. 아래의 예시 코드를 확인해보세요:

<div class="content-ad"></div>

```js
@keyframes img-anim {
  0% {
    filter: hue-rotate(0deg);
  }
  50% {
    filter: hue-rotate(360deg);
  }
  100% {
    filter: hue-rotate(0deg);
  }
}

img {
  animation: img-anim 2s infinite;   
}
```

여기서 CSS 필터를 사용하여 모든 이미지 요소를 애니메이션화했습니다. 웹 페이지에 이미지를 추가하면 아래와 같이 계속 애니메이션됩니다:

<img src="https://miro.medium.com/v2/resize:fit:1122/1*gvEMS1jTQOYeSpBjYWQ0sw.gif" />

대부분의 웹 개발자들은 사용자 상호작용을 더 향상시키기 위해 이미지 썸네일에 CSS 필터 기반 전환 효과를 추가합니다. MDN 문서에서 모든 지원되는 표준 CSS 필터 함수를 확인하세요.

<div class="content-ad"></div>

# 형태 변환 수행하기

CSS 애니메이션 및 전환은 주로 요소의 위치, 크기, 회전 및 왜곡을 변경해야 하는데, transform CSS 속성은 다양한 내장 요소 변환 함수로 이러한 요구 사항에 대한 해결책을 제공합니다. 간단한 변환 예제로 시작해 보겠습니다. X, Y 및 Z 축에서 요소를 확대하는 함수를 사용하여 요소를 확대하는 것이 가능합니다. 이것은 scaleX 함수를 이용해 구축된 2차원 코인 스피너 애니메이션 입니다:

```js
<div class="coin"></div>
```

```css
@keyframes coin-anim {
  0% {
    transform: scaleX(1);
  }
  50% {
    transform: scaleX(0.1);
  }
  100% {
    transform: scaleX(1);
  }
}

.coin {
  width: 80px;
  height: 80px;
  background-color: #f5b642;
  border-radius: 50%;
  animation: coin-anim 2s infinite;   
}
```

<div class="content-ad"></div>

위의 코드 스니펫은 다음과 같은 동전 스피너 애니메이션을 렌더링합니다:

![coin-spinner](https://miro.medium.com/v2/resize:fit:1096/1*sRnWMiUIaFzWUL1EtXepWg.gif)

scale 함수를 사용하여 DOM 요소를 확대 또는 축소할 수 있습니다. 대부분의 개발자는 이를 사용하여 마우스 오버 시 SVG 아이콘 버튼을 확대하는 데 사용합니다. CSS 변환 기능은 또한 요소 이동 함수를 제공합니다. 예를 들어, 다음 정의는 요소를 오른쪽으로 10px, 아래로 10px 이동시킵니다:

```js
transform: translate(10px, 10px);
```

<div class="content-ad"></div>

회전 함수는 CSS만 사용한 로더 애니메이션을 만드는 데 도움을 줍니다:

```js
<div class="loader"></div>
```

```js
@keyframes loader-anim {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.loader {
  width: 100px;
  height: 100px;
  background-color: transparent;
  border-radius: 50%;
  border: dashed 10px #ccc;
  box-sizing: border-box;
  animation: loader-anim 3s linear infinite;   
}
```

위의 코드는 간단한 CSS만 사용한 로더 애니메이션을 렌더링하며, 다음 미리보기에서 확인할 수 있습니다:

<div class="content-ad"></div>


![image](https://miro.medium.com/v2/resize:fit:1096/1*gD9XK2O42GC8Ud6oB6JdVQ.gif)

CSS 변형은 XYZ 평면에서 고급 요소 왜곡을 위한 skew와 matrix를 지원합니다.

## 배경 이미지로 그라데이션 사용하기

우리는 과거에 웹 버튼을 그라데이션 배경으로 만들기 위해 Adobe Photoshop을 사용했던 시대를 지나왔습니다. 이제 현대적인 CSS 구현은 배경 이미지 CSS 속성에 그라데이션 함수 사용을 지원하며 세 가지 주요 그라디언트 이미지 유형을 구현할 수 있도록 합니다.


<div class="content-ad"></div>

- 선형 그라데이션
- 원형 그라데이션
- 원뿔 그라데이션

이러한 그라데이션 유형에는 반복 버전도 포함되어 있습니다. 즉, 반복되는 원형 그라데이션 배경이 있습니다.

간단한 선형 그라데이션 배경을 가진 버튼을 만들어 보겠습니다:

```js
button {
  border: none;
  border-radius: 4px;
  padding: 10px;
  font-size: 20px;
  background-image: linear-gradient(to top, #ff52c5, #a10261);
  color: #eee;
  font-family: Ubuntu;
  transition: all 0.2s linear;
}

button:hover {
  filter: brightness(120%);
}
```

<div class="content-ad"></div>

위의 CSS 스니펫은 모든 `button` 요소를 다음과 같이 선형 그라디언트로 채워진 버튼으로 변환합니다:

![image](https://miro.medium.com/v2/resize:fit:1096/1*mkZkMRw6_xcgWYlF2JjdRA.gif)

radial-gradient는 원 그라디언트 이미지 배경을 생성하는 데 도움이 됩니다:

```js
background-image: radial-gradient(circle at center, #ff52c5, #a10261);
```

<div class="content-ad"></div>

위의 CSS 정의는 다음 버튼을 생성합니다:

![Button](https://miro.medium.com/v2/resize:fit:1096/1*zcmve2Q6guLXLavkKI44Ew.gif)

한편, conic-gradient 함수를 사용하면 점을 중심으로 회전하는 그라데이션 패턴을 만들 수 있습니다. conic gradient 배경을 사용하여 CSS만으로 파이 차트를 만들 수 있습니다.

# 수학 함수 사용하기

<div class="content-ad"></div>

모든 인기있는 범용 프로그래밍 언어는 내장 표준 라이브러리를 통해 수학 함수를 제공합니다. 예를 들어, JavaScript 해석기는 수학 함수에 접근하기 위해 전역 Math 객체를 노출합니다. CSS는 프로그래밍 언어가 아니지만, CSS 정의 내에서 사용할 수 있는 내장 수학 함수를 제공하여 JavaScript의 개입을 줄일 수 있습니다.

calc 함수를 사용하면 기본적인 수학 계산을 수행할 수 있으며, CSS에서 다양한 요구에 대해 별도의 수학 함수를 사용할 수 있습니다. 예를 들어 삼각함수를 다음과 같이 사용할 수 있습니다:

```css
margin-left: calc(sin(45deg) * cos(60deg) * 100px);
```

CSS는 또한 pow와 sqrt 수학 함수를 지원합니다:

<div class="content-ad"></div>

```js
패딩-왼쪽: calc((pow(10, 2) + sqrt(1024)) * 1px);
```

MDN 문서에 따르면 아직 모든 최신 브라우저가 pow 및 sqrt 함수를 지원하지 않을 수 있으므로 실제 제품에서 사용하기 전에 브라우저 호환성을 확인하는 것이 좋습니다.

CSS에서 min, max 및 clamp 함수를 사용하여 비교를 수행할 수 있습니다. 예를 들어, 다음 코드 스니펫은 max 함수를 사용하여 요소 너비를 동적으로 조절합니다:

```js
<div class="container"></div>
```

<div class="content-ad"></div>

```js
.container {
  background-color: #ccc;
  height: 200px;
  width: max(50%, 200px);
}
```

위의 코드 조각은 다음과 같은 규칙을 사용하여 부모 컨테이너의 50% 너비를 가집니다. 계산된 너비가 200px보다 작을 때 — 그렇지 않으면 너비를 200px로 설정합니다:

<img src="https://miro.medium.com/v2/resize:fit:1098/1*oE7OBQK20SseLBhvaVvnng.gif" />

# attr 함수 사용하여 DOM 속성에 액세스하기


<div class="content-ad"></div>

널리 알려진 ::before와 ::after 가상 요소는 일반적으로 CSS 스타일 시트를 통해 DOM 요소를 스타일링된 내용으로 확장하는 데 도움을 줍니다. 예를 들어, ::before를 사용하여 JavaScript로 새 DOM 요소를 삽입하지 않고 링크 앞에 SVG 아이콘 또는 이모티콘을 추가할 수 있습니다. attr CSS 함수는 CSS에서 속성 값을 액세스할 수 있도록 해주어 content 속성을 사용하여 내용을 추가하는 데 ::after와 ::before 요소와 함께 사용할 수 있습니다.

다음 예제 코드 스니펫을 살펴보세요:

```js
<ul>
  <li><a href="https://developer.mozilla.org">MDN Docs</a></li>
  <li><a href="https://www.w3schools.com">W3Schools</a></li>
</ul>
```

```js
li {
  padding-bottom: 4px;
}

a:hover::after {
  content: ' ► ' attr(href);
}
```

<div class="content-ad"></div>

위의 CSS 코드 조각은 각 `a` 요소의 링크 텍스트 뒤에 하이퍼링크 href를 마우스 오버하면 표시됩니다. 다음 미리보기에서 확인할 수 있습니다:

![마우스 오버 이미지](https://miro.medium.com/v2/resize:fit:1096/1*gnlblFtjAtijlPN65g2Fyw.gif)

현대 CSS 표준은 미래적인 웹 프론트엔드를 개발하기 위한 생산성 중심 기능을 구현하고 있습니다. 다음 이야기에서 현대 CSS 기능을 배워보세요:

읽어 주셔서 감사합니다.