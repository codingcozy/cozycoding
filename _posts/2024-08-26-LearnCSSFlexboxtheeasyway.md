---
title: "CSS Flexbox를 가장 쉽게 배우는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_0.png"
date: 2024-08-26 17:54
ogImage: 
  url: /assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_0.png
tag: Tech
originalTitle: "Learn CSS Flexbox, the easy way"
link: "https://medium.com/gitconnected/learn-css-flexbox-the-easy-way-27bd01922040"
isUpdated: true
updatedAt: 1724742826273
---


<img src="/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_0.png" />

오랜 시간 동안 CSS 레이아웃을 만드는 데 사용할 수있는 유일한 도구는 float 및 positioning과 같은 기능이었습니다.

그러나 일부 레이아웃 디자인은 다음과 같은 도구로 어렵게 달성되었습니다:

- 부모 요소 내에 콘텐츠 블록을 수직 가운데 정렬하기
- 컨테이너의 모든 자식 요소가 사용 가능한 너비 또는 높이의 동일한 양을 차지하도록 만들기
- 서로 다른 양의 내용을 포함하고 있더라도 모든 열이 동일한 높이를 채택하도록 만들기

<div class="content-ad"></div>


![Flexbox](/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_1.png)

이 게시물을 보면, 플렉스박스가 이러한 레이아웃 작업을 훨씬 쉽게 만든다는 것을 알 수 있을 거에요.

## Display: Flex: The Switch

좋아요, 컨테이너 `div`안에 몇 가지 요소가 있다고 상상해보세요:


<div class="content-ad"></div>

```js
<div class="container">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
</div>
```

Flexbox 레이아웃을 활성화하려면 컨테이너 div에 display: flex를 추가하면 됩니다.

```js
.container {
    display: flex;
}
```

<img src="/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_2.png" />


<div class="content-ad"></div>

그렇게 해서 당신은 보이지 않는 두 개의 축을 만들었습니다:

- Main Axis: 여기서 요소들이 주로 정렬됩니다. (기본적으로 수평행)
- Cross Axis: 이것은 보조 방향으로, 주 축에 대해 90도 방향에 있습니다. (기본적으로 수직 열을 생각하십시오)

## 플렉스 방향: 흐름 변경

이 축들의 흐름을 변경하는 또 다른 속성이 있습니다 - flex-direction이며 기본적으로 row로 설정되어 있습니다. 이것은 항목이 왼쪽에서 오른쪽으로 수평으로 흐른다는 것을 의미합니다.

<div class="content-ad"></div>

하지만 요소들이 수직으로 쌓이기를 원한다면 flex-direction 속성을 column으로 설정할 수 있어요.

```js
.container {
    display: flex;
    flex-direction: column;
}
```

<img src="/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_3.png" />

하지만 알다시피, Flexbox는 유연합니다. 어느 축이든 순서를 반대로도 바꿀 수 있어요.

<div class="content-ad"></div>

- column-reverse: 요소가 아래에서 위로 수직적으로 흐릅니다.
- row-reverse: 요소가 오른쪽에서 왼쪽으로 수평적으로 흐릅니다.

## Justify Content: 주축 조절하기

주축을 따라 요소를 정렬하기 위해 justify-content 속성을 사용합니다.

기본적으로 이는 flex-start로 설정되어 있으며, 요소들은 주축의 시작 부분에서 모여 있습니다.

<div class="content-ad"></div>

```js
.container {
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
}
```

다음은 이 속성에 사용할 수 있는 잠재적인 값입니다:

![flexbox](/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_4.png)

- flex-end: 항목이 주축 끝에 정렬됩니다.
- center: 항목이 공손하게 중앙에 모인다.
- space-between: 항목이 처음 항목이 시작 부분에 닿고 마지막 항목이 끝 부분에 닿게 균등하게 펼쳐집니다.
- space-around: space-between과 유사하지만 각 항목 주변에 동일한 공간이있는 공간이 있습니다 (가장자리 포함).
- space-evenly: 각 항목이 모든 측면에 정확히 동일한 양의 공간을 얻습니다.

<div class="content-ad"></div>

## 항목 정렬: 교차 축 제어

교차 축을 제어하기 위해서는 align-items 속성을 사용합니다.

align-items의 기본값은 stretch이며, 이는 항목들이 교차 축을 채우기 위해 늘어나도록합니다.

```js
.container {
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: stretch;
}
```

<div class="content-ad"></div>

<img src="/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_5.png" />

- flex-start: 아이템은 교차 축의 시작점에 정렬됩니다.
- flex-end: 아이템은 교차 축의 끝에 정렬됩니다.
- center: 아이템은 교차 축의 중앙에 정렬됩니다.
- baseline: 아이템은 텍스트 기준선을 기준으로 정렬됩니다.

핵심 포인트 — 축을 기억하세요: 이해해야 할 가장 중요한 것은 justify-content가 주축을, align-items가 교차 축을 제어한다는 것입니다. 이 개념을 이해하면 Flexbox가 매우 쉬워집니다!

## Gap 속성

<div class="content-ad"></div>

플렉스박스 레이아웃을 사용하면 요소에 수동으로 여백을 추가할 필요가 없어요. 갭 속성을 사용하면 항목 간의 간격을 제어하여 모든 요소 사이에 즉시 간격을 적용할 수 있어요.

```css
.container {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    gap: 30px;
}
```

<img src="/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_6.png" />

## 플렉스 랩: 갈림길 피하기

<div class="content-ad"></div>

기본적으로 플렉스 아이템들은 한 줄로 뭉쳐집니다. 그러나 flex-wrap: wrap을 사용하면 우아하게 새 줄로 흐를 수 있어요.

```js
.container {
    display: flex;
    flex-wrap: wrap;
}
```

<img src="/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_7.png" />

- nowrap (기본): 모든 아이템이 한 줄에 남아 있습니다.
- wrap: 필요한 경우 아이템이 여러 줄로 분리됩니다.
- wrap-reverse: 아이템이 역순으로 여러 줄로 분리됩니다.

<div class="content-ad"></div>

## 콘텐츠 정렬: 감싼 행의 간격 제어하기

flex-wrap를 wrap으로 설정하면 새로운 속성인 align-content를 사용할 수 있습니다. 이를 통해 감싼 행의 간격을 제어할 수 있습니다.

안에 든 행에 대해 justify-content와 비슷하게 작동하지만 여러 행에 적용됩니다:

![image](/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_8.png)

<div class="content-ad"></div>

- flex-start: 줄이 시작 부분으로 정렬됩니다.
- flex-end: 줄이 끝 부분으로 정렬됩니다.
- center: 줄이 가운데로 정렬됩니다.
- space-between: 간격이 같게 해서 줄이 퍼져 나갑니다.
- space-around: 간격을 동일하게 유지하여 줄이 퍼져 나갑니다.
- stretch (기본값): 줄이 컨테이너를 채우도록 늘어납니다.

## 개별 항목을 위한 Flexbox 속성

Flexbox는 컨테이너에서 끝나지 않습니다. 우리는 개별 항목에도 스타일을 적용할 수 있습니다:

align-self 도구를 사용하여 특정 항목에 대한 align-items 설정을 무시할 수 있습니다.

<div class="content-ad"></div>

일부 요소를 가운데 정렬하고 다른 요소들은 상단에 위치하도록 하려면 다음과 같이 작성하세요.

```css
.item:nth-child(3) {
    align-self: center;
}
```

<img src="/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_9.png" />

다른 가능한 값들은 아래와 같습니다.

<div class="content-ad"></div>

- auto (기본값): 부모 컨테이너의 align-items 값을 상속합니다.
- flex-start: 교차 축의 시작 부분에 정렬합니다.
- flex-end: 교차 축의 끝 부분에 정렬합니다.
- center: 항목을 교차 축의 가운데에 정렬합니다.
- baseline: 항목의 기준선을 다른 기준선과 맞춥니다.
- stretch: 항목을 교차 축을 채우도록 늘립니다 (높이가 설정되지 않은 경우).

## Flex Grow: 늘기 원하는 정도

이것을 항목이 성장하기 원하는 정도로 생각해보세요. 이는 항목이 형제 항목들에 비해 얼마나 추가 공간을 차지해야 하는지를 제어합니다. 숫자가 높을수록 추가 공간을 더 많이 차지할 것입니다.

예를 들어:

<div class="content-ad"></div>

```js
.item:nth-child(1) {
    flex-grow: 0; // 기본 값
}
.item:nth-child(2) {
    flex-grow: 1;
}
.item:nth-child(3) {
    flex-grow: 2;
}
```

<img src="/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_10.png" />

이 경우, 1번 아이템은 전혀 늘어나지 않습니다. 2번 아이템은 사용 가능한 공간의 동등한 부분을 차지하게 되고, 3번 아이템은 2번 아이템보다 사용 가능한 공간의 두 배를 차지하게 됩니다.

## Flex Shrink: 축소 용의성

<div class="content-ad"></div>

아이템의 축소 의향과 관련된 것이에요. 공간이 부족할 때 아이템이 얼마나 축소될지를 제어해요. 숫자가 높을수록 더 쉽게 축소돼요.

- flex-shrink: 1 (기본값): 필요하다면 아이템이 비례해서 축소돼요.
- flex-shrink: 0: 아이템이 얼마나 overflow가 발생하더라도 축소하려 하지 않아요.

예를 들어, 첫 번째와 마지막 아이템을 축소하지 않도록 설정할 수 있어요:

```js
.item:nth-child(1) {
    flex-shrink: 0;
}

.item:nth-child(6) {
    flex-shrink: 0;
}
```

<div class="content-ad"></div>


![Flex Basis](/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_11.png)

## Flex Basis

이 설정은 여분의 공간이 분배되기 전에 항목의 초기 크기를 설정합니다. 항목의 시작점으로 생각할 수 있습니다.

```js
.item:nth-child(4) {
    flex-basis: 50%;
}
```

<div class="content-ad"></div>


![Flexbox](/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_12.png)

특정 길이(예: 200px 또는 30%) 또는 auto(아이템의 컨텐츠 크기 사용)나 content(컨텐츠의 최소 크기 사용)와 같은 키워드로 설정할 수 있습니다.

## 보너스 팁: Flex Shorthand

flex-grow, flex-shrink 및 flex-basis를 별도로 작성하는 대신 매우 편리한 flex shorthand 속성을 사용할 수 있습니다.


<div class="content-ad"></div>

위 단축어를 사용하면 동일한 효과를 얻을 수 있어요:

```js
.item:nth-child(4) {
    flex: 1 0 0;
}
```

단축어에서 값의 순서가 중요해요. 이 경우에는 flex-grow를 1로, flex-shrink를 0으로, 그리고 flex-basis를 0으로 설정해요.

## 순서

<div class="content-ad"></div>

그리고 마지막으로, 아이템의 시각적 순서를 변경하는 order 속성이 있습니다. 숫자를 취하며, 낮은 숫자일수록 먼저 나타납니다.

```js
.item:nth-child(1) {
    order: 1;
}
.item:nth-child(6) {
    order: -1;
}
```

<img src="/assets/img/2024-08-26-LearnCSSFlexboxtheeasyway_13.png" />

그러나 논리적인 순서를 변경하기 위해 order에만 의존하면 접근성에 영향을 줄 수 있습니다. 이는 스크린 리더 및 키보드 탐색이 소스 코드 순서를 따르기 때문입니다.

<div class="content-ad"></div>

여기 Flexbox 사용법을 정리한 무료 치트시트입니다.

만약 이 게시물이 도움이 되었다면, 무료 HTML 및 CSS 강좌도 확인해보세요.