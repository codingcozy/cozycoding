---
title: "CSS로 만든 태양계"
description: ""
coverImage: "/assets/img/2024-08-26-TheSolarSysteminCSS_0.png"
date: 2024-08-26 20:06
ogImage: 
  url: /assets/img/2024-08-26-TheSolarSysteminCSS_0.png
tag: Tech
originalTitle: "The Solar System in CSS"
link: "https://dev.to/madsstoumann/the-solar-system-in-css-51bo"
isUpdated: false
---


태양계는 CSS로 많이 만들어졌어요 - Codepen에서 검색해보세요! 그럼 왜 다시 만들어야 하죠?

더 좋고 간단해지기 때문이죠 - 지금은 몇 줄의 CSS로 반응형 태양계를 만들 수 있어요.

아주 기본적인 마크업으로 시작해봐요:

```js
<ol>
  <li class="sun"></li>
  <li class="mercury"></li>
  <li class="venus"></li>
  <li class="earth"></li>
  <li class="mars"></li>
  <li class="jupiter"></li>
  <li class="saturn"></li>
  <li class="uranus"></li>
  <li class="neptune"></li>
</ol>
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

행성은 순서대로 나열되어 있기 때문에 순서가 있는 목록을 사용합니다.

다음으로, 기본 `ol` 스타일을 해제하고 그리드로 스타일을 설정합니다:

```js
ol {
  all: unset;
  aspect-ratio: 1 / 1;
  container-type: inline-size;
  display: grid;
  width: 100%;
}
```

이제 행성 궤도에는 "그리드 스택"을 사용할 것입니다. 절대 위치와 많은 변환 대신, 그리드 항목을 모두 스택할 수 있습니다:

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
li {
  grid-area: 1 / -1;
  place-self: center;
}
```

각 행성마다 지름에 맞는 --d 변수를 설정하고 width: var(--d);를 사용하여 이미지 크기를 조절했습니다:

<img src="/assets/img/2024-08-26-TheSolarSysteminCSS_0.png" />

멋지죠! 이제 가상 요소 ::after를 사용하여 행성들을 추가해봅시다:

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
li::after {
  aspect-ratio: 1 / 1;
  background: var(--b);
  border-radius: 50%;
  content: '';
  display: block;
  width: var(--w, 2cqi);
}
```

ChatGPT에게 각 행성을 위한 멋진 방사형 그라데이션을 생성해달라고 요청해보세요. 우리가 태양계를 만든다고 말하고 행성의 크기는 1에서 6cqi 사이로 하도록 요청해보세요. 완전히 정확하지는 않지만, 여전히 상당히 큰 크기 차이를 유지하면서도 인식할 수 있는 차이를 유지하도록 해요.

```js
.mercury {
  --b: radial-gradient(circle, #c2c2c2 0%, #8a8a8a 100%);
  --w: 2.0526cqi;
}

.venus {
  --b: radial-gradient(circle, #f4d03f 0%, #c39c43 100%);
  --w: 2.6053cqi;
}

.earth {
  --b: radial-gradient(circle, #3a82f7 0%, #2f9e44 80%, #1a5e20 100%);
  --w: 3.1579cqi;
}

.mars {
  --b: radial-gradient(circle, #e57373 0%, #af4448 100%);
  --w: 3.7105cqi;
}

.jupiter {
  --b: radial-gradient(circle, #d4a373 0%, #b36d32 50%, #f4e7d3 100%);
  --w: 4.8158cqi;
}

.saturn {
  --b: radial-gradient(circle, #e6dba0 0%, #c2a13e 100%);
  --w: 5.3684cqi;
}

.uranus {
  --b: radial-gradient(circle, #7de3f4 0%, #3ba0b5 100%);
  --w: 4.2632cqi;
}

.neptune {
  --b: radial-gradient(circle, #4c6ef5 0%, #1b3b8c 100%);
  --w: 6cqi;
}
```

이제 우리는:


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

<img src="/assets/img/2024-08-26-TheSolarSysteminCSS_1.png" />

행성을 각기 다른 궤적 속도로 애니메이션화하려면 다음을 추가합니다:

```js
li::after {
  /* 이전 스타일 */
  animation: rotate var(--t, 3s) linear infinite;
  offset-path: content-box;
}
```

offset-path를 주목하세요. 이것이 궤적 애니메이션을 간단하게 만드는 열쇠입니다. `li`의 모양을 따라 행성을 이동시키기 위해 해야 할 일은 다음과 같습니다:

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
@keyframes rotate {
  to {
    offset-distance: 100%;
  }
}
```

그게 다에요! 저는 ChatGPT에게 "Neptune"을 기반으로 하고 회전 속도가 20초인 타이밍을 계산해 달라고 했어요:

## 결론

몇 가지 규칙만으로 순수 CSS로 간단한 2D 버전 태양계를 만들었어요. 더 깊이 들어가고 싶다면 이렇게 해도 돼요:

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

- 실제 거리와 크기를 사용하고 calc()를 사용해주세요.
- `ul`에 `transform: rotateX(angle)`를 추가하여 의사 3D로 변형해보세요:

![image](/assets/img/2024-08-26-TheSolarSysteminCSS_2.png)

... 그리고 행성들을 "다시 펴는" 용으로 matrix3d를 사용해볼까요?

코딩하세요!