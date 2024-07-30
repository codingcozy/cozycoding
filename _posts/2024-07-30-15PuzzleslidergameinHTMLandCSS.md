---
title: "HTML과 CSS로 만드는 15 퍼즐 슬라이더 게임 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-30 16:36
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "15 Puzzle slider game in HTML and CSS"
link: "https://dev.to/alohci/15-puzzle-slider-game-in-html-and-css-5g2i"
---


퍼즐로 넘어가세요

얼마 전, 어린 시절 회상으로 내 머릿속에 작은 퍼즐 장난감이 떠올랐어요. 15개의 사각 타일이 하나의 빈 공간을 남기고 프레임에 4 x 4 형식으로 배치된 슬라이더 퍼즐이었습니다. 각 타일과 프레임의 가장자리에 있는 곡선과 홈이 타일이 프레임 안에서 서로 슬라이딩하도록 하고 타일을 고정하는 역할을 합니다. 언제든지 빈 공간과 인접한 타일이 해당 공간으로 이동할 수 있고, 그 외에는 타일이 움직이지 못하도록 막혀 있습니다. 타일을 빈 공간으로 이동시키면 그 타일이 먼저 있던 자리에 새로운 빈 공간이 생기고, 그 자리에 다른 타일이 이동할 수 있게 됩니다. 이 방식으로 타일을 계속 슬라이딩하여 타일을 미리 정해둔 순서로 정렬하는 것이 아이디어입니다.

이것이 바로 "15 퍼즐"이라고 불리며 1870년대부터 존재해왔다고 합니다. 웹을 검색하면 다양한 프로그래밍 언어로 작성된 다수의 재현본이 나타나는데, 여기 dev.to에도 여러 기사가 있습니다. JavaScript로 작성된 https://dev.to/artydev/let-us-code-a-sliding-puzzle-9n, https://dev.to/xzanderzone/making-a-slider-puzzle-in-java-script-83m, 및 https://dev.to/claurcia/slide-puzzle-5c55. 그리고 Go로 작성된 https://dev.to/mfbmina/building-a-sliding-puzzle-with-go-3bnj도 있어요. 또 JavaScript를 배우는 사람들에게 좋은 초보 도전 과제로 소개되기도 합니다.

저의 관심을 끈 것은 프로그래밍 언어를 전혀 사용하지 않고 웹 상에서 다시 만들 수 있을 것이라는 생각이었습니다! 즉, 순수 HTML과 CSS만을 사용한 구현입니다. 아래에서 이를 소개하겠습니다. 제가 한 유일한 타협점은 제공된 10개의 게임이 미리 섞인 시작 위치로 고정되어 있다는 점입니다.

<div class="content-ad"></div>

이를 위해, 미리 정해진 순서대로 완성된 이미지를 보여주는 것입니다.

이 구현의 기본 원리는 각 타일이 프레임 내에서의 위치 정보를 상태 기록으로 유지한다는 것입니다. HTML 및 CSS에서 상태를 변경하고 유지하는 방법은 많지 않지만, 가장 일반적인 방법은 "체크박스 해킹"이며, 이 구현은 그것을 많이 활용합니다. "체크박스 해킹"을 잘 모르시는 분들을 위해 설명하자면, `label` 요소를 클릭하거나 탭할 때 연관된 폼 컨트롤의 활성화 동작이 발생한다는 것입니다. 연관된 컨트롤이 체크박스인 경우, 체크박스의 상태가 토글되어 선택되지 않은 상태에서 선택된 상태로, 선택된 상태에서 선택되지 않은 상태로 변경됩니다. 마찬가지로, 연관된 컨트롤이 라디오 버튼인 경우 해당 라디오 버튼이 선택되며, 같은 그룹 내의 다른 라디오 버튼이 선택되어 있는 경우 선택이 해제됩니다. 그런 다음 CSS 선택기를 사용하여 컨트롤의 선택 여부를 `:checked` 가상 클래스를 통해 측정하고 그 상태에 따라 다른 요소에 대해 다른 CSS를 적용할 수 있습니다.

따라서 각 타일은 4개의 라디오 버튼이 있는 라디오 버튼 그룹 두 개를 가지고 있습니다. 이 그룹 중 하나는 X축에서 타일의 위치를 유지하고, 다른 하나는 Y축에서 타일의 위치를 유지합니다. (또는 수평 위치와 수직 위치를 선호하는 경우 각각) 15개의 타일은 초기에 라디오 버튼을 통해 서로 다른 X 및 Y 좌표 조합이 주어져서 각 타일이 프레임 내에서 다른 셀을 차지하도록 합니다.

타일은 초기에 프레임의 상단 왼쪽 셀에 배치되며, 그 후에 CSS를 통해 라디오 버튼의 상태를 측정하고, 이에 따라 그들에게 이동 변환을 적용하여 프레임 내에서 이동합니다:
```js
/* "X"는 X축 셀 위치를 나타내며, "Y"는 Y축 셀 위치를 나타냅니다.
 * 0, 1, 2, 3은 해당 축의 위치를 나타내며,
 * 0 = 좌측 상단, 3 = 우측 하단을 의미합니다.
 */

.tile:has(.X0:checked~.Y0:checked) {
     transform: translate(0%, 0%);
}
.tile:has(.X0:checked~.Y1:checked) {
     transform: translate(0%, 100%);
}
.tile:has(.X0:checked~.Y2:checked) {
     transform: translate(0%, 200%);
}
.tile:has(.X0:checked~.Y3:checked) {
     transform: translate(0%, 300%);
}
.tile:has(.X1:checked~.Y0:checked) {
     transform: translate(100%, 0%);
}
/* 나머지 열여섯 가지 조합에 대해서도 비슷하게 반복 */
```

<div class="content-ad"></div>

제목에는 8개의 라디오 버튼에 해당하는 8개의 레이블 요소가 포함되어 있습니다. 각 레이블은 절대 위치로 겹쳐지고 각각이 타일을 완전히 가리도록 설정됩니다. 레이블은 투명하며, 처음에는 모두에게 pointer-events:none을 설정하여 클릭 및 탭에 응답하지 않도록 설정됩니다.

다음 단계는 CSS 셀렉터가 빈 셀이 어디에 있는지 식별하는 것입니다. 이는 제거를 통해 수행됩니다. X, Y 좌표가 15개의 타일 중 하나의 라디오 버튼 그룹 쌍에 의해 나타내지지 않는 셀입니다.

예를 들어, 다음과 같이 매칭되는 경우:

```js
.frame:not(:has(.tile .X0:checked~.Y0:checked)) { .... }
```

<div class="content-ad"></div>

그 후, 빈 셀이 현재 왼쪽 상단 셀에 있어야 합니다. 이를 16개의 셀 각각에 반복하면 정확히 일치하는 하나의 셀이 있을 것입니다.

그 다음 비어 있는 셀의 옆에 있는 셀을 확인할 수 있습니다. 비어 있는 셀이 모서리에 있는 경우 해당 셀로 이동할 수 있는 타일은 정확히 두 개이고, 그 외 프레임의 측면 중 하나에 비어 있는 셀이 있는 경우 해당 셀로 이동할 수 있는 타일은 세 개입니다. 그렇지 않으면 비어 있는 셀은 네 가지 가운데 셀 중 하나여야하며 그 셀로 이동할 수 있는 타일은 네 개입니다. 이러한 타일 각각에 대해 타일의 여덟 레이블 중 정확히 하나가 해당 타일을 비어 있는 셀로 이동해야 하는 올바른 라디오 버튼을 활성화합니다. 이 라벨은 포인터 이벤트 값을 다시 자동으로 설정하여 활성화됩니다. 예시는 다음과 같습니다:

```js
/* 맨 위, 왼쪽 모퉁이 */
.frame:not(:has(.tile .X0:checked ~ .Y0:checked)) {
  :is(
      .tile:has(.X0:checked ~ .Y1:checked) label.Y0,
      .tile:has(.X1:checked ~ .Y0:checked) label.X0
    ) {
    pointer-events: auto;
  }
}

/* 두 번째 행의 가장 오른쪽 세 번째 셀 */
.frame:not(:has(.tile .X1:checked ~ .Y3:checked)) {
  :is(
      .tile:has(.X1:checked ~ .Y2:checked) label.Y3,
      .tile:has(.X0:checked ~ .Y3:checked) label.X1,
      .tile:has(.X2:checked ~ .Y3:checked) label.X1
    ) {
    pointer-events: auto;
  }
}

/* 세 번째 행에서 왼쪽에서 두 번째 셀 */
.frame:not(:has(.tile .X2:checked ~ .Y1:checked)) {
  :is(
      .tile:has(.X2:checked ~ .Y0:checked) label.Y1,
      .tile:has(.X2:checked ~ .Y2:checked) label.Y1,
      .tile:has(.X1:checked ~ .Y1:checked) label.X2,
      .tile:has(.X3:checked ~ .Y1:checked) label.X2
    ) {
    pointer-events: auto;
  }
}
```

게임의 마지막 단계는 퍼즐이 해결되었을 때를 식별하는 것입니다. 이는 15개의 타일이 모두 X 축과 Y 축 라디오 버튼이 "해결" 위치로 설정되어 있는지 확인하는 것으로 간단히 할 수 있습니다.

<div class="content-ad"></div>

```js
/* 각 타일은 "a"부터 "o"까지의 문자가 할당됩니다. 
 * 퍼즐은 타일이 알파벳 순서로 정렬되어
 * 왼쪽에서 오른쪽, 위에서 아래로 읽을 때 풀립니다
*/
.frame:has(.a .X0:checked ~ .Y0:checked):has(.b .X1:checked ~ .Y0:checked):has(
    .c .X2:checked ~ .Y0:checked
  ):has(.d .X3:checked ~ .Y0:checked):has(.e .X0:checked ~ .Y1:checked):has(
    .f .X1:checked ~ .Y1:checked
  ):has(.g .X2:checked ~ .Y1:checked):has(.h .X3:checked ~ .Y1:checked):has(
    .i .X0:checked ~ .Y2:checked
  ):has(.j .X1:checked ~ .Y2:checked):has(.k .X2:checked ~ .Y2:checked):has(
    .l .X3:checked ~ .Y2:checked
  ):has(.m .X0:checked ~ .Y3:checked):has(.n .X1:checked ~ .Y3:checked):has(
    .o .X2:checked ~ .Y3:checked
  )
  ~ .options
  .success {
  display: block;
}
```

나머지는 장식입니다. 슬라이딩은 위에 설명된 transform의 간단한 전환으로 이루어집니다

```js
.tile {
  transition: 0.5s transform;
  @media (prefers-reduced-motion) {
    transition: none;
  }
}
```

그리고 각 타일은 배경 크기 및 배경 위치를 사용하여 게임 이미지의 일부를 보여줍니다


<div class="content-ad"></div>

```js
.tile {
  background-size: 400%;
}
#board1 .tile {
  background-image: url("https://alohci.net/image/dev.to/slidergame/mullermarc-k7bQqdUf954-unsplash.webp");
}
.a {
  background-position: 0% 0%;
}
.b {
  background-position: 33.333% 0%;
}
.c {
  background-position: 66.667% 0%;
}
.d {
  background-position: 100% 0%;
}
.e {
  background-position: 0% 33.333%;
}
/* 남은 타일들도 마찬가지로 */
```

그리고 여러분이 플레이할 게임을 선택할 단일 세트의 라디오 버튼이 있습니다.

게임을 플레이하려면 빈 셀로 슬라이드하려는 타일을 간단히 클릭하거나 탭하세요.

또한 HTML 및 CSS 작동 방식을 이해하는 데 도움이 될 수 있는 타일 문자와 라디오 버튼을 표시하는 "최소한" 모드를 제공했습니다.

<div class="content-ad"></div>

여기에 완성된 퍼즐 게임이 있어요. 피드백이 있으면 언제든 알려주세요!