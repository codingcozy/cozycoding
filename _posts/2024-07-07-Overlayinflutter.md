---
title: "플러터에서 오버레이 사용 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-07 22:19
ogImage:
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Overlay in flutter"
link: "https://medium.com/@rkishan516/overlay-in-flutter-45abf2859fe2"
---

안녕하세요! 플러터에서 오버레이는 명령적인 방식으로 작동한다는 것을 아시다시피, 그러나 만약 제가 당신에게 선언적인 방식으로 사용할 수 있다고 말한다면 어떨까요?

![이미지](https://miro.medium.com/v2/resize:fit:996/1*VnOvOp07Y--0x3SoJNTaHg.gif)

네, 플러터 포털을 사용하면 매우 쉽게 할 수 있어요. 플러터 포털을 사용하여 선언적 오버레이를 만드는 방법을 살펴보겠습니다.

```js
// flutter_portal을 pub 종속성에 추가하세요
flutter_portal: ^latest_version
```

<div class="content-ad"></div>

이제 위젯 위에 검은색 바리어를 만들 수 있다고 가정해 봅시다.

```js
PortalTarget(
  visible: visible,
  closeDuration: closeDuration,
  anchor: Aligned.center,
  portalFollower: GestureDetector(
     behavior: HitTestBehavior.opaque,
     onTap: onClose,
     child: TweenAnimationBuilder<Color>(
      duration: kThemeAnimationDuration * 2,
      tween: ColorTween(
         begin: Colors.transparent,
         end: visible ? const Color(0x8F0E0E0E) : Colors.transparent,
      ),
      builder: (context, color, child) {
        return ColoredBox(color: color);
      },
    ),
  ),
  child: child,
)
```

위 코드를 사용하면 visible이 true일 때 위젯 위에 검은 색을 겹쳐 놓을 수 있습니다. 여기서 child는 우리가 오버레이를 그리는 위젯을 가리키고, portalFollower는 오버레이로 동작하는 위젯을 가리킵니다. Anchor에 따라 portalFollower를 child에 대해 정렬할 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1000/1*4MW3FF2xspdPHLlbu-1Zcw.gif)

<div class="content-ad"></div>

안녕하세요! 우리는 선언적 방식으로 오버레이를 성공적으로 생성했어요.

다음 글에서 만나요!
