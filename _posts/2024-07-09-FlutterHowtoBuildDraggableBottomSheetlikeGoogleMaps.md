---
title: "플러터 구글 맵스 스타일의 드래그 가능한 바텀 시트 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-FlutterHowtoBuildDraggableBottomSheetlikeGoogleMaps_0.png"
date: 2024-07-09 22:51
ogImage: 
  url: /assets/img/2024-07-09-FlutterHowtoBuildDraggableBottomSheetlikeGoogleMaps_0.png
tag: Tech
originalTitle: "Flutter: How to Build Draggable Bottom Sheet like Google Maps"
link: "https://medium.com/@tsung-wei_hsu/flutter-how-to-build-draggable-bottom-sheet-like-google-maps-1165f5b07366"
isUpdated: true
---




<img src="/assets/img/2024-07-09-FlutterHowtoBuildDraggableBottomSheetlikeGoogleMaps_0.png" />

자주 볼 수 있는 드래그 가능한 하단 시트, 특히 네비게이션 앱에서 자주 볼 수 있습니다. 여기서 세부 정보를 포함하는 자식 뷰를 부모 뷰 옆에 배치해야 합니다. BottomSheet, ModalBottomSheet 또는 Dialog와 같은 위젯이 떠오르겠지만, 이러한 요소들이 부모 뷰를 상호작용 불가능하게 만들어서 어둡게 만들어져서 사용 불가능해지는 것을 빨리 깨닫게 됩니다.

이 특정 뷰에 대해 보통 원하는 것은:

- 자식 뷰가 부모에게 추가 정보를 제공합니다.
- 자식 뷰의 크기 조정 및 닫기가 가능합니다.
- 부모 및 자식 뷰 모두 상호작용할 수 있습니다.

<div class="content-ad"></div>

다행히도, 위의 모든 사항을 확인하는 공식 위젯이 있습니다. 이를 DraggableScrollableSheet이라고 합니다. 앱에 원활히 구현하는 방법에 대한 몇 가지 팁과 특이사항은 다음과 같습니다:

## Draggable Sheet 빌드하기

시트는 비교적 유연하며 모든 컨테이너에 배치할 수 있습니다. 아래에서 우리는 제공되는 모든 속성을 갖춘 시트를 생성합니다:

우선, initialChildSize를 0.5로 설정하여 기본적으로 표시될 수 있도록 합니다. 그렇지 않으면 0으로 설정하면 시트가 처음에 숨겨집니다.

<div class="content-ad"></div>

All sizes are defined fractionally to the parent's height. For example, a max size of 0.5 means the sheet can be expanded up to half of its parent's height. In this case, the sheet can be expanded to cover the entire parent.

The snap feature allows the sheet to move to the nearest snap point instead of staying in its current dragging position. It's important to mention that maxChildSize and minChildSize are automatically considered as snap points, so there's no need to define them again in snapSizes.

It's crucial to assign the scrollController in the builder, especially if you have a scrollable widget (which is often the case). This enables scrolling for the widget when the sheet reaches its maximum height. Without assigning the scrollController, you can still scroll, but the sheet won't be draggable.

## Adjust the Sizes

<div class="content-ad"></div>

표가 완전히 닫히기 전에 시트가 축소된 상태여야 하는 경우가 종종 있습니다. 또한, 축소된 상태는 일반적으로 소수가 아닌 특정 높이를 가지고 있습니다. LayoutBuilder 아래에서 부모 높이를 얻고 60px의 정확한 높이를 소수로 변환합니다:

이제 하단 시트의 표준 상태가 설립되었습니다. 즉, 확장된 상태, 고정된 상태, 축소된 상태 및 마지막으로 숨겨진 상태입니다.

## 포지션 처리하기

minChildSize가 0이므로 사용자가 하단으로 너무 깊이 드래그하면 시트가 스냅되어 자동으로 닫힙니다. 이를 피하고 싶은 경우가 있습니다. 이때는 위에서 정의한 _controller가 초기화할 때 리스너를 추가하여 이를 처리합니다:

<div class="content-ad"></div>

시트가 드래그될 때마다 리스너가 실행되며, 시트의 현재 크기를 정확히 추적하여 간단한 조건으로 0에 달라붙지 않도록 하고, 접힌 지점 위에 유지하도록 할 수 있습니다.

이전에 시트 위젯에 제공된 _sheet을 기억하시나요? 이제 이것은 빌드 트리에 정의된 snapSizes에 액세스하기 위한 참조로 사용되며, maxChildSize 등의 다른 속성에 액세스할 수 있습니다. 이러한 값은 시트를 위치시키고 전체 시트 컨트롤을 상대적으로 동적으로 만드는 데 도움이 됩니다.

## 원하는 대로 사용자 정의

이제 각각의 사용 사례에 따라 시트 내에 표시할 콘텐츠를 사용자 정의할 시간입니다. 배경과 상호 작용 가능한 환경의 기능적인 시트를 만드는 중요한 포인트들은 아래와 같습니다:

<div class="content-ad"></div>

- DraggableScrollableSheet은 showBottomSheet()처럼 호출되지 않지만 위젯이 직접 위젯 트리에 배치됩니다.
- 모든 크기는 높이를 기준으로 하며, 부모 위젯에 따라 비율로 정의됩니다.
- GlobalKey와 LayoutBuilder는 부모의 높이, 스냅 크기 등과 같은 유용한 속성을 얻는 데 도움이 됩니다.
- 시트에는 빌더에서 제공된 ScrollController로 할당된 스크롤 가능한 뷰가 포함되어야 하므로 시트를 끌 때와 콘텐츠 스크롤할 때 겹치지 않습니다.
- 위젯 트리 위나 아래에 시트 위치를 제어하는 상태 관리를 추가하는 것을 고려하세요.

결국 주요 네비게이션 앱과 유사한 아래와 같은 것을 얻을 수 있을 것입니다:

![이미지](https://miro.medium.com/v2/resize:fit:620/1*nuYK_wi47-OnxbsREAjPXw.gif)

코딩 즐기세요! 🙌🏼
아래에는 플러터에서 유용한 정보 몇 가지가 있습니다. 함께 보시면 도움이 될 수 있을 것입니다: