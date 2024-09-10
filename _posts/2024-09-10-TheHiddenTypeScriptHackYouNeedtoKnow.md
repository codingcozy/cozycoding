---
title: "알아두면 유용한 숨은 TypeScript 팁"
description: ""
coverImage: "/assets/img/2024-09-10-TheHiddenTypeScriptHackYouNeedtoKnow_0.png"
date: 2024-09-10 18:19
ogImage: 
  url: /assets/img/2024-09-10-TheHiddenTypeScriptHackYouNeedtoKnow_0.png
tag: Tech
originalTitle: "The Hidden TypeScript Hack You Need to Know"
link: "https://medium.com/gitconnected/the-hidden-typescript-hack-you-need-to-know-6c6bc48250b5"
isUpdated: false
---


<img src="/assets/img/2024-09-10-TheHiddenTypeScriptHackYouNeedtoKnow_0.png" />

안녕하세요 TypeScript 열정가들 여러분.

오늘은 긴 소개로 여러분의 시간을 낭비하지 않을 거예요, 바로 시작해볼게요!

# 1. 속임수

<div class="content-ad"></div>

어떤 시간이 지나 TypeScript를 사용해 왔다면, 아마도 union 타입을 마주쳤을 것입니다.

Union 타입은 다음과 같은 형태를 띕니다:

```js
type Fruit = "apple" | "orange"
```

이제 Fruit 타입으로 변수를 주석 처리하면, 해당 변수에는 사과나 오렌지만 할당할 수 있습니다.

<div class="content-ad"></div>

```js
type Fruit = "apple" | "orange";

// Can only be "apple" or "orange"
const fruit: Fruit = "apple";
```

이건 멋지지만, 바나나, 베리, 토마토 같은 다른 과일들이 있어요.

그리고 댓글에서 이것에 대해서 논쟁하지 말아 주세요. 토마토는 채소가 아니라 과일입니다.

당연히 매번 새로운 과일에 대해 Fruit 타입을 확장하는 것은 현실적이지 않습니다. 그래서 다른 해결책이 필요합니다.

<div class="content-ad"></div>

당신은 아마 'Fruit' 유형에 문자열을 합치는 것이 유혹적일 것입니다.

```js
type Fruit = "apple" | "orange" | string
```

이렇게 하면 사과, 오렌지 및 기타 모든 과일이 포함된 것으로 예상할 수 있습니다.

하지만 그렇지 않습니다.

<div class="content-ad"></div>

이 코드는 사실 과일 유형을 문자열로 평가하므로 기존 과일에 대한 유형 안전성을 잃게 됩니다!

```js
// 원하는 바가 아니지만, 이것이 발생하는 일입니다.
type Fruit = string
```

그래서 이 문제를 어떻게 해결할까요? 알아보는 걸로 해요.

# 2. 트릭 사용하기

<div class="content-ad"></div>

가능한 한 이 기교를 명확하게 만들기 위해 맥락을 설명하는 예제를 사용해봅시다.

이 예제에서는 애플리케이션에 로그인할 때 사용할 수 있는 인증 제공자를 나타내는 문자열 인수를 받는 함수를 정의합니다.

예: "google"은 Google 로그인을 나타냅니다.

```js
type Provider = "google" | "github"

async function authenticate(provider: Provider) {
  await signIn(provider)
}

// "google" 또는 "github"만 허용됩니다.
await authenticate("google")
```

<div class="content-ad"></div>

코드에 따르면 "google" 또는 "github"만 입력할 수 있지만 이 함수를 typesafety하게 확장하여 google 및 github를 수락하는 동시에 resend나 facebook과 같은 추가 공급업체를 수용해야 한다면 어떻게 해야 할까요?

이미 | string을 추가하는 것이 올바른 접근 방식이 아니라는 것에 대해 이야기했으니, 올바른 방법은 무엇일까요?

좀 어색할 수 있지만 자세히 살펴보겠습니다.

다음은 이 문제를 해결하기 위한 코드입니다:

<div class="content-ad"></div>

```js
타입 Provider = "google" | "github" | (string & {})
```

우리가 유니언 스트링 대신에 유니언 스트링 & '' 을 추가하는 이유는 무엇일까요?

타입스크립트의 타입 체커는 일반 문자열 타입과 리터럴 문자열 타입("apple"과 같은)을 구분하는 데 약하기 때문입니다.

반면에, string & ''은 null 또는 undefined 값이 아닌 모든 가능한 문자열을 나타내는 타입으로, 그래서 '' 타입과 교차하기 때문입니다.

<div class="content-ad"></div>

그 말은 문자열 & ''이 문자열보다 엄격한 버전이라는 것이며, 따라서 기존 리터럴 문자열의 형식 안전성을 보존하지만 추가 문자열도 받아 들이는 방식으로 "catch-all" 유형과 비슷합니다.

이 트릭을 적용한 업데이트된 authenticate 함수를 살펴보세요:

```js
// 새로운 "(string & {}) 유형은 catch-all과 같이 작동합니다"
// 형식 안전성을 보존합니다!
type Provider = "google" | "github" | (string & {})

async function authenticate(provider: Provider) {
  await signIn(provider)
}

// "google"과 "github"에 대한 형식 안전성은 유지되며, 그 외 어떤 문자열이든 환영합니다.
// 예를 들어 "resend"를 여기에 입력하면 작동하며 오류가 발생하지 않습니다!
await authenticate("resend")
```

# 결론

<div class="content-ad"></div>

이 기법은 새로운 인증 제공 업체를 점진적으로 웹 애플리케이션에 채택하고 각 번 type 정의를 업데이트하고 싶지 않을 때 매우 유용합니다.

물론 가능한 한 정확하게 type를 유지하는 것이 가장 유지 관리하기 좋은 방법이므로, 이 TypeScript 트릭을 철학적으로 사용해서는 안 됩니다. JavaScript 영역으로 천천히 거슬러 올라가게 될 것이기 때문입니다.

그러나 인증 제공 업체 예제와 같이 특정 경우에는 개발자 경험에 매우 유용할 수 있으므로 TypeScript에도 그 자리가 있습니다.