---
title: "Flutter 위젯 초기화 후 함수 실행하는 방법"
description: ""
coverImage: "/assets/img/2024-06-22-HowtoFlutterrunningfunctionsafterwidgetinitialization_0.png"
date: 2024-06-22 15:42
ogImage: 
  url: /assets/img/2024-06-22-HowtoFlutterrunningfunctionsafterwidgetinitialization_0.png
tag: Tech
originalTitle: "“How to Flutter”: running functions after widget initialization."
link: "https://medium.com/@wartelski/how-to-flutter-running-functions-after-widget-initialization-7d7b4150b147"
isUpdated: true
---





이 글에서는 WidgetsBinding.instance.addPostFrameCallback 메서드를 자세히 검토하고 싶습니다.

![image](/assets/img/2024-06-22-HowtoFlutterrunningfunctionsafterwidgetinitialization_0.png)

예를 들어 서버에서 가져온 데이터로 데이터 그리드를 표시하거나 UI가 렌더링된 후에 대화 상자를 표시하려고 합니다. 이를 어떻게 구현할까요?

아마 이미 이해하신 것처럼 WidgetsBinding.instance.addPostFrameCallback 메서드를 사용할 것입니다.

<div class="content-ad"></div>

## WidgetsBinding.instance.addPostFrameCallback은 무엇인가요?

WidgetsBinding.instance.addPostFrameCallback은 플러터(Flutter)에서 개발자가 현재 프레임이 그려진 후에 콜백을 실행할 수 있도록 하는 메서드입니다. 이는 이 콜백 내의 코드가 플러터 프레임워크가 빌드 및 레이아웃 단계를 완료하고 화면에 프레임이 렌더링된 후에 실행된다는 것을 의미합니다.

다음은 사용 방법의 기본 예시입니다:

```js
WidgetsBinding.instance.addPostFrameCallback((_) {
  ...
});
```

<div class="content-ad"></div>

Underscore _ 는 콜백의 매개변수로 사용되며, 이는 프레임의 지속 시간을 의미합니다.

하지만 한 걸음 물러나서 "프레임"에 대해 이야기해 봅시다. 프레임이라는 개념은 프레임워크가 사용자 인터페이스를 렌더링하는 방식에 중요합니다.

## 프레임이란?

Flutter는 UI를 초당 60프레임(fps) 또는 120Hz 업데이트를 지원하는 장치에서는 초당 120프레임으로 렌더링되도록 설계되었습니다. 각 렌더링을 프레임이라고 합니다. 즉, 대략 16ms마다 UI가 업데이트되어 애니메이션 또는 UI의 다른 변경 사항이 반영됩니다. 16ms보다 오랜 시간이 걸리는 프레임은 디스플레이 장치에서 간섭(부드럽지 않은 모션)을 일으킵니다.

<div class="content-ad"></div>

성능 문제가 발생할 수 있습니다. 프레임을 구축, 레이아웃, 페인트하거나 합성하는 데 필요한 작업 시간이 이 시간 예산을 초과하면 프레임이 삭제되고 눈에 띄는 지연이 발생할 수 있습니다.

새 프레임을 레이아웃하고 페인트할 때 엔진은 자동으로 handleDrawFrame을 호출하며, 이는 drawFrame을 호출하고(빌드 및 렌더 파이프라인을 활성화하여 프레임을 생성)합니다.

각 프레임은 다음 단계로 구성됩니다:

- 빌드 단계:

<div class="content-ad"></div>

- 빌드 단계에서는 Flutter가 위젯 트리를 구성합니다. 트리의 각 위젯은 현재 상태를 기반으로 새 위젯 인스턴스를 생성하기 위해 빌드 메서드를 호출합니다.
이 단계는 위젯 트리의 다른 부분에 대해 여러 번 발생할 수 있지만, 최종적으로 하나의 프레임으로 렌더링됩니다.

2. 레이아웃 단계:

- 레이아웃 단계에서는 Flutter가 각 위젯의 크기와 위치를 계산합니다. 이 단계는 각 위젯이 화면에 어떻게 배치되어야 하는지 결정하는 데 중요합니다.
위젯은 제약 조건을 트리로 내려보내고, 각 위젯은 이러한 제약 조건을 기반으로 자체 크기를 조정합니다.

3. 페인트 단계:

<div class="content-ad"></div>

- 페인트 단계에서 Flutter는 각 위젯을 캔버스로 렌더링합니다. 이 단계는 텍스트, 이미지 및 모양을 포함한 위젯의 시각적 표현을 그리는 과정입니다.
페인팅 프로세스는 매우 최적화되어 있어 Flutter가 복잡한 UI를 효율적으로 렌더링할 수 있습니다.

4. 합성 단계:

- 합성 단계는 그려진 레이어를 최종 이미지로 결합하여 화면에 표시합니다.
이 단계를 통해 Flutter는 클리핑, 변형 및 투명도와 같은 시각적 효과를 별도의 레이어를 병합하여 효율적으로 처리할 수 있습니다.

## WidgetsBinding.instance.addPostFrameCallback 및 프레임

<div class="content-ad"></div>

WidgetsBinding.instance.addPostFrameCallback은 현재 프레임이 렌더링된 후에 실행될 콜백을 예약하는 메서드입니다.

프레임 렌더링 프로세스에 대해 이해하는 것이 매우 중요합니다:

- 프레임 이후: addPostFrameCallback로 등록된 콜백은 현재 프레임의 빌드, 레이아웃, 페인트 및 합성 단계가 모두 완료된 후에 실행됩니다. 이는 콜백이 실행되기 전에 UI가 완전히 업데이트되었음을 보장합니다.
- 일회성 실행: 이 콜백은 한 번만 실행되며 프레임이 렌더링된 직후 즉시 실행됩니다. 반복 작업을 위해서는 콜백을 다시 등록해야 합니다.

## addPostFrameCallback 메서드의 사용 사례.

<div class="content-ad"></div>

- 일회성 초기화 논리: 위젯 트리가 빌드된 후 한 번 실행해야 하는 코드에 주로 사용됩니다.

```js
@override
void initState() {
  super.initState();
  WidgetsBinding.instance.addPostFrameCallback((_) {
    // 여기서 후프레임 초기화를 수행합니다.
    _showDialog();
  });
}
```

2. 레이아웃 계산: 위젯의 크기와 위치에 의존하는 작업(예: 사용자 정의 애니메이션 또는 위치 지정)을 수행해야 하는 경우, addPostFrameCallback을 사용하면 레이아웃이 완료된 후 이러한 계산이 이루어집니다.

```js
@override
Widget build(BuildContext context) {
  WidgetsBinding.instance.addPostFrameCallback((_) {
    final RenderBox box = context.findRenderObject() as RenderBox;
    final position = box.localToGlobal(Offset.zero);
  });
  return Container(); // 여기에 위젯을 추가하세요
}
```

<div class="content-ad"></div>

3. 상태 관리: 상태가 있는 위젯에서는 처음 렌더링 이후에만 특정 작업이나 상태 변경을 트리거하고 렌더링이나 상태 불일치 문제를 방지하는 것이 좋습니다.

```js
@override
void initState() {
  super.initState();
  WidgetsBinding.instance.addPostFrameCallback((_) {
    setState(() {
      _initializeState();
    });
  });
}
```

언급할만한 사항...

➡️ addPostFrameCallback 내의 코드가 효율적이고 UI 반응성을 유지하기 위해 무겁지 않도록 확인하세요.

<div class="content-ad"></div>

➡️ 다른 개발자들에게 명확히 목적을 전달하기 위해 코드에서 addPostFrameCallback의 사용을 문서화하세요. 일반적인 build-render 흐름을 변경하기 때문에 특히 중요합니다.

➡️ addPostFrameCallback를 과용하거나 오용하는 것은 성능 문제를 야기할 수 있습니다. 특히 복잡한 로직이 콜백에서 실행될 때 그렇습니다.

➡️ 콜백은 한 번만 실행됩니다. 반복 작업을 위해서는 개발자들이 콜백을 다시 등록해야 합니다.

➡️ 콜백 내에서 상태 변경을 관리하는 것은 때로 코드가 더 어려워지고 조심스럽게 다루지 않으면 잠재적인 버그를 유발할 수 있습니다.

<div class="content-ad"></div>

잘 하셨습니다! 이 글을 읽어주셔서 감사합니다. 도움이 되셨기를 바랍니다.

제 다른 글을 더 살펴보시고 앞으로의 업데이트 소식을 받아보기 위해 팔로우하시거나 👏 몇 개 주시면 감사하겠습니다!