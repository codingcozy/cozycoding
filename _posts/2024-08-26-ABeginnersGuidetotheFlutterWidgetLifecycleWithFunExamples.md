---
title: "플러터 위젯 라이프사이클 정리(코드 있음)"
description: ""
coverImage: "/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_0.png"
date: 2024-08-26 19:51
ogImage: 
  url: /assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_0.png
tag: Tech
originalTitle: "A Beginners Guide to the Flutter Widget Lifecycle (With Fun Examples)"
link: "https://medium.com/@sharma-deepak/a-beginners-guide-to-the-flutter-widget-lifecycle-with-fun-examples-abc1938cea85"
isUpdated: true
updatedAt: 1724744091289
---


플러터 개발을 시작했군요! 이제 위젯의 세계에 더 깊이 들어가려고 하시나봐요. 하지만 멀리 나아가기 전에 플러터 위젯 라이프사이클을 이해하는 것이 중요합니다. 이 라이프사이클을 알고 있다면 나중에 머리 아픔을 막을 수 있어요. 걱정 마세요 – 생각보다 쉽답니다. 한 발자국씩 안내해 드릴 테니 함께 즐겁게 배워봐요!

![이미지](/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_0.png)

## 1. 플러터 위젯 라이프사이클이란 무엇인가요?

플러터 위젯을 정원의 식물들처럼 생각해보세요. 🌱 자라고 변하며 때로는 시들기도 해요. 플러터 위젯 라이프사이클은 위젯이 처음 심어질(생성될) 때부터 정원에서 제거될(해제될) 때까지 거치는 과정입니다. 이 라이프사이클을 이해한다면 위젯을 더 잘 다룰 수 있어요 – 마치 좋은 정원사처럼 말이죠!

<div class="content-ad"></div>

여기 간단한 설명이 있어요:

- 생성: 위젯이 처음 심어질 때
- 초기화: 위젯이 성장을 시작할 때
- 생성/렌더링: 위젯이 피어서 아름다움을 보여줄 때
- 업데이트: 위젯이 변할 때, 식물이 새 잎사귀를 내놓는 것처럼
- 삭제: 위젯이 정원에서 제거될 때

## 2. 씨앗 심기: `createState()`

여행은 `createState()`로 시작돼요. 새 위젯이 위젯 트리에 추가될 때(즉, 씨앗을 심을 때) 호출돼요. 이것은 위젯의 상태를 생성하는 것인데, 이는 식물에게 자라기 위한 화분을 주는 것과 같아요.

<div class="content-ad"></div>


![image](/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_1.png)

Example: Let’s say you’re making a widget to display user profiles. The `createState()` method prepares your widget for what’s to come.

![image](/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_2.png)

## 3. Starting to Grow: `initState()`


<div class="content-ad"></div>

다음으로, `initState()`이 호출됩니다. 이 메서드는 위젯이 성장하기 시작하는 곳입니다. 여기서 데이터를 가져오거나 타이머를 시작하는 것과 같은 작업을 설정할 때 사용됩니다.

![image](/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_3.png)

예: 매 초마다 UI를 업데이트하는 타이머를 설정할 수 있습니다. 여러분의 식물이 싹을 틔우기 시작했습니다!

![image](/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_4.png)

<div class="content-ad"></div>

## 4. 화려한: `build()`

이제 자란 것을 자랑할 때입니다! `build()` 메서드는 플러터가 화면에 위젯을 표시해야 할 때마다 호출됩니다. 이곳에서는 당신이 어떤 모습인지 정의합니다 - 즉, 꽃들입니다.

![예시](/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_5.png)

예시: 사용자 프로필을 만들고 있다고 상상해보십시오. `build()` 메서드에서는 사용자의 이름과 사진을 표시할 수 있습니다. 여러분의 식물이 완전히 피었습니다!

<div class="content-ad"></div>

![이미지](/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_6.png)

## 5. 계절과 함께 변하는 `didUpdateWidget()`

삶은 변화로 가득하고, 당신의 위젯도 마찬가지입니다. `didUpdateWidget()`은 당신의 위젯의 부모가 사용자 프로필을 업데이트하는 것과 같이 무언가를 변경할 때 호출됩니다. 이는 당신의 식물이 오래된 잎을 떨어뜨리고 새 잎을 자라게 하는 것과 비슷합니다.

![이미지](/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_7.png)

<div class="content-ad"></div>

예시: 사용자 ID가 변경되면 이 방법에서 새 데이터를 가져올 수 있습니다. 당신의 식물이 새롭게 열린 잎을 얻었어요!

![이미지](/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_8.png)

## 6. 쉬는 중: `deactivate()`와 `activate()`

가끔씩 당신의 위젯은 잠깐 쉬어야 할 때가 있습니다. 😴 그럴 때 `deactivate()`가 필요해요 – 당신의 위젯이 일시적으로 트리에서 제거될 때 호출됩니다. 하지만 걱정하지 마세요, `activate()`로 다시 일어날 수 있어요!

<div class="content-ad"></div>

<img src="/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_9.png" />

## 7. 이별 인사: `dispose()`

모든 좋은 일은 끝이 날 때가 있어요. 🌻 위젯이 트리에서 영원히 제거될 때, `dispose()`가 호출됩니다. 여기서는 식물에 마지막으로 물을 주는 것처럼 정리를 합니다.

<img src="/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_10.png" />

<div class="content-ad"></div>

예시: 애니메이션 컨트롤러나 스트림 같은 것이 있다면, 그들을 정리하여 메모리 누수를 방지하는 것이 좋습니다. 안녕, 식물!

![이미지](/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_11.png)

# 요약: 재미있는 예제 앱

간단하고 재미있는 예제로 모두 함께 해보겠습니다. 1초마다 증가하는 카운터로 상상해보세요. 마치 실시간으로 자라는 식물 같은 모습! 🌱

<div class="content-ad"></div>

<img src="/assets/img/2024-08-26-ABeginnersGuidetotheFlutterWidgetLifecycleWithFunExamples_12.png" />

## 결론

여기 있습니다! 플러터 위젯 라이프사이클을 이해하는 것은 앱에 대한 마스터 가드너가 되는 것과 같습니다. 🌷 위젯이 언제, 어떻게 성장하고 변하며 종료되는지 알고 있다면, 효율적이고 아름다운 플러터 애플리케이션을 만들 수 있습니다.

자, 이제 나가서 당신의 위젯을 활짝 피워보세요! 즐거운 코딩되세요!

<div class="content-ad"></div>

만약 이 안내서가 도움이 되었고 플러터 개발에 대해 더 알고 싶다면, 더 많은 팁, 튜토리얼 및 재미있는 예제를 보려면 나를 팔로우하지 않는 것을 잊지 마세요! 또한 LinkedIn에서 저와 소통할 수도 있습니다. 함께成 가자! 🌱