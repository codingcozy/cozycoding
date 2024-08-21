---
title: "CSS starting-style로 다이얼로그와 팝오버 요소 애니메이션 주는 방법"
description: ""
coverImage: "/assets/img/2024-08-04-AnimatingdialogandpopoverelementswithCSSstarting-style_0.png"
date: 2024-08-04 19:48
ogImage: 
  url: /assets/img/2024-08-04-AnimatingdialogandpopoverelementswithCSSstarting-style_0.png
tag: Tech
originalTitle: "Animating dialog and popover elements with CSS starting-style"
link: "https://dev.to/logrocket/animating-dialog-and-popover-elements-with-css-starting-style-1a43"
isUpdated: true
updatedAt: 1724246255245
---


라훌 차드에 의해 작성되었어요✏️

네이티브 대화 상자 및 팝오버 요소는 현대 프런트엔드 웹 개발에서 각자의 명확한 역할을 가지고 있어요. 대화 상자 요소는 사용자와 소통하고 입력을 수집하는 데 사용되는 반면, 팝오버는 사용자에게 부차적인 낮은 우선순위의 정보를 제공하는 데 더 적절해요.

이전에 대화 상자 대 팝오버에 관한 기사에서 두 요소 모두 전용 JavaScript API를 가지고 있다는 것을 논의했었어요. 이를 통해 두 요소를 최대한 활용할 수 있어요.

이 요소들에 애니메이션과 상호작용을 추가할 때는 JavaScript 라이브러리가 CSS보다 선호돼요. 이러한 요소들은 일반적으로 최소한의 애니메이션을 요구하고, 몇 가지 간단한 효과를 추가하려고 거대한 애니메이션 라이브러리를 사용하는 것은 앱에 부당한 부하를 크게 증가시킬 수 있어요.

<div class="content-ad"></div>

이 문제를 해결하기 위해 이 기사에서는 순수 CSS 기술을 사용하여 대화 상자 및 팝오버에 애니메이션 효과를 코딩하는 데 도움이 될 것입니다. CSS 키프레임과 최근에 도입된 @starting-style at-rule에 대해 다룰 것인데, 이 두 기술은 섬세한 애니메이션을 만드는 데 사용될 수 있으며 성능이 향상됩니다.

## 오버레이 요소에 애니메이션 적용하는 문제

대화 상자와 팝오버는 오버레이 요소이므로 브라우저에 의해 렌더링된 가장 위쪽 레이어에서 작동합니다. 이전에 논의한 바와 같이, 이러한 요소들은 또한 표시 및/또는 모달리티를 관리하기 위해 전용 API에 의존합니다.

전통적인 CSS 전환 기술을 사용하여 대화 상자와 팝오버에 애니메이션을 적용할 때 직면하는 어려움을 살펴보겠습니다.

<div class="content-ad"></div>

### display 속성에 대한 의존성

일반적으로 CSS에서는 display와 같은 이산 속성을 한 값에서 다른 값으로 전환할 수 없습니다. 이는 표준 투명도 0 ~ 100퍼센트 접근 방식으로 전환을 생성하는 데 사용하는 방법도 작동하지 않는다는 것을 의미합니다. 왜냐하면 display 속성은 전환이 완료되기 전 값 전환 사이의 지연을 허용하지 않기 때문입니다.

![이미지](https://res.cloudinary.com/practicaldev/image/fetch/s--hVfAE0ja--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://blog.logrocket.com/wp-content/uploads/2024/07/dialog-popover-dynamic-display.gif%3Fw%3D895)

참고: DevTools에서 popover 요소의 계산된 display가 자동으로 업데이트되지 않는 이유가 있습니다. 업데이트된 값을 확인하려면 다른 노드를 선택한 다음 다시 popover 노드를 선택해야 합니다.

<div class="content-ad"></div>

위에 표시된 대로, 다이얼로그와 팝오버 요소의 화면 표시 여부는 CSS 표시 속성을 사용하여 브라우저 내에서 처리됩니다.

다음 데모는 다이얼로그와 팝오버 요소가 표시 속성에 종속되어 있어 표준 CSS 전환 방법이 그들에 대해 효과적이지 않음을 보여줍니다:

[CodePen 링크](https://codepen.io/_rahul) 에서 Opacity/Visibility transitions doesn't work with dialogs and popovers 을(를) 보실 수 있습니다.

전환을 만들기 위해 불투명도와 변형에 집중하기 전에, 해당 요소들의 화면에 표시되는 방식을 지배하는 표시 속성을 먼저 고려해야 합니다.

<div class="content-ad"></div>

### 렌더링 전 초기 스타일 부족

오버레이 요소에 대한 또 다른 문제는 초기 스타일의 부재입니다. 이는 DOM에 동적으로 추가되는 요소나 표시 속성을 동적으로 제어하는 요소의 적절한 전환을 보장하기 위해 중요합니다.

웹 페이지에 렌더링되는 동안 요소가 서서히 페이드 인되어야 한다고 가정해 봅시다. 이 경우 요소의 초기 불투명도를 영으로 설정하고, 그 후 웹 페이지에 완전히 렌더링될 때까지 100%로 전환해야 합니다. 보통 우리가 사용할 수 있는 초기 상태는 요소의 현재 상태뿐이며, 이 상태에 0의 불투명도가 지정된다면 화면에서 요소가 사라질 것입니다.

이것을 기능적인 효과로 변환하기 위해 JavaScript를 사용하여 프로그래밍 지연, 클래스 전환 또는 CSS 키프레임 애니메이션을 추가하여 전환 효과를 흉내 낼 수 있습니다.

<div class="content-ad"></div>

다음 섹션에서는 표시 속성이 전환을 지원하지 못하고 렌더링 전 초기 요소 스타일이 부족한 문제에 대해 탐구할 것입니다.

## 이산형 CSS 속성 전환

위에서 설명한대로 대화상자(Dialog) 및 팝오버(Popover) 요소는 화면에서 표시되기 위해 표시 속성에 의존하므로 CSS 전환을 사용하여 애니메이션화하는 것이 거의 불가능합니다.

표시 속성은 이산적인 성질을 가지고 있으며 값들 사이에서 급작스럽게 변경됩니다. 예를 들어, transition-duration에서 정의된 지연 시간을 고려하지 않고 블록에서 없음으로 변할 수 있습니다. 이는 투명도, 너비, 높이 및 기타 값을 추가로 허용하는 속성과 달리 중간 상태가 없기 때문입니다.

<div class="content-ad"></div>

CSS 전환과 호환되는 이산 속성을 만들기 위해, 특히 숫자 형식, 픽셀 또는 백분율로 첨가 값이 없는 이산 요소에 대해 전환을 어떻게 행동하게 할 것인지에 대해 지정할 수 있는 transition-behavior라는 새로운 전환 속성이 소개되었습니다.

값 사이를 부드럽게 전환하는 대신, allow-discrete 행동은 한 이산 값에서 다른 이산 값으로의 변경을 지정된 transition-duration이 경과할 때까지 연기합니다:

```js
.dynamic-display {
  transition: opacity 0.5s, translate ..., display 0.5s allow-discrete;
  ...
}
```

위 스니펫에서 allow-discrete 행동은 전환 기간으로 지정된 반초 동안 디스플레이 값이 갑자기 전환되는 대신 대기하도록 보장합니다.

<div class="content-ad"></div>

이러한 이산 값 전환 지연은 덧셈 값으로 다른 속성 전환에 충분한 시간을 제공하여 작업이 완료되도록합니다.

[Rahul의 allow-discrete transition-behavior를 사용한 예제 보기](https://codepen.io/_rahul/pen/)

'allow-discrete' 전활동을 통해 렌더링 또는 표시가 동적으로 관리되는 요소에 출구 전활동을 추가할 수 있다는 것을 알게 되었습니다. 그러나 미리 렌더링된 스타일이 없으면 후입 전활동은 작동하지 않을 것입니다. 다음 몇 섹션에서는 입구 전활동을 추가하는 몇 가지 기술을 탐색할 것입니다.

## CSS keyframes로 대화 상자 및 팝 오버 애니메이션 설정하기

<div class="content-ad"></div>

지금까지 동적으로 추가하고 관리되는 요소에 종료 전환을 통합하는 방법을 배웠습니다. 이제 이 기술을 대화상자(dialogs)와 팝오버(popovers)에도 적용해 보겠습니다.

먼저 진입 및 종료 애니메이션을 모두 선언하고 CSS 키프레임(keyframes)이 어떻게 요소의 표시 여부에 관계없이 전환에 진입 지점을 어느 정도 추가하는 데 효과적일 수 있는지 살펴보겠습니다.

### CSS 키프레임을 사용하여 진입 애니메이션 추가

요소의 시작 스타일을 모방하기 위해 CSS 키프레임을 사용하는 것은 간단합니다. 대화상자(dialog) 요소의 배경을 포함하여 요소 및 배경에 대한 진입 및 종료 애니메이션을 추가하는 방법부터 시작해 보겠습니다.

<div class="content-ad"></div>

위의 표에 있는 태그를 Markdown 형식으로 변경해보세요.

<div class="content-ad"></div>

위의 코드 조각을 결합하여 대화 상자(dialog)와 팝오버(popover) 요소에 사용하면 아래에서 공유된 데모와 유사한 결과물이 나옵니다. 이 기술은 입장 애니메이션에 좋지만 출장 애니메이션 부분은 완전히 건너뛴다는 것을 알 수 있습니다:

[Rahul(@_rahul)의 CodePen에서 CSS 키프레임을 사용한 HTML5 대화 상자(dialog) 및 팝오버(popover) 입장 애니메이션 확인하기](https://codepen.io/_rahul/pen/)

마이크로 상호 작용에 대한 감각이 예민하다면, 대화 상자를 열 때 희미하게 나타나는 입장 애니메이션이 잘 작동함을 알 수 있지만 닫거나 취소할 때 희미하게 사라지는 퇴장 애니메이션은 제대로 작동하지 않는 것을 알 수 있습니다. 다음 섹션에서 그 이유에 대해 알아봅시다.

### 대화 상자(dialog)와 팝오버(popover)에 퇴장 애니메이션 추가하기

<div class="content-ad"></div>

위 데모에서 종료 애니메이션이 작동하지 않는 이유는 팝오버와 대화상자 API로 인해 요소의 계산된 표시가 갑자기 변경되기 때문입니다. 이전에 우리는 transition-behavior 속성이 이산 CSS 속성을 전환과 함께 관리하는 데 어떻게 도움이 되는지에 대해 논의했습니다. 이 시나리오에서 transition-behavior 속성을 사용하여 문제를 해결할 수 있는지 확인해봅시다.

[CodePen에서 Rahul에 의해 작성된 HTML5 Dialog 및 팝오버 입/퇴장 애니메이션 확인하기](https://codepen.io/_rahul/pen/)

다행히도 display 및 overlay 속성에 allow-discrete 동작을 추가하는 것으로 종료 애니메이션 문제를 해결했습니다. 입장 및 퇴장 애니메이션 모두 올바르게 작동합니다.

실제로 이 방법은 각 애니메이션 선언 블록마다 두 개에서 세 개의 공급업체별 변형이 포함된 훨씬 더 큰 코드로 이어집니다. 우리가 여기서 구현한 효과는 복잡하지 않으며 대화상자와 팝오버 요소가 아니었다면 CSS 전환을 사용하여 달성할 수 있었을 것입니다.

<div class="content-ad"></div>

CSS 키프레임은 키프레임 애니메이션을 만드는 데 최적화되어 있지만, 사전 렌더 최적화는 제공하지 않습니다. 그러나 최신에 소개된 @starting-style at-rule을 사용하면 CSS 키프레임 애니메이션 없이 전환 효과를 구현할 수 있습니다.

## CSS의 @starting-style at-rule은 무엇인가요?

이전에 DOM 종속 요소가 전환할 초기 스타일의 시작 지점을 요구하는 방법을 논의했었는데, 새로운 @starting-style CSS at-rule이 정확히 그 역할을 해내줍니다.

@starting-style at-rule은 전환 요소에 대한 속성의 시작 값을 선언하는 데 사용되는 CSS Transition Level 2 기능으로, 첫 번째 스타일 업데이트부터 시작합니다.

<div class="content-ad"></div>

아래 구문은 전환이 시작하고 작동할 요소의 스타일의 시작점을 지정할 수 있도록 합니다. 이 at-rule의 셀렉터 내에 포함된 속성들은 연결된 전환에 관여하게 될 속성들이어야 합니다:

```js
@starting-style {
  .셀렉터 {
    불투명도: 0;
    ...
  }
}
```

아래 데모에서 트리거 버튼을 눌러 요소를 동적으로 다시 렌더링해 보세요. 알아두세요, 연결된 요소들이 렌더링되기 전에 @starting-style을 사용해 전환의 시작점을 만드는 것이 얼마나 간단한지를 확인할 수 있습니다:

CodePen의 Rahul(@_rahul)가 제작한 HTML5 대화 상자와 팝오버 입구 및 출구 애니메이션 w/ CSS 키프레임을 확인해보세요.

<div class="content-ad"></div>

@starting-style 기능은 대형 웹 브라우저에서 견고한 지원을 받을 것으로 예상되며 현재 Chromium 및 Webkit 기반 브라우저에서 잘 지원됩니다. 최신 지원 사항은 여기에서 확인할 수 있습니다.

## @starting-style를 사용한 대화상자 및 팝오버 전환

위의 패턴을 따라서, allow-discrete 전환 동작과 @starting-style을 사용하여 대화상자 및 팝오버 요소에 서서히 나타나는 애니메이션을 추가할 수 있습니다.

계속하기 전에, 먼저 디스플레이 및 오버레이 속성의 전환에 allow-discrete 동작을 사용해야 합니다. 이는 셀렉터 내에서 transition-behavior 속성을 명시적으로 정의하거나 다른 전환과 함께 transition 속성에 결합하여 아래와 같이 표시할 수 있습니다:

<div class="content-ad"></div>

```js
.my-dialog,
.my-popover {
  transition: opacity 0.5s, translate 0.5s,
              overlay 0.5s allow-discrete,
              display 0.5s allow-discrete;
  &::backdrop {
    transition: background 0.5s,
                overlay 0.5s allow-discrete,
                display 0.5s allow-discrete;
  }
}
```

열린 상태의 초기 스타일을 처리하기 위해 @starting-style 블록을 추가하여 전환 효과를 담당하는 속성을 추가해야 합니다. 여기에는 이미 대화 상자와 팝오버 API에서 관리하고 있는 display 및 overlay 속성을 포함할 필요가 없습니다.

```js
@starting-style {
  .my-dialog[open],
  .my-popover:popover-open {
    opacity: 0;
    transform: translateY(-1em);
  }

  .my-dialog[open]::backdrop {
    background-color: hsl(0 0 0 / 0%);
  }
}
```

대화 상자와 팝오버를 사용하면 dialog[open] 및 :popover-open과 같은 특정 속성 및 가상 클래스를 사용하여 열린 상태를 대상으로 할 수 있습니다:

<div class="content-ad"></div>

마지막으로, 닫는 전환에 해당하는 스타일을 원본 요소에 할당할 수 있습니다. 이것은 기본적으로 닫힌 상태입니다. 다시 말하면, 다이얼로그 요소가 기본적으로 사라지고 위로 올라가도록 유지합니다:

```js
.my-dialog,
.my-popover {
  opacity: 0;
  translate: 0 -1em;
}

.my-dialog {
  &::backdrop {
    background-color: transparent;
  }
}
```

다음 데모는 전환에 allow-discrete 동작을 적용하고 @starting-style로 초기 스타일을 정의하며, 열린 상태와 닫힌 상태에 대한 스타일링을 지정한 결과를 반영합니다. 이제 다이얼로그와 팝오버 요소에 대해 입력 및 출력 애니메이션이 원활하게 작동하며, CSS 키프레임 대비 코드가 적게 사용되었습니다:

<div class="content-ad"></div>

위의 코드 펜에서는 HTML5 다이얼로그 및 팝오버의 등장 및 퇴장 애니메이션을 보실 수 있습니다. 

![다이얼로그 및 팝오버](/assets/img/2024-08-04-AnimatingdialogandpopoverelementswithCSSstarting-style_0.png)

## @starting-style로 고급 다이얼로그 애니메이션

다이얼로그 요소에 대해 다른 애니메이션을 구현하여 한 단계 더 나아가봅시다. 기본 원칙은 동일합니다: 변형 및 전환과 관련된 속성만 변경됩니다.

<div class="content-ad"></div>

### 회전하는 대화상자들

세련된 회전 대화상자를 만드는 아이디어는 투명도와 몇 가지 CSS 변형 속성을 활용하는 것입니다:

```js
.my-dialog {
  transition: opacity 0.5초, translate 0.5초, rotate 0.5초, 
              overlay 0.5초 allow-discrete,
              display 0.5초 allow-discrete;
}
```

대화상자의 열린 상태에 대한 시작 스타일은 다음과 같습니다:

<div class="content-ad"></div>

- 처음에 투명하게 만들기 위해 불투명도를 0%로 설정합니다.
- 대화 상자를 가운데에서 왼쪽으로 이동시키기 위해 x축 방향으로 -50%만큼 이동합니다.
- 대화 상자를 시야에 보이지 않는 위치로 이동시키기 위해 y축 방향으로 -100%만큼 이동합니다.
- 대화 상자가 열릴 때 양수 회전을 위해 -180도 회전합니다.

여기에 코드가 있습니다:

```js
@starting-style {
  .my-dialog[open] {
    opacity: 0;
    translate: -50% -100%;
    rotate: -180deg;
  }
}
```

닫힌 상태는 시작 스타일과 닮아 있지만, 대화 상자 요소를 나올 때 반대로 움직이도록 변환 및 회전을 수정한 것입니다:

<div class="content-ad"></div>

```js
.my-dialog {
  /* 이전 스타일 */
  opacity: 0;
  translate: 50% 100%;
  rotate: 180deg;
}
```

열린 상태의 대화 상자 요소는 불투명도가 100%이며, 어느 축에도 변위가 없으며 회전도 없이 화면의 중앙에 위치합니다:

```js
.my-dialog[open] {
  opacity: 1;
  translate: 0 0;
  rotate: 0deg;
}
```

최종 결과는 아래와 같이 보이게 됩니다:

<div class="content-ad"></div>

CodePen에서 Rahul님의 HTML5 대화상자 및 popover 회전 애니메이션을 확인해보세요.

### 바운스 인-아웃 대화상자

CSS 키프레임 애니메이션을 사용하지 않고 바운스 효과를 만들기 위해, 대화상자의 변형 및 불투명도 전환에 대한 transition-timing 함수로 Bezier 곡선을 활용할 수 있습니다. 이 효과를 위해 scale 변환을 사용할 것입니다.

베지어 곡선을 위한 다른 x1, y1, x2 및 y2 값을 실험해보세요. 그리고 여러분의 애니메이션 프로젝트에 구현해보세요!

<div class="content-ad"></div>

```js
.my-dialog {
  transition: opacity 0.4s cubic-bezier(0.4, 1.6, 0.4, 0.8),
              scale 0.4s cubic-bezier(0.4, 1.6, 0.4, 0.8), 
              overlay 0.4s allow-discrete,
              display 0.4s allow-discrete;
}
```

이제 시작 스타일과 열린 상태, 닫힌 상태를 쉽게 판단할 수 있습니다. 열린 상태와 닫힌 상태의 초기 스타일은 동일합니다 - 대화 상자는 영점으로 축소되고 완전히 투명해집니다.

열린 상태에서 대화 상자는 100% 불투명도를 가지고 크기가 1로 확대될 것입니다. 나머지 전환 효과는 베지어 곡선을 이용한 전환 효과로 처리될 것입니다:

```js
@starting-style {
  .my-dialog[open] {
    opacity: 0;
    scale: 0;
  }
}

.my-dialog {
  opacity: 0;
  scale: 0;

  &[open] {
    opacity: 1;
    scale: 1;
  }
}
```

<div class="content-ad"></div>

아래와 같이 작동합니다:

다음 링크에서 Rahul(@_rahul)이 만든 HTML5 대화상자와 popover의 바운싱 애니메이션을 볼 수 있습니다.

### 슬라이딩 및 바운싱 대화상자

이 애니메이션에서는 이전 효과와 달리 비슨 곡선을 사용하여 간단하게 유지하겠지만 효과는 다르게 나타납니다. 이전 효과에서 한 것처럼 다이얼로그를 확대하는 대신 y축을 따라 번역하는 것입니다:

<div class="content-ad"></div>

```js
.my-dialog {
  transition: 
    opacity 0.5s cubic-bezier(0.4, 1.6, 0.4, 0.8),
    translate 0.5s cubic-bezier(0.4, 1.6, 0.4, 0.8),
    overlay 0.5s allow-discrete, 
    display 0.5s allow-discrete;
}
```

다이얼로그를 처음에는 뷰포트의 y 축 상단에 유지하는 것이 아이디어입니다. 그런 다음, 다이얼로그를 열 때 0으로 변환한 다음, 마지막으로 축을 따라 아래로 이동합니다:

```js
@starting-style {
  .my-dialog[open] {
    opacity: 0;
    translate: 0 -200%;
  }
}

.my-dialog {
  opacity: 0;
  translate: 0 200%;

  &[open] {
    opacity: 1;
    translate: 0 0;
  }
}
```

100% 양수 또는 음수 번역을 적용하는 대신에, 다이얼로그 상자에서 긴급감을 표현하도록 두 배로 늘렸습니다. 아래에서 실제로 동작하는 것을 확인해보세요:

<div class="content-ad"></div>

위의 코드는 CodePen에서 Rahul(@_rahul)이 만든 HTML5 대화 상자 및 팝오버 슬라이드 업-다운 바운스 애니메이션을 보여줍니다.

## @starting-style와 함께 나타나는 서서히 사라지는 팝오버 애니메이션

위의 효과는 대화 상자 요소와 잘 어울리지만 팝오버 요소와는 잘 어울리지 않을 수 있습니다. 이 섹션에서는 팝오버가 팝오버로 보이고 그 이상으로는 보이지 않도록 하는 멋진 팝오버 애니메이션 효과에 중점을 둡니다.

### 떠오르고 가라앉는 팝오버

<div class="content-ad"></div>

이 효과는 우리가 처음에 만든 팝오버 효과와 비슷합니다. 그 예에서 팝오버는 뷰포트의 오른쪽 하단에 팝업이 예상할 수 있는 것과는 다르게 위에서 나타나며 y-축을 따라 슬라이드 및 페이드효과를 보였습니다.

이를 수정하여 시작 스타일과 닫힌 상태에서도 y-축의 이동을 추가하여 이를 보완해 보겠습니다. 그 외에는 모두 변경되지 않습니다:

CodePen에서 Rahul(@_rahul)님의 '위로 올라가고 내려가는 팝오버 애니메이션'을 확인해주세요.

### 커지고 줄어드는 팝오버

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경해보세요.

<div class="content-ad"></div>

이 기법은 알림 토스트에 애니메이션을 만드는 데 자주 사용됩니다. 이 효과를 얻기 위해선 팝오버 엘리먼트를 오른쪽으로 100% 이동하여 뷰포트 밖으로 이동시키면 됩니다. 그리고 열린 상태에서는 효과를 완성하기 위해 다시 0으로 이동시킬 수 있습니다.

CodePen에서 Rahul(@_rahul)님이 제공한 Slide in out from right popover animation을 확인해보세요.

## 결론

우리는 순수 CSS를 사용하여 대화 상자 및 팝오버 엘리먼트에 CSS 전환 기반 애니메이션을 통합하는 방법에 대해 배웠습니다. 우리는 오버레이 엘리먼트의 전통적인 전환의 복잡성과 문제점에 대해 논의했고, 그 후에 CSS keyframes 및 더 중요한 @starting-style at-rule을 사용하여 하나씩 이러한 문제를 해결했습니다.

<div class="content-ad"></div>

그러나 @starting-style 기능은 꽤 새롭고 아직 전역적으로 사용할 수는 없습니다. 따라서 CSS 키프레임과 Web Animation API를 사용하는 것은 프로덕션에서 합리적인 선택이며 애니메이션 효과를 더 세밀하게 추가할 수 있습니다.

그렇다고 해서 @starting-style 접근 방식을 추천하면, 이것이 널리 채택되면 CSS 전환 응용 프로그램을 간단하고 가볍게 유지하는 데 도움이 될 것입니다.

## 프론트엔드가 사용자 CPU를 점거하고 있나요?

웹 프론트엔드가 점점 복잡해지면, 리소스 소모가 많은 기능이 브라우저로부터 더 많은 요구를 받게 됩니다. 프로덕션에서 모든 사용자의 클라이언트 측 CPU 사용량, 메모리 사용량 등을 모니터링하고 추적하려면 LogRocket을 사용해보세요.

<div class="content-ad"></div>


![image](/assets/img/2024-08-04-AnimatingdialogandpopoverelementswithCSSstarting-style_1.png)

LogRocket은 웹 앱, 모바일 앱 또는 웹사이트에서 발생하는 모든 사항을 기록하는 DVR처럼 작동합니다. 문제가 발생한 이유를 추측하는 대신 핵심 프론트엔드 성능 지표를 집계하고 보고할 수 있으며, 사용자 세션과 애플리케이션 상태를 리플레이하며 네트워크 요청을 기록하고 모든 오류를 자동으로 보고할 수 있습니다.

웹 및 모바일 앱의 디버깅 방식을 현대화하고 무료로 모니터링을 시작하세요.
