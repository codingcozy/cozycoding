---
title: "플러터 개발자라면 반드시 지켜야할 50가지 규칙"
description: ""
coverImage: "/assets/img/2024-09-10-50CodingLawsThatWouldMakeYouADecentFlutterDeveloper_0.png"
date: 2024-09-10 19:54
ogImage: 
  url: /assets/img/2024-09-10-50CodingLawsThatWouldMakeYouADecentFlutterDeveloper_0.png
tag: Tech
originalTitle: "50 Coding Laws That Would Make You A Decent Flutter Developer"
link: "https://medium.com/flutter-minds/50-coding-laws-that-would-make-you-a-decent-flutter-developer-7cea7e209c88"
isUpdated: true
updatedAt: 1726022602187
---


<img src="/assets/img/2024-09-10-50CodingLawsThatWouldMakeYouADecentFlutterDeveloper_0.png" />

# 소개

모바일 앱 개발의 빠르게 변화하는 세계에서, Flutter는 개발자가 단일 코드베이스로 모바일, 웹 및 데스크톱용 네이티브 컴파일된 애플리케이션을 만들 수 있는 강력하고 다재다능한 툴킷으로 빛을 발합니다. 그러나 위대한 힘이 있는만큼 큰 책임도 따릅니다. 깔끔하고 효율적이며 유지보수가 쉬운 코드를 작성하는 것은 장기적인 성공에 중요합니다. 경험이 많은 Flutter 개발자이든 초심자이든, 최고의 실천 방법을 준수하면 코드 품질과 앱 성능을 급격히 향상시킬 수 있습니다. 여기에는 더 나은 Flutter 개발자가 되도록 안내해줄 50가지 코딩 법칙이 있습니다.

## 1. DRY 원칙을 따르세요

<div class="content-ad"></div>

DRY (Don’t Repeat Yourself)은 코드 반복을 줄이기 위한 소프트웨어 개발의 기본 원칙입니다.

예시:

```js
// 대신에
double calculateArea(double width, double height) {
  return width * height;
}
double calculateVolume(double width, double height, double depth) {
  return width * height * depth;
// 사용
double calculateArea(double width, double height) {
  return width * height;
}
double calculateVolume(double width, double height, double depth) {
  return calculateArea(width, height) * depth;
}
```

## 2. 고정된 상수 집합에 대해 enum을 사용하세요.

<div class="content-ad"></div>

열거형(Enum)은 고정된 상수 집합을 나타내는 훌륭한 방법으로, 코드를 더 읽기 쉽고 유지보수하기 쉽게 만들어줍니다.

예시:

```js
enum Status {
  active,
  inactive,
  pending,
}

// 사용 방법
Status userStatus = Status.active;
```

## 3. 암호적인 약어를 피하세요

<div class="content-ad"></div>

## 4. 실시간 데이터를 위해 StreamBuilder 사용하기

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해주세요.

<div class="content-ad"></div>

비동기 데이터를 FutureBuilder를 사용하여 표시하세요.

예시:

```js
class DataWidget extends StatelessWidget {
  Future<String> fetchData() async {
    await Future.delayed(Duration(seconds: 2));
    return 'Data';
  }

@override
  Widget build(BuildContext context) {
    return FutureBuilder<String>(
      future: fetchData(),
      builder: (context, snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return CircularProgressIndicator();
        } else if (snapshot.hasError) {
          return Text('Error: ${snapshot.error}');
        } else {
          return Text('Data: ${snapshot.data}');
        }
      },
    );
  }
}
```

## 6. 필요에 따라 프로젝트 요구사항에 맞는 적절한 상태 관리를 사용하세요

<div class="content-ad"></div>

프로젝트에 가장 적합한 상태 관리 솔루션을 선택해보세요.

예시:

```js
// 간단한 상태 관리를 위해 Provider를 사용합니다
class Counter with ChangeNotifier {
  int _count = 0;
  int get count => _count;
  void increment() {
    _count++;
    notifyListeners();
  }
// 복잡한 상태 관리를 위해 Bloc을 사용합니다
class CounterBloc extends Bloc<CounterEvent, int> {
  CounterBloc() : super(0);
  @override
  Stream<int> mapEventToState(CounterEvent event) async* {
    if (event is Increment) {
      yield state + 1;
    }
  }
}
```

## 7. 효율적인 업데이트를 위해 Key 위젯을 사용하세요

<div class="content-ad"></div>

위젯이 업데이트 되어야 하는지를 식별하는 데 키가 도움이 됩니다.

예시:

```js
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: items.length,
      itemBuilder: (context, index) {
        return ListTile(
          key: ValueKey(items[index].id),
          title: Text(items[index].name),
        );
      },
    );
  }
}
```

## 8. 중첩 Try-Catch 블록을 피하세요.

<div class="content-ad"></div>

에러 처리를 간단하게 유지하는 것이 가독성을 높일 수 있어요.

예시:

```js
try {
  // 예외가 발생할 수 있는 코드
} catch (e) {
  // 예외 처리
}
```

## 9. 레이아웃과 로직 분리

<div class="content-ad"></div>

친구야, 코드를 더 관리하기 쉽게 유지하기 위해 UI 레이아웃과 비즈니스 로직을 분리해보세요.

예시:

```js
// 레이아웃
class LoginScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: LoginButton(),
      ),
    );
  }
}

// 로직
class LoginButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {
        // 로그인 처리
      },
      child: Text('로그인'),
    );
  }
}
```

## 10. 상수에 대한 구성을 사용하세요.

<div class="content-ad"></div>

상수를 관리하기 쉽게 하기 위해 구성 파일이나 상수 클래스에 상수를 저장하세요.

예시:

```js
const String API_URL = 'https://api.example.com';
```

## 11. 지나치게 복잡한 위젯을 피하세요

<div class="content-ad"></div>

위젯을 간단하고 집중해서 유지하세요.

예시:

```js
// 대신에
class ComplexWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Title'),
        Expanded(
          child: ListView(
            children: [
              ListTile(title: Text('Item 1')),
              ListTile(title: Text('Item 2')),
            ],
          ),
        ),
        ElevatedButton(onPressed: () {}, child: Text('Button')),
      ],
    );
  }
}


// 사용
class SimpleWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Title'),
        ListView(
          children: [
            ListTile(title: Text('Item 1')),
            ListTile(title: Text('Item 2')),
          ],
        ),
        ElevatedButton(onPressed: () {}, child: Text('Button')),
      ],
    );
  }
}
```

## 12. 상태가 변경되지 않는 경우 Final 키워드 사용

<div class="content-ad"></div>

초기화 후 변경되지 말아야 하는 변수에는 `final`을 사용하세요.

예시:

```js
final String userName = 'John Doe';
```

## 13. UI 구성 요소에는 위젯을 사용하세요

<div class="content-ad"></div>

재사용 가능한 위젯으로 UI 구성 요소를 캡슐화하여 코드 재사용을 증진시키세요.

예시:

```js
class MyButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(onPressed: () {}, child: Text('Press Me'));
  }
}
```

## 14. 생성자 초기화 목록 사용하기

<div class="content-ad"></div>

클래스의 생성자에 대해 초기화 목록을 사용하면 성능과 가독성이 더 좋아집니다.

예시:

```js
class User {
  final String name;
  final int age;
  
  // 생성자 몸통 내에서 필드를 초기화하는 대신
  User(String name, int age) : name = name, age = age;
}
```

## 15. 함수 이름은 동사여야 합니다

<div class="content-ad"></div>

함수는 그들이 하는 일을 명확히 전달하기 위해 동작을 설명해야 합니다.

예시:

```js
// 대신에
void nameCheck() {}

// 사용하세요
void checkName() {}
```

## 16. 데이터 유효성 검사에 assert 사용을 피하세요

<div class="content-ad"></div>

프로덕션 환경에서는 데이터 유효성 검사에 단언문(assertions)을 사용하지 말고 디버깅에 사용하세요.

예시:

```js
// 다음과 같이 사용하지 마세요
assert(user != null, 'User cannot be null');

// 대신에
if (user == null) {
  throw ArgumentError('User cannot be null');
}
```

## 17. Try-Finally를 사용하여 리소스 관리하기

<div class="content-ad"></div>

자원 관리를 위해 try-finally 블록을 사용하세요.

예시:

```js
// await 및 try-finally를 사용한 자원 관리
Future<void> readFile() async {
  final file = File('example.txt');
  try {
    final contents = await file.readAsString();
    print(contents);
  } finally {
    // 파일이 제대로 닫히거나 자원이 정리되도록 보장합니다
  }
}
```

## 18. 타입 주석 사용하기

<div class="content-ad"></div>

친절한 톤으로 번역해드립니다.

## 19. 의존성 주입 사용하기

<div class="content-ad"></div>

의존성 주입을 사용하여 컴포넌트 간의 느슨한 결합을 촉진하세요.

예시:

```js
class AuthService {
  void login(String username, String password) {}
}

class LoginScreen {
  final AuthService _authService;
  LoginScreen(this._authService);
}
```

## 20. 관련된 코드 라인을 가깝게 유지하기

<div class="content-ad"></div>

비슷한 코드 라인을 함께 그룹화 해서 이해와 유지보수를 더 쉽게하세요.

예시:

```js
class Order {
  final List<Item> items;

  Order(this.items);
  
  double calculateTotal() {
    return items.fold(0, (total, item) => total + item.price);
  }
}
```

## 21. 마법 숫자를 피하세요

<div class="content-ad"></div>

하드 코딩된 값을 사용하면 혼란스러울 수 있습니다. 더 확실한 이해를 위해 상수에 이름을 지정하세요.

예시:

```js
// 기존 방식
final padding = 16.0;

// 대신에
const double kPadding = 16.0;
final padding = kPadding;
```

## 22. null 안전하게 처리하기

<div class="content-ad"></div>

null 값이 안전하게 처리할 수 있도록 null-aware 연산자를 사용하세요.

예시:

```js
// 아래와 같이
if (user != null) {
  print(user.name);

// 다음과 같이 사용하세요
print(user?.name);
```

## 23. 특정 오류에 대해 커스텀 예외를 사용하세요

<div class="content-ad"></div>

더 나은 오류 처리를 위해 사용자 지정 예외를 만들어 보세요.

예시:

```js
class CustomException implements Exception {
  final String message;
  CustomException(this.message);
  @override
  String toString() {
    return 'CustomException: $message';
  }
}
```

## 24. 변수에 설명적인 이름을 사용하세요.

<div class="content-ad"></div>

변수명은 해당 목적을 설명해야 해요.

예시:

```js
// 대신에
var n = 100;

// 다음과 같이 사용하세요
var maxUsers = 100;
```

## 25. 불변 클래스에 대해 const 생성자를 사용하세요

<div class="content-ad"></div>

변하지 않는 클래스에 대해 const 생성자를 사용하세요.

예시:

```js
class Point {
  final double x;
  final double y;

  const Point(this.x, this.y);
}
```

## 26. 클래스는 작아야 합니다

<div class="content-ad"></div>

클래스는 하나의 책임만 가지도록 해야 해요.

예시:

```js
// 이렇게 하지 말고
class User {
  String name;
  String address;
  void login() {}
  void logout() {}

// 이렇게 사용하세요
class User {
  String name;
}
class AuthService {
  void login() {}
  void logout() {}
}
```

## 27. 상태 변화 UI에 Stateful 위젯 사용하기

<div class="content-ad"></div>

UI가 동적으로 변경되어야 할 때 StatefulWidget을 사용하세요.

예시:

```js
class Counter extends StatefulWidget {
  @override
  _CounterState createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _count = 0;
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('$_count'),
        ElevatedButton(
          onPressed: () {
            setState(() {
              _count++;
            });
          },
          child: Text('Increment'),
        ),
      ],
    );
  }
}
```

## 28. 위젯에 const 키워드를 현명하게 사용하세요

<div class="content-ad"></div>

위젯에 const 키워드를 사용하면 Flutter는 다시 빌드를 줄이고 위젯 트리의 효율성을 향상시켜 성능을 최적화할 수 있습니다.

예시:

```js
// 이렇게 하지 말고
Widget build(BuildContext context) {
  return Text('Hello, World!');
}

// 이렇게 사용하세요
Widget build(BuildContext context) {
  return const Text('Hello, World!');
}
```

## 29. 너무 긴 메서드 피하기

<div class="content-ad"></div>

긴 메서드를 더 작고 관리하기 쉬운 조각으로 나눠보세요. 각 메서드는 하나의 작업을 수행해야 합니다.

예시:

```js
// Instead of
void processOrder(Order order) {
  validateOrder(order);
  calculateTotal(order);
  updateInventory(order);
  sendConfirmationEmail(order);

// Use
void processOrder(Order order) {
  validateOrder(order);
  calculateTotal(order);
  updateInventory(order);
  sendConfirmationEmail(order);
}
void validateOrder(Order order) {
  // Validation logic
}
void calculateTotal(Order order) {
  // Calculation logic
}
void updateInventory(Order order) {
  // Inventory update logic
}
void sendConfirmationEmail(Order order) {
  // Email sending logic
}
```

## 30. 복잡한 위젯에 대해 빌더 패턴을 사용하세요

<div class="content-ad"></div>

복잡한 객체의 생성을 해당 객체의 표현과 분리하기 위해 빌더 패턴을 사용하세요.

예시:

```js
class ComplexWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        buildHeader(),
        buildContent(),
        buildFooter(),
      ],
    );
  }

  Widget buildHeader() {
    return Text('Header');
  }

  Widget buildContent() {
    return Text('Content');
  }

  Widget buildFooter() {
    return Text('Footer');
  }
}
```

## 31. 매개변수와 반환 유형 지정

<div class="content-ad"></div>

항상 함수 매개변수와 반환 유형을 명시해주세요.

예시:

```js
// Instead of
void add(a, b) {
  return a + b;
  
// Use
int add(int a, int b) {
  return a + b;
}
```

## 32. 잡음/중복 단어 피하기

<div class="content-ad"></div>

이름을 간결하고 명료하게 유지하세요.

예시:

```js
// 아래와 같이
void getUserName() {}

// 이렇게 쓰세요
void fetchUser() {}
```

## 33. 함수 크기를 작게 유지하세요

<div class="content-ad"></div>

함수는 단 하나의 일만 해야 합니다. 이렇게 하면 테스트와 유지보수가 쉬워집니다.

예시:

```js
// 대신에
void manageOrder(Order order) {
  if (order.isValid()) {
    order.calculateTotal();
    order.updateInventory();
    order.sendConfirmationEmail();
  }

// 사용
void manageOrder(Order order) {
  if (order.isValid()) {
    calculateTotal(order);
    updateInventory(order);
    sendConfirmationEmail(order);
  }
}
void calculateTotal(Order order) {
  // 계산 로직
}
void updateInventory(Order order) {
  // 재고 업데이트 로직
}
void sendConfirmationEmail(Order order) {
  // 이메일 발송 로직
}
```

## 34. 복잡한 상태 관리 피하기

<div class="content-ad"></div>

당신의 요구를 충족하는 가장 간단한 상태 관리 솔루션을 선택해보세요.

예시:

```js
// 간단한 상태 관리를 위해 Provider 사용
class Counter with ChangeNotifier {
  int _count = 0;

  int get count => _count;
  void increment() {
    _count++;
    notifyListeners();
  }
}
```

## 35. UI와 비즈니스 로직 분리

<div class="content-ad"></div>

유지 관리성을 위해 UI와 비즈니스 로직을 분리하세요.

예시:

```js
// UI
class CounterScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<CounterBloc, int>(
      builder: (context, count) {
        return Text('$count');
      },
    );
  }
}

// 비즈니스 로직
class CounterBloc extends Bloc<CounterEvent, int> {
  CounterBloc() : super(0);
}
```

## 36. 비동기 코드에는 async와 await 사용하기

<div class="content-ad"></div>

비동기 코드를 async와 await을 활용하여 간편하게 작성하세요.

예시:

```js
Future<void> fetchData() async {
  try {
    final response = await http.get('https://api.example.com/data');
    // 응답 처리
  } catch (e) {
    print('에러: $e');
  }
}
```

## 37. 클래스 이름은 명사여야 합니다.

<div class="content-ad"></div>

클래스 이름은 엔티티를 나타내야 합니다.

예시:

```js
// 아래처럼
class DoSomething {}

// 아래처럼 사용해주세요
class User {}
```

## 38. 경로 하드 코딩 피하기

<div class="content-ad"></div>

환경 변수나 설정 파일을 사용해보세요.

예시:

```js
// 이렇게
final apiUrl = 'https://api.example.com';

// 이렇게
final apiUrl = dotenv.env['API_URL'];
```

## 39. 단일 책임 원칙을 사용하세요

<div class="content-ad"></div>

각 클래스는 하나의 책임만을 가져야 합니다.

예시:

```js
// 아래와 같이
class User {
  String name;
  String address;
  void login() {}
  void logout() {}
```

```js
// 아래처럼 사용하세요
class User {
  String name;
}
class AuthService {
  void login() {}
  void logout() {}
}
```

<div class="content-ad"></div>

## 40. 하드 코딩된 문자열을 피하십시오

상수나 지역화를 사용하여 문자열을 처리하세요.

예시:

```js
// 대신에
final title = 'Welcome';

// 다음과 같이 사용하세요
const String kTitle = 'Welcome';
final title = kTitle;
```

<div class="content-ad"></div>

## 41. 긴 목록에는 ListView를 사용하세요

ListView를 사용하여 효율적으로 긴 목록을 표시하세요.

예시:

```js
class LongList extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: 100,
      itemBuilder: (context, index) {
        return ListTile(
          title: Text('Item $index'),
        );
      },
    );
  }
}
```

<div class="content-ad"></div>

## 42. 임시 변수 사용을 피하세요

불필요한 임시 변수를 제거하세요.

예시:

```js
// 대신에
final tempResult = calculate();
final finalResult = tempResult * 2;

// 사용하세요
final finalResult = calculate() * 2;
```

<div class="content-ad"></div>

## 43. 메인 스레드 블로킹 피하기

UI가 고정되는 것을 방지하기 위해 무거운 작업을 별도의 분리된 공간으로 이동시킵니다. 이 기사를 읽어보세요.

예시:

```js
Future<void> fetchData() async {
  final data = await compute(fetchDataInBackground, url);
}

Future<String> fetchDataInBackground(String url) {
  // 무거운 계산 수행
  return data;
}
```

<div class="content-ad"></div>

## 44. 타입 확인을 위해 is와 is not을 사용하세요

is와 is not 연산자를 사용하여 타입을 확인하세요.

예시:

```js
// 사용하지 마세요
if (widget.runtimeType == Text) {
  // 작업 수행

// 사용해보세요
if (widget is Text) {
  // 작업 수행
}
```

<div class="content-ad"></div>

## 45. 깊은 중첩 피하기

깊게 중첩된 코드는 읽고 유지하기 어렵습니다.

예시:

```js
// 대신에
if (x) {
  if (y) {
    doSomething();
  }

// 사용
if (x && y) {
  doSomething();
}
```

<div class="content-ad"></div>

## 46. 기본값으로 ?? 사용하기

기본값으로 null-aware operator인 ??를 사용하세요.

예시:

```js
String name = userName ?? 'Guest';
```

<div class="content-ad"></div>

## 47. 예외 처리와 함께 컨텍스트 제공하기

디버깅을 도와주기 위해 오류 메시지를 정보를 포함하여 제공하세요.

예시:

```js
try {
  // 예외를 발생시킬 수 있는 코드
} catch (e) {
  print('에러: $e');
}
```

<div class="content-ad"></div>

## 48. setState 메소드를 현명하게 사용하세요

성능을 향상시키기 위해 setState의 범위를 최소화하세요.

예시:

```js
// 대신에
setState(() {
  _counter++;
  _someOtherState = true;
});

// 사용하세요
setState(() {
  _counter++;
});
```

<div class="content-ad"></div>

## 49. 존중받는 코드 작성 표준 준수하기

커뮤니티에서 받아들여지는 코딩 표준과 스타일 가이드를 준수하세요. 

예시:

```js
// Dart의 효과적인 코드 스타일을 따르세요
void main() {
  runApp(MyApp());
}
```

<div class="content-ad"></div>

## 50. 이 49가지 법칙을 숙달한 후 현명하게 어기세요

이러한 법칙들은 숙달된 플러터 개발자가 되기 위한 길잡이입니다. 깨끗하고 유지보수 가능하며 효율적인 코드를 작성하기 위해 이 원칙을 준수하는 것이 중요하지만, 진정한 숙련의 증표는 언제 벗어날지를 알고 있는 것입니다. 경험이 쌓이고 이해가 깊어지면 규칙을 어겼을 때 더 나은 해결책으로 이끌 수 있는 직관력을 발전시킬 수 있을 것입니다.

# 추가 읽을거리

이 코딩 법칙들이 마음에 들고 플러터 개발에 더 심취하고 싶다면, 제 다른 기사들을 확인해보세요:

<div class="content-ad"></div>

- SOLID 원칙을 Flutter에 적용하기: 실용적 안내서
- CLEAN Architecture로 Flutter 앱 개발하기
- 필수 Linter 규칙으로 Flutter 개발하기
- Dart Metrics를 활용한 Flutter/Dart에서 높은 품질의 코드 해제하기
- Flutter에서 Keys의 힘을 활용하기: 최상의 실천 사례와 사용 사례
- Flutter에서 StatefulWidget 라이프사이클: 깊이 탐구
- Flutter에서 무거운 작업 오프로드하기: 아이솔릿의 힘
- Flutter Mixins 설명: 예제와 사용 사례로 구체적으로 설명
- 보일러플레이트 버리기: Flutter에서 json_serializable을 사용한 JSON 파싱 방법

이 글을 읽어 주셔서 감사합니다.

무언가 빠뜨렸거나 실수한 부분이 있다면 댓글로 알려주시면 감사하겠습니다. 항상 배우고 개선하려는 마음으로 노력하고 있습니다.

만약 이 글이 도움이 되었다면 👏 클릭해 주세요.