---
title: "CSS  px 소수점 사용하여 스타일링 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-27-CSSStylingwithpxdecimals_0.png"
date: 2024-07-27 13:44
ogImage: 
  url: /assets/img/2024-07-27-CSSStylingwithpxdecimals_0.png
tag: Tech
originalTitle: "CSS  Styling with px decimals"
link: "https://medium.com/@rayonline/css-styling-with-px-decimals-7d683bc8bbf8"
---


<img src="/assets/img/2024-07-27-CSSStylingwithpxdecimals_0.png" />

CSS 스타일링의 신비로운 세계로 들어가 봅시다. 이번에는 iOS와 Android 간의 서로 다른 픽셀 처리 방식에 초점을 맞춰 보겠습니다. 아이폰에서 안드로이드 기기로 이동하면 동일한 CSS가 약간 어색하게 보이는 것을 주목한 적이 있나요? 그렇다면, 그것은 iOS와 Android가 픽셀, 특히 소수점을 다르게 다루기 때문입니다.

픽셀의 고민

iOS는 세심함을 중요시하는 친구처럼 작동하며, 소수점을 정밀하게 렌더링합니다. 반면에 Android는 덜 깐깐하게, 그 소수를 반올림하거나 내립니다. 이것이 큰 문제로 들리지 않을 수도 있지만, 픽셀 완벽을 목표로 할 때면 머리 아픈 문제가 될 수 있습니다.

<div class="content-ad"></div>

또 다른 중요한 측면은 iOS가 모든 소수 값을 버린다는 점입니다. 이는 값들을 최종 CSS 형식으로 렌더링하기 전에 값을 사전 처리하면 소수점이 남아 예상치 못한 결과를 초래할 수 있습니다. 저는 Android가 iOS가 하는 것처럼 버림이 아닌 반올림을 하기 때문에 우위를 가지고 있다고 생각합니다.

코드 예시 및 팁

두 플랫폼 모두에서 깔끔하게 보이는 반응형 버튼을 작업 중이라고 가정해봅시다. 다음과 같이 작성할 수 있습니다:

```js
.button {
   padding: 10.5px 20.5px;
   font-size: 14.5px;
}
```

<div class="content-ad"></div>

iOS에서는 이 값들이 내림된 것으로 처리될 것이지만 Android에서는 조금 더 큰 값을 줄 수도 있어요...