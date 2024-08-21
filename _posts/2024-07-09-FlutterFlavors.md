---
title: "Flutter 프로젝트에서 Flavors 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-FlutterFlavors_0.png"
date: 2024-07-09 22:37
ogImage: 
  url: /assets/img/2024-07-09-FlutterFlavors_0.png
tag: Tech
originalTitle: "Flutter Flavors"
link: "https://medium.com/go-with-flutter/flutter-flavors-d81e1b3259c5"
isUpdated: true
---





![이미지](/assets/img/2024-07-09-FlutterFlavors_0.png)

플레이버(Flavor)란 같은 코드베이스 또는 단일 프로젝트에서 다양한 환경 또는 목적을 위한 앱의 변형이나 구성을 만드는 개념을 의미합니다. 이러한 변형에는 다양한 BASE_URL, 로고, 브랜딩, 테마, 기능, 이름 등 특정 환경에 특화된 많은 다른 설정이 모두 단일 프로젝트 내에서 포함될 수 있습니다.

Flutter에서는 다음 패키지를 사용하여 수동으로 또는 자동으로 Flavor를 생성할 수 있습니다:

- flutter_flavorizr
- flutter_flavor
- flavor_getter
- flavor_config


<div class="content-ad"></div>

수동으로 플레이버를 만드는 방법은 이미 위에서 언급했습니다. 수동 단계를 진행하려면 Android 및 iOS용으로 application_id, icon, res 파일, info.plist, target_files 등을 더 많이 정의해야 합니다.
이러한 모든 변경 사항을 피하려면 패키지를 사용하는 것이 좋습니다. 한 번의 명령으로 모든 구성을 만들어줍니다.

내가 좋아하는 패키지 중 하나는 flutter_flavorizr입니다.
flutter_flavorizr로 시작해봅시다.

# 플레이버 생성 단계

- 다음을 설치하세요:
    - Ruby
    - Gem
    - Xcodeproj (RubyGems로 설치)

<div class="content-ad"></div>

```bash
$ [sudo] gem install xcodeproj
```

2. Define flutter_flavorizr in `pubspec.yaml` file under `dev_dependencies`:

```yaml
dev_dependencies:
  flutter_flavorizr: 
```

3. Run the following command:

<div class="content-ad"></div>

```yaml
flutter pub get
```

4. Create `flavorizr.yaml` file in your Flutter project.

5. Define the flavors you need to make in `flavorizr.yaml`.

```yaml
app:
  android:
    flavorDimensions: "demoflavor"

flavors:
  dev:
    app:
      name: "Flavor Dev"

    android:
      applicationId: "com.example.flavorapp.dev"
      icon: "assets/logos/dev.png"
      resValues:
        test_id:
          type: "string"
          value: "4345927247683568~1200376308"
      firebase:
         config: ".firebase/banana/google-services.json"

    ios:
      bundleId: "com.example.flavorapp.dev"
      icon: "assets/ios/dev.png"
      variables:
        TEST_ID:
          value: "4345927247683568~3899043482"
      firebase:
         config: ".firebase/banana/google-services.json"
    macos:
      bundleId: "com.example.flavorapp.dev"
      firebase:
         config: ".firebase/banana/google-services.json"

  prod:
    app:
      name: "Flavor Prod"

    android:
      applicationId: "com.example.flavorapp.prod"
      icon: "assets/logos/prod.png"
      resValues:
        test_id:
          type: "string"
          value: "4345927247683568~1200376308"

    ios:
      bundleId: "com.example.flavorapp.ios.prod"
      icon: "assets/ios/prod.png"
      variables:
        TEST_ID:
          value: "4345927247683568~3899043482"
    macos:
      bundleId: "com.example.flavorapp.prod"
```

<div class="content-ad"></div>

위와 같이 각 플레이버에 대해 사용자 정의 구성을 손쉽게 할 수 있습니다. 한꺼번에 application_id, res_values, icons, firebase 파일, app_name 등을 모두 정의할 수 있습니다.

- Android의 res_val은 소문자여야 하며(예: test_id), iOS의 경우 대문자여야 합니다(예: TEST_ID).
- 플레이버 이름은 소문자여야 합니다.
- 로고에는 .png 파일을 넣어야 합니다.

6. 모든 플레이버를 위해 프로젝트를 초기화하려면 다음 명령을 실행하세요.

```js
flutter pub run flutter_flavorizr

// 위 명령은 flutter_flavorizr에서 모든 프로세서를 실행합니다
//flutter pub run flutter_flavorizr -p assets:download,assets:extract,android:buildGradle,android:dummyAssets,android:icons,flutter:targets,ios:xcconfig,ios:buildTargets,ios:schema,ios:dummyAssets,ios:icons,ios:plist,ios:launchScreen,macos:xcconfig,macos:configs,macos:buildTargets,macos:schema,macos:dummyAssets,macos:icons,macos:plist,google:firebase,assets:clean,ide:config
```

<div class="content-ad"></div>

아래는 flavorizr.yaml에서 정의된 각 플레이버에 대한 모든 설정을 만듭니다.

7. 가정해 봅시다, Android 및 iOS용 플레이버를 정의하려고 합니다. 다음과 같이 flutter_flavorizr 프로세서를 실행할 수 있어요.

```bash
flutter pub run flutter_flavorizr -p <processor_1>,<processor_2>
```

```bash
// Android 및 iOS 용
flutter pub run flutter_flavorizr -p assets:download,assets:extract,android:buildGradle,android:dummyAssets,android:icons,flutter:targets,ios:xcconfig,ios:buildTargets,ios:schema,ios:dummyAssets,ios:icons,ios:plist,ios:launchScreen,google:firebase,assets:clean,ide:config
```

<div class="content-ad"></div>

8. 앱을 빌드하기 위해 다음 명령을 실행하세요:

```js
flutter run -t lib/main_dev.dart --debug --flavor=dev
flutter run -t lib/main_dev.dart --release --flavor=dev
flutter build apk --release --flavor dev -t lib/main_dev.dart
flutter build appbundle --release --flavor dev -t lib/main_dev.dart
flutter build appbundle --build-name=2.1.5 --build-number=20 --release --flavor dev -t lib/main_dev.dart
```

flutter_flavorizr을 사용하여 Flavor를 만드는 방법입니다. 어떠한 피드백도 환영합니다.
제출하실 수 있는 기사들은 여기에서 제출해주세요:

<div class="content-ad"></div>

제 소셜 미디어 계정을 팔로우해 주세요:
   
- LinkedIN
- YouTube
- Google DevLibrary
- Substack
- GitHub

"Make Yourself The Software Developer"
곧 더 많은 소식을 전합니다.
감사합니다 🙂