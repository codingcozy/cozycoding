---
title: "자바스크립트 delete 연산자의 비밀을 털어라"
description: ""
coverImage: "/assets/img/2024-09-10-TheSecretsofthedeleteOperatorinJavaScript_0.png"
date: 2024-09-10 18:18
ogImage: 
  url: /assets/img/2024-09-10-TheSecretsofthedeleteOperatorinJavaScript_0.png
tag: Tech
originalTitle: "The Secrets of the delete Operator in JavaScript"
link: "https://medium.com/itnext/the-secrets-of-the-delete-operator-in-javascript-1d56080b8490"
isUpdated: false
---


<img src="/assets/img/2024-09-10-TheSecretsofthedeleteOperatorinJavaScript_0.png" />

내 뉴스레터에서 원래 발행되었습니다.

delete 연산자는 자바스크립트의 오래된 언어 기능 중 하나로 볼 수 있습니다. 이름 그대로 무언가를 파괴하려는 의도를 갖고 있지만, 정확히 무엇을 파괴할 수 있을까요?

# delete 0

<div class="content-ad"></div>

삭제 작업 수행 시 0이 실행 시스템에서 삭제됩니까?

당연히 그렇지 않습니다. 실제 목적은 객체의 속성 참조를 삭제하는 것입니다.

```js
delete object.property;
delete object['property'];
```

일반적으로 성공적인 삭제는 true를 반환하고 실패는 false를 반환하지만, 몇 가지 예외 사항이 있습니다.

<div class="content-ad"></div>

# 소유 속성

삭제 연산자는 자체 속성에만 작동합니다. 프로토타입 체인에 동일한 이름의 속성이 있는 경우, 이 속성은 건너뛰게 됩니다.

![이미지](/assets/img/2024-09-10-TheSecretsofthedeleteOperatorinJavaScript_1.png)

# 존재하지 않는 속성

<div class="content-ad"></div>

만약 삭제한 속성이 존재하지 않는다면, 삭제는 효과가 없지만 여전히 true를 반환합니다.

```js
const object = {};
delete object.name; // true
```

# Non-configurable property

Non-configurable properties는 삭제할 수 없습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-09-10-TheSecretsofthedeleteOperatorinJavaScript_2.png" />

비 엄격 모드에서 자신의 구성할 수 없는 속성을 제거하면 false를 반환하지만, 엄격 모드에서는 TypeError가 throw됩니다.

<img src="/assets/img/2024-09-10-TheSecretsofthedeleteOperatorinJavaScript_3.png" />

# var, let, const로 선언된 속성

<div class="content-ad"></div>

전역 범위에서 var로 선언된 속성이나 함수 선언문으로 선언된 함수(비함수 표현식)는 삭제할 수 없습니다. 왜냐하면 두 선언된 속성이 globalThis에 장착되어 있고 non-configurable이기 때문에 삭제할 때 이전 항목의 논리가 따를 것입니다.

![이미지](/assets/img/2024-09-10-TheSecretsofthedeleteOperatorinJavaScript_4.png)

또한, var, let, const로 선언된 속성과 해당 함수는 전역 범위나 함수 범위에서도 삭제할 수 없습니다.

```js
{
  let object = {
    name: 1,
  };

  function getName(obj) {
    return obj.name;
  }

  console.log(delete object); // false
  console.log(delete getName); // false
}
```

<div class="content-ad"></div>

비 엄격 모드에서는 false가 반환되지만, 엄격 모드에서는 TypeError가 아닌 SyntaxError가 발생합니다.

![image](/assets/img/2024-09-10-TheSecretsofthedeleteOperatorinJavaScript_5.png)

# 배열 속성

먼저 배열의 length 속성은 configurable하지 않으므로 엄격 모드에서는 삭제하면 TypeError가 발생합니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-09-10-TheSecretsofthedeleteOperatorinJavaScript_6.png" />

그리고 배열 요소를 삭제할 때 삭제된 항목은 비어 있을 것입니다.

<img src="/assets/img/2024-09-10-TheSecretsofthedeleteOperatorinJavaScript_7.png" />

# 결론

<div class="content-ad"></div>

JavaScript의 'delete 연산자'의 진정한 목적은 객체의 속성 참조를 삭제하는 것입니다. 특정 경우에 이상하게 동작할 수 있으므로 기본적으로 객체 자체에 존재하는 구성 가능한 속성을 제거하는 데만 사용하는 것이 좋습니다.

또한 객체에서 속성을 제거할 때, 이 속성의 값이 나머지 참조가 없는 객체인 경우, JavaScript 런타임은 해당 속성이 소유한 객체를 자동으로 해제할 것입니다. 프로그래밍 언어의 메모리 관리에 대한 더 많은 통찰을 얻으려면 이전 기사를 참조하십시오:

만약 이 정보가 도움이 되었다면, 웹 개발에 관한 더 유용한 기사와 도구를 더 많이 받아보기 위해 제 뉴스레터를 구독해 주시기 바랍니다. 읽어 주셔서 감사합니다!