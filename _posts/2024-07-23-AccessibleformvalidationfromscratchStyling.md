---
title: "쉽게 따라하는 접근성 높은 폼 유효성 검사와 스타일링 방법"
description: ""
coverImage: "/assets/img/2024-07-23-AccessibleformvalidationfromscratchStyling_0.png"
date: 2024-07-23 11:23
ogImage: 
  url: /assets/img/2024-07-23-AccessibleformvalidationfromscratchStyling_0.png
tag: Tech
originalTitle: "Accessible form validation from scratch  Styling"
link: "https://medium.com/user-experience-design-1/accessible-form-validation-from-scratch-styling-6810b1a21b36"
isUpdated: true
---




<img src="/assets/img/2024-07-23-AccessibleformvalidationfromscratchStyling_0.png" />

이 글에서는 양식에 스타일을 추가하여 JavaScript 대신 CSS가 할 수 있는 일을 최대한 활용하려고 합니다.

다음 (그리고 아마도 마지막) 글에서는 검증을 수행하기 위해 JavaScript를 추가할 것입니다.

# 소개

<div class="content-ad"></div>

아래는 이 시리즈의 기사 링크입니다 (지금까지):

- 파트 1: 요구 사항
- 파트 2: 마크업
- 파트 3: 유효성 검사 준비
- 파트 4: 스타일링

이 기사에서 다룰 주제는 다음과 같습니다:

- 눈이 아프지 않도록 만들기
- 초점 관리
- 유효성 검사 영역 강조
- 즉시 관련이 없는 것 숨기기
- aria-invalid 사용하여 인라인 유효성 검사 표시
- 추가 기능 및 장식성

<div class="content-ad"></div>

내가 최대한 간단한 스타일을 유지하려고 노력할 거야. 정당화 되었을 때 초점을 두고, 합리적인 곳에서는 분리를 제공할 거야. 내가 사용하는 색상과 글꼴은 매우 기본적이야. 스타일링은 템플릿이고 더도 덜도 아무것도 아니야. CSS 마법사들에게 아름다운 마법을 부탁할게.

# 눈 아프지 않게 만들기

Part 3에서 솔루션이 어떻게 보였는지 살펴보자:

조금 더 프레젠테이션하기 좋게 최소한의 스타일 변경을 해보자. 이렇게 추가할 거야:

<div class="content-ad"></div>

- 텍스트 상자와 선택 컨트롤의 높이를 늘립니다.
- 입력 그룹, 요약 유효성 및 인라인 유효성에 더 많은 패딩 및 여백 값을 추가합니다.
- 입력 그룹에 회색 하단 테두리를 추가합니다.
- 레이블의 글꼴 크기와 굵기를 늘립니다.
- 도움말 텍스트의 글꼴 크기를 약간 줄입니다.
- 제출 버튼의 여백 및 패딩을 늘립니다.

```js
input, label, fieldset, legend {
  display: block;
}

input[type="checkbox"], 
input[type="radio"],
span
{
  display: inline;
}

input[type="email"],
input[type="password"],
input[type="text"],
select
{
  height: 1.5rem;
}

#sectionValidation {
  padding: 1rem;
  margin: .5rem;
}

.inputGroup {
  padding: .5rem;
  border-bottom: .1rem #ccc solid;

  label {
    font-size: 1.1rem;
    font-weight: bold;
  }
  .helpText {
    font-size: .95rem;
  }

  .errorMessage {
    padding: .5rem;
    margin: .3rem;
  }
}

button {
  padding: .5rem;
  margin: 1rem;  
}
```

여기에는 크게 중요한 사항은 없습니다 — 그리고 이것이 렌더링된 모습입니다 (첫 두 입력 그룹 보기):

<img src="/assets/img/2024-07-23-AccessibleformvalidationfromscratchStyling_1.png" />

<div class="content-ad"></div>

# 포커스 관리

## Outline-offset

기본적으로 포커스된 텍스트 상자는 텍스트에 굵은 스타일을 추가한 것과 유사합니다. 가장 명확한 스타일 변경이 아닙니다. 포커스된 컨트롤을 더 돋보이게 만들기 위해 다음을 추가했습니다:

```js
input, select, button {
  outline-offset: .3rem;
}
```

<div class="content-ad"></div>

기본 아웃라인 포커스 표시기를 사용하여 컨트롤의 테두리에서 밀어내어 포커스된 컨트롤과 포커스가 해제된 컨트롤을 약간 더 쉽게 구분할 수 있습니다.

다음은 비교입니다:

![비교 이미지](/assets/img/2024-07-23-AccessibleformvalidationfromscratchStyling_2.png)

스타일링은 매우 최소한이며, 색상, 더 큰 아웃라인 두께 및 더 큰 오프셋을 사용하여 사용할 수 있습니다.

<div class="content-ad"></div>

추가 자료:

- MDN Web Docs: outline
- Sara Soueidan의 접근성 및 WCAG 준수 초점 표시기 설계 가이드
- Andy Barnes의 접근성이 뛰어난 사용자 정의 초점 표시기

## 초점이 맞춰진 입력 그룹 강조

양식에서는 사용자가 어떤 것을 추측해야 하는 것을 원하지 않습니다. 특히 어느 입력 컨트롤이 초점을 가졌는지에 대해서는 절대 추측하게 하고 싶지 않습니다. 그래서 이전에 작성한 글에서 입력 그룹 (.inputGroup)을 고려했습니다.

<div class="content-ad"></div>

입력 그룹 내의 입력 컨트롤이 포커스를 받으면 전체 입력 그룹이 강조 표시되도록 하려고 합니다.

이것은 :focus-within 가상 클래스를 사용하여 수행할 수 있습니다.

```js
.inputGroup:focus-within {
    background-color: #F2FF8F; /* 연한 노랑 */
    box-shadow: 0 0 3px black;
}
```

여기서는 :focus-within을 사용하여 해당 그룹 내의 컨트롤에 포커스가 있는 경우 전체 입력 그룹에 스타일을 적용하고 있습니다.

<div class="content-ad"></div>

저는 포커스를 받는 입력 그룹 주변에 강조 효과를 주기 위해 상자 그림자를 추가하고 있어요. 입력 그룹의 내용이 아래로 내려가거나 오른쪽으로 이동하는 것을 방지하기 위해 테두리 대신 상자 그림자를 사용하고 있어요.

아래 링크에서 어떻게 보이는지 확인해보세요:

![image](https://miro.medium.com/v2/resize:fit:1400/1*ytbUYavoxPFNGevg37L6LA.gif)

과한가요? 저는 과한 것 같지 않아요. Maya Shavin의 논문인 "포커스 가시성 향상 - focus-within 또는 has(:focus)?"에서 언급하듯이요.

<div class="content-ad"></div>

입력 그룹의 도움말 텍스트를 모두 숨겨놓고, 포커스를 받을 때까지 숨겨둘 거야. 하지만 그 부분은 나중에 기사에서 다룰 거야.

더 많은 정보:

- MDN 웹 문서: focus-within
- MDN 웹 문서: box-shadow

# 유효성 검사 영역 강조

<div class="content-ad"></div>

이렇게 하면 어때요? `table` 태그를 Markdown 형식으로 변경하겠습니다.


| Property | Value     |
|----------|-----------|
| Name     | John Doe  |
| Age      | 30        |

```js
#sectionValidation {
  background-color: pink;
  padding: 1rem;
  margin: .5rem;
  color: #630000;
}

.errorMessage {
  background-color: pink;
  padding: .5rem;
  margin: .3rem;
  color: #630000;
}
```

당연히 CSS를 더 깔끔하게 만들기 위해 변수와 중첩을 사용할 수 있지만, 지금은 그렇게 신경 쓸 필요가 없어요.

또한, 이번에 좀 더 일찍 해야 했는데, 요약된 유효성 메시지를 `h2` 요소로 만들고 더미 불릿을 넣을 거에요.

<div class="content-ad"></div>

여기에 원본 텍스트를 한국어로 번역한 내용입니다:

<img src="/assets/img/2024-07-23-AccessibleformvalidationfromscratchStyling_3.png" />

이전 글에서 언급한 대로, 요약 유효성 검사 섹션의 항목들을 관련된 입력 컨트롤로 링크하겠습니다.

# 즉시 관련이 없는 것을 숨기는 방법

<div class="content-ad"></div>

지금 대부분의 양식을 채우고 스타일링했으니, 아직 보여줄 필요가 없는 부분을 제거해 봅시다. 이에는 다음이 포함됩니다:

- 요약 검증
- 인라인 검증
- 포커스되지 않은 입력 그룹의 도움말 텍스트

첫 두 가지는 명백합니다. 검증이 발생하지 않는 한 표시하지 않아야 할 것입니다. 그러나 포커스되지 않은 필드에 대한 도움말 텍스트가 표시되어야 하는 이유를 보지 못합니다. 필드에 의미 있는 의존성이 없으며, 포커스되지 않은 도움말 텍스트는 양식의 소음만 늘리게 됩니다.

도움 텍스트(또는 인라인 검증)를 숨기는 방법은 개인적인 선호에 따라 다릅니다.

<div class="content-ad"></div>

## display: none으로 숨기기

display: none;으로 요소를 스타일링하면 요소의 내용물 뿐만 아니라 차지하던 공간도 제거됩니다. 결과적으로 나중에 해당 요소를 표시하면 그 아래의 내용물이 아래쪽으로 이동하고 오른쪽으로 이동할 수 있습니다.

## visibility: hidden으로 숨기기

visibility: hidden을 사용하여 내용물을 숨길 수도 있습니다. 이 경우 숨긴 내용물은 여전히 차지하던 공간을 유지합니다. 그 결과로 나타낼 때 내용물은 이동하지 않을 것입니다 (요소 내의 내용물을 변경하는 경우를 제외하고).

<div class="content-ad"></div>

## CSS를 사용하여 JavaScript 대신에

이 예제에서 (그리고 웹 개발 전반에서) 나의 한 가지 원칙은 JavaScript 대신 CSS를 사용하는 것입니다.

따라서 JavaScript나 jQuery를 사용하여 인라인 유효성 검증 또는 도움말 텍스트를 숨기거나 표시하는 대신, 포커스를 이용하고 aria-invalid 속성을 이용하여 CSS가 처리하도록 할 것입니다.

모든 유효성 요소와 도움말 텍스트를 숨겨보겠습니다. 이 예에서는 유효성 요소를 숨기기 위해 display: none을 사용하고 도움말 텍스트를 숨기기 위해 visibility: hidden을 사용할 것입니다.

<div class="content-ad"></div>

```js
#sectionValidation {
  background-color: pink;
  padding: 1rem;
  margin: .5rem;
  color: #630000;
  display: none;  /* 요약 유효성 검사 숨기기 */
}

.errorMessage {
  background-color: pink;
  padding: .5rem;
  margin: .5rem;
  color: #630000;
  display: none;  /* 인라인 유효성 검사 숨기기 */
}

.helpText {
  font-size: .95rem;
  visibility: hidden; /* 도움말 텍스트 숨기기 */
}
```

이제 우리의 양식은 다음과 같습니다:

<img src="/assets/img/2024-07-23-AccessibleformvalidationfromscratchStyling_4.png" />

양식이 훨씬 관리하기 쉬워 보이죠?

<div class="content-ad"></div>

같은 방법으로 도움말 텍스트를 보여줄 수 있어요. 입력 그룹을 강조하는 방법과 동일하게 (:focus-within 가상 클래스를 사용하여) 해볼게요:

```js
.inputGroup {
  padding: .5rem;
  border-bottom: .1rem #ccc solid;

  .helpText {
    font-size: .95rem;
    visibility: hidden;
  }

  &:focus-within {
    background-color: #F2FF8F;
    box-shadow: 0 0 3px black;
    .helpText {
      visibility: visible; /* 컨트롤이 포커스를 받았을 때 도움말 텍스트 표시 */
    }
  }

...
}
```

이렇게 보이게 됩니다:

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*Ya4cec6He9Uhm4QH-ezrCA.gif)

<div class="content-ad"></div>

얏호! 우리가 할 작업을 보시다시피, 표시기가 너무 가까워서 도움말 텍스트에 약간 여백을 추가해야 합니다:

```js
.helpText {
    font-size: .95rem;
    visibility: hidden;
    margin-bottom: .5rem; /* Don't stand so close to me */
}
```

이것으로 해결될 것입니다:

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*TuJJJhG0XZdQ8W9OC_KHGQ.gif)

<div class="content-ad"></div>

좋아요, 훨씬 나아졌네요.

# 인라인 유효성 검사를 표시하는 방법: aria-invalid 사용하기

## aria-invalid란 무엇인가요?

aria-invalid 속성은 보조 기술(AT)을 사용하는 사용자에게 제출된 양식의 데이터가 잘못되었음을 알려줍니다.

<div class="content-ad"></div>

이 속성은 JavaScript에 의해 설정되어야 합니다. 이에 대해 다음 글에서 다룰 것입니다. input 요소에 aria-invalid가 true로 설정되면, JAWS와 NVDA가 일반적으로 "잘못된 입력"이라고 말합니다.

## CSS를 사용하여 인라인 유효성 검사 표시하기

유효성 검사를 수행하는 JavaScript 단계에 도달했을 때, 그 기능을 최소화하고 싶습니다. 스타일을 변경하고 싶다면, JavaScript보다는 CSS에서 처리하고 싶습니다.

JavaScript는 인라인 유효성 검사 텍스트를 추가하고, 관련 입력 제어에 aria-invalid 속성을 추가할 것입니다.

<div class="content-ad"></div>

해당 속성이 있는 경우 CSS를 사용하여 인라인 검증을 표시할 수 있습니다:

```js
.inputGroup:has([aria-invalid="true"]) > .errorMessage
  {    
      display: block; 
  }
```

더 읽어보기:

- MDN Web 문서: aria-invalid
- Omar Andani의 '사용자 친화적인 양식 만들기'

<div class="content-ad"></div>

# 벨과 불룩

## 화살표와 체크 표시 추가하기

aria-invalid을 사용하여 인라인 오류 메시지를 숨기거나 표시하는 것 외에도, 오류가 있는지 또는 오류가 수정되었는지를 추가적으로 시각적으로 표시할 수 있습니다.

화살표나 체크 표시를 꺼내기 전에, 먼저 해야 할 일은:

<div class="content-ad"></div>

- `form` 요소를 margin-left를 사용하여 오른쪽으로 이동시킵니다.
- :: 이모티콘을 절대 위치로 표시할 수 있도록 .inputGroup 클래스에 position: relative를 추가합니다.
- 선택자 구조를 약간 바꿉니다.

aria-invalid="true"를 가진 자식 요소 중 하나라도 있는 입력 그룹마다 빨간색 화살표를 추가하려고 합니다.

aria-invalid="false"를 가진 자식 요소 중 하나라도 있는 입력 그룹마다 녹색 체크 마크를 추가하려고 합니다.

이러한 속성과 값을 추가하고 전환하는 작업은 JavaScript에서 처리될 것입니다. aria-invalid 속성이 없는 컨트롤에는 이러한 기호가 표시되지 않습니다.

<div class="content-ad"></div>

```js
form {
  margin-left: 2rem;
}

...

.inputGroup {
  padding: 0.5rem;
  border-bottom: 0.1rem #ccc solid;
/* 화살표와 체크마크의 절대 위치 지정을 용이하게 하기 위해 이 부분을 추가했어요 */
  position: relative;  
...

/* 어떤 요소라도 aria-invalid="true"가 있으면... */
&:has([aria-invalid="true"]) {
    &:before {
      content: "\2192";  /* 오른쪽 화살표 */
      position: absolute;
      left: -2.0rem;
      top: 50%;
      transform: translateY(-50%);
      font-size: 2rem;
      color: red;
    }
    > .errorMessage {
      display: block;
    }
  }
&:has([aria-invalid="false"]) {
    &:before {
      content: "\2713"; /* 체크 마크 */
      position: absolute;
      left: -2.0rem;
      top: 50%;
      transform: translateY(-50%);
      font-size: 2rem;
      color: green;
    }
  }

...

}
```

이번에는 비밀번호와 비밀번호 확인 필드에 aria-invalid="true" 속성을 추가하고, 이메일 필드에는 aria-invalid="false" 속성을 추가해보세요. 어떻게 보이는지 확인해봅시다:

<img src="/assets/img/2024-07-23-AccessibleformvalidationfromscratchStyling_5.png" />

물론, 이것은 할 수 있는 예시일 뿐이에요. 중복적인가요? 절대로요! 하지만 모든 중복은 나쁜 것은 아니에요.

<div class="content-ad"></div>

## 약간 수정

선택 컨트롤의 크기를 늘리고 포커스 표시기가 레이블과 겹치지 않도록 상단 마진을 추가했어요.

```js
select {
  margin-top: 0.5rem;
  height: 2rem;
}
```

또한 체크박스와 라디오 버튼의 레이블이 텍스트 상자의 레이블보다 굵고 글꼴 크기가 크다는 것을 깨달았어요. 그 부분도 수정했어요.

<div class="content-ad"></div>

```js
.inputGroup {

...

&:has(input[type="text"], input[type="password"], input[type="email"], select)
  { 
    label {
      font-size: 1.1rem;
      font-weight: bold;
    }
  }
  legend {
      font-size: 1.1rem;
      font-weight: bold;    
  }

...

}

```

CSS 정리는 더 이상 신경 쓰지 않을 거예요. 그것은 이 시리즈의 목적이 아니에요.

지금까지의 솔루션을 여기까지 적용해보세요:

이게 지금은 충분해요. 다음 글에서는 JavaScript를 추가해서 유효성 검사를 수행할 거에요.


<div class="content-ad"></div>

# 링크

## 시리즈의 기사

- 제 1부: 요구 사항
- 제 2부: 마크업
- 제 3부: 유효성 검사 준비
- 제 4부: 스타일링

## 언급됨

<div class="content-ad"></div>

- MDN Web Docs: outline
- Sara Soueidan의 접근성 있는, WCAG 적합한 포커스 표시자 설계 안내서
- Andy Barnes의 접근성 있는 맞춤형 포커스 표시자
- Maya Shavin의 포커스 가시성 향상 — focus-within 또는 has(:focus)?
- MDN Web Docs: focus-within
- MDN Web Docs: box-shadow
- MDN Web Docs: aria-invalid
- Omar Andani의 폼을 사용자 친화적으로 만들기

## 추가로 읽을거리

- Taras Bakusevych의 텍스트 필드 및 폼 디자인 — UI 구성 요소 시리즈
- Kate Williamson Kaplan의 오류 메시지에서의 적적한 패턴