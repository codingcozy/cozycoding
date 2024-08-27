---
title: "Salesforce에서 커스텀 CSS를 활용한 반응형 디자인 하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 17:47
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Mastering Responsive Design with Custom CSS in Salesforce"
link: "https://medium.com/@rathoredeep2020/mastering-responsive-design-with-custom-css-in-salesforce-46f71af62cb6"
isUpdated: true
updatedAt: 1724742777411
---


세일스포스란 무엇인가요-

세일스포스 CSS 이해하기 —

세일스포스는 세일스포스 라이트닝 디자인 시스템(SLDS)을 제공합니다. 이는 세일스포스의 디자인 원칙에 따라 제작된 스타일과 구성 요소의 포괄적인 라이브러리입니다. SLDS는 훌륭한 시작점이지만 때로는 특정 디자인 요구 사항을 충족시키기 위해 사용자 정의 CSS가 필요합니다.

세일스포스 개발 시, 세일스포스 라이트닝 디자인 시스템(SLDS)과 사용자 정의 CSS는 각각의 강점을 가지고 있습니다. 사용할지 여부는 여러분의 특정 요구 사항과 프로젝트의 문맥에 따라 다릅니다. 다음은 여러분의 요구 사항에 더 적합한 것을 결정하는 데 도움이 되는 상세한 비교입니다:

<div class="content-ad"></div>

Salesforce Lightning Design System (SLDS) — -

## Custom CSS Advantages: — -

- Flexibility: —
Custom CSS allows you to implement unique and highly specific design requirements that may not be possible with SLDS alone.

- Control: You have complete control over the styles, ensuring that every aspect of your design is exactly as intended.

<div class="content-ad"></div>

혁신: 사용자 정의 CSS를 사용하면 혁신적이고 최첨단 디자인을 만들어 기본 Salesforce UI와 구분할 수 있습니다.

## 단점

# 유지 보수

애플리케이션이 성장하고 발전함에 따라 사용자 정의 CSS는 유지하기 어려워질 수 있습니다. 효과적으로 관리하기 위해서는 철저한 문서화와 좋은 관행이 필요합니다.

<div class="content-ad"></div>

반응성: 완전히 반응형인 사용자 정의 CSS를 보장하려면 다양한 장치 및 화면 크기에서 추가적인 노력과 테스트가 필요할 수 있습니다.
호환성: SLDS 또는 다른 Salesforce 구성 요소와 충돌하지 않도록 사용자 정의 CSS를 철저히 테스트해야 합니다.

어느 쪽이 좋을까요?

SLDS를 사용해야 하는 경우: —

일관성이 필요할 때: Salesforce 외관과 느낌을 일관되게 유지해야 하는 경우 SLDS를 사용하는 것이 좋습니다.
속도 및 효율성: 미리 구축된 구성 요소를 사용하여 신속하고 효율적으로 개발해야 할 때.
유지보수성: 사용자 정의 스타일과 관련된 유지보수 부담을 줄이고 싶을 때.
표준화: 표준화가 중요한 팀이나 다중 프로젝트에 작업할 때.

<div class="content-ad"></div>

Custom CSS를 사용해야 할 때:

고유한 디자인 요구 사항: SLDS(Salesforce Lightning Design System)로 충족시킬 수 없는 고유한 디자인 요구 사항이 있을 때.
높은 사용자 정의: SLDS로는 불가능한 특정 사용자 정의를 달성해야 할 때.
혁신적 디자인: 매우 혁신적이고 고유한 사용자 인터페이스를 만들고자 할 때.

Salesforce Lightning 컴포넌트에 스타일을 추가하는 방법:

Salesforce Lightning 컴포넌트에 스타일을 추가하는 방법은 SLDS(Salesforce Lightning Design System) 및 사용자 정의 CSS를 통해 수행할 수 있습니다. Lightning Web Components(LWC) 또는 Aura Components와 작업 중인지에 따라 접근 방식이 약간 다를 수 있습니다. 각 유형의 컴포넌트에 스타일을 적용하는 방법에 대한 안내는 다음과 같습니다:

<div class="content-ad"></div>

번개 웹 컴포넌트(LWC) 스타일링

# SLDS 클래스 사용하기

SLDS는 HTML 템플릿에서 직접 사용할 수 있는 포괄적인 CSS 클래스 세트를 제공합니다.

```html
```

<div class="content-ad"></div>

```js
<! - myComponent.html →
<template>
 <div class="slds-box slds-p-around_medium">
 <p class="slds-text-heading_medium">Hello, Salesforce!</p>
 </div>
</template>
```

# 사용자 정의 CSS 추가

동일한 이름을 가진 CSS 파일을 만들어 사용자 지정 스타일을 추가하십시오. 이 CSS 파일은 구성 요소에 스코프가 지정됩니다.

// myComponent.js
import ' LightningElement ' from ‘lwc’;


<div class="content-ad"></div>

```js
export default class MyComponent extends LightningElement ''

// myComponent.html
<template>
    <div class="custom-container">
        <p class="custom-text">Hello, Salesforce!</p>
    </div>
</template>

/* myComponent.css */
.custom-container {
    background-color: #f4f6f9;
    padding: 20px;
    border: 1px solid #d8dde6;
}

.custom-text {
    color: #16325c;
    font-size: 18px;
    font-weight: bold;
}

## Combining SLDS and Custom CSS

<div class="content-ad"></div>

<! - myComponent.html →
<template>
 <div class="slds-box custom-container">
 <p class="slds-text-heading_medium custom-text">안녕하세요, Salesforce!</p>
 </div>
</template>

/* myComponent.css */
.custom-container {
 border-bottom: 1px solid #000;
 width: 100%;
 box-sizing: border-box;
 margin: 0;
 padding: 0;
}
@media (max-width: 768px) {
 .custom-container {
 padding: 10px;
 }
}
.custom-text {
 color: #ff0000; /* 사용자 정의 색상 */
}

Best Practices — —

SLDS 활용: Salesforce의 디자인 원칙과 일관성을 유지하기 위해 표준 스타일링 작업에 SLDS 유틸리티 클래스를 활용하세요.
스코프된 CSS: LWC에서 CSS는 자동으로 스코프가 적용됩니다. Aura에서는.THIS 선택자를 사용하여 스타일을 컴포넌트에 스코프하세요.
반응형 디자인: SLDS 그리드와 유틸리티 클래스를 사용하고 커스텀 CSS에 미디어 쿼리를 추가하여 컴포넌트가 반응형인지 확인하세요.
!important 사용 최소화: CSS를 유지보수하기 어렵지 않도록 !important 선언을 삼가세요.
일관된 명명 규칙: 사용자 정의 CSS 클래스에 명확하고 일관된 명명 규칙을 사용하여 충돌을 피하고 유지 관리성을 향상시키세요.

<div class="content-ad"></div>

결론 -

Salesforce에서 사용자 정의 스타일링된 라이트닝 컴포넌트를 만들면 반응형이고 시각적으로 매력적이며 사용자 친화적인 애플리케이션을 구축할 수 있습니다. Salesforce 라이트닝 디자인 시스템(SLDS)과 사용자 정의 CSS를 활용하여 일관성을 유지하면서도 특정 디자인 요구 사항을 충족할 수 있습니다. 라이트닝 웹 컴포넌트(LWC) 또는 오라 컴포넌트를 사용하더라도 구성 요소를 효과적으로 스타일링하는 방법을 이해하는 것은 높은 품질의 사용자 경험을 제공하는 데 중요합니다.

추가 자료 -

Salesforce 개발에 대한 지식과 기술을 더 향상시키기 위해 다음 자료를 확인해보세요.

<div class="content-ad"></div>

대화에 참여해주세요 —

당신의 의견을 듣고 싶어요! 라이트닝 컴포넌트를 스타일링하면서 경험한 것이나 마주한 어려움을 공유해주세요. 당신에게 효과적이었던 조언이나 꿀팁이 있다면, 아래 댓글란에 공유해주세요.

연결 유지하기 —
더 많은 꿀팁, 튜토리얼, 업데이트를 보시려면, 트위터와 링크드인에서 저희를 팔로우해주세요. Salesforce 개발의 최신 소식을 받으시려면 뉴스레터로 구독해주세요.