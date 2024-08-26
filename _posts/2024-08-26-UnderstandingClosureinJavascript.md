---
title: "자바스크립트에서 클로저(closure) 이해하기"
description: ""
coverImage: "/assets/img/2024-08-26-UnderstandingClosureinJavascript_0.png"
date: 2024-08-26 17:15
ogImage: 
  url: /assets/img/2024-08-26-UnderstandingClosureinJavascript_0.png
tag: Tech
originalTitle: "Understanding Closure in Javascript"
link: "https://medium.com/@singhpooja0704/closure-in-javascript-3c5d4404bfc4"
isUpdated: false
---



<img src="/assets/img/2024-08-26-UnderstandingClosureinJavascript_0.png" />

클로저는 하나 이상의 함수가 묶여 있는 조합입니다. 이는 함수가 외부 스코프에서 변수나 함수를 기억하고 액세스할 수 있는 능력을 가리킵니다. 심지어 외부 함수가 실행을 마친 후에도 그 변수나 함수에 액세스할 수 있습니다.

```js
function add() {
  const x = 5;

  function addY(element) {
    console.log(x + element);
  }

  return addY;
}

const myClosure = add();
myClosure(10);
myClosure(20);
```

# 설명


<div class="content-ad"></div>

- Outer Function: add은 변수 x와 addY를 정의합니다.
- Inner Function: addY는 외부 범위에 있는 x에 접근합니다.
- Closure: add를 호출하면 addY를 반환합니다. add 함수의 실행이 완료되어도 addY는 여전히 x에 액세스할 수 있습니다. 이는 addY가 클로저를 형성하여 항상 x에 액세스할 수 있도록 "close over"하기 때문입니다.

함수가 외부 범위 변수에 액세스 권한을 유지하는 능력은 JavaScript에서 클로저를 강력한 기능으로 만드는 것입니다.

JavaScript에서 클로저가 내부적으로 어떻게 작동하는지 더 자세히 살펴보겠습니다:

## 1. 렉시컬 환경

<div class="content-ad"></div>

JavaScript에서 함수가 생성되면 해당 함수는 자신의 렉시컬 환경을 참조합니다. 렉시컬 환경이란 함수가 선언된 컨텍스트를 의미합니다. 이 컨텍스트에는 아래와 같은 내용이 포함됩니다:

- 함수가 생성된 시점의 유효 범위 내에 있는 모든 변수들.
- 외부 함수의 변수와 함수에 대한 참조.

## 2. 함수 생성

다른 함수 내부에 함수를 정의할 때, JavaScript는 그냥 내부 함수를 독립적으로 생성하지 않습니다. 대신 함수와 생성된 환경을 결합한 클로저를 만듭니다.

<div class="content-ad"></div>

```js
function outerFunction() {
  let outerVariable = 'Outer';

  function innerFunction() {
    console.log(outerVariable);
  }

  return innerFunction;
}
```

위 코드에서:

- innerFunction이 생성될 때, outerFunction에서의 outerVariable에 대한 참조를 가지고 있습니다.

## 3. 실행 후에도 참조 유지하기

<div class="content-ad"></div>

outerFunction이 호출되고 실행이 완료되면 outerVariable이 사라질 것으로 예상할 수 있습니다. 왜냐하면 outerFunction이 끝났기 때문입니다. 그러나 innerFunction이 반환되고 outerFunction 외부에서 사용되기 때문에 outerVariable에 대한 참조가 유지됩니다.

이것은 클로저가 작동하는 방식 때문입니다. JavaScript 엔진은 외부 함수의 실행이 완료될 때 렉시컬 환경을 그냥 버리지 않습니다. 만약 여전히 그 환경에 접근해야 하는 함수(예: innerFunction)가 있다면, 엔진은 해당 환경을 유지합니다.

## 4. 가비지 콜렉션 및 메모리 관리

보통 함수 내부의 변수는 함수의 실행이 끝나면 정리되거나 ("가비지 컬렉션"됨)합니다. 그러나 클로저의 경우, JavaScript 엔진은 해당 변수가 여전히 내부 함수에 의해 필요하다고 인식합니다. 내부 함수가 존재하고 호출할 수 있는 한, 해당 변수가 있는 환경은 가비지 컬렉트되지 않습니다.

<div class="content-ad"></div>

## 예시를 통해 실제 동작을 살펴봅시다:

```js
function counter() {
  let count = 0;

  return function() {
    count += 1;
    return count;
  };
}

const increment = counter(); // `counter`가 호출되지만 그 환경은 계속 유지됩니다.
console.log(increment()); // 출력: 1
console.log(increment()); // 출력: 2
console.log(increment()); // 출력: 3
```

다음은 단계별로 발생하는 일들입니다:

- counter 호출: counter()가 호출될 때 새로운 렉시컬 환경이 생성됩니다. 이 환경에는 0으로 설정된 변수 count가 포함됩니다.
- 내부 함수 반환: 내부 함수가 반환되며, count가 존재하는 렉시컬 환경에 대한 참조를 유지합니다.
- increment 호출: increment()를 호출할 때마다, 처음에 counter()에 의해 생성된 count 변수에 접근합니다. counter()의 실행이 끝났지만 클로저 덕분에 count 변수는 여전히 사용 가능합니다.

<div class="content-ad"></div>

# 클로저의 장점

- 데이터 캡슐화: 클로저를 사용하면 함수 외부에서 직접 액세스할 수 없는 비공개 변수를 생성할 수 있습니다. 이는 데이터 숨김에 유용합니다.
- 함수 팩토리: 클로저를 사용하여 특정 동작을 하는 다른 함수를 생성하는 팩토리 함수를 만들 수 있습니다.
- 상태 유지: 클로저를 사용하여 전역 변수에 의존하지 않고 여러 함수 호출 간에 상태를 유지할 수 있습니다.
- 콜백 및 이벤트 핸들러: 클로저는 콜백 및 이벤트 핸들러에서 맥락을 유지하는 데 자주 사용됩니다.

# 행동 요청

JavaScript의 클로저에 관한 질문이나 통찰이 있으신가요? 아래 댓글로 공유해주세요! 더 많은 JavaScript 튜토리얼과 기사를 보려면 박수를 치고 팔로우하세요!