---
title: "접근 가능한 이름 없는 폼이 ARIA 랜드마크로 노출되지 않는 이유"
description: ""
coverImage: "/assets/img/2024-07-24-FormswithoutanaccessiblenamearenotexposedasARIAlandmarks_0.png"
date: 2024-07-24 11:43
ogImage: 
  url: /assets/img/2024-07-24-FormswithoutanaccessiblenamearenotexposedasARIAlandmarks_0.png
tag: Tech
originalTitle: "Forms without an accessible name are not exposed as ARIA landmarks"
link: "https://dev.to/stefanjudis/forms-without-an-accessible-name-are-not-exposed-as-aria-landmarks-3132"
isUpdated: true
---




좋은 웹 시민이 되려면 의미론적이고 접근성이 있는 HTML을 사용해야 합니다. "주요 위치" 요소들은 a 요소들이며, 내비게이션은 nav 요소에 속해 있어야 합니다. 버튼은... 그러니까... 버튼이고, 폼 컨트롤은 폼 태그로 감싸야 합니다.

올바른 요소를 사용하면 HTML에 의미를 부여할 수 있습니다. 의미 있는 마크업은 기계가 다루고 있는 내용을 이해할 수 있도록 돕습니다. Googlebot이 사이트를 읽어들일 때, 제목을 찾아 중요한 섹션을 이해할 수 있습니다. 스크린 리더와 같은 보조 기술은 사이트를 탐색하고 사용자 경험을 더 좋게 만들어줄 수 있습니다. div 태그로 이뤄진 코드를 버리고 의미 있는 HTML을 사용해보세요. 정말 좋은 방법입니다!

예를 들어, Mac의 VoiceOver를 실행시키고 시맨틱 HTML에서 노출된 ARIA landmark 영역을 통해 사이트를 탐색할 수 있습니다.

![이미지](/assets/img/2024-07-24-FormswithoutanaccessiblenamearenotexposedasARIAlandmarks_0.png)

<div class="content-ad"></div>

배너는 헤더 요소로 매핑되고, 내비게이션은 nav 요소입니다. 그리고 접근 가능한 이름을 가진 요소가 있다면, 그것은 ARIA 역할 옆에도 나열됩니다. 멋지죠!

내비게이션 landmark를 살펴보면, 페이지에 두 개의 nav 요소가 있기 때문에 Main과 Footer의 접근 가능한 이름도 추가했습니다. 기사 제목들도 제목과 함께 풍부하게 표시했어요.

하지만! 알고 있어요. 페이지에 양식이 있는 것을 압니다. VoiceOver의 landmark 개요에서 뉴스레터 가입 양식이 왜 나타나지 않는 걸까요?

첫 번째 단계로, 개발 도구를 열어 액세스 가능성 패널을 검사해보았어요.

<div class="content-ad"></div>

![image](/assets/img/2024-07-24-FormswithoutanaccessiblenamearenotexposedasARIAlandmarks_1.png)

role=generic? 내 폼 요소에 접근 가능한 폼 역할이 아닌 일반 역할이 붙었다고? 저 배신감을 느껴요.

ARIA 사양을 빠르게 살펴보면 답이 나옵니다.

![image](/assets/img/2024-07-24-FormswithoutanaccessiblenamearenotexposedasARIAlandmarks_2.png)

<div class="content-ad"></div>

와우! 만약 당신이 양식에 접근 가능한 이름을 설정하지 않는다면, 그 양식에는 양식 역할이 없을 것입니다. 즉, 양식으로 표시되지 않아서 발견하기 어렵고 접근성이 떨어질 수 있어요.

어떻게 접근 가능한 이름을 제공할 수 있을까요?

다양한 방법이 있으며, 각각 요소에 따라 다릅니다.

예를 들어, 제목은 컨텐츠로부터 접근 가능한 이름을 받아요. 쉽죠. 양식 컨트롤은 연결된 레이블로부터 이름을 받아요. 입력란에 레이블을 달아주세요! 정말 그 요소에 따라 다르겠죠. 궁금하고 더 배우고 싶다면, "접근 가능한 이름과 설명 제공" 가이드를 읽어보세요. 그렇다면 양식에 대해서는 어떻게 할까요?

<div class="content-ad"></div>

요 WAI-ARIA 1.3 명세서에서 약간의 지침을 더 드릴게요:

그 말이 많은 의미가 있어요. 양식을 만들 때, 이 양식이 무엇을 나타내는지 알려주는 근처에 무언가 있어야 해요. 그렇다면 aria-labelledby를 사용하여 양식을 해당 목적을 나타내는 요소와 연결하고 액세스 가능한 이름을 제공할 수 있어요.

```js
<form aria-labelledby="form-heading">
  <h2 id="form-heading">뉴스레터 구독</h2>
  <!-- 더 많은 양식 내용 -->
</form>
```

만일 "이유"에 대한 시각적인 제목을 포함할 수 없다면, aria-label을 통해 문자열 값을 제공할 수도 있어요. 그러나 시각적 설명을 포함한 양식은 더 나은 사용자 경험을 제공하며, aria-label은 브라우저에서 번역을 사용하는 경우에도 오작동할 수 있어요.

<div class="content-ad"></div>


```js
<form aria-label="Newsletter sign up">
  <!-- more form stuff -->
</form>
```

위의 조정으로, 내 폼은 이제 VoiceOver에 표시되며 접근 가능한 이름으로 보강되었으며 적절한 폼 역할을 갖추었습니다.

<img src="/assets/img/2024-07-24-FormswithoutanaccessiblenamearenotexposedasARIAlandmarks_3.png" />

성공!
