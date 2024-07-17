---
title: "Flutter에서 WebView에 대한 모든 것"
description: ""
coverImage: "/assets/img/2024-07-07-EverythingaboutWebViewinFlutter_0.png"
date: 2024-07-07 22:26
ogImage: 
  url: /assets/img/2024-07-07-EverythingaboutWebViewinFlutter_0.png
tag: Tech
originalTitle: "Everything about WebView in Flutter"
link: "https://medium.com/@MarvelApps_/everything-about-webview-in-flutter-ab56a2315f0f"
---


웹뷰는 모바일 애플리케이션의 중요한 부분입니다. 앱 내에서 웹사이트에 접근할 수 있게 해주어 브라우저로 이동하지 않아도 됩니다. 앱 내부에서 외부 웹 리소스를 보여줘야 할 때 유용하게 사용될 수 있습니다. 특히, 모바일 애플리케이션을 구축하기 위해 많은 비용과 시간이 소요된다면, 웹뷰를 사용하여 앱에서 해당 웹 리소스를 표시할 수 있습니다.

이 블로그에서는 Flutter에서 웹뷰를 구현하는 방법을 살펴볼 것입니다. webview_flutter 패키지를 사용하여 주어진 URL에 호스팅된 웹사이트의 내용을 보여주는 WebViewWidget을 사용할 것입니다.

![이미지](/assets/img/2024-07-07-EverythingaboutWebViewinFlutter_0.png)

우선, 다음 명령어를 사용하여 처음부터 새 애플리케이션을 생성해보겠습니다.

<div class="content-ad"></div>

```js
flutter create webview_flutter_module
```

애플리케이션이 오류 없이 성공적으로 생성되면, pubspec.yaml 파일에 webview_flutter 종속성을 추가하고 저장한 다음, 프로젝트의 루트에서 터미널에서 "flutter pub get" 명령을 실행하세요.

```js
dependencies:
  webview_flutter: ^4.2.2
```

Android에서 webview_flutter를 사용하려면 android/app/build.gradle 파일에서 minSdkVersion을 다음과 같이 20으로 변경해야 합니다. 

<div class="content-ad"></div>

```js
android {
    //...
defaultConfig {
        applicationId "com.example.webview_flutter_module"
        minSdkVersion 20                           // 수정
        targetSdkVersion flutter.targetSdkVersion
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
    }
```

# **Flutter 애플리케이션에 WebView 위젯 추가하기**

웹뷰 위젯을 추가하려면 먼저 나중에 WebViewWidget에 전달될 WebViewController를 인스턴스화해야 합니다. 이 컨트롤러는 URL을 로드하거나 이벤트를 수신하거나 네이게이션을 처리하는 등의 작업에 사용되므로 매우 중요합니다. WebViewWidget을 사용할 때 이 컨트롤러에 종속됩니다.

Flutter에 WebView 위젯을 추가하기 위해 다음과 같이 웹사이트 URI로 loadRequest() 메서드를 호출합니다.

<div class="content-ad"></div>

```js
controller = WebViewController()
      ..loadRequest(
        Uri.parse('https://flutter.dev'),
      );
```

이제 위의 코드를 StateFul 위젯의 initState 메서드에 다음과 같이 추가하고, controller를 WebViewWidget에 전달하여 웹사이트 내용만 플러터 애플리케이션에서 표시할 수 있습니다.

```js
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';
import 'package:webview_flutter_module/constants/text_constants.dart';

class WebViewScreen extends StatefulWidget {
  const WebViewScreen({Key? key}) : super(key: key);
  @override
  State<WebViewScreen> createState() => _WebViewScreenState();
}
class _WebViewScreenState extends State<WebViewScreen> {
  late WebViewController controller;
  @override
  void initState() {
    super.initState();
    controller = WebViewController()
      ..loadRequest(
        Uri.parse('https://flutter.dev'),
      );
  }
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        centerTitle: true,
        title: const Text(TextConstants.appBarTitle),
      ),
      body: WebViewWidget(
        controller: controller,
      ),
    );
  }
}
```

이제 플러터 애플리케이션에서 flutter.dev 웹사이트 내용을 표시하는 기본 WebView를 만들었으므로 프로젝트 루트에서 다음 명령을 터미널에서 실행하여 플러터 프로젝트를 실행해 보겠습니다.

<div class="content-ad"></div>

```js
flutter run
```

프로젝트가 성공적으로 컴파일되고 실행되면, 다음과 같은 화면을 볼 수 있어요

<img src="https://miro.medium.com/v2/resize:fit:704/1*AxyfnLdWo-IebCCZmimxJA.gif" />

# 페이지 로드 이벤트 수신하기

<div class="content-ad"></div>

기본 웹뷰 애플리케이션을 만들었어요. 웹뷰 컨트롤러에 전달된 URL에서 호스팅된 웹사이트 스냅샷만 표시하는 기능을 구현했지만 웹사이트를 표시하는 것 외에는 아무것도 하지 않았어요.
그러니까 좀 더 심층적으로 들어가서 setNavigationDelegate() 메서드를 사용하여 페이지 로드 이벤트를 감지해봐요.

```js
controller
..setNavigationDelegate(NavigationDelegate(
        onPageStarted: (url) {
          setState(() {
            loadingPercentage = 0;
          });
        },
        onProgress: (progress) {
          setState(() {
            loadingPercentage = progress;
          });
        },
        onPageFinished: (url) {
          setState(() {
            loadingPercentage = 100;
          });
        },
      ))
..loadRequest(Uri.parse('https://flutter.dev'));
```

그리고 WebViewScreen 위젯에서 다음과 같이 변경해봐요.

```js
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';
import 'package:webview_flutter_module/constants/text_constants.dart';

class WebViewScreen extends StatefulWidget {
  const WebViewScreen({Key? key}) : super(key: key);
  @override
  State<WebViewScreen> createState() => _WebViewScreenState();
}
class _WebViewScreenState extends State<WebViewScreen> {
  late WebViewController controller;
  var loadingPercentage = 0;
  @override
  void initState() {
    super.initState();
    controller = WebViewController()
      ..setNavigationDelegate(NavigationDelegate(
        onPageStarted: (url) {
          setState(() {
            loadingPercentage = 0;
          });
        },
        onProgress: (progress) {
          setState(() {
            loadingPercentage = progress;
          });
        },
        onPageFinished: (url) {
          setState(() {
            loadingPercentage = 100;
          });
        },
      ))
      ..loadRequest(
        Uri.parse('https://flutter.dev'),
      );
  }
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        centerTitle: true,
        title: const Text(TextConstants.appBarTitle),
      ),
      body: Stack(
        children: [
          WebViewWidget(
            controller: controller,
          ),
          loadingPercentage < 100
              ? LinearProgressIndicator(
                  color: Colors.red,
                  value: loadingPercentage / 100.0,
                )
              : Container()
        ],
      ),
    );
  }
}
```

<div class="content-ad"></div>

위의 코드를 실행한 후 아래의 gif와 같은 결과물이 나올 것입니다. gif에서 보이는 것처럼, 앱 바 바로 아래에 웹 사이트가 앱에로드된 퍼센트를 보여주는 빨간색 직선 진행 표시기가 있습니다. 웹 사이트가 완전히로드되면 사라질 것입니다.

![웹 사이트 로드 퍼센트를 표시하는 예시 gif](https://miro.medium.com/v2/resize:fit:704/1*8hEoEqifC-vrNevN5JtdhQ.gif)

# Flutter 앱의 WebViewWidget을 위한 탐색 컨트롤

다중 화면 웹 사이트의 경우, 플러터 애플리케이션에 백, 앞으로 또는 새로고침과 같은 탐색 컨트롤을 추가하고 싶을 수 있습니다. WebViewWidget에서 보여주는 웹 사이트의 탐색을 제어하기 위해 다시 WebViewController를 다음과 같이 사용하게 될 것입니다.

<div class="content-ad"></div>

위의 코드를 실행한 후에는 다음 출력물이 있어야 합니다.

<img src="https://miro.medium.com/v2/resize:fit:704/1*jUzBybIExQicyLHV440RVQ.gif" />

이제 특정 웹사이트에 액세스할 때 네비게이션을 차단하려는 경우가 있을 수 있습니다. 이 경우에는 NavigationDelegate를 사용하여 네비게이션을 추적해야 하며 이를 수행하기 위해 appBar에 메뉴 버튼을 추가할 것입니다. 다음 섹션에서 이를 구현할 것입니다.

<div class="content-ad"></div>

# NavigationDelegate를 사용하여 탐색 추적 및 AppBar에 메뉴 버튼 추가하기

먼저 아래와 같이 컨트롤러에 NavigationDelegate를 추가해보겠습니다. 여기서는 URL에 "youtube.com"이 포함된 웹페이지로의 이동을 원하지 않습니다.

```js
controller = WebViewController()
      ..setNavigationDelegate(NavigationDelegate(onPageStarted: (url) {
        setState(() {
          loadingPercentage = 0;
        });
      }, onProgress: (progress) {
        setState(() {
          loadingPercentage = progress;
        });
      }, onPageFinished: (url) {
        setState(() {
          loadingPercentage = 100;
        });
      },
// NavigationDelegate를 이용하여 탐색 추적 유지
          onNavigationRequest: (navigation) {
        final host = Uri.parse(navigation.url).host;
        if (host.contains('youtube.com')) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(
              content: Text(
                '$host로의 탐색 차단 중',
              ),
            ),
          );
          return NavigationDecision.prevent;
        }
        return NavigationDecision.navigate;
      }))
      ..loadRequest(
        Uri.parse('https://flutter.dev'),
      );
```

이제 메뉴 바를 생성하고 아래와 같이 앱 바에 추가해봅시다.

<div class="content-ad"></div>

```js
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';

enum _MenuOptions {
  navigationDelegate,
}
class Menu extends StatelessWidget {
  const Menu({required this.controller, Key? key});
  final WebViewController controller;
  @override
  Widget build(BuildContext context) {
    return PopupMenuButton<_MenuOptions>(
      onSelected: (value) async {
        switch (value) {
          case _MenuOptions.navigationDelegate:
            await controller.loadRequest(Uri.parse('https://youtube.com'));
            break;
        }
      },
      itemBuilder: (context) => [
        const PopupMenuItem<_MenuOptions>(
          value: _MenuOptions.navigationDelegate,
          child: Text('YouTube로 이동'),
        ),
      ],
    );
  }
}
```

프로젝트를 실행한 후 앱 바에 메뉴를 추가하면 다음과 같은 결과가 나와야 합니다

<img src="https://miro.medium.com/v2/resize:fit:704/1*zCOA8RlRBBre07OsnHhJow.gif" />

# JavaScript 평가하기


<div class="content-ad"></div>

우리의 웹뷰에서 자바스크립트를 활성화하려면 다음과 같이 controller.setJavaScriptMode()를 사용할 것입니다.

```js
controller = WebViewController()
      ..setNavigationDelegate(NavigationDelegate(onPageStarted: (url) {
        setState(() {
          loadingPercentage = 0;
        });
      }, onProgress: (progress) {
        setState(() {
          loadingPercentage = progress;
        });
      }, onPageFinished: (url) {
        setState(() {
          loadingPercentage = 100;
        });
      },
// NavigationDelegate를 사용하여 탐색 추적
          onNavigationRequest: (navigation) {
        final host = Uri.parse(navigation.url).host;
        if (host.contains('youtube.com')) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(
              content: Text(
                'Blocking navigation to $host',
              ),
            ),
          );
          return NavigationDecision.prevent;
        }
        return NavigationDecision.navigate;
      }))
      ..setJavaScriptMode(JavaScriptMode.unrestricted)
      ..loadRequest(
        Uri.parse('https://flutter.dev'),
      );
```

이제 자바스크립트를 실행하려면 Menu 위젯을 다음과 같이 수정하겠습니다.

```js
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';

enum _MenuOptions { navigationDelegate, userAgent }
class Menu extends StatefulWidget {
  const Menu({required this.controller, Key? key});
  final WebViewController controller;
  @override
  State<Menu> createState() => _MenuState();
}
class _MenuState extends State<Menu> {
  @override
  Widget build(BuildContext context) {
    return PopupMenuButton<_MenuOptions>(
      onSelected: (value) async {
        switch (value) {
          case _MenuOptions.navigationDelegate:
            await widget.controller
                .loadRequest(Uri.parse('https://youtube.com'));
            break;
          case _MenuOptions.userAgent:
            final userAgent = await widget.controller
                .runJavaScriptReturningResult('navigator.userAgent');
            if (!mounted) return;
            ScaffoldMessenger.of(context).showSnackBar(SnackBar(
              content: Text('$userAgent'),
            ));
            break;
        }
      },
      itemBuilder: (context) => [
        const PopupMenuItem<_MenuOptions>(
          value: _MenuOptions.navigationDelegate,
          child: Text('Navigate to YouTube'),
        ),
        const PopupMenuItem<_MenuOptions>(
          value: _MenuOptions.userAgent,
          child: Text('Show user-agent'),
        ),
      ],
    );
  }
}
```

<div class="content-ad"></div>

위의 코드를 실행한 후에는 다음 출력이 있어야 합니다

<img src="https://miro.medium.com/v2/resize:fit:704/1*3z9q6YUBAs6bwkcnjg26NQ.gif" />

# JavaScript 채널 처리

먼저 controller.addJavaScriptChannel()을 사용하여 JavaScript 채널을 추가해야 합니다.

<div class="content-ad"></div>

```js
controller..addJavaScriptChannel(
        'SnackBar',
        onMessageReceived: (message) {
          ScaffoldMessenger.of(context).showSnackBar(SnackBar(
              content: Text(
            message.message,
            style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
          ))
```

그리고 다시 Menu 위젯에 팝업을 추가해 JavaScript 채널을 실행합니다.

```js
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';

enum _MenuOptions { navigationDelegate, userAgent, javascriptChannel }
class Menu extends StatefulWidget {
  const Menu({required this.controller, Key? key});
  final WebViewController controller;
  @override
  State<Menu> createState() => _MenuState();
}
class _MenuState extends State<Menu> {
  @override
  Widget build(BuildContext context) {
    return PopupMenuButton<_MenuOptions>(
      onSelected: (value) async {
        switch (value) {
          case _MenuOptions.navigationDelegate:
            await widget.controller
                .loadRequest(Uri.parse('https://youtube.com'));
            break;
          case _MenuOptions.userAgent:
            final userAgent = await widget.controller
                .runJavaScriptReturningResult('navigator.userAgent');
            if (!mounted) return;
            ScaffoldMessenger.of(context).showSnackBar(SnackBar(
              content: Text('$userAgent'),
            ));
            break;
          case _MenuOptions.javascriptChannel:
            await widget.controller.runJavaScript('''
                  var req = new XMLHttpRequest();
                  req.open('GET', "https://api.ipify.org/?format=json");
                  req.onload = function() {
                    if (req.status == 200) {
                      let response = JSON.parse(req.responseText);
                      SnackBar.postMessage("IP Address: " + response.ip);
                    } else {
                      SnackBar.postMessage("Error: " + req.status);
                    }
                  }
                  req.send();''');
            break;
        }
      },
      itemBuilder: (context) => [
        const PopupMenuItem<_MenuOptions>(
          value: _MenuOptions.navigationDelegate,
          child: Text('YouTube로 이동'),
        ),
        const PopupMenuItem<_MenuOptions>(
          value: _MenuOptions.userAgent,
          child: Text('사용자 에이전트 보기'),
        ),
        const PopupMenuItem<_MenuOptions>(
          value: _MenuOptions.javascriptChannel,
          child: Text('IP 주소 조회'),
        ),
      ],
    );
  }
}
``` 

위 코드를 실행하면 다음과 같은 결과가 나올 것입니다.

<div class="content-ad"></div>

알림: 데모 비디오에서 IP는 마스킹되지만, 이 코드가 구현되는 동안에는 여러분의 IP를 볼 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:704/1*CWyOHZ4cZF0Mv_8W72B_Ig.gif)

# 쿠키 관리

이제 팝업 메뉴를 추가하고 웹뷰에서 쿠키를 관리하는 방법을 추가하겠습니다.

<div class="content-ad"></div>

Menu 위젯 내에 아래와 같은 방식으로 쿠키 목록, 쿠키 지우기, 쿠키 추가, 쿠키 설정 및 쿠키 제거를 담당할 다섯 가지 메소드를 만들겠습니다.

```js
// 모든 쿠키 나열
  Future<void> _onListCookies(WebViewController controller) async {
    final String cookies = await controller
        .runJavaScriptReturningResult('document.cookie') as String;
    if (!mounted) return;
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(cookies.isNotEmpty ? cookies : '쿠키가 없습니다.'),
      ),
    );
  }
// 모든 쿠키 지우기
  Future<void> _onClearCookies() async {
    final hadCookies = await cookieManager.clearCookies();
    String message = '쿠키가 있었지만, 이제는 없어요!';
    if (!hadCookies) {
      message = '지울 쿠키가 없습니다.';
    }
    if (!mounted) return;
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(message),
      ),
    );
  }
// 쿠키 추가
  Future<void> _onAddCookie(WebViewController controller) async {
    await controller.runJavaScript('''var date = new Date();
  date.setTime(date.getTime()+(30*24*60*60*1000));
  document.cookie = "FirstName=John; expires=" + date.toGMTString();''');
    if (!mounted) return;
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('사용자 지정 쿠키가 추가되었습니다.'),
      ),
    );
  }
// 쿠키 설정
  Future<void> _onSetCookie(WebViewController controller) async {
    await cookieManager.setCookie(
      const WebViewCookie(name: 'foo', value: 'bar', domain: 'flutter.dev'),
    );
    if (!mounted) return;
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('사용자 지정 쿠키가 설정되었습니다.'),
      ),
    );
  }
// 쿠키 제거
  Future<void> _onRemoveCookie(WebViewController controller) async {
    await controller.runJavaScript(
        'document.cookie="FirstName=John; expires=Thu, 01 Jan 1970 00:00:00 UTC" ');
    if (!mounted) return;
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('사용자 지정 쿠키가 제거되었습니다.'),
      ),
    );
  }
```

그리고 각 메소드에 대한 팝업 메뉴도 Menu 위젯에 추가하면, 완성된 Menu 위젯은 아래와 같이 보일 것입니다.

```js
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';

enum _MenuOptions {
  navigationDelegate,
  userAgent,
  javascriptChannel,
  listCookies,
  clearCookies,
  addCookie,
  setCookie,
  removeCookie,
}
class Menu extends StatefulWidget {
  const Menu({required this.controller, Key? key});
  final WebViewController controller;
  @override
  State<Menu> createState() => _MenuState();
}
class _MenuState extends State<Menu> {
  final cookieManager = WebViewCookieManager();
  // 위와 동일한 내용
...
```

<div class="content-ad"></div>

위의 코드를 실행한 후에는 다음 출력물이 있어야 합니다

<img src="https://miro.medium.com/v2/resize:fit:704/1*HreO4EuAuM01UHUmGJn-AQ.gif" />

이렇게 하여 Flutter 애플리케이션에서 WebView를 사용할 수 있는 거의 모든 가능성을 탐색해 보았습니다.

참고:
WebView를 Flutter 앱에 추가하기 - Flutter Codelab

<div class="content-ad"></div>

이 글의 모든 소스 코드는 여기에서 찾을 수 있어요.

도움이 되었다면 이 블로그에 박수를 보내주세요. 👏👏👏👏

❤❤ 읽어주셔서 감사합니다!!! ❤❤