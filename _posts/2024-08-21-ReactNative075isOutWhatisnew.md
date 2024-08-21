---
title: "React Native 0.75에 새로 추가된 기능 정리"
description: ""
coverImage: "/assets/img/2024-08-21-ReactNative075isOutWhatisnew_0.png"
date: 2024-08-21 17:28
ogImage: 
  url: /assets/img/2024-08-21-ReactNative075isOutWhatisnew_0.png
tag: Tech
originalTitle: "React Native 0.75 is Out. What is new"
link: "https://medium.com/javascript-in-plain-english/react-native-0-75-is-out-what-is-new-1bf103963255"
isUpdated: true
updatedAt: 1724245507729
---


<img src="/assets/img/2024-08-21-ReactNative075isOutWhatisnew_0.png" />

# 소개

React Native 0.75의 릴리스는 이 인기 있는 프레임워크의 발전을 한 단계 더 나아가게 했습니다. 새로운 기능과 중요한 업데이트를 통해 성능, 개발자 경험 및 호환성을 향상시켰습니다.

여기에서는 이 버전에서의 주요 변경 사항을 자세히 살펴보며, 여러분의 프로젝트에서 이 개선 사항을 활용할 수 있는 통찰력을 제공하겠습니다.

<div class="content-ad"></div>

# 용어 및 정의

새로운 내용을 살펴보기 전에, 이번 업데이트를 이해하는 데 필수적인 기술 용어들을 되짚어봅시다.

## 기본으로 설정된 Hermes

Hermes가 이제 React Native 0.75에서 기본 JavaScript 엔진으로 설정되었습니다. Hermes는 React Native 앱의 성능을 최적화하기 위해 설계되었으며, 앱의 시작 시간과 메모리 사용량을 줄여 부드러운 사용자 경험을 제공하는 데 중요합니다. 이 변경으로 인해 개발자들의 설정이 간단해지며, Hermes가 React Native 빌드 프로세스에 직접 통합되어 있습니다.

<div class="content-ad"></div>

## 리액트 네이티브에서의 요가

앱 레이아웃의 규칙과 측정테이프로 요가를 상상해보세요. C++로 작성된 요가는 플렉스박스 지시사항을 해석하고 UI 요소의 정확한 위치를 계산합니다. 이를 통해 다양한 기기에서 일관된 시각적으로 매력적인 디자인을 보장합니다.

## Flexbox 갭 속성 지원

리액트 네이티브 0.75에서는 플렉스박스 레이아웃 내에서 갭 속성을 지원합니다. 이 추가로 플렉스 컨테이너 내 요소들 간의 간격을 단순하게 만들어줍니다. 이로써 복잡한 레이아웃을 디자인하기 쉬워지며 번거로운 마진 속성에 의존할 필요가 없어집니다.

<div class="content-ad"></div>

## Yarn

Yarn을 여러분의 프로젝트 사서로 생각해보세요. 앱이 정상적으로 작동하려면 필요한 필수 라이브러리(의존성)를 꼼꼼하게 관리합니다. 설치, 업데이트 및 제거를 간편하게 처리하여 프로젝트를 체계적이고 효율적으로 유지합니다.

## Android SDK

구글에서 제공하는 이 도구 모음을 사용하면 필요한 도구와 리소스를 제공받아 훌륭한 안드로이드 애플리케이션을 개발할 수 있습니다.

<div class="content-ad"></div>

## 향상된 TurboModules

이번 릴리스에서는 TurboModules에 대한 추가 최적화가 도입되었습니다. TurboModules는 네이티브 모듈을 로드하고 실행하는 데 관련된 오버헤드를 줄여 성능을 향상시키는 데 사용됩니다. 0.75 버전에서 이러한 개선 사항은 JavaScript와 네이티브 코드 간의 훨씬 더 빠른 통신을 의미하며, 더 반응적인 앱을 제공합니다.

## 병행 렌더링 개선

React Native 0.75 버전은 병행 렌더링을 계속해서 개선하고 있습니다. 이 기능은 React가 여러 작업을 동시에 처리할 수 있도록 하여 무거운 작업 중에도 반응성을 향상시킵니다. 이러한 개선 사항은 앱이 복잡한 작업을 수행하는 동안에도 앱이 부드럽고 상호 작용적이라도록 보장합니다.

<div class="content-ad"></div>

# 주요 변경 사항

1- 사용 중단된 API가 제거되었습니다.

2- PushNotificationIOS API 업데이트가 이루어졌습니다.

3- Android API 레벨 요구 사항이 상승되었습니다.

<div class="content-ad"></div>

# 결론

React Native 0.75는 어플리케이션 성능을 향상시키고 개발 프로세스를 간단화하는 중요한 업데이트를 소개합니다.

기본 엔진으로 Hermes를 사용하고, Flexbox gap 속성을 지원하는 TurboModules를 향상시키며, 개선된 동시 렌더링을 제공하여 이번 릴리스는 더 빠르고 효율적인 앱을 개발하는 데 도움이 됩니다.

또한, 폐기된 API를 제거하고 Android SDK 레벨을 높이는 등, React Native는 현대적이고 간소화된 프레임워크로 발전하고 있습니다. 이러한 개선 사항을 최대한 활용하기 위해 프로젝트를 업데이트하는 것을 권장합니다.

<div class="content-ad"></div>

React Native의 최신 버전에 대한 자세한 내용은 CasaInnov에서 제공하는 이 기사나 공식 릴리스 노트를 확인해보세요.

React Native 버전을 최신 안정 버전으로 업그레이드해야 할 경우 이 서비스를 확인해보세요: https://casainnov.com/mobiledevelopment.

또한, 회사는 Edge 기술 개발 (AI, Web3 등), 컨설팅 및 개발과 같은 다양한 기술 서비스를 제공합니다. 확인해보세요: https://casainnov.com/

React Native, React 및 TypeScript에 관한 통찰을 공유하고 있습니다. Linkedin이나 Medium에서 제 팔로우를 해주세요.

<div class="content-ad"></div>

# 쉽게 이해할 수 있는 영어 커뮤니티에 참여해 주셔서 감사합니다! 이제 가시기 전에:

- 작가를 추천하고 팔로우해 주세요 ️👏️️
- 저희를 팔로우하세요: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼도 방문해 보세요: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 만나보세요