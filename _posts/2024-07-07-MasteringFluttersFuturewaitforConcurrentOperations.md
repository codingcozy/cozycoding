---
title: "동시 작업을 위한 Flutter의 Futurewait 완벽 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-MasteringFluttersFuturewaitforConcurrentOperations_0.png"
date: 2024-07-07 19:37
ogImage: 
  url: /assets/img/2024-07-07-MasteringFluttersFuturewaitforConcurrentOperations_0.png
tag: Tech
originalTitle: "Mastering Flutter’s Future.wait for Concurrent Operations"
link: "https://medium.com/@gimhantharuke856/mastering-flutters-future-wait-for-concurrent-operations-a561ad4367fc"
isUpdated: true
---




<img src="/assets/img/2024-07-07-MasteringFluttersFuturewaitforConcurrentOperations_0.png" />

현대 앱 개발에서 비동기 작업을 효율적으로 처리하는 것은 중요합니다. Dart로 구동되는 Flutter는 비동기 작업을 관리하기 위한 강력한 도구를 제공합니다. Future.wait는 다수의 미래 작업이 완료될 때까지 기다리는 것을 허용하는 도구 중 하나입니다. 이 글은 Future.wait의 개념 및 실제 사용법을 예제와 함께 안내해 드릴 것입니다.

# Future.wait란?

Future.wait는 Dart 메서드로, future 목록을 가져와 제공된 모든 future가 완료될 때까지 대기한 후 결과 목록과 함께 완료되는 새로운 future를 반환합니다. 코드 내 다음 단계로 진행하기 전에 모든 비동기 작업이 완료되길 기다려야 하는 경우 특히 유용합니다.

<div class="content-ad"></div>

# Future.wait를 사용하는 이유

동시성: 여러 비동기 작업을 동시에 실행합니다.
동기화: 모든 작업이 완료될 때까지 기다립니다.
효율성: 독립적인 작업들을 순차적으로 실행하지 않고 성능을 최적화합니다.

# Future.wait 사용하기

Future.wait가 어떻게 작동하는지 이해하기 위해 실제 예제로 들어가 봅시다.

<div class="content-ad"></div>

예시: 여러 소스에서 데이터 가져오기

세 가지 다른 API에서 데이터를 가져와서 결합된 결과를 처리해야 한다고 상상해보세요. Future.wait을 사용하면 이를 효율적으로 처리할 수 있습니다.

```js
import 'dart:async';
void main() async {
 // 몇 개의 Future 정의
 Future<String> future1 = Future.delayed(Duration(seconds: 2), () => 'API 1에서의 데이터');
 Future<String> future2 = Future.delayed(Duration(seconds: 1), () => 'API 2에서의 데이터');
 Future<String> future3 = Future.delayed(Duration(seconds: 3), () => 'API 3에서의 데이터');
// 모든 Future가 완료될 때까지 기다림
 List<String> results = await Future.wait([future1, future2, future3]);
// 결과 처리
 print('모든 API에서의 결과: $results');
}
```

# 설명

<div class="content-ad"></div>

미래 값 정의: 세 가지 미래 값이 정의되어 있으며, 각각이 지연을 시뮬레이션하고 API에서 검색된 것처럼 문자열을 반환합니다.

완료 대기: Future.wait는 이러한 미래 값을 포함하는 리스트를 가져와 모두 완료될 때까지 대기합니다.

결과 처리: 모든 미래 값이 완료되면 결과는 리스트로 제공되며, 필요에 따라 처리할 수 있습니다.

# 오류 처리

<div class="content-ad"></div>

여러 가지 futures를 다룰 때 잠재적인 오류를 처리하는 것이 중요합니다. 목록에서 미래 중 하나가 오류를 throw하면 Future.wait로부터의 결과 future가 해당 오류로 완료됩니다. 오류를 관리하는 방법은 다음과 같습니다:

```js
import 'dart:async';

void main() async {
  // 에러가 있는 futures를 정의합니다.
  Future<String> future1 = Future.delayed(Duration(seconds: 2), () => 'API 1에서의 데이터');
  Future<String> future2 = Future.delayed(Duration(seconds: 1), () => throw Exception('API 2 실패'));
  Future<String> future3 = Future.delayed(Duration(seconds: 3), () => 'API 3에서의 데이터');

  try {
    // 모든 futures가 완료될 때까지 기다립니다.
    List<String> results = await Future.wait([future1, future2, future3]);
    print('모든 API에서의 결과: $results');
  } catch (e) {
    print('오류가 발생했습니다: $e');
  }
}
```

이 예제에서 future2가 예외를 발생시키면 해당 오류가 캐치되어 적절하게 처리됩니다.

# 결론

<div class="content-ad"></div>

`Future.wait`은 Flutter에서 동시 비동기 작업을 관리하는 강력한 도구입니다. `Future.wait`를 이해하고 활용함으로써 앱의 성능을 최적화하고 여러 비동기 작업을 효율적으로 처리할 수 있습니다.