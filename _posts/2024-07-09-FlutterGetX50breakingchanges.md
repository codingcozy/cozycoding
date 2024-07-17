---
title: "Flutter GetX 50 주요 변경 사항 및 주의사항"
description: ""
coverImage: "/assets/img/2024-07-09-FlutterGetX50breakingchanges_0.png"
date: 2024-07-09 09:44
ogImage: 
  url: /assets/img/2024-07-09-FlutterGetX50breakingchanges_0.png
tag: Tech
originalTitle: "Flutter. GetX 5.0 breaking changes"
link: "https://medium.com/@yurinovicow/flutter-getx-5-0-breaking-changes-28f381931c96"
---



![Screenshot](/assets/img/2024-07-09-FlutterGetX50breakingchanges_0.png)

오늘은 제 프로젝트 중 하나에서 GetX 패키지를 5.0-release-candidate-6 버전으로 업데이트했어요. 그러자 API 변경이 있었어요. 심각한 문제는 아니지만 문서가 부족해서 조금 시간이 걸렸어요.

그래서 GetX 패키지를 사용하는 나머지 프로젝트들도 확인하기로 했어요.

아래는 결과에요.


<div class="content-ad"></div>

## 변경 사항

- 이전으로 X번 갑니다.

```js
Get.close(2);  // 더 이상 컴파일되지 않음
```

GetX 4.6.6: 라우팅 스택에서 두 개의 경로를 제거했습니다. (한 번에 두 개의 경로로 뒤로 이동합니다.)

<div class="content-ad"></div>

GetX 5.0: 모든 대화 상자, 바텀시트 및 스낵바를 닫습니다. 이것은 좋은 변화라고 말할 수 있어요. 이런 메서드가 있는 것은 좋아요. 이제 메서드의 이름이 메서드가 하는 일을 더 잘 반영하고 있어요.

두 개의 라우트로 돌아가는 방법은 뭐라고 추측하시나요? 답은...

```js
Get.back(times: 2);
```

당연히, 이것 또한 좋은 변화에요. 문제는 항상 작동하지는 않는다는 것이에요. 현재 제가 이해한 바로는 Navigator 2.0을 사용할 때 작동한다고 해요. Navigator 1.0에서는 이렇게 사용해야 해요:

<div class="content-ad"></div>

```js
Get.backLegacy(times: 2);
```

2. Close drawer

```js
Get.back(); 
```

Get.back()으로 서랍을 닫기 위해 사용했었어요. 하지만 GetX 5.0에서는 작동하지 않아요.

<div class="content-ad"></div>

새로운/이전 방법이 있어요:

```js
Get.backLegacy();
```

음, 더 일관성 있었으면 좋겠네요: backLegacy()가 있으면 closeLegacy()는 왜 없는 걸까요?

3. 테마를 동적으로 업데이트하기

<div class="content-ad"></div>

```js
Get.changeTheme(themeData);
```

이전에는 완벽히 작동했어요. 새로운 ThemeData 객체를 만드는 메소드가 있어요. 이 메소드는 색상, 조명 모드, 그리고 앱바의 투명도에 따라 변경 가능한 객체를 생성해요 (색상 또는 모드 또는 투명도를 변경할 수 있어요). GetX 5.0 에서는 조금 일관성이 없는 것 같아요: 때때로 작동하고, 때로는 작동하지 않아요. 문제를 완전히 이해하기 전에 작동하는 해결책을 찾았어요.

```js
Get.changeThemeMode(themeMode); // 매개변수는 ThemeMode의 인스턴스여야 합니다
Get.changeTheme(themeData);     // 매개변수는 ThemeData의 인스턴스여야 합니다
```

테마를 변경하는 것이 아니라, 모드와 테마 둘 다를 변경하도록 했어요.

<div class="content-ad"></div>

이전 방식이 더 좋았죠. ThemeData은 어쨌든 ThemeMode 정보를 보관하죠. 그래도 작동하고 코드 한 줄을 더 추가하면 되니까 문제는 크지 않아요.

4. 익명 라우팅

다음과 같이 코드 작성

```js
Get.off(const PageTwo());
...
Get.to(const PageThree());
```

<div class="content-ad"></div>

GetX 5에서 컴파일되지 않아요.

다음과 같이 작성해야 해요:

```js
Get.off(() => const PageTwo());
...
Get.to(() => const PageThree());
```

5. 익명 및 이름 지정된 라우트 혼합

<div class="content-ad"></div>

다음은 제가 가지고 있는 코드 프로젝트입니다.

```js
위젯 build(BuildContext context) {
    return GetMaterialApp(
      home: const HomePage(),
      getPages: [
        GetPage(name: '/page-three', page: () => const PageThree()),
        GetPage(
            name: '/page-four/:data',
            page: () => const PageFour()) // 동적 라우트
      ],
    );
  }
```

HomePage는 명명된 경로로 정의되지 않았습니다. 따라서 애플리케이션은 페이지 세로부터 시작합니다.

<img src="/assets/img/2024-07-09-FlutterGetX50breakingchanges_1.png" />

<div class="content-ad"></div>

제가 좋아하는 접근 방식은 명명된 routes를 사용하는 것이기 때문에 GetX 방식을 선호합니다.

6. 반응형 비 기본(primitive) 상태 업데이트

```js
class UserController extends GetxController {
  final user = User().obs;

  void updateUser(int age) {
    user.update((value) {
      value.age = count;
    });
  }
}
```

이 코드는 GetX 5에서 컴파일되지 않습니다.

<div class="content-ad"></div>

사용자 업데이트에 전달되는 매개변수 함수는 아래와 같이 값을 반환해야 합니다:

```js
class UserController extends GetxController {
  final user = User().obs;

  void updateUser(int age) {
    user.update((value) {
      value.age = count;
      return value; // 여기에 주목
    });
  }
}
```

지금은 컴파일되지만 작동하지 않습니다. Obx 위젯이 알림을 받지 못합니다.

작동하도록 만들기 위해 refresh()를 추가해야 합니다:

<div class="content-ad"></div>

```js
class UserController extends GetxController {
  final user = User().obs;

  void updateUser(int age) {
    user.update((value) {
      value.age = age;
      return value; 
    });
    user.refresh(); // look here
  }
}
```

Alternatively, you can write it this way:

```js
  void updateUser(int age) {
    user().age = age;
    user.refresh();
  }
```

This shorter version is more concise and, thus, better.

<div class="content-ad"></div>

위의 문제를 해결하는 다른 방법이 있습니다:

```js
void updateUser(int age) {
    user.value.age = age;
    user.refresh();
}
```

위의 문제에 대한 해결책을 찾는 중에 위의 변경 사항을 다루는 이 공식 마이그레이션 안내서를 발견했습니다.

내 모든 프로젝트를 확인했고 지금까지 발견한 모든 호환성 파괴 변경사항입니다. 내 프로젝트는 작고 특정한 기능을 배우기 위해 설계되었습니다.

<div class="content-ad"></div>

어떤 상황에선, 실제 운영 애플리케이션에서는 더 많은 요소가 있을 수 있습니다

우리가 왜 GetX 5.0을 필요로 할까요? 계속해서 4를 사용할 수 있을까요? GetX 5는 Navigator 2.0 API의 사용을 간단하게 만들어 줍니다. 다음 기사에서 자세히 다룰 예정이에요.

읽어 주셔서 감사합니다. 즐거운 코딩되세요!