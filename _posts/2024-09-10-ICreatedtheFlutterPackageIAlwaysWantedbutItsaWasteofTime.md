---
title: "Flutter 패키지 직접 만드는 것이 시간 낭비인 이유"
description: ""
coverImage: "/assets/img/2024-09-10-ICreatedtheFlutterPackageIAlwaysWantedbutItsaWasteofTime_0.png"
date: 2024-09-10 19:53
ogImage: 
  url: /assets/img/2024-09-10-ICreatedtheFlutterPackageIAlwaysWantedbutItsaWasteofTime_0.png
tag: Tech
originalTitle: "I Created the Flutter Package I Always Wanted, but Its a Waste of Time"
link: "https://medium.com/codex/i-created-the-flutter-package-i-always-wanted-but-its-a-waste-of-time-19e509def1f6"
isUpdated: true
updatedAt: 1726022884894
---


이미지 추가:

![이미지](/assets/img/2024-09-10-ICreatedtheFlutterPackageIAlwaysWantedbutItsaWasteofTime_0.png)

# 문제점

웹 개발 환경에서 왔는데, Flutter를 처음 배울 때 내게는 이해하기 어렵다고 느꼈습니다. 웹 개발에서는 HTML이 콘텐츠를 담당하고, CSS가 스타일링을 담당하며, JavaScript가 상호작용을 처리합니다. 이 구조는 styled-components나 Tailwind CSS와 같은 라이브러리를 사용할 때에도 어느 정도 유지됩니다. 그러나 Flutter에서는 모든 것이 위젯을 중심으로 돌아갑니다. 패딩을 추가하려면 Padding 위젯을 사용하고, 불투명도를 조절해야 한다면 Opacity 위젯을 사용합니다. 이 위젯 기반의 접근 방식은 깊게 중첩된 위젯 트리를 만들어냅니다:

```js
Scaffold(
  backgroundColor: Colors.lightBlue,
  body: Center(
    child: Transform.rotate(
      angle: math.pi / 2,
      child: Opacity(
        opacity: 0.8,
        child: Container(
          color: Colors.yellow,
          margin: const EdgeInsets.all(8.0),
          child: const Text("Welcome to Flutter"),
        ),
      ),
    ),
  ),
);
```

<div class="content-ad"></div>

여기서 확인할 수 있듯이, 여기에 있는 유일한 "콘텐츠 관련" 위젯은 Text 위젯이지만 이미 여섯 개의 레이어가 있습니다.

또 다른 문제는 ColorScheme, TextTheme에 접근하는 데 많은 보일러플레이트 코드가 필요하며 ThemeExtensions를 언급하지 않았습니다. 🤕

```js
Theme.of(context).colorScheme;
Theme.of(context).textTheme;
Theme.of(context).extension<MyTheme>()!;
```

# (가능한) 해결책

<div class="content-ad"></div>

위 문제들을 해결하기 위해, 저는 새로운 패키지인 flutter_style_extensions를 만들었습니다. 이 패키지는 확장 메서드의 힘을 활용합니다.

우선, 이 패키지는 모든 스타일 관련 위젯에 대한 확장 기능을 제공합니다: SizedBox, AspectRatio, Transform.rotate 등등. 이 패키지를 사용하면 위젯 트리를 아래와 같이 간소화할 수 있습니다:

```js
Scaffold(
  backgroundColor: Colors.lightBlue,
  body: const Text("플러터에 오신 것을 환영합니다")
      .margin(const EdgeInsets.all(8.0))
      .backgroundColor(Colors.yellow)
      .opacity(0.8)     
      .rotate(math.pi / 2)
      .center(),
);
```

훨씬 더 깔끔해 보이죠?

<div class="content-ad"></div>

또한, BuildContext를 통해 테마 관련 데이터에 쉽게 액세스할 수 있습니다:

```js
context.theme;
context.colorScheme;
context.textTheme;
// 만약 테마 확장 클래스가 MyCustomTheme으로 명명된 경우
context.themeExtension<MyCustomTheme>();
```

# 예상치 못한 경쟁

저는 같은 플러터 개발자인 친구와 이 패키지를 공유했고, 그가 비슷한 것을 사용했다는 사실을 언급했습니다: styled_widget. 더 놀라운 것은 styled_widget의 인기입니다. 800개 이상의 좋아요를 받았는데, 명백히 플러터 커뮤니티에서 호응을 얻었습니다. 이것은 제게 약간의 타격이었습니다. 왜냐하면 충분한 조사 없이 패키지를 만든 것이 아닌가 싶어졌기 때문입니다. 😭

<div class="content-ad"></div>

그러나 제 생각에는 우리의 개념이 약간 다르다고 믿습니다. styled_widget은 다양한 위젯에서 광범위한 기능을 제공하지만, flutter_style_extensions 같은 내 패키지는 특히 스타일 관련 위젯에 중점을 둡니다. 이 패키지는 개발자들이 응용 프로그램의 스타일링을 강화하고 싶지만 필요하지 않은 추가 기능의 오버헤드 없이 더 가볍고 간결하게 만들어 줍니다.

내 의견으로는, 강력한 애니메이션 기능이 필요하다면 애니메이션 처리에 더 탁월한 flutter_animate을 사용하는 것이 좋습니다. 경쟁을 무릅쓰더라도, flutter_style_extensions의 간결함과 목표 기능성을 발견해 주는 개발자들이 많아졌으면 좋겠습니다.Flutter 도구상자에 보완 도구로서의 가치를 찾게 해주기를 바랍니다.