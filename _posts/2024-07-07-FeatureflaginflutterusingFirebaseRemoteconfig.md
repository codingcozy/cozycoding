---
title: "Firebase Remote Config를 사용한 Flutter 기능 플래그 설정 방법"
description: ""
coverImage: "/assets/img/2024-07-07-FeatureflaginflutterusingFirebaseRemoteconfig_0.png"
date: 2024-07-07 22:18
ogImage: 
  url: /assets/img/2024-07-07-FeatureflaginflutterusingFirebaseRemoteconfig_0.png
tag: Tech
originalTitle: "Feature flag in flutter using Firebase Remote config"
link: "https://medium.com/@rkishan516/feature-flag-in-flutter-using-firebase-remote-config-d602f65a22da"
isUpdated: true
---





파이어베이스는 앱 업데이트 없이 앱의 동작을 변경할 수 있는 클라우드 서비스로 원격 구성을 제공합니다. 기능 플래그는 앱 내의 기능 가용성을 제어하는 방법입니다. 기능 플래그를 사용하면 새로운 기능을 일부 사용자 또는 특정 사용자 그룹에 배포할 수 있습니다. 이는 새로운 기능을 테스트하거나 의견을 수집하거나 기능의 다른 버전을 A/B 테스트하는 데 도움이 될 수 있습니다.

<img src="https://miro.medium.com/v2/resize:fit:960/1*l2lPFZ7KWYreRpZ-VgiLGA.gif" />

시작해봅시다.

기능 플래그로 파이어베이스 원격 구성을 사용하려면 먼저 파이어베이스 콘솔에서 매개변수를 만들어야 합니다. 매개변수 값은 해당 기능이 활성화되어 있는지 여부를 제어하는 데 사용됩니다. 따라서 먼저 파이어베이스 프로젝트로 이동하여 측면 내비게이션에서 원격 구성을 선택하세요.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-07-FeatureflaginflutterusingFirebaseRemoteconfig_0.png)

이제 여기에 "매개변수 추가" 버튼을 누르세요. 그런 다음 enable_feature_x라는 매개변수를 만들어 기본값을 false로 설정할 수 있습니다. 이렇게 하면 새로운 기능이 기본적으로 비활성화됩니다. 그리고 이 구성을 저장하세요.

![이미지](/assets/img/2024-07-07-FeatureflaginflutterusingFirebaseRemoteconfig_1.png)

이제 "게시" 변경 내용을 눌러 변경 사항을 게시하세요.


<div class="content-ad"></div>


<img src="/assets/img/2024-07-07-FeatureflaginflutterusingFirebaseRemoteconfig_2.png" />

이제 콘솔 쪽은 모두 설정했으니, 플러터 쪽으로 가봅시다:-

먼저 firebase_remote_config를 pub 종속성으로 추가해야 합니다. 이를 위해 프로젝트 디렉토리의 콘솔에서 flutter pub add firebase_remote_config을 실행하세요.

이제 원격 구성을 가져오기 위해 아래 코드 조각을 사용하세요.


<div class="content-ad"></div>

```js
class RemoteConfigProvider {
  final FirebaseRemoteConfig _remoteConfig;

  RemoteConfigProvider(FirebaseRemoteConfig remoteConfig) : _remoteConfig = remoteConfig;

  Future initialise() async {
    try {
      await _remoteConfig.setDefaults(_defaults);
      await _remoteConfig.setConfigSettings(
        RemoteConfigSettings(
          fetchTimeout: const Duration(seconds: 5),
          minimumFetchInterval:
              EnvironmentConfig.PROD ? const Duration(hours: 1) : const Duration(seconds: 0),
        ),
      );
      await _remoteConfig.fetchAndActivate();
      _remoteConfig.onConfigUpdated.listen((configUpdate) {
        _remoteConfig.activate();
      });
    } on Exception catch (e, s) {
      // Track when it fails to fetch
    }
  }

  static const String _enableFeatureX = 'enable_feature_x';
  bool get enableFeatureX => _remoteConfig.getBool(_enableFeatureX);

  final Map<String, dynamic> _defaults = {
    _enableFeatureX : false,
  };
}
```

Now, let's see how to use it:-

```js
void main() async {
  final remoteConfigProvider = RemoteConfigProvider();
  await remoteConfigProvider.initialise();

  // Check the value of the `enable_feature_x` parameter.
  bool enableFeatureX = remoteConfig.enableFeatureX;

  // If the feature is enabled, show the new feature to the user.
  if (enableFeatureX) {
    // Show the new feature.
    runApp(AppWithFeature());
  } else {
    // Hide the new feature.
    runApp(AppWithoutFeature());
  }
}
```

Yaay! we did it.

<div class="content-ad"></div>

아래는 Markdown 형식의 표입니다.


| 이미지 |
|---|
| ![image](https://miro.medium.com/v2/resize:fit:996/1*QrEqCxPojo-W2pSh-YUfVg.gif) |

다음 기사에서 뵙길 바랍니다.
