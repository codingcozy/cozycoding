---
title: "CSS와 JavaScript로 떠다니는 하트 만드는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 18:13
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Out of the Blue Creating Floating Hearts with a Range Slider with CSS and JavaScript"
link: "https://dev.to/rodrigoantunes/bringing-floating-hearts-to-life-with-css-and-javascript-2hcd"
isUpdated: true
updatedAt: 1724246321234
---


아침에 본 애니메이션에서 영감을 받아, 자바스크립트를 사용하여 비슷한 것을 만들어보기로 결심했어요 🎨.
훨씬 덜 인상적이지만, 재미있고 도전적인 작업으로 보였어요.

그러니, 시작해봐요!

## HTML

간단한 구조부터 시작해볼게요:

<div class="content-ad"></div>

```js
<main>
  <input type="range" min="0" max="100" value="0" />
  <div class="container"></div>  <!-- 하트를 넣을 공간입니다 -->
</main>
```

## CSS

이 프로젝트에서 CSS가 매우 중요한 역할을 합니다. 두 가지 주요 측면을 강조하고 싶습니다:

- 강조 색상
- CSS 애니메이션

<div class="content-ad"></div>

### 강조 색상

'input type="range"' 요소의 기본 모양을 변경하는 방법을 기억하지 못했어요. 조사하던 중, ::-webkit-slider-runnable-track 및 ::-moz-range-track과 같은 다양한 벤더 접두사가 있는 가상 요소를 발견했지만, 해당 기술들에 대한 지원이 좋지 않았어요.

그리고 'accent-color'라는 편리한 CSS 속성을 발견했어요. 이 속성은 이전에 몇 번 본 적이 있었지만, 기억에 남을만큼 자주 보지는 못했어요. 이것은 체크박스 및 라디오 입력 요소에 스타일을 적용하는데 완벽한 선택이었어요.

잘 했어요, CSS Working Group! 🥳

<div class="content-ad"></div>

```js
input {
  /* ... */
  accent-color: white;
}
```

### CSS 애니메이션

보통 저는 CSS 애니메이션을 처음부터 코딩하는 것을 즐기며, 디자인 목표나 구체적인 창의적인 방향과 맞추기 위해 섬세하게 조정합니다. 그러나 오늘은 기분이 안 좋았어요. 그래서 ChatGPT에게 몇 가지 다양한 변형과 랜덤성을 갖춘 세 가지 유기적인 부유 애니메이션을 작성해 달라고 요청했어요. 첫 번째 시도에서 완벽히 만족스러운 결과를 얻지 못했기 때문에 애니메이션을 수동으로 조정하는 대신 프롬프트를 수정했어요. 손수 코드를 조정하는 것이 빠를 수도 있었지만, 시간이 조금 지난 후에 결과물이 충분히 좋아졌어요.

편집: 애니메이션을 섬세하게 조정했어요 😁

<div class="content-ad"></div>

특히 구문에 대해 굉장히 까다로웠던 한 가지는 였어요. 처음에는 이런 식으로 생성되었어요:

```js
0% {
  transform: translateX(0px) translateY(0px) rotateX(45deg) rotateY(30deg) scale(0.25);
}
```

내 반응은 Michael Scott의 유명한 "오 안 돼, 제발!" 미미와 비슷했어요. CSS Transform Module Level 2 구문은 훨씬 깔끔하죠:

```js
0% {
  translate: 0px 0px;
  rotate: 45deg 30deg;
  scale: 0.25;
}
```

<div class="content-ad"></div>

하지만 나도 이 `new` 구문에 아직 익숙해지는 중이니까 큰 문제는 없어. 

## Javascript

자, 엔진을 가동해보자 🚛.

생각 과정을 설명하기가 어려웠지만, 나는 문제를 고민하는 데 거의 1시간을 보냈어. 내 접근 방식을 요약해 보겠어:

<div class="content-ad"></div>

```js
1. 입력 이벤트를 청취합니다.
2. 이벤트가 발생할 때 새 요소를 생성합니다.
3. 각 요소가 입력이 위치한 곳에서 위로 애니메이션합니다.
4. 각 요소가 애니메이션이 완료된 후 제거합니다.
```

그러나 "슬라이더의 엄지"가 화면 어디에 있는지의 직접적인 방법은 없습니다. 입력 요소 자체만 알 수 있습니다. 이 문제에 대한 간단한 해결책이 있었네요:

```js
range.addEventListener('input', (e) => {
  // ...
  const heart = document.createElement('div');
  heart.style.left = `${e.target.value}%`;
  // ...
}
```

코드를 확인해보세요!


<div class="content-ad"></div>

표 태그를 Markdown 형식으로 변경할 수 있습니다.