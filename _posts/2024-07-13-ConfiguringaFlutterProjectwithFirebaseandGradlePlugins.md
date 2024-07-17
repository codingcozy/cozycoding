---
title: "Flutter 프로젝트에서 Firebase 및 Gradle 플러그인 설정하는 방법"
description: ""
coverImage: "/uidev-css.github.io/assets/no-image.jpg"
date: 2024-07-13 21:33
ogImage: 
  url: /uidev-css.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Configuring a Flutter Project with Firebase and Gradle Plugins"
link: "https://medium.com/@mnc12004/configuring-a-flutter-project-with-firebase-and-gradle-plugins-7e2ef7e898cf"
---


몇 분은 ~% flutter build apk 또는 appbundle을 실행할 때 이 메시지를 보았을 수 있습니다.

# Deprecated imperative apply of Flutter’s Gradle plugins

```js
flutter needs upgrade: You are applying Flutter's 
app_plugin_loader Gradle plugin imperatively using the apply script method, 
which is deprecated and will be removed in a future release. 
Migrate to applying Gradle plugins with the declarative 
plugins block: https://flutter.dev/go/flutter-gradle-plugin-apply
```

많은 시행착오 끝에, ./gradlew clean을 실행할 때 Gradle 파일이 오류 없이 컴파일되는 해결책을 마침내 찾았습니다.

<div class="content-ad"></div>

참고: Flutter Gradle 플러그인 적용

여기에 작업 방법이 있어요!

## 이 튜토리얼은 필수 Gradle 플러그인과 Firebase 종속성을 구성하여 오류 없이 성공적으로 설정할 수 있도록 Flutter 프로젝트를 안내합니다.

## 다루는 주제:

<div class="content-ad"></div>

- Android/build.gradle 파일 설정하기
- Android/app/build.gradle 파일 구성하기
- settings.gradle에서 플러그인 관리하기
- 리소스 축소 및 최소화 활성화하기
- ProGuard 규칙이 적용되어 있는지 확인하기

# 요구 사항:

- Flutter 및 안드로이드 개발의 기본적인 이해.
- Flutter SDK가 설치되어 있어야 합니다.
- 기존의 Flutter 프로젝트가 있어야 합니다.

# 단계 1: Android/build.gradle을 구성하기

<div class="content-ad"></div>

- 플러터 프로젝트를 열고 android 디렉토리로 이동하세요.
- android/build.gradle 파일을 편집하여 필요한 저장소를 포함하고 Kotlin 버전을 정의하세요.

```js
allprojects {
    repositories {
        google()
        mavenCentral()
    }
}

rootProject.buildDir = "../build"
subprojects {
    project.buildDir = "${rootProject.buildDir}/${project.name}"
}
subprojects {
    project.evaluationDependsOn(":app")
}

tasks.register("clean", Delete) {
    delete rootProject.buildDir
}
```

# 단계 2: android/app/build.gradle 구성

- android/app/build.gradle로 이동하세요.
- 필요한 플러그인을 적용하고 Android 빌드 설정, 종속성 및 서명 구성을 구성하세요.

<div class="content-ad"></div>

```js
plugins {
    id "com.android.application"
    id "kotlin-android"
    // 안드로이드 및 코틀린 Gradle 플러그인을 적용한 후에는 플러터 Gradle 플러그인을 적용해야 합니다.
    id "dev.flutter.flutter-gradle-plugin"
}

def localProperties = new Properties()
def localPropertiesFile = rootProject.file("local.properties")
if (localPropertiesFile.exists()) {
    localPropertiesFile.withReader("UTF-8") { reader ->
        localProperties.load(reader)
    }
}

def flutterVersionCode = localProperties.getProperty("flutter.versionCode")
if (flutterVersionCode == null) {
    flutterVersionCode = "1"
}

def flutterVersionName = localProperties.getProperty("flutter.versionName")
if (flutterVersionName == null) {
    flutterVersionName = "1.0"
}

def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android {
    namespace = "com.example.your_app"
    compileSdk = flutter.compileSdkVersion
    ndkVersion = flutter.ndkVersion

    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }

    defaultConfig {
        // TODO: 본인의 고유한 응용 프로그램 ID를 지정합니다 (https://developer.android.com/studio/build/application-id.html).
        applicationId = "com.example.your_app"
        // 다음 값을 업데이트하여 애플리케이션 요구 사항에 일치시킬 수 있습니다.
        // 자세한 정보는 다음을 참조하세요: https://docs.flutter.dev/deployment/android#reviewing-the-gradle-build-configuration.
        minSdk = 24
        targetSdk = 34
        versionCode = flutterVersionCode.toInteger()
        versionName = flutterVersionName
    }

    signingConfigs {
        release {
            if (keystorePropertiesFile.exists()) {
                keyAlias keystoreProperties['keyAlias']
                keyPassword keystoreProperties['keyPassword']
                storeFile file(keystoreProperties['storeFile'])
                storePassword keystoreProperties['storePassword']
            }
        }
    }

    buildTypes {
        release {
            // TODO: 릴리스 빌드용 사용자 지정 서명 구성을 추가합니다.
            // 현재는 디버그 키로 서명하여 `flutter run --release`가 작동합니다.
            signingConfig = signingConfigs.debug
        }
    }
}

flutter {
    source = "../.."
}
```

### 단계 3: settings.gradle에서 플러그인 관리

- android/settings.gradle로 이동합니다.
- pluginManagement 블록에서 플러그인 버전을 정의합니다.

```js
pluginManagement {
    def flutterSdkPath = {
        def properties = new Properties()
        file("local.properties").withInputStream { properties.load(it) }
        def flutterSdkPath = properties.getProperty("flutter.sdk")
        assert flutterSdkPath != null, "local.properties에 flutter.sdk가 설정되지 않았습니다"
        return flutterSdkPath
    }()

    includeBuild("$flutterSdkPath/packages/flutter_tools/gradle")

    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
    }
}

plugins {
    id "dev.flutter.flutter-plugin-loader" version "1.0.0"
    id "com.android.application" version "7.3.0" apply false
    id "org.jetbrains.kotlin.android" version "1.7.10" apply false
}

include ":app"
```

<div class="content-ad"></div>

# 단계 4: ProGuard 규칙이 올바르게 설정되었는지 확인해주세요

- 안드로이드 앱 디렉토리에 proguard-rules.pro 파일이 있는지 확인해주세요.
- 파일이 없는 경우 다음과 같은 기본 내용으로 생성해주세요:

```js
# 프로젝트별 ProGuard 규칙을 여기에 추가하십시오.
# 이 파일의 플래그는 기본적으로 /usr/local/share/android-sdk/tools/proguard/proguard-android.txt에 지정된 플래그에 추가됩니다.
# build.gradle에서 proguardFiles 지시문을 수정하여 포함 경로와 순서를 수정할 수 있습니다.

# 자세한 내용은 아래 링크를 참조해주세요:
#   http://developer.android.com/guide/developing/tools/proguard.html

# 프로젝트별 보관할 옵션을 여기에 추가하십시오:
```

# 단계 5: 설정 검증하기

<div class="content-ad"></div>

- 프로젝트 정리: android 디렉토리로 이동하여 다음을 실행해 주세요:

```js
./gradlew clean
```

2. 프로젝트 빌드: 프로젝트 루트 디렉토리로 이동하여 다음을 실행해 주세요:

```js
flutter build apk
```

<div class="content-ad"></div>

3. 앱 실행하기: 

```js
flutter run
```

위 단계를 따르면 Flutter 프로젝트가 필요한 Gradle 플러그인 및 Firebase 종속성이 올바르게 구성되어 오류없이 실행됩니다. 문제가 발생하면 각 단계를 다시 확인하고 모든 구성이 올바른지 확인해주세요.