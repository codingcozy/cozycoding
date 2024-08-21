---
title: "JavaScript Falsy 정리"
description: ""
coverImage: "/assets/img/2024-08-21-HowUnderstandingFalsyValuesCanImproveYourJavaScriptCode_0.png"
date: 2024-08-21 17:38
ogImage: 
  url: /assets/img/2024-08-21-HowUnderstandingFalsyValuesCanImproveYourJavaScriptCode_0.png
tag: Tech
originalTitle: "How Understanding Falsy Values Can Improve Your JavaScript Code"
link: "https://medium.com/@kushalsharma44/how-understanding-falsy-values-can-improve-your-javascript-code-dd2fc2465072"
isUpdated: true
updatedAt: 1724245843686
---


<img src="/assets/img/2024-08-21-HowUnderstandingFalsyValuesCanImproveYourJavaScriptCode_0.png" />

자바스크립트에서 falsy 값은 불리언 컨텍스트에서 "false"로 간주되는 값입니다. Falsy 값의 사용을 이해하면 코드에서 불리언 확인 수를 줄일 수 있습니다. 0 (음수 포함), null, undefined, false, NaN 및 빈 문자열 ("")은 모두 자바스크립트에서 falsy 값으로 간주됩니다.

여러분이 알아야 할 필수적인 이론입니다. 이제 이 개념을 실생활 예제에 적용하는 방법을 살펴봅시다. 아래에 몇 가지 실용적인 예제를 제공하겠습니다.

사용자 배열이 있다고 가정해봅시다:

<div class="content-ad"></div>

```js
const users = [
  { name: "Kushal", marks: 99 },
  { name: "" },
  { name: "John" },
  { name: null, marks: 87 },
  { marks: 34 },
  { name: "Bob", marks: 0 }
];
```

요구 사항 1: 이름이 적어도 1개의 문자를 포함하는 사용자를 출력합니다.

이를 달성하는 전형적인 방법은 다음과 같습니다:

```js
for (let i = 0; i < users.length; i++) { 
  if (users[i].name !== undefined && users[i].name !== null && users[i].name !== "") { 
    console.log("사용자 이름은", users[i].name); 
  } 
}
```

<div class="content-ad"></div>

그러나 falsy 값들을 사용하여 코드를 이렇게 간단하게 만들 수 있어요:

```js
for (let i = 0; i < users.length; i++) {
  if (users[i].name) {
    console.log("사용자 이름은", users[i].name, "입니다.");
  } else {
    console.log("N/A");
  }
}
```

이 경우, 이름이 undefined, null, 빈 문자열이거나 기타 falsy 값 중 하나인 경우에는 if 블록에서 false로 간주됩니다.

요구사항 2: 사용자 이름이 존재하면 사용자 이름을 출력하고, 그렇지 않으면 “N/A”를 출력하세요.

<div class="content-ad"></div>

일반적인 방법은 다음과 같습니다:

```js
for (let i = 0; i < users.length; i++) {
  if (users[i].name !== undefined && users[i].name !== null && users[i].name !== "") {
    console.log("사용자 이름은", users[i].name);
  } else {
    console.log("사용자 이름은 N/A입니다");
  }
}
```

Falsy 값을 사용하면 코드를 간소화할 수 있습니다:

```js
for (let i = 0; i < users.length; i++) {
  console.log("사용자 이름은", users[i].name || "N/A");
}
```

<div class="content-ad"></div>

이 기사가 도움이 되었기를 바랍니다! 도움이 되셨다면 좋아요와 댓글을 남겨주시면 감사하겠습니다.