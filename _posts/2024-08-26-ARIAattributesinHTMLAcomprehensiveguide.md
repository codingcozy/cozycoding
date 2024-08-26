---
title: "HTML에서 ARIA 속성 포괄적 가이드 "
description: ""
coverImage: "/assets/img/2024-08-26-ARIAattributesinHTMLAcomprehensiveguide_0.png"
date: 2024-08-26 19:57
ogImage: 
  url: /assets/img/2024-08-26-ARIAattributesinHTMLAcomprehensiveguide_0.png
tag: Tech
originalTitle: "ARIA attributes in HTML A comprehensive guide "
link: "https://dev.to/disane/aria-attributes-in-html-a-comprehensive-guide-577e"
isUpdated: false
---


![ARIA attributes](/assets/img/2024-08-26-ARIAattributesinHTMLAcomprehensiveguide_0.png)

## ARIA 속성이란 무엇인가요? 🤔

ARIA는 "Accessible Rich Internet Applications"의 약자입니다. 장애를 가진 사람들을 위해 웹 콘텐츠와 애플리케이션을 더 접근성 있게 만들기 위해 개발된 속성들의 모음입니다. 특히, ARIA 속성은 스크린 리더와 같은 보조 기술을 사용하는 사용자들의 접근성을 향상시키는 데 도움이 됩니다.

이러한 속성은 HTML 요소의 의미론적 의미를 확장하고 보조 기술에 의해 해석되는 데 필요한 추가 정보를 제공합니다. 예를 들어, 요소의 역할을 정의하거나 상태와 속성을 지정하거나 요소 간의 관계를 설정하는 데 사용할 수 있습니다.

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

### ARIA 속성은 왜 중요할까요? 🚀

상상해보세요:

ARIA 속성이 없다면, 모달 대화상자, 슬라이더 또는 동적 콘텐츠 영역과 같은 상호작용 컴포넌트는 보이지 않고 이해할 수 없는 상태로 남게 됩니다. 페이지를 사용하는 방법에 대한 혼란스러운 지시 사항만 들을 뿐이거나 아무 정보도 받지 못할 것입니다.

이는 단순히 답답한 것뿐만 아니라 중요한 온라인 제공물에서 거의 제외되는 것이기도 합니다. ARIA 속성은 보조 기술을 의존하는 사용자로서 웹사이트의 전체 기능을 경험할 수 있도록 확실하게 하는데 중요합니다. 개발자들이 복잡한 사용자 인터페이스를 설계할 수 있도록 해서 각자의 능력에 관계없이 모든 사람에게 접근 가능하고 이해하기 쉬운 환경을 제공합니다.

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

ARIA 속성은 특히 보조 기술을 활용하여 웹을 이용하는 사람들을 대상으로 합니다. 이에는 웹 사이트의 정보를 읽어 주는 스크린 리더를 사용하는 사용자와 특수한 입력 장치를 사용하는 운동 장애가 있는 사람들이 포함됩니다.

ARIA 속성을 사용하면 이러한 사용자들이 복잡한 웹 응용 프로그램을 더 잘 이해하고 상호 작용할 수 있습니다. 또한, 개발자와 디자이너들도 ARIA 속성을 사용함으로써 자신들의 응용 프로그램이 보다 많은 사람들에게 접근 가능하다는 것을 확실하게 할 수 있습니다.

## 왜 소프트웨어가 포용적이어야 할까요? 🌍

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

포괄적인 소프트웨어는 윤리적으로 바람직할 뿐만 아니라 상업적 이점도 있습니다. 가능한 한 많은 사람들이 사용할 수 있도록 소프트웨어를 설계함으로써, 광범위한 사용자층을 대상으로 접근할 수 있으며, 많은 국가에서의 법적 요구사항을 준수할 수 있습니다.

또한 포괄적인 소프트웨어는 모든 사용자를 신경 쓰는 회사임을 보여줌으로써 긍정적인 브랜드 이미지를 홍보하는 데 도움이 됩니다. 결과적으로, 이는 사용자들의 브랜드에 대한 신뢰와 충성도를 강화합니다.

<img src="https://res.cloudinary.com/practicaldev/image/fetch/s--fPtDMLBj--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExbGl0ZDhoZmVjYTI0dDVtbm9jd2x0eG15OHNnZDhqM3NsNmh0ZHRwaSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/0ozmBje9bTxfPlKO7m/giphy.gif" />

## 기본 ARIA 역할 및 속성 📜

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

ARIA에는 Roles, Properties 및 States 세 가지 주요 카테고리로 나눌 수 있는 다양한 특성이 제공됩니다.

### ARIA roles 🎭

Roles는 페이지의 요소가 나타내는 것이나 하는 기능을 정의합니다. 가장 흔히 사용되는 역할 중 일부는 다음과 같습니다:

- role="button": 요소에 버튼의 역할을 할당합니다. div 또는 span을 버튼으로 사용하고 싶을 때 유용합니다.
- role="navigation": 페이지의 섹션을 탐색 영역으로 식별합니다.
- role="dialog": 사용자의 주의를 필요로 하는 모달 대화상자를 정의합니다.
- role="alert": 보조 기술에서 즉시 알려지는 경고나 중요 메시지로 요소를 표시합니다.

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

### ARIA 속성들 🏷️

속성들은 요소와 다른 요소들과의 관계에 대한 추가 정보를 제공합니다. 중요한 ARIA 속성들은 다음과 같습니다:

- aria-labelledby: 현재 요소를 라벨링하는 다른 요소를 가리킵니다. 특히 폼 요소에서 유용합니다.
- aria-describedby: aria-labelledby와 유사하지만 요소에 대해 더 구체적인 설명을 제공합니다.
- aria-controls: 현재 요소가 다른 요소와 상호 작용을 제어한다는 것을 나타냅니다. 예를 들어, 메뉴를 여는 스위치와 같은 경우입니다. 예를 들어, 메뉴가 나타나거나 숨겨지는 스위치.

### ARIA 상태들 🔄

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

인터랙티브 요소의 현재 상태를 나타내는 상태(State)는 다음과 같습니다. 예시는 다음과 같습니다:

- aria-expanded: 영역이 확장(true)되었는지 또는 축소(false)되었는지를 나타냅니다.
- aria-checked: 체크 상자나 스위치가 활성화(true), 비활성화(false)되었는지 또는 혼합 상태(mixed)인지를 나타냅니다.
- aria-hidden: 요소가 보이는지(false) 안 보이는지(true)를 점감합니다. 

## ARIA 어트리뷰트 사용의 실제 예시 🛠️

ARIA 어트리뷰트를 사용하는 실제 예시를 살펴보며 더 잘 이해해 봅시다.

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

### 사용자 정의 버튼

개발자들은 종종 div나 span 요소를 사용하여 표준 HTML 버튼과 다른 사용자 정의 버튼을 만듭니다. 이러한 사용자 정의 버튼이 스크린 리더에서도 접근할 수 있도록 하려면 ARIA 속성을 사용해야 합니다:

```js
<div role="button" aria-pressed="false" tabindex="0" onclick="toggleButton(this)">
  Click me
</div>
```

이 예제에서 role="button" 속성은 div 요소에 버튼 역할을 할당합니다. aria-pressed 속성은 버튼이 현재 눌려 있는지 여부를 나타내며, tabindex="0"은 요소를 키보드로 초점을 맞출 수 있도록 합니다.

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

### 아코디언 메뉴

아코디언 메뉴는 여러 개의 축소/확장 가능한 섹션으로 구성됩니다. 화면 리더 사용자가 이해하기 쉽도록 ARIA 속성을 사용할 수 있습니다:

```js
<div id="accordion">
  <h3 id="section1-heading">
    <button aria-expanded="false" aria-controls="section1">
      Section 1
    </button>
  </h3>
  <div id="section1" role="region" aria-labelledby="section1-heading">
    <p>첫 번째 섹션의 내용</p>
  </div>

  <h3 id="section2-heading">
    <button aria-expanded="false" aria-controls="section2">
      Section 2
    </button>
  </h3>
  <div id="section2" role="region" aria-labelledby="section2-heading">
    <p>두 번째 섹션의 내용</p>
  </div>
</div>
```

여기에 각 버튼 요소는 연관된 섹션이 열렸는지 닫혔는지를 나타내기 위해 aria-expanded 속성을 사용합니다. aria-controls 속성은 버튼과 콘텐츠 영역 간의 관계를 나타내며, role="region"은 콘텐츠를 사용자의 관심 영역으로 식별합니다.

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

### 모달 대화상자

모달 대화상자는 자주 사용되는 UI 구성 요소로, ARIA 속성을 사용하여 접근성을 확보할 수 있습니다:

```js
<div role="dialog" aria-labelledby="dialog-title" aria-describedby="dialog-description">
  <h2 id="dialog-title">확인이 필요합니다</h2>
  <p id="dialog-description">이 작업을 진행하시겠습니까?</p>
  <button onclick="closeDialog()">닫기</button>
</div>
```

이 예시에서 role="dialog"는 요소를 모달 대화상자로 식별합니다. aria-labelledby 속성은 대화상자를 제목과 연결하고, aria-describedby는 대화상자 콘텐츠의 설명을 참조합니다.

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

## ARIA 속성을 사용할 때 흔히 범하는 실수 🚨

상당한 혜택에도 불구하고, ARIA 속성을 구현할 때 개발자들이 종종 범하는 일반적인 실수가 있습니다. 이러한 실수를 피해야 접근성을 의도치 않게 저하시키지 않도록 중요합니다.

### ARIA 과도한 사용

ARIA는 필요할 때에만 사용해야 합니다. HTML 요소가 이미 의미론적으로 의미 있는 경우(예: 버튼, 내비게이션, 헤더 등)에는 ARIA를 사용하지 않아야 합니다.

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

### 상태 동기화 부족

요소의 상태(예: aria-expanded, nav, header)가 누락된 경우 ARIA를 사용하지 않아야 합니다. 사용자 입력에 의해 요소의 상태(예: aria-expanded)가 변경된 경우, 해당 ARIA 속성이 적절하게 업데이트되도록 보장해야 합니다.

### 불필요한 역할 할당

모든 요소에는 역할 할당이 필요하지 않습니다. 예를 들어, 버튼 요소에는 추가적인 role="button" 할당이 필요하지 않습니다.

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

## ARIA 사용에 대한 최상의 실천 방안 🌟

ARIA 속성을 최대한 활용하고 접근성을 극대화하기 위해 몇 가지 최상의 실천 방안을 준수해야 합니다:

### 가능한 경우에는 기본 HTML 요소 사용

기본으로 제공되는 접근성 기능을 활용할 수 있는데 ARIA가 제공하지 못할 수도 있습니다.

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

### 정기적으로 접근성 테스트를 수행하세요

스크린 리더 또는 전문 소프트웨어와 같은 도구를 사용하여 ARIA 구현이 제대로 작동하는지 확인하세요.

### ARIA 구현 사례를 문서화하세요

코드에서 ARIA 속성을 사용하는 경우, 해당 속성이 왜 사용되고 어떻게 사용되는지 문서화하여 다른 개발자가 코드를 더 잘 이해할 수 있도록 해주세요.

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

## ARIA를 사용해서는 안 되는 경우 (예시와 함께) 🚫

ARIA 속성은 강력하지만, 이미 내재된 의미와 접근성 속성을 가진 기본 HTML 요소 대신 사용해서는 안 됩니다. 예를 들어, 좋은 이유가 없다면 일반 버튼 요소 대신 role="button" 속성을 가진 div 요소를 사용해서는 안 됩니다.

버튼, 입력란, 레이블 또는 선택란과 같은 기본 HTML 요소는 이미 탁월한 접근성 지원을 제공합니다. ARIA 속성을 과도하게 사용하면 코드가 복잡해지고 유지보수가 어려워지며, 경우에 따라 잘못 구현된 경우 접근성을 더 악화시킬 수도 있습니다.

## ARIA 자가 테스트 방법 🔍

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

ARIA 속성을 테스트하는 것은 웹 애플리케이션이 완전히 접근 가능하다는 것을 보장하는 중요한 단계입니다. ARIA 속성을 테스트하기 위한 다양한 방법이 있습니다.

가장 효과적인 방법 중 하나는 NVDA(NonVisual Desktop Access)나 VoiceOver(맥)와 같은 스크린 리더를 사용하는 것입니다. 이러한 도구는 페이지의 내용을 읽어주고 ARIA 속성이 올바르게 구현되었는지 표시해줍니다. 또한, 개발자는 브라우저의 개발자 도구 내의 접근성 검사자(Accessibility Inspector)나 Axe, WAVE와 같은 전문적인 접근성 도구를 사용하여 잠재적인 문제를 식별하고 해결할 수 있습니다.

이러한 도구들을 사용한 정기적인 테스트는 접근성을 지속적으로 향상시키는 데 도움이 됩니다.

## 접근성을 중요시하는 회사들 🌟

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

일부 회사들은 모든 사람이 이용할 수 있는 진정으로 포괄적인 디지털 경험을 만들기 위해 노력하고 있습니다. 한 가지 뛰어난 예는 Microsoft입니다. 이 회사는 접근성에 강한 헌신을 지니고 있으며 장애를 가진 사람들을 지원하는 도구와 기술을 지속적으로 개발하고 있습니다. "AI for Accessibility" 프로그램과 같은 이니셔티브를 통해 Microsoft는 장애를 가진 사람들의 삶을 개선하기 위한 혁신을 촉진하고 있습니다. Microsoft의 운영 체제 및 오피스 응용 프로그램은 시각, 청각 또는 기동 장애를 가진 사람들이 생산적으로 작업할 수 있도록 포괄적인 접근성 기능을 제공합니다.

![이미지](/assets/img/2024-08-26-ARIAattributesinHTMLAcomprehensiveguide_1.png)

또 다른 예는 Apple입니다. 이 회사는 제품 개발 과정에서 항상 접근성에 주의를 기울이고 있습니다. VoiceOver와 같은 기능은 시각 장애가 있는 사용자들이 기기를 완전히 음성 제어로 작동할 수 있게 하며, 기동 장애가 있는 사용자들을 위한 다양한 사용자 정의 옵션은 산업의 표준을 제시하고 있습니다. Apple은 처음부터 포괄적인 기술을 개발하여 아무도 배제되지 않도록 합니다.

![이미지](/assets/img/2024-08-26-ARIAattributesinHTMLAcomprehensiveguide_2.png)

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

Google은 접근성을 우선시하는 회사 중 하나입니다. Google 어시스턴트와 같이 장애를 가진 사람들을 위해 최적화된 제품을 개발하고 Android 및 Chrome에 접근성 도구를 포함시킴으로써, Google은 포용이 그 제품 철학의 중요한 구성 요소라는 것을 보여줍니다.

![이미지](/assets/img/2024-08-26-ARIAattributesinHTMLAcomprehensiveguide_3.png)

이러한 기업들은 접근성을 회사의 DNA에 통합시킬 수 있는 격려적인 예시입니다. 그들은 모든 사람에게 접근 가능한 디지털 제품과 서비스를 만드는 것이 가능할뿐만 아니라 경제적으로 이익을 가져다 줄 수 있다는 것을 증명합니다.

## 결론 🎯

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

ARIA 속성은 웹 애플리케이션의 접근성을 향상시키는 강력한 도구입니다. 이를 통해 개발자들은 상호작용적이고 동적인 콘텐츠를 보조 기술을 사용하는 사용자를 포함해 모든 사용자에게 접근 가능하게 할 수 있습니다. ARIA 속성을 신중하게 구현함으로써 웹을 더 포용 가능한 공간으로 만들 수 있습니다.

ARIA 속성을 구현하는 데 도움이 필요하거나 궁금한 점이 있다면 아래 댓글 섹션을 자유롭게 사용해주세요. 함께 모든 사용자에게 더 접근 가능한 웹을 만들어봅시다!

제 글이 마음에 드신다면, 더 많은 기술 관련 내용을 보기 위해 제 블로그를 팔로우해주시면 좋을 것 같아요!