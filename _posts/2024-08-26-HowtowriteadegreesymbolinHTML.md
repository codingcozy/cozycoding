---
title: "HTML에서 온도 도수 표기하는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-HowtowriteadegreesymbolinHTML_0.png"
date: 2024-08-26 17:01
ogImage: 
  url: /assets/img/2024-08-26-HowtowriteadegreesymbolinHTML_0.png
tag: Tech
originalTitle: "How to write a degree symbol in HTML "
link: "https://medium.com/@frontendinterviewquestions/how-to-write-a-degree-symbol-in-html-61406f387455"
isUpdated: true
updatedAt: 1724743454967
---


<img src="/assets/img/2024-08-26-HowtowriteadegreesymbolinHTML_0.png" />

더 많은 질문과 답변을 보려면 저희 웹사이트 Frontend Interview Questions을 방문해주세요.

HTML에서 온도 기호(°)와 같은 특수 문자를 웹 페이지에 추가하는 것은 간단합니다. 도 기호는 온도, 각도 및 지리적 좌표를 나타내는 등 다양한 맥락에서 흔히 사용됩니다. 이 기사에서는 HTML에서 도 기호를 작성하는 다양한 방법을 안내해 드릴 것입니다.

## HTML에서 도 기호를 작성하는 방법

<div class="content-ad"></div>

HTML 코드에 도수 기호를 삽입하는 여러 가지 방법이 있습니다. 이러한 방법에는 HTML 엔터티, 유니코드 및 기호의 직접 입력을 사용하는 방법이 포함됩니다.

## 1. HTML 엔터티 사용하기

HTML 엔터티는 HTML에서 특수 문자를 나타내는 방법으로, 그렇지 않으면 코드로 해석될 수 있는 문자를 나타냅니다. 도수 기호는 다음 HTML 엔터티를 사용하여 작성할 수 있습니다:

- &deg;: 도수 기호에 가장 일반적으로 사용되는 엔터티입니다.
- &#176;: 도수 기호를 나타내는 수치 엔터티입니다.

<div class="content-ad"></div>

예시:

```js
<p>오늘 온도는 25℃입니다.</p>
<p>각도는 90°입니다.</p>
```

이 예시에서 &deg;와 &#176;은 렌더링된 HTML 결과에서 도 기호(°)를 생성합니다. 두 줄 모두 다음과 같은 결과가 출력됩니다:

```js
오늘 온도는 25°C입니다.
각도는 90°입니다.
```

<div class="content-ad"></div>

## 2. Unicode 사용하기

도수 기호는 유니코드 문자 참조를 사용하여 추가할 수도 있습니다. 도수 기호의 유니코드는 U+00B0이며, 다음 형식으로 HTML에서 표현될 수 있습니다:

- \u00B0: HTML 문서 내 JavaScript에서 도수 기호를 표현하는 방법입니다.

예시:

<div class="content-ad"></div>

```js
<p>The latitude is 40\u00B0 North.</p>
```

해당 줄이 렌더링되면 다음이 표시됩니다:

```js
The latitude is 40° North.
```

## 3. Direct Input of the Degree Symbol


<div class="content-ad"></div>

에디터가 지원하는 경우 HTML 문서에도 바로 도 전체 기호를 입력하거나 붙여넣을 수 있습니다. 이 방법은 간단하며 특별한 인코딩이 필요하지 않습니다.

예시:

```js
<p>물의 끓는 점은 100°C 입니다.</p>
```

도 기호는 의도한 대로 표시됩니다.

<div class="content-ad"></div>

```js
물은 100°C에서 끓습니다.
```

## 최선의 방법

세 가지 방법 모두 유효하지만, HTML 엔티티(&deg; 또는 &#176;)를 사용하는 것이 일반적으로 다양한 브라우저와 편집기 간의 호환성을 보장하는 데 도움이 됩니다. HTML 엔티티는 특수 문자를 직접 지원하지 않을 수 있는 텍스트 파일과 함께 작업할 때 또는 문자가 다양한 플랫폼에서 올바르게 렌더링되도록 보장할 때도 유용합니다.

## 결론

<div class="content-ad"></div>

HTML에 도수 기호를 넣는 것은 쉽습니다. HTML 엔티티, 유니코드 또는 직접 입력을 통해 수행할 수 있습니다. 가장 신뢰할 수 있는 방법은 &deg; 또는 &#176; 엔티티를 사용하는 것입니다. 이는 모든 브라우저와 기기에서 일관된 렌더링을 보장하기 때문입니다. 온도, 각도 또는 좌표를 표시하더라도, 도수 기호를 올바르게 삽입하는 방법을 알면 웹 콘텐츠의 가독성과 전문성이 향상됩니다.

고지: 본 내용에는 부담 없는 제품 링크가 포함되어 있습니다. 지원해 주셔서 감사합니다!

여기서 구입하실 수 있습니다: Angular에 대한 실용적이고 포괄적인 가이드