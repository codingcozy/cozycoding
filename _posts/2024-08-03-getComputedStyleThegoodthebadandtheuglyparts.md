---
title: "getComputedStyle 장점, 단점 및 주의해야 할 부분들"
description: ""
coverImage: "/assets/img/2024-08-03-getComputedStyleThegoodthebadandtheuglyparts_0.png"
date: 2024-08-03 19:32
ogImage: 
  url: /assets/img/2024-08-03-getComputedStyleThegoodthebadandtheuglyparts_0.png
tag: Tech
originalTitle: "getComputedStyle The good, the bad and the ugly parts"
link: "https://dev.to/nikneym/getcomputedstyle-the-good-the-bad-and-the-ugly-parts-1l34"
isUpdated: true
updatedAt: 1724246272141
---


`getComputedStyle`은 독특한 기능을 가지고 있어요. 이 함수는 백그라운드에서 몇 가지 재미있는 일을 처리하고 있으며, 사용하는 방식 또한 독특해요. 이 글에서는 이 약간 쌉싸름한 API에 대한 제 경험과 생각을 공유할 거에요.

요즘 이 함수를 자주 다루다 보니, 나중에 글을 편집할 생각이에요. 또한 다양한 특수한 케이스에 적용할 수 있는 경우들이 많다고 생각해요. 그런데, 글 마지막에 저에게 도움이 된 몇 가지 링크를 올려볼게요.

## `getComputedStyle`이란 무엇인가요?

MDN에서 바로 인용하자면:

<div class="content-ad"></div>

친절한 톤으로 번역해드리겠습니다.

그냥 넘어가면 주어진 요소의 스타일을 반환합니다. 여기서 흥미로운 부분은 계산을 해결하는 것입니다. CSS에서 'calc()'를 사용하여 width 속성이 지정된 경우를 가정해보겠습니다:

Markdown 포맷으로 변경:

사용 예시: 일반적인 JavaScript에서

```js
const element = document.getElementById("box");

// 요소의 계산된 스타일 가져오기
const styles = getComputedStyle(element);
```

간단히 말하면 주어진 요소에 대한 스타일을 반환합니다. 여기 흥미로운 부분은 계산을 해결하는 것입니다. CSS에서 'calc()'로 지정된 width 속성이 있다고 해보겠습니다:

```js
#box {
  width: calc(100px - 80px);
}
```

<div class="content-ad"></div>

결과가 20픽셀이 나올 거예요. 그 말은 실제로 자바스크립트에서 CSS 계산 결과를 액세스할 수 있다는 뜻이에요. 뷰포트와 백분율 단위(100vh, 50% 등)도 계산하고 결과를 픽셀로 제공해줘요. 멋지죠!

잡았지요! 계산된 값은 속성에 따라 달라지며 정적이 아니에요. 아래와 같은 계산을 기대할 수 없다는 거죠:

```js
#box {
  --box-width: calc(100px - 80px);
  width: var(--box-width);
}
```

```js
// boxWidth 변수에 문자열 `calc(100px - 80px)`가 할당될 거에요
// `20px`가 아니에요
const boxWidth = getComputedStyle(element)["--box-width"];
```

<div class="content-ad"></div>

마크다운 형식으로 표를 변경하는 건 완전 납득해요. CSS 변수의 결과를 정적으로 계산하는 건 불가능하니까요. 그건 환경과 속성에 따라 다르거든요 (예를 들어, 백분율을 생각해보세요).

하지만 `width` 속성에 액세스하려 하면 20픽셀이 반환될 거에요. 이건 특정 요소의 너비를 계산하게 될 거라구요:

```js
// 20px
const { width } = getComputedStyle(element);
```

그건 멋지죠! 근데 실제 사용 사례는 뭐가 있나요?

<div class="content-ad"></div>

## 0px에서 auto로 요소 전환

만약 calc-size()가 현대 브라우저에서 널리 사용 가능해진 후에 이 글을 읽고 있다면, 그만두고 가서 사용해보세요. 아마도 우리의 해결책보다 성능이 더 좋을 겁니다. 자동으로 전환할 수 없는 현실에 갇혀 있다면 계속 읽어보세요!

웹 개발 여정에서 auto로 전환하는 작업에 대해 여러 번 마주치게 될 수 있습니다. 여기에는 애니메이션 라이브러리가 잘 맞을 수 있지만, 모두 사용할 필요는 없다는 사실을 알려드릴게요.

요소를 켜고 끌 수 있는 상자가 있다고 가정해 봅시다. 해당 상자에는 일부 텍스트 내용이 포함되어 있습니다. 텍스트 내용은 동적이므로 미리 최대 높이를 지정할 수 없습니다. 디자이너는 높이가 0px에서 자동 높이로 천천히 전환되는 애니메이션이 필요하다고도 합니다.

<div class="content-ad"></div>

다음은 우리의 HTML과 CSS 코드입니다:

```js
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  </head>
  <style>
    #box {
      position: relative;
      display: block;
      width: fit-content;
      height: 0px;
      overflow: hidden;
      background-color: rebeccapurple;
      color: whitesmoke;
      transition: 1s;
    }
  </style>
  <body>
    <button type="button" id="toggle">toggle</button>
    <div id="box"></div>

    <script type="module" src="/src/main.ts" defer></script>
  </body>
</html>
```

스크립트 부분에서는 시작할 때 많은 작업을 하지 않을 것입니다. 단순히 텍스트 콘텐츠를 추가하거나 제거하기 위한 간단한 상태를 유지하면 됩니다. (TS를 사용할 지 여부는 여러분의 결정입니다):

```js
// wrapper div 요소 가져오기
const box = document.getElementById("box") as HTMLDivElement;
// 트리거 버튼 가져오기
const button = document.getElementById("toggle") as HTMLButtonElement;
// 추적을 유지하는 상태 변수
let toggle = false;

function changeBoxContent() {
  if (toggle) {
    box.innerText = `Lorem ipsum dolor sit, 
    amet consectetur adipisicing elit.
    Minima culpa ipsum quibusdam quia
    dolorum excepturi sequi non optio,
    ad eaque? Temporibus natus eveniet
    provident sit cum harum,
    praesentium et esse?`;
  } else {
    box.innerText = "";
  }
}

button.addEventListener("click", () => {
  // toggle 상태 변경
  toggle = !toggle;
  // 콘텐츠 업데이트
  changeBoxContent();

  // ...
});
```

<div class="content-ad"></div>

여기 있는 코드는 간단하게 상자의 innerText를 의사 문자열로 설정하거나 비어있는 문자열로 되돌립니다.

지금 버튼을 클릭하면 아무것도 변경되지 않습니다. 그 이유는 상자의 높이를 0픽셀로 설정하고 overflow를 숨겼기 때문입니다.

텍스트에 필요한 공간이 얼마나 필요한지 알기 위해 상자의 높이를 자동으로 설정하고 상자에서 getComputedStyle를 호출할 수 있습니다. 이 곳의 아이디어는 요소를 자동으로 확장하여 필요한만큼 크게 만들어주는데, 여기서 getComputedStyle는 그 크기를 픽셀 단위로 가져올 것입니다.

```js
button.addEventListener("click", () => {
  // 토글 전환
  toggle = !toggle;
  // 콘텐츠 업데이트
  changeBoxContent();

  // 요소의 높이를 `auto`로 설정하여 콘텐츠에 맞게 요소를 확장합니다.
  box.style.height = "auto";

  // 우리가 원하는 높이 속성을 얻었습니다!
  const height = getComputedStyle(box).height;

  console.log(height);
});
```

<div class="content-ad"></div>

버튼을 클릭할 때 상자가 필요로 하는만큼의 높이를 갖도록 해야합니다. 콘솔에서 픽셀 단위의 높이를 확인할 수도 있습니다:

![image](/assets/img/2024-08-03-getComputedStyleThegoodthebadandtheuglyparts_0.png)

이것은 멋지지만, 확실히 전환되지는 않습니다.

필요한 높이를 알았으니, 상자의 높이를 이전 상태로 설정할 수 있을 것입니다. requestAnimationFrame 호출 내에서 getComputedStyle에서 얻은 높이로 다시 설정해보세요. 한번 시도해보세요!

<div class="content-ad"></div>

```js
button.addEventListener("click", () => {
  // 토글을 변경합니다.
  toggle = !toggle;
  // 내용을 업데이트합니다.
  changeBoxContent();

  // 요소의 높이를 `auto`로 설정하여 콘텐츠에 맞게 요소를 확장합니다.
  box.style.height = "auto";

  // 원하는 높이 속성을 얻었습니다!
  const height = getComputedStyle(box).height;

  // 높이를 다시 0px로 설정합니다.
  box.style.height = "";

  // 다음 애니메이션 프레임에서 최종 높이 설정
  requestAnimationFrame(() => {
    box.style.height = height;
  });
});
```

이제 높이를 0px로 설정하고 다음 애니메이션 프레임에서 변경하면 애니메이션을 볼 수 있어야 합니다. 맞나요?

어떤 브라우저에서는 애니메이션이 정상적으로 작동하지 않을 수 있습니다. 콤퓨터에서 Firefox와 Chrome의 비교:

<div class="content-ad"></div>


![image](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExbmttY2VkN2k4Nnl3czQ5b3VvZ253NjNtNnliNHNhcTNkN3kzOXcxaSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/kJUdqPuzTvBx85aYOP/giphy.gif)

이것이 발생하는 이유는 레이아웃 계산이 스타일 이후에 발생하기 때문입니다. 레이아웃 계산이 스타일 계산 이전에 발생할 것을 보장할 수 없습니다. 브라우저들은 requestAnimationFrame에 대해 다른 구현을 가지고 있는 것으로 보입니다. (여기에서 더 자세히 알아볼 수 있습니다)

하지만 절망하지 마세요! 이 문제에 대한 다양한 해결책이 있습니다. 우리는 다음을 할 수 있습니다:

- 브라우저에게 레이아웃 계산을 즉시 실행하도록 요청하기
- 다음 틱에서 애니메이션이 실행되도록 intertwined requestAnimationFrames 사용하기 (이것은 double rAF()로도 알려져 있습니다)


<div class="content-ad"></div>

브라우저에 스타일을 먼저 계산하도록 강제하는 방법을 시도해봅시다. getComputedStyle를 기억하나요? 확실히 기억하실 거에요. 여기서도 우리를 구해줄 거예요!

높이를 0px로 다시 설정한 직후에 레이아웃을 다시 계산하도록 강요할 거에요:

```js
button.addEventListener("click", () => {
  // 토글 변경
  toggle = !toggle;
  // 콘텐츠 업데이트
  changeBoxContent();

  // 요소의 높이를 `auto`로 설정하여 콘텐츠에 맞게 요소를 확장함
  box.style.height = "auto";

  // 우리가 원하는 높이 속성을 가져왔어요!
  const height = getComputedStyle(box).height;

  // 높이를 원래대로 설정합니다 (초기화)
  box.style.height = "";

  // 우리는 동기적으로 브라우저에게 요소의 높이를 다시 계산하도록 강요하고 있어요
  getComputedStyle(box).height;

  // 다음 애니메이션 프레임에서 최종 높이를 설정합니다
  requestAnimationFrame(() => {
    box.style.height = height;
  });
});
```

주의사항: 왜 높이 속성에 접근하지만 어떤 변수에 할당하지 않는지 궁금할 수 있어요. getComputedStyle는 액세스될 때 속성을 계산합니다. 이것은 최적화된 상태를 유지하기 위한 것이며, 상단, 왼쪽, 하단, 오른쪽, 너비, 높이에 액세스할 때만 레이아웃 계산을 실행합니다. 문서화되어 있지 않지만 염두에 둘 만한 점이에요. getComputedStyle(box)로 변경해보세요. 아무 변화가 없다는 것을 알 수 있겠죠.

<div class="content-ad"></div>

이 방법으로 문제를 해결하는 다른 방법이었습니다. 솔직히 말해서 이 방법이 훨씬 좋아요. 물론 double rAF() 트릭도 알아두는 게 좋아요.

그래서 간단하게 requestAnimationFrame을 다른 requestAnimationFrame으로 감싸면 됩니다:

```js
button.addEventListener("click", () => {
  // 토글 전환
  toggle = !toggle;
  // 콘텐츠 업데이트
  changeBoxContent();

  // 요소의 높이를 `auto`로 설정하여 콘텐츠에 맞게 요소가 확대됩니다.
  box.style.height = "auto";

  // 우리가 원하는 높이 속성을 얻었어요!
  const height = getComputedStyle(box).height;

  // 높이를 이전 상태로 다시 설정합니다 (초기화)
  box.style.height = "";

  // 다음 애니메이션 프레임에서 최종 높이를 설정합니다
  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      box.style.height = height;
    });
  });
});
```

다음 프레임에서 실행될 애니메이션을 예약했다고 생각하면 돼요. 조금 이상하게 들릴 수 있지만 이 방법은 Chrome에서도 버그를 해결하는 데 유용했던 것으로 알려져 있어요.

<div class="content-ad"></div>

요렇게 마무리 지었습니다! 현대 세상에서 우리가 WAAPI를 갖고 있을지라도, transitions와 getComputedStyle은 여전히 유용하답니다! 처음에는 이해하기 조금 까다로울 수 있지만, 그 고유한 방식이 있어요. 뭐랄까요!

소스:
Markus Oberlehner의 "Transition to Height Auto With Vue.js"
Webventures의 "Need cheap paint? Use getComputedStyle().opacity"