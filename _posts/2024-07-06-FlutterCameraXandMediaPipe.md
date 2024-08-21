---
title: "카메라 앱을 위한 최신 기술 Flutter, CameraX, MediaPipe 완벽 가이드"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-06 10:45
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Flutter, CameraX, and MediaPipe"
link: "https://medium.com/gitconnected/flutter-camerax-and-mediapipe-13c33ca95f8d"
isUpdated: true
---




앱을 만들어보고 싶군요! 실시간으로, 예를 들어 개를 분류하는 앱을 만들고 싶다고 하셨군요. 실시간이라 함은 개가 카메라 화면에 들어오자마자 그 종이 표시되어야 하며, 버튼을 누르거나 할 필요 없이 바로 보여져야 한다는 뜻이죠. 이를 위해 어떤 소프트웨어와 도구를 사용할 것인가요?

여러 가능성이 있습니다. 제가 증명-of-concept 결과를 제시해 드리겠습니다.

- UI에는 Flutter
- 카메라를 다루기 위해 CameraX
- Object Detection 및 Image Classification에는 MediaPipe

Flutter는 사용자 인터페이스를 만드는 데 탁월한 도구입니다. 안드로이드 카메라 API의 경우 CameraX가 오랫동안 사용되어왔고 당연한 선택입니다. 또한 최근에 출시된 MediaPipe는 "미리보기 모드"에 있지만, 디바이스에서 AI를 수행하는 데 훌륭한 도구가 될 잠재력이 있습니다.

<div class="content-ad"></div>

저는 이러한 도구를 사용하여 간단한 프로토타입 앱을 만들었습니다. 이 코드는 GitHub에서 확인하실 수 있어요.

이 문서를 읽는 분들은 플러터(Flutter)와 코틀린(Kotlin)에 어느 정도의 경험이 있다고 가정합니다.

## Flutter

“플러터(Flutter)는 구글이 제공하는 오픈소스 프레임워크로, 아름다운 네이티브 컴파일된 멀티 플랫폼 애플리케이션을 하나의 코드베이스로 작성할 수 있습니다.” 저는 모든 앱에 플러터를 사용하고 있으며 그에 대해 매우 좋아해요…
