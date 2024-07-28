---
title: "자동 완성 기능 완전히 끄는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-28 13:41
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "How to Disable Autofill for Real"
link: "https://dev.to/gera2ld/how-to-disable-autofill-for-real-1411"
---


알고 계시겠지만, 브라우저는 입력란에 대해 자동 완성 및 자동 기입 기능을 제공합니다.

하지만 때로는 이를 필요로 하지 않을 때가 있습니다. 예를 들어, 입력 값이 매번 다른 값임을 알고 있는 경우입니다.

자동 완성이 어떻게 작동하는지 살펴보고 이를 비활성화하는 신뢰할 수 있는 방법이 있는지 알아봅시다.

## Autocomplete vs Autofill

<div class="content-ad"></div>

자동 완성은 제안과 같이 동작합니다. 따라서 사용자 이름과 같은 내용을 입력하면 브라우저가 해당 내용을 저장하고 다음에 이 웹사이트를 열 때 동일한 값을 제안합니다.

자동완성은 다르게 작동합니다. 브라우저가 인식하는 사용자 이름, 비밀번호 및 다른 양식 필드를 미리 채워넣으려고 시도합니다. 이는 일반적으로 브라우저의 내장 비밀번호 관리자에 의해 수행됩니다.

비밀번호 관리자는 또한 어떤 값을 채워 넣어야 할지 감지하기 위해 자동 완성을 사용합니다.

## 자동 완성 속성

<div class="content-ad"></div>

브라우저에서는이 동작을 제어할 수 있도록 자동완성 속성을 제공합니다. 자세한 내용은 MDN을 참조하세요.

이 속성을 사용하면 브라우저에 입력 내용을 어떻게 채울지 알려줄 수 있습니다. 예를 들어:

- `input autocomplete="off"` - 자동 완성 끄기
- `input autocomplete="email"` - 사용자 이메일로 자동 채우기
- `input autocomplete="one-time-code"` - 일회용 비밀번호

브라우저는 `autocomplete="one-time-code"`을 인식하여 당연히 잘못된 값으로 채우지 않습니다.

<div class="content-ad"></div>

## 비밀번호 관리자

비밀번호 관리자는 다른 이야기를 가지고 있어요.

일부로 자동 완성 속성을 존중합니다. 예를 들어, `input autocomplete="new-password"`는 새 비밀번호를 위한 것이라는 것을 알고 있어요. 그래서 현재 비밀번호로 채워지지 않아야 해요. 하지만 그게 다에요.

억지로 비밀번호 관리자와 은행 사이에 이야기가 있었다고 하더라고요.

<div class="content-ad"></div>

은행들이 브라우저가 비밀번호를 기억하고 자동으로 입력하는 것을 금방 발견하자, 비밀번호가 쉽게 유출될 수 있다는 걱정을 했습니다. 그래서 항상 autocomplete="off"를 추가하고 사용자들을 손으로 비밀번호를 입력하도록 강제했습니다. 결과적으로 사용자들은 기억하기 쉬운 간단한 비밀번호를 설정하는 경향이 있었습니다.

비밀번호 관리자 개발자들은 은행들의 의견에 동의할 수 없었습니다. 그들은 비밀번호 관리자가 각 서비스에 대해 다른, 보다 복잡한 비밀번호를 제공할 수 있고, 그렇기 때문에 더 안전하다고 생각했습니다. 그리고 한 비밀번호가 해킹당해도 다른 것들은 안전하게 보호됩니다. 그래서 그들은 특히 사용자명과 비밀번호와 관련된 필드에 대해 autocomplete="off" 속성을 무시하기로 결정했습니다.

게다가, 비밀번호 관리자들은 항상 스마트하려고 노력합니다. 그래서 입력에 autocomplete 속성이 없는 경우 스스로 이를 해결하려고 노력합니다.

비밀번호 입력란이 있는지 쉽게 확인할 수 있습니다. 우리는 `input type="password"`를 찾기만 하면 됩니다. 비밀번호 입력란이 발견되면 브라우저는 사용자명을 위한 입력란을 찾으려고 할 것입니다. 만약 `input autocomplete="username"`가 발견되지 않으면, 그냥 비밀번호 입력란 이전에 있는 가장 가까운 입력란을 사용자명 입력란으로 취급합니다.

<div class="content-ad"></div>

여기 몇 가지 예시입니다:


<!-- `input[autocomplete="username"]`은 사용자 이름 입력으로 간주됩니다 -->
<div>Input 1: <input></div>
<div>Input 2: <input type="password"></div>          <!-- 비밀번호 -->
<div>Input 3: <input autocomplete="username"></div>  <!-- 사용자 이름 -->



<!-- 유일한 다른 입력은 사용자 이름 입력으로 간주됩니다 -->
<div>Input 1: <input type="password"></div>  <!-- 비밀번호 -->
<div>Input 2: <input></div>                  <!-- 사용자 이름 -->



<!-- 비밀번호 입력 바로 앞에 있는 입력은 사용자 이름 입력으로 간주됩니다 -->
<div>Input 1: <input></div>                  <!-- 사용자 이름 -->
<div>Input 2: <input type="password"></div>  <!-- 비밀번호 -->
<div>Input 3: <input></div>


<div class="content-ad"></div>

## 자동완성 비활성화 방법

만약 암호 입력란이 있지만 다른 입력란에는 자동완성이 되지 않기를 기대한다면 어떻게 해야 할까요?

예를 들어, 사용자가 이미 로그인했지만 민감한 작업을 위해 암호를 다시 입력할 것으로 기대하는 경우입니다.

```js
<div>일회용 메모: <input placeholder="자동완성 금지"></div>
<div>암호: <input type="password"></div>
```

<div class="content-ad"></div>

브라우저 또는 비밀번호 관리자가 autocomplete=off 지시를 완전히 따르지 않을 수 있으므로 우리는 그들을 속일 방법을 찾아야 할 수도 있어요.

여기 해결책이 있어요: 숨은 입력란을 추가하고 브라우저에게 그것을 사용자명 입력란으로 처리하라고 알려주는 거야.

```js
<div>일회용 메모: <input placeholder="Don't autofill me"></div>
<input autocomplete="username" style="display:none">
<div>비밀번호: <input type="password"></div>
```

됐어요!

<div class="content-ad"></div>

`input type="hidden" autocomplete="username"`은 의도한 대로 작동하지 않을 수 있습니다. hidden 타입의 input은 일반 input 구성 요소가 아니므로 단순히 무시됩니다.

## 핵심 포인트

- Autocomplete은 이전 입력에 대한 제안을 제공하며, autofill은 모든 인식된 필드를 채웁니다.
- 브라우저들이 은행들과 대립했고, 그들은 암호 관련 필드에 대해 autocomplete=off을 무시하기로 결정했습니다.
- 입력 필드에 대한 autofill 비활성화를 위해 적절한 autocomplete 속성을 사용해야 하며, 다른 필드를 보호하기 위해 사용자 이름용 hidden input을 추가해야 할 수도 있습니다.