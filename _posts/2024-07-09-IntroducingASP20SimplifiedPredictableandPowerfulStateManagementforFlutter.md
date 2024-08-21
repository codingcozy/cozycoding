---
title: "Flutter 상태 관리, 쉽게 예측 가능하고 강력하게 - ASP 20 소개"
description: ""
coverImage: "/assets/img/2024-07-09-IntroducingASP20SimplifiedPredictableandPowerfulStateManagementforFlutter_0.png"
date: 2024-07-09 22:45
ogImage: 
  url: /assets/img/2024-07-09-IntroducingASP20SimplifiedPredictableandPowerfulStateManagementforFlutter_0.png
tag: Tech
originalTitle: "Introducing ASP 2.0: Simplified, Predictable and Powerful State Management for Flutter"
link: "https://medium.com/flutterando/introducing-asp-2-0-simplified-predictable-and-powerful-state-management-for-flutter-2f92f821b39b"
isUpdated: true
---




<img src="/assets/img/2024-07-09-IntroducingASP20SimplifiedPredictableandPowerfulStateManagementforFlutter_0.png" />

풍부한 피드백을 받은 후, 우리는 API를 변경하여 표준을 더 직관적이고 사용자 친화적으로 만들었습니다. 더 복잡한 프로젝트에 대해 더 엄격한 제한을 추가하고 상태 변경에서의 "예측 가능성" 시스템을 도입하여 추적 및 디버깅을 향상시키고 원치 않는 부작용을 줄였습니다. 일부 변경 사항은 진화적으로 보일 수 있지만, 새로운 API에 기회를 주면 모든 것이 더 의미 있을 것으로 믿습니다.

제거된 내용부터 새로운 기능을 소개합니다:

- -[중대 변경 사항]: RxList, RxSet 및 RxMap과 같은 모든 컬렉션 및 RxFuture 및 RxStream과 같은 비동기 변환기를 제거했습니다. 처음에는 이러한 도우미들이 유용해보였지만, 곧 어려움을 만들었기 때문에 제거하기로 결정했습니다. 결과적으로 모든 확장 기능도 제거되었습니다. 이제 Atom을 만드는 유일한 방법이 있습니다:

<div class="content-ad"></div>

```js
최종 productsState = atom<List<Product>>([]);
```

- [중요 변경 사항]: 새로운 API 기능으로 인해 일부 위젯이 변경되었습니다. ASP는 이제 상태 분배를 위해 hook_state를 채택하며, 제거된 RxRoot를 대체했습니다. 나중에 후크를 기반으로 한 새로운 상태 분배 방법에 대해 알아볼 것입니다. 동일한 이유로 RxCallback도 제거되었습니다. 의미 규명을 위해 RxBuilder는 이제 AtomBuilder로 이름이 변경되었습니다.
- [중요 변경 사항]: ASP의 Reducer API는 혼란과 불만의 주요 원인이었습니다. 여러 Atom에서 값을 추출할 수 있는 매력적이면서도 반응형 액션 시스템이 예측 가능하고 학습이 쉬운 프로세스가 되도록 다시 고민되었어야 했습니다. RxReducer의 가파른 학습 곡선으로 인해 이를 두 개의 새로운 API로 분리하기로 결정했습니다: AtomAction과 AtomSelector. 결과적으로 RxReducer는 더 이상 존재하지 않습니다. 그러나 걱정하지 마세요-훨씬 더 나은 것이 옵니다.
- [중요 변경 사항]: ASP는 함수를 클래스보다 강조하며 보다 기능적인 접근 방식으로 전환 중입니다. Atom과 같은 일부 구성 요소가 함수로 변환되었습니다. 주목할 가장 큰 차이점은 구성 요소 이름의 대문자화 변화입니다:

```js
//v1
final counter = Atom(0);

//v2
final counter = atom(0);
```

- [중요 변경 사항]: 반응성에서 가장 큰 도전 과제는 관찰 가능성입니다. 우리는 ASP로 이를 한 단계 더 나아가게 했습니다. 이제 Atom의 상태를 수정한 액션을 추적하고 누가 Atom을 듣고 있는지에 대한 정보를 얻을 수 있습니다. 관찰 가능성 API를 약간 조정해야 했지만 분명히 그 가치가 있습니다:

<div class="content-ad"></div>

```js
void main() {

  AtomObserver.changes((status) {
    print(status.atom); // counterState
    print(status.action); // incrementAction
    print(status.event.name) // change
  });

  runApp(MyApp());
}
```

## Atomic 상태 관리의 새로운 기능과 방향

- [NEW]: 보다 엄격한 아키텍처적 한계를 갖는 Atom.

Atom은 원자적 상태의 핵심입니다. 이를 통해 자율적인 마이크로 상태를 저장할 수 있는 객체를 생성할 수 있어 widget이나 함수와 같은 모든 것에서 관찰할 수 있습니다. 그러나 Atom은 어디에서든지 수정될 수도 있으며, 이는 전통적인 부작용 문제로 이어질 수 있습니다. 이를 해결하기 위해 상태 설정자를 제거하고 AtomAction이라는 새 구성 요소를 추가했습니다. 이렇게 함으로써 Atom은 여전히 누구나 감시할 수 있지만, 특정 위치에서만 수정할 수 있도록 되어 예측 가능성을 향상시키고 제어되지 않은 부작용을 줄이게 되었습니다.


<div class="content-ad"></div>

예시:

```js
// atom 생성
final counterState = atom(0);

// 변경 사항 청취
counterState.addListener((){
    print(counterState.state); // 1
});

// action 생성
final increment = atomAction((set){
    return set(counterState, 1);
});

// action 실행
increment()
```

Actions은 최대 3개까지의 인수를 받아들일 수 있으며 pub-sub 메커니즘으로 사용할 수도 있습니다:

```js
final counterAction = atomAction1<String>((set, action){
    final state = counterState.state;

    if(action == 'INCREMENT'){
        set(counterState, state + 1);
    } else if(action == 'DECREMENT'){
        set(counterState, state - 1);
    }
});
```

<div class="content-ad"></div>

atomAction 기능은 여러 가지 가능성을 제공하여 간단한 액션과 복잡한 리듀서로 작동합니다. 우리는 액션 호출을 위해 RxReducer를 AtomAction으로 대체했습니다. RxReducer를 완전히 대체하기 위해, 여러 Atom의 파생을 처리하는 방법이 필요했고, AtomSelector API를 사용하여 이를 달성했습니다.

- [NEW]: AtomSelector를 사용한 데이터 파생.

상태 분배 및 여러 상태를 새로운 상태로 수렴시키는 것을 처리하는 것이 ASP 생성으로 이어진 사례 연구에서 주요 도전 과제로 확인되었습니다. 여러 Atom을 결합하고 그 중 하나라도 변경될 때마다 새 값을 계산하는 매우 간단한 방법을 소개했습니다. AtomSelector로 이 작업을 아주 간단하게 처리할 수 있습니다:

```js
// atoms
final nameState = atom('');
final lastNameState = atom('');

// selectors
final fullName = selector((get){
    final name = get(nameState);
    final lastName = get(lastNameState);
    return '$name $lastName';
});

// actions
final changeName = atomAction1<String>((set, name){
    set(nameState, name);
});

changeName('Matias');
```

<div class="content-ad"></div>

선택자의 내부 범위 내에서 get 속성을 사용하여 Atom에 구독할 수 있습니다. 이 시각적인 표시는 무엇을 관찰하고 있는지 보여줍니다. 위 예시는 ASP의 세 가지 요소인 Atoms, Selectors, Actions를 보여줍니다.

또한 AsyncSelector를 사용하여 상태를 비동기적으로 파생하는 방법이 있습니다:

```js
final userIdState = atom(1);

final userState = asyncSelector<User>(
    User.empty(),
    (get) async {
        final id = get(userIdState);
        final response = await dio.get('/user/$id');
        return User.fromJson(response.data);
    }
);
```

이 예시는 Atom을 수정하면 외부 API 호출이 발생하는 보다 복잡한 파생을 보여줍니다. 이 복잡성에도 불구하고 RxReducer 사용자들은 친숙하게 느낄 것입니다. 어떤 Selector도 Atom이며 동일한 내부 API를 공유한다는 점을 유념해야 합니다. 이제 Atom을 Selector와 함께 사용하고 Actions에서만 수정하는 방법을 보여주었으니, Widgets와 모두 연결하는 것을 더 쉽게 만든 방법에 대해 이야기해 보겠습니다.

<div class="content-ad"></div>

- [NEW]: `rxObserver`를 `atomEffect`로 교체했습니다.

위젯에 대해 알아보기 전에, 이제 Atom의 기본 리스너가 `rxObserver`에서 `atomEffect`로 변경되었다는 작은 변화에 대해 이야기해보겠습니다.

반응성의 투명성을 포기하면서, Selector에서 볼 수 있는 것과 같이 `get` 속성을 사용하여 각 반응성을 자동으로 등록해야 합니다. 이렇게 하면 구독되고 있는 것이 무엇인지 더 명확하게 이해할 수 있습니다. 게다가, 추가 구독이 필요하지 않고 범위 안에서 다른 Atoms를 사용할 수 있습니다. 구문은 크게 변경되지 않았지만, [BREAKING CHANGE]이기 때문에 강조할 필요가 있습니다.

```js
final disposer = atomEffect(
    (get) => get(counterState),
    (state) => print('Number is $state'),
);

// release
disposer();
```

<div class="content-ad"></div>

`atomEffect`가 자주 사용되지 않을 수 있습니다. 같은 작업을 수행하며 자동으로 라이프사이클을 관리하는 훅이 있기 때문입니다.

- [NEW]: HOOKS를 사용한 자동 구독.

위젯에 Atom을 구독하기 위한 훅 시스템을 추가하고 있습니다. flutter_hook과는 달리 위젯을 변경하지 않아도 되는 hook_state를 사용합니다. 믹스인을 추가하기만 하면 바로 사용할 수 있어요.

두 가지 훅이 있습니다:
1. useAtomState: Atom을 구독하고 상태가 변경될 때 위젯을 다시 빌드합니다.
2. useAtomEffect: 하나 이상의 Atom을 구독하고 변경될 때 콜백을 트리거합니다. 이는 위젯의 상태에 영향을 주지 않습니다. Snackbar 또는 Navigator와 같은 코드 실행에 적합합니다.

<div class="content-ad"></div>

useAtomEffect을 사용하면 더 이상 RxCallback이 필요하지 않으며 제거되었습니다. Hooks를 가능하게 하는 mixin에 주의해주세요:

StatelessWidget의 경우, HookMixin mixin을 추가하세요.
StatefulWidget의 경우, State에 HookStateMixin mixin을 추가하세요.

mixin을 올바르게 추가한 후에만 Hooks를 사용할 수 있습니다.

예시:

<div class="content-ad"></div>

```js
class CounterPage extends StatelessWidget with HookMixin {
  const CounterPage({super.key});

  void callSnackBar(int state){
    if(state > 5){
        final snackBar = SnackBar(content: Text('Max $state'));
        Asuka.showSnackBar(snackBar);
    }
  }

  @override
  Widget build(BuildContext context) {
    //listen atom
    final state = useAtomState(counterState);

    // listen and call function
    useAtomEffect(
        (get) => get(counterState),
        effect: callSnackBar,
    );
    ...
  }
}
```

Hooks는 라이프사이클을 자동으로 관리하므로 "dispose"와 같은 메모리 "해제"에 대해 걱정할 필요가 없습니다. 꿈이 이루어진 것 같아요!

기타 변경 사항

- [NEW]: Atom에 distinct 속성이 추가되어 동일한 상태가 수신되더라도 반응성을 트리거할지 여부를 지정할 수 있습니다.
- [NEW]: https://asp.flutterando.com.br에서 새 문서가 제공됩니다.
- [FIX]: 모든 사용 예제를 수정했습니다.

<div class="content-ad"></div>

당신이 개발자이세요. 위의 텍스트를 친근한 톤으로 한국어로 번역해주세요.

브라질 최대의 플러터 커뮤니티인 플러터란도에서 모든 지원은 무료입니다. 디스코드에서 함께해요.

함께 패턴을 설정해봐요!