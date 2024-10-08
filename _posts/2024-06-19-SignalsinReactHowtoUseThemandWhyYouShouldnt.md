---
title: "리액트에서의 시그널 사용 방법과 사용하지 말아야 하는 이유"
description: ""
coverImage: "/assets/img/2024-06-19-SignalsinReactHowtoUseThemandWhyYouShouldnt_0.png"
date: 2024-06-19 00:25
ogImage: 
  url: /assets/img/2024-06-19-SignalsinReactHowtoUseThemandWhyYouShouldnt_0.png
tag: Tech
originalTitle: "Signals in React: How to Use Them and Why You Shouldn’t"
link: "https://medium.com/@goldinevgeny/signals-in-react-how-to-use-them-and-why-you-shouldnt-645e9d15c6bc"
isUpdated: true
---





<img src="/assets/img/2024-06-19-SignalsinReactHowtoUseThemandWhyYouShouldnt_0.png" />

# 소개

시그널은 본질적으로 반응형 값 홀더로, 값이 변경될 때 해당 값에 종속된 계산 또는 효과를 자동으로 업데이트합니다. 이 방식은 SolidJS와 Angular와 같은 현대 웹 프레임워크에서 특히 유용하며, React의 상태(state) 및 훅(hooks)과 같은 기존 방법보다 상태(state) 및 반응성 관리를 더 효과적으로 지원합니다. 예를 들어, SolidJS에서 시그널을 생성하고 해당 값을 사용하여 업데이트를 트리거할 수 있습니다.

## ECMAScript에 시그널 추가를 위한 공식 제안

<div class="content-ad"></div>

ECMAScript 언어의 진화를 감독하는 TC39 위원회가 JavaScript에 새로운 기능으로 신호를 추가하기 위한 공식 제안을 하였습니다. 이 제안은 신호 개념을 표준화하여 언어 내에서 섬세한 반응성을 활성화하는 것을 목표로 합니다. 제안된 API에는 Signal.subtle.Watcher와 같은 여러 신호를 생성하고 관리하는 방법이 포함되어 있어, 개발자들이 신호의 변화를 감지하고 적절히 반응할 수 있도록 합니다.

제안에 대한 자세한 내용은 TC39 위원회가 관리하는 GitHub 리포지토리를 참조하세요.

# 신호의 장점

자동 의존성 추적: 신호는 자동 의존성 추적을 제공하며, 신호의 값이 변경될 때마다 해당 신호에 의존하는 반응형 계산 또는 효과가 자동으로 업데이트됩니다. 이는 수동으로 의존성을 관리하는 것을 제거하여 코드를 단순화하고 오류를 줄이는 것을 의미합니다.

<div class="content-ad"></div>

게으른 평가: 시그널은 필요할 때만 계산을 수행하여 성능을 향상시키는 게으른 평가를 지원합니다. 이를 통해 계산은 결과가 필요할 때에만 수행됩니다.

메모이제이션: React Signals와 마찬가지로 Solid.JS는 비용이 많이 드는 계산의 결과를 캐시함으로써 동일한 입력이 다시 발생할 때 이를 재사용하여 성능을 향상시킵니다.

그래, 이것이 잘 정립된 반응성의 메커니즘입니다. 그런데 UI에서는 어떻게 작용할까요? Solid.JS에서는 이것이 어떻게 작동하는지 살펴봅시다.

SolidJS에서 JSX 컴포넌트는 시그널 값이 변경될 때마다 DOM 노드를 생성하고 업데이트하는 효과로 컴파일됩니다. 다음은 예시 컴포넌트와 그 컴파일된 출력입니다:

<div class="content-ad"></div>

```js
import { createSignal } from 'solid-js';

export const Counter = () => {
  const [count, setCount] = createSignal(0);
  return <div>{count()}</div>;
};

// Compiled output
import { createSignal } from 'solid-js';
import { template as _$template } from "solid-js/web";
import { insert as _$insert } from "solid-js/web";

const _tmpl$ = /*#__PURE__*/_$template(`<div></div>`, 2);

export const Counter = () => {
  const [count, setCount] = createSignal(0);
  return (() => {
    const _el$ = _tmpl$.cloneNode(true);
    _$insert(_el$, count);
    return _el$;
  })();
};
```



컴파일된 출력 내용에서:

- _tmpl$은 컴포넌트의 DOM 구조를 위한 템플릿을 생성합니다.
- cloneNode은 템플릿을 복제합니다.
- _insert는 값을 채워넣고 반환된 DOM 요소를 부모 DOM 요소에 삽입합니다.

## 렌더링에서 함수 감싸기


<div class="content-ad"></div>

컴포넌트를 렌더링할 때, SolidJS는 신호 의존성의 적절한 설정을 보장하기 위해 함수를 전달해야 합니다. 이렇게 함으로써 의존성의 적절한 관리와 정리를 보장할 수 있습니다:

```js
import { render } from 'solid-js/web';
const dispose = render(() => <App />, document.getElementById("app"));
```

그래서 코드에 console.log가 있는 경우, 한 번 실행된 후 반응성 시스템이 작동합니다:

## JavaScript에서 신호에 대한 메커니즘

<div class="content-ad"></div>

시그널은 Pub-Sub (Publish-Subscribe), Observer 및 이벤트 주도 아키텍처와 같은 확립된 패턴을 기반으로 합니다. 각 패턴은 구성 요소를 분리하고 반응적이고 민첩한 시스템을 가능하게 하지만 고유한 방식으로 이러한 목표를 달성합니다.

Pub-Sub 패턴: 구성 요소는 중앙 이벤트 버스를 통해 통신하여 발신자와 수신자를 분리합니다.
Observer 패턴: 객체(주체)는 상태 변경에 대해 알림을 보내는 의존 객체(옵저버) 목록을 유지합니다.
이벤트 주도 아키텍처: 구성 요소 또는 서비스가 이벤트를 발생시키고 다른 구성 요소가 이를 청취하고 반응합니다.

이러한 것들은 기본적으로 행동과 반응의 아이디어입니다. 시그널은 이러한 패턴에서 요소를 빌려와 세밀한 반응성을 향상시킵니다.

# 성능 및 자동 종속성 관리

<div class="content-ad"></div>

상태 관리에서 가능한 가장 낮은 노드 또는 리프에 구독하는 것은 상태의 가장 작고 가장 세분화된 조각에 집중하는 것을 의미합니다. 이는 상태가 변경될 때 다시 렌더링하거나 다시 계산해야 하는 횟수를 줄여 성능을 크게 향상시킬 수 있습니다. 전체 구성 요소 또는 데이터 구조를 변경할 필요가 없이 일부분이 변경될 때 영향을 받는 구성 요소나 데이터만 업데이트되기 때문입니다.

## 학생 객체를 이용한 예제

학생 객체에는 이름, 성, 그리고 아바타라는 속성이 있습니다. 전체 학생 객체를 구독하는 대신 각 속성을 개별적으로 구독할 수 있습니다.

```js
function Name() {
  const studentName = useSelector((state) => state.student.name);
  return <p>{studentName}</p>;
}

function LastName() {
  const studentLastName = useSelector((state) => state.student.lastname);
  return <p>{studentLastName}</p>;
}

function Avatar() {
  const avatar = useSelector((state) => state.student.avatar);
  return <img src={avatar} />;
}

function StudentExample() {
  return (
    <div>
      <Name/>
      <LastName/>
      <Avatar/>
    </div>
  );
}

export default StudentExample;
```

<div class="content-ad"></div>

위의 예시에서 이름만 변경될 경우 Name 구성요소만 다시 렌더링되며 StudentExample 전체가 다시 렌더링되지는 않습니다. 이 타겟팅된 반응성은 불필요한 업데이트를 최소화하여 성능을 향상시킵니다.

Solid.JS에서는 그런 게 필요없어요. 이름이 바뀌었어요? 실제 DOM의 내부 HTML 내용만 다시 렌더링돼요!

성능 측면에서 보면, 기본적으로 제공되는 기능은 아니지만 React 애플리케이션에서는 우리가 제어할 수 있는 부분이에요.

## 자동 의존성 관리

<div class="content-ad"></div>

개발자들이 열광하는 또 다른 큰 변화는 자동 의존성 관리입니다. 이에 대해 자세히 설명하지는 않겠지만, 우리 리액트 개발자들은 useEffect와 useMemo의 의존성을 지정하기 지겨워합니다. 리액트 팀은 몇 년간 React Forget Compiler에 대해 얘기해 왔고, 이제 React 19부터 드디어 사용할 수 있습니다.

# 리액트의 시그널(Signals)

따라서 우리는 Signals가 일종의 발행-구독 메커니즘임을 명확히 이해하게 됩니다. 다행히도, 우리를 위해 React에는 외부(리액트 메커니즘 외부) 데이터를 구독할 수 있는 훅이 있습니다. 바로 `useSyncExternalStore`입니다.

`useSyncExternalStore`는 외부 스토어를 구독할 수 있게 해주는 리액트 훅입니다.

<div class="content-ad"></div>

## 매개변수

- subscribe: 단일 콜백 인수를 허용하고 그것을 저장소에 구독하는 함수입니다. 저장소가 변할 때 제공된 콜백을 호출해야 합니다. 이로써 컴포넌트가 다시 렌더링됩니다. subscribe 함수는 구독을 정리하는 함수를 반환해야 합니다.
- getSnapshot: 컴포넌트에서 필요한 저장소 데이터의 스냅샷을 반환하는 함수입니다. 저장소가 변경되지 않는 동안, getSnapshot에 대한 반복 호출은 동일한 값을 반환해야 합니다. 저장소가 변경되고 반환된 값이 다른 경우(객체의 동등성 비교로 확인), React는 컴포넌트를 다시 렌더링합니다.

우리는 useSignal 훅을 구성하는 것부터 시작해봅시다.

```js
function useSignal<S>(
  signal: [Accessor<S>, Setter<S>]
): [S, Dispatch<SetStateAction<S>>] {
  const [value, setter] = signal;
  const storeValue = useSyncExternalStore<S>(subscribe, value);

  const setState: Dispatch<SetStateAction<S>> = useCallback(
    (action: SetStateAction<S>) => {
      if (typeof action === 'function') {
        setter((action as (prevState: S) => S)(value()) as any);
      } else {
        setter(action as any);
      }
    },
    []
  );

  return [storeValue, setState];
}
```

<div class="content-ad"></div>

우리의 후크는 React 외부에서 만들어진 신호를 받고, 그 값을 useSyncExternalStore로 전달합니다. useSyncExternalStore는 데이터의 '스냅샷'을 반환하는 함수이며 값이 변경될 때마다 실행되는 subscribe 함수를 반환합니다.

문제는 Solid.JS가 모든 것이 렌더링 함수 또는 createRoot 아래에서 실행되어야 한다고 기대한다는 것입니다. 내부적으로 Solid는 "소유자 스코프"를 만들어 사용합니다.

내부적으로 계산 (effects, memos 등)은 소유자를 만들며 해당 소유자의 하위 소유자로 생성됩니다. 이 소유권 트리는 Solid가 자동으로 dispose된 계산을 클린업하도록 해주며, 계산이 dispose될 때 하위 트리를 탐색하여 모든 onCleanup 콜백을 호출합니다. createEffect의 종속성이 변경되면 예를 들어, effect는 모든 하위 onCleanup 콜백을 호출한 후에 다시 effect 함수를 실행합니다. getOwner를 호출하면 현재 실행 블록의 폐기를 담당하는 현재 소유자 노드를 반환합니다.

<div class="content-ad"></div>

최종 결과를 확인해봐요:

# 리액트에서 사용하지 말아요. 아직 그렇지 마요.

Signals를 React에서 사용하는 도중에 마주치게 될 도전과 잠재적인 단점을 자세히 살펴봅시다.

1. 호환성 및 통합 문제:

<div class="content-ad"></div>

- 라이프사이클 불일치: React와 SolidJS는 서로 다른 라이프사이클 관리 메커니즘을 가지고 있습니다. React는 부작용 관리를 위해 useEffect와 같은 훅에 의존하는 반면, SolidJS는 createEffect와 소유자 기반 정리를 사용합니다. 이러한 불일치는 React에 신호를 통합할 때 문제를 발생시킬 수 있습니다. 이것은 구독 함수를 설정하는 복잡성에서 확인할 수 있습니다.
- 범위 및 소유권: SolidJS의 반응형 계산을 위한 소유자 범위는 React의 컴포넌트 라이프사이클과 직접적으로 매핑되지 않아 적절한 정리 및 메모리 누수 방지에 도전이 있을 수 있습니다.

2. 성능 부하:

- 반응성 시스템의 차이: React의 가상 DOM 차이 비교 및 조정 프로세스는 SolidJS의 세심한 반응성과 다릅니다. React에서 신호를 사용하면 이러한 차이로 인해 추가 부하가 발생할 수 있으며, 이로 인해 성능 이점이 상쇄될 수 있습니다.
- 브릿지 코드 복잡성: React의 상태 관리와 SolidJS 신호 사이의 브릿지를 구현하려면 여분의 코드와 복잡성이 필요합니다. 이는 유지 관리성과 성능에 영향을 미칠 수 있습니다.

3. 개발자 경험:

<div class="content-ad"></div>

- 학습 곡선: React 및 SolidJS의 반응성 모델을 모두 이해해야만 React 내에서 신호를 효과적으로 사용할 수 있습니다. 이 이중 이해는 도전적일 수 있고 개발 초기에 개발 속도를 늦출 수 있습니다.
- 디버깅 및 도구: 두 가지 다른 시스템을 결합할 때 반응적 업데이트를 디버깅하고 반응성 흐름을 이해하는 것은 더 복잡해질 수 있으며, 버그를 진단하고 해결하는 데 문제가 발생할 수 있습니다.

4. 생태계 및 커뮤니티 지원:

- 제한된 생태계 지원: React 생태계는 많은 라이브러리와 도구가 React의 반응성 모델에 특별히 설계되어 있어 광범위합니다. 신호를 통합하면 이러한 도구의 사용이 제한될 수 있거나 추가적인 해결책이 필요할 수 있습니다.
- 커뮤니티 관행: React 커뮤니티는 주로 훅 및 상태 관리에 대한 확립된 관행을 따릅니다. 신호를 도입하면 코드베이스에서 팀 환경을 포함해서 단편화와 일관성에 이르기까지 다양한 문제가 발생할 수 있습니다.

하루의 끝에, Signal의 복잡한 객체가 있다면 해당 데이터를 React에 첨부할 때 렌더링은 로컬 상태를 만든 것과 같습니다. 모든 하향식 탐색은 전체 React 트리를 다시 렌더링하게 만들어서 사실상 '세밀한' 반응성을 요소 수준에서 달성할 수 없습니다.

<div class="content-ad"></div>

# 앞으로 나아가는 길

현재, 시그널은 React, React의 병행성, 그리고 컴파일러 메커니즘에서 명확한 위치가 없습니다. 현재 React에서 시그널을 구현하는 것은 쉬운 일일 것입니다.

## 다른 사고 방식 — 제어 흐름 대 데이터 흐름

React 이전에는 개발자들이 UI를 초기화하고 업데이트하는 데 별도의 로직을 작성했으며, 이로 인해 일관성이 떨어지기도 했습니다. React는 이를 단순화하여 UI가 초기화와 업데이트 사이에서 일관되게 유지되도록 보장합니다. React는 컴포넌트 함수를 다시 실행하고 UI를 최신 상태로 업데이트하여 사용자가 올바른 상태를 확인할 수 있도록 합니다. 이로써 컴포넌트가 처음 나타났는지 업데이트되었는지에 관계없이 사용자가 올바른 상태를 볼 수 있게 됩니다.

<div class="content-ad"></div>

```js
function StudentList({ students, emptyHeading }) {
  const count = students.length;
  let heading = emptyHeading;
  if (count > 0) {
    const noun = count > 1 ? '학생들' : '학생';
    heading = count + ' ' + noun;
  }
  return <h1>{heading}</h1>;
}
```

React에서는 데이터를 가져와 결정을 내리고 입력을 형식화하여 템플릿에 삽입하는 렌더링 로직을 작성합니다. 이 로직은 초기화 및 업데이트에서 모두 실행되어 UI가 데이터와 동기화된 채로 유지되도록 합니다.

Solid는 템플릿에서 특정 "구멍(holes)"만 반응적인 방식으로 다루는 다른 방식을 취합니다. 이는 렌더링 로직에 대해 최상위 제어 흐름을 의존할 수 없다는 것을 의미합니다. 대신 개별 값 주위에 코드를 구조화해야 합니다:

```js
function StudentList(props) {
  const count = () => props.students.length;
  const heading = () => {
    if (count() > 0) {
      const noun = count() > 1 ? "학생들" : "학생";
      return count() + " " + noun;
    } else {
      return props.emptyHeading;
    }
  }
  return <h1>{heading()}</h1>;
}
```

<div class="content-ad"></div>

리액트는 컴포넌트 함수 내에서 if 문과 같은 제어 흐름을 사용할 수 있어 더 많은 로직을 추가하기 쉽게 해줍니다. 반면 솔리드에서는 각 반응형 함수 내에서 조건을 복제해야 합니다 (if(count() ` 0).

두 세계의 장점을 결합하는 것이 목표입니다. 리액트의 간단한 제어 흐름과 솔리드의 성능 최적화를 함께 사용하는 상상을 해보세요. 이렇게 하면 개발자들이 수동 최적화에 대해 걱정하지 않고 명확하고 유지보수 가능한 코드를 작성할 수 있게 될 것입니다.

향후 컴파일러가 코드를 최적화하여 수동 재구성 없이 유사한 성능을 달성할 것을 희망합니다. 이를 통해 리액트의 간편함과 솔리드의 효율성을 함께 얻을 수 있을 것입니다.

# 요약

<div class="content-ad"></div>

신호는 성능 및 자동 종속성 관리 측면에서 중요한 이점을 제공하지만, React에 완벽하게 맞지는 않습니다. 수명주기 관리, 반응성 시스템, 그리고 제어 흐름 대 데이터 흐름이라는 마인드셋의 차이가 있습니다. 지금 당장은 React 개발자로써 기존 도구를 숙달하는 데 집중하고, 희망적으로 React와 SolidJS의 각각의 장점을 결합할 수 있는 잠재적인 미래 개선 사항을 기다릴 필요가 있습니다.