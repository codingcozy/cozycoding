---
title: "flex 1의 용도는 무엇인가요"
description: ""
coverImage: "/assets/img/2024-07-30-Whatistheuseofflex1_0.png"
date: 2024-07-30 16:44
ogImage: 
  url: /assets/img/2024-07-30-Whatistheuseofflex1_0.png
tag: Tech
originalTitle: "What is the use of flex 1"
link: "https://medium.com/javascript-in-plain-english/what-is-the-use-of-flex-1-9c1932fa3969"
isUpdated: true
---




<img src="/assets/img/2024-07-30-Whatistheuseofflex1_0.png" />

현대 웹 디자인의 세계에서 동적이고 반응형 레이아웃을 구현하는 것이 중요합니다. 이 때 Flexbox가 구원의 손길을 내밀어 줍니다! 💪 유연성 있는 레이아웃을 손쉽게 만들 수 있도록 하는 직관적인 속성을 가진 Flexbox는 개발자들에게 힘을 불어넣어 줍니다. 이 강력한 도구의 핵심에는 flex 속성이 있으며, 이는 컨테이너 내에서 요소의 동작을 제어하기 위한 무한한 가능성을 열어 줍니다. flex 속성의 복잡성을 이해하고, 그 하위 속성들을 해석하며, 실제 예제를 통해 그 실무적 응용을 밝혀 낼 여행을 떠나봅시다.

# flex 속성 해독하기 🧐

flex 속성은 세 가지 하위 속성의 힘을 결합한 단축명령어 💎입니다:

<div class="content-ad"></div>

- flex-grow: 이 속성은 요소가 컨테이너의 나머지 공간 내에서 얼마나 커질 수 있는지를 결정합니다. 기본적으로 0으로 설정되어 있어서 요소가 초기 크기를 초과하지 않는다는 것을 의미합니다. 그러나 양수를 할당하면 요소가 다른 플렉스 항목들에 비례하여 확장될 수 있습니다. 이를 스트레칭 슈퍼파워로 생각해보세요! 🦸‍♀️
- flex-shrink: 제한된 공간에서의 댄스에 flex-shrink가 필요합니다. 이 속성은 컨테이너 공간이 부족해지면 요소가 얼마나 줄어야 하는지를 결정합니다. 기본 값인 1은 비례적인 축소를 가능하게 합니다 – 요소들이 사이즈를 우아하게 조정하여 오버플로우 혼돈을 피하게 되는 모습을 상상해보세요! 🧘‍♀️ 이를 0으로 설정하면 요소가 축소되지 않고 자신의 위치를 유지합니다.
- flex-basis: 이 속성은 요소의 초기 스테이지 존재감으로 생각할 수 있습니다. 나머지 공간 분배가 이뤄지기 전에 요소의 기본 크기를 설정합니다. 기본값인 auto는 요소가 콘텐츠의 자연적인 크기를 받아들일 수 있도록 합니다. 그러나 초기 공간 배정을 안내하기 위해 특정 값을 설정할 수도 있습니다.

이 강력한 세트의 구문은 다음과 같습니다:

```js
flex: <flex-grow> <flex-shrink> <flex-basis>;
```

# flex 의 마법 풀기: 1 🪄

<div class="content-ad"></div>

요소의 flex 속성을 1로 설정하는 것은 해당 요소가 자신의 flex 컨테이너 내에서 사용 가능한 공간의 동등한 비율을 갖게 된 것과 같습니다. 이는 "여러분, 이 공간을 공평하게 나누어 쓰도록 합시다!"라고 말하는 것과 같습니다. 이 과정이 어떻게 이루어지는지 살펴보겠습니다:

```js
flex: 1; /* 이는 flex: 1 1 0%; 와 동일합니다. */
```

- flex-grow: 1: 요소가 확장되어 사용 가능한 공간의 공정한 비율을 차지할 수 있도록 합니다.
- flex-shrink: 1: 요소가 공간이 부족해지면 다른 요소들과 함께 우아하게 줄어들어 레이아웃 내에서 조화를 유지합니다.
- flex-basis: 0%: 이는 공간이 분배되기 전에 요소가 내용을 기준으로 넘어서는 추가 공간을 요구하지 않도록 합니다.

# 실생활 응용: 내비게이션 바 심포니 🎶

<div class="content-ad"></div>

웹 사이트를 탐색하는 것은 기본 사용자 경험이며, 여기서 flex: 1이 정말 빛을 발합니다! 화면 크기와 관계없이 항목들이 우아하게 조정되고 동등하게 분배되는 내비게이션 바를 만들어보세요. 마법 같죠? 이를 실현해봅시다:

HTML 구조:

```js
<nav class="navbar">
  <a href="#">Link 1</a>
  <a href="#">Link 2</a>
  <a href="#">Link 3</a>
</nav>
```

CSS 스타일링:

<div class="content-ad"></div>

```js
.navbar {
  display: flex;
}

.navbar a {
  flex: 1;
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
}
```

각 링크의 flex 속성을 1로 설정함으로써, 네비게이션 바의 사용 가능한 너비를 조화롭게 공유하게 됩니다. 화면 크기가 데스크톱, 태블릿 및 모바일 사이를 왔다갔다 할 때, 링크들은 쉽게 적응하여 시각적으로 매력적이고 일관된 사용자 경험을 유지합니다.

# 다른 Flex 값 탐색하기 🧭

flex: 1은 유연성과 적응성 때문에 인기 있는 선택지이지만, 다른 값들을 이해함으로써 레이아웃을 더 세밀하게 조정할 수 있습니다:


<div class="content-ad"></div>

- flex: 0: 이 설정은 요소의 내용이나 정의된 너비를 제어합니다. 남은 공간을 자동으로 채우지 않습니다. 이것은 "자신의 차선을 유지하는" 방식으로 생각해보세요.
- flex: none: 이 값은 요소의 크기를 잠그고 성장하거나 축소되지 못하게 합니다. 특히 특정 크기를 유지해야 할 때 매우 편리합니다. 이것은 "양보하지 않는" 옵션으로 생각할 수 있습니다.

# 결론 🎉

flex 속성과 해당 하위 속성을 숙달하면 웹 디자인에서 창의적인 가능성이 열립니다. flex-grow, flex-shrink 및 flex-basis의 상호작용을 이해하면 동적이고 반응형이며 시각적으로 매력적인 레이아웃을 만들 수 있는 도구를 갖게 됩니다. 계속 실험하고 창의적인 아이디어를 살펴보며 빈틈없는 유연성과 우아함을 갖춘 웹 디자인이 어떻게 현실로 이루어지는지 지켜보세요!

# 간단히 얘기하면 🚀

<div class="content-ad"></div>

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

- 작가를 클랩하고 팔로우해주세요 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 만나보세요