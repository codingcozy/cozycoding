---
title: "플러터에서 비밀번호 저장 및 자동 완성하는 방법"
description: ""
coverImage: "/assets/img/2024-06-21-SavePasswordAutofillPasswordinFlutter_0.png"
date: 2024-06-21 22:56
ogImage: 
  url: /assets/img/2024-06-21-SavePasswordAutofillPasswordinFlutter_0.png
tag: Tech
originalTitle: "Save Password , Autofill Password in Flutter"
link: "https://medium.com/@1998.singh.amarjit/save-credentials-autofill-credentials-in-flutter-33058295dd9d"
isUpdated: true
---





## 로그인할 때마다 로그인 자격 증명을 입력하는 것보다 더 쉬운 게 있을까요? 자동 입력 자격 증명이 여러분이 찾던 프로세스입니다!

이 기사를 읽고 나서 기대할 수 있는 것들은 다음과 같습니다:

![이미지](/assets/img/2024-06-21-SavePasswordAutofillPasswordinFlutter_0.png)

![이미지](/assets/img/2024-06-21-SavePasswordAutofillPasswordinFlutter_1.png) 

<div class="content-ad"></div>

# 안드로이드

안드로이드 파트는 매우 간단합니다. Android 모듈에서 구성을 설정할 필요가 없습니다.

# iOS

iOS 모듈의 경우, 작업을 완료하기 위해 약간의 노력이 필요합니다.

<div class="content-ad"></div>

그럼 시작해 봅시다!

이미 플러터 프로젝트를 생성했다고 가정합니다.

### 단계 1

먼저 XCode에서 iOS 모듈을 엽니다. 그런 다음 "Signing & Capabilities(서명 및 기능)" 섹션으로 이동합니다. "AutoFill Credential Provider(자동 완성 자격 증명 제공자)" 및 "Keychain Sharing(키체인 공유)" 기능을 추가하세요.

<div class="content-ad"></div>

단계 2

"Keychain Sharing" 기능을 위해 프로젝트에 키 체인 그룹을 추가하세요. 그룹은 번들 식별자가 될 수 있습니다. 아래 그림 1을 참조하세요.

![그림 1](/assets/img/2024-06-21-SavePasswordAutofillPasswordinFlutter_2.png)

단계 3

<div class="content-ad"></div>

다음으로, 기기의 키체인에 사용자 자격 증명을 저장하기 위해 사용될 "연결된 도메인" 기능을 추가해주세요.

**단계 4**

"연결된 도메인" 기능을 위해 webcredentials 및 applinks에 대한 도메인을 추가해야 합니다. 도면 2에 나와있는 도메인을 프로젝트의 실제 도메인으로 대체해야 합니다.

<div class="content-ad"></div>

혹시 iOS 기기의 키체인에서 사용자의 자격 증명을 앱과 관련시키기 위해 이 도메인을 사용하려고 합니다.

**단계 5**

이제 Apple 앱 사이트 연결을 위해 관련 도메인을 호스팅해야 합니다. 이를 위해 JSON 형식의 앱 데이터가 포함된 파일을 생성해야 합니다.

```js
{
  "applinks": {
      "details": [
           {
             "appIDs": [ "{teamID}.{bundleID}", "{teamID}.{bundleID}"],
             "components": [
               {
                  "#": "no_universal_links",
                  "exclude": true,
                  "comment": "fragment 값이 no_universal_links인 URL을 모두 일반 링크로 열지 않도록 시스템에 지시합니다."
               },
               {
                  "/": "/buy/*",
                  "comment": "경로가 /buy/로 시작하는 모든 URL과 일치합니다."
               },
               {
                  "/": "/help/website/*",
                  "exclude": true,
                  "comment": "/help/website/로 시작하는 경로를 가진 URL을 모두 일반 링크로 열지 않도록 시스템에 지시합니다."
               },
               {
                  "/": "/help/*",
                  "?": { "articleNumber": "????" },
                  "comment": "경로가 /help/로 시작하고 'articleNumber'라는 쿼리 항목이 있으며 값이 정확히 네 자리인 URL을 모두 일치시킵니다."
               }
             ]
           }
       ]
   },
   "webcredentials": {
      "apps": [ "{teamID}.{bundleID}" ]
   },


    "appclips": {
        "apps": ["{teamID}.{bundleID}"]
    }
}
```

<div class="content-ad"></div>

NOTE: 프로젝트의 팀 ID로 'teamID' 및 앱의 번들 ID로 'bundleID'를 교체해야 합니다.

**단계 6**

파일을 호스팅한 후에 아래 제공된 링크를 사용하여 파일이 성공적으로 호스팅되었는지 확인할 수 있습니다. 페이지가 성공적으로 로드되면 정상적으로 진행됩니다.

[https://'your domain'/.well-known/assetlinks.json](https://'your domain'/.well-known/assetlinks.json)

<div class="content-ad"></div>

현재 앱 구성이 완료되었습니다.

# FLUTTER

단계 1

<div class="content-ad"></div>

거의 다 왔어요. 이제 로그인 화면을 열고 텍스트 필드를 Column 위젯으로 감싸세요. 그리고 해당 "Column" 위젯을 "AutofillGroup" 위젯으로 감싸세요.

단계 2

사용자 이름을 입력하는 텍스트 필드와 비밀번호를 입력하는 또 다른 텍스트 필드 두 개가 있다고 가정합니다. 사용자 이름 텍스트 필드에 AutofillHints.username을 설정한 후, 비밀번호 텍스트 필드에 AutofillHints.password을 설정하세요. 아래 그림과 같이 말이죠. 

![image](/assets/img/2024-06-21-SavePasswordAutofillPasswordinFlutter_4.png)

<div class="content-ad"></div>

알림: iOS와 Android에 따라 저장된 비밀번호 프로세스가 다르게 작동됩니다.

이제 빌드를 정리하고 실행하세요. 사용자가 사용자 이름과 비밀번호를 입력하고 텍스트 필드를 제출하면, 비밀번호 저장 프롬프트가 나타납니다. 그리고 다음에 다시 로그인을 시도하면, 이전에 추가된 자격증명을 자동으로 채울 것을 제안받게 됩니다.

즐거운 코딩되세요...