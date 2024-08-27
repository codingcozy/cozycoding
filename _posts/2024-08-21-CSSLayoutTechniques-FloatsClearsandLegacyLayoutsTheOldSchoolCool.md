---
title: "CSS 레이아웃 기법 - float, clear 및 레거시 레이아웃 배치 방법"
description: ""
coverImage: "/assets/img/2024-08-21-CSSLayoutTechniques-FloatsClearsandLegacyLayoutsTheOldSchoolCool_0.png"
date: 2024-08-21 18:59
ogImage: 
  url: /assets/img/2024-08-21-CSSLayoutTechniques-FloatsClearsandLegacyLayoutsTheOldSchoolCool_0.png
tag: Tech
originalTitle: "CSS Layout Techniques - Floats, Clears, and Legacy Layouts The Old School Cool"
link: "https://dev.to/gdebojyoti/css-layout-techniques-floats-clears-and-legacy-layouts-the-old-school-cool-2apj"
isUpdated: true
updatedAt: 1724246047608
---


과거의 추억 속으로 환영합니다! Flexbox와 Grid가 근대 웹 디자인의 슈퍼히어로처럼 등장하기 전에는, float와 clear가 CSS 우주를 지배했던 시대가 있었습니다. 웹 디자인 초보자라면, "float와 clear가 무엇이며, 왜 중요한가요?"라고 궁금해할 수 있습니다. 우리는 CSS 역사 속에서 향수로운 여행을 떠날 것이니, 함께 준비하세요.

스포일러: 오늘 사용할만한 멋진 트릭을 발견할 수도 있을 거에요!

![이미지](/assets/img/2024-08-21-CSSLayoutTechniques-FloatsClearsandLegacyLayoutsTheOldSchoolCool_0.png)

## Floats: 최초의 레이아웃 요술사

<div class="content-ad"></div>

CSS 세계의 반항아로서 float를 상상해보세요. 항상 줄 밖으로 벗어나 조금의 혼란을 일으키죠. float는 원래 이미지 주변에 텍스트를 감싸기 위해 설계되었지만, 뛰어난 개발자들은 곧 그것을 레이아웃을 만들기 위해 사용할 수 있다는 것을 깨달았어요.

```js
img {
    float: left;
    margin-right: 15px;
}
```

위의 예시에서 float: left;는 이미지를 왼쪽에 "띄우기"하여 텍스트가 그 주위로 감싸지도록 해줍니다. Flexbox와 Grid가 등장하기 전에는 다중 열 레이아웃을 만드는 인기 있는 선택지였어요. 그러나 조심히 사용하지 않으면 레이아웃 문제를 일으킬 수 있기 때문에 조금 까다로울 수도 있어요.

## Clears: 평화를 지키는 자들

<div class="content-ad"></div>

플롯이 조금 방심스러웠을 수도 있지만, 클리어는 그들의 평화 수호자였다. 클리어 속성은 플롯이 만들 수 있는 혼돈을 해결하기 위해 개입하는 중재자와 같다. 만약 플롯을 사용하고 있는데 요소들이 서로 겹치거나 예상과 다르게 동작한다면, 클리어가 도움이 될 수 있어.

```css
.clearfix::after {
    content: "";
    display: table;
    clear: both;
}
```

위의 CSS를 포함한 clearfix 클래스를 추가하면, 부동 요소들이 부모 컨테이너 내에 포함되도록 보장할 수 있다. 이것은 요소들이 실제로 음산해져 버리는 번거로운 레이아웃 결함을 방지하는 놀라운 방법이야.

## 과거의 레이아웃 기술: 레트로 부활

<div class="content-ad"></div>

플렉스박스와 그리드가 주목을 받기 전 CSS에는 몇 가지 더 트릭이 있었어요. 여기 몇 가지 클래식 레이아웃 기술이 있습니다:

- Inline-Block: 수평 레이아웃을 만드는 간단한 방법입니다. 요소를 display: inline-block;으로 설정하여 부유(float) 기능 없이도 옆으로 나란히 배열할 수 있어요.

```js
.box {
    display: inline-block;
    width: 30%;
    margin-right: 2%;
}
```

- 테이블 레이아웃: 네, 테이블은 표 데이터만을 위한 것이 아니에요! display: table;, display: table-row;, display: table-cell;을 사용하여 그리드 형식의 레이아웃을 만들 수 있어요.

<div class="content-ad"></div>

```js
.container {
    display: table;
    width: 100%;
}

.item {
    display: table-cell;
    width: 33%;
}
```

이러한 방법들은 현대 레이아웃 기술에 크게 가려지기는 했지만, 웹 디자인의 진화를 살펴볼 수 있는 기회를 제공합니다.

## 현대적인 응용: 옛 것과 새 것의 섞임

그래서, 현대 웹 디자인 세계에서는 여전히 플롯과 클리어가 자리를 차지하고 있을까요? 절대로요! Flexbox와 Grid가 현재 레이아웃을 만드는 데 주로 사용되는 도구이지만, 플롯과 클리어는 간단한 레이아웃이나 레거시 코드와 작업할 때와 같이 특정 시나리오에서 여전히 유용할 수 있습니다.

<div class="content-ad"></div>

예를 들어 텍스트 래핑이 필요할 때나 몇 가지 요소를 간단하게 정렬하고 싶을 때에는 floats를 사용할 수 있습니다. 그러나 복잡하고 반응형 레이아웃을 만들고 싶다면, Flexbox와 Grid가 여러분의 가장 친한 친구가 될 것입니다.

## 마무리하며

Floats, clears 및 다른 기존 레이아웃 기술은 과거의 유물처럼 보일 수 있지만, 이것들은 CSS의 풍부한 역사의 일부입니다. 이를 이해하면 탄탄한 기반을 갖고 현대적인 레이아웃 시스템의 힘을 이해할 수 있습니다. 게다가, 이런 구시대적인 트릭들을 알고 있다면 늙은 프로젝트나 엉뚱한 디자인 도전 과제(또는 끔찍한 면접 과정)과 상대할 때 도움이 될 수 있습니다.

즐거운 코딩 되세요!