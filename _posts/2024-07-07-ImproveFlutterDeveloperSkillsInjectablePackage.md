---
title: "플러터 개발자 실력 향상을 위한 Injectable 패키지 사용 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-07 22:22
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Improve Flutter Developer Skills “Injectable Package”"
link: "https://medium.com/@ms3byoussef/improve-flutter-developer-skills-injectable-package-89fca602862e"
isUpdated: true
---



<img src="https://miro.medium.com/v2/resize:fit:1400/1*vPsRYWOqdWjuY8Fa5NLd3w.gif" />

소프트웨어 개발에서 의존성 주입(Dependency Injection, DI)은 느슨한 결합을 유지하고 코드 유지 관리, 모듈성, 테스트 가능성을 향상시키는 디자인 패턴입니다. 이 패턴은 클래스가 자체적으로 객체(의존성)를 생성하는 대신 동작을 위해 필요한 객체들을 제공하는 것을 중심으로 합니다. 이런 책임 분리는 여러 이점을 가져다 줍니다:

- 향상된 테스트 가능성: 단위 테스트 중에 의존성을 위한 모의 객체나 테스트 대역을 쉽게 주입하여 테스트하는 동안 효과적으로 테스트 대상 클래스를 분리하고 동작을 확인할 수 있습니다.
- 향상된 유지 관리성: 의존성을 명시적으로 만들면 그 구현의 변경이 더 쉬워집니다. 구체적인 구현을 교체할 수 있으며 의존 클래스에 영향을 주지 않습니다.
- 증가된 유연성: DI를 통해 환경(예: 개발, 스테이징, 프로덕션) 또는 사용 사례에 따라 다른 의존성 구현을 제공할 수 있습니다.

GetIt
GetIt은 Flutter 및 Dart 프로젝트에서 사용할 수 있는 간단한 서비스 로케이터 패키지입니다. InheritedWidget나 Provider 대신에 이 패키지 덕분에 응용 프로그램 UI에서 중앙 레지스트리 시스템에 등록된 객체에 액세스할 수 있습니다.

<div class="content-ad"></div>

**injectable**  
`Injectable`은 `get_it` 패키지를 위해 작성된 코드 생성 패키지입니다. 이 패키지의 주석을 사용하여 `injectable_generator`를 통해 build_runner를 통해 필요한 모든 코드를 생성할 수 있습니다.

플러터(Flutter)에서 일반적인 의존성 주입(DI) 기술을 설명하고 각각의 장단점과 고려사항을 살펴보겠습니다:

**설명:** 의존성은 클래스의 생성자에 명시적으로 전달됩니다.

**장점:**

<div class="content-ad"></div>

1. 의존성에 대해 명확하고 명시적입니다.

2. 느슨한 결합을 장려합니다.

3. 단순한부터 중간 정도 복잡한 응용 프로그램에 적합합니다.

고려 사항:

<div class="content-ad"></div>

클래스의 종속성이 많을 경우 생성자가 길어질 수 있습니다.

테스트 목적으로 추가적인 보일러플레이트 코드가 필요할 수 있습니다.

- 설명: 클래스 내 setter 메서드를 통해 종속성이 주입됩니다.
- 장점:
- 동적으로 종속성을 변경하는 데 더 많은 유연성을 제공합니다.
- 고려 사항:
- 생성자 주입과 비교해서 종속성에 대해 덜 명시적입니다.
- 종속성이 제대로 설정되지 않으면 런타임 오류의 가능성이 있습니다.

- 설명: 종속성의 인스턴스를 저장하고 제공하는 전역 레지스트리.
- 장점:
- 애플리케이션 어디서든 종속성에 쉽게 접근할 수 있습니다.
- 싱글톤이나 애플리케이션 전체에 걸쳐 공유되는 종속성에 유용할 수 있습니다.
- 고려 사항:
- 서비스 로케이터 자체에 강한 결합을 도입합니다.
- 남용시 코드를 유지보수하기 어렵고 테스트하기 어려울 수 있습니다.

<div class="content-ad"></div>

- 설명: 의존성 관리를 위한 공급자 트리를 제공하는 인기 있는 서드파티 패키지입니다.
- 장점:
  - 위젯 트리 전반에 걸쳐 의존성 주입을 간편하게 해줍니다.
  - 의존성 관리에 명확하고 선언적인 접근법을 촉진합니다.
- 고려사항:
  - 다른 기술에 비해 추가 설정이 필요할 수 있습니다.

이제 애플리케이션에 이를 추가할 수 있습니다:

```js
flutter pub add get_it
flutter pub add injectable
flutter pub add injectable_generator --dev
flutter pub add build_runner --dev
```

- 새 Dart 파일을 만들어서 GetIt 인스턴스용 전역 변수를 정의하세요.

<div class="content-ad"></div>

```js
import 'package:get_it/get_it.dart';

/// 모든 모듈에서 주입을 받기 위해 사용해야 합니다.
GetIt getIt = GetIt.instance;
```

- 최상위 함수를 정의하세요 (configureDependencies라고 부르겠습니다) 그리고 @injectableInit으로 주석을 달아주세요.
- 코드에서 생성된 생성된 다트 파일을 import하세요. 이는 파일 이름에 따라 @InjectableInit으로 주석이 달린 함수가 따를 것입니다. 예를 들어 app_injection_container.config.dart입니다.
- 생성된 확장 함수 getIt.init()을 호출하거나 configure 함수 내에서 사용자 정의 초기화자 이름을 호출하세요.

```js
import 'package:injectable/injectable.dart';
@InjectableInit(
  initializerName: 'configureDependencies', // 필요시 사용자 정의
  preferRelativeImports: true, // 코드 가독성 향상
  asExtension: true, // 편리함: `GetIt.I.get<T>()` 사용
)
void configureDependencies(final $Env env) => $env.initFromEnvironment();
```

의존성 등록하기 (Injectable 클래스):

<div class="content-ad"></div>

- 제너레이터가 주입할 수 있는 것을 나타내기 위해 서비스나 프로바이더 클래스에 @injectable을 추가하세요:

```js
// api_service.dart
@injectable
class ApiService {
  Future<String> fetchData() async {
    // ... (당신의 API 로직)
  }
}
// local_storage_service.dart
@injectable
class LocalStorageService {
  Future<String> getLocalData() async {
    // ... (당신의 로컬 저장소 로직)
  }
}
```

제너레이터 실행:

- 필요한 파일을 생성하기 위해 빌드 명령을 실행하세요 (보통 injection.g.dart와 같은 파일이 생성됩니다):

<div class="content-ad"></div>

```yaml
flutter pub run build_runner watch  # 변경 사항을 만들 때마다 지속적으로 생성합니다.
# 또는
flutter pub run build_runner build   # 일회성으로 생성합니다
```

생성된 코드 활용:

- main.dart에서 생성된 파일(injection.g.dart 등)을 import하세요:

```yaml
import 'package:injectable/injectable.dart';
import 'injection.g.dart'; // 생성된 파일
@injectableInit
void configureDependencies() => $configureDependencies();
void main() async {
configureDependencies();
runApp(MyApp());
}
```

<div class="content-ad"></div>

앱을 실행하기 전에 configureDependencies()를 사용하여 종속성을 초기화하세요. 이 함수는 주입 가능한 환경을 설정하고 의존성을 등록합니다.

# 결론

Flutter에는 내장된 DI 프레임워크가 없지만, 주입은 사용하기 쉬운 코드 생성 기능과 다양한 DI 스타일 지원으로 널리 알려진 옵션입니다. 특히 프로젝트가 커지고 복잡해질수록 Flutter 애플리케이션의 종속성을 관리하는 데 유용한 자산이 될 수 있습니다.

# 참고문헌

<div class="content-ad"></div>

https://www.youtube.com/watch?v=KNcP8z0hWqs

https://pub.dev/packages/injectable#setup
