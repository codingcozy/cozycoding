---
title: "플러터에서 상태 관리하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 19:38
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Mastering Riverpod The Future of State Management in Flutter"
link: "https://medium.com/@sharjeel-482/mastering-riverpod-the-future-of-state-management-in-flutter-c2aebb3382d8"
isUpdated: true
updatedAt: 1724742772940
---


플러터 개발자 여러분, 복잡한 상태 관리와 씨름하는 데 지치셨나요? 더 직관적이고 강력한 해결책을 갈망하고 계신가요? 그렇다면 더 이상 기다릴 필요 없어요 — Riverpod이 나타나면서 플러터 애플리케이션에서 상태를 다루는 방법을 혁신할 준비가 되었습니다.

Riverpod은 플러터 커뮤니티를 열광케 하는 차세대 상태 관리 라이브러리입니다. 경험 많은 개발자이든 막 시작하시는 플러터 여행자이든, 이 게시물은 앱 개발 기술을 높이는 데 필요한 지식과 기술을 제공할 거예요.

이 글을 끝까지 읽으시면 다음 내용을 이해하게 될 거에요:
- 플러터에서 상태 관리가 중요한 이유
- Riverpod이 이전 솔루션의 한계를 어떻게 해결하는지
- 프로젝트에서 Riverpod 구현하기까지의 단계
- 상태 관리 최적화를 위한 고급 기술들
- 실제 예제와 최고의 실천 방법

이제 여러분의 플러터 앱의 잠재력을 펼치기 ready하신가요? 함께 시작해봐요!

<div class="content-ad"></div>

플러터에서 상태 관리의 진화

플러터 개발의 초기 시절을 기억하시나요? 상태 관리는 종종 SetState와 같은 솔루션이 단순한 앱을 넘어선다는 느낌으로 머리아플 때가 많았습니다. 그런 다음 Provider가 등장했고, 상태 관리의 성지로 여겨졌던 적도 있었습니다. 그러나 앱이 점점 복잡해지자 Provider조차도 한계를 보이기 시작했습니다.

그리고 나타난 Riverpod — Provider의 창시자 레미 루셀레가 탄생시킨 작품입니다. Riverpod는 Provider의 우수한 기능을 그대로 살리면서도 고급 개념을 도입하여 완전한 상태 관리를 위한 강력한 해결책을 제공합니다.

하지만 왜 Riverpod가 중요한지 궁금할 수도 있습니다. 이를 자세히 알아보겠습니다:

<div class="content-ad"></div>

- 타입 안정성: 상태 관리와 관련된 런타임 오류와 작별해 보세요.
- 테스트 용이성: Riverpod를 사용하면 상태 로직의 단위 테스트가 간단해집니다.
- 확장성: 작은 프로젝트에서 엔터프라이즈급 앱까지, Riverpod는 함께 성장합니다.
- 유연성: 서로 다른 상태 관리 접근 방식을 원활하게 결합할 수 있습니다.

궁금하신가요? 그럼 좋아해야 할 것입니다. 하지만 제 말만 믿지 말고 — Riverpod가 어떤지 직접 보시죠. 시작하기 전에 아래 내용을 확인해주시기 바랍니다.

Riverpod로 시작하기

우선, 프로젝트에 Riverpod를 추가해 봅시다. `pubspec.yaml` 파일을 열고 다음 종속성(dependencies)을 추가해주세요.

<div class="content-ad"></div>

```js
의존성:
  flutter:
    sdk: flutter
  flutter_riverpod: ^2.3.6
  riverpod_annotation: ^2.1.1
개발 의존성:
  build_runner: ^2.4.6
  riverpod_generator: ^2.2.3
```

`flutter pub get`을 실행하여 패키지를 가져오세요.

이제 Riverpod을 사용하여 간단한 카운터 앱을 만들어보겠습니다. 아래는 설정 방법입니다:

```js
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

// 프로바이더 정의
final counterProvider = StateProvider((ref) => 0);

void main() {
  runApp(
    // 앱을 ProviderScope로 감싸세요
    ProviderScope(
      child: MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: HomePage(),
    );
  }
}

class HomePage extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.watch(counterProvider);

    return Scaffold(
      appBar: AppBar(title: Text('Riverpod 카운터')),
      body: Center(
        child: Text('카운트: $count'),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () => ref.read(counterProvider.notifier).state++,
        child: Icon(Icons.add),
      ),
    );
  }
}
```

<div class="content-ad"></div>

리버포드 카운터 예시 이해하기

리버포드가 어떻게 작동하는지 이해하기 위해 우리의 카운터 앱을 해부해봅시다:

1. Provider 정의:

```js
final counterProvider = StateProvider((ref) => 0);
```

<div class="content-ad"></div>

이 줄은 시간이 지남에 따라 변경될 수 있는 간단한 상태에 적합한 `StateProvider`를 생성합니다. 이는 초기값 0으로 시작합니다.

2. ProviderScope:

```js
ProviderScope(
  child: MyApp(),
)
```

`ProviderScope`는 중요합니다. 이는 앱 전체에 Riverpod를 초기화하여 위젯 트리에서 그 아래에 있는 모든 위젯이 providers에 액세스할 수 있도록 합니다.

<div class="content-ad"></div>

3. ConsumerWidget:

```js
class HomePage extends ConsumerWidget {
  // ...
}
```

`ConsumerWidget`은 Riverpod의 `StatelessWidget` 버전입니다. `WidgetRef` 매개변수에 액세스할 수 있게 해주는데, 이를 통해 providers와 상호 작용할 수 있습니다.

4. 프로바이더 감시하기:

<div class="content-ad"></div>

```js
final count = ref.watch(counterProvider);
```

`ref.watch()` 함수는 `counterProvider`를 감시합니다. 카운터 값이 변경될 때마다 이 위젯이 다시 빌드됩니다.

5. 상태 수정:

```js
ref.read(counterProvider.notifier).state++
```

<div class="content-ad"></div>

`ref.read()`은 리스너를 설정하지 않고 공급자에 액세스합니다. 우리는 `.notifier`를 사용하여 `StateController`를 가져와 그 상태를 수정합니다.

주요 포인트

- Providers는 위젯 외부에서 정의되어 쉽게 액세스하고 테스트할 수 있습니다.
- `ProviderScope`는 앱을 위해 Riverpod 생태계를 설정합니다.
- `ConsumerWidget`(또는 더 세분화된 제어를 위해 `Consumer`)는 위젯이 공급자와 상호 작용할 수 있게 합니다.
- `ref.watch()`는 상태 변경을 읽고 듣는 데 사용됩니다.
- `ref.read()`는 일회성으로 상태를 읽거나 수정하는 데 사용됩니다.

고급 Riverpod 개념

<div class="content-ad"></div>

기본 사항을 다룬 이제 Riverpod의 좀 더 고급 기능을 살펴봅시다:

- Family Providers

Family providers를 사용하면 매개변수를 받는 프로바이더를 만들 수 있습니다. 이는 동적 데이터 가져오기에 매우 유용합니다.

```js
final userProvider = FutureProvider.family<User, String>((ref, userId) async {
  return await fetchUser(userId);
});

// 사용법
final user = ref.watch(userProvider('user_123'));
```

<div class="content-ad"></div>

2. 공급자 종속성

Riverpod를 사용하면 다른 공급자에 의존하는 공급자를 쉽게 만들 수 있습니다.

```js
final nameProvider = Provider((ref) => 'John Doe');
final greetingProvider = Provider((ref) {
  final name = ref.watch(nameProvider);
  return 'Hello, $name!';
});
```

3. 자동 해제 항목

<div class="content-ad"></div>

리버포드는 더 이상 필요하지 않을 때 제공자를 자동으로 해제하여 메모리 누수를 방지합니다. 그러나 이 동작을 사용자 정의할 수 있습니다.

```js
final myProvider = StateProvider.autoDispose((ref) {
  ref.onDispose(() {
    print('Provider is being disposed');
  });
  return 0;
});
```

4. 리버포드 생성기

대규모 프로젝트의 경우 리버포드는 보일러플레이트를 줄이기 위해 코드 생성을 제공합니다.

<div class="content-ad"></div>

```js
@riverpod
class Counter extends _$Counter {
  @override
  int build() => 0;

  void increment() => state++;
}
```

이 코드는 `ref.watch(counterProvider)` 처럼 사용할 수 있는 프로바이더를 생성합니다.

Best Practices and Tips

- 프로바이더를 작고 집중된 상태로 유지하세요. 하나의 크고 복잡한 프로바이더보다 여러 작은 프로바이더를 가지는 것이 좋습니다.
- 미세한 재구성을 위해 `select`를 사용하세요.

<div class="content-ad"></div>

```js
 final name = ref.watch(userProvider.select((user) => user.name));
```

3. `ref.listen`를 활용하여 부수 효과 처리하기:

```js
   ref.listen(counterProvider, (previous, next) {
     if (next == 10) {
       showDialog(context, 'You reached 10!');
     }
   });
```

4. 더 나은 코드 구조를 위해 프로바이더를 별도의 파일로 구성하세요.

<div class="content-ad"></div>

5. 복잡한 상태 객체를 변경해야 하는 경우에는 `StateNotifierProvider`를 사용하십시오.
   
결론

Riverpod은 Flutter에서 상태 관리를 위한 강력하고 유연한 접근 방식을 제공합니다. 해당 개념과 최적의 방법을 숙지하여 유지보수가 용이하고 테스트 가능하며 확장 가능한 앱을 만들 수 있습니다.

Riverpod 습득에는 연습이 필요합니다. 다양한 프로바이더 유형을 실험하고 공식 문서를 탐색하며 Flutter 커뮤니티에서 질문을 주저하지 마세요.

<div class="content-ad"></div>

다음 플러터 프로젝트에서 Riverpod을 사용해보는 게 기대되시나요? Riverpod의 어떤 측면이 가장 흥미로우신가요? 아래 댓글에 여러분의 생각과 경험을 공유해주세요!

더 깊이있는 플러터 개발 관련 기사를 기대해주세요. 즐거운 코딩되세요!