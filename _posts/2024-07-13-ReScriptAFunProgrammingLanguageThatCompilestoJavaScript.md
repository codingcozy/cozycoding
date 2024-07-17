---
title: "ReScript 자바스크립트로 컴파일되는 재미있는 프로그래밍 언어"
description: ""
coverImage: "/assets/img/2024-07-13-ReScriptAFunProgrammingLanguageThatCompilestoJavaScript_0.png"
date: 2024-07-13 21:36
ogImage: 
  url: /assets/img/2024-07-13-ReScriptAFunProgrammingLanguageThatCompilestoJavaScript_0.png
tag: Tech
originalTitle: "ReScript: A Fun Programming Language That Compiles to JavaScript"
link: "https://medium.com/@louispetrik/rescript-a-fun-programming-language-that-compiles-to-javascript-5f9a3fc7a287"
---


![image](/assets/img/2024-07-13-ReScriptAFunProgrammingLanguageThatCompilestoJavaScript_0.png)

모든 프레임워크와 라이브러리를 지나, 새로운 트렌드가 등장했습니다: JavaScript로 변환되는 프로그래밍 언어를 구축하는 사람들이 나타났습니다. 이런 프로그래밍 언어 중 일부는 유일한 목적으로 이를 가지고 있습니다. 그 중 하나가 ReScript입니다.

이 언어는 JavaScript와 TypeScript의 대안이 되고자 하며, 전체 스택 어디에서나 둘 다 대체할 수 있습니다.

우리가 모든 기능을 살펴본다면 함께 고수할 거라고 믿지 않겠지요? 그래서, ReScript가 JavaScript와 TypeScript와 비교했을 때 어떤 점이 다른지에 초점을 맞추겠습니다.

<div class="content-ad"></div>

# 시작하기

새 프로젝트를 만드는 것은 쉽습니다: npm create rescript-app@latest. 템플릿으로 "Basic"을 선택하여 시작하세요.

저는 두 개의 터미널을 열 것을 추천합니다. 첫 번째 터미널에서는 npm run res:dev를 사용하여 ReScript 변환기를 시작합니다.

또 다른 터미널에서는 생성된 JavaScript 파일을 nodemon으로 실행합니다. 이렇게 하면 ReScript 코드를 변경할 때마다 생성된 JavaScript 코드가 실행됩니다: nodemon src/Demo.res.mjs. 또는 node src/Demo.res.mjs를 사용하여 생성된 JS 코드를 수동으로 다시 실행할 수도 있습니다.

<div class="content-ad"></div>

시작할 준비가 됐나요?

# Hello world

트랜스 파일러와 (혹은 없는 경우) nodemon이 실행 중이면, ReScript로 hello world를 작성할 수 있어요.

```js
let message = "Hello world"
```

<div class="content-ad"></div>

```js
Js.log(message)
```

Js.log을 사용하면 JavaScript의 console.log에 액세스할 수 있습니다. 일반적으로 ReScript에서는 JavaScript의 기본 객체들에 이 표기법을 통해 액세스할 수 있습니다. 예를 들어 Math 객체에도 적용됩니다:

```js
Js.log(Js.Math._PI) // 3.141...
```

그러나 현재 ReScript에는 Console.log()도 있어서 대신 사용할 수 있습니다. 둘 다 네이티브 console.log()로 컴파일됩니다.

<div class="content-ad"></div>

변수를 좀 더 살펴보는 재미있는 시간을 갖어봅시다. 우리는 인사 변수의 값을 변경하려고 합니다:

```js
let message = "Hello world"
message = "new message"
Js.log(message)
```

문제가 될 게 없겠죠? 실은 그렇지 않습니다.

ReScript에서는 let-binding은 불변입니다. 이는 이 변수의 값을 덮어쓸 수 없다는 것을 의미합니다.

<div class="content-ad"></div>

좀 더 구체적으로 좁혀보면, 변수 선언을 위한 let-binding이 유일하게 사용할 수 있는 것입니다. JavaScript에서처럼 "var" 또는 "const"가 없습니다. 좀 더 자세히 이야기해보아야 할 것 같아요.

ReScript의 불변성은 바닐라 JS와 TS와 비교했을 때 엄청난 차이점입니다.

# 불변성

지금까지는 변수의 값을 덮어쓸 수 없습니다. 할 수 있는 것은 전체 변수를 덮어쓰는 것 뿐입니다. 이것은 바람직한 방법이 아니며, 변수 값의 실제 변경 가능성에 대한 대안으로 의도되지 않았습니다.

<div class="content-ad"></div>

```js
let message = "Hello world"
let message = "New message"
```

```js
Js.log(message) // New message
```

그래서 변수 값을 덮어쓰는 대신 변수를 다시 선언하고 초기화했습니다. 일반 JavaScript에서도 같은 방법을 사용할 수 있습니다.

의도적으로 이러한 행동을 하면 안 되지만, 그래도 우리는 값을 변경할 수 없게 되었습니다. 하지만 이를 피하는 방법이 있습니다. 이번에는 나쁜 습관이 아닙니다.

<div class="content-ad"></div>

우리가 "ref"로 값에 감싸면, 하나의 속성과 그에 연결된 값이 있는 ReScript 객체가 생성됩니다. 이는 React의 useRef와 유사하게 작동합니다.

```js
let message = ref("Hello world")
message.contents = "New message"
Js.log(message.contents) // New message
```

객체.contents를 항상 작성하는 것은 귀찮은 일이기 때문에, 간략하게 작성할 수 있는 방법이 있습니다:

```js
message := "New message"
```

<div class="content-ad"></div>

# 패턴 매칭

ReScript에서 제가 가장 좋아하는 기능 중 하나에 대해 이야기해 봅시다: 패턴 매칭입니다.
이 기능을 통해 우리는 다양한 경우를 처리하는 매우 가독성 있는 방식으로 데이터 구조를 해체할 수 있습니다.

ReScript에서 패턴 매칭은 switch 문을 사용하여 수행됩니다.

여기서 우리가 튜플을 패턴 매칭하는 방법이 있습니다:

<div class="content-ad"></div>

```js
let coordinate = (1, 2)

let description = switch coordinate {
    | (0, 0) => "Origin"
    | (0, y) => "Y-axis"
    | (x, 0) => "X-axis"
    | (x, y) => "Quadrant"
}

Console.log(description) // Quadrant
```

위 | 연산자들은 각 경우를 나타내며, =`는 리턴문으로 사용됩니다.

# 목록

솔직할 때, 우리는 JavaScript의 배열에 대해 크게 생각하지 않습니다. 배열은 거의 모든 것이 가능합니다; 우리는 다른 데이터 유형에 사용할 수 있으며, 실제로 대안이 없습니다.

<div class="content-ad"></div>

ReScript에서 배열의 대체재로 리스트가 있습니다.

리스트는 ReScript에서 특별합니다. 내부적으로 우리는 곧 볼 것처럼 연결 리스트입니다.

간단한 리스트를 만들어서 결과적인 JavaScript 코드를 살펴보겠습니다.

```js
let names = list{"Max", "Tom", "John"}
```

<div class="content-ad"></div>

그럼 다음 JavaScript 코드로 변환됩니다:

```js
var names = {
  hd: "Max",
  tl: {
    hd: "Tom",
    tl: {
      hd: "John",
      tl: 0
    }
  }
};
```

리스트의 각 요소는 머리와 꼬리 두 부분으로 구성됩니다. 그 후 꼬리에는 다른 모든 요소가 포함됩니다. 시각적으로 이 데이터 구조가 연결 리스트라고 불리는 이유를 쉽게 알 수 있을 거에요.

하지만 어째서 우리가 그것을 사용할까요? ReScript의 리스트는 이러한 이유 때문에 있죠:

<div class="content-ad"></div>

- 변경 불가능
- 목록 끝에 추가 및 삭제가 빠름
- 목록의 시작에 추가하거나 삭제할 때 빠름
- 연결 작업이 빠름

반면에 목록은 다음과 같은 경우에 느립니다:

- 인덱스 n의 요소에 액세스
- 인덱스 n에서 추가하거나 삭제할 때 느림
- 재귀 구조 때문에 메모리 효율이 나쁘게 작용하는 대량 데이터 처리 시

그렇기 때문에 목록은 스택과 같은 작업에 이상적이며, 목록의 시작점에 요소를 추가하거나 제거할 때 적합합니다. 목록은 무작위 요소에 액세스하고 무작위 인덱스에 추가 또는 제거하는 경우에는 적합하지 않습니다.

<div class="content-ad"></div>

마지막으로, 목록 작업 방법을 알아봅시다.

목록의 첫 번째 요소는 "head"이며 List.head() 함수를 사용하여 액세스할 수 있습니다:

```js
let names = list{"Max", "Tom", "John"}
let firstName = List.head(names)
Js.log(firstName) // Max
```

반면에 tail 함수 List.tail()은 head 요소를 제외한 모든 값을 반환합니다.

<div class="content-ad"></div>

add 함수를 사용하면 새 요소를 추가할 수 있습니다. 추가된 요소는 기존 요소들 중 첫 번째로 위치하게 됩니다. 리스트 자체는 변경 불가능하기 때문에 변경되지 않습니다.

```js
let fourPeople = List.add(names, "Mara")

console.log(fourPeople) 
// { hd: 'Mara', tl: { hd: 'Max', tl: { hd: 'Tom', tl: [Object] } } }

console.log(names) 
// { hd: 'Max', tl: { hd: 'Tom', tl: { hd: 'John', tl: 0 } } }
```

# 배열

다행히도, 리스트 외에도 ReScript는 배열을 제공합니다. 이들은 실제로 JavaScript 배열을 기반으로 합니다.

<div class="content-ad"></div>

배열을 만들고 요소에 접근하며 값 업데이트는 JavaScript와 동일합니다:

```js
let names = ["Max", "Tom", "John"]
```

```js
Js.log(names[0])
```

그러나 push 및 기타 함수를 사용하려면 ReScript가 제공하는 JS 인터페이스를 사용해야 합니다:

<div class="content-ad"></div>

```js
let names = ["Max", "Carl", "Tom"]
```

```js
let pushedElement = Js.Array2.push(names, "Mara")
Js.log(names) // [ 'Max', 'Carl', 'Tom', 'Mara' ]
```

배열에서 push 함수를 사용하는 경우에는 실제로 데이터를 변경하는 것에 유의하세요.

## 배열과 함께 하는 패턴 매칭

<div class="content-ad"></div>

지난 섹션에서 배운 내용을 결합해 봅시다. 배열은 패턴 매칭과 아주 잘 어울립니다:

```js
let names = ["Max", "Tom", "John"]

let arraySize = switch names {
| [] => "빈 리스트"
| [x] => "1 요소"
| [x, y] => "2 요소"
| [x, y, z] => "3 요소"
| _ => "3개 이상 요소"
}

Console.log(arraySize)
```

물론 리스트도 패턴 매칭과 함께 사용할 수 있습니다.

# 결론

<div class="content-ad"></div>

물론 이것들은 ReScript가 제공하는 기능 중 일부에 불과해요. 나에게는 이 중에서 가장 재미있는 것들이에요. 왜냐하면 이것들은 일반 JavaScript와 다르기 때문이거든. 한편, JavaScript 코드로 변환하는 언어는 몇 가지 있지만, 그 중에서도 그냥 다른 언어 기능을 제공하지 않는 것들이 있어요.