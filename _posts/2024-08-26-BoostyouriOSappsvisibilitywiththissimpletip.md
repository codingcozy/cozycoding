---
title: "iOS 앱 홍보를 위한 간단한 팁으로 앱의 가시성 향상하기"
description: ""
coverImage: "/assets/img/2024-08-26-BoostyouriOSappsvisibilitywiththissimpletip_0.png"
date: 2024-08-26 19:35
ogImage: 
  url: /assets/img/2024-08-26-BoostyouriOSappsvisibilitywiththissimpletip_0.png
tag: Tech
originalTitle: "Boost your iOS apps visibility with this simple tip"
link: "https://medium.com/@Sergey.Zhuravel/boost-your-ios-apps-visibility-with-this-simple-tip-7832e3898c2b"
isUpdated: false
---


![Boost your iOS app's visibility with this simple tip](/assets/img/2024-08-26-BoostyouriOSappsvisibilitywiththissimpletip_0.png)

안녕하세요! 여러분! 제 이름은 Sergey입니다. 저는 Futurra Group에서 iOS 팀 리드로 6년 이상 일하고 있습니다. 이 글에서는 여러분의 앱 가시성을 높이고 사용률을 향상시킬 수 있는 간단한 팁을 공유하겠습니다. iOS 앱을 더 쉽게 발견할 수 있는 간단하면서도 강력한 기술을 발견해보세요.

이 팁은 Info.plist의 kMDItemKeywords 키와 관련이 있습니다.

kMDItemKeywords는 iOS 및 macOS 시스템에서 사용되는 메타데이터 속성으로, 파일이나 문서와 관련된 키워드를 저장하는 데 사용됩니다. 이러한 키워드는 시스템 내의 파일을 검색하고 구성하는 데 사용될 수 있으며, iOS의 앱 라이브러리나 macOS의 Spotlight 및 기타 검색 도구를 통해 파일을 쉽게 찾을 수 있게 합니다.

<div class="content-ad"></div>

예를 들어, kMDItemKeywords를 사용하여 파일에 키워드를 추가하면 해당 키워드를 검색란에 입력하여 쉽게 해당 파일을 찾을 수 있습니다. 이 속성은 이미지, 문서 또는 기타 파일의 메타데이터 컨텍스트에서 종종 사용되며 분류 및 조직을 개선하는 데 사용됩니다.

이것은 Apple의 공식 문서에서는 찾을 수 없을 것입니다 (적어도 iOS에 대해서는). iOS용 앱 라이브러리에 앱이 디스플레이 이름만이 아니라 여러 이름으로 나타나도록하려면 다음 단계를 따르세요.

1. 앱의 Info.plist 업데이트:

- kMDItemKeywords라는 새 키 추가
- 타입을 문자열로 설정

<div class="content-ad"></div>

2. 키워드 설정:

- 앱을 검색할 때 사용할 대체 이름 목록을 쉼표로 구분하여 입력하세요. 이는 이전 앱 이름이나 경쟁 앱의 이름을 포함할 수 있습니다.

![Boost your iOS app's visibility with this simple tip](/assets/img/2024-08-26-BoostyouriOSappsvisibilitywiththissimpletip_1.png)

3. 변경 사항 테스트하기:

<div class="content-ad"></div>

- 중요: 변경 사항이 적용되도록단말기/시뮬레이터에서 이전 앱 버전을 제거한 후 새로 설치해주세요.
- 당신의 장치나 시뮬레이터에 앱을 설치해주세요.

## 작동 방식

저희 수학 문제 해결 앱 MathMaster를 사용 예시로 설명 드리겠습니다. 아래 스크린샷에서 저는 경쟁사 앱인 "Microsoft Math"를 검색에 입력했고, 결과를 보시다시피 검색 결과에는 해당 앱만 나타났습니다.

![매체](/assets/img/2024-08-26-BoostyouriOSappsvisibilitywiththissimpletip_2.png)

<div class="content-ad"></div>

이제 kMDItemKeywords 필드를 값 "Microsoft Math"로 추가할 거에요. 아래 스크린샷에서 보는 것처럼, 이제 "Microsoft Math" 키워드 검색 결과에 우리 앱이 나타납니다.

<img src="/assets/img/2024-08-26-BoostyouriOSappsvisibilitywiththissimpletip_3.png" />

연락처: 내 트위터, 링크드인