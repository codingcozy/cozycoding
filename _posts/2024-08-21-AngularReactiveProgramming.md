---
title: "Angular에서 반응형 웹사이트 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-AngularReactiveProgramming_0.png"
date: 2024-08-21 17:13
ogImage: 
  url: /assets/img/2024-08-21-AngularReactiveProgramming_0.png
tag: Tech
originalTitle: "Angular Reactive Programming"
link: "https://medium.com/@adam.ondrejkovic/angular-reactive-programming-f01116647516"
isUpdated: true
updatedAt: 1724246082424
---


요즘 웹 개발 환경에서는 반응성과 효율성을 모두 갖춘 애플리케이션을 구축하는 것이 매우 중요합니다. 선두적인 프런트엔드 프레임워크인 Angular은 상태 관리 및 사용자 상호작용을 반응적으로 처리하는 강력한 도구를 제공합니다. RxJS(JavaScript용 반응형 확장)와 Angular Signals 두 가지 도구는 반응형 프로그래밍에 대한 고유하면서도 상호 보완적인 접근 방식을 제공합니다. 이 글에서는 이러한 도구들이 어떻게 작동하며, 어떤 차이가 있는지, 그리고 이들을 결합하여 동적이고 고성능의 Angular 애플리케이션을 만드는 방법을 탐구합니다.

식당에서 주문하는 상황을 상상해보세요:

```js
let food = 🍔;
let beverage = 🧃;
let order = food + beverage;
console.log(order); // 출력: 🍔🧃
```

이제 주스에서 커피로 바꾸기로 결정했다고 가정해봅시다:

<div class="content-ad"></div>

```js
beverage = ☕️;
console.log(order); // 출력: 🍔🧃
```

주문이 새로운 음료에 반영되지 않는 것을 주목하셨나요? 이것은 주문이 초기 값에 기반하여 설정되어 있어 변경 사항이 자동으로 반영되지 않기 때문입니다. Angular에서 이러한 반응성 부족은 응용 프로그램이 사용자 상호 작용이나 데이터 변경에 동적으로 응답하지 않을 수 있다는 것을 의미합니다.

반응형 프로그래밍은 데이터 변경이 응용 프로그램 전체로 자동으로 전파되도록 보장하여 응답 및 효율성을 향상시킵니다.

![Angular Reactive Programming](/assets/img/2024-08-21-AngularReactiveProgramming_0.png)

<div class="content-ad"></div>

# Angular에서의 신호: 반응성 재정의

Angular 신호는 컴포넌트 내에서 반응적 상태를 관리하는 정교한 메커니즘을 소개합니다. 기존 변수와 달리, 신호는 값이 동적으로 변할 수 있도록 허용하고 의존 컴포넌트에서 자동으로 업데이트를 트리거할 수 있습니다. 이전 예제를 신호를 사용하여 다시 작성하는 방법은 다음과 같습니다:

```js
const food = signal(🍔);
const beverage = signal(🧃);

const order = computed(() => food() + beverage());

console.log(order); // 출력: 🍔🧃

beverage.set(☕️); 

console.log(order); // 출력: 🍔☕️
```

설명:

<div class="content-ad"></div>

- signal(🍔) 및 signal(🧃)는 반응 변수를 생성합니다.
- computed(() =` food() + beverage())를 사용하면 음식 또는 음료가 변경될 때 자동으로 업데이트되는 계산된 속성이 정의됩니다.
- beverage.set(☕️)을 사용하여 음료를 업데이트하면 주문이 즉시 이 변경을 반영합니다.

시그널(Signals)은 Angular 애플리케이션에서 반응성을 관리하는 더 효율적인 방법을 제공합니다. 이러한 방식은 변경 감지를 최적화하여 변경된 데이터에 종속된 컴포넌트만 다시 렌더링되도록 보장합니다. 이는 성능 오버헤드를 최소화하고 사용자 경험을 향상시킵니다.

![Angular Reactive Programming](/assets/img/2024-08-21-AngularReactiveProgramming_1.png)

# RxJS: 비동기 데이터 스트림 처리

<div class="content-ad"></div>

시그널은 Angular 구성 요소 내에서 반응적 상태를 관리하는 데 훌륭하지만, RxJS는 비동기 데이터 스트림 및 복잡한 이벤트 처리를 처리하는 데 탁월합니다. RxJS는 시간이 지남에 따라 발생하는 데이터 또는 이벤트 시퀀스를 나타내기 위해 옵저버블을 사용하여, 이러한 이벤트에 구독하고 반응할 수 있도록 합니다.

다음 비유를 생각해보세요: 주문을 처리하는 커피 머신을 상상해보세요:

- 소스: 커피 머신.
- 옵저버블: 커피 제조 과정 - 분쇄, 추출.
- 구독자: 커피를 기다리는 사람.

![이미지](/assets/img/2024-08-21-AngularReactiveProgramming_2.png)

<div class="content-ad"></div>

다음은 RxJS에서 시작 예제가 어떻게 보일 수 있는지에 대한 설명입니다:

```js
// 초기값으로 BehaviorSubjects를 초기화합니다
const food$ = new BehaviorSubject<string>(🍔);
const beverage$ = new BehaviorSubject<string>(🧃);

// food$와 beverage$에서 가장 최신 값을 결합합니다
const order$ = combineLatest([food$, beverage$]).pipe(
  map(([food, beverage]) => `${food}${beverage}`)
);

// order$에 구독하여 변경 사항을 로깅합니다
order$.subscribe(order => console.log(order)); // 출력: 🍔🧃

// 음료를 업데이트하고 order$의 변경 내용을 관찰합니다
beverage$.next(☕️); // 출력: 🍔☕️
```

설명:

- food$와 beverage$는 초기값과 업데이트를 관리하는 데 이상적인 BehaviorSubject 인스턴스입니다.
- combineLatest([food$, beverage$])는 최신 값을 하나의 Observable로 결합합니다.
- map 연산자는 결합된 값을 문자열로 형식화합니다.
- order$에 구독하여 업데이트를 로깅하면 beverage$의 변경 사항이 반영됩니다.

<div class="content-ad"></div>

이 방법은 동적 상태 변경을 처리하고 여러 Observable 소스를 결합하여 응용 프로그램이 실시간으로 업데이트에 반응하도록 하는 RxJS의 효과적인 사용 방법을 보여줍니다.

RxJS는 Angular 응용 프로그램에 불가결한 역할을 하며 다음과 같은 비동기 작업을 관리하기 위한 강력한 도구를 제공합니다.

- 라우팅: Observable을 사용하여 라우팅 상태의 변경에 반응하여 컴포넌트가 탐색 이벤트에 대응할 수 있습니다.
- 반응형 폼: RxJS를 통해 양식이 사용자 입력의 변경에 반응하여 실시간으로 유효성 검사 및 피드백을 제공할 수 있습니다.
- HTTP 통신: 백엔드 서버와 통신할 때 RxJS Observable을 사용하여 HTTP 요청과 응답을 처리하여 데이터가 도착하는 대로 응용 프로그램이 반응할 수 있습니다.

# 시그널과 RxJS 비교

<div class="content-ad"></div>

Angular에서 반응형 프로그래밍에 강력한 기능을 제공하는 Signals와 RxJS는 다른 목적을 가지고 있지만:

- Signals: 성능 오버헤드를 최소화하면서 컴포넌트 내에서 상태를 효과적으로 관리하기에 최적입니다. 빈번히 변경되는 데이터를 신속하게 UI에 반영해야 하는 경우 Signals을 사용하세요.
- RxJS: 비동기 데이터 스트림 및 복잡한 이벤트 처리를 다루기에 이상적입니다. RxJS는 HTTP 요청, 이벤트 스트림, 외부 서비스와의 통합과 같은 작업에 유용합니다.

# 결론

Angular에서 반응형 프로그래밍을 숙달하기 위해서는 Signals와 RxJS를 효과적으로 활용해야 합니다. Signals은 컴포넌트 내에서 상태 관리와 성능을 향상시키는 데 도움을 주며, RxJS는 비동기 데이터 및 복잡한 이벤트 처리를 관리하는 데 중요합니다. 이러한 도구들을 결합하면 Angular 애플리케이션을 더 반응적이고 효율적으로 만들 수 있습니다. Signals와 RxJS를 함께 사용하면 애플리케이션의 반응성과 효율성이 크게 향상될 것입니다.