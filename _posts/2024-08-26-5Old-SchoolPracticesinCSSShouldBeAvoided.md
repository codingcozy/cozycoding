---
title: "CSS에서 피해야 할 5가지 안좋은 방식"
description: ""
coverImage: "/assets/img/2024-08-26-5Old-SchoolPracticesinCSSShouldBeAvoided_0.png"
date: 2024-08-26 17:20
ogImage: 
  url: /assets/img/2024-08-26-5Old-SchoolPracticesinCSSShouldBeAvoided_0.png
tag: Tech
originalTitle: "5 Old-School Practices in CSS Should Be Avoided"
link: "https://medium.com/@bogdanfromkyiv/5-old-school-practices-in-css-should-be-avoided-0122f89d7b92"
isUpdated: true
updatedAt: 1724744133680
---


저의 이전 글 중 하나에서는 요즘 사람들이 자주 사용하는 HTML의 구식 습관에 대해 썼습니다. 이제 CSS의 구식 속성과 기술에 대해 이야기해보고, 이를 현대화하는 방법에 대해 이야기해보겠습니다.

# Float은 더 이상 사용되지 않습니다!

예전에는 우리 모두 float 속성을 사용했던 고전적인 시절이 있었습니다.

현대 브라우저가 flex와 grid와 같은 속성을 지원하기 시작한 이후로, float의 사용을 계속할 이유가 없다고 생각합니다.

<div class="content-ad"></div>

간단한 페이지 레이아웃을 만들어 보겠습니다. 사이드바와 콘텐츠 영역이 있는 레이아웃입니다.

## ❌ Old Skool

여기에서는 플로트 레이아웃을 사용했습니다:

플로트 속성을 사용하는 주요 단점은 요소가 페이지의 일반 흐름에서 제거된다는 것입니다. 따라서 `aside`와 `article` 요소 모두에 대해 플로트를 사용하면 부모 요소인 `main`이 높이를 가지지 않게 됩니다.

<div class="content-ad"></div>

만약 `footer` 요소에 `clear: both;`를 사용하지 않으면 페이지가 이렇게 보일 거에요:

![이미지](/assets/img/2024-08-26-5Old-SchoolPracticesinCSSShouldBeAvoided_0.png)

그리고 `main` 요소의 높이 값을 수정하려면(동시에 `footer`도 수정됩니다) 이렇게 오래된 녀석을 추가해야 해요:

```js
<div class="clearfix"></div>
```

<div class="content-ad"></div>

```js
.clearfix {
  clear: both;
}
```

너무 많은 수정과 버그가 있는 단순한 레이아웃이잖아요, 그러지 않나요?

## ✅ Knu Skool

<div class="content-ad"></div>

다행히도, Flexbox 레이아웃 방식이 있어요!

요즘에는 훨씬 편리하고 현대적인 방식으로 레이아웃을 다시 만들어봐요:

사이드바는 이제 내용 영역과 함께 전체적으로 늘어날 거예요!

여분의 div가 필요 없어요!

<div class="content-ad"></div>

내가 가장 좋아하는 부분은 자동 응답성 기능이에요! 부모 요소(`main` 예시에서)에 `flex-wrap: wrap;`를 추가하고 `aside` 요소에 고정된 `flex-basis` 값을 설정하면(예를 들면 15rem) 됩니다. 위의 예시를 사용하여 프레임 크기를 조정해보세요. 그러면 이렇게 마법같이 변화하는 것을 보실 수 있어요:

![image](/assets/img/2024-08-26-5Old-SchoolPracticesinCSSShouldBeAvoided_1.png)

자동 응답성에 관한 더 많은 팁은 제 글에서 읽어보세요:

# 열에 대한 측면 패딩

<div class="content-ad"></div>

## ❌ 옛날 얘기..

예전에는 Bootstrap 프레임워크를 많이 사용했었죠. 아마도 당신도 HTML 마크업에서 col-md-4 col-lg-5 col-sm-3 클래스들을 기억하고 계실 것입니다. 그런데 Bootstrap에 대한 내 가장 큰 우려 중 하나는 열 사이에 갭을 만들기 위해 해당 열에 padding-left와 padding-right 값을 추가한다는 점이었습니다.

![이미지](/assets/img/2024-08-26-5Old-SchoolPracticesinCSSShouldBeAvoided_2.png)

그리고 이로 인해 전체 행이 외부 레이아웃보다 작아지기 때문에 행 자체에 음수 마진을 추가해야만 했습니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-08-26-5Old-SchoolPracticesinCSSShouldBeAvoided_3.png" />

## ✅ The modern way

이렇게하면 CSS Grid 레이아웃을 사용하여 쉽게 동일한 열 레이아웃을 만들 수 있습니다:

```js
.row {
  display: grid;
  gap: 15px;
  grid-template-columns: repeat(3, minmax(0, 1fr));
}
```

<div class="content-ad"></div>

이 방법은 RAM 기법이라고도 불립니다. 다음 기사에서 더 자세히 살펴보세요:

# 컬럼 쌓이는 미디어 쿼리

## ❌ 다시 부트스트랩

이전 예제에 좀 더 오래 머물러 봅시다. 작은 화면에서 컬럼이 쌓이도록 하려면 다음과 같이 미디어 쿼리와 함께 여러 가지 브레이크포인트를 추가해야 했습니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-26-5Old-SchoolPracticesinCSSShouldBeAvoided_4.png)

최소 너비 값을 초과하면 열이 자동으로 쌓일 때 멋지지 않을까요?

## ✅ 2024년에는 이렇게 합시다!

이미 RAM 방법을 언급했으니 그 힘을 빌려봅시다:


<div class="content-ad"></div>

```js
display: grid;
grid-template-columns: repeat(auto-fit, minmax(15em, 1fr));
```

왔어요!

# 링크의 밑줄 스타일링을 위한 테두리

링크에 밑줄을 추가하는 것은 그들을 강조하는 가장 좋은 (그리고 가장 오래된) 방법입니다. 이것은 이렇게 말하는 것과 같습니다: "저, 단순한 텍스트가 아니에요. 저는 링크야! 클릭할 수 있어요!".

<div class="content-ad"></div>

접근성에도 큰 영향을 미치거든요! 시각 장애가 있는 분들은 기사 텍스트 안에 있는 "약간 더 밝거나 진한" 링크를 감지하는 데 어려움을 겪을 겁니다.

페이지 내 모든 링크에 대해 얘기하는 게 아니에요. 내비게이션 컨텍스트(예: 헤더/푸터 메뉴) 내에 있는 경우는 밑줄을 넣지 않아도 괜찮아요.

그러나 기사 내부나 항상 그 자리에 있을 것으로 예상되지 않는 곳(예: 폼 내부)에 있는 링크에는 꼭 밑줄을 추가해야 해요!

## ❌ 하지만 밑줄은 예쁘지 않다고요...

<div class="content-ad"></div>

개발자들이 웹 사이트에서 기본적으로 모든 링크에 text-decoration: none;을 설정하는 것을 많이 보았어요. 대신, 가상 클래스를 사용하여 밑줄을 추가합니다.

```js
a {
  position: relative;
}
a::after {
  position: absolute;
  content: "";
  height: 1px;
  width: 100%;
  bottom: -5px;
  left: 0;
  right: 0;
  margin: 0 auto;
  background: #3f567e;
}
```

또는 밑줄 대신 하단 테두리를 추가할 수도 있어요.

```js
a {
  border-bottom: 1px solid #3f567e;
}
```

<div class="content-ad"></div>

## ✅ 밑줄을 완전히 제어하세요!

현대적인 CSS 속성 덕분에 우리는 원하는 대로 밑줄을 완전히 사용자 정의할 수 있습니다!

여기 그 속성들의 목록입니다:

- text-decoration-line
- text-decoration-skip-ink
- text-decoration-style
- text-decoration-thickness
- text-decoration-color
- text-underline-position
- text-underline-offset

<div class="content-ad"></div>

각 속성을 더 자세히 살펴보려면 MDN 문서를 참조해보세요: [MDN 링크](https://developer.mozilla.org/en-US/docs/Web/CSS/text-decoration)

하지만 여 hoe뿐만 아니라 여기에서 각 속성을 실험해볼 수 있는 빠른 데모도 있습니다:

# 여러 값에 transform 속성 사용하기

transform CSS 속성 덕분에 페이지의 요소를 쉽게 변형할 수 있습니다. 레이아웃이나 페인트 단계를 트리거하지 않기 때문에, CSS만으로 부드러운 애니메이션을 만들 수 있습니다!

<div class="content-ad"></div>

# ❌ 그래서 문제가 뭐야?

변형 속성은 요소를 회전, 확대/축소, 기울이기 또는 이동할 수 있게 합니다. 이를 달성하기 위해 적절한 함수를 사용합니다. 그래서 속성에 너무 많은 함수가 있으면 읽기 어려워지고 덮어씌우기가 어려워집니다.

이게 내 말하는 바입니다:

```js
.some-class {
  transform: perspective(500px) translate(10px, 0, 20px) rotateY(3deg) scale3d(2.5, 1.2, 0.3);
}
```

<div class="content-ad"></div>

그리고 `rotateY`만 오버라이드하고 싶다고 가정해 봅시다:

```js
.some-class:hover {
  transform: perspective(500px) translate(10px, 0, 20px) rotateY(20deg) scale3d(2.5, 1.2, 0.3);
}
```

따라서 모든 값을 유지하고 rotateY의 값 하나만 수정하려면 해당 값을 복사하여 수정해야 합니다.

## ✅ transform을 하위 속성으로 분할할 수 있습니다.

<div class="content-ad"></div>

2022년부터 엘리먼트를 변형하기 위해 독립적인 CSS 속성을 사용할 수 있습니다!

다음은 해당 속성들입니다:

- translate
- rotate
- transform
- scale
- perspective

참고로, skew는 독립적인 변형 값이 아닙니다!

<div class="content-ad"></div>

이전 예제를 더 읽기 쉽게 만들기 위해 다음과 같이 다시 작성해 봤어요:

```js
.some-class {
  perspective: 500px;
  translate: 10px 0 20px;
  rotate: y 3deg;
  scale: 2.5 1.2 0.3;
}
```

그리고 rotateY 값을 재정의하기 위해서는 해당 특정 속성만 재정의하면 돼요:

```js
.some-class:hover {
  rotate: y 20deg;
}
```

<div class="content-ad"></div>

개별 변환 속성을 사용할 때 명심해야 할 몇 가지 중요한 변경 사항이 있어요:

- 다른 CSS 속성과 마찬가지로 값을 구분할 때 공백을 사용하세요. 쉼표는 함수에서만 사용해요! (예: 10px 0 20px;)
- 속성 이름에 수정 기호를 사용할 수 없어요(rotateY가 아닌 rotate만 사용 가능해요). 그러나 걱정 마세요. 여전히 단일 값을 수정할 수 있어요(예: rotate: y 3deg;). 다양한 값을 사용하는 방법에 대한 자세한 내용은 MDN의 해당 속성 문서를 확인하세요.

오늘은 여기까지에요! 놓친 것이 있을까요? 어떤 CSS 속성이 널리 사용되길 기다리고 있나요? 댓글에 적어주세요 ;)

그리고 "7가지 HTML 오래된 방식"에 관한 제 글을 놓치신 경우를 위해:

<div class="content-ad"></div>

만약 이 기사를 흥미롭거나 유익하다고 생각하신다면 주저하지 말고 박수를 치고 구독하고 댓글로 생각을 공유해 주세요 😊

읽어 주셔서 감사합니다!
안전하고 평화로운 하루 되세요!