---
title: "CSS 박스 모델 content-box vs border-box 정리"
description: ""
coverImage: "/assets/img/2024-08-21-UnderstandingtheCSSBoxModelcontent-boxvsborder-boxinlinevsblockelements_0.png"
date: 2024-08-21 18:58
ogImage: 
  url: /assets/img/2024-08-21-UnderstandingtheCSSBoxModelcontent-boxvsborder-boxinlinevsblockelements_0.png
tag: Tech
originalTitle: "Understanding the CSS Box Model content-box vs border-box, inline vs. block elements"
link: "https://dev.to/horaceshmorace/understanding-the-css-box-model-content-box-vs-border-box-inline-vs-block-elements-1amh"
isUpdated: true
updatedAt: 1724244570767
---


프론트엔드 개발자로서, CSS 박스 모델을 이해하는 것은 픽셀 완벽한 레이아웃을 제공할 수 있는 데 있어서 매우 중요합니다. 바로 이제 시작해서 인라인 및 블록 요소가 두 가지 상자 모델 유형인 content-box와 border-box의 맥락에서 어떻게 다르게 작동하는지 알아봅시다.

## 기본 사항: 상자 안에는 무엇이 있나요?

![이미지](/assets/img/2024-08-21-UnderstandingtheCSSBoxModelcontent-boxvsborder-boxinlinevsblockelements_0.png)

자세한 내용을 살펴보기 전에, 상자 모델이 무엇인지 다시 한 번 되짚어보는 것이 좋습니다. 페이지의 모든 요소는 상자입니다 (예, 심지어 모양이 실제로 상자 모양이 아닌 것도 상자입니다). 상자는 다른 상자 내부에 있을 수도 있고, 다른 상자를 포함할 수도 있고, 다른 상자와 나란히 있을 수도 있습니다.

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

상자 모델은 다음과 같이 적용되며 다음으로 구성됩니다:

- 콘텐트 상자: 실제 콘텐츠가 들어가는 HTML 요소나 CSS 의사 요소입니다. 텍스트, 이미지 등이 포함됩니다.
- 패딩: 콘텐츠와 테두리 사이의 공간입니다.
- 테두리: 패딩과 콘텐츠를 감싸는 선입니다.
- 마진: 다른 요소를 멀리 밀어내는 테두리 외부의 공간입니다.

따라서 `body`를 포함한 거의 모든 HTML 요소와 모든 CSS 의사 요소는 상자입니다. 그 상자의 "벽"은 테두리이며, 두께(또는 너비)를 가질 수 있습니다. 해당 콘텐츠와 상자의 벽 사이에는 패딩이 있습니다. 그 벽과 다른 상자 사이에는 마진이 있습니다.

## 인라인 요소와 블록 요소 간의 주요 차이점

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

박스 모델이 인라인 요소와 블록 요소에 어떻게 영향을 미치는지의 주요 차이를 이해하는 것이 중요합니다:

- 너비: 블록 요소는 기본적으로 컨테이너를 채우도록 확장됩니다. 인라인 요소는 컨텐츠에 맞게 충분한 공간을 차지합니다.
- 높이: 블록 요소의 경우, 패딩, 테두리 및 마진이 모두 높이를 늘립니다. 인라인 요소는 수직 패딩이나 테두리에 관계없이 라인 높이 내에 유지됩니다.
- 레이아웃 영향: 블록 요소는 수평 및 수직 레이아웃에 모두 영향을 미칩니다. 인라인 요소는 주로 수평 흐름에 관한 것이며 수직 간격에는 최소한의 영향을 미칩니다.
- 마진 병합: 마진 병합은 블록 요소에 특정한 동작으로, 인접한 수직 마진이 병합될 수 있습니다 (따라서 한 요소에 margin-bottom:20px가 있는 경우, 그것은 다음 요소의 margin-top:20px으로 병합되어 하나의 20px 마진이 생깁니다). 인라인 요소는 이 게임에서 제외됩니다.

이제 박스 모델의 구성 요소와 인라인 요소와 블록 요소 간의 차이를 알았으니, 박스 크기 속성에 의해 콘텐트 상자가 영향을 받는 방법을 살펴보겠습니다.

## 박스 크기 지정: content-box 대비 border-box

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

`box-sizing` 속성은 요소의 너비와 높이를 계산하는 방식을 제어하며 레이아웃에 큰 영향을 줄 수 있습니다.

### 1. Inline Elements에 `content-box` 적용하기

`box-sizing: content-box`를 인라인 요소에 적용할 경우:

- 너비와 높이: 너비는 콘텐츠에만 의해 결정됩니다. 여기에 패딩, 테두리, 여백이 추가됩니다.
- 레이아웃 영향: 인라인 요소는 텍스트 흐름을 중단시키지 않기 때문에 요소의 너비는 콘텐츠의 폭만큼입니다. 수직 패딩과 테두리는 시각적으로 표시되지만 주변 줄의 높이에 영향을 주지 않습니다.

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

### 2. content-box with Block Elements

블록 요소에 box-sizing: content-box가 적용되면:

- 너비와 높이: 지정된 너비 또는 높이는 콘텐츠 영역에만 적용됩니다. 패딩과 테두리는 이외의 영역에 추가되어 요소의 전체 크기를 증가시킵니다.
- 레이아웃 영향: 블록 요소는 기본적으로 컨테이너의 전체 너비로 확장됩니다. 패딩 및 테두리는 요소의 크기를 늘려 인접한 요소를 더 멀리 이동시킵니다.

### 3. border-box with Inline Elements

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

인라인 요소에 box-sizing: border-box가 적용되면:

- 너비와 높이: 너비에는 콘텐츠, 패딩, 테두리가 포함됩니다. 콘텐츠 영역은 지정된 너비 내에서 패딩과 테두리를 수용하기 위해 축소됩니다.
- 레이아웃 영향: 요소의 너비는 여전히 콘텐츠에 의해 결정되지만 이제 패딩과 테두리가 이 너비 내에 포함됩니다. 수직 패딩과 테두리는 시각적으로 유지되지만 라인 높이를 변경하지 않습니다.

### 4. 블록 요소에 border-box 적용하기

블록 요소에 box-sizing: border-box가 적용되면:

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

- 너비와 높이: 지정된 너비와 높이는 내용물, 패딩 및 테두리를 포함합니다. 이는 요소의 총 크기가 지정된 차원과 일관되게 유지되므로 패딩이나 테두리를 얼마나 추가하던지 상관없이 동일한 크기로 유지됩니다.
- 레이아웃 영향: 블록 요소의 크기는 패딩과 테두리가 지정된 너비 내에 포함되기 때문에 더 예측 가능합니다. 이는 요소를 옆으로 정렬할 때 레이아웃 디자인을 더 쉽게 관리할 수 있게 합니다.

밑줄 그어진 내용 중요한 부분이에요! content-box가 기본값이기는 하지만, border-box가 더 직관적이고 더 많은 제어를 제공한다는 사실을 많은 사람들이 인정하고 있습니다.

CSS 박스 모델은 강력한 개념일뿐만 아니라 프론트엔드 개발자 도구 상자에 꼭 필요한 도구입니다. 인라인 및 블록 요소에 박스 크기가 어떻게 다르게 영향을 미치는지를 이해하면 레이아웃을 더욱 완벽하게 생성할 수 있어요. 레이아웃 문제 해결에 시달리지 않고 선명하고 기능적인 레이아웃을 만들어낼 수 있게 도와줄 거예요.

박스 모델에 대한 이 내용이 유용했다면, 왼쪽 상단에 하트 모양 박스 위에 마우스를 올려서 이 게시물에 모든 사랑을 보여주세요!