---
title: "Flutter Web에서 정밀한 위젯 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-06-NuancedwidgetsinFlutterWeb_0.png"
date: 2024-07-06 02:44
ogImage: 
  url: /assets/img/2024-07-06-NuancedwidgetsinFlutterWeb_0.png
tag: Tech
originalTitle: "Nuanced widgets in Flutter Web"
link: "https://medium.com/@kon.syrokostas/nuanced-widgets-in-flutter-web-cf254df80116"
isUpdated: true
---





## 제가 Fluttercon EU 2024에서 발표한 동반자

Fluttercon EU 2024에서 나의 발표 중에 저희가 Flutter를 사용하여 반응형 UI를 어떻게 구축할 수 있는지 탐색했습니다. 그런 다음 조건부 가져오기와 SEO와 같은 Flutter Web의 일부 세부 사항에 대해 이야기했습니다. 그러나 발표 중에 웹에서 특별한 주의를 요하는 일부 위젯에 대해 다루지 못했습니다.

/assets/img/2024-07-06-NuancedwidgetsinFlutterWeb_0.png

본 블로그 포스트는 이 발표의 동반자로서 시간 제약으로 무시된 부분을 다루고 있습니다.

<div class="content-ad"></div>

# 시작 화면

실제로 위젯이 아니라 위젯이 없어야 하는 것이 고려되어야 합니다.

Flutter Web 애플리케이션이 열릴 때 프레임워크는 다음 단계를 실행합니다:

- index.html 파일을 표시합니다.
- flutter.js 스크립트를 다운로드합니다.
- Flutter 엔진을 초기화합니다.
- Flutter 주요 함수를 실행합니다.
- 앱의 UI(즉, 위젯)를 표시합니다.

<div class="content-ad"></div>

문제는 이러한 단계들이 완료되기까지 시간이 걸린다는 것입니다. 그리고 완료되기 전까지는 index.html의 내용만 표시됩니다. 기본적으로 그것은 하얀 화면입니다.

/assets/img/2024-07-06-NuancedwidgetsinFlutterWeb_1.png

몇 초 동안 하얀 화면을 보는 것은 웹 앱을 사용하는 사람들의 경험을 해치게 할 수 있습니다. 할 수 있는 최적화 방법은 많지만, 가장 간단한 방법은 index.html 파일을 수정하는 것입니다.

다음 코드에서 예를 들어 시작 페이지의 색상을 수정하고 가운데 텍스트를 추가했습니다:

<div class="content-ad"></div>

```js
<body style="background-color: orange">
  <div style="height: 100vh; display: flex; justify-content: center; align-items: center;">
    <div>애니메이션이나 로딩 인디케이터를 사용하세요 :)</div>
  </div>
  <!-- Flutter 초기화 스크립트를 여기에 추가 -->
</body>
```

시작 UI가 다르게 됩니다.

/assets/img/2024-07-06-NuancedwidgetsinFlutterWeb_2.png

당연히 전체를 주황색으로 채운 후 텍스트를 넣는 것은 추천하는 것은 아닙니다 (비록 멋지긴 하지만요). 제 의겢은 몇 줄의 HTML 코드로 앱에 대한 사용자의 첫 경험을 쉽게 개선할 수 있다는 것입니다.

<div class="content-ad"></div>

# 선택 가능한 텍스트

기본적으로 플러터의 Text 위젯은 선택할 수 없습니다. 이것은 모바일 앱에 대한 합리적인 설계 선택이지만, 동일한 위젯이 웹(또는 마우스를 포함한 기타 플랫폼)에서 사용될 때 문제가 발생하기 시작합니다.

다행히도 플러터 팀은 우리를 위한 편리한 해결책을 만들어 냈습니다. SelectableText 위젯은 Text와 정확히 같은 방식으로 작동하지만 텍스트를 선택할 수도 있습니다.

SelectableText.rich 생성자를 사용하여 RichText와 유사한 동작도 달성할 수 있습니다.

<div class="content-ad"></div>

마침내 SelectableArea 위젯이 추가되었습니다. 이는 하위 항목을 모두 선택 가능하게 만듭니다. 더 편리할 수 있지만, 기본 텍스트가 클릭 가능할 때(예: URL을 표시하는 텍스트에 GestureDetector를 사용하는 경우) 예기치 않은 동작을 보일 수 있다는 것을 발견했습니다.

# MouseRegion

클릭 가능한 텍스트에 대해 말씀드리면, GestureDetector 위젯은 클릭 동작을 통보하기 위해 커서 아이콘을 변경하지 않을 수 있습니다.

이 문제는 MouseRegion 위젯을 사용하여 쉽게 해결할 수 있습니다. 이 위젯은 커서가 들어가면 커서 아이콘을 변경할 것입니다.

<div class="content-ad"></div>


/assets/img/2024-07-06-NuancedwidgetsinFlutterWeb_3.png

아웃오브박스로 이 동작을 지원하지 않는 클릭 가능한 위젯에 이것이 완벽합니다.

```js
@override
Widget build(BuildContext context) {
  return MouseRegion(
    cursor: SystemMouseCursors.click,
    child: GestureDetector(
      onTap: onClick,
      child: const Text("Click me"),
    ),
  );
}
```

하지만 이것은 또한 커서를 다른 것으로 변경하는 데에도 사용할 수 있습니다.


<div class="content-ad"></div>

다만 가능하다면 가능한 경우 시스템 또는 재료 버튼을 사용하는 것이 좋습니다. 왜냐하면 접근성과 같은 추가 기능도 처리하기 때문이죠.

# AdaptiveScaffold

이것이 마지막입니다.

AdaptiveScaffold는 플러터 팀에 의해 개발된 패키지로, 적응형 UI를 쉽게 디자인할 수 있도록 도와줍니다.

<div class="content-ad"></div>

화면을 여섯 개의 부분(일부는 무시할 수 있음)으로 분할하고 화면 크기에 따라 일부를 표시합니다.

![이미지](/assets/img/2024-07-06-NuancedwidgetsinFlutterWeb_4.png)

`AdaptiveScaffold`를 사용하면 이렇게 보일 수 있습니다:

![이미지](https://miro.medium.com/v2/resize:fit:1400/0*wh4omf50o935nGTv.gif)

<div class="content-ad"></div>

아래는 AdaptiveScaffold가 사용하는 브레이크포인트가 머티리얼 가이드라인에서 제안하는 내용을 사용하고 그에 따라 화면에 표시되는 내용을 변경한다.

/assets/img/2024-07-06-NuancedwidgetsinFlutterWeb_5.png

구체적으로는:

- Compact 화면 크기에서는 네비게이션을 위한 바텀바 아이콘이나 햄버거 아이콘이 있습니다.
- Medium 화면 크기에서는 네비게이션을 위한 사이드바 아이콘이 있습니다.
- Expanded 화면 크기에서는 네비게이션을 위한 텍스트가 포함된 사이드바 아이콘이 있습니다.
- 주요 바디는 항상 표시됩니다.
- 보조 바디는 Medium 및 Expanded 화면 크기에서만 표시됩니다.

<div class="content-ad"></div>

AdaptiveScaffold을 사용하는 UI를 구현하는 것은 매우 쉽고, 이 위젯은 기본적으로 멋진 애니메이션을 제공합니다. 그러나 사용하면 추가 의존성이 생기며, 동일한 동작은 ShellRoute 및 LayoutBuilder 또는 MediaQuery.sizeOf를 사용하여 구현할 수도 있습니다.

AdaptiveScaffold를 사용했을 때 일부 성능 문제가 있었지만, 이는 1년 전 넘게 일어난 일이라 문제가 아닐 수 있습니다. 그럼에도 불구하고 테스트를 권장합니다.

# 마무리 말씀

이러한 내용과 Fluttercon 토론이 유용했다면 좋을 것 같습니다. 계속 대화를 이어 나가고 싶다면 언제든 LinkedIn으로 연락하세요. Flutter Web 탐험을 즐기세요!