---
title: "Kotlin inline, noinline, crossinline 개념 쉽게 이해하기"
description: ""
coverImage: "/assets/img/2024-08-26-KotlininlinenoinlineandcrossinlineThewholestorysimplified_0.png"
date: 2024-08-26 19:19
ogImage: 
  url: /assets/img/2024-08-26-KotlininlinenoinlineandcrossinlineThewholestorysimplified_0.png
tag: Tech
originalTitle: "Kotlin inline, noinline and crossinline. The whole story simplified"
link: "https://medium.com/@shivanshgoel007/kotlin-inline-noinline-and-crossinline-the-whole-story-simplified-80a9ea802e09"
isUpdated: false
---


<img src="/assets/img/2024-08-26-KotlininlinenoinlineandcrossinlineThewholestorysimplified_0.png" />

안녕하세요! 이 기사는 Kotlin에서 inline, crossinline 및 noinline 키워드의 사용법을 간단하게 설명합니다.

일반적으로 Kotlin에서 고차 함수를 사용해야 하는 시나리오를 만나게 됩니다 (함수를 매개변수로 전달하는 경우). 아래와 같이 할 수 있습니다:

```js
fun main() {
    println("메인 함수 내부")
    test {
        println("콜백 함수 내부")
    }
    println("다시 메인 함수 내부")
}

fun test(cb: () -> Unit) {
    println("테스트 함수 내부")
    cb()
    println("콜백 완료")
}
```

<div class="content-ad"></div>

작업이 완료되었을 것이고 다음과 같은 결과가 나올 겁니다.

```js
Inside main
Inside test
Inside callback
콜백 완료
Inside main again
```

# 그래서, 문제가 무엇인가요?

![그림](/assets/img/2024-08-26-KotlininlinenoinlineandcrossinlineThewholestorysimplified_1.png)

<div class="content-ad"></div>

디컴파일된 바이트코드를 보면 문제를 깨닫게 됩니다:

```js
public final class MainKt {
   public static final void main() {
      String var0 = "Inside main";
      System.out.println(var0);
      test((Function0)null.INSTANCE);
      var0 = "Inside main again";
      System.out.println(var0);
   }

   public static final void test(@NotNull Function0 cb) {
      Intrinsics.checkNotNullParameter(cb, "cb");
      String var1 = "Inside test";
      System.out.println(var1);
      cb.invoke();
      var1 = "Callback completed";
      System.out.println(var1);
   }

    // $FF: synthetic method
   public static void main(String[] args) {
      main();
   }
}
```

이 바이트코드의 간소화된 버전을 살펴봅시다:

```js
public final class MainKt {
   public static final void main() {
      System.out.println("Inside main");
      test(a); // a는 람다 함수의 인스턴스입니다
      System.out.println("Inside main again");
   }

   public static final void test(Function cb) {
      System.out.println("Inside test");
      cb.invoke();
      System.out.println("Callback completed");
   }

   public static void main(String[] args) {
      main();
   }
}
```

<div class="content-ad"></div>

위의 코드 세부사항까지 들어갈 필요는 없고 다음 라인을 주의하세요:

test((Function0)null.INSTANCE);

여기서 test 함수를 호출할 때마다 test 함수의 인수로 전달할 Function0 타입의 인스턴스가 생성됨을 알 수 있습니다. 안드로이드 프로젝트에서 작업 중이라면, 메모리 과다 사용으로 저장소 목록 성능에 영향을 줄 수 있습니다.
 
![이미지](/assets/img/2024-08-26-KotlininlinenoinlineandcrossinlineThewholestorysimplified_2.png)

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경하면 됩니다.

# 인라인 함수

메모리 사용량을 최적화하기 위해 인라인 키워드를 사용할 수 있습니다.

인라인 함수를 사용하여 테스트 함수를 인라인으로 만들어 보고 무슨 일이 벌어지는지 살펴봅시다:

```js
fun main() {
    println("메인 함수 내부")
    test {
        println("콜백 함수 내부")
    }
    println("다시 메인 함수 내부")
}

inline fun test(cb: () -> Unit) {
    println("테스트 함수 내부")
    cb()
    println("콜백 완료")
}
```

<div class="content-ad"></div>

위 코드에서는 여전히 동일한 출력을 얻습니다:

```js
Inside main
Inside test
Inside callback
Callback completed
Inside main again
```

하지만, 위의 코드에 대한 생성된 바이트 코드를 살펴봅시다:

```js
public final class MainKt {
   public static final void main() {
      String var0 = "Inside main";
      System.out.println(var0);
      int $i$f$test = false;
      String var1 = "Inside test";
      System.out.println(var1);
      int var2 = false;
      String var3 = "Inside callback";
      System.out.println(var3);
      var1 = "Callback completed";
      System.out.println(var1);
      var0 = "Inside main again";
      System.out.println(var0);
   }

   public static final void test(@NotNull Function0 cb) {
      Intrinsics.checkNotNullParameter(cb, "cb");
      int $i$f$test = false;
      String var2 = "Inside test";
      System.out.println(var2);
      cb.invoke();
      var2 = "Callback completed";
      System.out.println(var2);
   }

   // $FF: synthetic method
   public static void main(String[] args) {
      main();
   }
}
```

<div class="content-ad"></div>

위의 내용을 간단히 요약하면 아래와 같이 보일 것입니다:

```js
public final class MainKt {
   public static final void main() {
      System.out.println("main 함수 내부");
      System.out.println("test 함수 내부");
      System.out.println("콜백 내부");
      System.out.println("콜백 완료");
      System.out.println("main 함수 다시 실행");
   }

   public static final void test(Function cb) {
      System.out.println("test 함수 내부");
      cb.invoke();
      System.out.println("콜백 완료");
   }

   public static void main(String[] args) {
      main();
   }
}
```

이제 main 함수 내부를 자세히 살펴보면, cb 함수에 대한 인스턴스가 생성되지 않았음을 알 수 있습니다. 대신, 컴파일 시점에서 test 함수 전체 본문이 포함되며, 각 test 함수 호출에 대한 추가 메모리를 절약합니다.

요약하면, 인라인 함수에 대해로니다:

<div class="content-ad"></div>

- 고차 함수를 사용할 때 추가 메모리를 절약하는 데 사용됩니다.
- 함수 앞에 `inline` 키워드를 추가하면 컴파일러가 호출하는 위치에 함수 본문을 복사해 넣습니다.

`inline` 키워드를 사용할 때 기억해야 할 중요한 사항:

- 람다를 매개변수로 받는 함수에만 함수를 `inline`으로 만들어도 의미가 있습니다.
- 우리의 바이트 코드를 크게 만들 수 있기 때문에 함수가 큰 경우 `inline`으로 만들지 않는 것이 좋습니다. 이로 인해 메모리 최적화를 위해 찾고 있던 문제가 발생할 수 있습니다.

# `noinline` 키워드

<div class="content-ad"></div>

한 개 이상의 람다 함수를 매개변수로 받는 함수의 예시를 살펴봅시다:

```js
fun main() {
    println("Inside main")
    test(
        cb1 = {
            println("Inside callback 1")
        },
        cb2 = {
            println("Inside callback 2")
        }
    )
    println("Inside main again")
}

inline fun test(cb1: () -> Unit, cb2: () -> Unit) {
    println("Inside test")
    cb1()
    println("Callback 1 completed")
    cb2()
    println("Callback 2 completed")
}
```

예상대로 위 프로그램은 다음과 같은 출력을 제공할 것입니다:

```js
Inside main
Inside test
Inside callback 1
Callback 1 completed
Inside callback 2
Callback 2 completed
Inside main again
```

<div class="content-ad"></div>

위의 토론에서 배운 대로, 컴파일러는 테스트 함수를 람다 cb1과 cb2와 함께 메인 함수 내에 복사하여 붙여넣을 것입니다.

그러나 다음과 같은 경우를 가정해 봅시다:

- cb1에 대해 인라인이 필요한 경우
- 그럼에도 어떤 이유로 인해 cb2에 대해 전통적인 함수 인스턴스가 필요한 경우

이 때 noinline 키워드가 유용합니다. 다음 코드를 살펴보겠습니다:

<div class="content-ad"></div>

```js
fun main() {
    println("메인 함수 내부")
    test(
        cb1 = {
            println("콜백 1 내부")
        },
        cb2 = {
            println("콜백 2 내부")
        }
    )
    println("다시 메인 함수 내부")
}

inline fun test(cb1: () -> Unit, noinline cb2: () -> Unit) {
    println("테스트 함수 내부")
    cb1()
    println("콜백 1 완료")
    cb2()
    println("콜백 2 완료")
}
```

여기서 cb2를 noinline로 만들었습니다. 따라서 컴파일러는 main 함수에 cb2 블록을 복사하지 않습니다. 디컴파일된 바이트코드를 살펴보겠습니다:

```js
public final class MainKt {
   public static final void main() {
      String var0 = "메인 함수 내부";
      System.out.println(var0);
      Function0 cb2$iv = (Function0)null.INSTANCE;
      int $i$f$test = false;
      String var2 = "테스트 함수 내부";
      System.out.println(var2);
      int var3 = false;
      String var4 = "콜백 1 내부";
      System.out.println(var4);
      var2 = "콜백 1 완료";
      System.out.println(var2);
      cb2$iv.invoke();
      var2 = "콜백 2 완료";
      System.out.println(var2);
      var0 = "다시 메인 함수 내부";
      System.out.println(var0);
   }

   public static final void test(@NotNull Function0 cb1, @NotNull Function0 cb2) {
      Intrinsics.checkNotNullParameter(cb1, "cb1");
      Intrinsics.checkNotNullParameter(cb2, "cb2");
      int $i$f$test = false;
      String var3 = "테스트 함수 내부";
      System.out.println(var3);
      cb1.invoke();
      var3 = "콜백 1 완료";
      System.out.println(var3);
      cb2.invoke();
      var3 = "콜백 2 완료";
      System.out.println(var3);
   }

   // $FF: synthetic method
   public static void main(String[] args) {
      main();
   }
}
```

간단하게 표현한 버전도 살펴봅시다:


<div class="content-ad"></div>

```java
public final class MainKt {
   public static final void main() {
      System.out.println("Inside main");
      Function cb2 = //Callback function instance
      System.out.println("Inside test");
      System.out.println("Inside callback 1");
      System.out.println("Callback 1 completed");
      cb2.invoke();
      System.out.println("Callback 2 completed");
      System.out.println("Inside main again");
   }

   public static final void test(Function cb1, Function cb2) {
      System.out.println("Inside test");
      cb1.invoke();
      System.out.println("Callback 1 completed");
      cb2.invoke();
      System.out.println("Callback 2 completed");
   }

   public static void main(String[] args) {
      main();
   }
}
```

위 코드를 보면 cb2에 Function 인스턴스가 있습니다.

noinline 키워드를 요약해 보면 다음과 같습니다:

- 이름에서 알 수 있듯이, 람다 매개변수와 함께 사용되어 해당 람다를 포함하는 함수의 인라인화에서 제외합니다.
- 람다 본문이 큰 경우 noinline을 사용하는 것이 좋습니다.

<div class="content-ad"></div>

# crossinline 키워드

람다에 조건 로직이 포함된 예제를 살펴보겠습니다:

```kotlin
fun main() {
    println("메인 함수 내부")
    val num = 4
    test(
        cb = {
            println("콜백 내부 1")
            if (num % 2 == 0)
                return
            println("숫자는 홀수입니다")
            //...
        }
    )
    println("다시 메인 함수 내부")
}

inline fun test(cb: () -> Unit) {
    println("테스트 내부")
    cb()
    println("콜백 완료")
}
```

위 예제에서는 숫자가 짝수이면 람다가 반환되도록 원합니다. 위 예제에서 좋습니다(num = 4이므로 참입니다). 그러므로 아래의 결과를 기대할 것입니다.

<div class="content-ad"></div>

```js
메인 안에
테스트 안에
콜백 내부 1
콜백 완료
메인 다시
```

하지만 그게 사실은 아니에요

![이미지](https://miro.medium.com/v2/resize:fit:800/0*owZOvJGo2YySXbYA.gif)


실제 결과물은:

<div class="content-ad"></div>

```js
메인 내부
테스트 내부
콜백 1 내부
```

하지만 왜 그럴까요?

그 이유는, 컴파일러가 테스트 함수를 인라인하는 동안에, 람다 논리 대신 메인 함수 자체 내부에 리턴 문을 추가했습니다. 이를 비로컬 리턴이라고 합니다.

이를 해결하기 위해 다음을 수행할 수 있습니다:

<div class="content-ad"></div>

```js
import kotlin.random.Random

fun main() {
    println("main 안에 있어요")
    val num = 4
    test(
        cb = {
            println("콜백 내부 1")
            if (num % 2 == 0)
                return@test
            println("숫자는 홀수에요")
            //...
        }
    )
    println("다시 main 안에 있어요")
}

inline fun test(cb: () -> Unit) {
    println("테스트 내부에 있어요")
    cb()
    println("콜백이 완료됐어요")
}
```

여기서 return 대신에 return@test를 사용했는데, 이는 컴파일러에게 이것이 람다 블록 전용 지역 리턴임을 알려줍니다.

위 코드의 실행 결과는:

```js
main 안에 있어요
테스트 내부에 있어요
콜백 내부 1
콜백이 완료됐어요
다시 main 안에 있어요
```

<div class="content-ad"></div>


![Image](/assets/img/2024-08-26-KotlininlinenoinlineandcrossinlineThewholestorysimplified_3.png)

문제를 해결하긴 하지만 여전히 실수로 비지역 반환을 작성하여 문제를 찾기 위해 몇 시간을 낭비할 수 있습니다.

개발자로서, 컴파일 시간 안전성을 좋아하는데 이것이 crossinline 키워드가 등장하는 곳입니다.

다음은 사용 방법입니다:


<div class="content-ad"></div>

```kotlin
inline fun test(crossinline cb: () -> Unit) {
    println("test 함수 내부")
    cb()
    println("콜백 완료")
}
```

이제 지역이 아닌 반환문을 사용하면 컴파일 오류가 발생합니다:

```kotlin
fun main() {
    println("main 함수 내부")
    val num = 4
    test(
        cb = {
            println("콜백 내부 1")
            if (num % 2 == 0)
                return@test
            println("숫자는 홀수입니다")
            //...
        }
    )
    println("다시 main 함수 내부")
}

inline fun test(crossinline cb: () -> Unit) {
    println("test 함수 내부")
    cb()
    println("콜백 완료")
}
```

위 코드는 crossinline으로 비지역 반환문을 제한하므로 다음과 같이 사용해야합니다:

<div class="content-ad"></div>

```kotlin
fun main() {
    println("메인 함수 안에서")
    val num = 4
    test(
        cb = {
            println("콜백 내부 1")
            if (num % 2 == 0)
                return@test
            println("숫자는 홀수입니다")
            //...
        }
    )
    println("다시 메인 함수 안에서")
}

inline fun test(crossinline cb: () -> Unit) {
    println("테스트 함수 안에서")
    cb()
    println("콜백 완료")
}
```

크로스인라인 키워드를 요약하면, 람다 인수에서 로컬 반환 문을 사용할 때 컴파일 시간 안전성을 제공합니다.

이것으로 이 기사를 마치겠습니다.

<img src="https://miro.medium.com/v2/resize:fit:996/0*X7LNRkvr1xRdnnE2.gif" />


<div class="content-ad"></div>

읽어주셔서 감사합니다. 🙌🙏✌.

의문이나 질문, 제안 사항이 있으면 언제든지 댓글을 달아주세요. 답변해드리는 것을 기쁘게 생각하겠습니다.

마음에 드셨다면 👏 박수를 치거나 안드로이드 개발, 코틀린 및 KMP에 대한 유용한 기사를 더 받아보시려면 팔로우해주세요.

안드로이드, 코틀린 및 KMP와 관련된 도움이 필요하시면 언제든지 편하게 물어봐주세요.

<div class="content-ad"></div>

LinkedIn과 Github에서 나를 팔로우해주세요!

건강한 개발 활동하세요!