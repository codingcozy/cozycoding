---
title: "Futurewait  Flutter 여러 작업을 효과적으로 관리하는 방법"
description: ""
coverImage: "/assets/img/2024-07-06-FuturewaitFlutterManagingMultipleTasksTogether_0.png"
date: 2024-07-06 02:49
ogImage: 
  url: /assets/img/2024-07-06-FuturewaitFlutterManagingMultipleTasksTogether_0.png
tag: Tech
originalTitle: "Future.wait — Flutter: Managing Multiple Tasks Together"
link: "https://medium.com/@sanjaysharmajw/future-wait-flutter-9add7ac71f7e"
---


/assets/img/2024-07-06-FuturewaitFlutterManagingMultipleTasksTogether_0.png

플러터(Flutter)와 다트(Dart)에서 Future.wait는 여러 비동기 작업이 완료될 때까지 기다릴 수 있게 해주는 메서드입니다. 이는 future 리스트를 가져와 모든 future가 완료될 때 결과 목록으로 완료되는 새로운 future를 반환합니다. 만약 리스트의 어떤 future라도 실패하면 반환된 future도 동일한 오류로 실패합니다.

다음은 Future.wait를 사용하는 예시입니다:

```js
import 'dart:async';

void main() async {
  // 예시 futures 정의
  Future<String> future1 = Future.delayed(Duration(seconds: 2), () => 'Future 1');
  Future<String> future2 = Future.delayed(Duration(seconds: 1), () => 'Future 2');
  Future<String> future3 = Future.delayed(Duration(seconds: 3), () => 'Future 3');

  // 모든 futures의 완료 기다리기
  List<String> results = await Future.wait([future1, future2, future3]);

  // 결과 출력
  print(results); // 출력: [Future 1, Future 2, Future 3]
}
```

<div class="content-ad"></div>

이 예제에서는 서로 다른 기간 후에 완료되는 세 가지 미래를 정의합니다. 모든 세 가지 미래가 완료될 때까지 기다리기 위해 Future.wait를 사용하고 결과를 출력합니다.

# 오류 처리

어떤 미래든 실패하면 돌아오는 미래도 동일한 오류로 실패합니다. Future.wait를 사용한 오류 처리를 보여주는 예제를 살펴보겠습니다:

```js
void main() async {
  // 몇 가지 예시 미래를 정의합니다
  Future<String> future1 = Future.delayed(Duration(seconds: 2), () => '미래 1');
  Future<String> future2 = Future.delayed(Duration(seconds: 1), () => throw Exception('미래 2 실패'));
  Future<String> future3 = Future.delayed(Duration(seconds: 3), () => '미래 3');

  try {
    // 모든 미래가 완료될 때까지 기다립니다
    List<String> 결과 = await Future.wait([future1, future2, future3]);
    print(결과);
  } catch (e) {
    print('오류: $e'); // 출력: 오류: Exception: Future 2 failed
  }
}
```

<div class="content-ad"></div>

이 예제에서 future2가 예외를 throw합니다. future.wait가 이 오류를 만나면 동일한 오류로 완료되고 catch 블록이 실행됩니다.

# 사용 사례

Future.wait은 여러 비동기 작업을 수행하고 모두 완료된 경우에만 진행해야 할 때 유용합니다. 일반적인 사용 사례는 다음과 같습니다:

- 데이터를 표시하기 전에 여러 소스(예: 네트워크 요청, 파일 읽기)에서 데이터를 로드합니다.
- 여러 데이터베이스 작업 수행하기.
- 병렬 계산 실행하기.

<div class="content-ad"></div>

future.wait을 사용하여 Flutter 애플리케이션에서 여러 비동기 작업을 효율적으로 관리할 수 있어요.

![Future.wait](/assets/img/2024-07-06-FuturewaitFlutterManagingMultipleTasksTogether_1.png)

이 글이 유용했기를 바랍니다! 제공된 정보에 만족하셨다면, 커피 사줄래요! 당신의 도움은 정말 감사하겠어요!