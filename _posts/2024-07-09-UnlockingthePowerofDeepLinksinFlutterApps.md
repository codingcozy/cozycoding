---
title: "Flutter 앱에서 딥 링크의 잠재력 해제하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-UnlockingthePowerofDeepLinksinFlutterApps_0.png"
date: 2024-07-09 09:52
ogImage: 
  url: /assets/img/2024-07-09-UnlockingthePowerofDeepLinksinFlutterApps_0.png
tag: Tech
originalTitle: "Unlocking the Power of Deep Links in Flutter Apps"
link: "https://medium.com/@sailesshakya/unlocking-the-power-of-deep-links-in-flutter-apps-ebf12824b710"
---


<img src="/assets/img/2024-07-09-UnlockingthePowerofDeepLinksinFlutterApps_0.png" />

딥 링크는 사용자를 일반적으로 앱을 시작하는 것이 아닌 특정 페이지나 콘텐츠로 안내하는 URL입니다. 이는 마케팅 캠페인, 알림, 특정 콘텐츠를 다른 사용자와 공유할 때 특히 유용합니다.

# 딥 링크의 종류

- 기본 딥 링크: 앱이 이미 설치된 경우에 작동합니다. 앱이 설치되지 않은 경우에는 링크가 실패합니다.
- 지연된 딥 링크: 이것은 앱이 설치되었든 안 되었든 상관없이 작동합니다. 앱이 설치되지 않은 경우 사용자가 먼저 앱을 설치하도록 App Store 또는 Play Store로 이동하고, 그런 다음 링크가 처리됩니다.
- Universal Links (iOS) 및 App Links (Android): 이들은 앱이 설치된 경우 앱의 특정 페이지를 열 수 있는 고급 딥 링크의 형태입니다. 앱이 설치되지 않은 경우 웹 브라우저에서 동일한 콘텐츠를 열도록 되돌아갑니다.

<div class="content-ad"></div>

# 플러터에서 딥 링크 설정하기

## 1. 의존성 추가

먼저, pubspec.yaml에 필요한 의존성을 추가하세요:

```yaml
dependencies:
  flutter:
    sdk: flutter
  uni_links3: ^0.5.3
```

<div class="content-ad"></div>

## 2. 플러터에서 딥 링크 처리하기

주요 위젯에서 들어오는 딥 링크를 처리하는 메소드를 생성하세요.

```js
import 'package:flutter/material.dart';
import 'package:uni_links/uni_links.dart';
import 'dart:async';

void main() => runApp(MyApp());

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  StreamSubscription _sub;

  @override
  void initState() {
    super.initState();
    initDeepLinkListener();
  }

  @override
  void dispose() {
    _sub.cancel();
    super.dispose();
  }

  void initDeepLinkListener() {
    _sub = uriLinkStream.listen((Uri? uri) {
      // 딥 링크 처리
      print('Received deep link: ${uri.toString()}');
      if (uri != null) {
        // 링크에 기반하여 해당 페이지로 이동
        Navigator.pushNamed(context, uri.path);
      }
    }, onError: (err) {
      print('Failed to receive deep link: $err');
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      routes: {
        '/': (context) => HomePage(),
        '/profile': (context) => ProfilePage(),
        // 다른 경로 추가
      },
      initialRoute: '/',
    );
  }
}
```

## 3. iOS 설정

<div class="content-ad"></div>

iOS에서 딥 링크를 처리하려면 앱의 Xcode 프로젝트를 설정해야 합니다.

- Universal Links:

- Xcode에서 앱의 대상 설정인 `Signing & Capabilities`로 이동하여 Capability `Associated Domains`를 추가합니다.
- applinks:yourdomain.com과 같이 applinks: 접두사가 있는 도메인에 대한 항목을 추가합니다.
- 웹 서버에 apple-app-site-association 파일을 추가합니다:

```js
{
  "applinks": {
    "apps": [],
    "details": [
      {
        "appID": "YOUR_TEAM_ID.com.yourcompany.yourapp",
        "paths": [ "/profile/*", "/home/*" ]
      }
    ]
  }
}
```

<div class="content-ad"></div>

## 4. Android 설정

안드로이드에서 깊은 링크를 처리하려면 AndroidManifest.xml을 구성해야 합니다.

- 앱 링크:

- AndroidManifest.xml에 메인 액티비티에 인텐트 필터를 추가하세요:

<div class="content-ad"></div>

2. assetlinks.json 파일을 호스팅하세요:

서버가 .well-known 디렉토리에서 assetlinks.json 파일을 제공할 수 있는지 확인해주세요. 예를 들어, 다음 위치에 파일을 배치해주세요:

```js
<intent-filter android:autoVerify="true">
    <action android:name="android.intent.action.VIEW"/>
    <category android:name="android.intent.category.DEFAULT"/>
    <category android:name="android.intent.category.BROWSABLE"/>
    <data android:scheme="https" android:host="yourdomain.com" android:pathPrefix="/profile"/>
</intent-filter>
```

```js
[
  {
    "relation": ["delegate_permission/common.handle_all_urls"],
    "target": {
      "namespace": "android_app",
      "package_name": "com.yourcompany.yourapp",
      "sha256_cert_fingerprints": [
        "AB:CD:EF:12:34:56:78:90:AB:CD:EF:12:34:56:78:90:AB:CD:EF:12:34:56:78:90:AB:CD:EF:12:34:56:78:90"
      ]
    }
  }
]
```

<div class="content-ad"></div>

# 딥 링크 테스트

딥 링크를 테스트하려면 다음 방법을 사용할 수 있어요:

iOS: xcrun을 사용하여 딥 링크를 시뮬레이션할 수 있어요.

```js
xcrun simctl openurl booted https://yourdomain.com/path
```

<div class="content-ad"></div>

안녕하세요! 안드로이드에서 adb를 사용하여 딥링크를 시뮬레이션하는 방법은 다음과 같습니다.

```js
adb shell am start -W -a android.intent.action.VIEW -d "https://yourdomain.com/profile" com.yourcompany.yourapp
```

iOS 및 Android용 Flutter에서 딥링크에 대해 안내하는 가이드를 읽어주셔서 감사합니다. 여러분의 프로젝트에서 유니버설 연결을 만들 때 교육적이고 유용한 정보로 쓰일 수 있기를 바라며, 딥링크는 앱 내의 특정 콘텐츠에 직접 접근하고 스무스한 네비게이션을 제공함으로써 사용자 경험을 크게 향상시킬 수 있습니다.

궁금한 점, 댓글, 건의사항이 있으시다면 언제든지 공유해주세요. 더 나은 이해를 위해 도와드릴 준비가 되어 있습니다. 즐거운 코딩 하세요!