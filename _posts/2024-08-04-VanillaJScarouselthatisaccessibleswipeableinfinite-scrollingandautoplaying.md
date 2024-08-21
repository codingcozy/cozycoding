---
title: "자바스크립트로 캐러셀 만들기 스와이프, 무한 스크롤, 자동 재생 기능"
description: ""
coverImage: "/assets/img/2024-08-04-VanillaJScarouselthatisaccessibleswipeableinfinite-scrollingandautoplaying_0.png"
date: 2024-08-04 19:58
ogImage: 
  url: /assets/img/2024-08-04-VanillaJScarouselthatisaccessibleswipeableinfinite-scrollingandautoplaying_0.png
tag: Tech
originalTitle: "Vanilla JS carousel that is accessible, swipeable, infinite-scrolling, and autoplaying"
link: "https://dev.to/masakudamatsu/vanilla-js-carousel-that-is-accessible-swipeable-infinite-scrolling-and-autoplaying-19kc"
isUpdated: true
updatedAt: 1724246202403
---


## TL;DR

접근성이 뛰어나고 스와이프 및 무한 스크롤이 가능하며 자동 재생되는 캐러셀을 순수 JS로 아래와 같이 작성할 수 있습니다.

접근성을 고려하여 ARIA Authoring Practices Guide의 지침을 구현하세요 (본 문서의 섹션 1 참조).

스와이프 기능을 위해서는 CSS Scroll Snap을 사용하세요 (섹션 2).

<div class="content-ad"></div>

무한 스크롤링을 위해, 첫 번째(마지막) 슬라이드를 복제하고, 맨 오른쪽(왼쪽)에 마지막(처음) 슬라이드를 배치하고, scrollTo() 메서드를 사용하여 원래 슬라이드로 즉시 "스크롤"하여 무한 스크롤링의 환상을 만들 수 있습니다(섹션 4).

자동 재생을 위해 setInterval() 메서드를 사용합니다. 사용자 경험을 향상시키기 위해 자동 재생을 중지하려면 Intersection Observer API 및 pointerenter, focus, touchstart 및 resize 이벤트에 대한 이벤트 리스너를 사용하세요(섹션 5).

구체적인 코드는 이 문서의 CodePen 데모에서 찾을 수 있습니다.

## 왜 이 글을 쓰게 되었을까요?

<div class="content-ad"></div>

바닐라 JavaScript를 사용하여 처음부터 캐러셀을 만드는 방법에 대한 기사가 많이 있어요. 그러나 제가 알기로는 특정 유형의 캐러셀에 대해 다룬 기사가 없어요: (1) 접근성, (2) 스와이프 기능, (3) 무한 스크롤, 그리고 (4) 자동 재생.

2024년에 발간된 W3C 웹 접근성 이니셔티브 기사에서 (1) 접근성, (3) 무한 스크롤, 그리고 (4) 자동 재생이 가능한 캐러셀을 만드는 방법을 설명하고 있어요. 하지만 스와이프 기능은 지원하지 않아요.

반 데어 스키(2021)가 제시한 캐러셀은 (2) 스와이프 기능과 (4) 자동 재생이 가능해요. 아주 멋지게 구현되어 있지만, 접근성이나 무한 스크롤은 지원하지 않아요.

불잔(2022)은 CSS 애니메이션을 사용하여 (3) 무한 스크롤과 (4) 자동 재생이 가능한 캐러셀의 코드 스니펫을 제공해요. 우아한 코드 스니펫이지만, 접근성이나 스와이프 기능은 지원하지 않아요.

<div class="content-ad"></div>

이 기사는 이 세 가지 캐러셀에서 최상의 코드 부분을 추출하여 접근성, 스와이프 가능, 무한 스크롤 및 자동 재생이 가능한 캐러셀을 만들기 위해 결합했습니다.

![이미지](/assets/img/2024-08-04-VanillaJScarouselthatisaccessibleswipeableinfinite-scrollingandautoplaying_0.png)

## 1. 접근성에 대한 HTML

이 섹션은 ARIA Authoring Practices Guide (W3C Web Accessibility Initiative 2024a 및 2024b)에 많이 의존합니다.

<div class="content-ad"></div>

웹 접근성을 위한 다양한 웹 사이트가 있지만, 제가 본 바로는 ARIA Authoring Practices Guide가 가장 권위 있는 것 같아요.

### 1.1 캐러셀 컨테이너

우리는 `div` 클래스가 "carousel"인 컨테이너로 시작합니다:

```js
<div 
  class="carousel"
  role="group" 
  aria-label="Ryoan-ji Temple’s Rock Garden" 
  aria-roledescription="carousel" 
>
</div>
```

<div class="content-ad"></div>

접근성을 고려하면 세 가지 속성이 필요합니다:

- role="group"은 스크린 리더에게 모든 하위 요소가 하나의 UI 개체로 함께 작동하는 관련 기능을 가지고 있다고 알려줍니다 (MDN 기고자들 2024a).
- aria-label은 스크린 리더에게 캐러셀이 무엇인지 알려줍니다. 캐러셀 제목이 캐러셀 컨테이너 내부의 다른 HTML 요소인 경우 aria-labelledby을 대신 사용할 수 있습니다.
- aria-roledescription="carousel"은 스크린 리더에게 `div` 요소가 캐러셀임을 알려줍니다. W3C 웹 접근성 이니셔티브 (2024b)에 따르면, 웹 페이지가 영어가 아닌 다른 언어로 작성된 경우 "carousel" 대신 해당 언어의 해당 단어를 사용하십시오 (예: 일본어의 경우 aria-roledescription="スライダー" 사용).

W3C 웹 접근성 이니셔티브 (2024a)에 따르면, role="group"은 사용해서는 안되지만, 캐러셀이 랜드마크 영역이면 (즉, 페이지의 상단 수준 섹션인 경우에는 `header`, `footer`, `main`, `aside`, `nav`, `form`에 옆에 있는) 사용해야 합니다. 이 경우 `section`을 사용하세요:

```js
<section
  class="carousel"
  aria-label="Ryoan-ji Temple’s Rock Garden"
  aria-roledescription="carousel"
>
</section>
```

<div class="content-ad"></div>

아래에서는 캐러셀이 거의 항상 사실일 것으로 예상되는 랜드마크 영역이 아님을 가정합니다.

### 1.2 슬라이드 랩퍼

캐러셀 컨테이너 내에서 먼저 슬라이드 래퍼 `div`를 추가합니다. 클래스는 "carousel__slides"로 지정하였습니다(BEM 규칙을 따르는 것으로 가정):

```js
<div 
  class="carousel"
  role="group" 
  aria-label="량택지의 바위 정원" 
  aria-roledescription="캐러셀" 
>
  <!-- 여기서 추가 -->
  <div
    class="carousel__slides"
    aria-atomic="false" 
    aria-live="off" 
  >
  </div>
  <!-- 여기까지 추가 -->
</div>
```

<div class="content-ad"></div>

접근성을 위해 두 가지 ARIA 속성이 필요합니다:

- aria-atomic="false"는 화면 판독기에게 해당 요소의 텍스트 내용이 업데이트될 때 전체 내용이 아닌 업데이트된 부분만 발표해야 한다고 알려줍니다 (MDN 기고자들 2024b).
- aria-live="off"은 텍스트 내용이 업데이트될 때 화면 판독기가 발표하지 않도록 하는 속성입니다. 다른 부분을 읽고 있는 동안 캐러셀이 자동으로 재생되면 화면 판독기 사용자들은 다음 슬라이드가 나타날 때마다 업데이트된 텍스트 내용을 듣고 싶어하지 않습니다 (W3C 웹 접근성 이니셔티브 2024b).

캐러셀 자동 재생이 비활성화될 때 aria-live 속성 값은 polite로 대체되어야 합니다. 이렇게 하면 화면 판독기 사용자가 슬라이드를 수동으로 전환할 때마다 업데이트된 내용이 발표됩니다.

우리는 섹션 5.2 및 5.3에서 autoplay가 꺼질 때(그리고 켜질 때) aria-live 값을 토글하는 데 바닐라 JS를 사용할 것입니다.

<div class="content-ad"></div>

### 1.3 슬라이드 컨테이너

4개의 슬라이드 컨테이너를 추가해 봅시다. 물론, 4개 이상의 슬라이드를 추가할 수도 있어요. 그러나 이 글에서는 설명을 쉽게 하기 위해 4개의 슬라이드로 제한할 거에요.

```js
<div 
  class="carousel"
  role="group" 
  aria-label="Ryoan-ji Temple’s Rock Garden" 
  aria-roledescription="carousel" 
>
  <div
    class="carousel__slides"
    aria-atomic="false" 
    aria-live="off" 
  >
    <!-- 여기부터 추가 -->
    <div
      class="carousel__slide"
      role="group"
      aria-label="1 of 4"
      aria-roledescription="slide"
    >
    </div>
    <div
      class="carousel__slide"
      role="group"
      aria-label="2 of 4"
      aria-roledescription="slide"
    >
    </div>
    <div
      class="carousel__slide"
      role="group"
      aria-label="3 of 4"
      aria-roledescription="slide"
    >
    </div>
    <div
      class="carousel__slide"
      role="group"
      aria-label="4 of 4"
      aria-roledescription="slide"
    >
    </div>
    <!-- 여기까지 추가 -->
  </div>
</div>
```

각 슬라이드 컨테이너인 `div class="carousel__slide"`는 접근성을 위해 세 가지 속성을 가져야 합니다:

<div class="content-ad"></div>

- role="group"은 화면 판독기에게 하위 요소가 전체적으로 하나의 슬라이드를 이룬다는 것을 알려줍니다.
- aria-label은 각 슬라이드를 식별합니다. 위 예시에서는 "1 of 4" 등을 사용하여 현재 읽고 있는 슬라이드가 무엇인지 화면 판독기 사용자에게 알려줍니다. 그러나 각 슬라이드를 식별하기 위해 다른 문자열을 사용할 수도 있습니다.
- aria-roledescription="slide"는 화면 판독기에게 `div` 요소가 슬라이드임을 알려줍니다. W3C 웹 접근성 이니셔티브 (2024b)에 따르면, 웹 페이지가 영어가 아닌 다른 언어로 작성된 경우 "slide" 대신 해당 언어의 단어를 사용해야 합니다 (예: 일본어의 경우 aria-roledescription="スライド").

### 1.4 슬라이드 콘텐츠

각 슬라이드 컨테이너는 하위 요소로 단일 `img` 요소부터 상호작용하는 카드를 이루는 요소 세트까지 모두 가질 수 있습니다.

설명의 편의를 위해, 이 글에서는 단일 `img` 요소를 계속 사용합니다:

<div class="content-ad"></div>


<div 
  class="carousel"
  role="group" 
  aria-label="Ryoan-ji Temple’s Rock Garden" 
  aria-roledescription="carousel" 
>
  <div
    class="carousel__slides"
    aria-atomic="false" 
    aria-live="off" 
  >
    <div
      class="carousel__slide"
      role="group"
      aria-label="1 of 4"
      aria-roledescription="slide"
    >
      <!-- 이곳에서 추가 -->
      <img src="https://translating-japanese-gardens.pages.dev/ryoanji/ryoanji-banner-spring-1882.jpg" width="991" height="702"/>
      <!-- 여기까지 추가 -->
    </div>
    <div
      class="carousel__slide"
      role="group"
      aria-label="2 of 4"
      aria-roledescription="slide"
    >
      <!-- 이곳에서 추가 -->
      <img src="https://translating-japanese-gardens.pages.dev/ryoanji/ryoanji-banner-summer-1882.jpg" width="991" height="702"/>
      <!-- 여기까지 추가 -->
    </div>
    <div
      class="carousel__slide"
      role="group"
      aria-label="3 of 4"
      aria-roledescription="slide"
    >
      <!-- 이곳에서 추가 -->
      <img src="https://translating-japanese-gardens.pages.dev/ryoanji/ryoanji-banner-autumn-1882.jpg" width="991" height="702"/>
      <!-- 여기까지 추가 -->
    </div>
    <div
      class="carousel__slide"
      role="group"
      aria-label="4 of 4"
      aria-roledescription="slide"
    >
      <!-- 이곳에서 추가 -->
      <img src="https://translating-japanese-gardens.pages.dev/ryoanji/ryoanji-banner-winter-1882.jpg" width="991" height="702"/>
      <!-- 여기까지 추가 -->
    </div>
  </div>
</div>


여기에는 제가 촬영한 교토에있는 유네스코 세계유산인 료안지 사원의 암자 정원을 담은 사진 네 장을 포함했습니다. 웹 접근성을 위해 대체 텍스트를 추가하고, 카루셀이 페이지 아래에 있다면 성능을 위해 loading="lazy"로 레이지 로딩을 구현해야합니다. 하지만 이 글의 주된 주제는 아니므로 해당 사항은 생략했습니다.

### 1.5 내비게이션 점

카루셀 슬라이드를 탐색하는 작은 점들을 종종 내비게이션 점이라고 합니다 (또는 navdots로 줄여서 부릅니다).


<div class="content-ad"></div>

많은 UX 디자인 전문가들은 네비게이션 닷을 피해야 한다고 말합니다. 왜냐하면 네비게이션 닷은 클릭하기에 너무 작기 때문이라고 합니다 (예: 프리드만 2022, 네비게이션 닷에 대안을 제안합니다). 하지만 제 겸손한 의견으로는, 네비게이션 닷은 사용자가 그들이 보고 있는 것이 캐로셀이라는 것을 실제로 이해하는 데 도움이 될 만큼 충분히 흔하다고 가정할 수 있습니다. 따라서 이 기사에서는 네비게이션 닷을 사용하겠습니다.

사용자 인터페이스 디자인 측면에서, 터치 기기 사용자들은 네비게이션 닷을 탭할 때 자신의 손에 의해 슬라이드가 가려지지 않도록 슬라이드 아래에 렌더링되어야 합니다.

그러나 접근성 측면에서는, 화면 판독기 사용자들이 네비게이션 닷을 눌러 슬라이드를 읽은 후 다시 슬라이드를 읽어야 하기 때문에 네비게이션 닷은 슬라이드 앞에 DOM 트리에 나와야 합니다 (웨켄만 2023):

```js
<div 
  class="carousel"
  role="group" 
  aria-label="Ryoan-ji Temple’s Rock Garden" 
  aria-roledescription="carousel" 
>
  <!-- 이 부분부터 추가 -->
  <div
    class="carousel__navdots"
    role="group"
    aria-label="Choose slide to display"
  >
    <button type="button" aria-label="1 of 4" aria-disabled="true"></button>
    <button type="button" aria-label="2 of 4" aria-disabled="false"></button>
    <button type="button" aria-label="3 of 4" aria-disabled="false"></button> 
    <button type="button" aria-label="4 of 4" aria-disabled="false"></button> 
  </div>
  <!-- 여기까지 추가 -->
  <div
    class="carousel__slides"
    aria-atomic="false" 
    aria-live="off" 
  >
    <!-- 요약을 위해 생략됨 -->
  </div>
</div>
```

<div class="content-ad"></div>

내비게이션 도트 컨테이너는 접근성을 위해 다음 두 가지 속성이 필요합니다:

- role="group"은 스크린 리더에게 그 하위 요소들이 네비게이션 컨트롤로서 함께 작동한다는 것을 알려줍니다.
- aria-label은 스크린 리더에게 이 버튼 그룹이 어떤 목적으로 사용되는지 알려줍니다.

각 네비게이션 도트는 그에 상응하는 `button type="button"` 요소로 렌더링되며 두 개의 ARIA 속성이 존재합니다.

먼저, aria-label은 해당 슬라이드의 접근 가능한 이름을 나타냅니다. 여기서 이론적으로는 aria-labelledby을 사용하여 각 슬라이드의 id 속성값을 참조할 수 있습니다. 그러나 놀랍게도, aria-labelledby의 구현은 스크린 리더별로 다릅니다(PowerMapper 2023, GrahamTheDev 2020에 인용됨). 각 네비게이션 도트를 직접 레이블링하는 것이 가장 좋습니다.

<div class="content-ad"></div>

첫째로, 현재 슬라이드에 해당하는 네비게이션 점에 대해 aria-disabled가 true 여야 합니다. 이렇게 하면 화면 낭독기 사용자에게 현재 표시된 슬라이드를 알려줍니다. 이 값을 섹션 3.6에서 바닐라 자바스크립트로 토글할 것입니다.

네비게이션 점을 접근 가능하게 만드는 방법에 대해서는 여기까지입니다. 그러나 넘어가기 전에 네비게이션 점을 마크업하는 몇 가지 대안적 접근 방법에 대해 논의해보겠습니다.

첫째로, `a` 요소를 사용하여 각 슬라이드에 id 속성을 할당하여 슬라이드로 이동하는 점프 링크로 네비게이션 점을 마크업할 수 있습니다. 간단한 예시는 Coyier (2019)에서 확인할 수 있습니다. van der Schee (2021)의 스와이프 및 자동 재생 가능한 캐러셀도 이 접근 방식을 사용합니다.

그러나 점프 링크 접근 방식은 섹션 4에서 볼 수 있듯이 무한 스크롤링 캐러셀을 만드는 데 편리하지 않습니다. 이것이 바로 `button` 요소를 사용하여 캐러셀을 스크롤하는 것을 선택한 이유입니다.

<div class="content-ad"></div>

두 번째로, 네비게이션 도트를 탭으로 표시할 수도 있습니다. 이 방법은 "탭형" 패턴으로 알려져 있습니다(이 글에서 채택된 "그룹화" 패턴과는 다릅니다). "탭형" 패턴을 구현하는 방법에 대해서는 W3C 웹 접근성 이니셔티브 (2024년)와 Deque Systems (날짜 미상)를 참조해보세요.

그러나 Webb(2021)는 시력이 낮거나 시각 장애가 있는 사용자들이 "탭형" 패턴을 매우 혼란스러워 한다고 보고하고 있습니다. 이 증거를 고려할 때, 나는 네비게이션 도트를 표시하는 데 "그룹화" 패턴이 더 나은 것으로 생각합니다.

## 2. 스와이프하여 슬라이드하기 위한 CSS

접근성을 고려해 HTML을 수정했으니, 터치 디바이스 사용자가 캐로셀을 스와이프하여 이전 및 다음 슬라이드를 볼 수 있도록 CSS 작업을 시작해봅시다.

<div class="content-ad"></div>

### 2.1 각 슬라이드 너비 설정

우선, 캐러셀과 각 슬라이드의 너비를 설정하세요:

```js
.carousel {
  width: 991px;
}
.carousel__slides,
.carousel__slide {
  width: 100%;
}
```

위 코드는 각 이미지가 1882px 너비이므로 각 슬라이드가 991px 너비인 캐러셀을 구현하며, 한 번에 하나의 슬라이드만 표시됩니다.

<div class="content-ad"></div>

만약 창의 가장자리에서 부분적으로 가려져있을 수 있는 중앙 슬라이드 양쪽에 여러 슬라이드를 보여주고 싶다면, 위의 CSS 코드를 아래와 같이 다시 작성하세요:

```js
.carousel {
  width: 100%;
}
```

여기서는 캐로셀 너비를 부모 요소에 위임합니다. 슬라이드 래퍼와 각 슬라이드의 너비를 명시적으로 지정하지 않으면, 이미지의 본래 너비가 각 슬라이드의 너비가 되며, 슬라이드 래퍼는 캐로셀 컨테이너와 동일한 너비를 가집니다.

### 2.2 CSS 스크롤 스냅을 위한 슬라이드

<div class="content-ad"></div>

카로셀을 스와이프할 수 있게 하는 데 가장 중요한 CSS 부분은 다음과 같아요:

```js
.carousel__slides {
  display: flex;
  column-gap: 20px; /* 선택 사항 */
  overflow: auto;
  scroll-snap-type: x mandatory;
}
.carousel__slide {
  flex: 0 0 auto;
  scroll-snap-align: center;
}
```

display: flex는 슬라이드를 가로로 배치하며, overflow: auto는 슬라이드 래퍼를 스크롤하여 넘치는 슬라이드를 볼 수 있게 해요. column-gap은 선택 사항이에요. 이는 슬라이드 쌍 사이에 여백을 추가할 수 있게 해요. flex: 0 0 auto는 각 슬라이드가 카로셀 너비에 의해 예상치 못하게 확대되거나 축소되지 않도록 해줘요.

슬라이드 래퍼의 중앙에 현재 슬라이드를 표시하는 데 있어서 중요한 것은 scroll-snap-type(슬라이드 래퍼용)와 scroll-snap-align: center(각 개별 슬라이드용)입니다. 이 기법은 CSS 스크롤 스냅이라고 불리며 다른 곳에서도 잘 문서화되어 있어요 (예: Rifki(2020)).

<div class="content-ad"></div>

이렇게 하면 터치 기기 사용자가 캐러셀을 좌측 또는 우측으로 스와이프하여 처음에 숨겨져 있던 슬라이드를 드러낼 수 있어요. CSS Scroll Snap은 각 스와이핑 동작 후 슬라이드가 중앙에 표시되도록 보장해줘요.

2020년 경에 모든 주요 브라우저가 CSS Scroll Snap을 지원하기 이전에 많은 캐러셀 플러그인이 개발되었어요. 이는 JavaScript를 사용하여 동일한 기능을 구현하게 되어 번들 크기가 커지고 성능이 나빠지게 했어요 (참조: Hempenius 2021).

가장 인기 있는 캐러셀 라이브러리인 Swiper는 CSS Scroll Snap을 지원하도록 API를 업데이트하여 CSS 모드라고 부르고 있어요. 제 생각에는 이로 인해 Swiper의 API가 필요 이상으로 복잡해 보이며, 이것이 저에게 캐러셀을 처음부터 구축하고 (그리고 이 기사를 쓰게 된) 계기가 되었어요.

### 2.3 스크롤 바 비활성화

<div class="content-ad"></div>

하지만, 위의 CSS 코드에서 스크롤 바가 나타납니다. 네비게이션 점이 동일한 목적을 제공하기 때문에 스크롤 바는 필요하지 않습니다. 그러나 스크롤 바를 숨기는 것은 약간 까다로울 수 있습니다:

```js
.carousel__slides {
  scrollbar-width: none; /* Firefox 및 최신 Chromium을 위한 코드 */
}
.carousel__slides::-webkit-scrollbar {
  display: none; /* Safari 및 이전 Chromium을 위한 코드 */
}
```

scrollbar-width 속성은 Firefox(2018년 이후) 및 일부 2024년 초에 출시된 Chromium 브라우저에서만 지원됩니다 (Can I Use 2024). 대안으로 ::-webkit-scrollbar 가상 요소가 필요합니다 (자세한 내용은 John의 2023 논문을 참조하세요).

## 3. 네비게이션 점을 위한 CSS 및 JS

<div class="content-ad"></div>

네비게이션 점을 스타일링하고 동적으로 접근할 수 있게 만들어 봅시다.

### 3.1 네비게이션 점 위치 지정

먼저, 슬라이드 아래에 네비게이션 점을 위치시킵니다. 위의 섹션 1.5에서 HTML 코드에서 먼저 네비게이션 점을 작성하고 나중에 슬라이드를 작성했습니다. CSS로 이 순서를 반대로 변경해야 합니다.

```js
.carousel {
  padding-bottom: 60px;
  position: relative;
}
.carousel__navdots {
  bottom: 0;
  position: absolute;
}
```

<div class="content-ad"></div>

position: relative를 사용하면 내비게이션 도트를 카루셀 컨테이너 내의 어디에든 배치할 수 있습니다. padding-bottom은 내비게이션 도트를 위해 슬라이드 아래에 공간을 만듭니다. 당연히 여러분의 디자인을 위해 어떤 값이든 설정할 수 있습니다.

그런 다음, 내비게이션 도트 컨테이너를 카루셀 컨테이너의 하단 가장자리에 배치합니다 (bottom: 0).

이후, 도트 레이아웃을 설정하기 위해 내비게이션 도트 컨테이너를 스타일링합니다:

```js
.carousel__navdots {
  bottom: 0;
  column-gap: 16px; /* 추가함 */
  display: flex;  /* 추가함 */
  justify-content: center;  /* 추가함 */
  left: 0; /* 추가함 */
  position: absolute;
  right: 0; /* 추가함 */
}
```

<div class="content-ad"></div>

플렉스박스와 column-gap을 함께 사용하여 네비게이션 점들을 동일한 간격으로 배치할 수 있습니다(이 예시에서는 16px). justify-content: center와 left: 0 및 right: 0의 조합을 사용하여 네비게이션 점을 캐러셀 컨테이너를 기준으로 수평으로 중앙 정렬할 수 있습니다.

### 3.2 각 네비게이션 점 스타일링

두 번째로, 각 개별 네비게이션 점을 스타일링합니다:

```js
.carousel__navdots button {
  /* 기본 버튼 스타일 초기화 */
  -moz-appearance: none;
  -webkit-apperance: none;
  appearance: none;
  border: 0;
  cursor: pointer;
  /* 회색 점 스타일로 설정 */
  background-color: #9a9a9a; 
  border-radius: 50%;
  height: 10px;
  padding: 0;
  width: 10px;
}
```

<div class="content-ad"></div>

첫 다섯 가지 CSS 속성은 `button` 요소의 기본 스타일을 재설정합니다 (Shadeed 2020). 나머지는 당신에게 맡깁니다. 여기에서 각 점은 지름이 10px인 회색 원입니다.

### 3.3 동적 스타일링

마지막으로, 내비게이션 점들의 포커스와 활성 상태를 스타일링하세요:

```js
.carousel__navdots button:focus-visible {
  outline: 2px solid #0060a8; /* 파란색 */
  outline-offset: 2px;
}
.carousel__navdots button.is-active {
  background-color: #0060a8; /* 파란색 */
}
```

<div class="content-ad"></div>

### 3.4 클릭 이벤트 핸들러를 위한 JS

현재 표시되는 슬라이드를 나타내기 위해 JavaScript로 .is-active 클래스가 추가될 것입니다.

다음 서브 섹션에서는 Buljan (2022)이 제안한 기술에 크게 의존합니다.

우선 캐러셀의 구성 요소를 수집해 봅시다:

<div class="content-ad"></div>

```js
// 구성요소
const carouselContainer = document.querySelector('.carousel');
const slideWrapper = document.querySelector('.carousel__slides');
const slides = document.querySelectorAll('.carousel__slide');
const navdotWrapper = document.querySelector('.carousel__navdots');
const navdots = document.querySelectorAll('.carousel__navdots button');
```

다음으로 매개변수를 가져오거나 설정하십시오:

```js
// 매개변수
const n_slides = slides.length;
const n_slidesCloned = 0;
let slideWidth = slides[0].offsetWidth;
let spaceBtwSlides = Number(window.getComputedStyle(slideWrapper).getPropertyValue('grid-column-gap').slice(0, -2)); // 마지막에 px 삭제
function index_slideCurrent() {
  return Math.round(slideWrapper.scrollLeft / (slideWidth + spaceBtwSlides) - n_slidesCloned);
}
```

첫 번째 매개변수인 n_slides는 단순히 캐로셀의 슬라이드 수입니다. 숫자를 하드코딩하는 대신에 slides.length로 설정하여 새로운 슬라이드가 캐로셀에 추가될 때 코드를 변경할 필요가 없도록 설정되어 있습니다.

<div class="content-ad"></div>

두 번째로 n_slidesCloned는 캐러셀을 무한 스크롤링할 때 중요한 역할을 합니다. 그러나 현재는 0으로 설정되어 있습니다.

세 번째로 slideWidth는 각 슬라이드의 너비입니다. 모든 슬라이드의 너비가 동일하다고 가정합니다. 사용자가 브라우저 창 너비를 조정할 때 슬라이드의 너비가 변경될 수 있으므로 let으로 정의되어 있습니다 (아래 3.7절 참조).

네 번째로 spaceBtwSlides는 서로 이웃한 두 슬라이드 사이의 여백입니다. 다시 한번, 브라우저 창 크기 조절에 따라 여백의 너비가 변경될 수 있으므로 let으로 정의되어 있습니다 (아래 3.7절 참조).

마지막으로 index_slideCurrent() 함수는 현재 표시된 슬라이드의 인덱스를 계산합니다 (slideWrapper.scrollLeft 값에 따라). 이 값을 사용하여 어떤 내비게이션 도트가 .is-active 클래스를 받아 파란색으로 색상이 변경되는지 결정합니다.

<div class="content-ad"></div>

그 다음, 각 네비게이션 닷에 클릭 이벤트 핸들러를 추가해 봅시다:

```js
// 네비게이션 닷 클릭 핸들러
function goto(index) {
  slideWrapper.scrollTo((slideWidth + spaceBtwSlides) * (index + n_slidesCloned), 0);
}
for (let i = 0; i < n_slides; i++) {
  navdots[i].addEventListener('click', () => goto(i));
}
```

이제 각 네비게이션 닷을 클릭하면 해당하는 슬라이드가 나타납니다.

### 3.5 부드러운 스크롤링

<div class="content-ad"></div>

navdot 클릭 핸들러가 scrollTo() 메서드를 실행할 때 슬라이드가 급격하게 스크롤됩니다. 그러나 사용자가 캐러셀을 가로로 스크롤하는 것처럼 보이는 환상을 만들고 싶습니다.

이를 위해 CSS 속성 scroll-behavior를 smooth로 설정해야 합니다:

```js
.carousel__slides.smooth-scroll {
  scroll-behavior: smooth;
}
```

여기서는 슬라이드 래퍼가 .smooth-scroll 클래스를 받을 때에만 부드럽게 스크롤되도록 적용하며, 이는 페이지 로드 시 JS 스크립트의 끝에서 처리됩니다:

<div class="content-ad"></div>

```js
// 초기화
slideWrapper.classList.add('smooth-scroll');
```

이렇게 하면 캐러셀을 무한 스크롤할 수 있게하기 위해 부드러운 스크롤을 프로그래밍 방식으로 켜고 끌 수 있습니다. 자세한 내용은 아래 섹션 4.2를 참조하세요.

### 3.6 동적으로 네비게이션 점 스타일링하는 JS

주어진 인덱스에 해당하는 네비게이션 점에 .is-active 클래스를 적용하는 도우미 함수로 시작합니다:

<div class="content-ad"></div>

```js
// 현재 슬라이드에 대한 네비게이션 점 표시하기
function markNavdot(index) {
  navdots[index].classList.add('is-active');
  navdots[index].setAttribute('aria-disabled', 'true');
}
```

만약 'aria-disabled' 속성 값의 토글을 왜 해야 하는지 잘 모르겠다면, 위의 섹션 1.5로 돌아가보세요.

그 후, 현재 표시된 슬라이드에 해당하는 네비게이션 점에 대해 markNavdot()를 실행하는 또 다른 도우미 함수를 생성합니다:

```js
// 표시된 네비게이션 점 업데이트하기
function updateNavdot() {
  const c = index_slideCurrent();
  if (c < 0 || c >= n_slides) return;
  markNavdot(c);
}
```

<div class="content-ad"></div>

두 번째 줄은 현재 필수가 아니지만, 캐로셀을 무한 스크롤로 만들 때 매우 중요한 역할을 합니다. 이때 첫 번째 슬라이드를 복사하여 마지막 슬라이드 오른쪽에 배치하면 c `= n_slides(현재 표시 중일 때)가 적용되거나, 마지막 슬라이드를 복사하여 첫 번째 슬라이드 왼쪽에 배치하면 c ` 0(현재 표시 중일 때)가 적용됩니다. 이러한 복제 슬라이드가 표시될 때, 우리는 즉시 원래 슬라이드로 교체하여 네비게이션 점을 업데이트하고 싶지 않습니다. 자세한 내용은 섹션 4를 참조하십시오.

그런 다음 스크롤 이벤트 핸들러를 사용하여 updateNavdot()을 실행합니다. 

```js
slideWrapper.addEventListener('scroll', () => {
  // reset
  navdots.forEach(navdot => {
    navdot.classList.remove('is-active');
    navdot.setAttribute('aria-disabled', 'false');
  });
  // mark the navdot
  updateNavdot();
});
```

사용자가 캐로셀을 스크롤할 때마다 스크롤 이벤트가 발생하며, 모든 네비게이션 점에서 is-active 클래스를 제거하고 현재 슬라이드에 해당하는 점을 표시합니다.

<div class="content-ad"></div>

마침내, 페이지 로딩 시 첫 번째 내비게이션 점을 표시합니다:

```js
// 초기화
markNavdot(0); // 추가됨
slideWrapper.classList.add('smooth-scroll');
```

이 섹션에 추가된 전체 JS 코드는 다음과 같습니다:

```js
// 현재 슬라이드에 대한 내비게이션 점 표시
function markNavdot(index) {
  navdots[index].classList.add('is-active');
  navdots[index].setAttribute('aria-disabled', 'true');
}
// 표시된 내비게이션 점 업데이트
function updateNavdot() {
  const c = index_slideCurrent();
  if (c < 0 || c >= n_slides) return;
  markNavdot(c);
}
slideWrapper.addEventListener('scroll', () => {
  // 초기화
  navdots.forEach(navdot => {
    navdot.classList.remove('is-active');
    navdot.setAttribute('aria-disabled', 'false');
  });
  // 내비게이션 점 표시
  updateNavdot();
});

// 초기화
markNavdot(0); // 추가됨
slideWrapper.classList.add('smooth-scroll');
```

<div class="content-ad"></div>

### 3.7 반응형 디자인

위의 코드는 캐러셀 슬라이드의 너비와 간격이 창 너비에 따라 변경될 때 문제를 일으킬 수 있습니다. 데스크톱 사용자들은 종종 브라우저 창 크기를 변경합니다. 그래서 우리는 resize 이벤트 핸들러를 설정하여 slideWidth와 spaceBtwSlides를 업데이트해야 합니다:

```js
window.addEventListener('resize', () => {
  // 파라미터 업데이트
  slideWidth = slides[0].offsetWidth;
  spaceBtwSlides = Number(window.getComputedStyle(slideWrapper).getPropertyValue('grid-column-gap').slice(0, -2)); // 마지막의 px를 삭제하고 문자열을 숫자로 변환
});
```

이 시점에서 표준 캐러셀이 정상적으로 작동 중입니다.

<div class="content-ad"></div>

이제 무한 스크롤 캐러셀을 만들 준비가 되었습니다.

## 4. 무한 스크롤링을 위한 JS

무한 스크롤링 캐러셀은 사용자 상호 작용을 잊고 구현한다면 상당히 쉽습니다. Oliver(2017)와 Pared(2021)는 둘 다 CSS 애니메이션을 사용하여 캐러셀을 무한 스크롤링하는 방법을 보여주고 있습니다.

한편, Buljan(2022)은 CSS 전환을 사용하여 사용자 상호 작용을 추가할 수 있도록 해줍니다. 이는 네비게이션 점과 이전/다음 버튼을 통한 상호 작용을 추가할 수 있게 합니다. 그러나 캐러셀이 translateX() CSS 함수로 "슬라이드"되므로 스와이핑 제스처는 비활성화됩니다.

<div class="content-ad"></div>

하지만 그의 방법은 나에게 깨달음을 주었어요: 사용자에게 무한 스크롤의 환상을 만들기 위해 첫 번째 슬라이드를 마지막 슬라이드 오른쪽에 복제하고 즉시 원래의 첫 번째 슬라이드로 "스크롤"해야 합니다.

이 섹션은 제가 스와이프할 수 있는 캐로셀에 대해 이 환상을 달성한 방법을 설명합니다. 아마 더 나은 방법이 있을 수도 있어요. 더 나은 방법을 알고 있다면 댓글을 남겨주세요.

### 4.1 슬라이드 복제하기

HTML 요소를 복제하려면 .cloneNode(true) 메서드를 사용할 수 있습니다:

<div class="content-ad"></div>

```js
const firstSlideClone = slides[0].cloneNode(true);
firstSlideClone.setAttribute('aria-hidden', 'true');
slideWrapper.append(firstSlideClone);
```

위의 코드는 복제된 첫 번째 슬라이드를 슬라이드 시리즈의 마지막에 추가합니다. 그러나 이 복제된 슬라이드는 스크린 리더 사용자에게는 의미가 없습니다. 그래서 aria-hidden: true를 추가했습니다.

마찬가지로, 복제된 마지막 슬라이드를 슬라이드 시리즈의 첫 번째로 추가할 수 있습니다:

```js
const lastSlideClone = slides[n_slides - 1].cloneNode(true);
lastSlideClone.setAttribute('aria-hidden', 'true');
slideWrapper.prepend(lastSlideClone);
```

<div class="content-ad"></div>

와이드 스크린 장치에서 여러 개의 슬라이드를 한 줄에 표시해야 할 때 두 번째 슬라이드 (그리고 뒤에서 두 번째 슬라이드)를 복제하고 그렇게 되면 이를 되풀이할 수 있습니다.

이제 파라미터 n_slidesCloned을 다음과 같이 변경해야 합니다:

```js
const n_slidesCloned = 1; /* 수정됨 */
```

이렇게 하면 올바른 탐색 닷이 표시됩니다 (위의 섹션 3.4를 참조하세요).

<div class="content-ad"></div>

### 4.2 원본으로 즉시 스크롤링 되돌아가기

이제 중복된 슬라이드를 원본 슬라이드와 함께 즉시 스크롤하고 사용자가 인지하지 못하게 하고 싶습니다. 그러나 부드러운 스크롤과 충돌하기 때문에 조심해야 합니다. 원본 슬라이드로 스크롤을 되돌릴 때 부드러운 스크롤을 비활성화해야 합니다. CSS의 부드러운 스크롤은 비활성화되기까지 시간이 걸리는 것을 경험적으로 알게 되었습니다.

슬라이드 래퍼에 .smooth-scroll 클래스를 적용해야 부드러운 스크롤을 활성화할 수 있다는 것을 명심하세요 (위의 섹션 3.5 참조). 따라서 첫 번째 슬라이드로 즉시 스크롤링 되돌아가는 도우미 함수는 다음과 같이 작성됩니다:

```js
function rewind() {
  slideWrapper.classList.remove('smooth-scroll');
  setTimeout(() => { // 부드러운 스크롤이 비활성화될 때까지 대기
    slideWrapper.scrollTo((slideWidth + spaceBtwSlides) * n_slidesCloned, 0);
    slideWrapper.classList.add('smooth-scroll');
  }, 100);
}
```

<div class="content-ad"></div>

100ms의 지연이 부드러운 스크롤을 비활성화하는 데 충분하다고 생각해요.

비슷하게, 마지막 슬라이드로 즉시 전환하는 함수는 아래와 같습니다:

```js
function forward() {
  slideWrapper.classList.remove('smooth-scroll');
  setTimeout(() => { // 부드러운 스크롤이 비활성화되기를 기다립니다
    slideWrapper.scrollTo((slideWidth + spaceBtwSlides) * (n_slides - 1 + n_slidesCloned), 0);
    slideWrapper.classList.add('smooth-scroll');
  }, 100);
}
```

scrollBy()나 scrollIntoView() 대신 scrollTo()을 사용하지 않는 이유가 궁금하실 수도 있어요. 저는 두 가지를 모두 시도해본 결과 신뢰성이 떨어진다는 것을 발견했어요. scrollBy()는 CSS Scroll Snap과 결합될 때 신뢰성이 떨어집니다: 사파리가 어떤 이유로 슬라이드를 가운데에 정렬하지 못하는 경우가 있어요. scrollIntoView()는 Werner(2019)의 코멘트에 따르면 가로 스크롤링의 신뢰성이 떨어지답니다.

<div class="content-ad"></div>

마지막 또는 첫 번째 슬라이드가 클론되어 화면에 완전히 나타난 후에 도우미 함수를 실행합니다. 이 때, 카루셀이 스크롤된다면, 스크롤 이벤트 핸들러(위의 섹션 3.6에서 정의된)를 다음과 같이 수정해야 합니다:

```js
// 스크롤 이벤트 처리
let scrollTimer; // 추가
slideWrapper.addEventListener('scroll', () => {
  navdots.forEach(navdot => {
    navdot.classList.remove('is-active');
    navdot.setAttribute('aria-disabled', 'false');
  });

  // 여기서부터 수정
  if (scrollTimer) clearTimeout(scrollTimer); // 스크롤이 계속되면 취소하도록
  scrollTimer = setTimeout(() => {
    if (slideWrapper.scrollLeft < (slideWidth + spaceBtwSlides) * (n_slidesCloned - 1 / 2)) {
      forward();
    }
    if (slideWrapper.scrollLeft > (slideWidth + spaceBtwSlides) * ((n_slides - 1 + n_slidesCloned) + 1/2)) {
      rewind();
    }
  }, 100);
  // 여기까지 수정

  updateNavdot();
});
```

먼저 코드가 즉시 스크롤되지 않도록 setTimeout()와 clearTimeout()를 사용합니다. 그리고 카루셀을 앞뒤로 스크롤하여 클론된 첫 번째 슬라이드의 약 절반 정도가 나타날 때 rewind()가 실행되고, 클론된 마지막 슬라이드의 약 절반 정도가 나타날 때 forward()가 실행됩니다.

<div class="content-ad"></div>

이유는 무엇인지 몰라도, scrollLeft 값이 슬라이드 수와 간격을 기반으로 한 이론적 값과 일치하지 않을 수 있으므로 임계값으로 절반을 사용합니다. 예를 들어, slideWidth가 312이고 spaceBtwSlides가 20인 경우, carousel이 왼쪽에 하나의 슬라이드를 숨길 때 scrollLeft 값이 331.5가 될 수 있습니다. 스크롤이 예기치 않게 발생하는 것을 방지하기 위해 슬라이드 너비의 절반으로 버퍼를 만듭니다.

임계값으로 _반_을 사용하는 또 다른 이유가 있습니다. 사용자가 스크롤을 멈추어도 CSS Scroll Snap은 슬라이드가 중앙 위치를 차지할 때까지 캐러셀이 계속 스크롤되도록 합니다. 그 시점에서 위의 스크롤 이벤트 핸들러가 setTimeout() 안의 코드를 실행합니다. 이 CSS Scroll Snap 동작을 활성화하려면 다음 요소의 절반 이상이 드러나면 충분합니다.

이렇게 함으로써 rewind()와 forward() 함수가 실행되어 사용자가 이를 인식하지 못한 채 복제된 슬라이드를 원본 슬라이드로 교체하여 무한 스크롤링의 환상을 만들 수 있습니다.

### 4.3 초기 슬라이드

<div class="content-ad"></div>

마침내 페이지로드 시 일반적으로 마지막으로 복제된 슬라이드가 아닌 첫 번째 슬라이드를 표시합니다:

```js
// 초기화
goto(0); // 추가됨
markNavdot(0);
slideWrapper.classList.add('smooth-scroll');
```

goto(0)는 활성 부드러운 스크롤링에 .smooth-scroll 클래스를 추가하기 전에 실행되어야 합니다. 그렇지 않으면 사용자가 캐러셀이 마지막으로 복제된 슬라이드에서 첫 번째 슬라이드로 스크롤되는 것을 볼 수 있습니다.

### 4.4 왜 점프 링크 내비게이션 점을 사용하지 않을까요?

<div class="content-ad"></div>

위의 섹션 1.5에서 내비게이션 점을 'a' 요소로 표시하는 것은 "무한 스크롤 캐로셀을 만들기에 편리하지 않다"고 썼어요. 이겉에서 설명했듯이, 우리는 무한 스크롤을 시뮬레이션하기 위해 첫 번째와 마지막 슬라이드를 중복해야 하기 때문이죠. 그럼 각 슬라이드의 id 속성 값도 중복됨으로써 점프 링크가 실패할 수 있어요.

어쩌다가 해결책이 있을지도 몰라요. 만약에 아이디어가 있으면 이 글에 댓글을 남겨주세요.

이제 캐로셀은 무한 스크롤이 되어있어요. 마지막 난관은 캐로셀을 자동 재생시키는 것입니다.

## 5. 자동 재생을 위한 JS

<div class="content-ad"></div>

### 5.1 다음 슬라이드로 이동하기

우선, 다음 슬라이드로 캐러셀을 스크롤하기 위해 실행할 함수가 필요합니다:

```js
function next() {
  goto(index_slideCurrent() + 1);
}
```

그런 다음, 이 함수를 반복적으로 실행하여 자동 재생 기능을 구현할 수 있습니다.

<div class="content-ad"></div>

### 5.2 자동 재생 시작하기

이를 위해 setInterval()을 사용하여 캐러셀을 자동으로 재생하는 함수를 만듭니다:

```js
const pause = 2500;
let itv;
function play() {
  // 사용자가 움직임을 줄이기를 선호하는 경우 빨리 리턴합니다.
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    return;
  }
  clearInterval(itv);
  slideWrapper.setAttribute("aria-live", "off");
  itv = setInterval(next, pause);
}
```

pause 변수는 간격을 설정합니다: 이 예시에서는 캐러셀이 자동으로 다음 슬라이드로 변경되는 간격이 2.5초입니다.

<div class="content-ad"></div>

그럼 "play()" 함수를 정의합니다. 이 함수는 캐러셀을 자동으로 재생시킵니다. 먼저 사용자가 애니메이션을 비활성화했는지 확인합니다. 애니메이션이 비활성화된 경우에는 자동 재생이 시작되지 않도록 이 함수가 반환됩니다.

그런 다음, aria-live 속성을 off로 설정하여 스크린 리더가 슬라이드 변경할 때마다 캐러셀 콘텐츠를 읽어 주지 않도록합니다(1.2절 참조).

마지막으로, 2.5초마다 위에서 정의한 "next()" 함수를 실행합니다(5.1절 참조). 이 setInterval() 함수는 itv 변수에 저장되어 있어서 play() 함수가 실행될 때마다 먼저 clearInterval()로 초기화됩니다. 그렇지 않으면 여러 개의 setInterval() 함수가 동시에 실행되어 슬라이드가 2.5초보다 자주 변경될 수 있습니다.

일반적으로 2.5초의 간격은 너무 짧을 수 있습니다. 이 짧은 간격은 캐러셀의 전체 목적이 짧은 시간 동안 여러 예제의 존재를 강조하여 각 예제의 세부 정보를 중요하지 않게 만드는 경우에 의미가 있습니다.

<div class="content-ad"></div>

일반적으로는 각 슬라이드에 대한 내용을 이해할 수 있도록 긴 간격이 바람직합니다. Friedman (2022)는 5~7초를 권장합니다.

### 5.3 자동 재생 중지

사용자가 캐로셀과 상호 작용할 때 자동 재생을 중지하는 것이 매우 중요합니다:

따라서 먼저 자동 재생을 중지하는 헬퍼 함수를 정의합니다:

<div class="content-ad"></div>

```js
function stop() {
  clearInterval(itv);
  slideWrapper.setAttribute("aria-live", "polite");
}
```

자동 재생을 구현하는 setInterval() 함수를 지웁니다. 그런 다음, 화면 낭독기가 내비게이션 도트를 누를 때 내용 변경 사항을 알릴 수 있도록 aria-live를 공손하게 설정합니다 (위의 1.2 섹션 참조).

### 5.4 Intersection Observer

캐러셀이 사용자 뷰포트에 완전히 표시될 때, 첫 번째 자동 재생이 시작됩니다. 사용자에게 표시되지 않을 때 캐러셀을 자동으로 재생하는 것은 의미가 없습니다.


<div class="content-ad"></div>

동일한 이유로, 카루셀의 일부가 뷰포트 바깥으로 벗어나면 자동 재생이 멈춰야 합니다.

이 기능을 구현하기 위해 강력한 Intersection Observer를 사용합니다:

```js
const observer = new IntersectionObserver(callback, {threshold: 0.99});
function callback(entries, observer) {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      play();
    } else {
      stop();
    }
  })
}

observer.observe(carouselContainer);
```

저는 0.99의 threshold를 사용합니다. threshold가 1인 경우, Edge는 Intersection Observer를 구현하지 못한다고 Maria(2019)가 보고하였습니다. threshold가 0.999인 경우, MacOS Safari는 페이지 로드 시 전체 카루셀이 표시될 때 자동 재생이 시작되지 않습니다. 왜냐하면 뷰포트 내에서 보이는 요소의 비율을 나타내는 intersectionRatio 속성을 약간 0.9988713264465332와 같은 값으로 계산하기 때문입니다. 실제로 Almand(2019)는 "숫자는 0.99와 1 사이에 위치할 것"이라고 보고하고 있습니다. 따라서 안전하게 threshold를 0.99로 설정하는 것이 좋습니다.

<div class="content-ad"></div>

또 하나 기억해야 할 중요한 점은 observer.observe()가 스크립트에서 실행될 때 콜백 함수가 실행된다는 것입니다 (snewcomer 2018). 이것은 콜백 함수가 단순히 play()일 경우에는 페이지가 로드될 때 자동 재생이 시작되어 Intersection Observer를 사용하는 전체 목적을 무효화할 수 있다는 의미입니다. entry.isIntersecting으로 캐러셀이 뷰포트 내에 있는지 확인해야 합니다.

### 5.5 마우스 사용자를 위한 자동 재생 전환

다음으로, 마우스 사용자가 캐러셀 위에 커서를 가져다 놓을 때 자동 재생을 비활성화하려고 합니다. 슬라이드 내의 버튼을 클릭하려고 하거나 네비게이션 점 중 하나를 클릭하려고 할 때 캐러셀이 움직이면 아무도 행복해하지 않을 것입니다.

이를 위해 포인터진입(pointerenter) 및 포인터나감(pointerleave) 이벤트 리스너를 사용하고 이를 캐러셀 컨테이너에 첨부합니다:

<div class="content-ad"></div>

```js
carouselContainer.addEventListener("pointerenter", () => stop());
carouselContainer.addEventListener("pointerleave", () => play());
```

### 5.6 키보드 사용자를 위한 자동 재생 전환

그 다음, 키보드 사용자는 내비게이션 도트와 같은 내부의 대화형 요소에 초점을 맞출 때 Tab 키를 누르면 캐러셀이 움직이지 않도록 하고 싶어합니다.

이를 위해 focus와 blur 이벤트 리스너를 사용합니다:

<div class="content-ad"></div>

```js
carouselContainer.addEventListener("focus", () => stop(), true);
carouselContainer.addEventListener("blur", () => {
  if (carouselContainer.matches(":hover")) return;
  play();
}, true);
```

true 옵션은 캐러셀 컨테이너 내부의 모든 요소에서 발생한 포커스/포커스 해제 이벤트를 잡기 위해 필요합니다.

포커스 해제 이벤트가 발생할 때, 위 코드는 먼저 커서가 캐러셀 내부에 있는지 확인합니다. 만약 그렇다면 자동 재생을 다시 시작하지 않습니다. 이는 손목 부상으로 인해 마우스 사용을 최소화하는 사용자들을 위해 버튼과 상호 작용하기 위해 Tab 키를 사용하는 마우스 사용자들을 위한 것입니다.

.matches(":hover")의 사용은 커서가 요소 내에 있는지 확인하는 똑똑한 기술로, Jacob (2019)에 의해 제안되었습니다.

<div class="content-ad"></div>

### 5.7 터치 디바이스 사용자를 위한 자동 재생 전환

스마트폰 및 태블릿 사용자는 캐러셀을 스와이프하여 새로운 슬라이드를 보는 후 자동 재생을 활성화하는 것이 귀찮을 수 있습니다.

제 의견으로는, 이 경우 최상의 사용자 경험(UX)은 다음과 같이 달성될 것입니다:

```js
carouselContainer.addEventListener("touchstart", () => stop());
```

<div class="content-ad"></div>

터치 이벤트(touchstart)는 사용자가 캐러셀을 터치할 때마다 발생하며, 이 경우 자동 재생이 중지됩니다. 그러나 pointerleave 및 blur 이벤트와 달리 touchend 이벤트가 발생할 때 자동 재생을 다시 시작하지 않습니다.

자동 재생을 위한 전체 JS 스크립트는 다음과 같습니다:

```js
// Autoplay
function next() {
  goto(index_slideCurrent() + 1);
}
const pause = 2500;
let itv;
function play() {
  // 사용자가 움직임을 줄이기를 선호하는 경우 일찍 반환합니다.
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    return;
  }
  clearInterval(itv);
  slideWrapper.setAttribute("aria-live", "off");
  itv = setInterval(next, pause);
}
function stop() {
  clearInterval(itv);
  slideWrapper.setAttribute("aria-live", "polite");
}
// 캐러셀이 완전히 표시될 때 자동 재생 시작
const observer = new IntersectionObserver(callback, {threshold: 0.99});
observer.observe(carouselContainer);
function callback(entries, observer) {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      play();
    } else {
      stop();
    }
  })
}
// 마우스 사용자를 위해
carouselContainer.addEventListener("pointerenter", () => stop());
carouselContainer.addEventListener("pointerleave", () => play());
// 키보드 사용자를 위해
carouselContainer.addEventListener("focus", () => stop(), true);
carouselContainer.addEventListener("blur", () => {
  if (carouselContainer.matches(":hover")) return;
  play();
}, true);
// 터치 디바이스 사용자를 위해
carouselContainer.addEventListener("touchstart", () => stop());
```

### 5.8 창 크기 조절에 대한 자동 재생 전환하기

<div class="content-ad"></div>

하나 더 고려할 사항이 있습니다. 데스크탑 사용자가 브라우저 창 크기를 조절할 때 자동 재생을 중지하고 싶습니다. 이렇게 하면 이전 창 크기를 기준으로 캐러셀이 슬라이딩하는 현상을 방지할 수 있습니다.

따라서 우리는 resize 이벤트 리스너(위의 섹션 3.7에서 정의됨)를 다음과 같이 수정합니다:

```js
let resizeTimer; // 수정
window.addEventListener('resize', () => {
  slideWidth = slides[0].offsetWidth;
  spaceBtwSlides = Number(window.getComputedStyle(slideWrapper).getPropertyValue('grid-column-gap').slice(0, -2)); // 끝에 px 제거

  // 여기부터 수정됨
  if(resizeTimer) clearTimeout(resizeTimer);
  stop();
  resizeTimer = setTimeout(()=>{
    play();
  }, 400);
  // 여기까지 수정됨
});
```

위에서 설명한 스크롤 이벤트 리스너와 유사하게, setTimeout() 및 clearTimeout()를 사용하여 사용자가 창 크기를 조절하는 것을 멈출 때까지 play()를 구현하는 것을 기다립니다.

<div class="content-ad"></div>

그거다! 이제 접근성이 좋고 스와이프 및 무한 스크롤과 자동 재생이 가능한 캐러셀이 완성되었어요!

## 마지막으로

아마도 여러분이 직접 구축해야 할 캐러셀은 위에서 만든 것과 정확히 일치하지는 않을 겁니다. 그럼에도 불구하고, 이 글이 여러분에게 직접 캐러셀을 만들 수 있는 문을 열어 줄 것을 바라며 많은 인용된 아티클들 아래 목록을 함께 제공합니다!

## 참고문헌

<div class="content-ad"></div>

Almand, Travis (2019) “Intersection Observer가 관찰하는 방법에 대한 설명”, CSS-Tricks, 2019년 9월 24일.

Buljan, Roko C. (2022) “무한 자바스크립트 캐러셀”, Stack Overflow, 2022년 7월 15일.

Can I Use (2024) “CSS 속성: scrollbar-width”, Can I Use?, 2024년 7월 21일 접속.

Coyier, Chris (2019) “실제로 간단한 슬라이더”, CodePen, 2019년 5월 7일.

<div class="content-ad"></div>

Deque Systems (날짜 미상) “Carousel (based on a tabpanel)”, Deque University, 날짜 미상.

Friedman, Vitaly (2022) “Usability Guidelines For Better Carousels UX”, Smashing Magazine, 2022년 4월 13일.

GrahamTheDev 2020 “They are indeed very similar, there is one key distinction...”, Stack Overflow, 2020년 7월 21일.

Hempenius, Katie (2021) “Best practices for carousels”, web.dev, 2021년 1월 26일.

<div class="content-ad"></div>

| Author            | Title                                                      | Source            | Date           |
|-------------------|------------------------------------------------------------|-------------------|----------------|
| Jacob (2019)      | “In modern browsers you can just do element.matches(`:hover`)" | Stack Overflow  | Aug 12, 2019  |
| John, Theodore (2023) | “Scrollbar Styling in Chrome, Firefox, and Safari — Customizing Browser Elements | Medium | Jul 4, 2023 |
| Мария (2019)      | “Intersection Observer doesn't work in Edge”               | Stack Overflow    | May 29, 2019  |
| MDN contributors (2024a) | “ARIA: group role”                                      | MDN Web Docs      | Apr 17, 2024  |

<div class="content-ad"></div>

MDN 기여자들 (2024b) “WAI-ARIA 기초”, MDN 웹 문서, 최근 업데이트일: 2024년 1월 1일.

Oliver, Jack (2017) “무한 자동재생 캐러셀”, CodePen, 2017년 11월 3일.

Pared, Warren (2021) “무한 CSS 캐러셀 만들기”, Dev Community, 2021년 4월 12일.

PowerMapper (2023) “WAI-ARIA 스크린 리더 호환성”, PowerMapper 스크린 리더 테스트, 2023년 12월 12일.

<div class="content-ad"></div>

Rifki, Nada (2020) “CSS Scroll Snap 사용 방법”, LogRocket, 2020년 3월 9일.

Shadeed, Ahmad (2020) “스타일링을 위한 오래된 버튼 엘리먼트”, ishadeed.com, 2020년 2월 19일.

snewcomer (2018) “이것이 기본 동작입니다. IntersectionObserver의 인스턴스를 인스턴스화할 때 콜백이 호출됩니다...”, Stack Overflow, 2018년 11월 20일.

van der Schee, Joost (2021) “CSS scroll snap을 사용한 캐러셀”, CodePen, 2021년 4월 17일.

<div class="content-ad"></div>

2024년에 W3C 웹 접근성 이니셔티브에 의해 업데이트된 "Carousel (Slide Show or Image Rotator) Pattern" 인 ARIA Authoring Practices Guide에 등재되었습니다.

2024년에 W3C 웹 접근성 이니셔티브에 의해 업데이트된 "Auto-Rotating Image Carousel Example with Buttons for Slide Control" 인 ARIA Authoring Practices Guide에 등재되었습니다.

2024년에 W3C 웹 접근성 이니셔티브에 의해 업데이트된 "Landmark Regions" 인 ARIA Authoring Practices Guide에 등재되었습니다.

2024년에 W3C 웹 접근성 이니셔티브에 의해 업데이트된 "Auto-Rotating Image Carousel with Tabs for Slide Control Example" 인 ARIA Authoring Practices Guide에 등재되었습니다.

<div class="content-ad"></div>

| Author               | Title                                                                                     | Publication          | Date         |
|----------------------|-------------------------------------------------------------------------------------------|----------------------|--------------|
| Webb, Jason (2021)   | “When testing the 'tabbed' pattern with live low-vision, blind, and deafblind users at...” | Dev Community        | Oct 2, 2021  |
| Weckenmann, Sonja    | “A Step-By-Step Guide To Building Accessible Carousels”                                    | Smashing Magazine    | Feb 17, 2023 |
| Werner, Thomas (2019) | “Unfortunately scrollIntoView doesn’t seem to behave consistently across platforms...”        | Dev Community        | Apr 12, 2019 |