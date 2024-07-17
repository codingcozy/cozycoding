---
title: "모든 Flutter 엔지니어가 알아야 할 5가지 디자인 패턴"
description: ""
coverImage: "/assets/img/2024-07-07-The5DesignPatternsForEveryFlutterEngineer_0.png"
date: 2024-07-07 19:39
ogImage: 
  url: /assets/img/2024-07-07-The5DesignPatternsForEveryFlutterEngineer_0.png
tag: Tech
originalTitle: "The 5 Design Patterns For Every Flutter Engineer"
link: "https://medium.com/@efenstakes101/the-5-design-patterns-for-every-flutter-engineer-a791bca05db7"
---



![The 5 Design Patterns for Every Flutter Engineer](/assets/img/2024-07-07-The5DesignPatternsForEveryFlutterEngineer_0.png)

Engineering Skills Boost!

Flutter, with its convenient hot reload and a plethora of widgets, opens up possibilities for creating stunning and interactive UIs. However, as our applications expand, managing data flow and maintaining well-organized code becomes more challenging.

Always remember to steer clear of accumulating technical debt, which typically occurs as your app's codebase grows into thousands of lines. This is where design patterns come into play—they offer proven solutions to common software development hurdles that engineers have encountered since the inception of system engineering and design.


<div class="content-ad"></div>

디자인 패턴을 코드의 구조적인 청사진으로 생각해보세요. 이들은 객체 생성, 통신 및 관계에 대한 체계적인 접근 방식을 제공하여 다음을 이끕니다:

- 재사용성: 방식을 다시 발명하지 마세요! 디자인 패턴은 여러 앱 부분에 적응할 수 있는 미리 정의된 솔루션을 제공합니다. 쉽죠 😊.
- 유지보수 용이성: 깨끗하고 잘 구조화된 코드는 이해, 수정 및 디버깅이 쉬워져서 오랜 기간을 절약하고 당신을 좌절에서 구해줄 거예요 🚀.
- 테스트 용이성: 디자인 패턴은 주로 관심사의 분리를 촉진하며, 앱의 개별 구성 요소를 분리하고 테스트하는 것이 쉬워집니다 ✅.

플러터 엔지니어를 위한 가장 흔한 디자인 패턴 중 일부와 그들의 힘을 설명하는 실제 예제를 살펴보겠습니다:

## 싱글톤 패턴

<div class="content-ad"></div>

앱 전체 데이터(예: 사용자 환경설정 또는 테마 관리자)를 저장할 중앙 위치가 필요했던 적이 있었나요? 싱글톤 패턴을 사용하면 클래스의 하나의 인스턴스만 애플리케이션 전체에서 존재한다는 점이 보장됩니다. 앱의 테마를 제어하는 단일 "SettingsManager" 클래스를 상상해보세요. UI의 어떤 부분이라도 이 관리자에 접근하여 현재 테마 설정을 검색하고 위젯을 적절히 스타일링할 수 있습니다. GraphQL을 사용하는 앱의 경우 단일 GraphQL 클라이언트가 존재하여 쿼리와 뮤테이션을 실행할 수 있다고 상상해보세요. 더 일반적인 예로는 웹소켓 연결이 있습니다. 귀하의 앱은 거의 항상 하나만 필요할 것입니다.

여기서 이 패턴이 활용될 수 있습니다.

```js
class SettingsManager {
  static final SettingsManager _instance = SettingsManager._internal();

  factory SettingsManager() => _instance;

  SettingsManager._internal();

  ThemeData _themeData = ThemeData.light();

  void switchTheme() {
    _themeData = _themeData.brightness == Brightness.light ? ThemeData.dark() : ThemeData.light();
    // 테마 변경을 알림(효율성을 위해 구현 내용 생략)
  }

  ThemeData getTheme() => _themeData;
}
```

## 팩토리 메서드 패턴

<div class="content-ad"></div>

데이터를 기반으로 UI 요소를 동적으로 생성해야 할 때, **Factory Method** 패턴은 정확한 클래스를 미리 지정하지 않고 객체를 생성하기 위한 인터페이스를 제공합니다. 이는 더 큰 유연성을 제공합니다. 예를 들어, Factory 클래스는 받은 데이터에 따라 다양한 유형의 버튼 (기본, 보조)을 생성할 수 있습니다.

스포츠 팬들을 위해 예를 들어보면, 사용자가 지정한 베팅을 보여주는 대시보드가 있는데, 게임 결과에 따라 사용자가 이긴 것인지 틀린 것인지 볼 수 있는 위젯이 있습니다.

```js
abstract class ButtonFactory {
  Widget createButton(String text);
}

class PrimaryButtonFactory implements ButtonFactory {
  @override
  Widget createButton(String text) => ElevatedButton(onPressed: null, child: Text(text));
}

class SecondaryButtonFactory implements ButtonFactory {
  @override
  Widget createButton(String text) => TextButton(onPressed: null, child: Text(text));
}

// 사용 예시
ButtonFactory buttonFactory = (isPrimary) => isPrimary ? PrimaryButtonFactory() : SecondaryButtonFactory();
Widget myButton = buttonFactory(true).createButton("Click Me"); // 기본 버튼 생성
```

## Provider Pattern

<div class="content-ad"></div>

프로바이더 패턴은 효율적인 데이터 관리를 위한 가벼운 상태 관리 솔루션입니다. 이는 InheritedWidget을 활용하여 데이터 변경을 위젯 트리 아래로 전파합니다.

예를 들어, 지출 앱을 위한 전역 사용자 지출 목록을 생각해보세요. 프로바이더 패턴을 사용하면 카운터 상태를 보유하고 변경 사항을 "청취"하는 단일 ExpenseProvider를 생성할 수 있습니다. 지출 내용을 표시해야 하는 트리 아래의 어떤 위젯이든 Provider를 통해 액세스할 수 있습니다.

```js
class MyExpensesProvider with ChangeNotifier {
  DateTime startDate = DateTime.now().subtract(const Duration(days: 30));
  DateTime endDate = DateTime.now();

  bool isLoading = false;
  String? error;

  List<Expense> expenses = [];


  MyExpensesProvider() {

    getExpenses();
  }

  getExpenses() async {
    
  }

}
```

## 조합 패턴

<div class="content-ad"></div>

Flutter의 주요 강점은 구성 가능한 위젯 시스템에 있습니다. 이는 작은 재사용 가능한 위젯을 조립하여 복합적인 UI를 구축하는 Composition Pattern과 완벽하게 일치합니다.

이 패턴은 재사용성, 유연성 및 테스트 용이성을 촉진합니다.

```js
class RoundedButton extends StatelessWidget {
  final String text;
  final VoidCallback onPressed;

  const RoundedButton({required this.text, required this.onPressed});

  @override
  Widget build(BuildContext context) {
    return TextButton(
      onPressed: onPressed,
      child: Container(
        padding: EdgeInsets.all(16.0),
        decoration: BoxDecoration(
          color: Colors.blue,
          borderRadius: BorderRadius.circular(10.0),
        ),
        child: Text(text, style: TextStyle(color: Colors.white)),
      ),
    );
  }
}

// 사용 예
Widget myScreen = Scaffold(
  body: Center(
    child: RoundedButton(text: 'Click Me', onPressed: () => print('Button Pressed')),
  ),
);
```

## Bloc 패턴

<div class="content-ad"></div>

Bloc 패턴은 상태 관리를 위한 구조화된 방법을 제공하여 UI(표현 계층)와 비즈니스 로직(데이터 가져오기, 계산 등)을 분리합니다. 이는 코드를 더 깔끔하고 유지보수가 쉽며 테스트하기 용이하게 만듭니다.

Bloc 패턴에는 이벤트를 처리하고 상태를 업데이트하며 발신하는 블록이 포함되어 있습니다. 앱 동작을 나타내는 이벤트, 앱에 포함된 데이터인 상태, 블록 수명주기를 관리하고 자식 위젯에 제공하는 BlocProvider도 있습니다. TypeScript와 React에 익숙한 사람들에게 React Redux와 많은 면에서 매우 유사함을 느낄 수 있습니다.

```js
@immutable
abstract class CounterEvent {}

// 이벤트
class IncrementEvent extends CounterEvent {}

class DecrementEvent extends CounterEvent {}

// 상태
class CounterState {
  final int counter;

  CounterState(this.counter);
}

// Bloc
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(CounterState(0));

  @override
  Stream<CounterState> mapEventToState(CounterEvent event) async* {
    if (event is IncrementEvent) {
      yield CounterState(state.counter + 1);
    } else if (event is DecrementEvent) {
      yield CounterState(state.counter - 1);
    }
  }
}

// 사용법 (UI에서)
Widget build(BuildContext context) {
  return BlocProvider(
    create: (context) => CounterBloc(),
    child: Scaffold(
      appBar: AppBar(title: Text('Counter')),
      body: Column(
        mainAxisAlignment: MainCenter,
        children: [
          BlocBuilder<CounterBloc, CounterState>(
            builder: (context, state) => Text('Count: ${state.counter}'),
          ),
          Row(
            mainAxisAlignment: MainSpaceEvenly,
            children: [
              ElevatedButton(
                onPressed: () => BlocProvider.of<CounterBloc>(context).add(IncrementEvent()),
                child: Icon(Icons.add),
              ),
              ElevatedButton(
                onPressed: () => BlocProvider.of<CounterBloc>(context).add(DecrementEvent()),
                child: Icon(Icons.remove),
              ),
            ],
          ),
        ],
      ),
    ),
  );
}
```

## 결론

<div class="content-ad"></div>

휴대폰 앱을 개발하는 대부분의 엔지니어들은 디자인 패턴에 익숙하지 않을 수 있습니다. 이 가이드의 목표는 이들을 위한 시작점을 제공하고 주니어 엔지니어들에게 가장 일반적인 패턴들을 상기시키는 것입니다.
저는 Flutter를 위해 이를 만들었는데, Swift와 같이 명확하게 이러한 패턴을 제시하지 않는 flutter 문서 때문입니다.

화이팅 🚀 항상 코드의 영광을 위하여.