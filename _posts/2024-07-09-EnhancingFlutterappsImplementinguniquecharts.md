---
title: "Flutter 앱 향상시키기 독특한 차트 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-09-EnhancingFlutterappsImplementinguniquecharts_0.png"
date: 2024-07-09 22:43
ogImage: 
  url: /assets/img/2024-07-09-EnhancingFlutterappsImplementinguniquecharts_0.png
tag: Tech
originalTitle: "Enhancing Flutter apps: Implementing unique charts"
link: "https://medium.com/mobile-app-circular/enhancing-flutter-apps-implementing-unique-charts-540fc1ab2749"
---


## Flutter를 사용하여 경로를 이용해 하트 모양의 액체 차트 만들기

![이미지](/assets/img/2024-07-09-EnhancingFlutterappsImplementinguniquecharts_0.png)

앱에 통합할 수 있는 다양한 차트 형태와 외양이 있습니다. 그러나 이러한 차트들은 종종 이해하기 어렵고 사용자의 시선을 사로잡지 못할 수 있습니다. 우리가 앱에 독특한 외관을 주기 위해 차별화된 차트 디자인을 찾을 때, 디자이너들이 특별한 아이디어인 액체 하트 차트를 고안했습니다. 이 차트를 개발하는 과정에서 두 가지 어려움을 겪었습니다:

- Dart로 하트 모양 경로 생성하기
- 모든 기기 크기에 확장 가능한 모양 만들기

<div class="content-ad"></div>

플러터에서이 디자인을 구현하는 방법에 대한 빠른 안내서를 제공합니다.

# 다양한 사용 사례

<img src="https://miro.medium.com/v2/resize:fit:1400/1*PK6IEdbTEyURWhz3sxl_pA.gif" />

하트 모양의 액체 차트는 다양한 시나리오에 적용될 수 있습니다. 예를 들어 게임에서 얻은 포인트, 사용자가 하루에 섭취한 칼로리 또는 할 일 목록에서 완료한 작업을 나타낼 수 있습니다. 저희 경우에는 사용자의 남은 월급을 표시하거나 예산을 초과했는지 여부를 확인하는 데 사용합니다. 이 기사에서는 다양한 필요에 맞게 적응할 수있는 차트 구현의 일반 버전을 제공합니다. 예를 들어 사용자가 재정 적자에 있을 때 색상을 변경하여 사용자가 언제 재정적 어려움에 처했는지 나타내도록 사용자 정의했습니다.

<div class="content-ad"></div>

# 액체 진행 표시기 패키지

애니메이션 효과가 있는 액체 차트를 만들기 위해, 우리는 pub.dev에서 liquid_progress_indicator_v2 패키지를 사용합니다.

먼저 해당 패키지를 플러터 프로젝트에 추가하세요. 이 패키지를 사용하면 액체 진행 표시 선을 만들 수 있습니다. "액체"란 선이 파도 모양을 가지며 형태를 채우면서 애니메이션 되는 것을 의미합니다. 예를 들어, LiquidCircularProgressIndicator를 사용하여 액체로 채워진 원을 얻을 수 있습니다.

```js
LiquidCircularProgressIndicator(
  value: 0.25,
  valueColor: AlwaysStoppedAnimation(Colors.pink),
  backgroundColor: Colors.white,
  borderColor: Colors.red,
  borderWidth: 5.0,
  direction: Axis.horizontal,
  center: Text("로딩 중..."),
);
```

<div class="content-ad"></div>


![이미지](https://miro.medium.com/v2/resize:fit:1400/1*U_lBDcZGhTbYcRaLCCxDug.gif)

LiquidCircularProgressIndicator를 다양한 색상과 최대값으로 사용자 정의할 수 있습니다. 또한 액체의 부유 방향을 정의하고 차트 중앙에 위젯을 추가할 수 있습니다. 이 패키지는 차트를 사용자 정의하는 데 다른 모양을 제공하지만 하트 모양의 사용 사례와 일치하지 않는 것이 없습니다. 다행히 LiquidCustomProgressIndicator도 있으며 차트에 사용자 정의 모양을 만들기 위해 사용자 지정 경로를 설정할 수 있습니다. 하트 모양의 액체 차트를 만들기 위해 이제 하트 경로를 만들 것입니다.

# 하트 모양의 경로 생성

하트 모양의 경로를 생성하려면 Path를 반환하는 함수를 정의합니다. 하트 모양이 모든 기기 크기에서 작동하는 것이 중요하기 때문에 기기의 너비를 매개변수로 전달하여 값을 적절히 조정합니다. 우리는 먼저 너비를 0.8로 곱하여 하트의 높이를 계산합니다. 하트를 그리기 위해 두 가지 함수를 사용합니다:


<div class="content-ad"></div>

- moveTo를 사용하여 시작 지점을 설정합니다.
- cubicTo를 사용하여 아치를 그립니다. 이 함수는 주어진 지점으로 곡선을 만들기 위해 3개의 지점을 매개변수로 사용합니다: 두 개의 제어 지점과 아치의 끝점.

```js
Path _buildHeartPath(double width) { 
   final scaleValue = 0.8; 
   final height = width * scaleValue; 

   return Path() 
     // 첫 번째 아치를 그리기 위한 시작 지점 
     ..moveTo(0.5 * width, height *  0.27) 
     // 제어 지점의 x, y 좌표, 아치의 끝점의 x, y 좌표 
     ..cubicTo(0.4 * width, 0, -0.27 * width, height * 0.2, 0.5 * width, height * 0.9) 
     // 두 번째 아치를 그리기 위한 시작 지점 
     ..moveTo(0.5 * width, height * 0.27) 
     // 제어 지점의 x, y 좌표, 아치의 끝점의 x, y 좌표 
     ..cubicTo(0.6 * width, 0, 1.27 * width, height * 0.2, 0.5 * width, height * 0.9); 
}
```

하트 모양은 두 개의 아치로 구성되어 완전한 모양을 형성할 수 있습니다. 경로를 만들기 위해 다음 단계를 구현합니다:

## 왼쪽 아치 그리기

<div class="content-ad"></div>

- moveTo 함수를 사용하여 하트의 상단 들여쓰기를 시작점으로 설정합니다.
- cubicTo를 사용하여 시작점부터 끝점까지 아치를 그립니다. 매개변수로는 아치의 곡선을 조절할 한 지점(왼쪽)과 아치를 완성할 끝점이 사용됩니다.

## 오른쪽 아치 그리기

- 다시 한 번 moveTo 함수를 사용하여 하트의 상단 들여쓰기를 시작점으로 설정합니다.
- 다시 한 번 cubicTo를 사용하여 아치를 그립니다. 첫 번째 왼쪽 아치와 동일한 끝점을 사용하지만 아치를 반대 방향으로 그리기 위해 제어 지점(오른쪽)을 변경합니다.

![이미지](/assets/img/2024-07-09-EnhancingFlutterappsImplementinguniquecharts_1.png)

<div class="content-ad"></div>

# 차트의 모양과 스타일

이제 flutter의 StatefulWidget을 생성하여 애니메이션 효과가 있는 하트 모양의 액체 차트를 구현할 것입니다. 위젯은 애니메이션을 추가하고 싶기 때문에 stateful 해야 합니다. LiquidHeartChartState에서는 애니메이션을 처리하고 액체가 최대 값까지 상승하도록 AnimationController를 생성할 것입니다. 이 애니메이션을 활성화하기 위해 State 클래스가 SingleTickerProviderStateMixin을 상속받아야 합니다. initState 메소드에서는 AnimationController를 vsync 매개변수를 this로 설정하여 SingleTickerProviderStateMixin을 활용하도록 초기화합니다. duration 매개변수를 사용하여 액체가 주어진 값에 채워질 때까지 얼마나 오래 걸리는지 정의할 수 있습니다. 게다가 AnimationController에 리스너를 추가하여 AnimationController 값이 변경될 때마다 State를 업데이트할 수 있습니다. 이 구현으로 인해 애니메이션 컨트롤러의 값이 ticker에 의해 증가하므로 하트가 액체로 4초 안에 채워지게 됩니다.

```js
class _LiquidHeartChartState extends State<LiquidHeartChart> with SingleTickerProviderStateMixin { 
  late AnimationController _animationController; 

  @override 
  void initState() { 
    super.initState(); 
    _animationController = AnimationController( 
      vsync: this, 
      duration: const Duration(seconds: 4), 
    ); 

    _animationController 
      ..addListener(() => setState(() {}))  
      ..forward(); 
  } 

  @override 
  void dispose() { 
    _animationController.dispose(); 
    super.dispose(); 
  } 

  @override 
  Widget build(BuildContext context) { 
    return Center( 
      child: LiquidCustomProgressIndicator( 
      value: _animationController.value, 
      direction: Axis.vertical, 
      backgroundColor: const Color.fromARGB(255, 139, 205, 141).withOpacity(0.25), 
      valueColor: const AlwaysStoppedAnimation(Color.fromRGBO(9, 107, 73, 1)), 
      shapePath: _buildHeartPath(MediaQuery.of(context).size.width), 
      center: Text( 
        _animationController.value.toString() 
      ),  
      ),
    );
  } 

  Path _buildHeartPath(double width) { 
    ...
  } 
} 
```

마지막으로, LiquidCustomProgressIndicator를 Center 위젯 안에 배치하여 하트 모양의 패스를 할당하고 요구 사항에 맞게 스타일을 지정합니다. value 속성은 정의된 AnimationController의 값으로 할당됩니다. 액체 차트가 특정 값에서 증가를 멈추길 원한다면 total과 part를 위젯의 매개변수로 입력할 수 있습니다. 그럼 차이와 차이의 백분율 값을 계산하여 특정 백분율이 도달되었을 때 AnimationController를 중지시킬 수 있습니다. 예를 들어 특정 백분율에 도달했을 때 AnimationController를 중지하는 방법은 아래와 같이 build 메소드에 배치할 수 있습니다:

<div class="content-ad"></div>

```js
const maxPercentage = 70; 
final difference = (widget.total - widget.part); 
final differencePercentage = difference / widget.total * 100; 
final chartValueInPercentage = _animationController.value * 100; 

/// 차트값이 차이나거나 최대 퍼센트보다 클 경우 차트의 증가를 멈춥니다.
if (chartValueInPercentage > differencePercentage || chartValueInPercentage > maxPercentage) { 
  _animationController.stop(); 
} 
```

# 결론

liquid_progress_indicator_v2를 사용하여 우리는 하트 모양의 독특한 액체 차트를 손쉽게 만들었고 이를 통해 사용자 경험을 독특하게 향상시켰습니다. 그러나 액체 차트에 실험해 볼 수있는 수많은 다른 모양들이 있습니다. 어떤 모양이 궁금하신가요? 댓글로 알려주세요! (Tobias Rump & Laura Siewert)