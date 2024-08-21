---
title: "Jetpack Compose 상태 애니메이션 사용 방법 정리"
description: ""
coverImage: "/assets/img/2024-08-21-Becarefulwithjetpackcomposestateanimation_0.png"
date: 2024-08-21 18:11
ogImage: 
  url: /assets/img/2024-08-21-Becarefulwithjetpackcomposestateanimation_0.png
tag: Tech
originalTitle: "Be careful with jetpack compose state animation"
link: "https://medium.com/@camp888/be-careful-with-jetpack-compose-state-animation-89cffc6d773c"
isUpdated: true
updatedAt: 1724246074075
---



<img src="/assets/img/2024-08-21-Becarefulwithjetpackcomposestateanimation_0.png" />

젯팩 컴포즈는 내장된 다양한 애니메이션을 제공하여 앱을 부드럽고 멋지게 만들 수 있습니다. 그러나 때로는 작동 방식에 대한 오해가 예기치 못한 결과로 이어질 수 있습니다.

오늘은 값 기반 애니메이션의 간단한 API를 살펴보고, 이러한 방식이 어떻게 잘못될 수 있는지 알아볼 것입니다.

# 단계 1. 값 기반 애니메이션에 대해 배우기.

<div class="content-ad"></div>

공식 문서에서:

음, 그건 정말 쉽다고 들리죠? 이제 예제를 살펴보겠습니다 (공식 문서에서도 가져온 내용입니다):

```js
var moved by remember { mutableStateOf(false) }
val pxToMove = with(LocalDensity.current) {
    100.dp.toPx().roundToInt()
}
val offset by animateIntOffsetAsState(
    targetValue = if (moved) {
        IntOffset(pxToMove, pxToMove)
    } else {
        IntOffset.Zero
    },
    label = "offset"
)
Box(
    modifier = Modifier
        .offset { // delegate to layout phase
            offset
        }
        .background(Color.Blue)
        .size(100.dp)
        .clickable(
            interactionSource = remember { MutableInteractionSource() },
            indication = null
        ) {
            moved = !moved
        }
)
```

위 코드는 상자의 오프셋을 0부터 100dp까지 애니메이션으로 표시하며, 상자를 클릭하면 한번 클릭한 후 되돌아가 100dp부터 0dp까지 변경합니다.

<div class="content-ad"></div>

<img src="https://miro.medium.com/v2/resize:fit:1400/1*irZHyiHXnb7QClvJZJHcGg.gif" />

대박이네요! 몇 줄의 코드로 이러한 종류의 애니메이션을 만들 수 있습니다.

# 단계 2. 더 복잡한 애니메이션 만들기.

이제 사용자가 끌면 객체의 수평 오프셋을 애니메이션화하고, 드래그 이벤트를 더 이상 받지 않을 때 객체를 초기 오프셋으로 다시 돌리는 상황을 상상해 봅시다.

<div class="content-ad"></div>

수평 드래그 제스처를 감지하는 것부터 시작해봐요.
이를 위해 pointerInput을 사용하고 pointerInput 블록 내에서 detectHorizontalDragGestures를 호출하여 Box 수정자에 적용할 수 있어요:

```js
Box(
    modifier = Modifier
        .pointerInput(Unit) { // 포인터 입력 이벤트를 받음
            detectHorizontalDragGestures( // 수평 제스처를 받음
                onDragStart = { },
                onDragEnd = { },
                onDragCancel = { },
                onHorizontalDrag = { change, dragAmount ->
                    // 수평 오프셋 델타 값을 받음
                }
            )
        }
)
```

좋아요, 그렇게 어렵지 않죠?
이제 Box의 오프셋 변경을 애니메이션화하는 방법을 알아봐야 해요.
IntOffset에는 값이 들어 있고 그 값이 변경될 거예요.
그래서.. 아마.. 값 기반의 애니메이션 API를 사용할 수 있을까요?

한 번 시도해 보죠.

<div class="content-ad"></div>

```kotlin
var offsetState by remember {
    mutableStateOf(IntOffset.Zero)
}
val offsetAnimation by animateIntOffsetAsState(
    targetValue = offsetState,
    label = "offset",
)

Box(
    modifier = Modifier
        .offset {
            offsetAnimation
        }
        .background(Color.Red)
        .size(100.dp)
        .pointerInput(Unit) {
            detectHorizontalDragGestures(
                onDragStart = { },
                onDragEnd = { // 이동을 초기 상태로 복귀
                    offsetState = IntOffset.Zero
                },
                onDragCancel = { // 이동을 초기 상태로 복귀
                    offsetState = IntOffset.Zero
                },
                onHorizontalDrag = { change, dragAmount ->
                    // 오프셋 값 업데이트
                    offsetState = offsetState.copy(
                        x = offsetState.x + dragAmount.roundToInt()
                    )
                }
            )
        }
)
```

거의 문서 예제처럼입니다:
1. 타겟 값이 포함된 animate*AsState 객체를 만듭니다.
2. Modifier.offset에 적용합니다.
3. 이로 인해 이득을 얻습니다.
유일한 차이점은 animate*AsState 객체가 이제 가로 드래그 제스처 이벤트를 받을 때 마다 변경되는 가변 변수를 보유한다는 점입니다. 괜찮지요?

이제 앱을 살펴봅시다.

<img src="https://miro.medium.com/v2/resize:fit:1400/1*vi5L3JCv0MVg-xFAvt44dw.gif" />

<div class="content-ad"></div>

좋아요, 잘 작동했네요!

이제 레이아웃 인스펙터에서 모든 것이 잘 되는지 확인하고 다음 작업으로 넘어갈게요.

![image](https://miro.medium.com/v2/resize:fit:1400/1*t442arsoVQTL_bPHzaueUw.gif)

![image](/assets/img/2024-08-21-Becarefulwithjetpackcomposestateanimation_1.png)

<div class="content-ad"></div>

음, Box 오프셋 변경을 레이아웃 단계로 옮겼음에도 불구하고 이제는 우리 함수가 오프셋 상태 변경마다 다시 구성됩니다.
왜 그럴까요? 자세히 살펴보겠습니다:

```js
var offsetState by remember { // 드래그 이벤트마다 변경됩니다
    mutableStateOf(IntOffset.Zero)
}
val offsetAnimation by animateIntOffsetAsState(
    targetValue = offsetState, // 값이 변경될 때마다 읽기 (재구성 유발)
    label = "offset",
)
```

그래서 animate*AsState는 매 값 기반의 경우에는 적합하지 않다는 것이 드러났네요.
이것이 우리가 오해로 인해 쉽게 실수를 저질렀던 이유이며, 이것이 이 기사의 마지막 단계로 우리를 이끕니다.

# 단계 3. 애니메이션 일시 중단.

<div class="content-ad"></div>

서스펜디드 애니메이션을 사용하기 위해 Animatable 객체를 생성할 것입니다.

문서에서:

```js
val offsetAnimation = remember { // 초기값으로 객체 초기화
    Animatable(IntOffset.Zero, IntOffset.VectorConverter)
}
```

우리는 옵셋 상태(offsetState)를 더 이상 사용하지 않기 때문에 애니메이트할 수 없는(animatable) 값을 읽지 못합니다.

<div class="content-ad"></div>

하지만 값을 어떻게 업데이트할 수 있을까요?
snapTo 함수를 사용하면 현재 값을 대상 값으로 설정하고 어떠한 애니메이션도 적용하지 않습니다.
만약 애니메이션을 실행하고 싶다면 어떻게 해야 할까요?
animateTo 함수를 사용하세요. 이 함수는 대상 값을 업데이트하고 애니메이션을 실행합니다 (기억해둬야 할 점은 조합 단계에서 값이 읽히지 않기 때문에 0회 재구성이 발생합니다). 또한 사용자 정의 애니메이션 스펙도 제공할 수 있습니다.

이전 코드를 새로운 Animatable 객체로 업데이트해보겠습니다:

```js
val coroutineScope = rememberCoroutineScope() // 중단되는 동작을 실행하기 위해
val offsetAnimation = remember {
    Animatable(IntOffset.Zero, IntOffset.VectorConverter)
}

fun animateToInitialPosition() {
    coroutineScope.launch {
        offsetAnimation.animateTo(
            targetValue = IntOffset.Zero,
            animationSpec = spring(
                dampingRatio = Spring.DampingRatioMediumBouncy,
                stiffness = Spring.StiffnessLow,
            )
        )
    }
}

Box(
    modifier = Modifier
        .offset {
            offsetAnimation.value
        }
        .background(Color.Green)
        .size(100.dp)
        .pointerInput(Unit) {
            detectHorizontalDragGestures(
                onDragStart = { },
                onDragEnd = { // 애니메이션을 사용하여 초기 위치로 이동
                    animateToInitialPosition()
                },
                onDragCancel = { // 애니메이션을 사용하여 초기 위치로 이동
                    animateToInitialPosition()
                },
                onHorizontalDrag = { change, dragAmount ->
                    // 애니메이션 없이 새 값으로 업데이트
                    coroutineScope.launch {
                        offsetAnimation.snapTo(
                            targetValue = offsetAnimation.value.copy(
                                offsetAnimation.value.x + dragAmount.roundToInt()
                            ),
                        )
                    }
                }
            )
        }
)
```

보시다시피, 이제 Animatable은 오프셋 값을 보유하고 snapTo 및 animateTo 함수를 통해 업데이트합니다.
animateTo 호출에서 애니메이션 스펙을 사용자 정의할 수도 있습니다.

<div class="content-ad"></div>

<img src="https://miro.medium.com/v2/resize:fit:1400/1*f9x099fE26_AG0yyUYX0Pg.gif" />

# 결론

젯팩 컴포즈를 사용하면 앱에 애니메이션을 쉽게 추가할 수 있습니다. 몇 줄의 코드만으로 앱을 보다 상호작용적이고 시각적으로 매력적으로 만드는 애니메이션을 만들 수 있습니다. 하지만 항상 적용 방법을 정확히 확인하세요.

# 링크

<div class="content-ad"></div>

저장소에 모든 코드가 있는데요. 제 링크드인 프로필을 보고 싶으시다면요.