---
title: "HTML, CSS로 자동 스크롤되는 캐러셀 애니메이션 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-AnimateanAuto-ScrollingCarouselwithOnlyHTMLandCSS_0.png"
date: 2024-08-26 17:22
ogImage: 
  url: /assets/img/2024-08-26-AnimateanAuto-ScrollingCarouselwithOnlyHTMLandCSS_0.png
tag: Tech
originalTitle: "Animate an Auto-Scrolling Carousel with Only HTML and CSS"
link: "https://medium.com/itnext/animate-an-auto-scrolling-carousel-with-only-html-and-css-8300c8c72841"
isUpdated: true
updatedAt: 1724744008874
---


## 라이브러리 사용 안 함, 자바스크립트 없이 CSS 키프레임 및 스크롤 스냅 모듈만 활용하기

![image](/assets/img/2024-08-26-AnimateanAuto-ScrollingCarouselwithOnlyHTMLandCSS_0.png)

# 소개

강력한 컴퓨터를 사용하는 웹 개발자로서, 우리는 메모리나 안정적인 인터넷 연결과 같은 것이 문제가 되지 않는다는 사실을 종종 고려하지 않습니다. 그러나 우리의 앱이 실행되는 환경에서는 문제가 될 수 있습니다.

<div class="content-ad"></div>

예를 들어, Raspberry Pi에서 실행할 웹 앱을 만드는 경우가 있습니다. 이는 쇼핑몰의 키오스크, 의사 사무실 또는 놀이 공원과 같은 곳에 적합할 수 있습니다.

작년에는 간단한 웹 애플리케이션을 패키징하고 셀룰러 기반 Blues Notecard를 사용하여 Raspberry Pi에 다운로드하는 방법을 보여주기 위해 직무를 맡게 되었습니다.

날씨 앱을 데모로 선택했는데, 사용자에게 도시 목록을 제공하고 해당 도시의 날씨 예보를 가져와 계속해서 회전하는 캐러셀에 표시하는 앱이었습니다.

앱 크기를 작게 유지하여 (셀룰러 데이터를 통해 Raspi로 더 빨리 다운로드할 수 있도록), 순수한 HTML, CSS 및 약간의 바닐라 JavaScript로 만들었습니다.

<div class="content-ad"></div>

각 도시의 일기 예보의 자동 스크롤 기능을 구현하기 위해 일반적으로 캐러셀 라이브러리를 찾아보겠지만, 앱을 가볍게 유지하고 싶어서 몇 가지 연구를 하다가 필요한 기능을 CSS만으로 구현할 수 있다는 것을 발견했어요.

오늘은 CSS 키프레임 애니메이션과 스크롤 스냅 모듈 그리고 몇 가지 잘 정의된 CSS 클래스를 사용하여 CSS만으로 자동 스크롤이 가능한 캐러셀을 만드는 방법을 알려드릴게요.

다음은 최종 자동 스크롤 캐러셀 애니메이션이 어떻게 보이게 될 지에 대한 예시입니다.

# CSS 키프레임과 스크롤 스냅

<div class="content-ad"></div>

자동 스크롤 캐러셀의 구현에 대해 들어가기 전에, 이를 가능하게 하는 CSS 기능에 대해 알아보겠습니다: CSS 키프레임과 CSS 스크롤 스냅 모듈입니다.

## CSS 키프레임

@keyframes 규칙은 CSS 애니메이션 시퀀스의 중간 단계를 제어하여 애니메이션 시퀀스 a길이의 키프레임(또는 웨이포인트)에 대해 스타일을 정의합니다. 이는 CSS 전환보다 애니메이션 시퀀스의 중간 단계를 더 세밀하게 제어할 수 있게 합니다.

@keyframes는 자바스크립트가 필요하지 않는 복잡한 애니메이션을 가능하게 할 뿐만 아니라 간단한 구문과 여러 요소 간의 재사용성을 제공합니다(이의 코드 예제에서 곧 확인하실 수 있습니다).

<div class="content-ad"></div>

CSS 스크롤 스냅

CSS 스크롤 스냅 모듈은 스냅 위치를 정의하여 패닝 및 스크롤 동작을 제어할 수 있는 속성을 제공합니다. 사용자가 스크롤 컨테이너 내에서 넘치는 콘텐츠를 스크롤할 때 콘텐츠가 해당 위치로 스냅될 수 있어 페이징 및 스크롤 위치 지정을 제공합니다.

오늘 캐러셀을 위해 특히 두 가지 속성을 활용할 것입니다.

CSS scroll-snap-type

<div class="content-ad"></div>

각종 다양한 값을 받아들입니다. 하지만 가장 일반적인 값은 다음과 같습니다:

- mandatory - 스크롤 컨테이너는 스크롤한 후 가장 가까운 스냅 지점에 맞추어야 합니다.
- proximity - 스크롤 컨테이너는 스크롤이 스냅 지점 근처에서 자연스럽게 멈출 때에만 해당 지점에 맞추어야 합니다.
- none - 스냅 행동이 적용되지 않습니다.

실제로는, mandatory는 스크롤의 최종 위치에 대해 정확한 제어가 필요한 경우에 사용할 수 있습니다(캐러셀이나 각 항목이 스크롤한 후 완전히 보일 수 있어야 하는 페이지네이션 콘텐츠와 같은 경우). 그리고 proximity는 사용자에게 스크롤 중단 지점에 대해 더 많은 제어권을 주고 부드럽고 덜 충격적으로 느껴지게 할 때 사용할 수 있습니다.

CSS scroll-snap-align

<div class="content-ad"></div>

이 스크롤 스냅 방정식의 다른 부분은 scroll-snap-align인데, CSS 스크롤 스냅 모듈을 사용할 때 상자의 스냅 위치를 지정합니다.

스크롤 스냅 컨테이너의 자식 요소는 scroll-snap-align 속성을 설정하여 스크롤이 멈출 때 뷰포트로 스냅되는 방식을 제어할 수 있습니다. 값으로는 다음이 포함됩니다:

- start - 항목의 시작 가장자리를 스크롤 컨테이너의 시작 가장자리에 맞춥니다.
- end - 항목의 끝 가장자리를 스크롤 컨테이너의 끝 가장자리에 맞춥니다.
- center - 항목을 스크롤 컨테이너 중앙에 정렬합니다.
- none - 스냅 정렬이 발생하지 않습니다.

그 백그라운드 정보를 얻고 코드를 진행해 봅시다.

<div class="content-ad"></div>

# HTML 및 CSS 설정하기

먼저, CSS 자동 스크롤링이 제대로 작동하려면 적절한 HTML을 설정해야 합니다.

1. 캐러셀의 일부인 HTML 요소를 부모 div로 묶기

먼저, 스크롤될 모든 슬라이드를 부모 컨테이너 내에 묶습니다. 여기에 CSS scroll-snap-type 속성이 적용되어 자식 요소가 적용된 scroll-snap-align 속성을 해석할 수 있도록 합니다.

<div class="content-ad"></div>

위의 HTML에서 볼 수 있듯이 weather-forecast-container 클래스는 특정 도시의 모든 날씨 정보를 포함하는 weather-template 클래스를 감싸며, weather-carousel-snapper 클래스를 가진 비어 있는 div를 가지고 있습니다.

<div class="content-ad"></div>

2. 비어 있는 div를 날씨 템플릿 div의 하단에 배치하여 carousel의 snapper 요소로 만듭니다.

이 요소가 weather-template div 내부에 있고 형제 요소가 아닌지 확인하세요. 이곳은 계속해서 비어 있지만 존재해야 합니다.

```js
<div class="weather-carousel-snapper"></div>
```

3. 부모 컨테이너에 기본 스타일을 추가하고 날씨 템플릿과 scroll-snap div를 내부에 배치합니다.

<div class="content-ad"></div>

CSS 스크롤 스냅 모듈이 여기에서 사용됩니다.

styles.css

```js
.weather-forecast-container {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  height: clamp(290px, 88vh, 430px);
  max-width: 780px;
  margin: 8px auto;
  scroll-snap-type: x mandatory;
  scroll-behavior: smooth;
  overflow-x: scroll;
}
```

날씨 예보 컨테이너 클래스 내부에서는 컨테이너의 기본 스타일링을 넘어서서 위치 지정, 높이 및 너비 정의(둘 다 Raspi에 연결된 작은 화면의 너비로 설정)와 함께 여기서 첫 번째 CSS 스크롤 스냅 속성을 추가하려고 합니다.

<div class="content-ad"></div>

- scroll-snap-type: x mandatory; - 이는 가로 축에서 필수적인 스냅을 정의합니다.
- scroll-behavior: smooth; - 캐러셀이 다음 페이지로 스크롤되는 것처럼 보이게하여 기본적으로 거기로 뛰어가는 대신 자연스럽게 이동합니다.
- overflow-x: scroll; - 필요에 따라 오버플로우 콘텐츠가 요소의 패딩 상자 내에서 수평으로 맞출 수 있습니다.

다음은 날씨 정보를 포함하고 빈 weather-carousel-snapper div를 포함하는 weather-template div의 스타일링입니다.

styles.css

```js
.weather-template {
  position: relative;
  flex: 0 0 100%;
  background-size: 780px 100%;
  background-repeat: no-repeat;
  border: 1px solid rgba(0, 0, 0, 0.25);
  border-radius: 4px;
  padding: 0 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}
```

<div class="content-ad"></div>

"weather-template" div의 스타일을 포함했어요. 이는 "weather-carousel-snapper" 요소의 실제 부모이기 때문에, 다만 표준 CSS 스타일링일 뿐이에요.

styles.css

```js
.weather-carousel-snapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  scroll-snap-align: center;
}
``` 

마지막으로 "weather-carousel-snapper" 클래스가 적용된 빈 div에 스크롤 스냅 CSS의 마지막 부분을 추가합니다.

<div class="content-ad"></div>

- scroll-snap-align: center; - 이 코드를 사용하면 각 항목이 컨테이너의 중앙에 정렬됩니다.

4. 예보 슬라이드를 자동으로 스크롤하는 애니메이션 keyframes를 생성하세요.

CSS 스타일링이 완료되고 scroll snap이 활성화된 후, 다양한 도시들이 화면을 통해 서서히 슬라이드하여 다음 도시로 이동하는 것처럼 보이도록 하는 @keyframes 애니메이션을 만들어보세요.

styles.css

<div class="content-ad"></div>

위의 코드에서는 세 가지 별도의 애니메이션이 정의되어 있습니다.

- tonext 및 tostart — 각 도시의 날씨 예보의 가로 위치를 캐로셀의 다른 단계에서 관리하며, 오른쪽으로 슬라이딩 효과 또는 매우 왼쪽으로 이동한 후 시작 위치로 재설정합니다(애니메이션을 다시 첫 번째 도시로 끊임없이 반복되게 만듭니다).
- snap— scroll-snap-align 속성을 제어하여 애니메이션 타임라인의 특정 지점에서 스냅 동작을 조정합니다. 이는 캐로셀에서 다음 도시의 날씨 예보를 가운데로 정렬시키는 동적인 스냅 동작을 만듭니다.

이 세 가지 키프레임을 결합하면 데모 비디오에서처럼 캐로셀이 한 예보에서 다음 예보로 부드럽게 슬라이드되는 멋진 애니메이션을 얻을 수 있습니다.

<div class="content-ad"></div>

5. 날씨 예보 컨테이너(parent div) 내부의 적절한 HTML 요소에 keyframe 애니메이션을 적용해 봅시다.

이제 앞에서 정의한 @keyframes 애니메이션을 적용할 차례입니다.

styles.css

```js
@media (hover: hover) {
  .weather-carousel-snapper {
    animation-name: tonext, snap;
    animation-timing-function: ease;
    animation-duration: 4s;
    animation-iteration-count: infinite;
  }

.weather-template:last-child .weather-carousel-snapper {
    animation-name: tostart, snap;
  }
}
```

<div class="content-ad"></div>

- @media (hover: hover) - 대부분의 스마트폰과 태블릿과 같이 호버링이 불가능한 기기에서는 캐러셀 애니메이션 및 스크롤 스냅이 적용되지 않도록 하기 위해, 나는 이러한 애니메이션을 호버 미디어 쿼리 내에 감쌌어요.

.weather-carousel-snapper 클래스는 동시에 animation-name: tonext, snap;으로 실행되는 tonext와 snap 애니메이션을 갖고 있으며, 이 애니메이션은 4초 동안 서서히 반복됩니다.

.weather-template 요소의 마지막 자식을 대상으로 하는 규칙은 weather-carousel-snapper 클래스를 가진 요소에 다른 애니메이션 조합 (tostart와 snap)을 적용하여, 마지막 도시의 날씨 예보가 화면에 표시된 후 캐러셀을 초기 위치로 재설정하여 첫 번째 도시로 다시 이동하도록합니다.

그리고 여기서 CSS만 사용하여 자동 스크롤링 캐러셀을 만들었습니다. 

<div class="content-ad"></div>

이 비디오에서 끝난 제품을 한번 더 시연합니다.

# 결론

특정 상황에서 웹 애플리케이션이 가능한 한 작아야 하는 경우가 매우 중요합니다. 예를 들어, 해당 앱이 저전력이거나 성능이 낮은 시스템(예: Raspberry Pi)에서 실행될 수 있는 경우입니다.

저는 셀룰러로 Raspi에 다운로드될 작은 데모 앱을 개발해야 했을 때 가능한 한 zip 파일 크기를 작게 유지하기 위해 순수한 HTML, CSS 및 약간의 바닐라 JS를 활용하여 React 또는 Svelte와 같은 JS 프레임워크 없이 구현하기로 결정했습니다.

<div class="content-ad"></div>

날씨 앱을 개발하고 싶어서, 목록에 있는 도시들의 예보를 표시하고 목록을 자동으로 회전시킨 후 처음으로 돌아가 다시 시작하는 기능을 추가하고 싶어했습니다. 처음에는 이를 구현하기 위해 캐러셀 라이브러리가 필요할 것이라고 생각했지만, CSS 스크롤 스냅과 키프레임 애니메이션만 있으면 JavaScript가 필요하지 않고도 똑같이 구현할 수 있다는 사실을 깨닫게 되었습니다. 정말 좋은 소식이죠!

몇 주 안에 다시 확인해주세요 — 저는 JavaScript, React, IoT 또는 웹 개발과 관련된 다른 주제에 대해 더 많이 다룰 예정입니다.

제가 쓰는 글을 결코 놓치지 않으려면, 제 뉴스레터 구독을 신청해주세요: https://paigeniedringhaus.substack.com

읽어 주셔서 감사합니다. 미래에 캐러셀이 필요하다면 (자동 회전 여부와 상관없이) CSS를 사용해 직접 구현해보기를 권장합니다 — 그렇게 할 때 필요한 코드가 얼마나 적은지 놀라실 거라고 생각합니다. 즐기세요!

<div class="content-ad"></div>

# 참고 문헌 및 추가 자료

- CSS-Tricks, CSS-Only Carousel article
- MDN 문서, CSS @keyframes
- MDN 문서, CSS scroll snap module
- MDN 문서, CSS scroll-snap-type
- MDN 문서, CSS scroll-snap-align
- GitHub repo, Cellular-Connected Electronic Kiosk

원문은 https://www.paigeniedringhaus.com에서 원래 게시되었습니다.