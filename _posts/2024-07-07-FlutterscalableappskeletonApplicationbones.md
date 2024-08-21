---
title: "Flutter 확장 가능한 앱 뼈대 만들기 어플리케이션 기본 구조 완전 정복"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-07 13:13
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Flutter scalable app skeleton. Application bones."
link: "https://medium.com/@orexjeka9/flutter-scalable-app-skeleton-application-bones-49514326deac"
isUpdated: true
---




플러터 3.22.1, Dart 3.4.1, Mac OS, VS Code.

- 설정. 의존성, 코드 스니펫 및 스크립트.
- 애플리케이션 기본 구조. DI, ENV, 폴더 구조, 네트워킹, 자산 및 간단한 디자인 시스템.
- 상태 관리. Redux.
- 네비게이션. AutoRoute 사용.
- 기능. 간단한 앱 기능 구현.
- 단위 테스트. Mockito 사용.

플러터 커뮤니티는 놀라울 만큼 강력합니다. pub.dev에는 많은 멋진 패키지가 있습니다. 그러나 프로젝트에 가장 적합한 최고의 도구와 라이브러리를 선택하는 것은 어떻게 해야 할까요? 그것은 달려있습니다.

# DI

<div class="content-ad"></div>

오브젝트를 만들고 저장하며 연결하는 방법입니다. GetIt + Injectable을 사용할 것이며, Injectable은 GetIt di 컨테이너를 생성하는 도구입니다.

```js
// lib/main.dart

Future<void> main() async {
  runZonedGuarded(
    () async {
      await runAppPreLaunchHandlers(); // get init 호출
      WidgetsFlutterBinding.ensureInitialized();

      runApp(
        RecordApp(
          store: createStore(), // Redux 레퍼런스 참조
        ),
      );

      await runAppPostLaunchHandlers();
    },
    (error, stackTrace) async {
      getIt<Analytics>().trackError(
        AnalyticsEventName.zoneGuardedError,
        stackTrace,
        error: error,
      );
    },
  );
}
```

```js
// lib/core/DI/get_it_initializer.dart

@InjectableInit(
  initializerName: r'$initGetIt',
  preferRelativeImports: true,
  asExtension: true,
)
void configureDependencies() async {
  await getIt.$initGetIt();
}
```

```js
// lib/core/DI/get_it.dart

final GetIt getIt = GetIt.instance;
```

<div class="content-ad"></div>

```js
// lib/core/repositories/env_vars.dart

import 'package:flutter_dotenv/flutter_dotenv.dart';
import 'package:injectable/injectable.dart';
import 'package:record_app/flutter_gen/assets.gen.dart';

@singleton
class EnvVars {
  @FactoryMethod(preResolve: true)
  static Future<EnvVars> create() async {
    await dotenv.load(fileName: _getEnvFileName());
    return Future.value(EnvVars());
  }

  String get baseUrl => dotenv.get('base_api_url');
}

String _getEnvFileName() {
  String? env = const String.fromEnvironment('ENV', defaultValue: 'dev');
  switch (env) {
    case 'dev':
      return Assets.env.aEnvDebug;  // assets/env/.env.debug
    case 'prod':
      return Assets.env.aEnvProd; // assets/env/.env.prod
    case 'profile':
      return Assets.env.aEnvProfile; // assets/env/.env.profile
    default:
      return Assets.env.aEnvDebug;
  }
}
```

assets/env/.env.x 파일에 넣어주세요.

```js
base_api_url = "http://localhost:8080";
```

네트워크 레이어로 가는데, Dio + Retrofit을 선택했습니다. 다시 말해 Retrofit은 dio용 코드 생성 도구로, metadata 주석 내에서 아주 좋은 뼈대를 제공합니다.

<div class="content-ad"></div>

```js
// lib/core/DI/third_party/dio_inject.dart

@module
abstract class DioModuleTag {
  @Named("BaseUrl")
  String get baseUrl => getIt<EnvVars>().baseUrl;

  @singleton
  Dio dio(@Named('BaseUrl') String url) => Dio(BaseOptions(baseUrl: url));
}
```

API 선언하기

```js
// lib/core/services/api/record_app_rest_api.dart

import 'package:dio/dio.dart';
// ignore: depend_on_referenced_packages
import 'package:retrofit/retrofit.dart';

part 'record_app_rest_api.g.dart';

@RestApi()
abstract class RecordAppRestApi {
  factory RecordAppRestApi(Dio dio) = _RecordAppRestApi;

  @POST('/user-query')
  @MultiPart()
  Future<void> userQuery(
    @Part() String name,
    @Part() String phone,
    @Part() List<MultipartFile> files,
  );
}
```

그리고 등록하기

<div class="content-ad"></div>

```js
// lib/core/DI/third_party/retrofit_rest_api_inject.dart

@module
abstract class RecordAppRestApiModuleTag {
  RecordAppRestApi build(Dio dio) => RecordAppRestApi(dio);
}
```

설정 문서에서 생성기를 실행하십시오.

이미 `getIt.RecordAppRestApi()`를 사용하여 RecordAppRestApi 객체에 접근할 수 있습니다. 축하합니다!

# 디자인 시스템

<div class="content-ad"></div>

이제 디자인 시스템에 관해 이야기해보겠습니다. 기본적으로 DS는 구체적인 값에 신경을 덜 쓰고 사전 정의된 추상화 세트로 작업할 수 있도록 도와주는 깔끔한 래퍼입니다. 회사의 규모에 따라 다르지만, 기본 사항을 알아보고 부드럽게 진행해 보겠습니다.

Atoms:

```js
// lib/core/design_system/atoms/size_type.dart

enum SizeType {
  xxl,
  xl,
  l,
  m,
  s,
  xs,
}
```

```js
// lib/core/design_system/atoms/spacings.dart

class Spacings {
  Spacings._();

  // Mobile Spacings
  static const double mobileXXl = 22.0;
  static const double mobileXl = 18.0;
  static const double mobileL = 14.0;
  static const double mobileM = 10.0;
  static const double mobileS = 8.0;
  static const double mobileXs = 6.0;

  // Tablet Spacings
  static const double tabletXXl = 26.0;
  static const double tabletXl = 20.0;
  static const double tabletL = 16.0;
  static const double tabletM = 12.0;
  static const double tabletS = 10.0;
  static const double tabletXs = 8.0;

  // Desktop Spacings
  static const double desktopXXl = 28.0;
  static const double desktopXl = 22.0;
  static const double desktopL = 18.0;
  static const double desktopM = 14.0;
  static const double desktopS = 12.0;
  static const double desktopXs = 10.0;
}
```

<div class="content-ad"></div>

```js
// lib/core/design_system/atoms/text_type.dart

enum TextType {
  heading1,
  heading2,
  heading3,
  body1,
  body2,
  body3,
}
```

```js
// lib/core/design_system/atoms/text_styles.dart

class TextStyles {
  TextStyles._();

  // Mobile
  static const TextStyle mobileHeading1 = TextStyle(
    fontSize: 26.0,
    fontWeight: FontWeight.bold,
  );

  static const TextStyle mobileHeading2 = TextStyle(
    fontSize: 22.0,
    fontWeight: FontWeight.bold,
  );

  static const TextStyle mobileBody1 = TextStyle(
    fontSize: 18.0,
    fontWeight: FontWeight.bold,
  );

  ....

  // Tablet
  static const TextStyle tabletHeading1 = TextStyle(
    fontSize: 28.0,
    fontWeight: FontWeight.bold,
  );
  .....
}
```

Molecules:

```js
// lib/core/design_system/molecules/spacing_fun.dart

double spacingFun(BuildContext context, SizeType type) {
  switch (type) {
    case SizeType.xxl:
      return onDevice(
        context,
        desktop: (context) => Spacings.desktopXXl,
        tablet: (context) => Spacings.tabletXXl,
        mobile: (context) => Spacings.mobileXXl,
      );
    case SizeType.xl:
      return onDevice(
        context,
        desktop: (context) => Spacings.desktopXl,
        tablet: (context) => Spacings.tabletXl,
...
```

<div class="content-ad"></div>

```js
// lib/core/design_system/molecules/text_style_fun.dart

TextStyle textStyleFun(
  BuildContext context,
  TextType type,
  Color color,
) {
  final TextStyle style;
  switch (type) {
    case TextType.heading1:
      style = onDevice(
        context,
        desktop: (context) => TextStyles.desktopHeading1,
        tablet: (context) => TextStyles.tabletHeading1,
        mobile: (context) => TextStyles.mobileHeading1,
      );
    case TextType.heading2:
      style = onDevice(
        context,
        desktop: (context) => TextStyles.mobileHeading2,
      );
    ....

  return style.copyWith(color: color);
}
```

```js
// lib/core/design_system/molecules/theme_data_func.dart

import 'package:flutter/material.dart';

ThemeData themeDataFun() {
  return ThemeData(
    useMaterial3: true,
    fontFamily: 'Roboto',
    colorScheme: const ColorScheme(
      brightness: Brightness.light,
      primary: Color.fromARGB(255, 0, 90, 193), // 필요시 atoms로 추출
      onPrimary: Color.fromARGB(255, 255, 255, 255),
      onPrimaryContainer: Color.fromARGB(255, 213, 213, 213),
      secondary: Color.fromARGB(255, 249, 246, 246),
      onSecondary: Color.fromARGB(255, 0, 0, 0),
      error: Color.fromARGB(255, 193, 0, 16),
      onError: Color.fromARGB(255, 255, 255, 255),
      surface: Color.fromARGB(255, 239, 240, 251),
      onSurface: Color.fromARGB(255, 0, 0, 0),
      shadow: Color.fromARGB(187, 0, 0, 0),
    ),
  );
}
```

그리고 사용자 정의 색상 네이밍 경우:

```js
// lib/core/design_system/color_sheme_ds_extensions.dart

extension ColorShemeDsExtensions on ColorScheme {
  Color get shadowLight => shadow.withAlpha(125);
}
```

<div class="content-ad"></div>

사용법

쉬운 사용을 위한 하나 더 작은 API

```js
// lib/core/design_system/build_context_ds_extensions.dart

extension BuildContextDsExtensions on BuildContext {
  ColorScheme get colorScheme => Theme.of(this).colorScheme;

  TextStyle textStyle(TextType style, Color color) {
    return textStyleFun(this, style, color);
  }

  double spacing(SizeType size) {
    return spacingFun(this, size);
  }
}
```

좋아요! 이제 커스텀 컴포넌트를 확인해보세요 👨🏽‍🎨

<div class="content-ad"></div>

```js
// 구성 요소 예시

body: Container(
  color: context.colorScheme.surface,
  child: Padding(
    padding: EdgeInsets.only(
      left: context.spacing(SizeType.m),
      right: context.spacing(SizeType.),
    ),
    child: Center(
      child: Text(
        '텍스트',
        style: context.textStyle(
          TextType.body2,
          context.colorScheme.onSurface,
        ),
      ),
    ),
  ),
```

응용 프로그램 위젯도 업데이트할 수 있습니다.

```js
// lib/app.dart

class RecordApp extends StatelessWidget {
  final Store<AppState> store;

  const RecordApp({
    required this.store,
    super.key,
  });

  @override
  Widget build(BuildContext context) {
    return StoreProvider(
      store: store,
      child: Layout(
        child: MaterialApp.router(
          theme: themeDataFun(),
          // 라우팅 문서 참조
          routerConfig: getIt<AppRouter>().config(
            navigatorObservers: () => [
              AutoRouteObserver(),
            ],
          ),
        ),
      ),
    );
  }
}
```

## 요약:

<div class="content-ad"></div>

당신의 어플리케이션을 작은 모듈로 분할하면 유지보수와 확장이 쉬워집니다.

일반적인 문제들은 플러터 커뮤니티에 의해 강화된 일반적인 해결책을 가지고 있어요.
🔻 flutter-web은 패키지 간에 덜 지원받고 있어요

자동 생성은 개발을 가속화시키고 코드를 최상의 방법으로 정렬해줍니다.

감사합니다, 그리고 더 많은 기사를 기대해주세요 👏🏽
