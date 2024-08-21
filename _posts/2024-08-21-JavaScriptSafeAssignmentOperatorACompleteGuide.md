---
title: "JavaScript 안전 대입 연산자 () 완벽한 안내"
description: ""
coverImage: "/assets/img/2024-08-21-JavaScriptSafeAssignmentOperatorACompleteGuide_0.png"
date: 2024-08-21 17:22
ogImage: 
  url: /assets/img/2024-08-21-JavaScriptSafeAssignmentOperatorACompleteGuide_0.png
tag: Tech
originalTitle: "JavaScript Safe Assignment Operator () A Complete Guide"
link: "https://medium.com/@shahbishwa21/introduction-to-the-safe-assignment-operator-in-javascript-ddc35e87d37c"
isUpdated: false
---



![image](/assets/img/2024-08-21-JavaScriptSafeAssignmentOperatorACompleteGuide_0.png)

자바스크립트는 새로운 연산자, ?=인 '안전 대입 연산자'를 소개하고 있습니다. 이 연산자는 코드 내에서 오류 처리를 간편하고 더 읽기 쉽게 만들어줍니다. 특히 실패할 수 있는 함수나 오류를 발생시킬 수 있는 상황에서 유용합니다.

## ?= 연산자는 무엇을 하는가?

?= 연산자를 사용하면 함수나 작업의 성공 여부를 확인합니다. 성공하면 결과를 반환하고, 실패하면 프로그램이 크래시되지 않고 오류를 반환합니다.


<div class="content-ad"></div>

여기 작동 방식이 있어요:

```js
const [error, result] ?= await fetch("https://example.com/data");
```

- fetch가 성공하면, error는 null이 되고 result에 데이터가 저장됩니다.
- fetch가 실패하면, error엔 에러 세부사항이 담기고 result는 null이 됩니다.

## ?= 연산자를 사용하는 이유?

<div class="content-ad"></div>

- 오류 처리를 간단하게: 복잡한 try-catch 블록을 작성할 필요가 없어집니다.
- 코드를 더 깔끔하게: 코드가 읽고 이해하기 쉬워집니다.
- 일관된 동작: 코드의 다양한 부분에서 오류를 처리하는 일관된 방법을 제공합니다.

## 예시: ?=를 사용한 오류 처리

데이터를 가져오고 가능한 오류를 처리하는 방법에 대한 예시:

```js
async function getData() {
  const [fetchError, response] ?= await fetch("https://api.example.com/data");

  if (fetchError) {
    console.error("Fetch error:", fetchError);
    return;
  }

  const [jsonError, jsonData] ?= await response.json();

  if (jsonError) {
    console.error("JSON error:", jsonError);
    return;
  }

  return jsonData;
}
```

<div class="content-ad"></div>

이 예제에서 ?= 연산자는 각 단계에서 오류를 쉽게 확인할 수 있게 도와줌으로써 코드를 더 안전하고 관리하기 쉽게 만들어줍니다.

## 결론

안전 대입 연산자 ?=은 JavaScript 개발자들에게 강력한 도구입니다. 특히 깔끔하고 신뢰할 수 있고 유지보수가 쉬운 코드를 작성하고 싶은 경우에 도움이 됩니다. 오류 처리를 간소화함으로써 예기치 않은 충돌을 막아주고 코드를 더 견고하게 만들어줍니다. Promise, async 함수 또는 오류를 발생시킬 수 있는 모든 것들과 함께 작업하고 있다면, ?= 연산자를 한번 시도해보세요!

## 참고:

<div class="content-ad"></div>

- [proposal-safe-assignment-operator](https://github.com/arthurfiorette/proposal-safe-assignment-operator)