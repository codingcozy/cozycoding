---
title: "플러터 ToDo 애플리케이션 파트 1 인프라설정 방법"
description: ""
coverImage: "/assets/img/2024-06-30-FlutterToDoapplicationPart1Infrastructure_0.png"
date: 2024-06-30 22:59
ogImage: 
  url: /assets/img/2024-06-30-FlutterToDoapplicationPart1Infrastructure_0.png
tag: Tech
originalTitle: "Flutter ToDo application Part 1: Infrastructure"
link: "https://medium.com/@sadegh.dehghani1992/flutter-todo-application-part-1-infrastructure-96a5ba762c15"
isUpdated: true
---





이 시리즈의 기사에서는 Flutter를 활용하여 ToDo 애플리케이션을 만들고 여러 플랫폼에서 사용하는 방법을 살펴보겠습니다. (전체 소스 코드는 GitHub에서 확인할 수 있으며, 이 시리즈의 기사를 건너뛰고 언제든지 소스 코드를 다운로드할 수 있습니다.)

우선, 이러한 모든 것을 이해하기 위해 몇 가지 요구 사항이 필요합니다:

- Clean Architecture
- Multi-language
- Multi-theme
- Bloc
- Unit test
- Relational Database

어느 부분이 익숙하지 않다면 각 섹션에 대해 발행된 기사를 클릭하여 읽을 수 있습니다.

<div class="content-ad"></div>

먼저 UI를 확인해야 해요. Bemani가 이 UI를 만들었고, 아래 링크에서 찾을 수 있어요.

프로젝트 분석 후 코딩을 시작해요. 이 프로젝트 구조는 Clean Architecture를 기반으로 하고, 관련 기사를 참고하여 다국어 및 다테마를 추가했어요. (이 구조에 대한 더 자세한 내용은 관련 기사를 읽어보세요.)

![이미지](/assets/img/2024-06-30-FlutterToDoapplicationPart1Infrastructure_0.png)

# res 폴더 안에는 무엇이 있나요?

<div class="content-ad"></div>

프로젝트에서 반복적으로 사용하는 도우미 클래스를 몇 개 만들었습니다. 아래는 일부 중요한 클래스의 소스 코드입니다.

## Dimens

UI의 모든 위젯에는 크기, 패딩, 둥근 모서리 등이 있습니다. 이러한 것들을 코드에 넣고 중앙에 배치하지 않으면 나중에 일부 치수를 변경할 때 문제가 발생할 수 있습니다. 그래서 이러한 값을 중앙에 정리하는 것이 좋습니다.

## drawable

<div class="content-ad"></div>

차원과 마찬가지로 아이콘, 카드 배경, 그림자 등과 같은 반복 가능한 드로어블을 사용합니다.

## 스타일

일부 텍스트 스타일과 애플리케이션 테마 클래스가 이 패키지 내에 있습니다.

# src 폴더에는 무엇이 있나요?

<div class="content-ad"></div>

이 폴더에서는 응용 프로그램의 src 코드를 작성합니다. 이 폴더는 깔끔한 아키텍처 역할을 따라 (더 자세한 내용은 이 기사를 읽어보세요) 흐릅니다. 우리의 src 코드에는 3개의 계층이 있습니다.

App, Domain, Data.

![이미지](/assets/img/2024-06-30-FlutterToDoapplicationPart1Infrastructure_1.png)

- app: 이 계층에는 UI 로직 및 다른 것들에 대한 UI와 관련된 모든 것이 의존하지 않고 포함되어 있습니다. 우리는 상태 관리자로 Bloc을 사용하고 UI를 간단한 재사용 가능한 위젯으로 분할했습니다. 각 블록 상태는 도메인 레이어에 있는 사용 사례에만 의존합니다.
- domain: 이 계층에는 UI 데이터 모델, 리포지토리 및 사용 사례가 포함되어 있습니다. UI 계층은 사용 사례에만 의존하며 각 사용 사례는 하나 이상의 리포지토리에 의존합니다. 사용 사례는 항목이나 기능으로 분할될 수 있으며, 예를 들어, 모든 작업에 대한 모든 기능을 포함하는 TaskUseCase가 있을 수 있습니다. 또는 AddTaskUseCase, EditTaskUseCase, DeleteTaskUseCase 등과 같이 기능별로 분할할 수 있습니다. 이 프로젝트에서는 작업을 위한 하나의 사용 사례, TaskUseCase만 있지만, 재사용성과 SOLID를 위해 기능별로 분할하는 것이 좋습니다.
- data: 이 계층은 도메인 레이어에 존재하는 리포지토리에 의존하며 데이터를 저장하거나 검색하는 데 필요한 모든 논리를 포함하고 있습니다. 도메인 레이어의 각 리포지토리에 대해 데이터 레이어에는 여러 데이터를 사용할 수 있는 하나의 구현이 있습니다. 각 데이터 제공자는 해당 구현에 데이터를 제공해야 합니다.

<div class="content-ad"></div>

왜 우리는 이러한 레이어를 사용하고 간단한 방법을 사용하지 않을까요?

이 질문에 대한 답은 확장성 및 테스트 용이성으로 요약됩니다. 여러 레이어가 있고 서로 직접적으로 관련이 없는 경우 각 레이어와 클래스에 대해 독립적으로 테스트를 작성할 수 있습니다.

애플리케이션을 개발할 때는 기능별로 코드를 작성해야 하며, 한꺼번에 모든 도메인 또는 데이터 레이어 클래스를 작성해서는 안 됩니다. 하지만 이 시리즈의 기사를 요약하면 이렇게 한다.

이 프로젝트의 전체 소스 코드는 GitHub의 이 저장소에서 확인할 수 있습니다.

<div class="content-ad"></div>

다음은 저희 애플리케이션의 다음 단계입니다:

도메인 레이어

데이터 레이어

애플리케이션 레이어