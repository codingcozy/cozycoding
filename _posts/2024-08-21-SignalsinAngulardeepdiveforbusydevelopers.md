---
title: "Angular signal에 대해 알아보기"
description: ""
coverImage: "/assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_0.png"
date: 2024-08-21 17:35
ogImage: 
  url: /assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_0.png
tag: Tech
originalTitle: "Signals in Angular deep dive for busy developers"
link: "https://medium.com/@maxkoretskyi/signals-in-angular-deep-dive-for-busy-developers-627264eb4c2a"
isUpdated: true
updatedAt: 1724245380774
---


사용자 인터페이스를 구축하는 일은 어려운 작업입니다. 현대의 웹 애플리케이션에서 UI 상태는 간단한 독립적인 값으로는 구성되지 않는 경우가 대부분입니다. 대신, 다른 값이나 연산된 상태의 복잡한 계층에 의존하는 복잡한 계산된 상태입니다. 그 상태를 관리하는 데에는 많은 작업이 필요합니다: 개발자는 그 값을 저장하고, 연산하고, 무효화하고, 동기화해야 합니다.

여러 해 동안 다양한 프레임워크와 기본 요소가 도입되어 웹 개발을 간소화하기 위해 도입되었습니다. 대부분의 경우 중심 주제는 반응형 프로그래밍으로, 응용 프로그램 상태를 관리하는 인프라를 제공하여 개발자가 비즈니스 논리에 집중할 수 있도록 합니다. 반복적인 상태 관리 작업보다 비즈니스 로직에 집중할 수 있게 합니다.

가장 최근에 추가된 것은 시그널(signal)로, 동적으로 변경되는 값을 표현하고 해당 값을 사용하는 소비자들이 그 값이 변경될 때 알림을 받을 수 있는 "반응형" 기본 요소입니다. 그런 소비자들은 재계산을 실행하거나 다양한 부수효과(예: 구성 요소 생성/소멸, 네트워크 요청 실행, DOM 업데이트 등)를 실행할 수 있습니다.

<div class="content-ad"></div>

다양한 프레임워크에서 신호(signal)의 다양한 구현체를 찾을 수 있어요. 현재는 신호를 표준화하려는 노력조차 있어요:

Angular에서의 신호 구현은 제안의 일부로 제공된 구현과 매우 닮았기 때문에, 이 글에서 두 가지를 상호 참조할 수도 있을 거에요.

## 기본으로서의 신호

신호는 시간이 지남에 따라 변경될 수 있는 데이터 셀을 나타내요. 신호는 "상태"(수동으로 설정된 값) 또는 "계산된"(다른 신호를 기반으로 한 공식처럼 생각해보세요) 것 중 하나일 수 있어요.

<div class="content-ad"></div>

계산된 신호 함수는 자동으로 해당 평가 중에 읽히는 다른 신호를 추적합니다. 계산된 신호가 읽힐 때, 이전에 기록된 종속성 중 어느 것이 변경되었는지 확인하고, 변경되었다면 자신을 다시평가합니다.

예를 들어, 여기에 상태 신호 counter와 계산된 신호 isEven이 있습니다. counter의 초기 값은 0으로 설정하고 나중에 1로 변경합니다. 계산된 신호 isEven이 counter 신호 업데이트 전후에 서로 다른 값 두 개를 생성하여 변경에 반응하는 모습을 확인할 수 있습니다:

```js
import { computed, signal } from '@angular/core';

// 상태/쓰기 가능한 신호
const counter = signal(0);

// 계산된 신호
const isEven = computed(() => (counter() & 1) == 0);

counter() // 0
isEven() // true

counter.set(1)

counter() // 1
isEven() // false
```

또한 위 예시에서 isEven 신호가 명시적으로 소스 counter 신호를 구독하지 않음에 유의하세요. 대신, 계산된 함수 내부에서 counter()를 사용하여 소스 신호를 호출합니다. 이것만으로도 두 신호를 연결하는 데 충분합니다. 결과적으로, 카운터 소스 신호가 새 값으로 업데이트될 때, 파생된 신호도 자동으로 업데이트됩니다.

<div class="content-ad"></div>

상태 및 계산된 시그널 모두 값의 생성자로 간주됩니다. 생성자들은 값을 생성하고 변경 통지를 전달할 수 있는 시그널을 나타냅니다.

상태 시그널은 API 호출을 통해 값이 업데이트될 때 값이 변경(생성)됩니다. 반면, 계산된 시그널은 콜백에서 사용된 종속성이 변경될 때 자동으로 새 값을 생성합니다.

계산된 시그널은 종종 소비자가 될 수도 있습니다. 왜냐하면 일부 생성자에 의존할 수도 있기 때문입니다. 다른 반응형 구현에서는 Rx 같은 소비자를 싱크라고도 합니다.

생산자 시그널의 값이 변경되면 종속적인 소비자들, 예를 들어 계산된 시그널,의 값이 즉시 업데이트되지는 않습니다. 계산된 시그널이 읽힐 때, 이전에 기록된 종속성 중 어느 것이 변경되었는지 확인하고 필요한 경우 다시 평가합니다.

<div class="content-ad"></div>

이는 계산된 신호를 게으르게 만들어, 접근할 때에만 평가된다는 것을 의미합니다. 즉, 기반 상태가 이전에 변경되었더라도 액세스될 때에만 평가됩니다. 위의 예에서는 computed 신호 값이 isEven()을 호출할 때에만 평가되며, 기저 종속성 카운터의 업데이트가 counter.set()을 실행할 때에 이루어졌습니다.

일반 쓰기 및 computed 신호 외에도 감시자(효과)의 개념이 있습니다. 계산된 신호의 pull 기반 평가와 대조적으로 생성자 신호를 변경하면 즉시 감시자에게 알림이 전달되어, 감시자의 알림 콜백이 동기적으로 호출됩니다. 효과들은 사용자에 노출되어 사용될 수 있도록 감시자를 래핑하는 프레임워크로서 작동합니다. 효과는 사용자 코드의 노티피케이션을 스케줄링을 통해 지연시킬 수 있습니다.

Promises와 달리, 신호 내의 모든 것은 동기적으로 실행됩니다:

- 새 값으로 신호를 설정하는 것은 동기적으로 이루어지며, 이것은 이후에 이 값을 종속하는 모든 computed 신호를 읽을 때 즉시 반영됩니다. 이러한 변형에 대한 내장된 일괄 처리가 없습니다.
- computed 신호를 읽는 것은 동기적으로 이루어집니다. 그들의 값은 항상 사용 가능합니다.
- 감시자는 동기적으로 알림을 받지만, 해당 감시자를 래핑하는 효과는 스케줄링을 통해 알림을 일괄 처리하거나 지연할 수 있습니다.

<div class="content-ad"></div>

# 구현 세부 정보

내부적으로, signal의 구현은 설명하고 싶은 여러 개념을 정의합니다: 반응적 컨텍스트, 의존성 그래프 및 효과 (watchers). 반응적 컨텍스트부터 시작해봅시다.

반응적 컨텍스트를 논의하기 위해, JavaScript 코드가 평가되고 실행되는 환경을 정의하는 스택 프레임 (실행 프레임)을 생각해보세요. 특히, 함수에 사용 가능한 객체 (변수)를 정의합니다. 이 객체들의 가용성이 컨텍스트를 정의한다고 말할 수 있습니다. 예를 들어, 웹 워커 컨텍스트에서 실행되는 함수는 document 전역 객체에 엑세스할 수 없습니다.

반응적 컨텍스트는 생산자에 의존하는 활성 소비자 객체를 정의하고, 소비자가 자신의 엑세서 함수에서 값을 읽을 때 언제든지 사용할 수 있습니다. 예를 들어, 여기에서는 isEvent라는 소비자가 있으며, 이 소비자는 counter 생산자에 의존하며(값을 소비), 계산된 콜백 내에서 counter의 값을 액세스하여 해당 의존성을 정의합니다.

<div class="content-ad"></div>

```js
isEvent = computed(() => (counter() & 1) === 0)
```

계산된 콜백이 실행될 때, 카운터 시그널의 엑서서 함수가 자동으로 실행될 것입니다. 이 경우에 카운터 시그널은 isEvent 소비자의 반응적 컨텍스트에서 실행되고 있다고 말할 수 있습니다. 따라서, 생산자는 실행을 내포하고 있는 반응적 컨텍스트에서 실행되고 있습니다. 그래서 만약 이 생산자의 값을 의존하는 활성 소비자가 있다면, 생산자는 반응적 컨텍스트에서 실행됩니다.

이 반응적 컨텍스트 메카니즘을 구현하기 위해서는 소비자의 값이 액세스되는 모든 시점에, 다시 계산되기 전에 (계산된 콜백이 실행되기 전), 해당 소비자를 활성 소비자로 설정할 수 있습니다. 이것은 단순히 그 소비자 객체를 전역 변수에 할당하고, 콜백이 실행되는 동안에 그곳에 유지시키면 됩니다. 이 전역 변수는 계산된 콜백 실행 중에 조회된 모든 생산자에 대해 이 소비자가 의존하는 모든 생산자들에 대한 반응적 컨텍스트를 정의할 것입니다.

이것이 바로 Angular가 하는 일입니다. 계산된 콜백이 실행될 때, 먼저 producerRecomputeValue에서 현재 노드를 활성 소비자로 설정할 것입니다:

<div class="content-ad"></div>

```js
function producerRecomputeValue(node: ComputedNode<unknown>): void {
  ...
  const prevConsumer = consumerBeforeComputation(node);
  let newValue: unknown;
  try {
    newValue = node.computation();
  } catch (err) {...} finally {...}
}

function consumerBeforeComputation(node: ReactiveNode | null) {
  node && (node.nextProducerIndex = 0);
  return setActiveConsumer(node);
}
```

Angular gets there from the `producerUpdateValueVersion` inside the `createComputed` factory function:

```js
function createComputed<T>(computation: () => T): ComputedGetter<T> {
  ...
  const computed = () => {
    producerUpdateValueVersion(node);
    ...
  };
}

function producerUpdateValueVersion(node: ReactiveNode): void {
  ...
  node.producerRecomputeValue(node);
  ...
}
```

This callstack also clearly demonstrates this implementation:


<div class="content-ad"></div>


![image](/assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_1.png)

그래서 계산된 콜백이 실행되는 동안, 활성 상태인 해당 소비자의 시간 동안 질의된 모든 생성자는 반응적인 컨텍스트에서 실행됨을 알 수 있습니다. 특정 소비자의 반응적 컨텍스트에서 실행되는 모든 생성자는 소비자의 종속성으로 추가됩니다. 이것이 반응적 그래프를 형성합니다.

Angular의 대부분의 기존 기능은 비반응적 컨텍스트에서 실행됩니다. setActiveConsumer의 null 값 사용을 찾아보기만 하면 쉽게 확인할 수 있습니다:

![image](/assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_2.png)


<div class="content-ad"></div>

예를 들어, Angular는 라이프사이클 후크를 실행하기 전에 반응적인 컨텍스트를 지웁니다.

```js
/**
 * 수명주기 후크를 실행하며 다음을 보장합니다:
 * - 반응성이 없는 컨텍스트에서 호출됨.
 * - 프로파일링 데이터가 등록됨.
 */
function callHookInternal(directive: any, hook: () => void) {
  profiler(ProfilerEvent.LifecycleHookStart, directive, hook);
  const prevConsumer = setActiveConsumer(null);
  try {
    hook.call(directive);
  } finally {
    setActiveConsumer(prevConsumer);
    profiler(ProfilerEvent.LifecycleHookEnd, directive, hook);
  }
}
```

Angular 템플릿 함수 (컴포넌트 뷰)와 이펙트는 반응적인 컨텍스트에서 실행됩니다.

## 반응형 그래프

<div class="content-ad"></div>

리액티브 그래프는 소비자와 생산자 간의 의존성을 통해 구축됩니다. 값을 엑세서를 통한 리액티브 컨텍스트 구현은 시그널 의존성이 자동적으로 추적되고 암묵적으로 처리될 수 있게 합니다. 사용자들은 의존성 배열을 선언할 필요가 없으며 특정 컨텍스트의 의존성 집합이 실행 간에 정적으로 유지될 필요도 없습니다.

생산자가 실행될 때, 현재 활성화된 소비자의 (현재 리액티브 컨텍스트를 정의하는 소비자) 의존성 목록에 자신을 추가합니다. 이는 producerAccessed 함수 내에서 발생합니다:

```js
export function producerAccessed(node: ReactiveNode): void {
  ...
  // 이 생산자는 `idx`번째 의존성입니다.
    const idx = activeConsumer.nextProducerIndex++;
    if (activeConsumer.producerNode[idx] !== node) {
      // 우리는 소비자의 새로운 의존성입니다 (`idx` 위치).
      activeConsumer.producerNode[idx] = node;
      // 활성화된 소비자가 활성 상태인 경우, 해당 노드를 활성 소비자로 추가합니다. 아닌 경우, 값으로 0을 사용합니다.
      activeConsumer.producerIndexOfThis[idx] = consumerIsLive(activeConsumer)
        ? producerAddLiveConsumer(node, activeConsumer, idx)
        : 0;
    }
```

생산자와 소비자 둘 다 리액티브 그래프에 참여합니다. 이 의존성 그래프는 양방향이지만, 각 방향에서 추적되는 의존성에는 차이가 있습니다.

<div class="content-ad"></div>

생산자는 소비자의 종속성으로 추적되며, producerNode 속성을 통해 소비자에서 생산자로 엣지를 생성합니다.

```js
interface ConsumerNode extends ReactiveNode {
  producerNode: NonNullable<ReactiveNode['producerNode']>;
  producerIndexOfThis: NonNullable<ReactiveNode['producerIndexOfThis']>;
  producerLastReadVersion: NonNullable<ReactiveNode['producerLastReadVersion']>;
```

특정 소비자는 "라이브" 소비자로도 추적되며, 반대 방향으로 엣지를 생성하여 생산자에서 소비자로 정보 전파에 사용됩니다. 이 엣지는 생산자의 값이 업데이트될 때 변경 통지를 전파하는 데 사용됩니다.

```js
interface ProducerNode extends ReactiveNode {
  liveConsumerNode: NonNullable<ReactiveNode['liveConsumerNode']>;
  liveConsumerIndexOfThis: NonNullable<ReactiveNode['liveConsumerIndexOfThis']>;
}
```

<div class="content-ad"></div>

소비자들은 항상 의존하는 생산자를 추적합니다. 생산자는 "live"로 간주되는 소비자로부터의 종속성만 추적합니다. 소비자는 consumerIsAlwaysLive 속성이 true로 설정되어 있는 경우나 "live"로 간주되는 소비자에 의해 의존되는 생산자일 때 "live"입니다.

Angular에서 두 가지 유형의 노드가 "live" 소비자로 정의됩니다:

- watch 노드(효과에 사용됨)
- 반응형 LView 노드(변경 감지에 사용됨)

다음은 그들의 정의입니다:

<div class="content-ad"></div>

```js
const WATCH_NODE: Partial<WatchNode> = /* @__PURE__ */ (() => {
  return {
    ...REACTIVE_NODE,
    consumerIsAlwaysLive: true,
    consumerAllowSignalWrites: false,
    consumerMarkedDirty: (node: WatchNode) => {
      if (node.schedule !== null) {
        node.schedule(node.ref);
      }
    },
    hasRun: false,
    cleanupFn: NOOP_CLEANUP_FN,
  };
})();

const REACTIVE_LVIEW_CONSUMER_NODE: Omit<ReactiveLViewConsumer, 'lView'> = {
  ...REACTIVE_NODE,
  consumerIsAlwaysLive: true,
  consumerMarkedDirty: (node: ReactiveLViewConsumer) => {
    markAncestorsForTraversal(node.lView!);
  },
  consumerOnSignalRead(this: ReactiveLViewConsumer): void {
    this.lView![REACTIVE_TEMPLATE_CONSUMER] = this;
  },
};
```

어떤 상황에서는 계산된 신호가 "실시간" 소비자가 될 수 있습니다. 예를 들어, 효과 콜백에서 사용될 때입니다.

다음 코드 설치:

```js
import { ChangeDetectorRef, Component, computed, effect, signal } from '@angular/core';
import { SIGNAL } from '@angular/core/primitives/signals';

@Component({
  standalone: true,
  selector: 'app-root',
  template: 'Angular Love',
  styles: []
})
export class AppComponent {
  constructor(private cdRef: ChangeDetectorRef) {
    const a = signal(0);

    const b = computed(() => a() + 'b');
    const c = computed(() => a() + 'c');
    const d = computed(() => b() + c() + 'd');

    const nodes = [a[SIGNAL], b[SIGNAL], c[SIGNAL], d[SIGNAL]] as any[];

    d();

    const A = 0, B = 1, C = 2, D = 3;

    const depBToA = nodes[B].producerNode[0] === nodes[A];
    const depCToA = nodes[C].producerNode[0] === nodes[A];
    const depDToB = nodes[D].producerNode[0] === nodes[B];
    const depDToC = nodes[D].producerNode[1] === nodes[C];

    console.log(depBToA, depCToA, depDToB, depDToC);

    const e = effect(() => b()) as any;

    // 변경 감지가 효과를 알릴 때까지 기다려야 함
    setTimeout(() => {
      // 효과는 B에 의존함
      const depEToB = e.watcher[SIGNAL].producerNode[0] === nodes[B];

      // 생방송 소비자는 제작자 A에서 B로 링크돼 있으며,
      // B에서 E로 연결되어 있으므로 E (효과)가 생방송 소비자임
      const depLiveAToB = nodes[A].liveConsumerNode[0] === nodes[B];
      const depLiveBToE = nodes[B].liveConsumerNode[0] === e.watcher[SIGNAL];

      console.log(depLiveAToB, depLiveBToE, depEToB);
    });
  }
}
```

<div class="content-ad"></div>

아래 표 형식을 Markdown 형식으로 변경하면 다음 그래프가 생성됩니다:

![SignalsinAngulardeepdiveforbusydevelopers_3](/assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_3.png)

액티브 컨슈머를 통한 반응적 컨텍스트의 구현은 동적 의존성 추적을 가능케 합니다. 특정 컨슈머가 활성화된 경우, 평가 중인 프로듀서는 해당 프로듀서 호출의 순서를 통해 동적으로 정의됩니다. 의존성 목록은 활성 컨슈머가 반응적 컨텍스트에서 이 소비자에게 접근될 때마다 재구성될 수 있습니다.

이를 구현하기 위해, 소비자의 의존성은 producerNode 배열에서 추적됩니다:

<div class="content-ad"></div>

```js
인터페이스 ConsumerNode는 ReactiveNode를 상속받습니다.
  producerNode: NonNullable<ReactiveNode['producerNode']>;
  producerIndexOfThis: NonNullable<ReactiveNode['producerIndexOfThis']>;
  producerLastReadVersion: NonNullable<ReactiveNode['producerLastReadVersion']>;
```

특정 소비자에 대한 계산이 다시 실행될 때, 해당 배열에 대한 포인터(인덱스) `producerIndexOfThis`가 0으로 초기화되고, 각 종속성 읽기는 이전 실행에서 포인터의 현재 위치에 있는 종속성과 비교됩니다. 일치하지 않을 경우, 종속성은 마지막 실행 이후 변경되었으므로 이전 종속성이 버려지고 새로운 것으로 대체될 수 있습니다. 실행이 끝나면 남은 일치하지 않는 종속성을 버릴 수 있습니다.

즉, 한 가지 분기에 필요한 종속성이 있고 이전 계산이 다른 분기를 취했을 때, 그 일시적으로 사용되지 않은 값이 변경되어도 계산된 신호가 다시 계산되지 않습니다. 이로 인해 한 실행에서 다음 실행으로 이어지는 다른 신호 집합에 액세스할 수 있는 가능성이 있습니다.

예를 들어, 다음 `computed signal`은 useA 신호의 값에 따라 dataA 또는 dataB를 동적으로 읽습니다:


<div class="content-ad"></div>

```js
const dynamic = computed(() => useA() ? dataA() : dataB());
```

어떤 시점에서든지, [useA, dataA] 또는 [useA, dataB]의 종속성 세트를 가지며, dataA와 dataB에 동시에 종속적일 수는 없습니다.

이 코드는 Angular에서의 이 테스트 케이스와 유사합니다. 또한 이 코드는 다음을 명확히 보여줍니다:

```js
import { computed, signal } from '@angular/core';
import { SIGNAL } from '@angular/core/primitives/signals';

const states = Array.from('abcdefgh').map((s) => signal(s));
const sources = signal(states);

const vComputed = computed(() => {
  let str = '';
  for (const state of sources()) str += state();
  return str;
});

const n = vComputed[SIGNAL] as any;
expectEqual(vComputed(), 'abcdefgh');
expectEqualArrayElements(n.producerNode.slice(1), states.map(s => s[SIGNAL]));

sources.set(states.slice(0, 5));
expectEqual(vComputed(), 'abcde');
expectEqualArrayElements(n.producerNode.slice(1), states.slice(0, 5).map(s => s[SIGNAL]));

sources.set(states.slice(3));
expectEqual(vComputed(), 'defgh');
expectEqualArrayElements(n.producerNode.slice(1), states.slice(3).map(s => s[SIGNAL]));

function expectEqual(v1, v2): any {
  if (v1 !== v2) throw new Error(`Expected ${v1} to equal ${v2}`);
}
function expectEqualArrayElements(v1, v2): any {
  if (v1.length !== v2.length) throw new Error(`Expected ${v1} to equal ${v2}`);
  for (let i = 0; i < v1.length; i++) {
    if (v1[i] !== v2[i]) throw new Error(`Expected ${v1} to equal ${v2}`);
  }
}
```

<div class="content-ad"></div>

위에서 볼 수 있듯이, 그래프에는 단일 시작 정점이 없습니다. 각 소비자는 종속성 생산자 목록을 유지하며, 이는 다시 종속성을 갖을 수 있습니다. 예를 들어, 계산된 시그널이 있습니다. 따라서 각 소비자는 접근 시점에 그래프의 루트 정점이라고 할 수 있습니다.

## 두 단계 업데이트

반응성을 위한 이전 푸시 기반 모델은 중복된 계산 문제에 직면했습니다: 상태 시그널에 대한 업데이트로 인해 계산된 시그널이 열심히 실행될 경우, 이는 궁극적으로 UI에 업데이트를 푸시할 수 있습니다. 그러나 다음 프레임 이전에 원본 상태 시그널에 또 다른 변경이 있을 경우 이 UI 쓰기는 조기일 수 있습니다.

예를 들어, 이러한 그래프의 경우, 이 문제는 무심코 A-` B-` D를 평가하고 C를 다시평가하고, C가 변경되었기 때문에 D를 다시평가하는 것을 포함합니다. D를 두 번 평가하는 것은 비효율적이며 사용자에게 눈에 띄는 장애물을 초래할 수 있습니다.

<div class="content-ad"></div>

![image](/assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_4.png)

이건 다이아몬드 문제로 알려져 있어요.

가끔 이런 버그로 인해 정확하지 않은 중간 값이 최종 사용자에게 표시되기도 했어요. 시그널은 푸시 방식이 아닌 풀 방식(레이지)으로 동작하여 이러한 동적을 해결합니다. UI 렌더링을 예약하는 시점에 프레임워크가 적절한 업데이트를 풀해와서, 계산에 낭비를 줄이고 DOM에 쓰기 작업을 피합니다.

다음 예시를 살펴보세요:

<div class="content-ad"></div>

```js
const a = signal(0);

const b = computed(() => a() + 'b');
const c = computed(() => a() + 'c');
const d = computed(() => b() + c() + 'd');

// run the computed callback to set up dependencies
d();

// update the signal at the top of the graph
setTimeout(() => a.set(1), 2000);
```

a가 업데이트되면 전파가 발생하지 않습니다. 노드의 값과 버전만 업데이트됩니다:

```js
function signalSetFn(node, newValue) {
  ...
  if (!node.equal(node.value, newValue)) {
    node.value = newValue;
    signalValueChanged(node);
  }
}

function signalValueChanged(node) {
  node.version++;
  ...
}
```

나중에 d()의 값을 액세스할 때, 신호 구현은 d를 통해 consumerPollProducersForChange로 종속성을 상위로 폴링하여 다시 계산이 필요한지 결정합니다.

<div class="content-ad"></div>

효율적인 처리를 위해 모든 반응 노드는 종속성 노드의 버전을 기록합니다. 변경 사항을 결정하려면 생산 노드의 저장된 버전을 노드의 실제 버전과 간단히 비교하면 충분합니다:

```js
interface ConsumerNode extends ReactiveNode {
  ...
  producerLastReadVersion: NonNullable<ReactiveNode['producerLastReadVersion']>;
}

function consumerPollProducersForChange(node) {
  ...
  // 변경 사항을 알아보기 위해 프로듀서를 폴링합니다.
  for (let i = 0; i < node.producerNode.length; i++) {
    const producer = node.producerNode[i];
    const seenVersion = node.producerLastReadVersion[i];
    // 먼저 버전을 확인합니다. 불일치하면 프로듀서의 값이 마지막으로 읽은 이후에 변경된 것으로 파악됩니다.
    if (seenVersion !== producer.version) {
      return true;
    }
```

만약 두 값이 다르다면, 프로듀서에 변경 사항이 있어 프로듀서의 값이 알려진 내용을 통해 새롭게 계산된 콜백을 실행할 것입니다:

```js
export function producerUpdateValueVersion(node: ReactiveNode): void {
  ...

  if (!node.producerMustRecompute(node) && !consumerPollProducersForChange(node)) {
    // 마지막으로 읽은 후 변경 사항을 보고한 프로듀서가 없으므로 값의 다시 계산이 필요하지 않으며 자체를 정리(clean)한 것으로 간주할 수 있습니다.
    node.dirty = false;
    node.lastCleanEpoch = epoch;
    return;
  }

  node.producerRecomputeValue(node);

  // 값을 다시 계산한 후에는 더는 변경되지 않습니다.
  node.dirty = false;
  node.lastCleanEpoch = epoch;
}
```

<div class="content-ad"></div>

C의 종속성을 재귀적으로 처리하는 과정이 됩니다. 이렇게 함으로써 노드 A에 도달하며, 이 시점에서 D-`C-`A 브랜치의 평가가 이뤄집니다. 그러나 D도 B 프로듀서에 의존하므로 D를 계산하기 전에 해당 값을 다시평가합니다. 이렇게 하면 D에 대해 중복 계산 문제가 없어집니다.

가끔씩, 특정 소비자에게 즉시 알릴 필요가 있는 경우가 있습니다. 아마도 그겪이 "실시간" 소비자들이라고 알려져 있을 것입니다. 이 경우, 변경 사항 알림은 프로듀서 값이 업데이트되면 즉시 그래프를 통해 전파되어 프로듀서에 의존하는 실시간 소비자들에게 알립니다.

이러한 일부 소비자는 파생 값일 수 있고, 따라서 프로듀서가 되기도 하며, 이들의 캐시된 값이 무효화되고 이를 통해 변경 사항 알림이 자신의 실시간 소비자에게 계속 전파됩니다. 이러한 과정을 통해 최종적으로 변경 사항 알림이 효과에 도달하고, 해당 효과는 자체를 재실행하도록 예약합니다.

이 단계에서 주요한 점은 부작용이 실행되지 않으며, 중간값이나 파생값의 재계산이 이뤄지지 않다는 것입니다. 오직 캐시된 값들의 무효화만이 일어납니다. 이를 통해 변경 사항 알림이 그래프의 모든 영향을 받는 노드에 도달하고 중간값이나 일시적이거나 혼란스러운 상태를 관찰할 가능성이 없게 됩니다.

<div class="content-ad"></div>

필요한 경우에는 이 변경 전파가 완료된 후 (동기적으로) 위에서 살펴본 지연 평가가 이어질 수 있습니다.

이 알림 단계가 실제로 어떻게 작동하는지 보려면 우리 설정에 실시간 소비자, 예를 들어 감시자를 추가해 보겠습니다. a가 업데이트되면 해당 업데이트가 종속된 실시간 소비자에게 전파됩니다:

```js
import { computed, signal } from '@angular/core';
import { createWatch } from '@angular/core/primitives/signals';

const a = signal(0);
const b = computed(() => a() + 'b');
const c = computed(() => a() + 'c');
const d = computed(() => b() + c() + 'd');

setTimeout(() => a.set(1), 3000);

// 감시자는 `d`에 의존성을 설정합니다
const watcher = createWatch(
  () => console.log(d()),
  () => setTimeout(watcher.run, 1000),
  false
);

watcher.notify();
```

a.set(1) 값을 업데이트하자마자 실시간 소비자의 알림을 확인할 수 있습니다.

<div class="content-ad"></div>


![Signals in Angular deep dive for busy developers](/assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_5.png)

노드 b와 c는 노드 a의 라이브 소비자이므로, Angular는 a를 업데이트할 때 node.liveConsumerNode를 확인하여 해당 노드들에 변경 사항을 알립니다.

하지만 앞서 언급했듯이, 여기서는 실제로 아무 일도 일어나지 않습니다. 노드가 단순히 dirty로 표시되고, producerNotifyConsumers를 통해 라이브 소비자에게 통지를 전파합니다:

```js
function consumerMarkDirty(node) {
  node.dirty = true;
  producerNotifyConsumers(node);
  node.consumerMarkedDirty?.(node);
}
```


<div class="content-ad"></div>

이 모든 것은 d에 의존하는 watcher(효과)까지 이어집니다. 일반적인 반응적 노드와는 달리 watch 노드는 consumerMarkedDirty 메서드에서 스케줄링을 구현합니다:

```js
const WATCH_NODE: Partial<WatchNode> = (() => {
  return {
    ...REACTIVE_NODE,
    consumerIsAlwaysLive: true,
    consumerAllowSignalWrites: false,
    consumerMarkedDirty: (node: WatchNode) => {
      if (node.schedule !== null) {
        node.schedule(node.ref);
      }
    },
    hasRun: false,
    cleanupFn: NOOP_CLEANUP_FN,
  };
})();
```

여기서 알림 단계와 그래프 순회가 중지됩니다.

이 두 단계로 구성된 프로세스는 때때로 "푸시/풀" 알고리즘으로 참조됩니다: 소스 신호가 변경될 때 "더러움"은 즉시 그래프를 통해 푸시되지만 값이 신호를 읽어서 끌어올 때만 게으르게 재계산이 수행됩니다.

<div class="content-ad"></div>

## 변경 감지

시그널 기반 알림을 변경 감지 프로세스에 통합하기 위해 Angular는 라이브 소비자 메커니즘을 활용합니다. 구성 요소 템플릿은 템플릿 표현식(JS 코드)으로 컴파일되어 해당 구성 요소 뷰의 반응형 컨텍스트에서 실행됩니다. 이러한 컨텍스트에서 시그널을 실행하면 값이 반환되지만 해당 구성 요소 뷰의 종속성으로서 시그널이 등록됩니다.

템플릿 표현식이 실시간 소비자이기 때문에 Angular는 프로듀서에서 템플릿 표현식 노드로 링크를 생성합니다. 프로듀서의 값이 업데이트되면 즉시 동기적으로 해당 프로듀서는 템플릿 노드에 알림을 보내게 됩니다. 알림을 받으면 Angular는 해당 구성 요소와 모든 조상을 확인하기 위해 표시합니다.

제 다른 글에서 이미 아시겠지만, 각 구성 요소의 템플릿은 내부적으로 LView 객체로 표현됩니다. 다음은 구성 요소를 위해 어떻게 보이는지에 대한 내용입니다:

<div class="content-ad"></div>

```js
@Component({...})
export class AppComponent {
  value = signal(0);
}
```

컴파일하면 이 컴포넌트의 변경 감지 중에 실행되는 일반적인 JS 함수인 AppComponent_Template과 같아집니다:

```js
this.ɵcmp = defineComponent({
  type: AppComponent,
  ...
  template: function AppComponent_Template(rf, ctx) {
    if (rf & 1) {
      ɵɵtext(0);
    }
    if (rf & 2) {
      ɵɵtextInterpolate1("", ctx.value(), "\n");
    }
  },
});
```

앵귤러가 변경 감지 구현에 시그널을 추가했을 때 모든 컴포넌트 뷰(템플릿 함수)를 ReactiveLViewConsumer 노드로 감쌌습니다:

<div class="content-ad"></div>

```ts
export interface ReactiveLViewConsumer extends ReactiveNode {
  lView: LView | null;
}
```

이 인터페이스는 REACTIVE_LVIEW_CONSUMER_NODE 노드에 의해 구현됩니다:

```ts
const REACTIVE_LVIEW_CONSUMER_NODE: Omit<ReactiveLViewConsumer, 'lView'> = {
  ...REACTIVE_NODE,
  consumerIsAlwaysLive: true,
  consumerMarkedDirty: (node: ReactiveLViewConsumer) => {
    markAncestorsForTraversal(node.lView!);
  },
  consumerOnSignalRead(this: ReactiveLViewConsumer): void {
    this.lView![REACTIVE_TEMPLATE_CONSUMER] = this;
  },
};
```

이 프로세스를 각 뷰가 자체 ReactiveLViewConsumer 소비자 노드를 받음으로써 생각할 수 있습니다. 템플릿 함수 내에서 액세스된 모든 시그널에 대한 반응적 컨텍스트를 정의합니다.

<div class="content-ad"></div>

우리의 경우에는 템플릿 함수가 변경 감지의 일부로 실행될 때, 템플릿 함수 노드의 컨텍스트에서 ctx.value() 생성자가 실행됩니다. 이는 ActiveConsumer인 템플릿 함수 노드의 컨텍스트에서 실행됩니다:

![image1](/assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_6.png)

결과적으로 이는 템플릿 표현식 노드 (소비자)가 생성자 value()에 대한 라이브 종속성으로 추가되는 것을 의미합니다:

![image2](/assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_7.png)

<div class="content-ad"></div>

이 종속성은 생산자 카운터의 값이 변경됨에 따라 즉시 소비자 노드(템플릿 표현식)에 알림을 보장합니다.

라이브 소비자는 값을 변경할 때 생산자에 의해 동기적으로 호출되는 consumerMarkDirty 메서드를 구현합니다:

```js
/**
 * 이 생산자의 라이브 소비자에게 더러운 알림을 전파합니다.
 */
function producerNotifyConsumers(node: ReactiveNode): void {
  ...
  try {
    for (const consumer of node.liveConsumerNode) {
      if (!consumer.dirty) {
        consumerMarkDirty(consumer);
      }
    }
  } finally {
    inNotificationPhase = prev;
  }
}

function consumerMarkDirty(node: ReactiveNode): void {
  node.dirty = true;
  producerNotifyConsumers(node);
  node.consumerMarkedDirty?.(node);
}
```

consumerMarkedDirty 내부에서 템플릿 표현식 노드는 markForCheck()가 이전에 수행한 방식과 유사하게 조상을 새로 고칠 수 있도록 markAncestorsForTraversal을 사용합니다:

<div class="content-ad"></div>

```js
const REACTIVE_LVIEW_CONSUMER_NODE: Omit<ReactiveLViewConsumer, 'lView'> = {
  ...
  consumerMarkedDirty: (node: ReactiveLViewConsumer) => {
    markAncestorsForTraversal(node.lView!);
  },
};

function markAncestorsForTraversal(lView: LView) {
  let parent = getLViewParent(lView);
  while (parent !== null) {
    ...
    parent[FLAGS] |= LViewFlags.HasChildViewsToRefresh;
    parent = getLViewParent(parent);
  }
}
```

마지막 질문은 Angular가 언제 현재 LView 소비자 노드를 ActiveConsumer로 설정하는지입니다. 이 모든 과정은 이미 저의 이전 게시물에서 알고 있을지도 모르는 refreshView 함수 내에서 발생합니다.

이 함수는 각 LView에 대해 변경 감지를 실행하고 일반적인 변경 감지 작업을 실행합니다: 템플릿 함수 실행, 후크 실행, 쿼리 새로 고침 및 호스트 바인딩 설정. 기본적으로 반응성을 처리하는 전체 코드 조각이 Angular이 이러한 작업을 실행하기 전에 추가되었습니다.

이렇게 보입니다:

<div class="content-ad"></div>

```js
function refreshView<T>(tView, lView, templateFn, context) {
  ...

  // 컴포넌트 반응적 컨텍스트 시작
  enterView(lView);
  let returnConsumerToPool = true;
  let prevConsumer: ReactiveNode | null = null;
  let currentConsumer: ReactiveLViewConsumer | null = null;
  if (!isInCheckNoChangesPass) {
    if (viewShouldHaveReactiveConsumer(tView)) {
      currentConsumer = getOrBorrowReactiveLViewConsumer(lView);
      prevConsumer = consumerBeforeComputation(currentConsumer);
    } else {... }

    ...

    try {
      ...
      if (templateFn !== null) {
        executeTemplate(tView, lView, templateFn, RenderFlags.Update, context);
      }
  }
```

이 코드는 executeTemplate 코드 내에서 Angular이 컴포넌트 템플릿 함수를 실행하기 전에 실행되므로 컴포넌트 템템플릿에서 사용되는 시그널의 접근 함수가 실행될 때 이미 반응적 컨텍스트가 설정되어 있습니다.

## 하이브리드 변경 감지

v18에서 Angular는 하이브리드 변경 감지 메커니즘으로 전환하여 시그널이 값을 변경할 때 비존중적인 변경 전파를 활성화합니다. 이 동작을 구현하는 부분은 markAncestorsForTraversal 함수 호출에 추가됩니다.

<div class="content-ad"></div>

```js
export function markAncestorsForTraversal(lView: LView) {
  lView[ENVIRONMENT].changeDetectionScheduler?.notify(
      NotificationSource.MarkAncestorsForTraversal
  );
  
  let parent = getLViewParent(lView);
  while (parent !== null) { ... }
}
```

changeDetectionScheduler 서비스는 notify 메서드를 구현하며, 변경 감지 실행 또는 tick을 마이크로/매크로 태스크 스케줄러를 통해 스케줄합니다:

```js
notify(source: NotificationSource): void {
    ...
    const scheduleCallback = this.useMicrotaskScheduler
      ? scheduleCallbackWithMicrotask
      : scheduleCallbackWithRafRace;

    this.pendingRenderTaskId = this.taskService.add();
    if (this.zoneIsDefined) {
      Zone.root.run(() => {
        this.cancelScheduledCallback = scheduleCallback(() => {
          this.tick(this.shouldRefreshViews);
        });
      });
    } else {
      this.cancelScheduledCallback = scheduleCallback(() => {
        this.tick(this.shouldRefreshViews);
        });
    }
  }
```

## 영향과 감시자

<div class="content-ad"></div>

효과는 어플리케이션 상태에 따라 부수 효과를 수행하는 전문 도구입니다. 효과는 콜백 함수로 정의된 실시간 소비자로, 리액티브 컨텍스트에서 실행됩니다. 이 함수의 신호 의존성이 캡처되고, 의존성 중 하나가 새 값을 생성할 때 효과가 통지됩니다.

대부분의 어플리케이션 코드에서는 효과가 거의 필요하지 않지만 일부 특정 상황에 유용할 수 있습니다. Angular 문서에서 제안하는 몇 가지 사용 예시는 다음과 같습니다:

- 데이터 로그 기록 또는 창 로컬스토리지와 동기화 유지
- 템플릿 구문으로 표현할 수 없는 사용자 지정 DOM 동작 추가, 예를 들어 `canvas` 요소에 사용자 정의 렌더링 수행

Angular는 변경 감지 메커니즘에서 효과를 사용하지 않아 컴포넌트 UI 업데이트를 트리거하지 않습니다. 변경 감지에 관한 섹션에서 설명한대로, 이 기능을 위해서는 실시간 소비자 메커니즘에 의존합니다.

<div class="content-ad"></div>

시그널 알고리즘은 표준화되어 있지만, 효과가 어떻게 동작해야 하는지에 대한 세부사항은 정의되어 있지 않고 프레임워크마다 다를 수 있습니다. 이는 효과 스케줄링의 세밀한 성격 때문인데, 이는 종종 프레임워크 렌더링 주기와 JavaScript가 액세스할 수 없는 고수준 프레임워크별 상태나 전략과 통합됩니다.

그러나 시그널 제안은 프레임워크 작성자가 자신의 효과를 만들기 위해 사용할 수 있는 일련의 기본 요소, 즉 워치 API를 정의합니다. 워처 인터페이스는 반응형 함수를 감시하고 해당 함수의 종속성이 변경될 때 알림을 수신하는 데 사용됩니다.

Angular에서는 효과가 워처 위에 래퍼입니다. 먼저 워처가 어떻게 작동하는지 살펴보고, 그들이 어떻게 효과 기본 요소를 구축하는 데 사용되는지 알아보겠습니다.

먼저, Angular 프리미티브에서 워처를 가져와서 알림 메커니즘을 구현해보겠습니다:

<div class="content-ad"></div>

```js
import { createWatch } from '@angular/core/primitives/signals';

const counter = signal(0);

const watcher = createWatch(
  // 사용자가 제공한 콜백을 실행하고 추적을 설정합니다. 
  // 이것은 `watcher.notify()` 후에 2회 실행됩니다. 
  // 1회는 `watcher.notify()` 후에 실행되고, 2회는 `this.counter.set(1)` 후에 실행됩니다.
  () => counter(),
  // 이것은 `notify` 메소드에 의해 호출됩니다. 
  // 또는 사용자 자신이 `consumerMarkDirty` 메소드를 통해 직접 호출할 수 있습니다. 
  // 사용자가 제공한 콜백을 1000밀리초 후에 실행하도록 예약합니다.
  () => setTimeout(watcher.run, 1000),
  false
);

// watcher를 dirty(유효하지 않은) 상태로 표시하여 사용자가 제공한 콜백을 
// 실행하도록 강제하고 `counter` 신호의 추적을 설정합니다.
// `notify` 메소드는 내부적으로 `consumerMarkDirty`를 호출합니다.
watcher.notify();

// 값이 변경되면 consumerMarkDirty가 실행됩니다. 
// 이는 사용자가 제공한 콜백을 실행하도록 예약합니다.
setTimeout(() => this.counter.set(1), 3000);
```

watcher.notify()를 실행하면, Angular은 watcher 노드에서 동기적으로 consumerMarkDirty 메소드를 호출합니다. 그러나 사용자 정의 알림 콜백은 알림을 받은 즉시 실행되지 않습니다. 대신에 미래의 어느 시점에 watcher.run을 통해 실행되도록 예약됩니다. watch는 "markDirty" 알림을 받으면 이 예약 작업을 단순히 호출할 뿐입니다.

여기서 실제로 실행되는 것을 확인할 수 있습니다:

<img src="/assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_8.png" />


<div class="content-ad"></div>

우리가 this.counter.set(1)를 실행할 때, 같은 호출 체인은 사용자가 제공한 콜백의 예약을 이끌어냅니다.

effect() 함수를 빌드하기 위해, Angular는 watcher를 EffectHandle 클래스 내부에 래핑합니다:

```js
export function effect(effectFn,options): EffectRef {
  const handle = new EffectHandle();
  ...
  return handle;
}

class EffectHandle implements EffectRef, SchedulableEffect {
  unregisterOnDestroy: (() => void) | undefined;
  readonly watcher: Watch;

  constructor(...) {
    this.watcher = createWatch(
      (onCleanup) => this.runEffect(onCleanup),
      () => this.schedule(),
      allowSignalWrites,
    );
    this.unregisterOnDestroy = destroyRef?.onDestroy(() => this.destroy());
  }
```

EffectHandle 클래스가 watcher가 설정되는 곳임을 볼 수 있습니다. 이전에 watcher를 사용한 위의 예제에서, effect 함수를 사용하면 설정 절차가 크게 간소화됩니다:

<div class="content-ad"></div>

```js
import { Component, effect, signal } from '@angular/core';

@Component({...})
export class AppComponent {
  counter = null;

  constructor() {
    this.counter = signal(0);

    // 이 부분은 2번 실행됩니다
    effect(() => this.counter());

    setTimeout(() => this.counter.set(1), 3000);
  }
}
```

effect 함수를 직접 사용할 때는 하나의 콜백만 전달합니다. 이것은 사용자 정의 콜백으로 의존성을 설정하고 Angular이 의존성이 업데이트될 때 실행할 것으로 예약합니다.

Angular 효과에서 현재 사용되는 스케줄러는 ZoneAwareEffectScheduler이며, 변경 감지 주기 후에 마이크로태스크 큐의 일부로 업데이트를 실행합니다:

```js
export class ZoneAwareEffectScheduler implements EffectScheduler {
  private queuedEffectCount = 0;
  private queues = new Map<Zone | null, Set<SchedulableEffect>>();
  private readonly pendingTasks = inject(PendingTasks);
  private taskId: number | null = null;

  scheduleEffect(handle: SchedulableEffect): void {
      this.enqueue(handle);
      if (this.taskId === null) {
        const taskId = (this.taskId = this.pendingTasks.add());
        queueMicrotask(() => {
          this.flush();
          this.pendingTasks.remove(taskId);
          this.taskId = null;
        });
      }
    }
```

<div class="content-ad"></div>

Angular에서 구현해야 하는 재미있는 특징이 하나 있어요. Watcher를 사용한 구현에서 본 것처럼, 이 효과를 "초기화"하기 위해 수동으로 watcher.notify()를 한 번 호출해야 합니다. Angular도 마찬가지로 이 작업을 수행해야 하며, 변경 감지의 첫 실행 과정 중 일부로 이를 수행합니다.

구현 방법은 다음과 같습니다.

컴포넌트의 주입 컨텍스트 내에서 효과 함수를 실행할 때, Angular는 컴포넌트의 뷰 개체 LView[EFFECTS_TO_SCHEDULE]에 알림 콜백을 추가합니다 :

```js
export function effect(
  effectFn: (onCleanup: EffectCleanupRegisterFn) => void,
  options?: CreateEffectOptions,
): EffectRef {
  ...
  const handle = new EffectHandle();

  // 효과를 초기 실행하려면 수동으로 효과를 dirty로 표시해야 합니다. 이 표시의 시기가 중요합니다. 왜냐하면 효과가 구성 요소 입력을 추적하는 신호를 읽을 수 있습니다. 그 신호는 해당 구성 요소가 첫 번째 업데이트 패스를 마친 후에만 사용할 수 있기 때문입니다.
  ...
  const cdr = injector.get(ChangeDetectorRef, null, {optional: true}) as ViewRef<unknown> | null;
  if (!cdr || !(cdr._lView[FLAGS] & LViewFlags.FirstLViewPass)) {
    // 이 효과는 뷰 주입기에서 실행되지 않거나, 뷰가 이미 처음의 변경 감지 패스를 거친 경우입니다. 이는 필요한 입력이 설정되기 위해 필요합니다.
    handle.watcher.notify();
  } else {
    // 뷰가 완전히 초기화될 때까지 효과의 초기화를 지연합니다.
    (cdr._lView[EFFECTS_TO_SCHEDULE] ??= []).push(handle.watcher.notify);
  }

  return handle;
}
```

<div class="content-ad"></div>

이렇게 추가된 알림 기능은 refreshView 함수 내에서 이 구성 요소의 뷰에 대한 첫 번째 변경 감지 실행 중에 한 번 실행됩니다.

```js
export function refreshView<T>(tView, lView, templateFn, context) {
   ...
  
   // 이 뷰에 대한 업데이트 전달을 기다리는 효과를 예약합니다.
    if (lView[EFFECTS_TO_SCHEDULE]) {
      for (const notifyEffect of lView[EFFECTS_TO_SCHEDULE]) {
        notifyEffect();
      }

      // 실행된 후, 배열을 삭제할 수 있습니다.
      lView[EFFECTS_TO_SCHEDULE] = null;
    }
}
```

notifyEffect를 호출하면 하위 감시자의 consumerMarkDirty 알림 콜백이 트리거되어 (변경 감지 후에) 기존 스케줄러를 사용하여 사용자가 제공한 콜백이 실행되도록 효과를 예약합니다:

![이미지](/assets/img/2024-08-21-SignalsinAngulardeepdiveforbusydevelopers_9.png)

<div class="content-ad"></div>

전체 이야기는 그것입니다 :)