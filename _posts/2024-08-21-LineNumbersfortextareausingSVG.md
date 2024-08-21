---
title: "textarea의 라인 번호 SVG를 활용해 추가하는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-LineNumbersfortextareausingSVG_0.png"
date: 2024-08-21 18:57
ogImage: 
  url: /assets/img/2024-08-21-LineNumbersfortextareausingSVG_0.png
tag: Tech
originalTitle: "Line Numbers for textarea using SVG"
link: "https://dev.to/madsstoumann/line-numbers-for-using-svg-1216"
isUpdated: true
updatedAt: 1724245748258
---


요 며칠 전에 JSON 스키마 생성기를 작업하고 있었는데, `텍스트영역`에 줄 번호를 표시하고 싶었어요. 아무것도 복잡한게 아니고, 소프트 라인 브레이크나 어떤 복잡한 것은 고려하지 않았어요.

여러 방법을 찾아보았어요:

- 배경 이미지 사용하기 (TinyMCE는 PNG를 사용해요)
- `ol` 정렬된 목록 사용하기.

그중에 마음에 드는 방법은 없었어요! 첫 번째 방법은 선명하지 않았고, 이미 제 `텍스트영역`에 적용된 스타일과도 일치하지 않았어요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

두 번째 방법은 순서가 지정된 목록을 유지하기 위해 많은 자바스크립트가 필요했어요: 동적으로 `li` 요소를 추가/제거하고, 스크롤 이벤트를 동기화하고 등등 많은 작업을 해야 했죠.

그래서 저는 하이브리드 방식을 만들기로 결정했어요.

CSS 사용자 지정 속성으로 저장된 동적으로 생성된 SVG이에요. 이 SVG는 배경 이미지로 사용되며, 부모 `textarea` 요소에서 스타일을 상속받아요.

![image](/assets/img/2024-08-21-LineNumbersfortextareausingSVG_0.png)

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

시작해보죠.

## JavaScript

먼저, 메인 메소드:

```js
lineNumbers(element, numLines = 50, inline = false)
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

`textarea` 요소를 사용할 element이며, numLines는 렌더링할 라인 수이고, inline은 생성된 이미지를 element에 저장할지 (true) 또는 document.body에 저장할지를 나타냅니다.

다음으로, 사용자 지정 속성에 대한 접두사를 정의합니다:

```js
const prefix = '--linenum-';
```

계속 진행하기 전에, 기존 속성을 재사용할지 확인합니다:

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
if (!inline) {
  const styleString = document.body.getAttribute('style') || '';
  const regex = new RegExp(`${prefix}[^:]*`, 'g');
  const match = styleString.match(regex);

  if (match) {
    element.style.backgroundImage = `var(${match[0]})`;
    return;
  }
}
```

다음으로, 요소에서 스타일을 추출하여 SVG를 동일한 글꼴 패밀리, 글꼴 크기, 줄 높이 등과 함께 렌더링합니다:

```js
const bgColor = getComputedStyle(element).borderColor;
const fillColor = getComputedStyle(element).color;
const fontFamily = getComputedStyle(element).fontFamily;
const fontSize = parseFloat(getComputedStyle(element).fontSize);
const lineHeight = parseFloat(getComputedStyle(element).lineHeight) / fontSize;
const paddingTop = parseFloat(getComputedStyle(element).paddingTop) / 2;
const translateY = (fontSize * lineHeight).toFixed(2);
```

또한 속성에 대한 무작위 ID가 필요합니다:

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
// 이제 SVG를 렌더링하는 시간입니다:

const svg = `<svg xmlns="http://www.w3.org/2000/svg">
  <style>
    svg { background: ${bgColor}; }
    text {
      fill: hsl(from ${fillColor} h s l / 50%);
      font-family: ${fontFamily};
      font-size: ${fontSize}px;
      line-height: ${lineHeight};
      text-anchor: end;
      translate: 0 calc((var(--n) * ${translateY}px) + ${paddingTop}px);
    }
  </style>
  ${Array.from({ length: numLines }, (_, i) => `<text x="90%" style="--n:${i + 1};">${i + 1}</text>`).join("")}
</svg>`;
```

이해를 돕기 위해 한 번 살펴보겠습니다: 


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

`style` 섹션에서는 이전에 `textarea`에서 추출한 스타일을 설정합니다. `text` 요소에 대해 y 및 dy 속성 대신 CSS를 사용하여 텍스트 요소를 번역하는 --n-속성을 사용합니다.

마지막 부분에서는 numLines에서 생성된 배열을 반복하고 `text` 요소를 주요 SVG에 추가합니다.

거의 다 왔어요!

생성된 SVG를 url() 속성으로 사용하려면 인코딩이 필요합니다:

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
const encodedURI = `url("data:image/svg+xml,${encodeURIComponent(svg)}")`;
```

그리고 마지막으로, 이 속성을 요소(element)나 문서 본문(document-body)에 설정합니다:

```js
const target = inline ? element : document.body;
target.style.setProperty(id, encodedURI);
element.style.backgroundImage = `var(${id})`;
```

그게 다입니다!

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

별로 나쁘지 않고 610바이트만 차지하고 있어요. 압축해서 최소화했답니다!

## 데모

여기에서 데모를 확인할 수 있고 전체 스크립트를 다운로드할 수도 있어요.

아래는 인라인 속성 로직을 사용하지 않은 단순화된 Codepen이에요:

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 장단점

장단점이 있나요? 물론 있죠!

저 개인적으로는 — 제 현재 프로젝트를 위해서 — `textarea` 내에서 JSON 미리보기에 줄 번호를 간단하고 깔끔하게 추가하는 방법이 필요했고, 이 방법이 딱 맞았어요.

### 장점

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

#### DOM 조작 감소

이 방법은 DOM을 조작하는 데 의존하지 않습니다. 줄 번호는 하나의 SVG로 생성되어 CSS 사용자 정의 속성 내에 저장됩니다.

#### 자동 동기화

줄 번호는 배경 이미지의 일부이므로 텍스트 내용과 함께 자동으로 스크롤되어 수동 동기화 논리가 필요 없습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

#### 요소 간 재사용성

CSS 사용자 정의 속성에 생성된 SVG를 저장함으로써 여러 요소에서 재사용할 수 있습니다. 이는 만약 여러 요소에서 동일한 줄 번호가 필요한 경우, 모두 동일한 사용자 정의 속성을 참조하여 중복된 SVG 생성을 피할 수 있습니다.

#### 확장성

SVG의 벡터 특성으로 인해 줄 번호는 어떤 확대 수준에서도 선명하고 명확하게 유지됩니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 단점

#### 접근성

순서가 있는 목록은 화면 낭독기 및 보조 기술에서 더 잘 인식되지만, SVG 기반 라인 번호는 무시되거나 잘못 해석될 수 있습니다.

#### 사용자 정의 복잡성

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

개별 줄 번호와 상호 작용하는 스타일링은 순서대로 나열된 목록에서 간단합니다. 그에 반해 SVG 접근 방식은 특정 줄 번호에 대한 사용자 정의나 상호 작용을 추가하기 어렵게 만듭니다.

#### 브라우저 호환성

SVG 및 CSS 사용자 정의 속성은 모든 브라우저에서 일관되게 렌더링되지 않을 수 있습니다. 현재 구현은 Safari에서 문제가 있으며 translateY에서 (paddingTop / 10)을 빼야 합니다.

#### 동적 콘텐츠 처리

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

정렬된 목록은 더 유연하게 동적 콘텐츠 업데이트를 다루는 데 사용될 수 있습니다. 행을 추가하거나 제거하는 경우 SVG 방식은 전체 배경 이미지를 재생산하고 다시 적용해야 할 수도 있습니다.