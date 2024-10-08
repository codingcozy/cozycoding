---
title: "Flutter 개발 초보자를 위한 Bare Bones Flutter 사용법"
description: ""
coverImage: "/assets/img/2024-06-27-JumpstartYourFlutterDevelopmentwithBareBonesFlutter_0.png"
date: 2024-06-27 18:27
ogImage: 
  url: /assets/img/2024-06-27-JumpstartYourFlutterDevelopmentwithBareBonesFlutter_0.png
tag: Tech
originalTitle: "Jumpstart Your Flutter Development with Bare Bones Flutter"
link: "https://medium.com/codex/jumpstart-your-flutter-development-with-bare-bones-flutter-a6592fda9d84"
isUpdated: true
---





<img src="/assets/img/2024-06-27-JumpstartYourFlutterDevelopmentwithBareBonesFlutter_0.png" />

휴대폰 앱 개발의 바쁜 세계에서는 효율성과 속도가 가장 중요합니다. 마음대로 확장 가능하고 유지보수 가능한 코드베이스를 지향하는 경우에는 Flutter 프로젝트를 처음부터 시작하는 데 시간이 많이 걸릴 수 있습니다. 이때 Bare Bones Flutter가 출동합니다. 바닥에서 뛰어오르는 번거로움 없이 시작하고자 하는 Flutter 개발자들을 위해 만들어진 종합적인 템플릿입니다.

# Bare Bones Flutter란 무엇인가요?

Bare Bones Flutter는 Flutter 개발자를 위해 특별히 설계된 템플릿 프로젝트입니다. 안정성을 보장하고 개발 프로세스를 가속화하기 위한 견고한 기반을 제공하여 출발점으로서 역할합니다. 단일 또는 복잡한 다수의 기능을 가진 앱을 작성하는 경우, Bare Bones Flutter는 빠르게 시작할 수 있도록 필요한 도구와 구조를 제공합니다.

<div class="content-ad"></div>

# 왜 Bare Bones Flutter를 선택해야 하는가?

Bare Bones Flutter 템플릿은 앱의 아키텍처와 로직에 중점을 두고 있습니다. 사용자 인터페이스가 아닌 것이죠. 이것은 고의적인 선택입니다. 템플릿을 간단하게 유지하고 핵심 기능에 집중함으로써, 개발자가 프로젝트를 자신의 특정한 요구사항과 선호도에 쉽게 적응할 수 있도록 보장합니다. 이 유연성은 사용자 지정 UI 구성 요소나 추가 기능을 프로젝트에 업데이트하고자 하는 개발자에게 중요합니다. Bare Bones Flutter 템플릿은 간단하면서도 견고한 기반을 제공하여 필요에 따라 기능을 쉽게 추가하거나 제거할 수 있도록 합니다.

# 주요 기능

## 로컬라이제이션

<div class="content-ad"></div>

글로벌 도달은 현대 애플리케이션의 중요한 측면입니다. Bare Bones Flutter는 다국어(EN, TR)를 지원하여 다양한 대상에 맞게 쉽게 제작할 수 있습니다. 템플릿에는 로컬라이제이션 파일이 포함되어 있어 앱이 전 세계 사용자와 효과적으로 소통할 수 있습니다. MakeFile을 사용하여 더 많은 언어를 쉽게 만들 수 있습니다:

```js
make -f Makefile localization
```

## MVVM 아키텍처

유지보수성은 소프트웨어 개발에서 중요한 고려 사항입니다. Bare Bones Flutter는 Model-View-ViewModel (MVVM) 아키텍처를 채택하여 코드베이스가 깔끔하고 조직적이 되도록 지원합니다. 이 아키텍처는 그래픽 사용자 인터페이스의 개발을 비즈니스 논리 또는 백엔드 논리(모델)와 분리하여 코드를 더 효과적이고 확장 가능하게 만듭니다.

<div class="content-ad"></div>

## 인증 페이지

사용자 인증은 대부분의 앱에서 핵심 기능입니다. 이 템플릿에는 미리 구축된 로그인 및 가입 페이지가 포함되어 있습니다. 텍스트 폼 필드에는 정규 표현식을 사용하여 유효성이 검증됩니다. 이 설정은 개발 시간을 절약할 뿐만 아니라 사용자 입력이 올바르게 서식이 지정되고 안전한지를 보장합니다.

## 내비게이션

원활한 사용자 경험을 위해서 효율적인 내비게이션은 필수적입니다. Bare Bones Flutter는 직관적인 내비게이션을 위해 go_router와 중첩 라우팅을 위해 shell_route를 활용합니다. 이 설정은 앱의 다른 부분 간 이동이 원할하고 직관적임을 보장합니다.

<div class="content-ad"></div>

## Firebase 통합 (firebase-riverpod 브랜치)

Firebase를 애플리케이션에 통합하려는 개발자를 위해, Bare Bones Flutter는 firebase-riverpod 브랜치라는 전용 브랜치를 제공합니다. 이 브랜치는 Firebase 통합을 추가하여 기능적으로 Sign In, Sign Out 및 Sign Up 기능을 갖춘 기본 템플릿을 바탕으로 구축됩니다. 또한 앱 상태를 깔끔하고 반응적으로 처리하기 위한 강력한 해결책인 상태 관리를 위해 Riverpod를 도입하고 있습니다.

## 브랜치 개요

Bare Bones Flutter는 주로 두 가지 브랜치로 구성되어 있습니다:

<div class="content-ad"></div>

## 메인

이 템플릿에는 상태 관리나 ViewModel 없이 견고한 기반을 제공하는 주요 기능이 포함되어 있습니다. 사용자 정의 기능과 상태 관리 솔루션을 추가하고자 하는 개발자들에게 이상적입니다.

## firebase-riverpod

이 템플릿은 사용자 인증을 위해 Firebase를 통합하고 상태 관리를 위해 Riverpod를 통합하여 메인 템플릿을 확장합니다. 인증 및 상태 관리에 대한 사용 준비 완료 솔루션이 필요한 개발자들에게 완벽합니다.

<div class="content-ad"></div>

# 일반 프로젝트 구조

프로젝트의 디렉토리 레이아웃에 대한 간단한 개요입니다 (다음 웹사이트에서 생성됨: https://ascii-tree-generator.com/):

```js
lib/
├─ core/
│  ├─ constants/
│  ├─ design_system/
│  ├─ di/
│  ├─ init/
├─ data/
│  ├─ di_repository/
│  ├─ repository/
├─ domain/
│  ├─ di_usecase/
│  ├─ model/
│  ├─ usecase/
├─ features/
│  ├─ auth/
│  ├─ dashboard/
│  ├─ navigation/
│  ├─ profile/
│  ├─ search/
├─ l10n/
│  ├─ en.arb
│  ├─ tr.arb
├─ main.dart
```

## Core

<div class="content-ad"></div>

코어 디렉토리에는 상수, 디자인 시스템, 의존성 주입(di) 및 초기화 코드와 같은 핵심 기능이 포함되어 있습니다.

## 데이터

데이터 레이어는 데이터 검색 및 저장을 처리하며, 리포지터리 구현 및 리포지터리를 위한 의존성 주입을 포함합니다.

## 도메인

<div class="content-ad"></div>

도메인 레이어는 모델과 사용 사례를 포함한 비즈니스 로직을 정의합니다. 핵심 로직을 다른 레이어로부터 분리하여 깔끔한 아키텍처를 유지합니다.

## 기능

앱의 각 기능은 해당 디렉토리로 구성되어 있어 인증, 네비게이션, 사용자 프로필과 같은 특정 기능을 쉽게 찾아보고 관리할 수 있습니다.

## 지역화

<div class="content-ad"></div>

로컬라이제이션 디렉터리에는 앱의 손쉬운 지역화 및 국제화를 가능하게 하는 다양한 언어용 .arb 파일이 포함되어 있습니다.

## Makefile

작업 관리를 간소화하기 위해 Bare Bones Flutter에는 Makefile이 포함되어 있습니다. 이 파일에는 로컬라이제이션 파일 생성 및 프로젝트 정리와 같은 일반적인 작업에 대한 명령이 포함되어 있습니다.

![이미지](/assets/img/2024-06-27-JumpstartYourFlutterDevelopmentwithBareBonesFlutter_1.png)

<div class="content-ad"></div>

이제 다 읽으셨습니다. 궁금한 점이 있으면 댓글로 질문해 주세요. 또한, 제 어플리케이션들을 여기서 한번 둘러보세요. 프로젝트 링크는 여기 있습니다. 이 튜토리얼 영상을 보는 것을 추천합니다.

읽어 주셔서 감사합니다! 계속해서 찾아와주세요!