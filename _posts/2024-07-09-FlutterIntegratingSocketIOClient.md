---
title: "Flutter Socket IO 클라이언트 통합 방법"
description: ""
coverImage: "/assets/img/2024-07-09-FlutterIntegratingSocketIOClient_0.png"
date: 2024-07-09 22:30
ogImage: 
  url: /assets/img/2024-07-09-FlutterIntegratingSocketIOClient_0.png
tag: Tech
originalTitle: "Flutter: Integrating Socket IO Client"
link: "https://medium.com/flutter-community/flutter-integrating-socket-io-client-2a8f6e208810"
isUpdated: true
---




<img src="/assets/img/2024-07-09-FlutterIntegratingSocketIOClient_0.png" />

## 웹 소켓 및 Socket IO란?

웹 소켓은 데이터 전송이 실시간 및 양방향인 양방향 풀 더플렉스 통신 기술입니다. Socket.io는 웹 소켓을 구현하는 데 사용되는 인기 있는 라이브러리입니다.

이 글에서는 Flutter에서 소켓 요청을 통합하는 방법을 살펴볼 것입니다. 서버 측은 NodeJS로 구축할 수 있지만, 이 글에서는 Flutter의 클라이언트 측 통합에만 초점을 맞춥니다.

<div class="content-ad"></div>

먼저, 기본 사항을 알아보고 소켓 요청이 어떻게 작동하는지 이해해 봅시다. 이미 알고 계신 경우 이 부분은 건너뛰셔도 됩니다.

## 작동 방식은 무엇인가요?

우리의 플러터 앱은 클라이언트로 간주되며, 백엔드는 서버로 간주되며 Socket IO를 사용하여 양방향 및 실시간 데이터 전송을 구축할 것입니다. 성공적인 데이터 전송을 위해 따라야 할 흐름 단계는 아래와 같습니다.

- 먼저, 서버와 연결을 설정해야 합니다.
- 당신의 앱은 이벤트를 듣고 있을 것이므로, 새로운 이벤트가 도착하면 UI가 즉시 반영될 것입니다 (채팅에서 새 메시지를 수신하는 것과 같이).
- 이벤트를 발생할 수 있습니다, 백엔드에 데이터를 방송하고 싶을 때 발생시킬 수도 있습니다 (채팅에서 새로운 메시지를 발생시킬 때와 같이).
- 클라이언트와 서버 간 연결을 닫는 것을 잊지 마세요.

<div class="content-ad"></div>

간단해 보이죠? 코드로 구현하는 방법을 살펴봅시다!

![image](https://miro.medium.com/v2/resize:fit:712/0*nYWgTRliCEULXsXA.gif)

## 코딩하는 방법

간단하게 말하면, 우리는 플러터 앱에 실시간 채팅 기능을 구현하기 위해 채팅 소켓 요청을 통합하는 것입니다.

<div class="content-ad"></div>

pubspec.yaml 파일에 소켓 io 클라이언트 패키지를 종속성으로 추가해 주세요:

```yaml
dependencies:
  socket_io_client: ^1.0.2
```

라이브러리를 import 하세요,

```js
import 'package:socket_io_client/socket_io_client.dart' as IO;
```

<div class="content-ad"></div>

채팅 페이지를 열면 사용자가 소켓 서버에 연결됩니다.

```js
IO.Socket socket;
@override
void initState() {
  initSocket();
  super.initState();
}
initSocket() {
  socket = IO.io(APIConstants.socketServerURL, <String, dynamic>{
    'autoConnect': false,
    'transports': ['websocket'],
  });
  socket.connect();
  socket.onConnect((_) {
    print('연결이 설정되었습니다');
  });
  socket.onDisconnect((_) => print('연결이 끊겼습니다'));
  socket.onConnectError((err) => print(err));
  socket.onError((err) => print(err));
}
```

연결이 설정되면 onConnect 콜백이 트리거되며 여기에 로직을 추가할 수 있습니다. 모든 메시지를 가져오거나 새 메시지를 수신하는 것과 같은 작업을 진행할 수 있습니다.

리스너를 추가하려면 socket.on()을 사용하면 신규 이벤트를 수신하고 소켓 서버에서 발생하는 모든 이벤트에 대한 트리거가 됩니다.

<div class="content-ad"></div>

```js
socket.on('getMessageEvent', (newMessage) {
  messageList.add(MessageModel.fromJson(data));
});
```

이벤트를 발신하기 위해 socket.emit()을 사용할 수 있습니다. 아래와 같이 사용하세요 (맵은 각자 상황에 맞게 조정해야 합니다):

```js
sendMessage() {
  String message = textEditingController.text.trim();
  if (message.isEmpty) return;
  Map messageMap = {
    'message': message,
    'senderId': userId,
    'receiverId': receiverId,
    'time': DateTime.now().millisecondsSinceEpoch,
  };
  socket.emit('sendNewMessage', messageMap);
}
```

연결을 종료하는 것을 잊지 마세요,

<div class="content-ad"></div>

```js
@override
void dispose() {
  socket.disconnect();
  socket.dispose();
  super.dispose();
}
```

만약 소켓 연결이 실패한다면, AndroidManifest.xml 파일의 애플리케이션 태그에 다음 속성을 추가해야 할 수 있습니다:

```js
<application
     .....
     android:usesCleartextTraffic="true">
```

그래도 실패한다면, 백엔드 소켓 IO 서버와의 호환성 문제일 수 있습니다. 버전 정보를 확인하고 서버와 클라이언트 라이브러리 버전이 호환되는지 확인하세요:

<div class="content-ad"></div>


<img src="/assets/img/2024-07-09-FlutterIntegratingSocketIOClient_1.png" />

플러터에서 소켓 IO 요청을 통합하는 방법이었습니다. 이 패키지 덕분에 모든게 쉬워졌어요.

읽어 주셔서 감사합니다. 좋았다면 박수를 부탁드립니다.

<img src="/assets/img/2024-07-09-FlutterIntegratingSocketIOClient_2.png" />


<div class="content-ad"></div>

자유 업무, 풀타임 또는 파트타임 역할에 관심이 있습니다. 망설이지 마시고 LinkedIn을 통해 연락 주세요. 감사합니다!