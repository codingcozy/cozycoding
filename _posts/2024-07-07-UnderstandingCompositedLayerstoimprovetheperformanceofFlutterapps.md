---
title: "Flutter 앱 성능을 향상시키는 Composited Layers 이해하기"
description: ""
coverImage: "/assets/img/2024-07-07-UnderstandingCompositedLayerstoimprovetheperformanceofFlutterapps_0.png"
date: 2024-07-07 19:40
ogImage: 
  url: /assets/img/2024-07-07-UnderstandingCompositedLayerstoimprovetheperformanceofFlutterapps_0.png
tag: Tech
originalTitle: "Understanding Composited Layers to improve the performance of Flutter apps"
link: "https://medium.com/@pomis172/understanding-composited-layers-to-improve-the-performance-of-flutter-apps-7b91079b4dd1"
isUpdated: true
---




플러터에서는 위젯, 엘리먼트 및 렌더 오브젝트 트리 외에도 레이어 트리가 있습니다. 엘리먼트 트리가 렌더 오브젝트 트리를 빌드 단계에서 생성하는 방식과 같이, 렌더 오브젝트 트리는 페인트 단계에서 레이어 트리를 생성합니다. RenderObject.isRepaintBoundary가 true인 경우 새 레이어가 생성됩니다.

레이어는 비용이 많이 들어갑니다. 아래 문서 단편은 그 이유를 설명합니다:

그러나 UI 일부를 별도의 레이어로 격리하여 플러터는 UI 일부만 변경될 때 전체 화면을 다시 그리는 것을 피할 수 있습니다. 레이어는 일부 UI를 독립적으로 애니메이션화하여 전체 UI에 영향을주지 않고 부드러운 애니메이션을 달성하는 데 도움을 줍니다.

<div class="content-ad"></div>

응용 프로그램 레이어를 최적으로 유지하기 위해 이 규칙들을 따르세요:

- 무언가가 애니메이션되면 가능한 한 별도의 레이어로 격리하여 너무 많은 렌더링을 유발하지 않도록 합니다.
- 반대로, 가능한 적은 레이어를 유지하세요. 각 레이어는 추가적인 계산 및 메모리 오버헤드를 추가합니다.

어떻게 이를 달성할 수 있는지 살펴봅시다:

# 1. 과도한 그림 그리기 피하기

<div class="content-ad"></div>

가장 쉬운 방법은 RepaintBoundary 위젯을 사용하는 것입니다. 이 위젯은 SingleChildRenderObjectWidget을 확장하므로, 이 위젯에는 하나의 RenderObject가 할당됩니다. 물론, 해당 RenderObject의 isRepaintBoundary가 true로 설정되어 있습니다.

## 1.1 애니메이션을 격리하세요

```js
RepaintBoundary(
  child: CircularProgressIndicator() // <- 우리의 애니메이션 위젯
),
```

이 예시에서는 애니메이션 위젯의 다시 그리기 범위를 제한하여, 그렇지 않으면 애니메이션의 각 틱마다 부모 레이어 전체가 다시 그려집니다.

<div class="content-ad"></div>

리페인트 경계를 추가하기 전 레이어

![이미지](/assets/img/2024-07-07-UnderstandingCompositedLayerstoimprovetheperformanceofFlutterapps_1.png)

리페인트 경계를 추가한 후 레이어:

![이미지](/assets/img/2024-07-07-UnderstandingCompositedLayerstoimprovetheperformanceofFlutterapps_2.png)

<div class="content-ad"></div>

RenderRepaintBoundary에는 RepaintBoundary 위젯의 사용이 유용한지 여부를 계산하는 metrics 속성이 있습니다. 이를 액세스하는 가장 쉬운 방법은 DevTools를 통해 액세스하는 것입니다:

![DevTools](/assets/img/2024-07-07-UnderstandingCompositedLayerstoimprovetheperformanceofFlutterapps_3.png)

우리가 보듯이, 이 경우 RepaintBoundary의 사용은 metrics에 따라 유용합니다.

## 1.2 고정 및 스크롤 가능한 내용 격리하기

<div class="content-ad"></div>

값을 참고할 만한 것은 또한 AppBar가 기본적으로 별도의 레이어로 그려진다는 것입니다. 만약 AppBar의 코드를 확인한다면, 레이아웃에 항상 합성이 필요한 RenderObject가 있는 AnnotatedRegion이 포함되어 있음을 알 수 있습니다.

이는 Scaffold의 본문을 스크롤할 때 AppBar이 고정되어 있어 다시 그릴 필요가 없기 때문에 이해됩니다. 애플리케이션에서 사용자 지정 AppBar을 구현하는 경우, 해당 그림을 별도의 레이어로 격리하는 것을 잊지 마세요.

# 2. 과도한 레이어 회피

## 2.1 이미지에 Opacity 또는 ColorFiltered 위젯을 사용하지 마세요.

<div class="content-ad"></div>

새로운 구성된 레이어를 만들어요. Image의 color와 blendMode 매개변수를 사용하세요. 이에 대해 더 자세히 알고 싶다면 이 기사를 읽어보세요.

## 2.2 애니메이션에 불투명도를 사용하지 마세요

AnimatedOpacity를 사용하면 성능이 더 우수해지며 전체 자식 위젯 트리를 다시 그리지 않고 불투명도 값을 애니메이션화할 수 있어요.

## 2.3 가능한 경우 클리핑 대신 장식(decoration)을 사용하세요

<div class="content-ad"></div>

문서에 따르면:

위젯에 모양을 추가하려면, 내용 클리핑이 필요하지 않은 경우 DecoratedBox 또는 Container의 decoration 속성을 사용하십시오. 모양과 클리핑에 관한 자세한 내용은 이 문서에서 확인할 수 있습니다.

## 2.4 필요한 경우에는 클리핑 비활성화

일부 표준 위젯은 기본적으로 클리핑이 활성화되어 있습니다. 예를 들어 위젯이 Stack 상자를 벗어나지 않는다는 것을 알고 있다면, 클리핑을 비활성화하는 것이 좋습니다:

<div class="content-ad"></div>

```js
Stack(
  clipBehavior: Clip.none,
  child: ...
)
```

- ClipPath는 기본적으로 Clip.antiAlias로 설정됩니다.
- ClipRRect는 기본적으로 Clip.antiAlias로 설정됩니다.
- ClipRect는 기본적으로 Clip.hardEdge로 설정됩니다.
- Stack은 기본적으로 Clip.hardEdge로 설정됩니다.
- EditableText는 기본적으로 Clip.hardEdge로 설정됩니다.
- ListWheelScrollView는 기본적으로 Clip.hardEdge로 설정됩니다.
- SingleChildScrollView는 기본적으로 Clip.hardEdge로 설정됩니다.
- NestedScrollView는 기본적으로 Clip.hardEdge로 설정됩니다.
- ShrinkWrappingViewport는 기본적으로 Clip.hardEdge로 설정됩니다.

## 2.5 다른 일부 위젯의 비용에 유의하세요

Transform, BackdropFilter, ShaderMask 및 Texture와 같은 위젯들도 새로운 레이어를 생성합니다. 이들을 다른 것으로 교체하는 것이 까다로울 수 있지만, 이들이 무료가 아니라는 것을 알아두세요.


<div class="content-ad"></div>

이 문서가 유용했기를 바랍니다. 새로운 기술을 발견할 때마다 업데이트하겠습니다. 최신 업데이트를 받으려면 트위터에서 저를 팔로우해주세요.

![UnderstandingCompositedLayerstoimprovetheperformanceofFlutterapps_4](/assets/img/2024-07-07-UnderstandingCompositedLayerstoimprovetheperformanceofFlutterapps_4.png)