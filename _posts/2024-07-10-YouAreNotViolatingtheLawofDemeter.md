---
title: "당신은 디미터 법칙을 위반하고 있지 않습니다"
description: ""
coverImage: "/assets/img/2024-07-10-YouAreNotViolatingtheLawofDemeter_0.png"
date: 2024-07-10 03:30
ogImage: 
  url: /assets/img/2024-07-10-YouAreNotViolatingtheLawofDemeter_0.png
tag: Tech
originalTitle: "You Are Not Violating the Law of Demeter"
link: "https://medium.com/code-like-a-girl/you-are-not-violating-the-law-of-demeter-cd48298aa02c"
---


최근 동료와 논의한 내용 중 하나가 제 코드가 데메테르 법칙을 위반한다고 말한 것이었습니다.

해당 코드는 Java Stream API를 단순히 활용한 것이었습니다.

소프트웨어 디자인에서 두 가지 기본적인 지표는 응집도(cohesion)와 결합도(coupling)입니다.

<div class="content-ad"></div>

로이 디미터 법칙은 결합과 관련이 있어요.

이 원칙을 간단하게 설명하자면 "메서드 호출을 연쇄시키지 말아야 한다" 에요.

일반적으로 다음과 같은 예시로 설명하곤 해요.

```js
dog.getBody().getTail().wag();
```

<div class="content-ad"></div>

"이것은 '기차 충돌' 코드로도 불리고 있습니다.

이 방식의 문제점은 우리가 고객 코드(이 방식으로 개를 사용하는 코드)를 개의 내부 구조에 결합하고 있다는 것입니다.

이 결합은 방향성이 있습니다. 즉, 우리의 고객 코드는 개의 구조에 결합되어 있습니다. 이는 개의 구조가 변경되면 고객 코드도 그에 맞게 수정해야 할 수도 있다는 의미입니다.

결합은 변경이 필요할 확률을 나타내는 강도를 가지고 있습니다."

<div class="content-ad"></div>

If the dog and its internal components have stable interfaces, such changes are less likely to occur.