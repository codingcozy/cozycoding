---
title: "ReactJS에서 useState의 Polyfill 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-CreatingaPolyfillforuseStateinReactJSAStep-by-StepGuide_0.png"
date: 2024-08-21 17:26
ogImage: 
  url: /assets/img/2024-08-21-CreatingaPolyfillforuseStateinReactJSAStep-by-StepGuide_0.png
tag: Tech
originalTitle: "Creating a Polyfill for useState in ReactJS A Step-by-Step Guide"
link: "https://medium.com/@rahuulmiishra/creating-a-polyfill-for-usestate-in-reactjs-a-step-by-step-guide-20dcee6cef74"
isUpdated: true
updatedAt: 1724245986288
---


## ReactJS 인터뷰 비밀: useState 폴리필을 전문가처럼 구현하는 방법

![2024-08-21-CreatingaPolyfillforuseStateinReactJSAStep-by-StepGuide_0.png](/assets/img/2024-08-21-CreatingaPolyfillforuseStateinReactJSAStep-by-StepGuide_0.png)

요즘에는 React 개발자와의 인터뷰를 진행하는 모든 면접관에게 이 질문이 최우선 선택 사항입니다.

이 기사는 전통적인 인터뷰 스타일을 사용하여 진행하겠습니다.

<div class="content-ad"></div>

면접관: useState가 무엇인지 알고 계신가요?

저: 네, useState는 React에서 제공하는 내장 훅으로, 함수형 컴포넌트에서 상태 관리 기능을 제공함으로써 반응형 변수와 해당 변수를 업데이트하는 함수를 제공합니다.

useState로 제공된 특별한 반응형 변수를 함수를 통해 업데이트하면, 이전 값과 새 값 사이에 차이가 있으면 컴포넌트가 해당 값을 업데이트하여 다시 렌더링됩니다.

![이미지](/assets/img/2024-08-21-CreatingaPolyfillforuseStateinReactJSAStep-by-StepGuide_1.png)

<div class="content-ad"></div>

면접관: 좋아요. 이게 바로 useState가 작동하는 방식이에요. 이제 내가 커스텀 훅의 이해를 확인하고 싶어서 내부 작동 방식을 알 수 있는 polyfill을 작성해볼래요?

나: 물론이죠. 여기서 작동할 수 있을거에요.

![Creating a Polyfill for useState in React - A Step-by-Step Guide](/assets/img/2024-08-21-CreatingaPolyfillforuseStateinReactJSAStep-by-StepGuide_2.png)

면접관: 좋아요. 하지만 값이 업데이트 될 때 업데이트가 트리거되지 않아요. 이걸 수정해 줄 수 있을까요?

<div class="content-ad"></div>

당신: 물론이죠, useState은 내부적으로 상태 업데이트를 관리하기 위해 useReducer을 사용하기 때문에 그것을 활용할 수 있어요. 여기 그 링크예요.

![링크](/assets/img/2024-08-21-CreatingaPolyfillforuseStateinReactJSAStep-by-StepGuide_3.png)

면접관: 좋은 점수를 받았어요, 하지만 이 강제 업데이트 때문에 모든 상태 업데이트마다 트리거될 것 같지 않나요?

당신: 아, 맞아요, 완전히 간과했어요. 이 문제를 고쳐야겠네요.

<div class="content-ad"></div>

<img src="/assets/img/2024-08-21-CreatingaPolyfillforuseStateinReactJSAStep-by-StepGuide_4.png" />

면접관: 이 조건을 추가한 후 이제 시작부터 존재해온 버그에 대해 설명해볼까요? initialValue = initialValue || value; 에 집중해서 어떠한 잠재적인 문제가 발생할 수 있는지 확인해볼 수 있을까요?

나: 네, 정말요. 이제 눈에 띄게 지적해 주셨네요 🦅. 이것은 문제를 발생시킬 수 있겠죠. 기본적으로 ||는 만약 왼쪽이 거짓 값(0, false, '', undefined, null, NaN, -0, 0n)이라면 오른쪽 값이 할당됩니다.

우리의 경우에는 newValue가 0이고 oldValue가 1이라면, newValue는 결코 할당받지 못할 것입니다. 이 문제를 ??를 사용하여 간단히 해결할 수 있습니다. ??는 왼쪽 값이 null 또는 undefined인 경우에만 오른쪽 값을 할당합니다.

<div class="content-ad"></div>

initialValue = initialValue ?? value;

면접관: 대단한데, 하나 더 문제가 있네요. 여러 useMyState 호출이 있을 때는 작동하지 않을 거에요. 이 부분을 어떻게 해결하겠습니까?

나: 네, 해볼게요.

![Image](/assets/img/2024-08-21-CreatingaPolyfillforuseStateinReactJSAStep-by-StepGuide_5.png)

<div class="content-ad"></div>

면접관: Bravo !!! 성공했네요. 🙌
나: 고마워요 !! 🥹

이것이 무언가를 배우는 데 도움이 되었기를 바랍니다. 여기 빠뜨린 부분이 있으면 알려주세요.