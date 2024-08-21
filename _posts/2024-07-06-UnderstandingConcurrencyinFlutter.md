---
title: "플러터에서 동시성 이해하기"
description: ""
coverImage: "/assets/img/2024-07-06-UnderstandingConcurrencyinFlutter_0.png"
date: 2024-07-06 02:46
ogImage: 
  url: /assets/img/2024-07-06-UnderstandingConcurrencyinFlutter_0.png
tag: Tech
originalTitle: "Understanding Concurrency in Flutter"
link: "https://medium.com/@rishad2002/concurrency-is-crucial-in-mobile-app-development-because-it-allows-an-app-to-perform-multiple-tasks-398faeb56f9d"
isUpdated: true
---





## /assets/img/2024-07-06-UnderstandingConcurrencyinFlutter_0.png

동시성은 모바일 앱 개발에서 중요합니다. 앱이 주 스레드를 차단하지 않고 동시에 여러 작업을 수행할 수 있도록 해줍니다. 이를 통해 사용자 인터페이스가 반응적으로 유지되어 부드러운 사용자 경험을 제공합니다. 예를 들어, 앱은 서버에서 데이터를 가져오고 이미지를 처리하며 사용자 입력을 동시에 처리할 수 있습니다. 적절한 동시성 관리 없이 이러한 작업들이 응답하지 않게 만들고 사용자 경험을 저하시키며 앱 충돌 가능성이 있습니다.

## 이 기사에서 다룰 내용 개요

이 기사에서는 Flutter에서 동시성의 개념과 Dart를 사용한 구현 방법을 탐색할 것입니다. 다음 핵심 포인트를 다룰 예정입니다:

<div class="content-ad"></div>

- 동시성의 기초 및 중요성.
- 플러터 뒤에 있는 Dart 언어가 어떻게 동시성을 처리하는지.
- 퓨처(Futures), async/await 및 스트림(Streams)을 사용하여 플러터에서 비동기 프로그래밍.
- Dart 아이솔레이트를 사용하여 중첩된 계산 및 백그라운드 작업 처리.
- 인기있는 Dart 패키지를 사용하여 동시성 관리.

동시성 이해하기

동시성은 시스템이 여러 작업을 동시에 처리할 수 있는 능력을 나타냅니다. 소프트웨어 개발에서는 여러 계산이나 프로세스를 동시에 실행함으로써 응용 프로그램의 성능과 반응성을 크게 향상시킬 수 있습니다. 동시성은 여러 작업을 관리하면서 그 사이를 전환하여 실행 중인 것처럼 보이게 하는 것을 의미합니다. 이는 CPU 및 메모리와 같은 자원이 제한적인 모바일 기기와 같은 환경에서 특히 중요합니다.

<div class="content-ad"></div>

동시성과 병렬성은 종종 서로 교차하여 사용되지만, 이 둘은 구별되는 개념입니다:

- 동시성은 여러 작업이 동시에 진행됨을 의미합니다. 작업의 실행을 번갈아가며 처리함으로써 한 번에 많은 일을 다루는 것을 의미합니다. 동시성은 주로 멀티스레딩, 비동기 프로그래밍 및 이벤트 루프와 같은 기술을 통해 달성됩니다.
- 병렬성은 실제로 여러 작업이 동시에 실행되는 것을 의미하며, 주로 복수의 프로세서나 코어에서 수행됩니다. 여러 일을 동시에 처리하는 것을 의미합니다. 병렬성은 하드웨어 지원이 필요하며, 여러 계산을 동시에 실행하여 성능 향상이 필요한 시나리오에서 자주 사용됩니다.

더 간단히 말하면, 동시성은 작업 관리와 시간을 효율적으로 활용하는 데 관련되어 있고, 병렬성은 하드웨어 자원을 최대로 활용하는 데 관련되어 있습니다.

<div class="content-ad"></div>

휴대폰 앱 개발에서 동시성은 몇 가지 이유로 중요합니다:

- 응담성: 모바일 앱은 사용자 상호작용에 대해 반응해야 합니다. 동시성이 없으면 네트워크 요청이나 데이터 처리와 같은 장기 실행 작업이 주 스레드를 차단하여 앱이 멈추고 나쁜 사용자 경험을 제공할 수 있습니다.
- 성능: 효율적인 동시성은 자원 이용률을 향상시켜 데이터 가져오기, 이미지 처리 또는 사용자 입력 처리와 같은 작업을 시스템에 과부하를 주지 않고 원활하게 수행할 수 있도록 합니다.
- 배터리 수명: 동시성을 올바르게 관리하면 작업을 스케줄링하고 실행하는 방법을 최적화하여 더 나은 배터리 사용을 도모할 수 있습니다.
- 사용자 경험: 사용자는 모바일 앱이 빠르고 반응적이기를 기대합니다. 동시성을 통해 백그라운드 작업이 앱의 UI의 원활한 작동을 방해하지 않고 연속적인 경험을 제공할 수 있습니다.

# Dart에서의 동시성

/assets/img/2024-07-06-UnderstandingConcurrencyinFlutter_3.png

<div class="content-ad"></div>

Dart은 기본적으로 단일 스레드로, 코드를 단일 시퀀스로 실행합니다. 이 간단함으로 인해 작성, 읽기 및 코드 디버깅이 쉬워집니다. 그러나 동시성을 달성하고 메인 스레드를 차단하지 않고 여러 작업을 처리하기 위해 Dart는 퓨처(Futures), async/await 및 스트림(Streams)과 같은 비동기 프로그래밍 기능을 제공합니다. 이를 통해 여러 스레드를 직접 처리하는 복잡성 없이 효율적인 작업 관리가 가능합니다.

/assets/img/2024-07-06-UnderstandingConcurrencyinFlutter_4.png

Dart는 이벤트 주도 모델을 사용하여 동시성을 관리하며, 이벤트 루프 및 마이크로태스크 큐를 사용하여 비동기 작업을 처리합니다.

- 이벤트 루프: 이벤트 루프는 Dart의 동시성 모델의 중심 부분입니다. 이벤트 루프는 I/O 작업, 타이머 및 기타 비동기 작업과 같은 이벤트를 계속 확인하고 해당 콜백을 실행합니다. 이 이벤트 루프는 메인 스레드가 장기 실행 작업에 의해 차단되지 않도록 보장하여 앱의 응답성을 유지합니다.
- 마이크로태스크 큐: 마이크로태스크 큐는 다른 비동기 이벤트보다 먼저 실행되어야 하는 작업을위한 특별한 큐입니다. 마이크로태스크는 일반 이벤트보다 우선 순위가 높으며, 현재 동기 코드 이후 곧바로 처리되고 기타 보류 중인 이벤트 이전에 처리됩니다. 이를 통해 중요한 작업이 즉시 처리되도록 보장됩니다.

<div class="content-ad"></div>

아래는 이벤트 루프와 마이크로태스크 큐를 설명하는 간단한 예제입니다:

```js
import 'dart:async';

void main() {
  log('시작');

  Future(() {
    log('미래 1');
  });

  scheduleMicrotask(() {
    log('마이크로태스크 1');
  });

  Future(() {
    log('미래 2');
  });

  scheduleMicrotask(() {
    log('마이크로태스크 2');
  });

  log('끝');
}
```

출력:

```js
시작
끝
마이크로태스크 1
마이크로태스크 2
미래 1
미래 2
```

<div class="content-ad"></div>

이는 이벤트 루프에서 미래보다 미세 작업이 먼저 실행됨을 보여줍니다.

Dart는 싱글 스레드이지만 Isolates를 통해 진정한 병렬 처리를 달성할 수 있는 방법을 제공합니다.

- Isolates: Isolates는 독립적인 워커로, 각자의 메모리 공간에서 실행되어 병렬 실행이 가능합니다. 각 Isolate는 자체 이벤트 루프와 미세 작업 대기열을 갖고 있으며, 다른 Isolate와 메모리를 공유하지 않아 락과 같은 동기화 메커니즘이 필요하지 않습니다. 이로써 Isolates는 안전하고 효과적으로 병렬 처리를 수행할 수 있습니다.
- Isolates 생성과 사용: Isolate.spawn 함수를 사용하여 Isolate를 생성하고, 함수와 해당 인수를 전달할 수 있습니다. Isolate 간에는 SendPort와 ReceivePort를 사용하여 메시지 전달을 통해 통신합니다.

Isolate 생성과 사용 예시:

<div class="content-ad"></div>

```js
import 'dart:isolate';

void printMessage(String message) {
  log(message);
}

void main() {
  Isolate.spawn(printMessage, 'Hello from isolate');

  log('Hello from main');
}
```

Output:

```js
Hello from main
Hello from isolate
```

이 예제에서는 printMessage가 별도의 isolate에서 실행되므로 메인 스레드가 isolate의 완료를 기다리지 않고 계속 실행됩니다.

<div class="content-ad"></div>

아이솔레이트는 주 스레드가 응답성을 유지하도록 보장하기 위해 무거운 계산이나 상당한 처리 시간이 필요한 작업에 특히 유용합니다.

# 플러터에서 비동기 프로그래밍

![이미지](/assets/img/2024-07-06-UnderstandingConcurrencyinFlutter_5.png)

## 퓨처(Futures)

<div class="content-ad"></div>

정의 및 사용 사례

Dart의 Future는 향후 일정 시점에 사용 가능한 값 또는 오류를 나타냅니다. 이는 네트워크 요청, 파일 I/O와 같은 비동기 작업 또는 주 스레드를 차단해서는 안 되는 시간이 많이 걸리는 작업을 처리하는 데 사용됩니다. 비동기 함수를 호출하면 값을 또는 오류와 함께 완료될 Future가 반환됩니다.

구문 및 예시

Future를 다루는 구문은 간단합니다. Future 생성은 Future 생성자를 사용하거나 Future를 반환하는 비동기 함수를 사용하여 생성할 수 있습니다.

<div class="content-ad"></div>

예시:

```js
Future<String> fetchData() {
  // 2초 후에 완료되는 네트워크 요청을 시뮬레이트합니다.
  return Future.delayed(Duration(seconds: 2), () => '데이터 로드됨');
}

void main() {
  fetchData().then((data) {
    log(data);  // 2초 후에 출력됨: 데이터 로드됨
  }).catchError((error) {
    log('에러: $error');
  });
}
```

이 예제에서 fetchData는 2초 지연 후에 완료되는 Future를 반환하여 네트워크 요청을 시뮬레이트합니다.

## async와 await

<div class="content-ad"></div>

`async`과 `await`가 Dart에서 어떻게 작동하는지

Dart의 `async`와 `await` 키워드는 Futures와 함께 작업하는 더 가독성이 좋고 간결한 방법을 제공합니다. `async` 키워드는 함수를 비동기로 표시하여 `await` 키워드를 사용할 수 있도록합니다. `await` 키워드는 Future가 완료될 때까지 함수의 실행을 일시 중단하여 비동기 코드가 동기 코드처럼 보이고 동작하도록 만듭니다.

비차단 코드에 `async`와 `await` 사용 예시

예시:

<div class="content-ad"></div>

```js
Future<String> fetchData() async {
  // 네트워크 요청 시뮬레이션
  await Future.delayed(Duration(seconds: 2));
  return '데이터 로드됨';
}

void main() async {
  try {
    String data = await fetchData();
    log(data);  // 2초 후 출력: 데이터 로드됨
  } catch (e) {
    log('에러 발생: $e');
  }
}
```

이 예제에서 fetchData는 async로 표시되었으며 Future가 완료될 때까지 실행을 일시 중지하기 위해 await가 사용되었습니다. main 함수 또한 async로 표시되어 await를 사용합니다.

## 스트림

/assets/img/2024-07-06-UnderstandingConcurrencyinFlutter_6.png


<div class="content-ad"></div>

정의 및 사용 사례

Dart에서의 Stream은 비동기 이벤트의 시퀀스입니다. Streams는 사용자 입력, 파일 읽기 또는 네트워크에서의 실시간 데이터와 같이 시간이 지남에 따라 도착하는 데이터를 처리하는 데 사용됩니다. Streams를 사용하면 모든 데이터가 한꺼번에 사용 가능해질 때까지 기다리는 대신 이벤트에 즉시 반응할 수 있습니다.

단일 구독(Stream)과 브로드캐스트(Stream)의 차이

- 단일 구독 Stream: 한 번만 청취할 수 있습니다. 파일을 읽는 등 특정한 순서로 발생하는 이벤트에 사용됩니다.
- 브로드캐스트 Stream: 여러 청취자가 동시에 청취할 수 있습니다. 사용자 입력 또는 실시간 업데이트와 같이 독립적인 이벤트에 사용됩니다.

<div class="content-ad"></div>

스트림 API 및 예제

하나의 구독 스트림을 사용하는 예제입니다:

```js
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}

void main() {
  countStream(5).listen((number) {
    log(number);  // 매 초마다 출력: 1, 2, 3, 4, 5
  });
}
```

이 예제에서 countStream은 숫자 시퀀스를 생성하며, 1초에 한 번씩 숫자를 발행합니다. listen 메서드를 사용하여 각 숫자를 처리합니다.

<div class="content-ad"></div>

브로드캐스트 스트림 사용 예시:

```js
StreamController<String> controller = StreamController<String>.broadcast();

void main() {
  controller.stream.listen((data) {
    log('Listener 1: $data');
  });

  controller.stream.listen((data) {
    log('Listener 2: $data');
  });

  controller.add('Hello');
  controller.add('World');
  controller.close();
}
```

이 예시에서는 StreamController.broadcast()를 사용하여 브로드캐스트 스트림을 생성했습니다. 두 리스너가 스트림에서 방출된 이벤트를 받습니다.

# 동시성을 위한 아이솔레이트 사용하기

<div class="content-ad"></div>

이솔레이트(Isolates)란 무엇이며 어떻게 작동하는가

이솔레이트는 Dart에서 병렬성을 달성하는 방법입니다. 이들은 독립적으로 작동하는 워커로, 자체 메모리 공간에서 실행되며 다른 이솔레이트와 메모리를 공유하지 않습니다. 이 격리는 경합 조건이나 동기화 필요가 없어져 동시 프로그래밍을 보다 안전하고 간단하게 만들어줍니다. 각 이솔레이트는 자체 이벤트 루프와 마이크로태스크 큐를 갖고 있어 주요 이솔레이트(메인 스레드)와 독립적으로 작업을 수행할 수 있습니다.

이솔레이트와 스레드의 차이점

- 메모리 격리: 이솔레이트는 메모리를 공유하지 않고, 스레드는 동일한 메모리 공간을 공유합니다. 이는 이솔레이트가 공유 데이터에 대한 동시 수정에 대해 걱정할 필요가 없어져 더 안전하게 만듭니다.
- 통신: 이솔레이트는 포트(SendPort와 ReceivePort)를 사용한 메세징을 통해 통신하는 반면, 스레드는 직접 공유 메모리에 접근하며, 락과 세마포어와 같은 동기화 메커니즘을 필요로 합니다.
- 동시성 모델: 이솔레이트는 더 안전하고 에러프루프하게 설계되어 병행성과 병렬성을 위한 방식으로, 스레드는 데드락이나 경합 조건과 같은 문제를 피하기 위해 신중한 관리가 필요합니다.

<div class="content-ad"></div>

새로운 독립체에서 함수를 실행하기 위해 독립체 생성 함수인 Isolate.spawn을 사용하세요. 이 함수는 함수와 인수를 가져와 새로운 독립체에서 함수를 실행합니다.

예시:

```js
import 'dart:isolate';

void sayHello(String message) {
  log('독립체에서의 메시지: $message');
}

void main() {
  Isolate.spawn(sayHello, '안녕하세요, 세계!');
  log('주 메시지');
}
```

<div class="content-ad"></div>

위의 예시에서는 sayHello 함수가 별도의 독립체에서 실행되어 메인 스레드가 계속 실행될 수 있도록 해줍니다.

SendPort 및 ReceivePort를 사용한 독립체 간 통신

<div class="content-ad"></div>

고립된 프로세스 간에 통신하려면 SendPort와 ReceivePort를 사용합니다. SendPort는 고립 프로세스로 메시지를 보내는 데 사용되고, ReceivePort는 메시지를 수신하는 데 사용됩니다.

예시:

```js
import 'dart:isolate';

void isolateEntry(SendPort sendPort) {
  // 메인 고립 프로세스로부터 메시지를 수신하기 위한 ReceivePort 생성
  ReceivePort receivePort = ReceivePort();

  // ReceivePort의 SendPort를 메인 고립 프로세스로 전송
  sendPort.send(receivePort.sendPort);

  // 메시지 수신 대기
  receivePort.listen((message) {
    log('고립 프로세스에서 수신: $message');
    sendPort.send('에코: $message');
  });
}

void main() async {
  // 고립 프로세스로부터 메시지를 수신하기 위한 ReceivePort 생성
  ReceivePort receivePort = ReceivePort();

  // 고립 프로세스 생성
  Isolate isolate = await Isolate.spawn(isolateEntry, receivePort.sendPort);

  // 고립 프로세스로부터 SendPort 가져오기
  SendPort sendPort = await receivePort.first;

  // 고립 프로세스로 메시지 보내기
  sendPort.send('안녕, 고립 프로세스!');

  // 고립 프로세스로부터의 응답 대기
  receivePort.listen((message) {
    log('메인에서 수신: $message');
  });
}
```

출력:

<div class="content-ad"></div>

```js
주요에서 받음: SendPort 
분리된 것에서 받음: 안녕, 분리체! 
주요에서 받음: Echo: 안녕, 분리체!
```

이 예시에서는 메시지 전달을 이용하여 주요 분리체가 생성된 분리체와 통신합니다. 주요 분리체의 ReceivePort는 생성된 분리체에서 SendPort를 받아 서로 메시지를 보낼 수 있도록 합니다.

플러터에서 Isolate의 실용적인 사용 사례

- 무거운 계산: 무거운 계산을 분리체로 옮기면 주요 스레드가 반응성을 유지할 수 있습니다. 예를 들어, 복잡한 수학 계산이나 이미지 처리와 같은 작업을 분리체에서 처리할 수 있습니다.

<div class="content-ad"></div>

```js
import 'dart:isolate';

void computeFactorial(SendPort sendPort) {
  int result = 1;
  for (int i = 1; i <= 10; i++) {
    result *= i;
  }
  sendPort.send(result);
}

void main() async {
  ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(computeFactorial, receivePort.sendPort);
  int result = await receivePort.first;
  log('팩토리얼: $result');
}
```

출력:

```js
팩토리얼: 3628800
```

- 백그라운드 작업: 아이솔레이트는 사용자 인터페이스에 영향을 주지 않는 백그라운드 작업을 실행하는 데 적합합니다. 예를 들어 백그라운드에서 서버와의 데이터 동기화를 처리하는 것과 같이.
- 파일 입출력 작업: 큰 파일의 읽기 및 쓰기 작업을 아이솔레이트로 오프로드하여 주 스레드 차단을 방지할 수 있습니다.


<div class="content-ad"></div>

트레이닝 스케줄 표를 마크다운 형식으로 변경하세요.

<div class="content-ad"></div>

- 미래 유틸리티: async 패키지는 Future와 함께 작동하는 추가 유틸리티를 제공합니다. 예를 들어, FutureGroup를 통해 여러 개의 Futures를 관리하고 모든 작업이 완료될 때까지 기다릴 수 있습니다.

예시:

```js
import 'dart:async';
import 'package:async/async.dart';

void main() async {
  FutureGroup<String> futureGroup = FutureGroup<String>();

  futureGroup.add(Future.delayed(Duration(seconds: 1), () => 'Future 1'));
  futureGroup.add(Future.delayed(Duration(seconds: 2), () => 'Future 2'));
  futureGroup.add(Future.delayed(Duration(seconds: 3), () => 'Future 3'));

  futureGroup.close();

  await futureGroup.future.then((results) {
    log('All futures completed: $results');
  });
}
```

결과:

<div class="content-ad"></div>

```js
모든 미래 작업이 완료되었습니다: [미래 1, 미래 2, 미래 3]
```

- Stream Utilities: StreamZip을 포함하여 여러 스트림을 하나의 스트림으로 결합하여 결합된 스트림에서 값의 튜플을 생성합니다.

예시:

```js
import 'dart:async';
import 'package:async/async.dart';

void main() {
  Stream<int> stream1 = Stream.periodic(Duration(seconds: 1), (i) => i).take(3);
  Stream<String> stream2 = Stream.periodic(Duration(seconds: 2), (i) => 'Str $i').take(3);

  StreamZip([stream1, stream2]).listen((values) {
    log('결합된 스트림: $values');
  });
}
```

<div class="content-ad"></div>

아래는 Markdown 형식으로 제공해드립니다.

```js
Combined Stream: [0, Str 0]
Combined Stream: [1, Str 1]
Combined Stream: [2, Str 2]
```

flutter_isolate: Flutter에서 Isolates 사용하기

flutter_isolate 패키지는 Flutter 애플리케이션에서 Dart isolates를 다루는 것을 더 쉽게 만들어 줍니다. 비동기 백그라운드 작업이나 무거운 계산을 주 스레드를 차단하지 않고 더 편리하게 실행할 수 있도록 isolates를 생성하고 관리하는 더 높은 수준의 API를 제공합니다.

<div class="content-ad"></div>

- Isolate 생성하기: 이 패키지는 아이솔레이트의 생성 및 관리를 단순화하며, 플러터 전용 요구 사항인 context 전달이나 상태 유지와 관련된 작업을 처리할 때 특히 유용합니다.

예시:

```js
import 'package:flutter/material.dart';
import 'package:flutter_isolate/flutter_isolate.dart';

void isolateFunction(String message) {
  log('Isolate: $message');
}

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Flutter Isolate Example'),
        ),
        body: Center(
          child: ElevatedButton(
            onPressed: () async {
              await FlutterIsolate.spawn(isolateFunction, 'Hello from isolate');
            },
            child: Text('Run Isolate'),
          ),
        ),
      ),
    );
  }
}
```

출력:

<div class="content-ad"></div>

```js
격리: 격리로부터 안녕하세요
```

rxdart: 다트와 플러터를 위한 반응형 확장 라이브러리

rxdart는 다트를 위한 반응형 함수형 프로그래밍 라이브러리로, Dart 스트림의 기능을 확장하여 추가적인 연산자와 변환을 제공합니다. ReactiveX에서 영감을 받아 비동기 데이터 흐름과 이벤트를 다루기 위한 다양한 도구를 제공합니다.

- Subjects: Subjects는 여러 리스너로 이벤트를 다중으로 브로드캐스팅할 수 있는 특수한 종류의 StreamController입니다. Stream과 Sink로 동작할 수 있어 반응형 프로그래밍에 강력한 도구로 사용될 수 있습니다.


<div class="content-ad"></div>

예시:

```js
import 'package:rxdart/rxdart.dart';

void main() {
  final subject = BehaviorSubject<int>();

  subject.stream.listen((value) {
    log('Listener 1: $value');
  });

  subject.add(1);
  subject.add(2);

  subject.stream.listen((value) {
    log('Listener 2: $value');
  });

  subject.add(3);
  subject.close();
}
```

결과:

```js
Listener 1: 1
Listener 1: 2
Listener 1: 3
Listener 2: 3
```

<div class="content-ad"></div>

- 연산자: rxdart은 맵, flatMap, debounce, merge 등 다양한 연산자를 제공하여 스트림을 변환하고 조작하는 데 사용할 수 있습니다.

연산자 사용 예시:

```js
import 'package:rxdart/rxdart.dart';

void main() {
  final stream1 = Stream.periodic(Duration(seconds: 1), (i) => i).take(3);
  final stream2 = Stream.periodic(Duration(seconds: 1), (i) => 'Str $i').take(3);

  Rx.combineLatest2(stream1, stream2, (a, b) => '$a - $b').listen((value) {
    log('Combined Stream: $value');
  });
}
```

결과:

<div class="content-ad"></div>

```js
결합된 스트림: 0 - Str 0
결합된 스트림: 1 - Str 1
결합된 스트림: 2 - Str 2
```

이 강력한 Dart 패키지를 활용하여, 개발자들은 플러터 애플리케이션에서 병렬성을 효과적으로 관리할 수 있으며, 반응성이 있고, 효율적이며, 복잡한 비동기 작업을 처리할 수 있는 앱을 구축할 수 있습니다.

# 플러터에서 병렬성의 최상의 사례

UI 반응성 유지의 중요성


<div class="content-ad"></div>

모바일 앱 개발에서 반응형 UI를 유지하는 것은 좋은 사용자 경험을 위해 중요합니다. 메인 스레드(UI 스레드)는 언제나 사용자 상호작용, UI 렌더링 및 애니메이션을 원활하게 처리할 수 있어야 합니다. 만약 메인 스레드가 무거운 계산이나 실행 시간이 오래 걸리는 작업에 의해 차단되면, 앱이 멈추거나 응답하지 않게 되어 사용자 경험이 떨어지고 사용자가 앱을 이탈할 수도 있습니다.

메인 스레드를 차단할 수 있는 작업의 예시

- 네트워크 요청: 서버에서 데이터를 가져오는 것은 특히 느린 네트워크 연결로 인해 시간이 오래 걸릴 수 있습니다.
- 파일 입출력: 큰 파일을 읽거나 쓰는 것은 시간이 많이 소요될 수 있습니다.
- 복잡한 계산: 고도의 수학 계산을 수행하는 것은 메인 스레드를 심각하게 느리게 만들 수 있습니다.
- 데이터베이스 작업: 큰 데이터베이스를 쿼리하는 것은 몇 밀리초 이상 소요될 수 있습니다.

메인 스레드를 차단하는 작업의 예시:

<div class="content-ad"></div>

```js
void main() {
  // 주 스레드에서 블로킹 작업 시뮬레이션 중
  heavyComputation();
  log('UI 스레드는 다른 작업을 처리할 수 있습니다.');
}

void heavyComputation() {
  // 주 스레드를 5초 동안 블로킹
  for (int i = 0; i < 1000000000; i++) {
    // 무거운 계산을 시뮬레이션
  }
  log('무거운 계산이 완료되었습니다.');
}
```

## async 및 await의 효과적인 사용

불필요한 async 함수 피하기

모든 Future를 반환하는 함수가 async로 표시되어야 하는 것은 아닙니다. 함수를 async로 표시하면 약간의 오버헤드가 발생합니다. 비동기 작업을 기다려야 할 때만 async를 사용하세요.

<div class="content-ad"></div>

효율적인 예시:

```js
Future<void> fetchData() {
  return Future.delayed(Duration(seconds: 2));
}
```

<div class="content-ad"></div>

여러 개의 비동기 호출을 효율적으로 연결하는 방법

여러 개의 비동기 작업을 순차적으로 수행해야 할 때, 효율적으로 연결하는 것은 중첩을 피하고 코드를 보기 좋게 유지하기 위해 중요합니다.

예시:

```js
Future<void> performTasks() async {
  try {
    var result1 = await task1();
    var result2 = await task2(result1);
    var result3 = await task3(result2);
    log('All tasks completed with result: $result3');
  } catch (e) {
    log('An error occurred: $e');
  }
}
```

<div class="content-ad"></div>

try-catch를 async-await과 함께 사용하기

async-await을 사용할 때, 비동기 작업 중 발생할 수 있는 모든 오류를 처리하기 위해 코드를 try-catch 블록으로 감싸세요.

예시:

```js
Future<void> fetchData() async {
  try {
    var data = await Future.delayed(Duration(seconds: 2), () => 'Fetched Data');
    log(data);
  } catch (e) {
    log('Error: $e');
  }
}
```

<div class="content-ad"></div>

미래와 스트림에서 오류 처리하기

미래(Futures)의 경우 오류를 처리하려면 catchError 메서드를 사용하세요. 스트림(Streams)의 경우 handleError 메서드나 listen 메서드의 onError 인수를 사용하세요.

미래(Futures) 예제:

```js
Future<void> fetchData() {
  return Future.delayed(Duration(seconds: 2), () => throw '에러가 발생했습니다')
      .then((data) {
    log(data);
  }).catchError((error) {
    log('오류 발생: $error');
  });
}
```

<div class="content-ad"></div>

스트림에 대한 예시:

```js
Stream<int> numberStream = Stream.periodic(Duration(seconds: 1), (count) {
  if (count == 3) throw 'Error at count 3';
  return count;
});

void main() {
  numberStream.listen((data) {
    log('Data: $data');
  }, onError: (error) {
    log('Caught error: $error');
  });
}
```

비동기 코드의 프로파일링과 최적화

Flutter DevTools와 같은 도구를 사용하여 앱을 프로파일링하고 성능 병목 현상을 식별하십시오. 불필요한 지연이나 리소스 사용 없이 효율적으로 작업이 처리되도록 비동기 코드를 최적화하세요.

<div class="content-ad"></div>

메모리 누수 방지하기: 아이솔레이트와 함께

아이솔레이트는 무거운 작업을 처리하는데 도움을 줄 수 있지만, 부적절한 사용은 메모리 누수로 이어질 수 있습니다. 아이솔레이트가 더 이상 필요하지 않을 때 제대로 종료되었는지 확인하세요.

적절히 아이솔레이트를 관리하는 예시:

```js
import 'dart:isolate';

void isolateTask(SendPort sendPort) {
  sendPort.send('작업 완료');
}

void main() async {
  ReceivePort receivePort = ReceivePort();
  Isolate isolate = await Isolate.spawn(isolateTask, receivePort.sendPort);

  receivePort.listen((message) {
    log(message);
    receivePort.close();
    isolate.kill(priority: Isolate.immediate);
  });
}
```

<div class="content-ad"></div>

이 가이드를 따라가면 플러터 애플리케이션에서 동시성을 효과적으로 관리하여 주요 쓰레드 차단, 비효율적인 async 사용, 부적절한 오류 처리 및 메모리 누출 같은 흔한 함정을 피하면서 반응성과 효율성 있는 사용자 경험을 보장할 수 있습니다.

## 네트워크에서 데이터 가져오기

네트워크 요청 수행을 위해 async 및 await 사용

이 예제에서는 http 패키지를 사용하여 API에서 데이터를 가져오기 위한 네트워크 요청을 수행합니다. 비동기 작업을 처리하기 위해 async-await 구문을 사용합니다.

<div class="content-ad"></div>

```js
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Fetch Data Example')),
        body: Center(child: FetchDataWidget()),
      ),
    );
  }
}

class FetchDataWidget extends StatefulWidget {
  @override
  _FetchDataWidgetState createState() => _FetchDataWidgetState();
}

class _FetchDataWidgetState extends State<FetchDataWidget> {
  String _data = 'Fetching data...';

  @override
  void initState() {
    super.initState();
    _fetchData();
  }

  Future<void> _fetchData() async {
    try {
      final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts/1'));
      if (response.statusCode == 200) {
        final jsonResponse = json.decode(response.body);
        setState(() {
          _data = jsonResponse['title'];
        });
      } else {
        setState(() {
          _data = 'Failed to load data';
        });
      }
    } catch (e) {
      setState(() {
        _data = 'Error: $e';
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Text(_data);
  }
}
```

이 예시에서, _fetchData 함수는 네트워크 요청을 수행합니다. async 키워드는 함수를 비동기 함수로 표시하고, await는 네트워크 요청이 완료될 때까지 기다리도록 합니다. setState를 호출하여 가져온 데이터로 UI가 업데이트됩니다.

## 무거운 계산 수행하기

Isolate로 무거운 계산 오프로드하기

<div class="content-ad"></div>

주요 쓰레드의 반응성을 유지하기 위해 무거운 계산 작업을 분리된 isolcate에 넘기고, 그 결과를 다시 주요 쓰레드로 통신할 것입니다.

```js
import 'dart:isolate';
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Isolate Example')),
        body: Center(child: IsolateWidget()),
      ),
    );
  }
}

class IsolateWidget extends StatefulWidget {
  @override
  _IsolateWidgetState createState() => _IsolateWidgetState();
}

class _IsolateWidgetState extends State<IsolateWidget> {
  String _result = '계산 중...';

  @override
  void initState() {
    super.initState();
    _startHeavyComputation();
  }

  Future<void> _startHeavyComputation() async {
    ReceivePort receivePort = ReceivePort();
    await Isolate.spawn(_heavyComputation, receivePort.sendPort);

    receivePort.listen((message) {
      setState(() {
        _result = '결과: $message';
      });
      receivePort.close();
    });
  }

  static void _heavyComputation(SendPort sendPort) {
    int result = 1;
    for (int i = 1; i <= 1000000000; i++) {
      result += i;
    }
    sendPort.send(result);
  }

  @override
  Widget build(BuildContext context) {
    return Text(_result);
  }
}
```

이 예제에서 _heavyComputation은 별도의 isolate에서 무거운 계산 작업을 수행합니다. 결과는 SendPort를 이용해 주요 쓰레드로 전송되고, UI가 결과로 업데이트됩니다.

## 스트림을 활용한 실시간 데이터 처리

<div class="content-ad"></div>

스트림을 사용하여 실시간 데이터 업데이트 다루기

이 예제에서는 스트림을 사용하여 실시간 데이터 업데이트를 시뮬레이션합니다. 데이터 업데이트를 처리하기 위해 스트림 구독을 효과적으로 관리할 것입니다.

```js
import 'dart:async';
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Stream Example')),
        body: Center(child: StreamWidget()),
      ),
    );
  }
}

class StreamWidget extends StatefulWidget {
  @override
  _StreamWidgetState createState() => _StreamWidgetState();
}

class _StreamWidgetState extends State<StreamWidget> {
  StreamController<int> _streamController = StreamController<int>();
  int _latestValue = 0;
  StreamSubscription<int>? _subscription;

  @override
  void initState() {
    super.initState();
    _subscription = _streamController.stream.listen((value) {
      setState(() {
        _latestValue = value;
      });
    });

    _startGeneratingData();
  }

  void _startGeneratingData() {
    Timer.periodic(Duration(seconds: 1), (timer) {
      _streamController.add(timer.tick);
      if (timer.tick == 10) {
        timer.cancel();
        _streamController.close();
      }
    });
  }

  @override
  void dispose() {
    _subscription?.cancel();
    _streamController.close();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Text('Latest value: $_latestValue');
  }
}
```

이 예제에서는 _startGeneratingData가 일정 간격으로 데이터를 생성하여 스트림에 추가합니다. 스트림의 구독은 최신 값으로 UI를 업데이트합니다. 메모리 누수를 방지하기 위해 dispose 메서드에서 구독을 취소하고 스트림 컨트롤러를 닫는 적절한 정리가 이루어집니다.

<div class="content-ad"></div>

## 주요 포인트 요약

이 글에서는 플러터(Flutter)에서의 동시성(concurrency) 개념과 모바일 앱 개발에서의 중요성을 살펴보았습니다. 우리는 동시성에 대한 소개로 시작하여 병렬성과의 차이를 설명하고 앱이 반응적으로 유지되고 효율적으로 작동하는 데 동시성의 중요성을 논의했습니다. 우리는 Dart의 단일 스레드 성격, 이벤트 루프, 마이크로태스크 큐, 그리고 동시성 처리 메커니즘으로 필수적인 독립체(isolates)에 대해 다뤘습니다.

이후에는 Flutter에서의 비동기 프로그래밍을 다루었는데, Future, async 및 await, 그리고 Stream에 대해 다루었습니다. 이러한 도구를 실제 예시와 함께 사용하는 방법을 시범했습니다. 또한 무거운 계산 작업을 외부로 회피하기 위해 Dart Isolates 사용 및 독립체 간 통신에 대해 논의했습니다.

그 다음으로 Flutter 애플리케이션에서 동시성 관리를 용이하게 하는 async, flutter_isolate, rxdart와 같은 인기 있는 Dart 패키지를 살펐습니다. 효과적인 동시성 관리를 위한 예제와 모범 사례, 주 스레드 블로킹 피하기의 중요성, 효율적인 async 및 await 사용, 오류 처리, 그리고 성능 최적화를 강조했습니다.

<div class="content-ad"></div>

마침내, 우리는 네트워크에서 데이터를 가져오는 실제 예시를 제시했고, 무거운 계산을 수행하며, 스트림을 사용하여 실시간 데이터를 처리하는 방법을 소개했습니다. 이러한 개념이 실제 시나리오에서 어떻게 적용될 수 있는지를 보여주었습니다.

## 높은 앱 성능을 위해 병렬성 이해와 구현의 중요성

높은 성능과 반응성을 갖춘 Flutter 애플리케이션을 개발하는 데 병렬성을 이해하고 효과적으로 구현하는 것은 중요합니다. Dart의 비동기 프로그래밍 기능과 고립체를 활용하여 복잡하거나 오랜 시간이 걸리는 작업을 수행할 때도 앱이 부드럽고 반응적인 상태를 유지할 수 있습니다. 적절한 병렬성 관리는 더 나은 사용자 경험, 앱 충돌 감소 및 전반적인 앱 효율성 향상으로 이어집니다.

## Flutter에서 다양한 병렬성 기술을 실험해보도록 장려

<div class="content-ad"></div>

이 글에서 논의된 다양한 동시성 기법을 시도해 보는 것을 장려합니다. 비동기 작업을 처리하기 위해 async와 await를 사용해보고, 실시간 데이터 처리를 위한 Streams의 기능을 탐색하고, 과부하가 걸리는 계산을 처리하기 위해 isolates를 활용해보세요. 작은 프로젝트를 실험하고 구축함으로써, Flutter 애플리케이션에서 동시성을 효과적으로 관리하는 방법에 대한 깊은 이해를 얻을 수 있습니다.

- LinkedIn 팔로우하기

추가 질문이나 추가 지원이 필요하면 언제든지 문의해주세요. 즐거운 코딩 되세요!