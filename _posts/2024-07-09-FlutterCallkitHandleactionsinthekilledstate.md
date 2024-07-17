---
title: "Flutter Callkit  앱이 종료된 상태에서 호출 액션 처리 방법"
description: ""
coverImage: "/assets/img/2024-07-09-FlutterCallkitHandleactionsinthekilledstate_0.png"
date: 2024-07-09 22:42
ogImage: 
  url: /assets/img/2024-07-09-FlutterCallkitHandleactionsinthekilledstate_0.png
tag: Tech
originalTitle: "Flutter Callkit — Handle actions in the killed state"
link: "https://medium.com/@Ayush_b58/flutter-callkit-handle-actions-in-the-killed-state-e6f296c603e6"
---


<img src="/assets/img/2024-07-09-FlutterCallkitHandleactionsinthekilledstate_0.png" />

안녕하세요! 저는 플러터 개발자인 Aayush입니다. 제가 마주한 독특하지만 중요한 도전 과제를 공유하고 싶어요.

# 문제

콜 알림을 구현하고 그에 대한 작업을 처리해야 하는 앱을 작업한 적이 있다면 Hien Nguyen의 flutter_callkit_incoming 플러그인을 사용해 본 적이 있을 겁니다. 그러나 이 플러그인의 문제점은 앱이 종료된 상태일 때 콜백을 제공하지 않는다는 것입니다.

<div class="content-ad"></div>

가정해보죠. 사용자가 전화를 수락하면 특정 화면으로 리디렉션하고 싶지만 앱이 종료된 경우에는 작동하지 않습니다. 그렇더라도 전화 알림을 표시할 수 있습니다.

저는 안드로이드에서 이 문제를 해결했고, 이 기사에서 같은 문제를 해결하는 데 도움을 드리겠습니다. 또한 이 기사를 요약하면, 앱을 시작한 Intent를 가로채서 해당 Intent에서 Flutter로 데이터를 전달하는 방법을 배울 수 있을 것입니다.

# Flutter Callkit Incoming

이것은 Hien Nguyen이 제공한 플러그인으로, 안드로이드에서 전화 처리를 위해 Android 통신 프레임워크를 사용하고 일반적인 알림 빌딩 및 처리에 사용합니다.

<div class="content-ad"></div>

작동하도록하려면 CallKitParams 객체를 초기화하고 필요한 속성을 제공해야합니다. ID를 제공하기 위해 UUID를 사용하세요. 호출자의 이름, 번호, 사진 등과 같은 다른 속성을 수정할 수 있습니다.

이 작업을 완료한 후에는 CallKitParams 객체를 FlutterCallkitIncoming의 showCallkitIncoming 메서드에 전달하면 됩니다. 이게 다입니다! 이제 기기에서 전화를 받을 수 있습니다.

이것을 FirebaseMessaging과 결합하여 실제 문제를 해결할 수 있습니다. 그러나 애플리케이션이 종료된 상태이고 알림을 렌더링한 경우에는, 전화를 수락한 후에 앱이 시작되지만 그게 전부입니다. 전화를 수락함으로써 앱이 열린 것을 식별할 방법이 없었기 때문에 다음에 무엇을 해야 할지에 대해 감이 오지 않습니다.

# 해결책

<div class="content-ad"></div>

안녕하세요! 안드로이드에서는 getIntent(자바) 또는 intent(코틀린)를 호출하여 응용 프로그램을 시작한 인텐트를 가로챌 수 있어요. 이 인텐트가 null이 아니고 플러터에서 지정한 Call accepted 동작과 일치하는지 확인한 후 앱을 시작할 때 사용된 intent에서 flutter_callkit_incoming을 지정하셨다면 해당 인텐트를 처리할 거에요.

이제 인텐트가 있으니, extras getter를 사용하여 데이터를 추출할 수 있어요. 이는 Bundle 유형이며, 이 Bundle을 구문 분석하여 HashMap 형태의 실제 페이로드를 가져와 Flutter에 전달할 거에요.

Native에서 Flutter로 데이터를 전달하려면 MethodChannel을 사용해야 해요.

Android에서 MethodChanel을 사용하여 메서드를 호출하면서, 해당 메서드 이름과 Flutter 쪽 MethodChannel과 일치해야 하는 데이터를 전달해야 해요. Flutter 측에서는 Native에서 선언한 이름과 동일한 MethodChannel에 setMethodCallHandler를 호출해야 해요. 이 핸들러에서 Native에서 호출된 메서드를 처리할 수 있어요.

<div class="content-ad"></div>

이제 마지막 단계는 이 데이터를 가져오는 것입니다. 이를 위해 Completer를 사용할 것입니다. 원시적으로 수신된 데이터가 null이 아니라면 해당 데이터로 future를 완료하겠지만, 그렇지 않으면 데이터를 찾을 수 없다는 오류를 포함하여 완료하겠습니다. 호출 키트 서비스에 위의 방법을 정의하여 Completer가 완료되기를 기다리고 데이터를 반환하도록 할 수 있습니다.

이제 루트 위젯의 initState에서 이 방법을 호출하고 데이터를 적절히 처리할 수 있습니다. 여기까지입니다. 이제 완벽하게 작동해야 합니다.

# 보너스 콘텐츠

Accept 작업을 처리하는 것이 쉬워졌습니다. 왜냐하면 앱이 시작되고 여기에서 Intent를 가로챌 수 있기 때문입니다. 그러나 거절 작업을 처리하려면 어떻게 해야 할까요? 이것은 활동을 시작하지 않기 때문에 까다롭습니다. 따라서 도움을 줄 수 없습니다.

<div class="content-ad"></div>

이 문제를 해결하기 위한 방법은 플러그인 자체를 수정하는 것입니다. 제가 한 작업은 CallKitParams 클래스 내에 onDecline이라는 Callback 함수를 추가했습니다. 이 함수는 JsonKey로 주석을 추가하고 includeFromJson 및 includeToJson을 false로 지정해야 합니다.

```js
@JsonKey(includeFromJson: false, includeToJson: false)
final Function(String reason)? onDecline;
```

이제 FlutterCallkitIncomingPlugin.kt 파일에서 sendEvent 함수를 수정해야 합니다. 플러그인은 활성화된 메소드 채널을 methodChannels 리스트에 저장합니다. 따라서 이벤트가 전화 거부 동작인지 비교한 다음 methodChannels 리스트를 반복하고 Flutter 측에서 콜백을 트리거하는 메소드를 호출해야 합니다. 이 로직은 위에서 설명한 것과 동일합니다.

플러그인의 showCallkitIncoming 함수에서는 setMethodCallHandler를 정의하여 onDecline 콜백을 트리거해야 합니다.

<div class="content-ad"></div>

```js
_channel.setMethodCallHandler((call) async {
      if (call.method == 'CALL_DECLINED_CUSTOM') {
        params.onDecline?.call(call.arguments);
      }
    });
```

감사합니다. 누군가에게 도움이 되었으면 좋겠네요. 추가 질문이 있으면 언제든지 댓글을 남기거나 LinkedIn에서 저에게 연락해 주세요. Flutter로 만든 내 웹사이트에 방문해 주세요. 🌟