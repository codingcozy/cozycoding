---
title: "플러터에서 Shorebird Code Push를 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_0.png"
date: 2024-07-09 22:23
ogImage: 
  url: /assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_0.png
tag: Tech
originalTitle: "How to implement Shorebird Code Push in Flutter?"
link: "https://medium.com/flutter-taipei/how-to-implement-shorebird-code-push-in-flutter-e65f88e80447"
---


와우! 우리는 온라인 제품을 즉시 업데이트할 수 있어요.

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_0.png)

코드 푸시(Code Push)가 무엇인가요? 이는 Over The Air 업데이트(OTA)로 불릴 수 있는데, 개발자들이 최신 버전을 온라인 애플리케이션에 즉시 푸시할 수 있게 해줍니다.

당신이 네이티브로 개발하거나 크로스 플랫폼으로 개발하더라도, 제품 리뷰 장애물에 직면하게 될 것입니다. 작은 기능 업데이트나 버그 수정이라도 공식 검토를 위해 앱 버전을 다시 제출해야 합니다. 이 과정에서 얼마나 걸릴지, 자세한 검토 단계나 새로운 통과 조건은 무엇인지 알 수 없습니다. 결과를 알기 위해 1일이나 몇 일을 기다려야 할 수 있습니다. 모두가 희망하는 것은 모바일 앱이 웹처럼 빠르게 업데이트되고 사용되어 문제를 빠르게 해결하며 사용자에게 최상의 경험을 제공하는 것입니다.

<div class="content-ad"></div>

Shorebird가 나타난 이유는 이 문제를 해결하기 위해서입니다. 지난해 팀은 Code Push를 시작했는데, 이를 통해 개발자들이 제품과 애플리케이션에 대한 OTA(Over-The-Air) 업데이트를 수행할 수 있습니다. 이는 새로운 앱 버전 코드를 사용자의 모바일 기기로 직접 전송할 수 있어서 표준적이고 복잡한 상점 검토 과정을 거치지 않아도 됩니다. 게다가, 모든 작업은 Shorebird CLI를 통해 이루어지며, 이는 CICD 도구인 Codemagic와 같은 도구들을 쉽게 통합하여 업데이트 배포 프로세스를 간소화할 수 있게 해줍니다. 매우 편리합니다.

일반 사용자들에게는 Code Push 업데이트 덕분에, 실시간으로 앱을 업데이트한 후 사용자들은 Play Store나 Apple Store를 통해 수동으로 업데이트하지 않아도 항상 최신 기능의 최신 버전을 사용할 수 있습니다.

# 왜 "Shorebird"라는 이름인가요?

Flutter의 첫 번째 사무실은 Mountain View의 Shorebird Way에 위치했기 때문입니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_1.png)

# 소개

## 인증

Shorebird를 사용하기 전에 사용자는 먼저 인증을 해야 합니다. 일반적인 OAuth 인증을 통해 유효하고 인증된 사용자만 액세스할 수 있도록 보장합니다.


<div class="content-ad"></div>

## 역할 권한

일반적으로 개발 팀 내에는 개발자와 감독관과 같은 다양한 직책이 있습니다. 우리는 패치를 생성할 권한이 있는 사람과 배포 관련 작업을 수행할 수 있는 사람을 정의할 수 있습니다.

## 즉시 업데이트

최종 사용자의 앱은 몇 분 안에 업데이트될 수 있어서, 다트 코드 변경 사항을 보장하여 모두가 최신 기능 및 최적화에 계속 액세스할 수 있습니다.

<div class="content-ad"></div>

## 쉽게 문제 해결하기

애플리케이션 문제를 신속하게 해결하고 불필요한 위험이나 스트레스를 겪지 않도록 합니다.

# 강력한 신뢰

Surebard의 코드 푸시는 Apple과 Google에서 철저히 검토 및 승인되었으며, 창립자 Eric 및 많은 잘 알려진 개발자들로부터 제품 보증을 받았습니다. 팀 멤버들은 손길이 민첩하고 활발합니다. 또한 인증되지 않은 액세스를 방지하기 위해 표준 암호화 및 인증 방법을 사용하여 최고 수준의 보안으로 애플리케이션 업데이트가 제공되도록 보장합니다.

<div class="content-ad"></div>

# 업데이트 프로세스

기본적으로 Shorebird는 앱을 실행할 때 백그라운드에서 새로운 패치를 확인하고 설치합니다. 애플리케이션의 시작 속도에 영향을 미치지 않도록 백그라운드 분리를 통해 처리됩니다. 이번에 설치된 패치는 다음에 애플리케이션을 시작할 때 사용할 수 있습니다.

# 공식문서

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_2.png)

<div class="content-ad"></div>

# Shorebird 설정

Shorebird CLI를 설치하고 터미널을 열어서 다음 명령어를 입력하세요

```js
# macOS
curl --proto '=https' --tlsv1.2 <https://raw.githubusercontent.com/shorebirdtech/install/main/install.sh> -sSf | bash
``` 

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_3.png)

<div class="content-ad"></div>

설치가 완료되면 터미널을 다시 열고 해당 명령어를 입력하세요. 로그인 링크를 클릭하여 확인하세요. 이는 일반적인 Google OAuth 로그인입니다.

```js
shorebird login
```

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_4.png)

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_5.png)

<div class="content-ad"></div>

지금은 Shorebird 환영 편지도 우체통으로 받게 될 거에요.

![Shorebird welcome letter](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_6.png)

# Shorebird 초기화

Flutter 프로젝트를 열고 설정 파일을 추가하고, 프로젝트의 루트 디렉토리에서 사용하고, 다음 명령을 실행하세요.

<div class="content-ad"></div>

```js
shorebird init
```

원본 데이터를 직접 덮어쓰기하려면 매개변수 --force를 사용하십시오.

Shorebird는 프로젝트가 소유한 플레이버 환경을 확인하고 모든 작업을 나열해줍니다. 이 작업에는 shorebird.yaml 파일을 생성하고 자산 설명을 위해 shorebird.yaml을 추가하는 것이 포함됩니다.

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_7.png)

<div class="content-ad"></div>

shorebird.yaml 파일의 app_id는 Shorebird가 애플리케이션을 식별하는 데 사용되며, 내부 정보는 비밀로 유지할 필요가 없습니다.

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_8.png)

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_9.png)

Shorebird 웹사이트에 방문하여 콘솔을 열고 프로젝트의 환경 설정을 확인하세요.

<div class="content-ad"></div>


![image](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_10.png)

# Shorebird Release

프로젝트의 릴리스 버전을 만들고 Shorebird 서버에 업로드하세요. 동시에 해당 플랫폼용 아티팩트 파일이 생성됩니다. Shorebird를 사용하면 flutter build를 대체할 shorebird release 명령을 사용할 수 있습니다.

네 가지 타겟: 


<div class="content-ad"></div>

- aar → Android 라이브러리 업로드
- android → Android 앱 업로드 (aab, apk)
- ios → iOS 앱 업로드
- ios-framework → iOS 프레임워크 업로드

```js
shorebird release <target>

# Android
shorebird release android --artifact apk
```

사용 가능한 매개변수:

- --target → 프로젝트의 주 프로그램 파일입니다. 기본값은 main.dart입니다. 물론 flavor를 지정할 수도 있습니다. 예를 들어, lib/main_dev.dart와 같이
- --flavor → 사용할 flavor 환경
- --artifact → Android에서 생성된 파일입니다. 기본값은 aab이며 apk로 변경할 수도 있습니다.

<div class="content-ad"></div>

얼음가마도 플러터 빌드와 비슷한 쇼어버드 릴리스를 할 수 있고 원본 파라미터를 수용할 수 있습니다. 반드시 두 개의 -- 구분자를 추가해주세요.

# 안드로이드 릴리스

```js
shorebird release android -- --dart-define="key=xxxxxx"
```

<img src="/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_11.png" />

<div class="content-ad"></div>

새 아티팩트 파일이 로컬에서 생성됩니다. AAB일 경우 로그에서 이 파일을 Play Store에 업로드할 수 있음을 알려줍니다. APK일 경우 시뮬레이터나 실제 기기에 직접 설치할 수 있습니다. 동시에 클라우드에 릴리스 기록도 생성됩니다.

릴리스 명령을 다시 실행하면 오류 메시지가 표시됩니다. 버전 번호를 올리고 다시 시도해주세요. 버전이 동일하기 때문입니다. 각 릴리스는 새로운 변경사항을 나타내므로 반드시 pubspec.yaml의 Flutter 버전을 수정하거나 클라우드에 있는 릴리스 기록을 삭제하여 릴리스가 성공하도록 해주세요!

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_14.png" />

# iOS 릴리스

먼저, 프로젝트의 iOS 아티팩트를 빌드하려면 shorebird release를 사용하여 일반적으로 .ipa 파일을 생성합니다.

```js
shorebird release ios --flavor dev --target ./lib/main_dev.dart
```

<div class="content-ad"></div>


<img src="/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_15.png" />

다음은 Apple Store에 배포하는 프로세스입니다. 이는 수동 데모이므로 XCode에서 처리하고 몇 가지 단계를 제공하겠습니다.

단계 1. 릴리스가 Shorebird 서버에 발행된 후 Runner.xcarchive 파일을 찾아 두 번 클릭하여 엽니다.

<img src="/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_16.png" />


<div class="content-ad"></div>

2단계. 일반적인 아카이브 프로세스이며, "앱 배포"를 선택합니다.

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_17.png)

3단계. 일반적으로 TestFlight 및 앱 스토어 옵션을 선택하지만 Shorebird는 다른 설정을 필요로 하므로 "사용자 정의"를 선택합니다.

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_18.png)

<div class="content-ad"></div>

### 단계 4. “App Store Connect”를 클릭하세요.

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_19.png)

### 단계 5. 이 단계가 가장 중요합니다. "버전 및 빌드 번호 관리"를 취소해야 하며 버전 정보를 업데이트할 필요가 없습니다.

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_20.png)

<div class="content-ad"></div>

## 단계 6. 그런 다음 올바른 개발 인증서와 프로필을 선택하여 Apple Store에 업로드하세요. 개발자들은 TestFlight를 통해 후속 테스트(패치 업데이트 포함)도 수행할 수 있습니다.

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_21.png)

## 릴리스 삭제

클라우드에 처음으로 게시된 릴리스 버전을 삭제할 수 있지만, 이 작업은 되돌릴 수 없음을 기억하세요.

<div class="content-ad"></div>

```js
해안새 릴리스 삭제

#flavor
해안새 릴리스 삭제 --flavor dev
```

# Shorebird 미리보기

해안새 릴리스가 출시된 후, 에뮬레이터 및 기기에서 새 릴리스를 미리보기할 수 있습니다. 릴리스 아티팩트 파일을 다운로드하고, 기기에 애플리케이션을 설치한 다음 애플리케이션을 실행합니다. 주로 개발 및 CI/CD 프로세스에서 사용됩니다.

```js
shorebird preview
```

<div class="content-ad"></div>

# Shorebird Patch

현재 릴리스 버전을 위한 패치를 게시하고 해당 버전을 사용하는 사용자들에게 업데이트하세요. 개발자가 패치를 릴리스하면 사용자들은 애플리케이션을 다운로드하고, 그 다음에 여는 경우 새 Dart 코드가 바로 실행됩니다.

패치는 현재 애플리케이션의 버전 번호를 변경하지 않습니다. 이는 새 버전이 아닌 기존 버전에 적용됩니다.

패치 업데이트 과정:

<div class="content-ad"></div>

- 최신 아티팩트 생성
- 해당 릴리스 아티팩트 다운로드
- 새 버전과 이전 버전 간의 차이를 확인하고 패치 아티팩트 생성
- 패치를 Shorebird 서버에 업로드
- 안정 채널로 패치를 업그레이드

```js
shorebird patch android

shorebird patch ios

# args
shorebird patch android --flavor dev --target ./lib/main_dev.dart -- --dart-define="key=xxxxxx"
```

# 플러터 개발

먼저, Shorebird 패키지 shorebird_code_push를 프로젝트에 추가하세요.

<div class="content-ad"></div>

```js
flutter pub add shorebird_code_push
```

이 패키지의 주요 API에는 다음이 포함됩니다:

- 기기가 Shorebird를 지원하는지 확인
- 현재 설치된 패치 버전 가져오기
- 다음 패치 버전 가져오기
- 다운로드할 수 있는 새 패치가 있는지 확인
- 새 패치 다운로드하기

💡 새 패치를 다운로드할 수 있는지 확인하기

<div class="content-ad"></div>

```js
shorebirdCodePush.isNewPatchAvailableForDownload()
```

💡 새 패치 다운로드하기

```js
shorebirdCodePush.downloadUpdateIfAvailable()
```

애플리케이션의 초기 단계에서 새 패치를 다운로드한 후에는 효과적으로 실행되고 새로운 버전의 Dart 코드를 실행하려면 애플리케이션을 다시 시작해야 합니다. 따라서 사용자가 업데이트가 완료되었으며 이 시점에 재시작될 것임을 상기시키는 것이 좋습니다. 사용자가 원하는 경우에만 작업을 수행하기 전에 수동으로 클릭할 수 있도록 하여 갑작스럽게 재시작하여 나쁜 경험을 유발하지 않도록 합니다.


<div class="content-ad"></div>

공식 예제를 참고하세요. 구현해야 할 주요 단계는 다음과 같습니다:

- 새로운 패치를 다운로드할 수 있는지 확인
- 새로운 패치가 있을 경우 사용자에게 업데이트할 것을 알림
- 사용자가 업데이트를 수행함
- 현재 사용자가 수행 중인 작업을 나타내는 업데이트 상태 메시지 표시
- 새로운 패치 다운로드
- 업데이트가 완료되었음을 나타내는 상태 메시지 표시, 앱을 다시 시작하여 새로운 기능을 사용할 수 있게 함
- 다시 시작은 restart_app 패키지의 도움으로 수행할 수 있습니다.

```js
final isUpdateAvailable =
      await _shorebirdCodePush.isNewPatchAvailableForDownload();
  if (!mounted) return;
  setState(() {
    _isCheckingForUpdate = false;
  });
  if (isUpdateAvailable) {
    _showUpdateAvailableBanner();
  } else {
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('업데이트 없음'),
      ),
    );
  }
}
void _showDownloadingBanner() {
  ScaffoldMessenger.of(context).showMaterialBanner(
    const MaterialBanner(
      content: Text('다운로드 중...'),
      actions: [
        SizedBox(
          height: 14,
          width: 14,
          child: CircularProgressIndicator(
            strokeWidth: 2,
          ),
        ),
      ],
    ),
  );
}
void _showUpdateAvailableBanner() {
  ScaffoldMessenger.of(context).showMaterialBanner(
    MaterialBanner(
      content: const Text('업데이트 가능'),
      actions: [
        TextButton(
          onPressed: () async {
            ScaffoldMessenger.of(context).hideCurrentMaterialBanner();
            await _downloadUpdate();
            if (!mounted) return;
            ScaffoldMessenger.of(context).hideCurrentMaterialBanner();
          },
          child: const Text('다운로드'),
        ),
      ],
    ),
  );
}
void _showRestartBanner() {
  ScaffoldMessenger.of(context).showMaterialBanner(
    const MaterialBanner(
      content: Text('새로운 패치가 준비되었습니다!'),
      actions: [
        TextButton(
          // 새로운 패치가 적용되도록 앱을 다시 시작합니다.
          onPressed: Restart.restartApp,
          child: Text('앱 다시 시작'),
        ),
      ],
    ),
  );
}
Future<void> _downloadUpdate() async {
  _showDownloadingBanner();
  await _shorebirdCodePush.downloadUpdateIfAvailable(),
  if (!mounted) return;
  ScaffoldMessenger.of(context).hideCurrentMaterialBanner();
  _showRestartBanner();
}
```

기억하세요! 기본적으로 활성 업데이트를 비활성화하려면 shorebird.yaml에서 auto_update를 false로 설정하십시오.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_22.png" />

수동 업데이트나 프로그램 코드 제어가 필요한 이유가 있습니다. 여러 이점이 있죠:

- 패치 설치를 제어할 수 있습니다 (예: 특정 계정만 업데이트, 특정 권한을 갖는 사용자, 서버 부하를 줄이거나 롤아웃 리스크 감소를 위한 AB 테스트).
- 사용자를 존중하며, 사용자에게 보안 확인을 제공하고 사용자가 적극적으로 업데이트할 수 있도록 합니다.

# 안드로이드 설정

<div class="content-ad"></div>

샸리버드 코드 푸시는 샤리버드 서버와 통신할 수 있도록 네트워크 권한을 필요로 합니다. 사용자들이 로컬 업데이트를 다운로드할 수 있도록 AndroidManifest.xml 파일을 수정해야 합니다.

```js
<manifest>
    <uses-permission android:name="android.permission.INTERNET" />
    ...
</manifest>
```

# 예시

## 안드로이드

<div class="content-ad"></div>

Dart 코드를 사용하여 업데이트를 트리거하는 방법을 보여줍니다. 먼저, 이전 버전에 대한 Shorebird 릴리스를 수행하세요. 일반적으로 이 버전은 Play Store에 릴리스된 버전일 것입니다.

지정된 환경에 대한 지침을 사용하고 Android를 데모용으로 사용하므로 여기에서 APK 파일을 선택하세요.

```js
shorebird release android --flavor dev --target lib/main_dev.dart --artifact apk
```

Android .apk 파일을 찾아서 Android 휴대폰에 설치하여 테스트하세요.

<div class="content-ad"></div>


![How to implement Shorebird Code Push in Flutter](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_23.png)

![How to implement Shorebird Code Push in Flutter](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_24.png)

휴대폰에 .apk 파일을 설치하세요. 이는 상점에서 다운로드하는 것처럼 시뮬레이션할 수 있습니다. 다음은 APP의 초기 화면입니다. 만약 현재 이 화면이나 기능에서 버그를 발견한다면, 사용자들이 겪는 문제를 수정하기 위해 Shorebird를 통해 긴급하게 패치를 릴리스해야 합니다.

이 예시에서는 버튼의 텍스트와 배경색을 조정하고 일부 코드를 변경한 후 shorebird patch 명령을 사용하여 새 버전을 게시합니다.


<div class="content-ad"></div>


shorebird patch android --flavor dev --target lib/main_dev.dart


Shorebird Cloud에서 릴리스 버전을 확인하고, 릴리스가 있으면 다운로드하여 새 코드와 결합합니다.

![How to implement Shorebird Code Push in Flutter](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_25.png)

사전 작업이 완료되면, 패치를 생성하고 생성된 아티팩트를 Shorebird Cloud에 업로드할지 묻습니다. 이때, 최종 사용자가 새로운 패치를 감지할 수 있습니다.


<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_26.png" />

패치 페이지에서는 배포에 관한 모든 정보가 표시됩니다. 패치 #2는 방금 만들어진 패치입니다.

<img src="/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_27.png" />

## iOS

<div class="content-ad"></div>

iOS 버전을 릴리스하는 것은 약간 복잡합니다. 먼저 쇼어버드 릴리스 명령을 사용하여 쇼어버드 아티팩트를 빌드하고, 그런 다음 애플 스토어에 업로드하세요.

```js
shorebird release ios --flavor prod --target lib/main_prod.dart
```

이 예시는 안드로이드와 같습니다. 버튼의 텍스트와 배경색을 조정하고, 코드를 변경한 후 shorebird 패치 명령을 사용하여 새 버전을 릴리스합니다 (여기서 필요한 것은 패치의 플러터 버전이 쇼어버드 릴리스 버전과 동일한지 확인하는 것입니다).

```js
shorebird patch ios --flavor prod --target ./lib/main_prod.dart
```

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_28.png)

# 데모

- 먼저 APK 또는 TestFlight를 통해 앱의 릴리즈 버전을 설치한 후 열어 첫 번째 버전을 표시합니다.
- Dart 코드를 변경하고 앱 초기 페이지의 버튼 구성을 변경합니다. 텍스트와 배경 색상이 포함됩니다.
- 동일한 버전을 기반으로 Shorebird 패치를 게시합니다.
- 사용자가 앱을 다시 시작하면 새로운 패치가 확인되어 다운로드되며 완료 후 앱이 다시 시작됩니다.
- 열리면 새로 패치된 앱의 새 버전이 될 것이며 새로운 Dart 코드를 사용합니다.

![이미지](https://miro.medium.com/v2/resize:fit:810/0*w27DoiOq9wXaDIIj.gif)


<div class="content-ad"></div>


![이미지](https://miro.medium.com/v2/resize:fit:830/0*XW7Bj4GI2iMm7FJ5.gif)

패치를 다운로드한 후 팀은 콘솔의 Insights 탭에서 현재 사용량 상태를 확인할 수 있습니다.

![이미지](/assets/img/2024-07-09-HowtoimplementShorebirdCodePushinFlutter_29.png)

# 확인 및 업데이트 시기


<div class="content-ad"></div>

- 기본 설정이 활성화된 경우, 사용자는 푸시 알림을 통해 알림을 받을 수 있습니다. 알림을 클릭하여 앱을 열면 Flutter 엔진이 시작되어 Shorebird가 패치를 다운로드하고 앱을 업데이트합니다.
- 수동 업데이트 및 코드 업데이트의 경우, 애플리케이션이 시작될 때 또는 실행 중일 때 주기적으로 패치를 확인하고 다운로드할 수 있습니다. 새 버전의 패치가 제공되면 다이얼로그, 스낵바, 토스트 등을 통해 사용자에게 알림을 보낼 수 있어 클릭하여 앱을 재시작하여 업데이트 가능합니다.

# Q/A

## Q1: 패치가 출시된 후 어떤 릴리즈 버전이 업데이트됩니까?

- 현재 패치는 최신 릴리즈 버전을 위한 것이므로, 사용자들은 패치를 통해 코드 푸시 업데이트를 수행하기 전에 먼저 최신 릴리즈를 설치해야 합니다.
- 이전 버전을 사용하는 사용자는 애플리케이션 내의 강제 업데이트 매커니즘을 사용하여 사용자들이 먼저 스토어의 최신 버전인 Shorebird 릴리즈 버전을 다운로드하도록 안내할 수 있습니다. 이후 애플리케이션은 나중에 패치를 통해 업데이트 될 수 있습니다.

<div class="content-ad"></div>

## Q2: Shorebird은 릴리스 버전을 관리합니다. 고객 제품에 대해 공식이 직접 코드를 푸시하는 것에 어떤 위험이 있을까요?

- 아니요, 공식은 현재 고객의 제품 코드를 볼 수 없거나 저장하지 않는 것을 보장합니다. 소스 코드 액세스 없이 "Push to Deploy"는 불가능합니다

## 공식 FAQ

# 커뮤니티

<div class="content-ad"></div>

Shorebard의 Discord 커뮤니티에 가입하여 최신 소식, 팀 진행 상황, 개발 아이디어를 알아보고 모두와 소통해 보세요.

# 해결을 기다리는 중

현재 자산 리소스 파일의 교체와 업데이트가 지원되지 않습니다 (현재 v1.0 stable이 출시되었으며 곧 처리될 예정입니다)

# 참고

<div class="content-ad"></div>

- [HeyFlutter•com](https://www.youtube.com/watch?v=D82d_gPXduY&ab_channel=HeyFlutter%E2%80%A4com)

## 기타 기사

- Flutter 2024년 1월 💙 Flutter 월간
- 개발 스킬 향상을 위해 Dart 3 사용하기. 더 많은 예제와 팁.
- Flutter 2023년 12월 💙 Flutter 월간
- Flutter 2023년 11월 💙 Flutter 월간
- Dart 3에 익숙해지며, 더 쉬운 삶 만들기!
- Flutter 3.16 및 Dart 3.2 중요 내용 요약!
- Flutter 2023년 10월 💙 Flutter 월간
- Flutter 2023년 9월 💙 Flutter 월간
- Flutter 2023년 8월 💙 Flutter 월간
- Flutter 2023년 7월 💙 Flutter 월간