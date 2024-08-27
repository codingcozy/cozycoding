---
title: "Kotlin 2.0.20 Compose 컴파일러의 주요 업데이트 내용 정리"
description: ""
coverImage: "/assets/img/2024-08-26-Kotlin2020KeyUpdatestotheComposeCompiler_0.png"
date: 2024-08-26 19:18
ogImage: 
  url: /assets/img/2024-08-26-Kotlin2020KeyUpdatestotheComposeCompiler_0.png
tag: Tech
originalTitle: "Kotlin 2.0.20 Key Updates to the Compose Compiler"
link: "https://medium.com/@camp888/kotlin-2-0-20-release-interesting-things-about-compose-compiler-update-bb8a64d1cb9a"
isUpdated: true
updatedAt: 1724743413296
---


<img src="/assets/img/2024-08-26-Kotlin2020KeyUpdatestotheComposeCompiler_0.png" />

최근 Compose Compiler의 최신 업데이트를 놓친 분들을 위해 간단히 복습해보겠습니다.

# 기본적으로 활성화된 강력한 스킵 모드

주요 포인트:
1. 불안정한 매개변수를 가진 Composable은 스킵할 수 있습니다.
2. 강력한 스킵 모드는 불안정한 캡처를 가진 람다를 자동으로 기억하여, Composable 함수에서 람다를 recomposition을 피하기 위해 remember로 래핑할 필요가 없게 되었습니다.
3. ❗️프로젝트를 업그레이드한 후에는 버그와 성능 문제를 확인해야 할 수 있습니다.

<div class="content-ad"></div>

강한 스킵 모드란 무엇인가요?
기본적으로 Compose 컴파일러는 모든 인수가 안정적인 값을 갖는 경우 composable 함수를 스킵 가능하다고 표시합니다. 강한 스킵 모드는 이를 변경합니다.
강한 스킵을 활성화하면:
1. 모든 다시 시작 가능한 composable 함수가 스킵 가능하게 됩니다. 이는 불안정한 파라미터가 있는지 여부에 관계없이 적용됩니다. 다시 시작할 수 없는 composable 함수는 스킵 불가능합니다.
2. Compose 컴파일러는 Composable 함수 내의 모든 람다를 remember 호출로 래핑합니다.

강한 스킵 모드를 비활성화하려면:
1. Composable 함수의 경우, 어노테이션 @NonSkippableComposable을 사용하십시오.

```js
@NonSkippableComposable
@Composable
fun MyNonSkippableComposable {}
```

2. 람다의 경우, 어노테이션 @DontMemoize를 사용하십시오.

<div class="content-ad"></div>

```kotlin
val lambda = @DontMemoize {
    ...
}
```

3. 전체 모듈/프로젝트에 대한 설정. build.gradle 파일에 다음 라인을 추가하세요:

```kotlin
composeCompiler {
   featureFlags = setOf(
        ComposeFeatureFlag.StrongSkipping.disabled()
    )
}
```

# 스킵하지 않는 그룹 최적화

<div class="content-ad"></div>

간단히 말하자면: 스킵할 수 없고 다시 시작할 수 없는 결합 가능한 함수들은 더 이상 결합 가능한 함수의 본문 주위에 그룹을 생성하지 않습니다. 이는 더 적은 할당으로 성능 향상으로 이어집니다.

이 기능을 활성화하려면:

```js
composeCompiler {
   featureFlags = setOf(
        ComposeFeatureFlag.OptimizeNonSkippingGroups
    )
}
```

# ⚠️ 멀티플랫폼 프로젝트에 대한 주의사항 ⚠️

<div class="content-ad"></div>

2.0.0에 소개된 불필요한 재구성 문제를 해결했습니다.

Compose 컴파일러 2.0.0에는 때때로 JVM이 아닌 대상이있는 멀티플랫폼 프로젝트에서 타입의 안정성을 잘못 추론할 수있는 문제가 있습니다. 이는 불필요한(또는 끝없는) 재구성으로 이어질 수 있습니다. Kotlin 2.0.0으로 제작된 Compose 앱을 2.0.10 버전 이상으로 업데이트하는 것을 강력히 권장합니다.

Compose 컴파일러 2.0.10 이상으로 빌드된 앱이지만 버전 2.0.0으로 빌드된 종속성을 사용하는 경우, 이전 종속성은 여전히 재구성 문제를 일으킬 수 있습니다. 이를 방지하려면 앱과 동일한 Compose 컴파일러로 빌드된 버전의 종속성을 업데이트하십시오.