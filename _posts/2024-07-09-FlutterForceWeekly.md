---
title: "2024년 주목해야 할 Flutter 최신 소식 주간 정리"
description: ""
coverImage: "/assets/img/2024-07-09-FlutterForceWeekly_0.png"
date: 2024-07-09 09:45
ogImage: 
  url: /assets/img/2024-07-09-FlutterForceWeekly_0.png
tag: Tech
originalTitle: "Flutter Force Weekly"
link: "https://medium.com/flutterforce/flutter-force-weekly-63843f06aa7c"
isUpdated: true
---




플러터에 관한 모든 것을 한 곳에서 확인하세요! 코드, 저장소, 라이브러리, 프로젝트 및 기사들을 발견해 보세요. 함께 멋진 앱을 만들어봐요!

![Flutter](/assets/img/2024-07-09-FlutterForceWeekly_0.png)

## 이번 주 뉴스레터에 오신 걸 환영합니다! 즐거운 업데이트와 통찰력을 공유할 예정이에요. 플러터 개발에 대한 최신 팁, 노하우 및 리소스로 빠르게 살펴보세요. 커뮤니티의 최신 콘텐츠로 당신을 최신 상태로 유지할 수 있게 도와드릴게요. 기술을 향상시키거나 새로운 라이브러리를 탐험하고 최신 트렌드에 대해 알아가고 싶다면, 이번 에디션은 누구나 도움이 될 거예요. 함께 멋진 앱을 만들어봐요! 🚀

# 기사들

<div class="content-ad"></div>

# Dart: 대수적 자료 유형

대수적 자료 유형(ADTs)은 개발자가 복잡한 데이터 구조를 전통적인 객체 지향 클래스보다 더 우아하게 모델링할 수 있는 강력한 함수형 프로그래밍 개념입니다. ADTs는 다른 유형을 결합하는 합성 유형이며, Dart 3.0에서 sealed 클래스와 패턴 매칭을 도입하여 Dart 3에서 ADTs를 가능하게 했습니다.

# 플러터 앱 성능 향상을 위한 합성 계층 이해

플러터에서 위젯, 엘리먼트, 렌더링 객체 트리 외에도 레이어 트리가 있습니다. 엘리먼트 트리가 빌드 단계에서 렌더링 객체 트리를 생성하는 방식과 마찬가지로, 렌더링 객체 트리는 페인트 단계에서 레이어 트리를 생성합니다. 렌더링 객체.isRepaintBoundary가 true인 경우 새 레이어가 생성됩니다.

<div class="content-ad"></div>

# Shared Preferences 플러그인 사용 시 권장 사항

Flutter에서 Shared Preferences 플러그인을 사용할 때 관리가 어려워질 수 있다는 점을 알아차리셨을 것입니다. 코드의 여러 곳에 Shared Preferences 키가 있을 때, 여러 곳에서 동일한 함수를 호출하고, 때로는 삭제해서는 안 될 항목까지 삭제하는 등의 문제가 발생할 수 있습니다. 이 글에서는 Flutter에서 Shared Preference 항목을 효과적으로 관리할 수 있도록 하는 권장 사항에 대해 알아보겠습니다.

# 독특한 액체 차트의 구현

앱에 통합할 수 있는 다양한 형태와 모양의 차트가 많이 있습니다. 그러나 이러한 차트는 종종 이해하기 어렵고 사용자의 시선을 사로잡지 못할 수 있습니다. 앱에 독특한 외관을 부여하기 위해 독특한 차트 디자인을 찾고 있을 때, 디자이너들은 액체 하트 차트라는 특별한 아이디어를 고안했습니다. 이 차트를 개발하는 동안 두 가지 어려움에 직면했습니다:

<div class="content-ad"></div>

- Dart를 사용하여 하트 모양의 경로 생성하기
- 모든 기기 크기에 맞게 크기 조정 가능한 모양 만들기

이 기사는 플러터(Flutter)에서 이 디자인을 구현하는 방법에 대한 빠른 가이드를 제공합니다.

# Docker를 사용한 Dart 및 Flutter 앱 배포: VPS 설정이 간단해지다

우분투 VPS에서 도커화된 Dart 컨테이너를 배포하는 시리즈의 마지막 부분에 오신 것을 환영합니다. 이 시리즈는 가장 경제적인 인프라 선택부터 도커 이미지를 만들고 서버를 배포할 준비를 하는 과정의 모든 단계를 안내하는 데 설계되었습니다. 이 마지막 부분까지 따라오면 제대로 된 Dart 및 Flutter 애플리케이션을 프로덕션 환경에 배포하는 데 필요한 지식과 자신감을 갖게 될 것입니다.

<div class="content-ad"></div>

이 기사에서는 Docker로 패키징된 Dart 코드를 Ubuntu VPS에 배포하는 데 초점을 맞출 것입니다. VPS의 전체 설정부터 배포 방법 및 이 프로세스를 더 쉽게 만들 수 있는 몇 가지 추가 기능을 포함할 것입니다.

## 패키지

## Forui

Forui는 Flutter용 UI 라이브러리로, shadcn/ui에서 큰 영감을 받은 최소화된 위젯 세트를 제공합니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-FlutterForceWeekly_1.png" />

# DuckRouter

덕 라우터는 인텐트를 사용하는 플러터 라우터입니다. Onsi에서 대규모로 테스트되었습니다. 덕 라우터는 신뢰성에 중점을 두면서 "마법"이 없는 철학으로 설계되었습니다.

# WoltModalSheet v0.7.0

<div class="content-ad"></div>

WoltModalSheet은 Flutter 앱의 모달 시트 사용을 혁신하기 위해 설계되었습니다. Wolt 등급의 디자인 품질로 제작되었으며 Wolt 제품에서 널리 사용되는 이 UI 구성 요소는 여러 페이지, 페이지 전환을 위한 동작 및 각 페이지 내에서 스크롤 가능한 내용을 제공하는 시각적으로 매력적이고 매우 사용자 정의 가능한 모달 시트를 제공합니다.

# Flutter/Dart `-` Rust 바인딩 생성기

- 보통의 Rust 코드를 작성하세요 (임의의 유형, 클로저, &mut, 비동기, 트레이트 등 모두 포함)
- 그리고 Rust 코드를 일반적인 Flutter 코드처럼 호출하세요
- 이 브릿지는 중간에서 필요한 모든 접착제를 생성할 것입니다

# os_ui

<div class="content-ad"></div>

이 패키지는 프로젝트를 전시하거나 컴퓨터 운영 체제 인터페이스를 사용하여 어떠한 종류의 프로젝트에도 적용할 수 있도록 도와줍니다.

이 패키지에 포함된 기능은 다음과 같습니다:

- 창 관리 시스템
- 사용자 정의 가능한 토스트 알림
- 운영 체제별 기능
- 다국어 지원

# 동영상

<div class="content-ad"></div>

# Future.wait (이번 주의 기술)

많은 Future가 있나요? Future.wait()을 사용하여 여러 Future가 완료될 때까지 기다렸다가 결과를 수집하세요.

# Flutter 및 Flame으로 게임 개발하기 | 2024

게임 개발에 관심이 있으신가요? 어디서부터 시작해야 할지 모르시겠나요? Flutter와 Flame을 이용한 게임 개발 학습 과정을 나누는 저와 함께해보세요! 경험이 풍부한 개발자이든 초보자이든, 제 이야기를 통해 여러분께 게임 개발 학습을 시작할 수 있는 영감과 실용적인 조언을 제공해 드릴 거예요. 이 비디오에서는 다음을 배울 수 있습니다: * 게임 개발에 Flutter와 Flame을 선택한 이유 * 학습 과정에서 얻은 주요 교훈과 팁 * 개발 과정에서 만든 프로젝트 예시

<div class="content-ad"></div>

# 자체 Flutter 프로젝트

이 비디오에서는 Bare Bones Flutter라는 템플릿 프로젝트에 대해 언급했습니다. 이 비디오는 프로젝트의 구조를 이해하는 데 도움이 됩니다.

Flutter Force를 읽어 주셔서 감사합니다. Flutter Force는 독자들의 지지를 받는 출판물입니다. 새로운 게시물을 받아보고 제 작업을 지원하려면 무료 또는 유료 구독자가 되는 것을 고려해 주세요.

읽어 주셔서 감사합니다. Substack를 통해 팔로우하는 것을 잊지 말아주세요!