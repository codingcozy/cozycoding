---
title: "Flutter에서 무거운 작업 처리하기 - Isolates 사용법 Liveliness 체크 예제 포함"
description: ""
coverImage: "/assets/img/2024-07-07-HandlingHeavyTasksSmoothlyinFlutter-IsolatesWithLivelinesscheckasanexample_0.png"
date: 2024-07-07 19:38
ogImage: 
  url: /assets/img/2024-07-07-HandlingHeavyTasksSmoothlyinFlutter-IsolatesWithLivelinesscheckasanexample_0.png
tag: Tech
originalTitle: "Handling Heavy Tasks Smoothly in Flutter- Isolates (With Liveliness check as an example)"
link: "https://medium.com/@ahadusefefe/handling-heavy-tasks-smoothly-in-flutter-isolates-with-liveliness-check-as-an-example-66e09fb83947"
---


<img src="/assets/img/2024-07-07-HandlingHeavyTasksSmoothlyinFlutter-IsolatesWithLivelinesscheckasanexample_0.png" />

플러터 앱에서 복잡한 기능을 구현했다는 걸 알았군요. 코드는 오류 없이 잘 작동하지만, 그 복잡성 때문에 다른 작업을 처리하는 동안 플러터가 이를 처리하는 데 꽤 부담스러운 것 같네요. 이는 최악의 반응성, 멈춘/빈 화면이 나타난다거나, 심지어 전체 앱이 다운되는 등의 문제로 나타납니다.

이러한 상황은 꽤 자주 발생합니다. 큰 JSON 데이터를 구문 분석하거나 파일을 압축/해제하거나 암호화하거나 이미지 처리를 하는 경우에 스플래쉬 화면이 앱의 전반적인 성능에 미치는 영향이 클 수밖에 없습니다.

이 문제의 근본적인 원인은 플러터/dart가 단일 스레드 프로그래밍 언어라는 점입니다. 각 작업은 주 이벤트 루프에 의해 메인 'UI 스레드'에서 하나씩 실행되도록 이동됩니다. 그래서 이 스레드에 실행 시 몇 초나 몇 분이 걸리는 중요한 작업이 포함된 경우, UI 반응성이나 애니메이션 처리 또는 다른 작업을 처리하는 다른 작업들이 잠시 이 스레드로 진입하는 것을 막히게 됩니다.

<div class="content-ad"></div>

# 솔루션 — 아이솔레이트

아이솔레이트는 Dart가 동시성을 지원하는 방법입니다. 이를 통해 개발자는 자신의 코드를 새로운 스레드에서 실행할 수 있는 자유를 얻게 됩니다. 당신은 큰 작업을 처리하는 코드 부분을 분리하고 이를 분리된 스레드에서 구현합니다.

아이솔레이트를 사용하기 전에, 메인 스레드와 우리의 스레드 간에 통신 수단이 필요하다는 점을 이해해야 합니다. 이것은 우리의 스레드로부터 응답을 가져오거나 처리할 데이터를 보내야 하기 때문에 필수적입니다. 이는 RecievePort를 통해 처리되며, 이는 새로운 스레드와 통신하기 위해 사용되는 스트림으로, 스레드가 주 스레드로 응답을 다시 보내는 자체 'SendPorts'를 보유하고 있습니다.

간단한 구현과 더 고급화된 구현을 사용한 두 가지 예제로 모든 것을 설명해보겠습니다.

<div class="content-ad"></div>

## 큰 데이터를 zip 파일로 압축하기

큰 고객 데이터와 압축해야 하는 디렉토리가 있다고 상상해 봅시다. 디렉토리/고객 데이터의 크기가 커지면, 압축하는 데 걸리는 시간과 메모리 양이 증가하면서 우리 앱이 그 과정에서 멈추는 경우가 발생할 수 있습니다. 아래 코드 블록을 확인해 보세요.

```js
 var json = jsonEncode(customersData);
    File file = File("${documentPath}main.json");
    await file.writeAsString(json);
    var outputPath = "${(await getTemporaryDirectory()).path}${ObjectId().hexString}.zip";
    var encoder = ZipFileEncoder();
    await encoder.zipDirectoryAsync(Directory(documentPath),
        filename: outputPath, onProgress: onProgress);
```

위 코드는 우리가 원하는 방식대로 작동하지만, 큰 데이터의 경우 마지막 메소드가 메인 스레드 안에서 시간이 오래 걸릴 수 있습니다.

<div class="content-ad"></div>

# 노트

![이미지](/assets/img/2024-07-07-HandlingHeavyTasksSmoothlyinFlutter-IsolatesWithLivelinesscheckasanexample_1.png)

이제 할 일은 아이솔레이트를 만들고 이 메서드를 그 안에서 실행하는 것뿐입니다. Isolate.spawn() 메서드를 사용하여 압축하는 메서드를 호출할 것입니다. 또한 이 메서드의 SendPort를 전달할 ReceivePort를 만들어야 합니다. 그 후에는 단순히 받는 포트를 듣기만 하면 됩니다. 보낸 포트의 첫 번째 응답이 우리의 압축 메서드의 응답일 것입니다.

![이미지](/assets/img/2024-07-07-HandlingHeavyTasksSmoothlyinFlutter-IsolatesWithLivelinesscheckasanexample_2.png)

<div class="content-ad"></div>

# Google MLkit 얼굴 감지를 사용한 활기 검사.

여기 시작하는 방법은 실제 흐름이 어떻게 되는지 살펴보는 것입니다. 먼저 카메라 뷰를 열고 카메라 입력을 수신하기 시작합니다. 각 프레임마다 그 순간의 사진을 얻습니다. 그런 다음 각 이미지를 얼굴 감지기가 이해할 수 있는 형식으로 변환해야 합니다. 그 후 각 프레임 이미지는 얼굴의 가용성을 확인하고 각 사용 가능한 얼굴 데이터가 웃는지 또는 눈을 깜빡이는지 여부를 확인합니다. 각 부분을 따로 살펴보겠습니다.

```js
cameraController.value!.startImageStream((inputImage) {
/* 
  각 이미지 데이터를 변환 (이 방법에 대한 공식 문서 링크는 아래에 첨부되어 있습니다)
*/
var image=await _inputImageFromCameraImage(
        inputImage);

// 구글 MLkit를 사용하여 사용 가능한 얼굴 감지
var faces = await faceDetector.processImage(image);
 for (var face in faces) {
      // 웃음 가능성 반환
      return {
        LivelinessDetectionType.smile: (face.smilingProbability ?? 0) > 0.9,
        LivelinessDetectionType.blink:
            ((face.leftEyeOpenProbability ?? 0) < 0.1) &&
                ((face.rightEyeOpenProbability ?? 0) < 0.1)
      };
    }
    return {
      LivelinessDetectionType.smile: false,
      LivelinessDetectionType.blink: false,
    };
});
```

저는 구글 MLkit 얼굴 감지기와 카메라 패키지를 사용하고 있으므로 이미지를 적절한 형식으로 변환해야 합니다. 이 코드를 공식 문서에서 가져와 사용하세요.

<div class="content-ad"></div>

이제 기능에 따라 이들을 분리하고 별도의 클래스에 넣을 것입니다. 이러한 메소드들이 정적 메소드임을 확인해주세요.

그 다음에는 우리의 독립체를 초기화하는 spawn 메소드를 만들어 봅시다. 여기에서도 독립체로부터의 응답을 수신하는 데 사용하는 수신 포트를 생성해야 합니다. 또한 응답이 어떻게 처리되는지 정의할 것입니다.

아래 코드를 확인하고 각 부분을 설명할 테니 참고해주세요.

![이미지](/assets/img/2024-07-07-HandlingHeavyTasksSmoothlyinFlutter-IsolatesWithLivelinesscheckasanexample_3.png)

<div class="content-ad"></div>

# 설명

먼저 통신에 사용할 SendPort와 RecievePort를 선언합니다. 이와 함께, isolate가 성공적으로 시작되고 메시지를 처리할 준비가 된 시점을 신호하는 completer도 선언했습니다.

다음은 spawn 메서드입니다. 이 메서드는 새로운 isolate을 초기화하기 위해 호출됩니다. 이 메서드에는 recieve port의 초기화, recieve port를 수신 대기할 때 수행될 작업을 설정하고, 새 isolate에서의 응답을 처리하고, 마지막으로 isolate을 시작하는 작업이 포함되어 있습니다.

_handleResponseFromIsolate에서는 전송된 메시지의 유형을 확인합니다. 메시지가 sendport인 경우(33번째 줄에서 전송됨) isolate에서 메시지를 보낼 수 있는 port를 받았다는 의미이므로 저장하고 isolate이 생성되었는지 확인합니다. 만약 메시지가 우리의 메서드에서 예상하는 유형인 경우, 필요한 응답을 수신했음을 의미하며 응답으로 무슨 작업을 수행할지 처리합니다.

<div class="content-ad"></div>

_startRemoteIsolate은 메인 독립체의 수신 포트로 생성된 포트를 가져와서 해당 포트를 통해 메시지를 전송합니다. 또한 자체 수신 포트를 생성하고 해당 포트를 메인 독립체로 다시 전송합니다(33번 줄과 24번 줄의 로직을 연결합니다). 그런 다음 메인 독립체가 처리할 메시지를 수신하기를 기다립니다. 메시지를 수신하면 메인 독립체가 실제로 초기화되었는지 확인하고 '무거운 처리 메서드'를 호출하여 작업을 시작합니다. 이 메서드를 통해 전송된 포트를 통해 메인 스레드에 응답을 보냅니다(38번 줄).

마지막으로 응답 방식과 감지를 시작하는 방법을 결정합니다. 응답 방식으로는 실시간으로 검출 상태를 업데이트할 수있는 스트림을 선택했습니다. 감지를 시작하는 데 필요한 이미지 및 기타 필수 데이터를 전달하기 위해 선택했습니다.

이제 검출을 시작하는 데 사용할 수있는 공용 메서드를 추가했습니다. 이 메서드는 그냥 Completer를 얻는 메서드를 호출한 다음 입력 데이터를 send port를 통해 전달할 것입니다.

```js
void detect(LivelinessIsolateData data) async {
    await _isolateReady.future;
    _sendPort.send(data);
}
```

<div class="content-ad"></div>

메인 recievePort (그리고 steamcontroller도)를 닫는 코드도 추가해주세요. 다트는 여러분의 격리체가 사용되지 않고 있음을 감지하고 제거할 것입니다.

```js
dispose() async {
    await _streamController.close();
    _receivePort.close();
}
```

서비스 클래스 전체 코드는 아래와 같습니다.

```js
import 'dart:async';
import 'dart:io';
import 'dart:isolate';
import 'dart:ui';

import 'package:qena_x_flutter_mobile/core/enums/data_enums.dart';
import 'package:camera/camera.dart';
import 'package:flutter/services.dart';
import 'package:google_mlkit_face_detection/google_mlkit_face_detection.dart';

class LivelinessService {
    // 코드 생략
}

class LivelinessIsolateData {
    // 코드 생략
}
```

<div class="content-ad"></div>

우리 서비스 클래스와의 작업 방식에 대한 내용을 마칩니다.

![image](/assets/img/2024-07-07-HandlingHeavyTasksSmoothlyinFlutter-IsolatesWithLivelinesscheckasanexample_4.png)

이제 아마도 더 복잡한 기능을 위해 격리체를 구현하는 방법을 이해하셨을 것입니다. 게다가, 여분의 보너스로 앱 사용자를 위한 활력 확인을 원할하게 추가하는 방법도 배우셨습니다.

그거면 돼요, 읽어 주셔서 감사합니다. 도움이 되었으면 좋겠습니다.
LinkedIn에서 연결해요 https://www.linkedin.com/in/ahadu-sefefe/ 또는 이메일로 안부를 전해주세요 ahadusefefe.123@gmail.com.