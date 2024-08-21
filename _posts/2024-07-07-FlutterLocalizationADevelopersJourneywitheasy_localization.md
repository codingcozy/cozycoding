---
title: "Flutter 다국어 지원 easy_localization 활용 개발자 여정"
description: ""
coverImage: "/assets/img/2024-07-07-FlutterLocalizationADevelopersJourneywitheasy_localization_0.png"
date: 2024-07-07 02:45
ogImage: 
  url: /assets/img/2024-07-07-FlutterLocalizationADevelopersJourneywitheasy_localization_0.png
tag: Tech
originalTitle: "Flutter Localization: A Developer’s Journey with easy_localization"
link: "https://medium.com/@juzuli.coinedone/flutter-localization-a-developers-journey-with-easy-localization-ebf5d93b871a"
isUpdated: true
---





<img src="/assets/img/2024-07-07-FlutterLocalizationADevelopersJourneywitheasy_localization_0.png" />

회사의 성공적인 로컬 앱을 발표하여 글로벌 관객을 대상으로 준비하는 새로운 과제를 맡게 되었습니다. 당신이 승낙할 경우, easy_localization 패키지를 사용하여 로컬라이제이션을 구현해야 합니다. 이 안내서는 플러터 앱을 다국어로 만드는 흥미로운 여정에서 여러분을 도울 동반자가 될 것입니다.

# 제 1장: 여정을 위한 준비

로컬라이제이션 모험을 떠나기 전에 우리는 필수품을 담은 가방을 챙겨야 합니다.

<div class="content-ad"></div>

# 단계 1: 프로젝트 설정

1.1. 먼저 easy_localization을 pubspec.yaml에 추가해주세요:

```js
dependencies:
  flutter:
    sdk: flutter
  easy_localization: ^3.0.0
```

1.2. 패키지를 가져오기 위해 flutter pub get을 실행해주세요. 앞으로의 여정을 위한 준비물을 준비하는 것과 같아요.

<div class="content-ad"></div>

1.3. 이제 프로젝트 구조를 만들어 봅시다:

```js
project-name
├── assets
│   └── translations
│       ├── en.json
│       ├── es.json
│       └── fr.json
└── lib
    └── translations
        ├── codegen_loader.g.dart
        └── locale_keys.g.dart
```

이 구조는 로컬라이제이션 환경을 안내하는 지도 역할을 할 거에요.

# Chapter 2: 기본 캠프 설정하기

<div class="content-ad"></div>

우리의 준비물을 싸면, 이제 우리의 기지 캠프를 설정할 시간입니다 — 우리 지역화된 앱의 기초를 마련하는 것이죠.

# 단계 2: easy_localization 초기화

main.dart에서 easy_localization을 초기화할 거에요. 이는 우리의 범용 번역기를 설정하는 것과 같아요:

```js
import 'package:easy_localization/easy_localization.dart';
```

<div class="content-ad"></div>

```js
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await EasyLocalization.ensureInitialized();
  runApp(
    EasyLocalization(
      supportedLocales: [Locale('en'), Locale('es'), Locale('fr')],
      path: 'assets/translations',
      fallbackLocale: Locale('en'),
      child: MyApp(),
    ),
  );
}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      localizationsDelegates: context.localizationDelegates,
      supportedLocales: context.supportedLocales,
      locale: context.locale,
      home: HomeScreen(),
    );
  }
}
```

# 제 3장: 우리의 구문 사전 만들기

이제 기본 캠프가 준비되었으니, 각 언어에 대한 우리의 구문 사전을 만드는 시간입니다.

# 단계 3: 번역 파일 생성하기


<div class="content-ad"></div>

자산/번역 폴더에 각 언어에 대한 JSON 파일을 생성합니다. 먼저 en.json 파일을 만들어 보겠습니다:

```js
{
  "hello": "안녕하세요!",
  "welcome": "저희 앱에 오신 걸 환영합니다, {name}님!",
  "items_count": {
    "zero": "아이템이 없습니다",
    "one": "1개의 아이템",
    "other": "{} 개의 아이템"
  }
}
```

es.json 및 fr.json 파일도 유사하게 생성하여 내용을 적절히 번역합니다. 이 파일들은 각 언어 영역에서 우리의 신뢰할 수 있는 안내서가 될 것입니다.

# 4장: 코드 생성의 마법

<div class="content-ad"></div>

우리의 언어 학습서가 준비되었으니, 마법을 부리고 Dart 파일을 생성할 때입니다.

# 단계 4: 로컬라이제이션 파일 생성

아래 명령어를 실행하여 필요한 Dart 파일을 생성하세요:

```js
flutter pub run easy_localization:generate -S "assets/translations" -O "lib/translations"
flutter pub run easy_localization:generate -S "assets/translations" -O "lib/translations" -o "locale_keys.g.dart" -f keys
```

<div class="content-ad"></div>

lib/translations 디렉터리에 codegen_loader.g.dart 및 locale_keys.g.dart이 생성됩니다. 이들은 여러 언어를 구사하는 앱을 돕는 귀하의 유니버설 번역기로 생각할 수 있습니다.

# 제5장: 다국어로 말하기

이제 흥미로운 파트인 다국어 앱 만들기가 시작됩니다!

# 단계 5: 번역 사용하기

<div class="content-ad"></div>

5.1. 생성된 로캘 키를 Dart 파일에 가져오세요:

```js
import 'package:your_project/translations/locale_keys.g.dart';
import 'package:easy_localization/easy_localization.dart';
```

5.2. 위젯에서 번역 사용하기:

```js
Text(LocaleKeys.hello.tr())
```

<div class="content-ad"></div>

5.3. 인수를 사용한 번역:

```js
Text(LocaleKeys.welcome.tr(args: ['John']))
```

5.4. 복수:

```js
Text(LocaleKeys.items_count.plural(itemCount))
```

<div class="content-ad"></div>

당신의 앱이 이제 다른 언어로 말하기 시작했어요!

# 장 6: 고급 언어 능력

우리 앱이 더 유창해지면, 일부 고급 언어 기술을 가르쳐 보겠어요.

# 단계 6: 동적 콘텐츠 처리

<div class="content-ad"></div>

6.1. 위치 인수를 사용하여:

```js
Text(LocaleKeys.greet_name.tr(args: ['John', 'Doe']))
```

6.2. 이름이 지정된 인수를 사용하여:

```js
Text(LocaleKeys.user_info.tr(namedArgs: {'name': 'Alice', 'age': '30'}))
```

<div class="content-ad"></div>

# 단계 7: 성별별 번역

```js
Text(LocaleKeys.gender_message.tr(gender: userGender))
```

# 단계 8: 연결된 번역

```js
Text(LocaleKeys.dialog_save_button.tr())
```

<div class="content-ad"></div>

# 제 7 장: 선택의 힘

사용자들에게 선호하는 언어를 선택할 수 있는 권한을 부여해보세요.

# 단계 9: 로케일 변경

```js
ElevatedButton(
  onPressed: () => context.setLocale(Locale('es')),
  child: Text('스페인어로 변경'),
)
```

<div class="content-ad"></div>

# 단계 10: 로컬라이제이션 속성에 접근하기

```js
print(context.locale.toString())
print(context.supportedLocales)
print(context.fallbackLocale)
```

# 맺음말: 언어 학습 여정을 위한 모범 사례

- 새로운 번역을 추가한 후에는 항상 구문서를 업데이트하세요 (코드 생성 실행).
- JSON 파일에서 의미 있는 키를 사용하세요. 이들은 코드에서 안내표 같은 존재입니다.
- 앱을 모든 언어로 테스트하세요. 누군가를 실수로 모욕하고 싶지 않으시겠죠!
- 앱 어휘가 확장되면 번역 관리 시스템을 사용하는 것을 고려하세요.
- 번역 파일을 최신 상태로 유지하세요. 언어는 살아있는 것이니, 앱도 그에 걸맞게 살아갈 필요가 있습니다.

<div class="content-ad"></div>

이 여정을 따라가면서 당신의 앱이 세계 시민으로 변모했어요. 전 세계의 사용자와 대화할 준비가 되었죠. 로컬라이제이션은 계속되는 모험이에요. 계속해서 탐험하고 배우며, 그러면 앱은 계속해서 다양한 언어와 문화를 통해 사람들과 연결될 거예요.

행복한 코딩하세요! 앱의 언어적 여정이 성공적이길 바랍니다! 🌍🚀