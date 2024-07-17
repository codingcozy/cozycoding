---
title: "Flutter 애플리케이션에 Unity 통합하는 방법"
description: ""
coverImage: "/assets/img/2024-07-06-IntegratingUnityintoaFlutterApplication_0.png"
date: 2024-07-06 02:43
ogImage: 
  url: /assets/img/2024-07-06-IntegratingUnityintoaFlutterApplication_0.png
tag: Tech
originalTitle: "Integrating Unity into a Flutter Application"
link: "https://medium.com/@dipakbhatupawar/integrating-unity-into-a-flutter-application-f9f1f1940c38"
---


# 소개

유니티와 플러터를 통합하면, 유니티의 강력한 3D 렌더링 및 AR 포털 기능과 플러터의 다재다능한 UI 프레임워크가 결합됩니다. 본 안내서는 이 통합을 달성하기 위한 단계를 안내해 드립니다.

# 준비 사항

- 유니티 설치 (버전 2022 이상)
- 안드로이드 스튜디오 설치 (2023 이상)
- 플러터 설치 (버전 3.0 이상)
- 유니티와 플러터의 기본 지식

<div class="content-ad"></div>

# 단계 1:- 플러터 프로젝트 설정

- 새로운 플러터 프로젝트를 생성하세요.
- 프로젝트 레벨에 새로운 디렉토리(폴더)를 생성하세요.

# 단계 2:- 유니티 프로젝트 설정

- 새로운 유니티 프로젝트를 생성하세요.

<div class="content-ad"></div>

/assets/img/2024-07-06-IntegratingUnityintoaFlutterApplication_0.png

# Step 3: 유니티 프로젝트에 플러터 통합

- 유니티 프로젝트 열기
- 처음에는 유니티 프로젝트에서 플러터 탭을 보지 못합니다. 플러터-유니티 패키지를 가져와야 합니다. 이 패키지는 flutter_unity_package 웹사이트에서 다운로드할 수 있습니다.
- flutter_unity_package을 프로젝트에 가져오려면 다음 단계를 따르세요:
1. 에셋을 우클릭하여 `패키지 가져오기` -> `사용자 지정 패키지`를 선택합니다.
2. 다운로드한 flutter_unity_package을 선택하고 가져오기를 클릭합니다.
- 유니티 프로젝트를 플러터 프로젝트 내의 유니티 디렉토리로 복사합니다. (중요)
- 이제 플러터 프로젝트 내의 유니티 디렉토리에서 복사한 프로젝트를 열어주세요.(1단계에서)

# Step 4: 유니티 프로젝트 빌드하기

<div class="content-ad"></div>

- 유니티에서 기본 씬이나 AR 포털을 만들어보세요.
- Android 및 iOS를 위한 빌드 방법은 다음과 같습니다:

![image](/assets/img/2024-07-06-IntegratingUnityintoaFlutterApplication_1.png)

- Android:
1. Platform을 Android로 선택하기 위해: 파일`빌드 설정`으로 이동합니다.
     
![image](/assets/img/2024-07-06-IntegratingUnityintoaFlutterApplication_2.png)

<div class="content-ad"></div>

2. 플레이어 설정 수정: 플레이어 설정 `기타 설정`에서 스크립팅 백엔드를 IL2CPP로 변경하고, ARMv7 및 ARM64를 체크 상태로 선택하십시오. 아래 이미지 참조하세요.

![Player Settings](/assets/img/2024-07-06-IntegratingUnityintoaFlutterApplication_3.png)

3. Active Input Handling을 Input Manager(Old) 또는 Both로 설정하세요.

4. ARCore나 기타 기능을 사용 중이라면 필요한 변경사항을 수행하세요.

<div class="content-ad"></div>

iOS:

- iOS로 플랫폼을 선택하세요.
- 앱을 실행할 위치에 따라 Target SDK를 변경하기 위해 플레이어 설정을 열어주세요.

# 단계 5: Flutter 프로젝트용 Unity 빌드 생성하기

Unity에서 Flutter 탭으로 이동하여 Android 또는 iOS와 같은 플랫폼을 위한 빌드를 생성하세요.

<div class="content-ad"></div>

안녕하세요,

Markdown 형식으로 테이블 태그를 변경하면 아래와 같이 나타납니다.


| Android |
| ------- |
| /assets/img/2024-07-06-IntegratingUnityintoaFlutterApplication_4.png |

1. 이 단계는 안드로이드 디렉토리(폴더) 하위에 Flutter 프로젝트에서 unityLibray를 자동으로 생성합니다.

2. `unityLibrary`의 `build.gradle` 파일에 따라 `build.gradle` (앱 수준) 파일을 수정하십시오. `compileSdkVersion`, `minSdkVersion`, `targetSdkVersion`와 같은 설정을 업데이트하십시오.


<div class="content-ad"></div>

/assets/img/2024-07-06-IntegratingUnityintoaFlutterApplication_5.png

iOS:

- 유니티에서 iOS 빌드 생성.
- info.plist 파일에서 카메라 및 기타 필요한 권한 부여.

pub.dev의 flutter_unity_widget 패키지 사용하여 Unity 프로젝트를 flutter 앱에서 실행합니다. 플러터 프로젝트의 메일 파일에 샘플 예제를 복사하고 Android 및 iOS용 애플리케이션을 실행하세요.

<div class="content-ad"></div>

행복한 글쓰기!

질문이나 지원이 필요하시면 dipakbhatupawar@gmail.com으로 연락해주세요.