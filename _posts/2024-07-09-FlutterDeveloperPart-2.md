---
title: "플러터 개발자 가이드 - 파트 2"
description: ""
coverImage: "/assets/img/2024-07-09-FlutterDeveloperPart-2_0.png"
date: 2024-07-09 22:38
ogImage: 
  url: /assets/img/2024-07-09-FlutterDeveloperPart-2_0.png
tag: Tech
originalTitle: "Flutter — Developer — Part-2"
link: "https://medium.com/@rubanraghavendar/flutter-developer-part-2-fb922bb177a7"
isUpdated: true
---




플러터: 완전한 로드맵 시리즈...

안녕하세요, 친구여, 저의 플러터 로드맵 시리즈에 다시 오신 것을 환영합니다. 만약 제 "플러터-개발자-파트1"을 놓치셨다면, 먼저 그것을 완료하고 돌아오세요...

프로그램 실행의 흐름을 제어하는 제어 흐름문은 다트에서 다음과 같은 주요 유형이 있습니다:

- if-else: 부울식에 따라 조건부로 코드를 실행합니다.
- for 루프: 특정 횟수만큼 코드 블록을 반복합니다.
- while 루프: 지정된 조건이 true인 경우 코드 블록을 반복합니다.
- do-while 루프: while 루프와 유사하지만 조건을 평가하기 전에 코드 블록이 한 번 실행됩니다.
- switch-case: 값을 기준으로 실행할 여러 코드 블록 중 하나를 선택합니다.
- break: 루프를 일찍 종료합니다.
- continue: 현재 반복을 건너뛰고 다음 반복으로 계속합니다.

<div class="content-ad"></div>

이러한 제어 흐름 문은 Dart 프로그램에서 복잡한 논리를 만들고 실행 흐름을 제어하는 데 사용할 수 있습니다.

![image](https://miro.medium.com/v2/resize:fit:700/1*2Hm-8CYeAnritxDmorbPjg.gif)

For Loop:

for 루프는 지정된 횟수만큼 반복해서 코드 블록을 실행할 수 있는 제어 흐름 구조입니다. Dart는 다양한 종류의 for 루프를 제공하여 다양한 루핑 시나리오에 적합한 방법을 제공합니다.

<div class="content-ad"></div>

Dart에서 for 루프의 기본 구문은 다음과 같습니다:

```js
for (initialization; condition; update) {
  // 각 반복에서 실행될 코드
}
```

- initialization: 루프 변수는 루프가 시작하기 전에 초기화됩니다.
- condition: 조건이 true인 한 루프가 계속됩니다.
- update: 각 반복 후에 루프 변수가 업데이트됩니다.

Dart에서 다양한 종류의 for 루프 예제를 살펴봅시다:

<div class="content-ad"></div>

- 표준 for 루프: 이것은 가장 흔한 유형의 for 루프입니다. 이것은 정확한 반복 횟수를 알 때 사용됩니다.

```js
for (int i = 0; i < 5; i++) {
  print('반복 $i');
}
```

2. for-in 루프: for-in 루프는 컬렉션 (예: 목록, 세트, 맵)의 요소를 반복하는 데 사용됩니다.

```js
List<String> fruits = ['사과', '바나나', '오렌지'];
for (String fruit in fruits) {
  print(fruit);
}
```

<div class="content-ad"></div>

3. forEach 루프: forEach 루프는 컬렉션 요소를 반복하는 더 간결한 방법입니다.

```js
List<String> fruits = ['Apple', 'Banana', 'Orange'];
fruits.forEach((fruit) {
  print(fruit);
});
```

While 루프:

Dart에서 while 루프는 특정 조건이 참인 동안 코드 블록을 반복적으로 실행할 수 있는 제어 흐름 구조입니다. 루프는 각 반복 전에 조건을 평가하고, 조건이 참이면 코드 블록이 실행됩니다. 조건이 거짓이 되면 루프가 종료되고, 프로그램은 루프 다음의 다음 문으로 진행합니다.

<div class="content-ad"></div>

Dart에서 while 루프의 기본 구문을 보여드릴게요:

```js
while (condition) {
  // 각 반복에서 실행될 코드
}
```

- condition: 조건이 true인 경우 루프가 계속됩니다.

Dart에서 while 루프를 사용하는 몇 가지 예제를 살펴보겠습니다:

<div class="content-ad"></div>

- 기본 while 루프: 이것은 while 루프의 기본적인 사용법입니다. 조건이 참인 한 계속 코드 블록을 실행합니다.

```js
void main() {
  int count = 0;

  while (count < 5) {
    print('Count: $count');
    count++;
  }
}
```

출력:

<img src="/assets/img/2024-07-09-FlutterDeveloperPart-2_0.png" />

<div class="content-ad"></div>

Do-While:

do-while 루프: do-while 루프는 while 루프와 유사하지만 조건을 확인하기 전에 코드 블록이 한 번은 실행됨을 보장합니다.

```js
void main() {
  int count = 0;

  do {
    print('Count: $count');
    count++;
  } while (count < 5);
}
```

while 루프는 특정 조건이 계속 참인 경우 코드 블록을 반복할 때 유용합니다. 무한 루프를 만들지 않으려면 반드시 나중에 루프 조건이 거짓이 되도록 해야 합니다.

<div class="content-ad"></div>

Break:

**break Statement:** 루프 실행을 즉시 종료하고 프로그램이 루프를 종료하고 루프 다음 문장을 계속 진행하도록 하는데 사용됩니다.

```js
void main() {
  for (int i = 0; i < 5; i++) {
    if (i == 3) {
      break; // i가 3일 때 루프를 종료합니다.
    }
    print('Value of i: $i');
  }
  print('Loop complete');
}
```

Continue:

<div class="content-ad"></div>

continue 문: continue 문은 루프의 현재 반복을 건너뛰고 다음 반복으로 넘어가기 위해 사용됩니다.

```js
void main() {
  for (int i = 0; i < 5; i++) {
    if (i == 2) {
      continue; // i가 2일 때, 루프의 나머지 부분을 건너뜁니다.
    }
    print('i의 값: $i');
  }
}
```

여기서:

where 키워드: where 키워드는 주어진 조건에 따라 요소를 필터링하는데 자주 사용됩니다. 주로 List, Set 및 Iterable 객체와 함께 사용되어 지정된 조건을 충족하는 요소만 포함하는 새 컬렉션을 만드는 데 사용됩니다.

<div class="content-ad"></div>

일반적으로 where 키워드가 사용되는 방식은 다음과 같습니다:

```js
Iterable<T> where(bool test(T element))
```

- T는 컬렉션 내 요소의 유형을 나타냅니다.
- test는 T 유형의 요소를 가져와 해당 요소를 필터된 컬렉션에 포함해야 하는지 여부를 나타내는 부울 값으로 반환하는 함수입니다.

where 키워드를 사용하여 리스트에서 요소를 필터링하는 간단한 예제를 살펴보겠습니다:

<div class="content-ad"></div>

```js
void main() {
  List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

  List<int> evenNumbers = numbers.where((number) => number % 2 == 0).toList();

  print('Original numbers: $numbers');
  print('Even numbers: $evenNumbers');
}
```

Output:

<img src="/assets/img/2024-07-09-FlutterDeveloperPart-2_1.png" />

이 예제에서는 숫자 목록이 있습니다. where 메서드를 사용하여 목록에서 짝수만 필터링하여 짝수 목록을 만듭니다. 조건 (number) => number % 2 == 0은 숫자가 짝수인지 확인합니다. 마지막으로, 필터링된 Iterable을 List로 다시 변환하기 위해 toList()를 사용합니다.

<div class="content-ad"></div>

"Branches"는 일반적으로 코드가 특정 조건에 따라 결정을 내릴 수 있도록 하는 조건문을 가리킵니다. Dart에는 if 문과 switch 문 두 가지 주요 분기문 유형이 제공됩니다.

If:

if 문: if 문은 지정된 조건이 참인 경우 코드 블록을 실행할 수 있게 합니다.

```js
void main() {
  int number = 10;

  if (number > 5) {
    print('Number is greater than 5');
  } else {
    print('Number is not greater than 5');
  }
}
```

<div class="content-ad"></div>

```js
if-else:

if-else if-else 문: else if를 사용하여 여러 조건을 연결하여 대체 경우를 제공할 수 있습니다. 

void main() {
  int number = 7;

  if (number > 10) {
    print('Number is greater than 10');
  } else if (number > 5) {
    print('Number is greater than 5 but not greater than 10');
  } else {
    print('Number is not greater than 5');
  }
}

<div class="content-ad"></div>

switch 문: switch 문은 지정된 표현식에 따라 실행할 코드 블록 중 하나를 선택할 수 있게 해줍니다.

void main() {
  String color = 'red';

  switch (color) {
    case 'red':
      print('정지!');
      break;
    case 'yellow':
      print('속도를 줄이세요');
      break;
    case 'green':
      print('출발');
      break;
    default:
      print('알 수 없는 신호');
  }
}

개발 환경 설정

![이미지](/assets/img/2024-07-09-FlutterDeveloperPart-2_2.png)

<div class="content-ad"></div>

Flutter 개발 환경을 설정하려면 다음 소프트웨어를 설치해야 합니다:

- Flutter SDK: 공식 웹사이트 (https://flutter.dev/docs/get-started/install)에서 Flutter SDK의 최신 버전을 다운로드하여 설치합니다.
- 통합 개발 환경 (IDE): Android Studio, Visual Studio Code, IntelliJ IDEA 또는 사용자의 선택에 따라 다른 IDE를 사용할 수 있습니다.
- 에뮬레이터 또는 실제 장치: Flutter 앱을 실행하고 테스트하기 위해 에뮬레이터나 실제 장치를 사용할 수 있습니다. Android Studio에서 제공하는 Android 에뮬레이터를 사용하거나 실제 Android 또는 iOS 장치를 사용할 수 있습니다.
- Git: 버전 관리에 사용되는 Git은 Flutter 개발을 위해 권장되며, https://git-scm.com/에서 Git을 다운로드하여 설치할 수 있습니다.
- Dart SDK: Dart는 Flutter에서 사용되는 프로그래밍 언어이며, Flutter 앱을 개발하는 데 필요한 Dart SDK가 필요합니다. Dart SDK는 Flutter SDK에 포함되어 있습니다.

필요한 모든 소프트웨어를 설치한 후, Flutter CLI 또는 IDE를 사용하여 새로운 Flutter 프로젝트를 생성하고 앱을 개발할 수 있습니다.

Flutter CLI (Command Line Interface)는 Flutter 프로젝트와 상호 작용하고 관리하는 데 사용되는 일련의 도구입니다. 이는 명령줄에서 Flutter 애플리케이션을 생성, 빌드, 테스트, 실행하고 종속성 및 패키지를 관리하는 명령을 제공합니다.

<div class="content-ad"></div>

다음은 자주 사용되는 Flutter CLI 명령어 몇 가지입니다:

- 새로운 Flutter 프로젝트 생성: 새로운 Flutter 프로젝트를 만들려면 `flutter create` 명령어 뒤에 프로젝트 이름을 붙여 사용하면 됩니다.

flutter create my_app

2. 앱 실행: `flutter run` 명령어는 연결된 장치(에뮬레이터 또는 실제 장치)에서 Flutter 앱을 실행합니다.

<div class="content-ad"></div>

flutter run

3. 앱 빌드하기: 프로덕션용 Flutter 앱을 빌드하려면 apk, appbundle, ios 등의 옵션을 사용하여 flutter build 명령을 사용할 수 있습니다.

flutter build apk --release

4. 의존성 관리: flutter pub 명령을 사용하여 프로젝트의 종속성을 추가, 업데이트 또는 삭제할 수 있습니다.

<div class="content-ad"></div>

flutter pub get      # 의존성 설치하기
flutter pub outdated # 구식된 의존성 확인하기
flutter pub upgrade  # 의존성 업데이트하기

5. 테스트 실행하기: flutter test 명령어는 프로젝트에 정의된 테스트를 실행합니다.

flutter test

6. 코드 포매팅: flutter format 명령어는 코드를 Dart의 스타일 가이드에 따라 포맷합니다.

<div class="content-ad"></div>

flutter format

7. 코드 분석: flutter analyze 명령어를 사용하여 코드를 잠재적인 문제와 오류를 분석합니다.

flutter analyze

8. 위젯 생성: 위젯 템플릿을 사용하여 flutter create 명령어로 위젯에 대한 보일러플레이트 코드를 생성할 수 있습니다.

<div class="content-ad"></div>

flutter create my_widget --template=widget

9. 모든 플러터 지원 명령어를 확인하려면 다음 명령어를 실행하세요.

flutter --help --verbose

10. 플러터 SDK의 현재 버전 및 프레임워크, 엔진 및 도구를 확인하려면 다음과 같이 실행하세요.

<div class="content-ad"></div>

flutter --version

이것들은 Flutter CLI에서 사용 가능한 많은 명령어 중 일부 예시에 불과합니다. 사용 가능한 명령어 및 옵션의 전체 목록을 보려면 flutter --help 또는 flutter `command` --help를 실행할 수 있습니다.

Flutter CLI를 사용하여 Flutter 프로젝트를 효율적으로 관리하고 앱을 개발하고 테스트하며 개발 워크플로우를 간소화할 수 있습니다.

Flutter 명령어:

<div class="content-ad"></div>

```
<img src="/assets/img/2024-07-09-FlutterDeveloperPart-2_3.png" />

<img src="/assets/img/2024-07-09-FlutterDeveloperPart-2_4.png" />

다트와 플러터에서 명령줄 및 서버 라이브러리 및 패키지는 각각 명령줄 응용 프로그램 및 서버 측 응용 프로그램에서 사용되도록 설계된 소프트웨어 구성 요소를 가리킵니다. 이러한 라이브러리 및 패키지는 개발자가 효율적이고 효과적인 명령줄 인터페이스 (CLI) 및 서버 응용 프로그램을 구축하는 데 도움이 되는 도구, 유틸리티 및 기능을 제공합니다.

명령줄 라이브러리 및 패키지:


<div class="content-ad"></div>

Dart의 명령행 라이브러리와 패키지들은 명령행 응용프로그램을 쉽게 만들 수 있도록 설계되었어요. 이들은 명령행 인자를 구문 분석하거나 출력 형식을 지원하고 사용자와 상호 작용하는 기능을 제공해요. 일반적인 명령행 라이브러리로는 다음과 같은 것들이 있어요:

- args: 명령행 인자를 구문 분석하기 위한 라이브러리.
- shelf: HTTP 서버를 구축하기 위한 패키지.
- dart:io: 파일, 디렉토리, 소켓 등과 작업하기 위한 클래스를 제공하는 Dart의 핵심 I/O 라이브러리.
- ansi_term: 터미널에서 색상과 스타일을 위한 ANSI 이스케이프 코드로 텍스트 형식을 지원하는 패키지.

서버 라이브러리 및 패키지:

Dart의 서버 라이브러리와 패키지는 네트워크를 통해 요청을 처리하고 서비스를 제공하는 서버 사이드 응용 프로그램을 개발하는 것에 사용됩니다. 이들은 주로 HTTP 요청 처리, 데이터베이스 관리, 동시성 처리 등을 포함한 기능을 제공해요. 인기 있는 서버 라이브러리로는 아래와 같은 것들이 있어요:

<div class="content-ad"></div>

- http: HTTP 요청을 보내거나 HTTP 서버를 구축하는 패키지입니다.
- shelf: 웹 서버를 구축하고 HTTP 요청을 처리하는 유연한 패키지입니다.
- aqueduct: 서버 애플리케이션을 구축하는 기능이 풍부한 웹 프레임워크입니다.
- angel: HTTP 서버와 API를 구축하기 위한 매우 사용자 정의할 수 있는 웹 프레임워크입니다.
- mongo_dart: MongoDB 데이터베이스를 다루기 위한 패키지입니다.

이러한 라이브러리와 패키지는 Dart 프로젝트에 pubspec.yaml 파일의 종속성으로 추가함으로써 사용할 수 있습니다. 예를 들어:

```yaml
dependencies:
  args: ^2.0.0
  shelf: ^1.0.0
```

선택하는 특정 라이브러리나 패키지는 명령줄 또는 서버 애플리케이션의 요구 사항에 따라 다를 것입니다. 이들은 일반적인 작업에 대한 미리 구축된 솔루션을 제공하여 여러분이 애플리케이션의 핵심 기능 구현에 집중할 수 있도록 시간과 노력을 절약할 수 있습니다.

<div class="content-ad"></div>

Dart 및 Flutter 생태계가 지속적으로 발전하고 있으며 시간이 지남에 따라 새로운 라이브러리와 패키지가 제공될 수 있습니다. 최신 및 최신 옵션을 확인하려면 항상 공식 Dart 패키지 저장소 (https://pub.dev) 를 확인하세요.

Dart에서 명령줄 앱을 빌드하는 것은 터미널이나 명령 프롬프트에서 직접 실행할 수 있는 유틸리티, 스크립트 또는 도구를 만드는 좋은 방법입니다. 시작하기 위한 간단한 단계별 가이드입니다:

- 환경 설정: 시스템에 Dart SDK가 설치되어 있는지 확인하세요. 공식 Dart 웹사이트 (https://dart.dev/get-dart) 에서 다운로드할 수 있습니다.
- 새 Dart 프로젝트 만들기: 프로젝트를 만들고자 하는 디렉토리로 터미널이나 명령 프롬프트를 열고 다음 명령을 실행하세요:

```bash
mkdir my_cli_app
cd my_cli_app
dart create .
```

<div class="content-ad"></div>

3. 코드 편집: 프로젝트의 bin 디렉토리를 열어보세요. 거기에는 Dart 파일(e.g. main.dart)이 있을 겁니다. 이 파일을 편집하여 명령줄 앱을 구현하세요. 예를 들어:

```js
import 'dart:io';

void main(List<String> arguments) {
  print('안녕, Dart 명령줄 앱!');
}
```

이 여정이 우리에게 더 유익한 경험이 되기를 바라요! 💯

이 마법 같은 여정에 함께 해줘서 너무 고맙습니다! 🫂

<div class="content-ad"></div>

다음 여정에서 뵙겠습니다👋👋👋

그때까지, 
Ruban ❤️