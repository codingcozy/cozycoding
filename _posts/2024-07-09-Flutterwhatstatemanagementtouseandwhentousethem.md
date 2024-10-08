---
title: "Flutter 상태 관리 방법과 언제 사용해야 할까"
description: ""
coverImage: "/assets/img/2024-07-09-Flutterwhatstatemanagementtouseandwhentousethem_0.png"
date: 2024-07-09 22:25
ogImage: 
  url: /assets/img/2024-07-09-Flutterwhatstatemanagementtouseandwhentousethem_0.png
tag: Tech
originalTitle: "Flutter : what state management to use and when to use them?"
link: "https://medium.com/@cloderaldo/flutter-what-state-management-to-use-and-when-to-use-them-3421b4efb4ba"
isUpdated: true
---




플러터(Flutter)에서 상태 관리는 애플리케이션을 구축하는 중요한 측면입니다. 올바른 상태 관리 솔루션을 선택하는 것은 애플리케이션의 복잡성과 요구 사항에 달려 있습니다. 다음은 플러터(Flutter)에서 인기 있는 상태 관리 솔루션과 그 사용 방법입니다:

## 1. setState

- 사용 사례: 간단한 상태 관리.
- 사용 시기: 여러 위젯 간에 공유할 필요가 없고 로컬 상태만을 가진 간단한 앱일 때 사용합니다.
- 예시 사용 사례: 단일 위젯 내에서만 필요한 상태를 가진 카운터 앱.

## 2. InheritedWidget / InheritedModel

<div class="content-ad"></div>

- 사용 목적: 위젯 트리 아래로 상태를 전파하는 데 사용됩니다.
- 사용 시기: 많은 위젯에 데이터를 전달해야 하지만 상태 변경이 드물 때 사용됩니다.
- 예제 사용 사례: 많은 위젯에서 액세스해야 하는 테마나 지역화 데이터와 같은 것들.

## 3. Provider

- 사용 목적: 간단한에서 중간 규모의 상태 관리에 사용됩니다.
- 사용 시기: 앱 전반에 걸쳐 상태를 효과적으로 관리하고 전파하는 강력하면서도 간단한 솔루션이 필요할 때 사용됩니다. InheritedWidget를 기반으로 널리 사용되었습니다.
- 예제 사용 사례: 사용자 인증 상태나 앱 설정과 같은 상태를 공유해야 하는 다중 화면 앱.

## 4. Riverpod

<div class="content-ad"></div>

- 사용 용도: Provider보다 안전하고 진보된 대안으로 사용합니다.
- 언제 사용할까요: Provider보다 더 많은 제어와 보일러 플레이트가 필요할 때 사용합니다. Riverpod은 컴파일 시간 안전성도 제공합니다.
- 예시 사용 사례: Provider를 사용할 때보다 개발자 경험이 향상되고 안전성이 높은 경우.

## 5. Bloc / Cubit (flutter_bloc)

- 사용 용도: 예측 가능한 상태 전환을 갖는 복잡한 상태를 관리합니다.
- 언제 사용할까요: 이벤트와 상태의 스트림이 있는 복잡한 상태 관리가 필요한 앱 일 때 사용합니다. Bloc은 비즈니스 로직을 UI에서 분리하는 데 도움이 됩니다.
- 예시 사용 사례: 메시지 로딩, 메시지 표시, 오류 처리 등 다양한 상태가 있는 채팅 애플리케이션.

## 6. GetX


<div class="content-ad"></div>

- **MobX**
    - **사용 목적:** 반응형 프로그래밍으로 간단한 상태 관리에서 복잡한 상태 관리까지 가능합니다.
    - **사용 시기:** 보일러플레이트 코드를 줄이고 쉬운 반응형 프로그래밍을 원할 때 사용합니다.
    - **예시 사용 사례:** 상태 변경에 따라 UI에서 실시간 업데이트가 필요한 경우, 라이브 스포츠 스코어 앱 등에서 사용 가능합니다.

- **Redux**
    - **사용 목적:** 반응형 상태 관리에 사용됩니다.
    - **사용 시기:** observable 기반 접근 방식을 선호하고 활성화에 대한 잘 정립된 패턴을 사용하고 싶을 때 사용합니다.
    - **예시 사용 사례:** 사용자 입력 및 다른 필드 값에 반응하는 각 필드의 양식 유효성 검사와 같은 경우에 사용됩니다.

<div class="content-ad"></div>

- 사용 용도: 단일 진실의 소스로 예측 가능한 상태 관리에 사용됩니다.
- 사용하는 시기: 단방향 데이터 흐름과 단일 중앙 상태 저장소가 필요한 경우에 사용됩니다. 예측 가능성과 유지 보수 가능성이 중요한 매우 큰 응용프로그램에 유용합니다.
- 예시 사용 사례: 다양한 앱 부분 간 복잡한 상호 작용이 있는 대규모 전자 상거래 응용프로그램.

# 9. States Rebuilder

- 사용 용도: 단순성과 성능을 고려한 반응적 상태 관리에 사용됩니다.
- 사용하는 시기: 반응형 프로그래밍에 초점을 맞춘 간단하고 강력한 솔루션이 필요한 경우에 사용됩니다.
- 예시 사용 사례: 최소한의 보일러플레이트로 매우 반응적인 UI 업데이트가 필요한 응용프로그램.

요약 표:

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-Flutterwhatstatemanagementtouseandwhentousethem_0.png)

적절한 상태 관리 솔루션을 선택하는 것은 애플리케이션의 요구 사항과 복잡성에 따라 다릅니다. 간단한 앱의 경우 setState나 InheritedWidget을 사용하는 것이 충분할 수 있습니다. 더 복잡한 시나리오의 경우 Provider, Riverpod, Bloc 또는 다른 솔루션이 더 적합할 수 있습니다.

다양한 상태 관리 솔루션을 알고 있는 것은 큰 장점이며 귀중한 경험입니다. 그러나 제안드리는 것은 사용하기 쉽고 편안한 3~4개의 솔루션을 숙달하는 것입니다. 새로운 팀이나 프로젝트에 참여할 때 새로운 상태 관리 솔루션을 배워야 할 수도 있습니다. 이것이 괜찮다고 생각하고 회사에 솔직하게 이야기하고 너무 과도하게 생각하지 마세요. 이미 다른 상태 관리 솔루션을 경험했기 때문에 쉬울 것입니다.

그렇다면 저는 무엇을 사용할까요? 저는 간단한 앱에는 setState를 사용하고 더 복잡한 앱에는 Provider를 사용합니다. 이것들은 몇 년 동안 숙달한 솔루션입니다. 새로운 팀에 참여하여 다른 상태 관리 솔루션을 사용한다면 새로운 도전이자 내가 현재 사용 중인 솔루션에 대한 지식과 경험을 공유할 수 있는 기회로 생각하겠습니다.

<div class="content-ad"></div>

너가 여기까지 왔다면, 박수를 치고 평화를 바란다.