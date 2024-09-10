---
title: "자바스크립트에서의 Yield 제너레이터의 힘을 발휘하기"
description: ""
coverImage: "/assets/img/2024-09-10-YieldinJavaScriptUnlockingthePowerofGenerators_0.png"
date: 2024-09-10 18:30
ogImage: 
  url: /assets/img/2024-09-10-YieldinJavaScriptUnlockingthePowerofGenerators_0.png
tag: Tech
originalTitle: "Yield in JavaScript Unlocking the Power of Generators"
link: "https://medium.com/frontend-simplified/yield-in-javascript-unlocking-the-power-of-generators-8bd0a3acbb7f"
isUpdated: false
---


<img src="/assets/img/2024-09-10-YieldinJavaScriptUnlockingthePowerofGenerators_0.png" />

Medium Premium이 없으신가요? 여기에서 읽어보세요: simplified.ninja.

JavaScript를 작성하는 상상을 해보세요. 실행 중간에 멈출 수 있고 제어권을 다시 넘겨주어 돌아갈 수 있으면 어떨까요? 마법 같죠? 그런 일들을 할 수 있는 곳이 바로 JavaScript의 yield와 generator 함수의 세계입니다.

# 소개

<div class="content-ad"></div>

JavaScript의 계속 변화하는 풍경 속에서, yield 키워드와 그 동반자인 생성자 함수가 가져온 호기심과 잠재력만큼 많은 관심을 끈 적이 없었습니다. ECMAScript 6(ES6)에서 소개된 이 강력한 구조물들은 JavaScript에서의 제어 흐름과 반복 가능성에 대한 생각을 바꿨습니다.

# Yield란?

본질적으로, yield는 생성자 함수 내에서만 사용되는 키워드입니다. 그렇다면 정확히 어떤 역할을 하는 걸까요?

- 일시 중단과 재개: yield 키워드는 함수가 실행을 일시 중단하고 호출자에게 값을 반환하되 자신의 상태를 유지하는 데 사용됩니다. 그리고 거기서 일시 중단된 곳부터 다시 시작할 수 있습니다.
- 값 생성: 생성자 함수가 값을 yield하면, 해당 값은 반복 시퀀스의 일부로 생성됩니다.
- 양방향 통신: yield는 생성자 함수에서 값을 보내고, 생성자가 재개될 때 값이 다시 돌아와서 받을 수 있습니다.

<div class="content-ad"></div>

여기 간단한 예제가 있어요:

```js
function* countToThree() {
    yield 1;
    yield 2;
    yield 3;
}
```

```js
const generator = countToThree();
console.log(generator.next().value); // 1
console.log(generator.next().value); // 2
console.log(generator.next().value); // 3
```

이 예제에서, 각 yield 문은 함수를 일시 중단하고 순서에서 다음 값을 반환해요. 함수는 next()가 다시 호출될 때 다시 실행돼요.

<div class="content-ad"></div>

yield를 이해하면 JavaScript 프로그래밍에서 새로운 패러다임이 열립니다. 복잡한 제어 흐름, 지연 평가, 무한 시퀀스와 관련된 문제에 우아한 해결책을 제공합니다. 다음 파트에서는 yield가 왜 강력한지, 효과적으로 사용하는 방법을 탐구하고 일부 실용적인 사용 사례에 대해 자세히 살펴볼 것입니다.

# 왜 Yield를 사용해야 하는가?

yield 키워드와 제너레이터 함수는 여러 가지 흥미로운 이점을 제공합니다:

- 지연 평가: 제너레이터는 필요한 때에 값들을 계산하므로 대량의 데이터 세트와 작업할 때 특히 더 나은 성능과 메모리 효율성을 제공할 수 있습니다.
- 간소화된 비동기 코드: 제너레이터는 비동기 코드를 더 읽기 쉽고 유지보수하기 쉽게 만들어줄 수 있습니다, 특히 프로미스와 결합했을 때 더욱 그렇습니다.
- 사용자 정의 순회 가능한 객체: 전체 반복자 프로토콜을 구현하는 복잡성 없이 사용자 정의 순회 가능한 객체를 쉽게 생성할 수 있습니다.
- 상태 보존: 제너레이터는 호출 사이에 내부 상태를 유지하기 때문에 복잡한 상태를 가진 알고리즘을 자연스럽게 표현할 수 있습니다.

<div class="content-ad"></div>

# Yield 사용 방법

Yield를 사용하려면 제너레이터 함수를 이해해야 합니다. 다음은 단계별 가이드입니다:

- 제너레이터 함수 정의: 함수 내부에 * 키워드를 사용하거나 객체 리터럴 또는 클래스의 메소드 앞에 *를 붙입니다.

```js
function* generatorFunction() {
  // 함수 본문
}
```

<div class="content-ad"></div>

- Yield 키워드 사용: 제너레이터 내부에서 실행을 일시 중지하고 값을 생성할 때는 yield를 사용하세요.

```js
function* countTo(n) {
  for (let i = 1; i <= n; i++) {
    yield i;
  }
}
```

- 제너레이터 객체 생성: 제너레이터 함수를 호출하여 제너레이터 객체를 생성하세요.

```js
const generator = countTo(3);
```

<div class="content-ad"></div>

- 반복: 생성된 값들을 반복하려면 next() 메소드를 사용하세요.

```js
console.log(generator.next().value); // 1
console.log(generator.next().value); // 2
console.log(generator.next().value); // 3
console.log(generator.next().done);  // true
```

# 사용 사례

- 이터러블 구현: 복잡한 데이터 구조에 대한 사용자 정의 이터러블을 만드는 데 사용하세요.

<div class="content-ad"></div>

```js
function* 피보나치수열() {
  let [이전, 현재] = [0, 1];
  while (true) {
    yield 현재;
    [이전, 현재] = [현재, 이전 + 현재];
  }
}
```

- 비동기 작업 처리: 비동기 코드를 간편하게 처리하고, 특히 async/await와 결합할 때 유용합니다.

```js
function* 사용자데이터가져오기() {
  const 사용자 = yield fetch('/api/user');
  const 게시물들 = yield fetch(`/api/posts?userId=${사용자.id}`);
  return { 사용자, 게시물들 };
}
```

- 고유한 ID 생성: 간단한 상태 저장형 ID 생성기 만들기.


<div class="content-ad"></div>

```js
function* idGenerator() {
  let id = 1;
  while (true) {
    yield `id_${id++}`;
  }
}
```

- Coroutines를 구현했습니다: 제너레이터를 사용하여 협력적으로 멀티태스킹을 구현합니다.

# 결론

yield 키워드와 제너레이터 함수는 JavaScript의 강력한 기능으로, 게으른 평가(lazy evaluation), 사용자 정의 이터러블(custom iterables), 복잡한 제어 흐름 시나리오에 대한 우아한 해결책을 제공합니다. 초보자에게는 문법이 기이하게 느껴질 수 있지만, yield를 마스터함으로써 효율적이고 가독성 있고 유지보수가 용이한 JavaScript 코드를 작성할 수 있는 능력을 크게 향상시킬 수 있습니다.


<div class="content-ad"></div>

yield를 탐험하면 프로젝트에 적용할 혁신적인 방법을 발견할 가능성이 높습니다. 성능을 최적화하거나 비동기 작업을 관리하거나 복잡한 알고리즘을 구현하는 등의 작업을 할 때 yield는 다양한 프로그래밍 과제를 해결하기 위한 유연하고 강력한 도구입니다.

기억하세요. yield와 같은 강력한 기능은 분별하여 사용해야 합니다. 특히 시퀀스, 반복 및 상태 관리와 관련된 시나리오에 특히 적합합니다. 제너레이터를 사용해 경험을 쌓으면 코드에서 가장 많은 이점을 제공할 때를 직감적으로 파악하게 될 것입니다.