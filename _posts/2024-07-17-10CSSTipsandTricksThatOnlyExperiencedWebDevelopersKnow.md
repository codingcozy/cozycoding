---
title: "경험 많은 웹 개발자만 아는 10가지 CSS 팁과 트릭"
description: ""
coverImage: "/assets/img/2024-07-17-10CSSTipsandTricksThatOnlyExperiencedWebDevelopersKnow_0.png"
date: 2024-07-17 23:29
ogImage: 
  url: /assets/img/2024-07-17-10CSSTipsandTricksThatOnlyExperiencedWebDevelopersKnow_0.png
tag: Tech
originalTitle: "10 CSS Tips and Tricks That Only Experienced Web Developers Know"
link: "https://medium.com/gitconnected/10-css-tips-and-tricks-that-only-experienced-web-developers-know-d18da43d2632"
---


웹 개발자들은 시멘틱 HTML 문서에 동적이고 창의적이며 사용자 친화적인 스타일을 추가하기 위해 CSS 스타일링 언어를 사용합니다. CSS를 처음 사용하는 웹 개발자들은 기본 CSS 선택자 구문, 속성, at-룰, 가상 클래스/요소를 연습하여 스타일링을 배우기 시작합니다. 이러한 CSS 기능을 알고 있다면 접근성이 좋고 아름다운 사용자 친화적이며 현대적인 웹 사이트를 구축하는 데 충분하지만, CSS의 전체 잠재력을 이해하기 위해 더 많은 것을 배워야 합니다.

웹 개발자들은 웹 사이트 프런트엔드 디자인을 위해 CSS를 널리 사용하며 CSS를 실험함으로써 생산성을 높이고 CSS 기술을 향상시키는 데 도움이 되는 새로운 기능, 해결책 및 기술을 발견하기도 합니다. 이러한 팁과 트릭을 통해 누구나 JavaScript, SVG 및 HTML 캔버스 기반 구현을 작성하지 않고도 디자인 문제에 대한 빠르고 매력적인 CSS 솔루션을 생산적으로 구현할 수 있습니다. 예를 들어, 이제 웹 개발자들은 단 한 줄의 JavaScript 코드를 작성하지 않고도 빠르고 가벼운 카운트다운 타이머를 생성할 수 있습니다.

이 기사에서는 JavaScript, SVG, HTML 캔버스를 사용하지 않고도 현대적인 웹 인터페이스를 효율적으로 구축하기 위해 CSS를 최대한 활용할 수 있는 CSS의 10가지 팁과 트릭을 설명하겠습니다. 이러한 기술을 연습하여 CSS 마스터리를 확장하세요!

<div class="content-ad"></div>

# 1. 요소를 가운데 정렬하는 가장 쉬운 방법

UI 요소를 올바르게 배치하는 것은 고품질 인터페이스를 설계하기 위한 필수 요건입니다. 대부분의 시나리오에서, 웹 개발자는 자식 요소를 수직 및 수평으로 가운데 정렬해야 합니다. 다양한 레이아웃 시스템과 위치 지정 기능으로 인해 CSS는 어떠한 요소도 중앙 정렬하는 단일 속성을 제공할 수 없었습니다. 반응형 디자인의 인기가 높아지기 전에, 웹 개발자들은 HTML 요소를 가운데 정렬하기 위해 음수 마진 트릭을 사용했으나, 지금은 다양한 현대적이고 예전 CSS 속성을 사용하여 요소를 중앙에 배치하는 다양한 방법을 찾을 수 있습니다. 그러나 CSS에서 요소를 중앙에 배치하는 가장 쉬운 방법은 무엇일까요?

현대적인 CSS 그리드 기능은 place-items 단축 속성을 사용하여 그리드 블록을 중앙에 배치하는 것을 지원합니다. 따라서 다음과 같이 자식 요소를 가운데 정렬할 수 있습니다:

```js
<div>
  <button>가운데 정렬된 버튼</button>
</div>

<style>
  div {
    height: 100vh;
    display: grid;
    place-items: center;
  }
</style>
```

<div class="content-ad"></div>


![image](/assets/img/2024-07-17-10CSSTipsandTricksThatOnlyExperiencedWebDevelopersKnow_1.png)

## 2. CSS Border로 삼각형 만들기

내장된 SVG 구현은 일반적으로 어떠한 고급 사용자 정의 모양도 생성할 수 있는 완전한 기능을 제공합니다. 따라서 표준 HTML 구현은 표준 DOM 요소로 임의의 모양을 생성할 수 있는 내장 태그를 제공하지 않습니다. 그러나 우리는 표준 CSS 속성을 사용하여 div 요소를 다양한 기하학적 모양으로 변환할 수 있습니다. 예를 들어, border-radius를 사용하여 원을 만드는 등의 작업이 가능합니다.

인기 있는 브라우저에서 기본 div 요소의 테두리 렌더링 동작은 동일하며, 이를 통해 우리는 다음과 같이 삼각형을 만들 수 있습니다:


<div class="content-ad"></div>

```js
<div></div>

<style>
  div {
    border-right: 20px solid transparent;
    border-bottom: 20px solid darkcyan;
    border-left: 20px solid transparent;
  }
</style>
```

![Image](/assets/img/2024-07-17-10CSSTipsandTricksThatOnlyExperiencedWebDevelopersKnow_2.png)

다양한 삼각형을 만들 수 있습니다. 다른 테두리 측면에 대해 시각적인 색상을 사용하고 테두리 두께를 변경함으로써 가능합니다.

# 3. 순수 CSS 카운트다운

<div class="content-ad"></div>

과거 웹 개발자들은 웹 페이지에서 심플한 카운트다운 타이머를 만들기 위해 JavaScript를 주로 사용했습니다. CSS 애니메이션 기능을 활용하면 JavaScript 코드를 작성하지 않고도 DOM 요소 시각적으로 동적으로 업데이트할 수 있었습니다. 이제 여러 개의 핵심 프레임을 정의하여 표준 CSS 애니메이션 생성 방식을 사용하여 카운트다운을 만들 수 있습니다.

@property at-rule, CSS 변수 및 CSS 카운터를 사용하면 자체적으로 많은 핵심 프레임을 만들지 않고 순수 CSS로 유연하고 사용자 정의 가능한 카운트다운을 작성할 수 있습니다. 다음은 CSS 스니펫을 통해 이를 구현하는 방법입니다:

```js
<div></div>
<style>

  @property --c {
    syntax: "<integer>";
    initial-value: 0;
    inherits: true;
  }
  
  @keyframes timer {
    from { --c: 10 }
    to   { --c: 0 }
  }
  
  :root { animation: timer 5s linear }

  div::after {
    counter-reset: c var(--c);
    content: counter(c, decimal-leading-zero);
  }
</style>
```

위 CSS 스니펫은 @property at-rule, 변수 및 카운터를 사용하여 CSS 애니메이션 기능을 활용한 타이머를 생성합니다. counter-reset 속성을 사용하여 각 애니메이션 프레임마다 동적 값이 렌더링되며, 많은 애니메이션 키프레임 정의가 필요하지 않습니다.

<div class="content-ad"></div>

아래는 마크다운 형식에 맞게 테이블 태그를 변경했습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*LYIFRU6u-zDGfuoELVqYOA.gif)

calc() 및 mod() CSS 수학 함수를 사용하여 전역 타이머 변수의 종료 값 조정으로 분, 초 및 밀리초를 표시하는 카운트 다운 타이머를 만들 수 있습니다.

아래 이야기에서 CSS 함수에 대해 더 알아보세요:

# 4. JavaScript를 사용하지 않고 사용자 지정 네이티브 폼 컨트롤 만들기

<div class="content-ad"></div>

웹 브라우저는 DOM 요소를 브라우저 뷰포트 영역 내에서 시각적 UI 요소로 렌더링합니다. 그러나 HTML 폼 요소는 일반적으로 표준 DOM 뷰포트를 벗어나 렌더링되는 네이티브 UI 요소를 열기 때문에, 네이티브 폼 요소의 팝업 요소를 사용자 정의하는 크로스 브라우저 솔루션이 없습니다. 예를 들어, HTML select 요소의 드롭다운 팝업을 사용자 정의할 수 없지만, 초기 폼 컨트롤 뷰가 DOM 내에서 렌더링되므로 모든 브라우저에서 select 상자의 초기 뷰를 사용자 정의할 수 있습니다.

```js
<div>
  <select>
    <option>React</option>
    <option>Angular</option>
    <option>Svelte</option>
    <option>Vue</option>
    <option>Lit</option>
  </select>
</div>

<style>
  div {
    position: relative;
    min-width: 200px;
  }

  select {
    appearance: none;
    padding: 6px;
    font-size: 14px;
    border-radius: 4px;
    width: 100%;
    border: 2px solid #ccc;
    outline: none;
  }

  select:focus { border: 2px solid #aaa; }

  div::after {
    border-right: 6px solid transparent;
    border-top: 6px solid #333;
    border-left: 6px solid transparent;
    position: absolute;
    top: 42%;
    right: 12px;
    content: "";
    pointer-events: none;
  }
</style>
```

위 CSS 정의는 HTML select 요소의 초기 뷰의 브라우저별 모양을 사용자 정의하기 위해 사용자 정의 아이콘과 테두리를 추가합니다. 아래 미리보기에서 확인할 수 있습니다:

![image](https://miro.medium.com/v2/resize:fit:1400/1*p6TbumLjLTvzvjb-KdoNAQ.gif)

<div class="content-ad"></div>

같은 기술을 사용하여 기본 스타일을 무시하고 네이티브 폼 컨트롤을 맞춤화하고 JavaScript로 요소를 추가하지 않고 다른 원천 기증 스타일시트의 기본 사항을 바꿀 수 있습니다.

# 5. 그라데이션으로 화려한 박스 모서리 만들기

유명한 'border-radius' CSS 속성은 직사각형 DOM 요소에 둥근 모서리나 타원형 모서리를 생성하는 데 도움이 됩니다. 'clip-path' 속성을 사용하여 멋진 모서리를 만들 수도 있습니다. 그라데이션 채우기를 사용하여 창의적인 모서리를 만들 수 있다는 것을 알고 계셨나요?

요소에 방사형 그라데이션 배경을 채워 완벽하고 날카로운 원을 만들 수 있습니다. 생성된 원을 투명하게 만들고 마스크로 사용하여 모서리로 이동시키면 사진 프레임과 같은 모서리가 생성됩니다:

<div class="content-ad"></div>

```js
<div></div>

<style>
  div {
    width: 200px;
    height: 100px;
    background: darkcyan;
    mask: radial-gradient(14px at 40px 40px, transparent 98%, black) -40px -40px;
  }
</style>
```

![Corner Design](/assets/img/2024-07-17-10CSSTipsandTricksThatOnlyExperiencedWebDevelopersKnow_3.png)

이 웹 앱을 사용하면 여러 매개변수를 동적으로 조정하여 이러한 유형의 코너를 생성할 수 있습니다.

# 6. 속성 선택자에서 패턴 매칭 사용하기


<div class="content-ad"></div>

웹 개발자들은 일반적으로 클래스와 식별자를 사용하여 의미론적인 HTML 요소에 스타일을 적용합니다. CSS 표준은 또한 패턴 매칭을 지원하는 속성 선택자를 지원하므로 이를 사용하여 요소에 대한 전역 스타일을 작성할 수 있습니다. 간단한 예로 모든 네이티브 텍스트 입력란에 대한 전역 스타일을 추가하려면 다음과 같은 선택기를 작성할 수 있습니다:

```js
input[type="text"] {
   /* ... */
}
```

속성 선택자는 모든 표준 HTML 속성 및 사용자 정의 속성, 즉 데이터 속성과 작동합니다. 또한 속성 선택자 구문은 패턴 매칭을 지원하므로 이 기술을 사용하여 편리하게 일반적인 HTML 문서의 스타일을 작성할 수 있습니다. HTML 문서를 수정할 수 없는 경우에도 손쉽게 일반적인 스타일을 작성하는 축약 선택기를 작성할 수 있습니다(예: 서드파티 HTML 문서 또는 CMS 페이지 템플릿 스타일링):

```js
<div>
  <button class="ok-btn">Ok</button>
  <button class="re-try-btn">Retry</button>
  <button class="cancel-btn">Cancel</button>
</div>

<style>
  button[class$="-btn"] {
    min-width: 6em;
  }
</style>
```

<div class="content-ad"></div>

위의 선택자는 -btn로 끝나는 클래스 이름을 가진 모든 버튼 요소에 스타일을 추가합니다.

다른 예시로, 위키피디아 하이퍼링크에 다른 스타일을 추가하기 위해 속성 패턴 일치를 사용하는 예시가 있습니다:

```js
a[href*=".wikipedia.org"] {
  color: darkcyan;
}
```

# 7. 사용자 정의 목록 스타일 유형 생성

<div class="content-ad"></div>

HTML 표준은 웹 페이지에서 목록을 만들기 위해 `ol` 및 `ul` 시맨틱 태그를 제공합니다. 이러한 태그는 기본 목록 스타일을 제공하지만, list-style-type 속성을 사용하여 해당 기본 목록 스타일을 재정의할 수 있습니다. 예를 들어, 다음 CSS 정의는 로마 숫자를 사용하는 예시입니다:

```js
ol {
  list-style-type: lower-roman;
}
```

@counter-style 규칙을 사용하면 미리 포함된 목록 스타일을 활용하거나 완전히 새로운 스타일을 만들어 list-style-type 속성에 사용할 수 있습니다. 다음 HTML 문서는 이모지를 사용하여 생성된 사용자 정의 목록 스타일을 사용합니다:

```js
<div>
  <ul>
    <li>React</li>
    <li>Angular</li>
    <li>Svelte</li>
    <li>Vue</li>
    <li>Lit</li>
  </ul>
</div>

<style>

  @counter-style emojis {
    system: cyclic;
    symbols: "\1F449\1F3FD";
    suffix: " ";
  }
  
  ul { list-style-type: emojis }

</style>
```

<div class="content-ad"></div>


![image](/assets/img/2024-07-17-10CSSTipsandTricksThatOnlyExperiencedWebDevelopersKnow_4.png)

이 CSS at-rule을 사용하면 기존 목록 카운터를 확장하고 사용자 정의 카운터를 만들 수 있습니다. 예를 들어, 다음 스타일 정의는 각 순서 목록 항목 앞에 하이픈(-)이 있는 숫자 주위에 대괄호를 렌더링합니다:

```js
  @counter-style custom-roman {
    system: extends lower-roman;
    prefix: "[";
    suffix: "] - ";
  }
  
  ol { list-style-type: custom-roman }
```

# 8. 히어로 섹션을 위한 예술적인 배경 만들기


<div class="content-ad"></div>

현대 웹사이트 디자인에는 웹사이트를 소개하고 여러 주요 작업 버튼을 배치하는 큰 소개 섹션인 히어로 섹션이 포함되어 있습니다. 웹 개발자들은 일반적으로 이러한 히어로 섹션을 창의적인 배경 이미지로 만듭니다. 이제 내장된 CSS 필터와 블렌드 모드 덕분에 히어로 섹션을 위한 창의적인 배경 이미지를 만들기 위해 원본 소스 이미지를 편집할 필요가 없어졌습니다.

다음과 같이 ::after 가상 요소를 사용하여 배경 이미지에 다양한 필터를 적용하여 히어로 섹션을 만들 수 있습니다:

```js
<div>
  <h1>히어로 섹션</h1>
</div>

<style>
  div {
    height: calc(100vh - 16px);
    position: relative;
    display: grid;
    place-items: center;
    color: white;
    overflow: auto;
  }

  div::after {
    content: "";
    position: absolute;
    inset: 0;
    background-image: url(https://raw.githubusercontent.com/codezri/static-media/main/unsplash-img1.jpg);
    background-size: cover;
    background-position: center;
    z-index: -1;
    filter: blur(4px) brightness(0.65) sepia(90%);
  }
</style>
```

<img src="/assets/img/2024-07-17-10CSSTipsandTricksThatOnlyExperiencedWebDevelopersKnow_5.png" />

<div class="content-ad"></div>

한편, mix-blend-mode 속성은 다음 예시에서 보듯이 배경 이미지에 대한 색상 혼합 효과를 추가하여 창의적인 히어로 섹션을 만들 수 있도록 돕습니다:

```js
  div {
    background-image: url(https://raw.githubusercontent.com/codezri/static-media/main/unsplash-img1.jpg);
    background-size: cover;
    background-position: center;
    isolation: isolate;
    height: calc(100vh - 16px);
    position: relative;
    display: grid;
    place-items: center;
    color: white;
  }

  div::after {
    content: "";
    position: absolute;
    background: #594100;
    inset: 0;
    z-index: -1;
    mix-blend-mode: multiply;
  }
```

<img src="/assets/img/2024-07-17-10CSSTipsandTricksThatOnlyExperiencedWebDevelopersKnow_6.png" />

또한 backdrop-filter CSS 속성을 사용하여 예술적인 히어로 섹션을 만들 수도 있지만, 아직 모든 인기있는 표준 브라우저에서 구현되지 않았습니다.

<div class="content-ad"></div>

# 9. CSS로 사용자 친화적이고 매력적인 스크롤 동작 추가하기

과거 JQuery 시대에서 .scrollTo() 함수를 사용하여 JavaScript/JQuery로 스무스한 스크롤 애니메이션을 구현했던 것을 기억하시나요? 이제 웹 개발자들은 순수 CSS로 스크롤 동작을 변경하고 스무스 스크롤을 활성화할 수 있습니다!

다음과 같은 CSS 정의를 살펴보세요:

```js
<a href="#target1">[1]</a>
<a href="#target2">[2]</a>

<div id="target1"></div>
<div id="target2"></div>

<style>
  div {
    height: 100vh;
    border-bottom: solid 40px #aaa;
    background-color: #ccc;
  }

  :root { scroll-behavior: smooth }
</style>
```

<div class="content-ad"></div>

이 CSS 정의를 사용하면 브라우저가 부드러운 스크롤을 활성화하며, 아래 미리보기에서 확인할 수 있습니다:

![부드러운 스크롤 데모](https://miro.medium.com/v2/resize:fit:1400/1*b6XLbg50bZDA2-uFkZ2tLg.gif)

CSS 스크롤 스냅 모듈은 스크롤 가능한 컨테이너의 사용성과 접근성을 향상시키기 위한 CSS 속성 세트를 제공합니다. 예를 들어, 다음 코드 스니펫은 기본 섹션에서 자동으로 스크롤을 중지하고 부분적인 섹션에서 중지하는 것을 피합니다:

```js
<section style="background: #ccc">1</section>
<section style="background: #aaa">2</section>
<section style="background: #ccc">3</section>
<section style="background: #aaa">4</section>

<style>
  section {
    height: 100vh;
    scroll-snap-align: start;
    display: grid;
    place-items: center;
    font-size: 7em;
  }
  
  :root {
    background-color: #eee;
    scroll-snap-type: y mandatory;
  }
</style>
```

<div class="content-ad"></div>


![Scroll Snap CSS](https://miro.medium.com/v2/resize:fit:1400/1*lapBTg-n5H5Yv7DL2IsWlg.gif)

CSS 스크롤 스냅 기능은 JavaScript 없이도 모바일 앱에서 스와이프 가능한 인터페이스에 대해 최소한의 순수 CSS 솔루션을 구현하는 데 도움이 됩니다. 공식 MDN 문서에서 스크롤 스냅 모듈에 대해 더 읽어보세요.

# 10. 클래스 없는 CSS 스타일링 기술

모든 웹 개발자가 스타일링을 시작하기 위해 클래스를 사용하는 것에 익숙합니다. 잘 구성된 클래스를 사용하는 것은 깨끗하고 관리 가능한 웹 페이지를 만드는 데 의심의 여지가 없이 좋은 방법입니다. 그러나 클래스를 만들어야 하는 것은 최소한의 웹 페이지를 만들기 위해 필수적이지 않습니다. 클래스 없는 웹 디자인 개념은 우리에게 시맨틱 HTML 태그를 사용해 CSS를 작성하도록 동기를 부여합니다.


<div class="content-ad"></div>


nav {}
ul {}
ul li {}
summary {}


Functional selectors (implemented as pseudo-classes) like :not(), :has(), and :is() help us write clean code by avoiding repetitive segments in selectors in classless stylesheets. For example, the following code snippet adds coloring styles to navigation menu items except the last one using :not():


<nav>
  <ul>
    <li>Home</li>
    <li>Services</li>
    <li>About</li>
    <li>Contact</li>
  </ul>
</nav>

<style>
  nav ul {
    display: flex;
    list-style: none;
    gap: 1em;
    background: #eee;
    padding: 1em;

    :not(li:last-child) {
      color: darkcyan;
    }
  }
</style>


<img src="/assets/img/2024-07-17-10CSSTipsandTricksThatOnlyExperiencedWebDevelopersKnow_7.png" />


<div class="content-ad"></div>

메뉴 항목에 SVG 아이콘을 렌더링하는 사용자 정의 스타일을 추가해야 한다고 가정해보세요. 그런 다음, 다음과 같이 :has() 기능 선택자를 사용하여 해당 메뉴 항목을 선택할 수 있습니다.

```js
li:has(svg) {
  /* ... */
}
```

:has() 가상 클래스는 속성 선택자와 CSS 변수와 결합하여 대화형 테마 변경을 구현할 수 있습니다.

```js
<div>
  <label><input type="radio" value="t1" name="t" checked>테마 1</label>
  <label><input type="radio" value="t2" name="t">테마 2</label>
</div>

<style>
  body { 
    background-color: var(--background-color);
    color: var(--text-color);
    transition: all 0.5s;
  }
  
  :root:has(input[value="t1"]:checked) {
    --background-color: darkcyan;
    --text-color: white;
  } 

  :root:has(input[value="t2"]:checked) {
    --background-color: skyblue;
    --text-color: black;
  }   
</style>
```

<div class="content-ad"></div>


![Table](https://miro.medium.com/v2/resize:fit:1400/1*yjwtJMBnLvyKV7ZYe-TnYA.gif)

:is() 함수 선택자는 쉼표로 구분된 긴 선택자들의 대체 약어 선택자를 작성하는 데 도움이 됩니다:

```js
/* 예전 방식 */
section h1, 
section h2, 
section h3, 
section h4, 
section h5, 
section h6 {
  color: darkcyan;
}

/* 최신 방식 */
section :is(h1, h2, h3, h4, h5, h6) {
  color: darkcyan;
}
```

다음 이야기에서는 클래스가 없거나 적은 CSS 프레임워크가 어떻게 미래 지향적인 웹 프론트엔드를 디자인하는 데 도움이 되는지 설명합니다:


<div class="content-ad"></div>

읽어 주셔서 감사합니다!