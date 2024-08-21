---
title: "웹 디자이너가 아닌 사람들을 위한 CSS 기본 가이드"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-23 21:32
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "CSS basics for non-web designers"
link: "https://dev.to/jojo97/css-basics-for-non-web-designers-50h7"
isUpdated: true
---




CSS는 웹 개발의 한 측면으로, 개발자들이 웹 페이지의 레이아웃, 시각적 스타일링 및 사용자 경험을 제어할 수 있게 합니다.

웹 디자이너가 아닌 사람들도 CSS 기본을 이해하면 스킬이 향상되고 경력에서 새로운 가능성을 열 수 있습니다. 이 기사에서는 CSS의 기본을 살펴보겠습니다. 구문, 선택자, 속성 및 값에 대해 다룰 것입니다. 또한 HTML 요소를 스타일링하는 몇 가지 실용적인 응용 사례도 살펴볼 것입니다.

시작해 봅시다!

## CSS란 정확히 무엇인가요?

<div class="content-ad"></div>

CSS는 Cascading Style Sheets의 약자입니다. HTML과 XHTML로 작성된 웹 페이지의 레이아웃과 외관을 제어하는 스타일링 언어입니다. CSS는 스타일이라고도 하는 규칙의 시리즈로 구성되어 있습니다. 이러한 스타일은 HTML 요소에 적용되어 그 표시를 수정할 수 있습니다. CSS는 종종 HTML과 JavaScript와 함께 사용되어 동적 웹 페이지를 생성하는 데 사용됩니다.

## CSS가 HTML과 JavaScript와 관련이 있는 방법

HTML은 웹 페이지의 구조를 제공하며, CSS는 그 구조를 스타일링합니다. 반면에 JavaScript는 페이지에 상호 작용성을 추가합니다. 이 세 가지 기술이 함께 웹 개발의 기본을 형성합니다.

## CSS 구문

<div class="content-ad"></div>

CSS 구문은 HTML 요소에 적용되는 스타일인 규칙으로 구성되어 있습니다. CSS 규칙은 선택자, 속성 및 값 세 부분으로 구성됩니다.

이제 CSS 코드의 기본 구조를 살펴보겠습니다.

```js
선택자 {
  속성: 값;
}
```

걱정 마세요, 이 글에서 위 코드를 나중에 자세히 살펴볼 거에요 😎.

<div class="content-ad"></div>

다음으로 어떻게 할까요? CSS 규칙을 이해해 보겠습니다.

CSS 규칙은 하나 이상의 선언으로 구성되며, 각 선언은 속성과 값으로 이루어집니다. 선택자는 규칙이 적용되는 HTML 요소를 결정합니다.

예시:

```js
p {
  color: blue;
}
```

<div class="content-ad"></div>

이 예시에서 셀렉터는 p이고, 프로퍼티는 color이며 값은 blue입니다. 이 규칙은 웹 페이지의 모든 문단 요소(p)에 적용되어 텍스트 색상을 파랑으로 설정합니다.

### 셀렉터

셀렉터는 CSS 규칙을 사용하여 특정 HTML 요소를 대상으로 하는 데 사용됩니다. 다음과 같은 여러 유형의 셀렉터가 있습니다:

- 요소 셀렉터는 태그 이름을 기반으로 요소를 대상으로 합니다.

<div class="content-ad"></div>

예시:

```js
h1 {
  font-size: 36px;
}
```

해당 규칙은 페이지의 모든 `h1` 요소에 적용되며, 그들의 글꼴 크기를 36px로 설정합니다.

- 클래스 선택자는 요소를 클래스 속성에 기반하여 대상으로 합니다.

<div class="content-ad"></div>

예시:

```js
.header {
   background-color: #f0f0f0;
}
```

이 규칙은 "header" 클래스를 가진 모든 요소에 적용되어 배경 색상이 #f0f0f0으로 설정됩니다.

- ID 선택자는 요소의 ID 속성을 기준으로 대상을 정합니다.

<div class="content-ad"></div>

예시:

```js
#로고 {
  너비: 200픽셀;
}
```

이 규칙은 ID가 "로고"인 요소에 적용되어 너비를 200픽셀로 설정합니다.

- 속성 선택자는 요소의 속성에 따라 대상을 지정합니다.

<div class="content-ad"></div>

예시:

```js
input[type="submit"] {
  background-color: #007bff;
}
```

이 규칙은 type 속성이 "submit"으로 설정된 모든 input 요소에 적용되어 배경색을 #007bff로 설정합니다.

- 속성과 값은 요소에 적용되는 스타일을 정의하는 데 사용됩니다.

<div class="content-ad"></div>

일반적인 CSS 속성 몇 가지는 다음과 같습니다:

- color: 텍스트 색상 설정
- background-color: 배경 색상 설정
- font-size: 글꼴 크기 설정
- padding: 안쪽 여백 설정
- margin: 바깥 여백 설정

## 속성 값 이해하기

속성 값은 키워드, 길이, 백분율 또는 색상일 수 있습니다.

<div class="content-ad"></div>

예시:

```js
p {
  color: blue; /* 키워드 값 */
  font-size: 18px; /* 길이 값 */
  background-color: #ff0000; /* 색상 값 */
  padding: 10%; /* 백분율 값 */
}
```

이 예시에서 color 속성은 "blue"라는 키워드 값으로 설정되었고, font-size 속성은 길이 값인 18픽셀로 설정되었습니다, background-color 속성은 색상 값인 #ff0000으로 설정되었고, padding 속성은 백분율 값인 10%로 설정되었습니다.

## HTML 요소 스타일링

<div class="content-ad"></div>

이제 CSS 구문, 선택자 및 속성의 기본 사항을 다루었으니, 이 지식을 활용하여 일부 HTML 요소를 스타일링해 봅시다.

### 제목

제목은 모든 웹 페이지의 중요한 부분이며, CSS는 제목을 스타일링하는 여러 가지 방법을 제공합니다.

예시:

<div class="content-ad"></div>

```js
h1 {
  font-size: 36px;
  color: #333;
  text-align: center;
}
```

이 예에서는 페이지의 모든 `h1` 요소를 대상으로 하고 글꼴 크기를 36px로, 색상을 #333으로, 텍스트 정렬을 가운데로 설정합니다.

### 단락

단락은 CSS로 스타일을 적용할 수 있는 또 다른 일반적인 요소입니다.

<div class="content-ad"></div>

예시:

```js
p {
  font-size: 18px;
  color: #666;
  padding: 10px;
}
```

이 예시에서는 페이지의 모든 `p` 요소를 대상으로 하여 글꼴 크기를 18px, 색상을 #666으로, 안쪽 여백을 10px로 설정하고 있습니다.

### 링크

<div class="content-ad"></div>

링크를 스타일링하여 모양과 동작을 변경할 수 있어요.

예시:

```js
a {
  color: #007bff;
  text-decoration: none;
}
```

이 예시에서는 페이지의 모든 `a` 요소를 대상으로 하며, 색상을 #007bff로 설정하고 텍스트 장식(밑줄)을 제거하고 있어요.

<div class="content-ad"></div>

### 이미지

이미지를 스타일링하여 크기, 여백 및 기타 속성을 변경할 수 있습니다.

예시:

```js
img {
  width: 100%;
  height: auto;
  margin: 10px;
}
```

<div class="content-ad"></div>

이 예에서는 페이지의 모든 'img' 요소를 대상으로 하여 너비를 부모 요소의 100%로 설정하고, 높이를 자동으로 설정하고, 여백을 10픽셀로 설정합니다.

## 색상 다루기

색상은 웹 디자인의 중요한 부분이며, CSS는 색상을 다루는 여러 가지 방법을 제공합니다.

### 색 이론 기초

<div class="content-ad"></div>

CSS 색상 모델들을 보기 전에, CSS 색상에 대한 몇 가지 기본 색 이론 개념을 알아보겠습니다:

- 색조(Hue): 실제 색상 (예: 빨강, 파랑, 초록)
- 채도(Saturation): 색상의 강도 (예: 밝은, 적인)
- 명도(Value): 색상의 밝기 또는 어둠 (예: 밝은, 어두운)

### CSS 색상 모델

CSS는 여러 색상 모델을 지원합니다.

<div class="content-ad"></div>

- RGB (Red, Green, Blue)
- HEX (16진수 코드)
- HSL (색조, 채도, 명도)
- RGBA (Red, Green, Blue, Alpha) - 투명도 지원
- HSLA (색조, 채도, 명도, Alpha) - 투명도 지원

### CSS에서 색상 사용하기

색상은 다양한 CSS 속성에 적용할 수 있습니다:

- color: 텍스트 색상 설정
- background-color: 배경 색상 설정
- border-color: 테두리 색상 설정

<div class="content-ad"></div>

예시:

```js
p {
  color: #ff0000; /* 텍스트 색상을 빨간색으로 설정합니다 (16진수) */
  background-color: rgba(0, 0, 0, 0.5); /* 배경색을 블랙(투명도 50%)으로 설정합니다 (RGBA) */
}
```

## 타이포그래피

타이포그래피는 웹 디자인에서 중요한 역할을 합니다. CSS는 여러 속성을 제공하여 글꼴 스타일을 제어할 수 있습니다.

<div class="content-ad"></div>

### 글꼴 패밀리

글꼴 패밀리는 font-family 속성을 사용하여 설정할 수 있어요.

예시:

```js
p {
  font-family: Arial, sans-serif; /* 글꼴을 Arial로 설정하고 sans-serif로 fallback합니다 */
}
```

<div class="content-ad"></div>

### 폰트 크기

폰트 크기는 폰트 크기 속성을 사용하여 설정할 수 있습니다.

예시:

```js
p {
  font-size: 18px; /* 폰트 크기를 18픽셀로 설정 */
}
```

<div class="content-ad"></div>

**글꼴 스타일**

글꼴 스타일은 글꼴 스타일 속성을 사용하여 설정할 수 있어요.

예시:

```js
p {
  font-style: italic; /* 글꼴 스타일을 이탤릭체로 설정합니다 */
}
```

<div class="content-ad"></div>

## 레이아웃 및 위치지정

레이아웃과 위치지정은 웹 디자인의 중요한 요소이며, CSS는 요소의 레이아웃과 위치를 제어하기 위해 여러 속성을 제공합니다.

### 표시 속성

표시 속성은 요소의 표시 유형을 결정하는데, 블록(block), 인라인(inline), 또는 숨김(none)과 같은 유형을 지정합니다.

<div class="content-ad"></div>

예시:

```js
div {
  display: block; /* display 속성을 블록으로 설정 */
}
```

### 위치 속성

position 속성은 요소의 위치 지정 방법을 결정합니다. static, relative, absolute 또는 fixed와 같은 값을 가질 수 있습니다.

<div class="content-ad"></div>

예시:

```js
div {
  position: relative; /* 상대적 위치 지정 */
}
```

### 플로트 속성

플로트 속성은 요소가 부모 요소의 왼쪽 또는 오른쪽으로 떠다니는지를 결정합니다.

<div class="content-ad"></div>

예시:

```js
div {
  float: left; /* float 속성을 left로 설정합니다 */
}
```

### 상자 모델

상자 모델은 요소의 구조를 나타내며, 콘텐츠 영역, 패딩, 테두리 및 여백을 포함합니다.

<div class="content-ad"></div>

예시:

```js
div {
  width: 200px; /* 콘텐츠 영역의 너비 설정 */
  padding: 10px; /* 패딩 설정 */
  border: 1px solid #000; /* 테두리 설정 */
  margin: 10px; /* 마진 설정 */
}
```

## 추가 CSS 개념

이해하는 데 중요한 몇 가지 추가 CSS 개념이 있습니다. 여기에 몇 가지가 있습니다:

<div class="content-ad"></div>

- CSS 단위 (px, em, rem, %)
- CSS 전처리기 (Sass, Less)
- CSS 프레임워크 (Bootstrap, Tailwind CSS)
- CSS 그리드 및 플렉스박스

## 결론

이 글에서는 선택자, 속성, 값, 단위를 포함한 CSS의 기본을 다뤘습니다. 또한 HTML 요소를 스타일링하는 방법, 색상 처리, 레이아웃 및 위치 지정을 컨트롤하는 방법을 탐색하였습니다. 이 지식을 바탕으로 아름다우면서 기능적인 웹 페이지를 만들 수 있을 것입니다.

제게 연락하고 싶다면 언제든지 메일 주세요.