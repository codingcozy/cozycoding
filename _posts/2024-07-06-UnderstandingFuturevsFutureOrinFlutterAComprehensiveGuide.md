---
title: "Flutter에서 Future와 FutureOr 이해하기 종합 가이드"
description: ""
coverImage: "/assets/img/2024-07-06-UnderstandingFuturevsFutureOrinFlutterAComprehensiveGuide_0.png"
date: 2024-07-06 10:46
ogImage: 
  url: /assets/img/2024-07-06-UnderstandingFuturevsFutureOrinFlutterAComprehensiveGuide_0.png
tag: Tech
originalTitle: "Understanding Future vs FutureOr in Flutter: A Comprehensive Guide"
link: "https://medium.com/@azharbinanwar/understanding-future-vs-futureor-in-flutter-a-comprehensive-guide-cb115f1d58cd"
isUpdated: true
---





/assets/img/2024-07-06-UnderstandingFuturevsFutureOrinFlutterAComprehensiveGuide_0.png

플러터는 강력한 비동기 프로그래밍 기능을 제공하여 비동기 작업을 처리하기 위한 두 가지 주요 유형인 Future 및 FutureOr을 제공합니다. 각각의 차이 및 적절한 사용 사례를 이해하면 코드의 효율성과 가독성을 크게 향상시킬 수 있습니다. 이 유형들이 무엇이며 어떻게 효과적으로 사용하는지 알아봅시다.

Future란 무엇인가요?

Future는 비동기 작업의 최종 완료(또는 실패)를 나타냅니다. 이는 해당 작업의 최종 결과(또는 오류)에 대한 자리 표시자 역할을 합니다.

<div class="content-ad"></div>

미래를 편지로 상상해 보세요. 당신도 알고 있듯이 그것이 오고 있다는 것을, 그러나 아직 갖고 있지는 않습니다. Future를 사용하는 코드는 async 및 await 또는 .then 및 .catchError와 같은 키워드를 사용하여 완료될 때까지 기다려야 하는 비동기적인 성격을 처리해야 합니다.

다음은 Future의 일반적인 사용 사례입니다:

- 네트워크로부터 데이터 가져오기: 네트워크 요청은 본질적으로 비동기적입니다. Future를 사용하여 API 호출로부터의 응답을 처리할 수 있어 코드가 메인 스레드를 차단하지 않고 실행을 계속할 수 있습니다.

```js
Future<String> fetchUserData(int userId) async {
  final response = await http.get(Uri.parse('https://api.example.com/users/$userId'));
  if (response.statusCode == 200) {
    final data = jsonDecode(response.body);
    return User.fromJson(data);
  } else {
    throw Exception('사용자 데이터를 가져오지 못했습니다');
  }
}
```

<div class="content-ad"></div>

이 예시에서 fetchUserData는 Future`String`을 반환합니다. 호출 코드는 비동기적인 성격을 처리하고 데이터를 수신한 후 사용자 데이터를 처리하기 위해 async와 await를 사용할 수 있습니다.

FutureOr 소개: 유연한 친척

FutureOr는 비동기 작업을 다룰 때 더 많은 유연성을 제공합니다. 이것은 Future이거나 실제 값 자체일 수 있는 값을 나타냅니다.

우체통을 확인하는 것과 같다고 생각해보세요 - 편지(Future)를 찾을 수도 있고, 우편물이 이미 배달된 상태(실제 값)일 수도 있습니다.

<div class="content-ad"></div>

여기서 FutureOr가 빛을 발하는 곳입니다:

- 미리 가져온 데이터 처리: 데이터가 이미 준비되어 있는 경우 (예: 캐시에서), FutureOr을 사용하여 값을 직접 반환하여 불필요한 비동기 작업을 피할 수 있습니다.

```js
FutureOr<String> getCachedValue(String key) {
  final cachedData = prefs.getString(key);
  if (cachedData != null) {
    return cachedData; // 캐시된 값을 직접 반환
  } else {
    // 비동기 데이터베이스 가져오기로 대체
    return _fetchValueFromDatabase(key);
  }
}

Future<String> _fetchValueFromDatabase(String key) async {
  // 데이터베이스 작업 시뮬레이션
  await Future.delayed(Duration(seconds: 1));
  return '데이터베이스에서 가져옴!';
}
```

이 예제에서 getCachedValue는 FutureOr`String`을 반환합니다. 먼저 캐시된 데이터를 확인하고 이용 가능한 경우 직접 반환합니다. 그렇지 않은 경우 값 비동기적으로 가져오고 Future를 반환합니다. 이는 호출 코드의 논리를 간단하게 만듭니다.

<div class="content-ad"></div>

## 주요 차이점

사용 방법:

- Future: 미래에 사용 가능할 값으로 항상 비동기 작업에 적합합니다.
- FutureOr: 즉시 값 또는 미래 값으로 나타낼 수 있어 동기적 또는 비동기적으로 반환할 수 있는 함수에 유용합니다.

유연성:

<div class="content-ad"></div>

- Future: 비동기 작업을 명확히 나타내어 async 흐름을 명시적이고 쉽게 따를 수 있습니다.
- FutureOr: 함수가 항상 비동기 작업을 수행할 필요가 없을 때 보일러플레이트를 줄이고 코드를 단순화합니다.

## Future 및 FutureOr 사용의 장점

Future:

- 명확하고 예측 가능한 async 흐름을 보장합니다.
- 항상 비동기적인 작업에 이상적입니다.

<div class="content-ad"></div>

FutureOr:

- 동기 및 비동기 반환을 모두 처리하여 다재다능성을 제공합니다.
- 함수가 혼합 반환 유형을 가질 때 코드 복잡성을 감소시키고 가독성을 향상시킵니다.

# 결론

Future와 FutureOr를 언제 사용해야 하는지 이해하면 Flutter 코드의 효율성과 가독성을 크게 향상시킬 수 있습니다. Future는 엄격히 비동기 작업에 적합하지만 FutureOr는 즉시 반환하거나 어떤 지연 후에 반환해야 하는 함수에 필요한 다재다능성을 제공합니다. 필요에 맞는 유형을 선택함으로써 Flutter에서 보다 간결하고 효과적인 비동기 코드를 작성할 수 있습니다.

<div class="content-ad"></div>

주석으로 의견을 공유해주세요!