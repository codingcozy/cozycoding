---
title: "ValueAnimator를 통해 안드로이드 UI를 생동감 있게 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-HowValueAnimatorBringsAndroidsUItoLife_0.png"
date: 2024-08-26 19:16
ogImage: 
  url: /assets/img/2024-08-26-HowValueAnimatorBringsAndroidsUItoLife_0.png
tag: Tech
originalTitle: "How ValueAnimator Brings Androids UI to Life"
link: "https://medium.com/@aghajari/how-valueanimator-makes-androids-ui-dance-01c325d79e42"
isUpdated: false
---



<img src="/assets/img/2024-08-26-HowValueAnimatorBringsAndroidsUItoLife_0.png" />

안녕하세요, 여러분! 오늘은 안드로이드의 ValueAnimator를 살펴보겠습니다. 부드럽고 활기찬 애니메이션을 만드는 데 필수적인 도구입니다. 또한 이를 더 잘 이해하기 위해 간단한 사용자 정의 버전을 만들어 볼 것입니다. 또한 메인 루퍼가 UI 이벤트 및 렌더링을 처리하는 방법을 살펴볼 것입니다. 자, 시작해봅시다. 우리의 UI를 춤추게 하는 방법을 알아봅시다!

# ValueAnimator란 무엇인가요?

먼저, 안드로이드에서 ValueAnimator를 어떻게 정의하는지 살펴봅시다:


<div class="content-ad"></div>

더 간단히 말하면, ValueAnimator는 지정된 기간 동안 시작점에서 끝점까지 값을 전환합니다. 0에서 1까지 변하는 소수 값인 비율(fraction)을 사용하여 애니메이션 진행 상황을 나타냅니다. 이 비율은 애니메이션이 시작할 때 0에서 시작하여 끝날 때 1에 도달합니다. ValueAnimator는 시간이 흐름에 따라 비율을 업데이트함으로써 중간 값을 계산하고 부드러운 전환을 생성합니다.

![HowValueAnimatorBringsAndroidsUItoLife](/assets/img/2024-08-26-HowValueAnimatorBringsAndroidsUItoLife_1.png)

# 예제: 뷰를 수평으로 이동하기

다음은 ValueAnimator의 하위 클래스인 ObjectAnimator를 사용하여 뷰를 오른쪽으로 100픽셀 이동하는 기본적인 예제입니다:

<div class="content-ad"></div>

```js
ObjectAnimator.ofFloat(view, "translationX", 0f, 100f).apply {
    duration = 1000L
    start()
}
```

또는 ValueAnimator를 직접 사용할 수도 있습니다:

```js
ValueAnimator.ofFloat(0f, 100f).apply {
    duration = 1000L
    addUpdateListener {
        view.translationX = animatedValue as Float
    }
    start()
}
```

# 뷰를 양방향으로 이동하기

<div class="content-ad"></div>

뷰를 한 번에 오른쪽으로 200픽셀, 아래로 350픽셀 이동하고 싶다면 두 개의 독립적인 ValueAnimator를 사용하여 이를 달성할 수 있습니다:

```js
ValueAnimator.ofFloat(0f, 200f).apply {
    duration = 1000L
    addUpdateListener {
        view.translationX = animatedValue as Float
    }
    start()
}

ValueAnimator.ofFloat(0f, 350f).apply {
    duration = 1000L
    addUpdateListener {
        view.translationY = animatedValue as Float
    }
    start()
}
```

또는, 하나의 ValueAnimator만 사용하여 더 효율적으로 만들 수도 있습니다. animatedFraction을 이용하면 ValueAnimator의 내부 계산을 건너뛰고 개별적인 계산을 할 수 있습니다:

```js
ValueAnimator.ofFloat(0f).apply {
    duration = 1000L
    addUpdateListener {
        view.translationX = 200f * animatedFraction
        view.translationY = 350f * animatedFraction
    }
    start()
}
```

<div class="content-ad"></div>

이 예제에서는 animatedFraction을 사용하여 뷰의 위치를 수동으로 계산합니다. 이 방법을 사용하면 분수를 애니메이션화하고 뷰의 속성에 대한 사용자 정의 계산을 적용하여 로직을 간단하게 만들 수 있습니다. 따라서 0에서 1로 분수를 애니메이션화하고 나머지는 우리만의 사용자 정의 논리로 처리할 수 있습니다.

## ValueAnimator는 언제 업데이트해야 하는지 어떻게 알까요?

각 디스플레이 새로 고침마다 ValueAnimator는 세 가지 주요 매개변수를 사용하여 분수를 다시 계산합니다: 애니메이션 시작 시간, 프레임 렌더 전 현재 시간 및 총 기간.

그렇다면 ValueAnimator가 새로운 프레임이 렌더링될 때 알고, 언제 업데이트 콜백을 호출해야 할까요?
이 마법은 Choreographer의 도움으로 이루어집니다. ValueAnimator는 디스플레이 새로 고침 비율에 애니메이션을 동기화하기 위해 Choreographer를 사용합니다.

<div class="content-ad"></div>

ValueAnimator가 시작되면 Choreographer에 프레임 콜백을 등록합니다. Choreographer는 화면이 다음 프레임을 그릴 준비가 되었을 때를 나타내는 VSYNC 신호를 수신합니다. VSYNC 신호를 받으면 Choreographer는 ValueAnimator의 doAnimationFrame() 메서드를 호출하여 분수를 업데이트하고 새로운 애니메이션 값으로 업데이트 리스너를 호출합니다.

![Image](/assets/img/2024-08-26-HowValueAnimatorBringsAndroidsUItoLife_2.png)

- ValueAnimator는 Choreographer와 프레임 콜백을 등록합니다.
- Choreographer는 VSYNC 신호를 기다려서 화면 업데이트가 디스플레이의 주사율과 동기화되도록 합니다.
- VSYNC에서 Choreographer가 ValueAnimator의 doAnimationFrame()을 호출합니다. ValueAnimator는 분수를 업데이트하고 새로운 애니메이션 값이 계산됩니다.

# 간단한 사용자 정의 애니메이터 생성

<div class="content-ad"></div>

안녕하세요! 안드로이드 Choreographer API를 사용하여 ValueAnimator의 간단한 버전을 만들어 봅시다. 여기에 샘플 구현이 있어요:

```js
fun startMySimpleAnimator(
    duration: Long = 300,
    startDelay: Long = 0,
    interpolator: TimeInterpolator = AccelerateDecelerateInterpolator(),
    onUpdate: (Float) -> Unit,
) {
    val durationNanos = duration * 1_000_000
    val startTimeNanos = System.nanoTime()

    val frameCallback = object : FrameCallback {
        override fun doFrame(frameTimeNanos: Long) {
            val elapsedNanos = frameTimeNanos - startTimeNanos
            val fraction = (elapsedNanos.toFloat() / durationNanos).coerceIn(0f, 1f)
            val interpolatedFraction = interpolator.getInterpolation(fraction)
            onUpdate(interpolatedFraction)

            if (fraction < 1f) {
                Choreographer.getInstance().postFrameCallback(this)
            }
        }
    }
    Choreographer.getInstance().postFrameCallbackDelayed(frameCallback, startDelay)
}
```

```js
startMySimpleAnimator(duration = 1000L) { animatedFraction ->
    view.translationX = 200f * animatedFraction
    view.translationY = 350f * animatedFraction
}
```

이 간단한 코드를 사용하여 animatedFraction에 의존하는 ValueAnimator와 유사한 기본 애니메이터를 만들었습니다. 이 예제를 확장하여 ValueAnimator가 제공하는 더 많은 기능을 지원하도록 쉽게할 수 있습니다. 그리고 화면의 새로 고침 속도와 매끄럽게 동기화되는 애니메이션을 유지할 수 있어요. 이렇게 간단하죠!

<div class="content-ad"></div>

# 하나의 타이밍 펄스로 애니메이션 동기화

안드로이드는 또한 다음과 같이 설명합니다:

우리는 기본 예제를 약간 수정하여 사용자 정의 애니메이터 구현에서 이와 같은 동작을 달성할 수 있습니다. 각 MySimpleAnimator가 자체 프레임 콜백을 처리하는 대신, 모든 애니메이션이 동기화되도록 이를 단일 타이밍 펄스로 집중할 수 있습니다. 아래에 설명된 방법을 따르세요:

```js
class MySimpleAnimator(
    private val onUpdate: (Float) -> Unit,
) : Animator(), FrameCallback {

    private var startTimeNanos: Long = 0L
    private var durationNanos: Long = 300L
    private var startDelayNanos: Long = 0L
    private var timeInterpolator: TimeInterpolator? = AccelerateDecelerateInterpolator()
    private var mIsRunning: Boolean = false

    override fun doFrame(frameTimeNanos: Long) {
        val elapsedNanos = frameTimeNanos - startTimeNanos - startDelayNanos
        if (elapsedNanos <= 0) {
            return
        }
        val fraction = (elapsedNanos.toFloat() / durationNanos).coerceIn(0f, 1f)
        val interpolatedFraction = timeInterpolator?.getInterpolation(fraction)
        onUpdate(interpolatedFraction ?: fraction)
        mIsRunning = fraction < 1f
    }

    override fun start() {
        startTimeNanos = System.nanoTime()
        mIsRunning = true
        MySimpleAnimatorHandler.attach(this)
    }

    override fun setStartDelay(startDelay: Long) {
        startDelayNanos = startDelay * NANO_MS
    }

    override fun setDuration(duration: Long): MySimpleAnimator {
        durationNanos = duration * NANO_MS
        return this
    }

    override fun setInterpolator(value: TimeInterpolator?) {
        timeInterpolator = value
    }

    override fun getStartDelay(): Long = startDelayNanos / NANO_MS
    override fun getDuration(): Long = durationNanos / NANO_MS
    override fun getInterpolator(): TimeInterpolator? = timeInterpolator
    override fun isRunning(): Boolean = mIsRunning
}

private const val NANO_MS = 1_000_000
```

<div class="content-ad"></div>

```kotlin
private object MySimpleAnimatorHandler : FrameCallback {
    private val animations = mutableListOf<MySimpleAnimator>()

    fun attach(animator: MySimpleAnimator) {
        if (animations.isEmpty()) {
            Choreographer.getInstance().postFrameCallback(this)
        }
        animations.add(animator)
    }

    override fun doFrame(frameTimeNanos: Long) {
        animations.removeIf { animator ->
            animator.doFrame(frameTimeNanos)
            animator.isRunning.not()
        }
        if (animations.isNotEmpty()) {
            Choreographer.getInstance().postFrameCallback(this)
        }
    }
}
```

이 설정은 하나의 FrameCallback을 사용하여 여러 애니메이션을 관리합니다. 이는 ValueAnimator가 모든 애니메이션에 대해 하나의 타이밍 펄스를 사용하는 방식과 유사합니다.

# Choreographer의 작동 방식은?

이제 ValueAnimator를 살펴보았으니, Choreographer를 사용하여 새로운 프레임을 렌더링할 시점을 안드로이드가 어떻게 결정하는지 살펴보겠습니다.


<div class="content-ad"></div>

안드로이드에 따르면, Choreographer는 Android의 UI 랜더링 시스템 중심 부분으로 애니메이션, 입력 이벤트 및 그리기의 타이밍을 관리합니다.

Choreographer를 시스템 수준의 타이밍 신호와 응용 프로그램 수준 콜백을 연결하는 다리로 생각해보세요. UI 렌더링 및 애니메이션을 다루고 있기 때문에 일반적으로 Main Looper에 연결된 MessageQueue를 사용하여 작동합니다. 각 Looper 쓰레드는 자체 Choreographer를 가지고 있음을 유의하세요.

# Looper Thread 이해하기

Choreographer가 작동하는 방식을 이해하기 위해서는 우리의 Main Thread 또는 UI Thread가 Looper Thread임을 이해하는 것이 중요합니다.

<div class="content-ad"></div>

루퍼 스레드는 메시지 및 이벤트를 대기하고 처리하는 루프를 계속 실행하는 스레드 유형입니다. Android에서 메인 스레드는 UI 업데이트, 사용자 상호 작용 처리 및 UI에서 수행해야 하는 작업을 담당하는 특별한 루퍼 스레드입니다. 이 스레드는 앱이 응답하도록 유지하며 사용자와 앱 간 상호 작용을 관리합니다.

메인 스레드는 주요 루퍼를 사용하여 연속된 루프를 실행하며 MessageQueue를 기반으로 실행됩니다. 다른 스레드와는 달리 이 MessageQueue는 앱 전체가 종료될 때까지 종료할 수 없습니다.

이 MessageQueue 내에서 Choreographer에 의해 특정 특수 메시지가 처리됩니다. 이러한 메시지들은 UI 업데이트, 애니메이션 및 렌더링의 타이밍 및 동기화를 관리하는 데 중요합니다.

# VSYNC를 이용한 UI 동기화

<div class="content-ad"></div>

VSYNC 신호: VSYNC 또는 수직 동기 펄스는 디스플레이가 새 프레임을 그리기 시작할 준비가 되었음을 나타내는 하드웨어 생성 신호입니다. 이 신호는 디스플레이 업데이트를 화면의 주사율과 동기화하는 데 중요합니다.

Choreographer는 VSYNC 신호를 수신하기 위해 UI 업데이트의 타이밍을 관리합니다. Android OS를 통해 SurfaceFlinger를 통해 DisplayEventReceiver로 이러한 신호를 수신하기위한 수준 낮은 구성 요소를 사용합니다. SurfaceFlinger는 시스템의 컴포지터로 작용하여 디스플레이 하드웨어와 소프트웨어 레이어를 조정합니다. VSYNC 신호가 DisplayEventReceiver로 라우팅되도록 보장하고, 이후 Choreographer에 프레임 업데이트 처리를 알립니다.

아래 순서도는 Android가 VSYNC 신호를 사용하여 UI 업데이트를 관리하는 방법을 설명합니다. 프로세스는 주 스레드가 초기화하고 ValueAnimator를 시작하여 Choreographer와 프레임 콜백을 등록하는 것으로 시작됩니다. 디스플레이 주사율과 동기화를 유지하기 위해 Choreographer은 scheduleVsync()를 호출하여 다음 VSYNC 이벤트를 요청합니다. 이 요청은 DisplayEventReceiver에 의해 처리되며, OS와 통신하여 VSYNC 신호를 수신합니다.

<div class="content-ad"></div>

VSYNC 신호를 수신하면 SurfaceFlinger는 원시 코드에서 dispatchVsync()를 직접 호출하고, 이후 Choreographer의 onVsync()를 호출합니다. Choreographer는 UI를 직접적으로 업데이트하는 대신, MessageQueue에서 doFrame() 메시지를 예약합니다. 이를 통해 MessageQueue에 있는 모든 미처리된 이전 메시지가 프레임을 업데이트하기 전에 처리됩니다. doFrame() 메시지가 처리되면 Choreographer는 ValueAnimator의 프레임 콜백을 포함한 여러 등록된 콜백을 호출하여 애니메이션의 분수를 업데이트하고 새로운 애니메이션 값을 계산하고 UI에 변경 사항을 적용합니다. 애니메이션이 계속 실행 중인 경우, ValueAnimator는 프레임 콜백을 다시 등록하고, 각 프레임에 대해 프로세스가 계속됩니다.

참고: VSYNC 신호를 수신하는 루프는 ValueAnimator의 프레임 콜백에 의존하지 않습니다. 이 루프는 메인 스레드가 활성 상태인 한 계속해서 실행됩니다.

# Choreographer에서의 콜백의 종류

doFrame()이 트리거되면 다음 순서대로 다양한 유형의 콜백이 실행됩니다:

<div class="content-ad"></div>

- 입력 Callbacks: 터치 이벤트 및 키 입력과 같은 사용자 상호 작용을 처리합니다. 이는 UI가 사용자 조작에 빠르게 응답하도록 보장합니다.
- 애니메이션 Callbacks: ValueAnimator 및 사용자 정의 애니메이터로 관리되는 애니메이션을 업데이트합니다. 이 단계에서는 현재 애니메이션 상태를 계산하고 UI 요소에 변경 사항을 적용합니다.
- 트래버스 Callbacks: 뷰 계층 구조의 레이아웃 및 그리기를 관리하며 화면에 뷰의 크기, 위치 및 렌더링을 결정합니다. 창의 루트 뷰로 작용하는 ViewRootImpl은 트래버스 Callback을 수신합니다. 이 콜백이 트리거되면 ViewRootImpl은 performTraversals() 메서드를 호출합니다. 이 메서드는 전체 뷰 계층 구조의 측정, 배치 및 그리기를 담당하며 각 뷰가 화면에 적절히 크기 조정, 위치 조정 및 렌더링된 상태를 보장합니다.
- 커밋 Callbacks: 디스플레이에 대한 변경 사항을 최종 확정 및 커밋합니다.

# 성능 모니터링 및 프레임 속도 조정

Android는 건너뛴 프레임 수를 감지하고 해당 수가 많은 경우 개발자에게 경고하여 성능을 모니터링하는 데 도움이 됩니다. performTraversals() 프로세스의 끝에서 Android는 장치에 대한 선호하는 프레임 속도를 설정합니다. 현재 작업 부하 및 표시되는 콘텐츠에 기반하여 새로 고침 속도를 동적으로 변경함으로써 Android는 배터리 사용량을 최적화하고 전반적인 성능을 향상시킵니다.

# Jetpack Compose에서 애니메이션

<div class="content-ad"></div>

젯팩 캄포즈에서 Animatable 클래스는 MonotonicFrameClock을 활용하여 AndroidUiFrameClock를 주로 사용하여 애니메이션에 일관된 시간 업데이트를 보장합니다. 이 클록 메커니즘은 일반적으로 메인 스레드의 UI Looper와 연결되어 있어 Android의 Choreographer와 매끄럽게 통합될 수 있습니다. 따라서 이 글에서 논의된 ValueAnimator와 Choreographer에 관한 원리와 메커니즘은 젯팩 캄포즈에서 애니메이션이 작동하는 방식에도 적용됩니다.

# 결론

Android 메인 스레드는 메인 루퍼와 연속적인 루프를 실행하는 루퍼 스레드로 작동합니다. 항상 UI 관련 메시지를 처리할 준비가 되어 있습니다. Choreographer는 DisplayEventReceiver를 통해 VSYNC 신호를 수신하고 doFrame() 메시지를 MessageQueue에 포스팅하여 프레임 업데이트를 예약합니다. doFrame()이 처리되면 Choreographer는 다양한 콜백을 트리거하며, 그 중에는 애니메이션용으로도 사용되는 것이 포함됩니다. 이러한 콜백은 ValueAnimator에 신호를 보내어 현재 시간과 애니메이션 지속 시간을 기반으로 분수를 업데이트하도록 합니다. 그 후 ValueAnimator는 애니메이션 값을 업데이트하고 업데이트 콜백을 호출하여 UI를 부드럽게 움직이며 생생하게 만듭니다.

# 마무리

<div class="content-ad"></div>

그게 다야! 우리는 ValueAnimator가 안드로이드 UI를 생동감 있게 만드는 방법을 배웠고, Choreographer가 모든 것을 원할하게 유지하는 방법을 알게 되었으며, 심지어 우리만의 사용자 정의 애니메이터를 만드는 방법도 배웠어요. 

읽어 주셔서 감사합니다! 계속해서 UI를 춤추게 만들어 보세요!