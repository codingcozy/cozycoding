---
title: "새로운 CSS 기능을 활용한 시간 기반 애니메이션 최적화 방법"
description: ""
coverImage: "/assets/img/2024-08-26-OptimizingCSStime-basedanimationswithnewCSSfunctions_0.png"
date: 2024-08-26 20:10
ogImage: 
  url: /assets/img/2024-08-26-OptimizingCSStime-basedanimationswithnewCSSfunctions_0.png
tag: Tech
originalTitle: "Optimizing CSS time-based animations with new CSS functions"
link: "https://dev.to/logrocket/optimizing-css-time-based-animations-with-new-css-functions-4a2i"
isUpdated: false
---


엠마누엘 오디오코✏️이 작성함

오랫동안 수학 함수의 제한된 지원으로 인해 시간 기반 CSS 애니메이션을 만드는 것은 더 어렵게 되었습니다. 기존의 애니메이션은 핵심 프레임과 지속 시간을 활용하지만 복잡한 계산에 기반한 시간 기반 업데이트의 유연성이 부족했습니다. mod(), round(), 삼각함수와 같은 CSS 함수의 등장으로 개발자들은 이제 CSS에서 시간 기반 애니메이션을 어떻게 구사할지 탐구할 수 있습니다.

## 시작하기 전에 알아야 할 사항

새로운 CSS 함수를 활용한 CSS의 시간 기반 애니메이션에 대한 이 글을 최대한 활용하기 위해서는 CSS 애니메이션 및 전환에 대한 좋은 이해가 있어야 합니다. @keyframes를 사용하여 애니메이션을 만들고 그 시간을 제어하는 방법을 알아야 합니다. 또한 DOM 요소를 조작하고 사용자 이벤트에 응답하는 능력에 초점을 맞춘 자바스크립트의 기본적인 이해가 필요합니다.

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

마지막으로, calc()와 같은 새로운 CSS 함수에 대한 이해, 그리고 mod(), sin()과 cos()를 포함한 삼각함수, 그리고 round()과 같은 새로운 기능들을 탐험할 준비가 되어 있다면 훌륭한 기초가 될 것입니다.

이 글을 읽는 동안, 여러분은 어떻게 애니메이션이 기존 CSS 함수와 비교되는지를 이해하게 될 것입니다. HTML 캔버스에서 JavaScript를 사용하여 전통적으로 구현된 애니메이션과 신규 CSS 함수와의 비교를 살펴볼 것입니다. 우리는 기존 CSS 프레임보다 mod(), round(), 그리고 삼각함수를 사용할 때의 용이성에 대해 이해하게 될 것입니다.

![image](/assets/img/2024-08-26-OptimizingCSStime-basedanimationswithnewCSSfunctions_0.png)

## CSS 시간 기반 애니메이션이란?

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

시간 기반 애니메이션은 새로운 것이 아닙니다 - 이들은 10년 이상 되어왔습니다. 어떤 것은 복잡하여 사용하기 어렵지만, 다른 것들은 그렇지 않습니다. 수학적 계산이 주된 역할을 하는 CSS 파일들을 아십니까? 시간 기반 애니메이션은 그 중 일부입니다.

그 이름에서 알 수 있듯이, 이러한 애니메이션은 시간과 밀접한 관련이 있으며 요소의 속성들, 예를 들면 위치, 크기, 색상, 불투명도 등이 시간이 지남에 따라 변화합니다. CSS의 시간 기반 애니메이션은 웹 애플리케이션의 느낌을 향상시키고 사용자 경험을 개선하는 부드러운 전환을 제공합니다.

시간 기반 CSS 애니메이션은 주로 정의된 시작 지점과 종료 시점, 그리고 보간점들로 구성됩니다. 여기서 보간은 애니메이션이 진행되는 동안 지정된 기간 내에서 시작과 끝 사이의 중간 값들을 계산하는 것을 의미합니다. 보간의 목적은 초기 상태에서 최종 상태로의 부드러운 전환을 제공하는 것입니다.

시간 기반 애니메이션은 CSS 변수와 몇 가지 수학적 함수의 결합으로 발생합니다. 이 통합을 통해 개발자는 시간이 흐름에 따라 변화하는 애니메이션을 만들 수 있으며, 키프레임 애니메이션만 꿈꿀 수 있는 보다 유연한 애니메이션 결과를 얻을 수 있습니다. 이제 핵심 개념과 작동 방식을 자세히 살펴보겠습니다.

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

## 시간 기반 애니메이션 분해

이 섹션에서는 시간 기반 애니메이션을 만들기 위한 일반적인 구조를 주요 구성 요소로 나누어 설명하겠습니다.

### 초기 상태

초기 상태는 애니메이션이 시작하기 전 요소의 초기 속성을 정의합니다. 이는 지정된 위치, 크기, 색상, 불투명도 등이 될 수 있습니다. 아래에 예시가 있습니다:

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
.box {
  opacity: 0;
  transform: translateY(-20px);
}
```

위의 코드에서, 클래스가 box로 지정된 요소의 초기 상태를 정의하는 opacity 및 transform 속성이 있습니다.

애니메이션 트리거는 애니메이션을 시작하는 이벤트를 지정합니다. 클릭 또는 호버와 같은 사용자 상호 작용, 페이지 로드 이벤트, 또는 사용자에 의한 특정 작업 완료와 같은 응용 프로그램의 특정 조건이 일반적인 트리거입니다.

애니메이션의 속성에는 애니메이션 지속 시간, 타이밍 함수, 지연, 반복 횟수, 방향 및 채우기 모드가 있습니다. 애니메이션은 이러한 속성 중 일부 또는 모두를 가질 수 있습니다. 호버 선택기가 있는 예제 트리거는 아래에 표시되어 있습니다:

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
.box:hover {
  animation: fadeIn 1s ease-in-out forwards;
}
```

이렇게 하면 hover되는 경우 class box가 있는 요소에 fadeIn 애니메이션이 추가되며, 1초 동안 지속됩니다. 애니메이션 동작과 타이밍도 지정되어 있습니다. 애니메이션 및 전환 타이밍 함수에 대한 자세한 정보는 이 문서를 읽어보세요.

### 보간점

이전에 설명한대로, 이들은 타임라인 상의 여러 지점에 걸쳐 애니메이션의 중간 상태입니다. 각 키프레임은 특정 시점의 요소 속성을 지정하여 초기 및 최종 상태 사이의 점진적인 전환을 허용합니다. 보간점의 예시 구현은 CSS keyframes 속성입니다.

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
@keyframes fadeIn {
  0% {
    opacity: 0;
    transform: translateY(-20px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}
```

위 예시는 fadeIn 애니메이션의 속성을 0%와 100% 애니메이션 진행률에서 정의하는 데 keyframes를 사용합니다.

### 시간을 기반으로 한 애니메이션의 일반적인 사용 사례

시간을 기반으로 한 애니메이션은 웹 애플리케이션에서 점점 더 중요해지고 있습니다. 이는 더 나은 사용자 경험을 제공하는 데 도움이 됩니다. 이러한 애니메이션의 사용은 섬세한 마이크로 상호작용에서부터 중요한 사이트 전환까지 다양하며, 웹 앱에 더 동적인 느낌을 제공합니다. 아래는 이러한 애니메이션의 일반적인 사용 사례입니다.

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

### 마이크로 상호작용

![이미지](https://res.cloudinary.com/practicaldev/image/fetch/s--BBVoQP2O--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://blog.logrocket.com/wp-content/uploads/2024/07/Micro-interactions.gif%3Fw%3D800)

위의 이미지에서 사용자가 클릭할 때 제출 버튼이 로더와 확인 표시를 보여줍니다. 이러한 마이크로 상호작용의 핵심은 제출 과정과 작업 성공을 사용자에게 알리는 것입니다.

### 전환

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

웹 애플리케이션에서 상태나 페이지 변경을 나타내기 위해 사이트 전환 효과는 페이딩, 슬라이딩 또는 요소 크기 조정과 같은 효과를 사용하여 유동적인 사용자 경험을 만들어냅니다. 시간 기반 애니메이션을 활용하여 이러한 전환 효과를 구현할 수 있습니다. 전환 효과를 주로 사용하는 예시로는 네비게이션 및 측면 메뉴 전환, 패럴랙스 애니메이션, 모달 창의 열기 및 닫기 등이 있습니다.

이미지 출처: https://medium.com/@9cv9official/create-a-beautiful-hover-triggered-expandable-sidebar-with-simple-html-css-and-javascript-9f5f80a908d1

위 GIF 이미지에서는 마우스 호버 이벤트로 사이드바를 확장시키는 전환 애니메이션을 사용하는 사이드바가 표시됩니다.

## 새로운 CSS 함수 탐색

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

새로운 수학적인 CSS 함수 mod(), round(), 그리고 삼각함수 sin(), cos(), tan()에 대해 자세히 설명해보면서 알아봅시다.

### Mod () 함수

JavaScript의 모듈로 연산자 % 와 마찬가지로, 이 함수는 두 피연산자에 대한 산술 모듈로 연산이 수행된 후의 나머지를 반환합니다. 본질적으로, 모듈러스는 나눗수인 피제수를 다른 피연산자인 제수로 나눈 후에 남은 값으로, 더 이상의 나눗셈이 일어나지 않을 때의 값입니다. JavaScript에서는 모듈로 연산자를 사용하면 다음과 같은 형식이 됩니다: 10%4.

이 작업은 나머지가 2인 모듈러스를 남길 것이며, 10은 나눗수 4로만 두 번 나눌 수 있기 때문에 나머지로 값 2를 남깁니다. 비슷하게, CSS Mod 함수는 대신 다음 구문으로 동일한 기능을 수행할 것입니다: mod(10, 4).

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

피제수(dividend)와 제수(divisor)를 가지는 mod() 함수는 두 개의 쉼표로 구분된 두 세트의 매개변수를 주로 허용합니다. 이러한 피연산자는 유효하게만 하기 위해 동일한 차원이어야 하며, 매개변수로 다양한 값을 사용할 수 있어 응용 범위가 향상됩니다. mod()로 전달된 피연산자는 숫자, 백분율 또는 차원이 될 수 있습니다.

또한 mod()는 피연산자의 단위(예: px, rem, vh, deg)를 수용하고 나눗셈 또는 제수로 수학 계산을 처리할 수도 있습니다. 이 CSS 함수의 사용 예시를 보여주는 몇 가지 예제는 아래에 있습니다:

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
/* 사용자 단위 없는 <숫자> 사용 */
scale: mod(18, 7); /* 결과는 4 */

/* 단위와 함께 <백분율> 및 <크기> */
height: mod(100vh, 30vh); /* 결과는 10vh */
width: mod(500px, 200px); /* 결과는 100px */
transform: rotate(mod(90deg, 20deg)); /* 결과는 10deg */

/* 음수 <백분율> 및 <크기>와 함께 */
height: mod(18rem, -4rem); /* 결과는 2rem */
rotate: mod(180deg, -100deg); /* 결과는 80deg */

/* 계산과 함께 작업 */
width: mod(40px*2, 15px); /* 결과는 5px */
transform: scale(mod(2*3, 1.8)); /* 결과는 0.6 */
rotate: mod(10turn, 8turn/2); /* 결과는 2turn */
```

위 코드 블록은 CSS 스타일에서 mod()의 다양한 응용을 보여줍니다.

표시된 예제는 알려진 값들을 사용하고 있지만, 시간 기반 함수는 CSS 변수와 함께 사용되어 예상대로 동작하며 동적으로 값이 변경되어 스타일이 변수에 전달된 함수에 따라 값이 변경될 수 있게 합니다. 작업의 결과는 지정된 변수를 사용한 계산에 따라 결정되며, 하드코딩된 값들을 사용할 때보다 더 넓은 범위의 결과물을 생산할 수 있습니다.

아래에는 MDN에서 설명된 mod()의 모든 가능성에 대한 일반적인 구문을 찾을 수 있습니다:

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
<mod()> = 
  mod( <calc-sum> , <calc-sum> )  

<calc-sum> = 
  <calc-product> [ [ '+' | '-' ] <calc-product> ]*  

<calc-product> = 
  <calc-value> [ [ '*' | '/' ] <calc-value> ]*  

<calc-value> = 
  <number>        |
  <dimension>     |
  <percentage>    |
  <calc-keyword>  |
  ( <calc-sum> )  

<calc-keyword> = 
  e          |
  pi         |
  infinity   |
  -infinity  |
  NaN       
```

위 구문에서, calc-sum은 나머지 연산의 피연산자를 나타냅니다. 이 구문은 또한 calc-sum이 포함할 수 있는 값의 유형 및 음수 및 양수 값의 가능성을 보여줍니다. 또한 위의 구문은 가능한 calc-keywords e, pi, infinity, -infinity 및 NaN도 보여줍니다.

### round() 함수

CSS round() 함수 값은 지정된 반올림 전략에 기반합니다. 여기서 전략은 값의 반올림 패턴인 반올림 상향 또는 내림, 제로에 반올림, 숫자의 가장 가까운 발생 지점에 반올림 등을 의미합니다.

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

### round() 함수의 매개변수 및 구문

CSS `round()` 함수를 적용하는 구문은 아래와 같습니다:

```js
round(<rounding-strategy>, valueToRound, roundingInterval)
```

이곳에서 CSS `round()` 함수를 작은 부분으로 쪼개어 각 키워드의 역할 및 가능한 값에 대한 하이라이트를 설명합니다.

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

### 반올림 전략

반올림 전략은 지정된 값에 대해 사용할 기술 유형을 의미합니다. 이는 선택 사항입니다(지정되지 않은 경우 가장 가까운 값으로 기본 설정됩니다) 및 다음 중 하나 일 수 있습니다:

- up — 값을 지정된 반올림 간격의 가장 가까운 정수 배수로 반올림합니다. 이 작업은 JavaScript의 Math.ceil() 메서드와 유사하며 값이 음수인 경우 더 긍정적인 결과를 생성합니다
- down — valueToRound를 지정된 반올림 간격의 가장 가까운 정수 배수로 내림합니다. 이는 JavaScript의 Math.floor() 메서드와 유사하며 값이 음수인 경우 더 부정적인 결과를 생성합니다
- nearest — valueToRound를 반올림 간격의 가장 가까운 정수 배수로 반올림합니다. 얻은 결과는 해당 값이 반올림 간격의 배수에 얼마나 가까운지에 따라 더 높거나 낮을 수 있습니다
- to-zero — 값을 0에 가까운 정수 배수로 반올림하며 JavaScript의 trunc() 메서드와 동등합니다

### valueToRound

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

이 함수를 사용하여 반올림하려는 값이며 `number`, `dimension`, `percentage`또는 mod() 함수에서 사용한 것처럼 수학 표현식일 수 있습니다.

### roundingInterval

반올림 간격은 값을 기준으로 반올림되는 간격을 나타냅니다. 이 항목은 `number`, `dimension`, `percentage`또는 수학 표현식일 수 있습니다.

아래는 CSS round() 함수 사용 예시입니다:

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
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>문서</title>
  <style>
    body{
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }

    /* round() 함수 사용 */
    .ball {
      width: 100px;
      height: 100px;
      background-color: red;
      color: white;
      text-align: center;
      line-height: 100px;
      margin: 10px;
    }
    .ball-1{
        border-radius: round(down, 70%, var(--rounding-interval)); /* 50% 간격으로 반올림 */
    }

    .ball-2{
        border-radius: round(up, 70%, var(--rounding-interval2)); /* 100% 간격으로 반올림 */
    }

    .ball-3{
        border-radius: round(nearest, 15%, var(--rounding-interval3)); /* 25% 간격으로 반올림 */
    }

  </style>
</head>
<body>
    <!-- 둥근 컨테이너 -->
  <div class="ball ball-1" style="--rounding-interval:50%;">50% 둥글게 된 요소</div>
  <div class="ball ball-2" style="--rounding-interval2:100%;">100% 둥글게 된 요소</div>
  <div class="ball ball-3" style="--rounding-interval3:25%;">25% 둥글게 된 요소</div>
</body>
</html>
```

<img src="/assets/img/2024-08-26-OptimizingCSStime-basedanimationswithnewCSSfunctions_1.png" />

```js
<round()> = 
  round( <rounding-strategy>? , <calc-sum> , <calc-sum>? )  

<rounding-strategy> = 
  nearest  |
  up       |
  down     |
  to-zero  

<calc-sum> = 
  <calc-product> [ [ '+' | '-' ] <calc-product> ]*  

<calc-product> = 
  <calc-value> [ [ '*' | '/' ] <calc-value> ]*  

<calc-value> = 
  <number>        |
  <dimension>     |
  <percentage>    |
  <calc-keyword>  |
  ( <calc-sum> )  

<calc-keyword> = 
  e          |
  pi         |
  infinity   |
  -infinity  |
  NaN        
```

위 구문에서 rounding-strategy는 의도된 반올림 패턴이며, calc-sum은 피연산자를 나타냅니다. 이 공식은 rounding-strategy와 calc-sum에 입력할 수 있는 가능한 항목을 보여줍니다. 마지막으로, 가능한 calc-keywords인 e, pi, infinity, -infinity, NaN을 개요로 보여줍니다.

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

### 삼각 함수

CSS 삼각 함수는 수학에서와 같은 작업을 수행합니다. sin() 함수는 숫자의 사인 값을 -1에서 1 사이의 값으로 반환하며, cos()는 값의 코사인을 반환하고, tan()은 지정된 값의 탄젠트를 반환합니다.

이러한 함수에 전달되는 인수는 숫자 또는 각도여야 하며, 라디안으로 처리됩니다. deg 및 turn과 같은 단위는 각도를 나타내며 여기에 인수로 사용할 수 있습니다.

아래에 이러한 함수의 예시 응용이 표시되어 있습니다:

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
scale: sin(45deg); /* 결과는 0.7071067811865475 */
rotate: cos(30deg); /* 결과는 0.8660254037844387 */
height: calc(50px * tan(30deg)); /* 결과는 28.86751345948129px */
```

모든 삼각함수 CSS 함수들은 유사성을 가지며, 각각 단일 매개변수를 취하는데 이는 각도로 해석됩니다.

#### sin()의 매개변수와 구문

Sin()은 하나의 매개변수만을 취하며, 숫자 또는 각도이거나 이 둘 중 하나로 해결되는 수학식을 취합니다. Sin()의 구문은 다음과 같습니다:

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
sin(angle)
```

sin()의 형식은 아래와 같이 표시됩니다:

```js
<sin()> = 
  sin( <calc-sum> )  

<calc-sum> = 
  <calc-product> [ [ '+' | '-' ] <calc-product> ]*  

<calc-product> = 
  <calc-value> [ [ '*' | '/' ] <calc-value> ]*  

<calc-value> = 
  <number>        |
  <dimension>     |
  <percentage>    |
  <calc-keyword>  |
  ( <calc-sum> )  

<calc-keyword> = 
  e          |
  pi         |
  infinity   |
  -infinity  |
  NaN        
```

위 문법은 calc-sum 및 calc-keyword의 가능한 값들을 보여줍니다.

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

#### cos() 함수의 매개변수와 구문

cos() 함수의 매개변수는 숫자, 각도 또는 하나의 계산이 포함된 값이어야 합니다.

따라서 cos() 함수의 구문은 다음과 같습니다:

```js
cos(각도)
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

모든 가능성의 cos()의 형식 구문은 아래와 같습니다:

```js
<cos()> = 
  cos( <calc-sum> )  

<calc-sum> = 
  <calc-product> [ [ '+' | '-' ] <calc-product> ]*  

<calc-product> = 
  <calc-value> [ [ '*' | '/' ] <calc-value> ]*  

<calc-value> = 
  <number>        |
  <dimension>     |
  <percentage>    |
  <calc-keyword>  |
  ( <calc-sum> )  

<calc-keyword> = 
  e          |
  pi         |
  infinity   |
  -infinity  |
  NaN      
```

여기서 calc-sum은 매개변수이고, calc-value는 허용되는 매개변수의 유형이며, calc-keyword는 수학식에 추가할 수 있는 가능한 단위입니다.

#### tan()의 매개변수 및 구문

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

tan() 함수는 숫자, 각도 또는 다른 삼각함수와 유사한 방식으로 변환해야 하는 단일 계산을 취할 수도 있습니다. tan()의 구문은 다음과 같습니다:

```js
tan(각도)
```

이 함수의 형식적인 구문은 아래와 같습니다:

```js
<tan()> = 
  tan( <calc-sum> )  

<calc-sum> = 
  <calc-product> [ [ '+' | '-' ] <calc-product> ]*  

<calc-product> = 
  <calc-value> [ [ '*' | '/' ] <calc-value> ]*  

<calc-value> = 
  <number>        |
  <dimension>     |
  <percentage>    |
  <calc-keyword>  |
  ( <calc-sum> )  

<calc-keyword> = 
  e          |
  pi         |
  infinity   |
  -infinity  |
  NaN        
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

이 구문은 calc-sum, 피연산자 및 calc 키워드의 모든 가능한 값들을 보여줍니다.

## CSS 함수와 키프레임 간 타이밍 애니메이션 비교

이 섹션에서는 CSS 함수를 사용하여 애니메이션을 만들고, 대체로 키프레임, 그리고 두 번째 대안으로 JavaScript를 사용할 것입니다. 마지막에는 코드를 비교하고 접근 방식을 대조하여 CSS 함수를 사용하는 것이 다른 옵션보다 CSS 애니메이션을 생성하는 데 더 많은 혜택이 있는지를 결정하겠습니다.

### CSS 함수를 사용하여 애니메이션 생성하기

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

음악 비트 바 애니메이션을 CSS 함수를 사용하여 만들어 보겠습니다. 이 애니메이션은 여러 막대를 애니메이션화하여 CSS 함수로 생성된 값으로 높이와 배경색 속성을 변경하는 데 중점을 둡니다.

여기서 위의 코드 블록을 분석해보겠습니다:

- 변수 --t의 값을 무한히 변경하는 루트 애니메이션을 만듭니다.
- body와 container 클래스에 스타일을 지정합니다.
- 바 클래스의 초기 상태를 생성하고 애니메이션에서 사용할 몇 가지 변수를 선언합니다. 여기서 우리는 트랜지션 속성을 생성하고 배경색을 동적으로 생성하기 위해 round() 및 mod() CSS 함수를 사용합니다.
- 그 다음, 각 막대에 높이 전환을 적용합니다. 이는 변수에서 얻은 값에 따라 다릅니다.
- 마지막으로 HTML 요소 구조가 있습니다.

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

<img src="https://res.cloudinary.com/practicaldev/image/fetch/s--uVfE0NdB--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://blog.logrocket.com/wp-content/uploads/2024/07/Music-beat-bar-animation-css.gif%3Fw%3D800" />

### CSS 키프레임으로 애니메이션 재현하기

이 섹션에서는 음악바 애니메이션을 다시 만들어보겠습니다. 하지만 우리는 애니메이션과 CSS 키프레임을 사용할 것입니다.

```js
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CSS 키프레임으로 비트바 애니메이션 만들기</title>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #222;
      margin: 0;
    }

    .container {
      height: 500px;
      width: 250px;
      position: relative;
      display: flex;
      gap: 10px;
    }

    .bar {
      position: absolute;
      bottom: 0;
      width: 20px;
      height: 20px;
      --amplitude: 30px;
      --base-height: 20px;
      --frequency: 1;
      transition: height 0.5s ease-in-out;
      animation: bounce 1s infinite;
    }

    @keyframes bounce {

      0%,
      100% {
        height: calc(var(--base-height) + var(--amplitude) * 0.5);
      }

      50% {
        height: calc(var(--base-height) + var(--amplitude));
      }
    }

    .bar1 {
      animation: bounce1 1s infinite, colorChange1 2s infinite;
      margin-left: 10px;
    }

    .bar2 {
      animation: bounce2 1s infinite, colorChange2 2s infinite;
      margin-left: 30px;
    }

    .bar3 {
      animation: bounce3 1s infinite, colorChange3 2s infinite;
      margin-left: 50px;
    }

    .bar4 {
      animation: bounce4 1s infinite, colorChange4 2s infinite;
      margin-left: 70px;
    }

    .bar5 {
      animation: bounce5 1s infinite, colorChange5 2s infinite;
      margin-left: 90px;
    }

    .bar6 {
      animation: bounce6 1s infinite, colorChange6 2s infinite;
      margin-left: 110px;
    }

    .bar7 {
      animation: bounce7 1s infinite, colorChange7 2s infinite;
      margin-left: 130px;
    }

    .bar8 {
      animation: bounce8 1s infinite, colorChange8 2s infinite;
      margin-left: 150px;
    }

    .bar9 {
      animation: bounce9 1s infinite, colorChange9 2s infinite;
      margin-left: 170px;
    }

    // 이하 생략
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

```html
<img src="https://res.cloudinary.com/practicaldev/image/fetch/s--3jymcR7n--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://blog.logrocket.com/wp-content/uploads/2024/07/Music-beat-bar-keyframes.gif%3Fw%3D600" />

### JavaScript를 사용하여 애니메이션 생성하기

여기에서는 HTML, CSS 및 JavaScript를 사용하여 이전 섹션의 애니메이션을 재현하는 방법을 보여드리겠습니다:

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Beats Bar Animation with JavaScript</title>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #222;
      margin: 0;
    }

    .container {
      display: flex;
      gap: 0;
      position: relative;
    }

    .bar {
      width: 20px;
      position: absolute;
      bottom: 0;
      transition: height 0.1s ease-in-out, background 0.1s ease-in-out;
    }

    .bar1 {
      left: 0;
    }

    .bar2 {
      left: 20px;
    }

    .bar3 {
      left: 40px;
    }

    .bar4 {
      left: 60px;
    }

    .bar5 {
      left: 80px;
    }

    .bar6 {
      left: 100px;
    }

    .bar7 {
      left: 120px;
    }

    .bar8 {
      left: 140px;
    }

    .bar9 {
      left: 160px;
    }
  </style>
</head>

<body>
  <div class="container">
    <div class="bar bar1" data-index="1"></div>
    <div class="bar bar2" data-index="2"></div>
    <div class="bar bar3" data-index="3"></div>
    <div class="bar bar4" data-index="4"></div>
    <div class="bar bar5" data-index="5"></div>
    <div class="bar bar6" data-index="6"></div>
    <div class="bar bar7" data-index="7"></div>
    <div class="bar bar8" data-index="8"></div>
    <div class="bar bar9" data-index="9"></div>
  </div>

  <script>
    const bars = document.querySelectorAll('.bar');
    const baseHeight = 100; // 바의 기본 높이
    const amplitude = 150; // 높이 변경의 진폭
    const frequency = 2; // 애니메이션의 주파수
    const animationSpeed = 0.1; // 애니메이션 속도

    function animateBars() {
      const currentTime = Date.now() / 1000; // 현재 시간(초)을 사용하여 플럭스 값으로 사용

      bars.forEach((bar, index) => {
        // 현재 시간을 기반으로 바의 높이 계산
        const timeOffset = index * frequency;
        const height = baseHeight + amplitude * Math.abs(Math.sin(currentTime * frequency + timeOffset));
        bar.style.height = `${height}px`;

        const hue = (currentTime * 50) % 360; // 시간에 따라 변하는 동적 색조
        const alpha = 1; // 투명도가 없도록 alpha 1 설정
        // 바의 배경색을 선형 그라데이션을 사용하여 설정
        bar.style.background = `linear-gradient(to top, hsla(${hue}, 100%, 50%, ${alpha}), hsla(${(hue + 180) % 360}, 100%, 50%, ${alpha}))`;
      });

      requestAnimationFrame(animateBars);
    }

    function initializeBars() {
      // 바의 초기 높이와 색상 설정
      bars.forEach((bar, index) => {
        const initialHeight = baseHeight + amplitude * Math.abs(Math.sin(index * frequency));
        bar.style.height = `${initialHeight}px`;

        const initialHue = (index * 50) % 360; // 인덱스에 따른 초기 색조
        const initialAlpha = 1; // 투명도가 없도록 초기 alpha 1 설정
        bar.style.background = `linear-gradient(to top, hsla(${initialHue}, 100%, 50%, ${initialAlpha}), hsla(${(initialHue + 180) % 360}, 100%, 50%, ${initialAlpha}))`;
      });
    }

    // 초기 높이와 색상을 가진 bars 초기화
    initializeBars();

    // 애니메이션 시작
    animateBars();
  </script>
</body>

</html>
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

<img src="https://res.cloudinary.com/practicaldev/image/fetch/s--9B10SJZp--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://blog.logrocket.com/wp-content/uploads/2024/07/Music-beat-bar-javascript.gif%3Fw%3D600" />

### 코드 비교

특정 기술적 측면을 사용하여 이러한 다양한 방법을 비교해 봅시다:

위에서 볼 수 있듯이 CSS를 애니메이션에 사용할 때 다른 구현 방법과 비교했을 때 간결성, 코드 재사용성, 제어 및 성능 면에서 우수하다는 것을 알 수 있습니다.

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

## 결론

이 글에서는 mod()부터 round() 그리고 삼각함수까지 다룬 시간 기반 애니메이션에 대해 살펴보았습니다.

우리는 이러한 함수들을 keyframes와 Javascript와 비교하며, 시간 기반 애니메이션이 주로 단순성, 향상된 재사용성, 그리고 성능 최적화로부터 성장한다는 것을 알 수 있었습니다. 이들은 가벼우며 복잡한 애니메이션과 비교했을 때 성능에 미치는 영향이 적어 성능 최적화에 도움이 됩니다.

이는 결과적으로 사용자 경험을 향상시켜줍니다. 계속해서 이러한 함수들을 탐구하고, 코딩을 계속해보세요!!

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

## 프론트엔드가 사용자의 CPU를 소비하고 있나요?

웹 프론트엔드가 점점 복잡해지면, 리소스를 많이 요구하는 기능들이 브라우저로부터 더 많은 것을 요구하게 됩니다. 프로덕션 환경에서 모든 사용자의 클라이언트 측 CPU 사용량, 메모리 사용량 등을 모니터링하고 추적하려면 LogRocket을 시도해보세요.

![image](/assets/img/2024-08-26-OptimizingCSStime-basedanimationswithnewCSSfunctions_2.png)

LogRocket은 웹 앱, 모바일 앱 또는 웹사이트에서 발생하는 모든 일들을 기록하는 웹 및 모바일 앱용 DVR과 같습니다. 문제가 발생한 이유를 추측하는 대신, 핵심 프론트엔드 성능 지표를 집계하고 보고하며, 사용자 세션을 애플리케이션 상태와 함께 다시 재생하고, 네트워크 요청을 기록하며, 모든 오류를 자동으로 노출합니다.

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

웹 및 모바일 앱의 디버깅을 현대화하세요 - 무료 모니터링을 시작하세요.