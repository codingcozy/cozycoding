---
title: "Riverpod으로 네트워크 호출하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-07 22:28
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Network calls with Riverpod"
link: "https://medium.com/@rkishan516/network-calls-with-riverpod-8eb53aec17bb"
isUpdated: true
---



모든 앱은 서버 측에서 데이터를 보내고 받기 위해 어떤 네트워크 호출을 해야 합니다. 플러터 앱에서 이를 간단하게 만들기 위해, Riverpod을 사용하여 리포지토리 레이어를 좀더 간단하게 만들겠습니다.

![image](https://miro.medium.com/v2/resize:fit:440/1*kOLf4msWFDgNtRyDkMkUlw.gif)

걱정 마세요, 어떻게 할지 보여드릴게요.

Riverpod은 코드 생성 버전과 코드 생성 없는 버전 두 가지가 있습니다. 두 버전을 살펴보고 앱 사용자를 가져오는 방법을 알아봅시다.

<div class="content-ad"></div>

```js
final userProvider = FutureProvider<User>((ref) {
  final result = await ref.read(httpClientProvider).get(currUserUri);
  final body = json.decode(result.body);
  return User.fromJson(body);
});
```

```js
@riverpod
Future<User> user(UserRef ref) async {
  final result = await ref.read(httpClientProvider).get(currUserUri);
  final body = json.decode(result.body);
  return User.fromJson(body);
}
```

이것으로 사용할 준비가 되었습니다. 그러니 어디서든 사용하는 방법을 확인해 봅시다. 먼저 위젯 내에서 사용해 봅시다 :-

```js
class UserImage extends ConsumerWidget {
  const UserImage({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    return ref.watch(userProvider).when(
          data: (user) => Image.network(user.image),
          error: (e,s) => ImageErrorScreen(),
          loading: () => ImageLoader(),
        );
  }
}
```

<div class="content-ad"></div>

가정을 해 보면, 어떤 알림기에서 사용자가 먼저 로드되길 기다린 후에 해당 알림기의 상태를 업데이트하고 싶을 수 있습니다. 그러기 위해 이 공급자의 미래에 대해 기다릴 수 있습니다. 이렇게 할 수 있습니다 :-

```js
class UserCurrentBalanceNotifier extends AutoDisposeNotifier<int> {
  @override
  int build() => ref.read(sharedPref).get('localBalance');

  void sync() async {
    final user = await ref.read(userProvider.future);
    state = user.balance;
  }
}
```

```js
@riverpod
class UserCurrentBalanceNotifier extends _$UserCurrentBalanceNotifier {
  @override
  int build() => ref.read(sharedPref).get('localBalance');

  void sync() async {
    final user = await ref.read(userProvider.future);
    state = user.balance;
  }
}
```

이와 같은 애플리케이션이 많을 수 있고, 이 패턴을 사용하여 코드를 이해하기 쉽고, 테스트하고, 디버그할 수 있습니다. 또한 UI와 비즈니스 로직을 분리할 수 있습니다.

<div class="content-ad"></div>

![image](https://miro.medium.com/v2/resize:fit:848/1*S_Ty8JzCgNyGfe-BJYGrjQ.gif)

Now let’s talk about pros and cons of using riverpod for network calls :-

Pros :-

- Decouples UI from business logic
- Easy to understand and use
- Provides a consistent API for making network calls
- Supports code generation

<div class="content-ad"></div>

아래는 Riverpod의 단점입니다:

- Riverpod은 제공자 트리를 만들고 유지하기 위해 일부 메모리를 사용할 수 있습니다.
- Riverpod은 비교적 새로운 라이브러리이기 때문에 다른 상태 관리 라이브러리들보다 문서화가 떨어질 수 있습니다.
- 크고 복잡한 앱에 대한 코드 생성이 느릴 수 있습니다.

네, 우리가 해냈습니다. 앞으로 오시는 글에서 뵙길 기대합니다!
