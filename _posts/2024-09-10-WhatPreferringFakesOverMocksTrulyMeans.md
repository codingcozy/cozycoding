---
title: "모의(mock) 대신 가짜(fakes) 선호하는 것이 의미하는 바"
description: ""
coverImage: "/assets/img/2024-09-10-WhatPreferringFakesOverMocksTrulyMeans_0.png"
date: 2024-09-10 19:16
ogImage: 
  url: /assets/img/2024-09-10-WhatPreferringFakesOverMocksTrulyMeans_0.png
tag: Tech
originalTitle: "What Preferring Fakes Over Mocks Truly Means"
link: "https://medium.com/@callmeryan/what-preferring-fakes-over-mocks-truly-means-b00f8b27cd32"
isUpdated: false
---


동료들과 함께한 테스팅에 관한 공유 세션을 참여한 후에 동일한 주제에 대해 두 번째로 논의한 것입니다. 해당 세션에서는 Martin Flower의 스텁, 페이크 및 목의 정의를 인용하고, 이에 대한 진정한 준수를 보여주지 않는 내용을 시연했습니다.

여러 해 동안 안드로이드 개발 커뮤니티에서는 유닛 테스트를 작성할 때 "모의 대신 페이크를 선호한다"는 것을 말하는 추세가 높아졌습니다. 이 추세는 많은 개발자들이 Mockito나 MockK와 같은 목 라이브러리를 수동으로 작성된 클래스로 대체하여 스텁, 페이크 및 목의 기본 개념을 정말로 이해하지 않은 채 혼란을 초래했습니다.

목 라이브러리에서 수동 클래스로 전환하는 것이 개선된 것처럼 보일 수 있지만, 이 두 가지 방법 모두 테스트 더블(페이크 및 목과 같은 것)이 실제로 무엇을 의미하는지 및 어떻게 사용해야 하는지에 대한 오해를 낳을 수 있음을 인식하는 것이 중요합니다. 진정한 논쟁이 벌어지는 것은 테스트 디자인, 행위 검증과 상태 검증의 역할, 그리고 언제 적절하게 페이크, 목 또는 스텁을 사용해야 하는지에 대한 깊은 논쟁입니다.

<div class="content-ad"></div>

# 마틴 파울러의 정의들

이 혼란을 해소하기 위해 마틴 파울러의 유명한 기사 "Mocks Aren't Stubs"에서 정의를 시작해 보겠습니다. 이 구분은 가짜(fakes), 목(mocks), 스텁(stubs)이 어떻게 우리의 테스트 전략에 맞는지 이해하는 데 중요합니다.

## 스텁(Stub)

스텁은 테스트 중에 메서드 호출에 대해 사전 정의된 정적 응답을 제공합니다. 그 목적은 실제 동작 없이 하드코딩된 값만 반환하는 종속성을 가벼운 대체물로 제공하는 것입니다.

<div class="content-ad"></div>

## 가짜

가짜는 시스템이나 구성 요소의 작동 구현이지만 테스트에 적합하도록 간소화된 것입니다. 가짜는 일반적으로 실제 세계의 동작을 흉내내지만 복잡한 논리의 오버헤드를 피하기 위해 단축했습니다.

## 목(Mock)

목은 사용되는 방식에 대한 기대를 미리 프로그래밍한 객체입니다. 스텁처럼 메서드 호출에 응답하는 것 외에도 목은 테스트 중에 특정 상호작용이 발생했는지 확인합니다. 목은 특정 메서드가 기대된 매개변수로 호출되었는지 확인합니다.

<div class="content-ad"></div>

## 주요 구분 사항

- Stubs: 의존성으로부터 사전 정의된 응답이 필요하지만 SUT가 그들과 상호작용하는 방식을 검증할 필요가 없을 때 사용합니다.
- Fakes: 실제 세계 동작을 모방하는 경량의 작동 컴포넌트 구현이 필요할 때 사용합니다. 테스트에서 복잡한 의존성을 대체하는 데 유용합니다.
- Stubs 및 Fakes는 상태 검증에 중점을 둡니다: 메서드 호출 이후 시스템의 상태를 확인합니다.
- Mocks는 동작 검증에 중점을 둡니다: 올바른 메서드가 호출되었는지 확인하며, 테스트 중 예상하는 상호작용과 순서를 명시적으로 지정합니다.

# 실제로 뭘 하는 건지 모를 때가 있다구요

이러한 정의들이 있음에도 혼란이 자주 발생합니다. 흔한 테스트 시나리오들을 탐구하고, 스텁, 페이크, 목의 사용과 관련이 어떤지 살펴보겠습니다.

<div class="content-ad"></div>

## 하드코딩된 사전 정의된 응답 — 무엇을 사용하고 있나요?

많은 단위 테스트에서 개발자들은 SUT를 격리시키기 위해 모든 외부 함수 호출에 대해 하드코딩된 사전 정의된 응답을 제공합니다. 예를 들어:

```js
val userService = mockk<UserService>()
// Stub 동작: 사전 정의된 응답 반환
every { userService.getUser("user123") } returns User("user123", "John")

// 참고: 실제 상황에서는 SUT 내에서 userService가 호출될 것을 기대합니다
// 우리는 보통 아래와 같이 단위 테스트를 수행하지는 않습니다
val user = userService.getUser("user123")

assertEquals("John", user.name)
```

이 경우 우리는 스텁을 사용하고 있습니다. 우리는 SUT가 userService와 상호 작용하는 방법을 확인하는 것보다는 응답을 제어하는 데 관심이 있기 때문입니다. 초점은 상태 확인에 있습니다. 호출 이후의 최종 상태를 확인합니다.

<div class="content-ad"></div>

## 동적 응답 - 여전히 스텁입니까?

하지만 미리 정의된 응답이 동적일 수 있다면 어떨까요? 예를 들어, SUT가 set() 함수를 호출할 때 값이 들어오고, SUT가 get()을 호출할 때 그 값이 반환되는 경우를 생각해 봅시다.

```js
class FakeCache : Cache {
    private var storedValue: String? = null

    override fun set(value: String) {
        storedValue = value
    }

    override fun get(): String? {
        return storedValue
    }
}

// 테스트
val cache = FakeCache()
cache.set("testValue")

// 참고: 실제로는 cache.get()이 SUT 내부에 있을 것으로 예상됩니다
// 일반적으로 아래와 같이 단위 테스트를 수행하지 않습니다
val value = cache.get()

assertEquals("testValue", value)
```

이 경우 더 이상 스텁을 사용하지 않습니다. 이것은 가짜(fake)입니다. 왜냐하면 객체의 동작이 동적이기 때문입니다. 이는 실제값을 저장하고 검색함으로써 현실 세계의 동작을 흉내내는데, 이는 정적 스텁보다 실제 구성요소에 더 가깝습니다.

<div class="content-ad"></div>

# 코틀린 안드로이드에서 사용하는 Mocking 라이브러리 — 목 이상의 가치

MockK와 Mockito와 같은 라이브러리를 종종 "모킹 라이브러리"라고 부르지만, 이것은 그들이 모의뿐만 아니라 더 많은 작업을 할 수 있기 때문에 오해일 수 있습니다. 이러한 라이브러리는 일반적으로 스텁과 모의를 생성할 수 있습니다.

## 스텁, 페이크 및 목을 생성하기 위해 Mocking 라이브러리 사용하기

여기에서는 MockK를 사용하여 다양한 유형의 테스트 더블을 생성하는 방법을 소개하겠습니다:

<div class="content-ad"></div>

🧪 MockK을 사용한 스텁: 이 경우에는 상호작용을 확인하지 않고 메소드 호출에 미리 정의된 응답을 제공합니다.

```js
val userService = mockk<UserService>()
// 스텁 동작: 정적으로 미리 정의된 응답을 반환
every { userService.getUser("user123") } returns User("user123", "John")

// 참고: 실제로는 SUT 내에서 userService가 호출되기를 기대합니다
// 보통 아래처럼 단위 테스트를 하지 않습니다
val user = userService.getUser("user123")

assertEquals("John", user.name)
```

여기서는 MockK를 사용하여 getUser() 메소드를 스텁한 것으로, 미리 정의된 User 객체를 반환했습니다. 메소드가 특정 방식으로 호출되었는지를 확인하는 것에는 관심이 없습니다.

🧪 MockK를 사용한 페이크: MockK는 람다 기반 응답 및 부수효과를 통해 외부 변수나 메소드 호출에 의해 반환 값을 실행 시간에 결정할 수 있어 페이크와 유사한 동적 동작을 시뮬레이션할 수 있습니다.

<div class="content-ad"></div>

그러나 이것은 완전한 기능적 구현을 갖고 있는 것이 아니기 때문에 진정한 가짜(fake)로 보기 어렵습니다. 대신, 동적이고 상태를 가지는 행동을 시뮬레이션할 수 있어 종종 가짜와 같이 느껴질 수 있습니다.

```js
var storedValue: String? = null
val cache = mockk<Cache>()
every { cache.set(any()) } answers { storedValue = arg<String>(0) }
every { cache.get() } answers { storedValue }

cache.set("testValue")

// 참고: 실제로는 cache.get()이 시스템 테스트 대상(SUT) 내에 위치한다고 예상됩니다.
// 보통 아래와 같이 단위 테스트를 수행하지 않습니다.
val value = cache.get()

assertEquals("testValue", value)
```

위의 방식은 동적 행동을 시뮬레이션하고 가짜가 할 것처럼 흉내를 내지만, 여전히 MockK 프레임워크가 이를 관리하며, 상세한 내부 로직을 갖춘 진정한 가짜와는 다릅니다.

- 이 문맥에서의 Mock는 사전 프로그램된 동작에 의존합니다(동적이지만). 반면 가짜는 일반적으로 수동으로 작성된 실제 클래스의 단순화된 작동 구현입니다.
- 진정한 가짜는 의존성의 실제 로직을 구현하지만, MockK는 전체 가짜 클래스를 구현하지 않고 동적 행동이 필요할 때 유용한 중간단계 역할을 할 수 있습니다.

<div class="content-ad"></div>

🧪 Mocking with MockK: 모크를 사용하여 특정 상호작용을 확인합니다.

```kotlin
val userService = mockk<UserService>()
every { userService.addUser(any()) } just Runs

// 참고: 실제 환경에서는 userService.addUser가 SUT 내에 위치함을 예상합니다
// 일반적으로 아래와 같이 단위 테스트를 수행하지는 않습니다
userService.addUser(User("user123", "John"))

verify(exactly = 1) { userService.addUser(any()) }
```

이 경우, 모크는 미리 정의된 동작을 제공하고 예상한 상호작용이 발생했는지 확인합니다.

## 모킹 라이브러리는 모크 외에도 더 많은 작업을 수행합니다

<div class="content-ad"></div>


모킹 라이브러리는 스텁, 가짜 객체 및 목 객체를 생성할 수 있어서 사용한다고 해서 반드시 목 객체와만 작업해야 한다는 뜻은 아닙니다. 이러한 라이브러리 없이도 전체 범위의 테스트 더블을 효과적으로 만들 수 있습니다. 사람들이 "모킹보다는 가짜 객체를 선호한다"고 할 때, 종종 모든 것을 모킹 라이브러리에 지나치게 의존하는 대신 수동으로 생성하는 가짜 객체를 옹호하는 경우가 많습니다. 그럼에도 불구하고 그들은 자신이 하는 구별을 깨닫지 못할 수도 있습니다.

# 목 객체를 사용하지 않고 테스트 더블(스텁, 가짜 객체, 목 객체)를 수동으로 작성할 수 있습니다

스텁이나 목 객체를 만들기 위해 목 객체 라이브러리가 필요하지 않습니다. 우리는 일반적인 코틀린 클래스로 직접 구현할 수 있습니다.

## 수동 스텁


<div class="content-ad"></div>

정적으로 미리 정의된 응답을 제공하는 스텁입니다.

```kotlin
class StubUserService : UserService {
    override fun getUser(userId: String): User {
        return User("user123", "John") // 미리 정의된 응답
    }

    override fun addUser(user: User) {
        // 실제 구현 없음
    }
}

// 테스트
val stubUserService = StubUserService()

// 참고: 실제로는 getUser()가 SUT 내에 있는 것을 기대합니다.
// 보통 아래와 같이 유닛 테스트를 수행하지 않습니다.
val user = stubUserService.getUser("user123")

assertEquals("John", user.name)
```

## 수동 가짜

실제 클래스처럼 동작하지만 단순화된 가짜 클래스입니다. 위에서 언급했듯이 일반적으로 우리는 목 라이브러리 대신 수동으로 가짜를 만들어야 합니다. 목 라이브러리를 사용하여 스텁 또는 목업을 생성했든 말든, 목 대신 가짜를 선호하는 것을 실현하는 것이 더 가능합니다.

<div class="content-ad"></div>

```kotlin
class FakeUserService : UserService {
    private val users = mutableMapOf<String, User>()
    
    override fun getUser(userId: String): User? {
        return users[userId]
    }

    override fun addUser(user: User) {
        users[user.id] = user
    }
}

// 참고: 실제로는 FakeUserService가 SUT에서 호출될 것으로 예상됩니다.
// 보통 아래와 같이 단위 테스트를 수행하지는 않습니다.
val fakeUserService = FakeUserService()
fakeUserService.addUser(User("user123", "John"))
val user = fakeUserService.getUser("user123")
assertEquals("John", user?.name)
```

## 수동 목(mock)

메서드 호출을 수동으로 확인하는 목(mock)입니다. 목(mock)을 생성하기 위해 모킹 라이브러리를 사용하지 않을 수도 있습니다. 다시 말해, 모킹 라이브러리를 수동 목(mock)에 선호하는 것은 아닙니다.

```kotlin
class MockUserService : UserService {
    var addUserCalled = false

    override fun getUser(userId: String): User? {
        return null // 테스트를 위해 이를 구현할 필요 없음
    }

    override fun addUser(user: User) {
        addUserCalled = true
    }

    fun verifyAddUserCalled() {
        assert(addUserCalled) { "addUser()가 호출되지 않았습니다!" }
    }
}

// 참고: 실제로는 userService가 SUT 내에서 호출될 것으로 예상됩니다.
// 보통 아래와 같이 단위 테스트를 수행하지는 않습니다.
val mockUserService = MockUserService()
mockUserService.addUser(User("user123", "John"))
mockUserService.verifyAddUserCalled()
``` 


<div class="content-ad"></div>

# "Prefer Fakes Over Mocks"의 진짜 의미

사람들이 "모의체보다 가짜를 선호한다"고 말할 때, 우리가 Martin Fowler가 제시한 정확한 정의를 따른다면, 이들은 단순히 모의 라이브러리를 대체하는 것뿐만 아니라 서로 다른 테스트 목표에 대해 언급하는 것이 분명합니다. 이 선호도의 진짜 의미는 테스트가 무엇을 확인하고 있는지에 관한 것입니다.

- 가짜는 비즈니스 로직과 현실 세계의 동작을 테스트할 때 선호됩니다. 가짜는 외부 종속성을 대체하는 간단하지만 현실적인 대안을 제공하고 상태 확인에 집중합니다.
- 모의는 SUT와 협력자 간의 특정 상호작용을 확인할 때 사용됩니다. 모의는 동작 확인에 중점을 둡니다.

"모의체보다 가짜를 선호한다"는 단순하고 현실적이며 취약성이 적은 테스트를 원한다는 욕구를 강조합니다. 가짜를 사용하는 테스트는 종종 현실 세계 시나리오에 가깝고 유지보수가 더 용이한 테스트를 제공합니다. 동시에, 모의는 세세한 수준에서의 상호작용 확인에 집중하여 취약성을 도입할 가능성이 있습니다.

<div class="content-ad"></div>

# 유니트 테스트에서 진짜 모의를 하는 것이 좋지 않은 이유

MockK 또는 Mockito와 같은 모의 라이브러리를 사용하거나 모의 클래스를 수동으로 구현하더라도, 모의 뒤에 있는 핵심 의도는 시스템 언더 테스트 (SUT)와 해당 의존성 간의 상호 작용을 확인하는 것입니다.

다시 말해, 모의는 특정 기능이 테스트 중에 특정 매개변수와 함께 호출되었음을 확인합니다. 그러나 이 접근 방식은 유니트 테스팅에서 중대한 문제를 일으킬 수 있습니다. 왜냐하면 이것은 우리가 SUT의 내부 작동 방식을 테스트하고 있는 것이 아니라, SUT가 무엇을 출력하거나 최종 동작이 무엇인지를 테스트하고 있음을 의미하기 때문입니다.

## 모의와 유단연 테스트의 취약성

<div class="content-ad"></div>

모의 객체에 의존하는 문제는 테스트를 SUT(시스템 언더 테스트)의 내부 메커니즘에 강하게 결합시킨다는 점입니다. 모의 객체를 사용하여 특정 메서드 호출(또는 해당 매개변수)을 확인할 때, 내부 구현이 변경되면 테스트가 실패할 수 있습니다. 이는 시스템의 외부 동작이 올바른 채로 유지되어도 테스트가 실패할 수 있다는 것을 의미합니다. 예를 들어, 우리가 효율성이나 가독성을 향상시키기 위해 메서드를 리팩터링하더라도 외부 기능은 같이 유지한다면 테스트가 실패할 수 있습니다. 왜냐하면 모의 객체가 동일한 상호작용 순서를 예상하기 때문입니다.

MockK를 사용한 Kotlin 예제를 살펴보겠습니다:

```kotlin
val userService = mockk<UserService>()
every { userService.addUser(any()) } just Runs

// 참고: 여기서는 실제로 userService가 SUT 내부에서 호출될 것으로 예상합니다.
// 일반적으로 아래와 같이 단위 테스트를 수행하지는 않습니다.
userService.addUser(User("user123", "John"))

verify(exactly = 1) { userService.addUser(User("user123", "John")) }
```

이런 경우 SUT의 내부 구현이 변경되면(예: addUser() 이전에 조건을 추가하거나 다른 메서드를 호출) 테스트가 실패할 수 있습니다. 비즈니스 로직과 최종 출력이 변경되지 않은 상태에서도 실패합니다. 이는 모의 객체가 구현을 확인하고 결과가 아닌 것을 보여줍니다.

<div class="content-ad"></div>

## 내부 메커니즘을 검증하는 데 문제가 있습니다

모의 객체는 코드가 어떻게 작동하는지(메서드 호출, 인수 및 상호 작용)에 초점을 맞추도록 장려합니다. 코드가 생성하는 결과물(예상 결과)에 중점을 두는 대신, 이는 여러 가지 문제를 일으킬 수 있습니다.

- 테스트가 불안정해집니다: 테스트가 구현 세부 사항과 강하게 결합되어 있어 내부 변경 사항 — 심지어 기능에 영향을 미치지 않는 경우도 — 테스트 실패의 원인이 될 수 있습니다. 리팩터링, 최적화 또는 메서드 구조 변경은 테스트를 부당하게 깨뜨릴 수 있습니다.
- 리팩터링이 어려워집니다: 모의는 특정 내부 상호 작용을 검증하므로 리팩터링이 위험하고 번거로워집니다. 코드의 내부 구조가 변경될 때마다 개발자는 테스트를 다시 작성해야 하며, 이는 개발 프로세스를 크게 늦출 수 있습니다.
- 행동에 집중하세요. 상태가 아니라: 단위 테스트는 SUT의 출력 및 최종 상태에 집중해야 하며, 외부에서 정상적으로 작동하는지 확인해야 합니다. 내부 호출을 검증하면이 중요한 내용을 놓치고 테스트가 실제 버그를 찾는 데 도움이 되지 않게 됩니다.

## 단위 테스트에서 내부 상호 작용을 검증해야 할까요?

<div class="content-ad"></div>

우리는 스스로에게 질문해야 합니다: SUT에서 특정 함수를 특정 매개변수 호출로 검증하는 것이 얼마나 중요한가요? 메소드가 특정 횟수로 호출되었는지 중요한가요, 아니면 시스템이 올바른 출력을 만드는지가 더 중요한가요?

예를 들어, 결제 시스템에서 무엇이 더 중요한가요:

- 결제 서비스에서 charge() 메소드가 정확히 한 번 호출되었는지 확인하는 것?
- 아니면 사용자의 계정이 성공적으로 차감되고 영수증이 생성되었는지 확인하는 것?

전자가 일부 경우 유용할 수 있지만(부수 효과 확인이나 중복 작업 방지와 같은 경우), 결과를 확인하는 것(예: 사용자 잔액이 올바르게 차감되었는지)이 내부 구현보다 신뢰할 수 있다는 것을 종종 확인할 수 있습니다.

<div class="content-ad"></div>

# 내 의견: 결과에 집중하고 내부 세부 사항에 집중하지 말기

단위 테스트에서 내부 세부 사항보다는 상태 및 출력 확인에 중점을 두어야 합니다. 여기에 설명한 이유는 다음과 같습니다:

- 유지 보수성: 함수의 최종 출력(상태 확인)에 집중함으로써 시스템의 외부 동작이 올바르다면 테스트가 리팩터링에 강건해집니다. 내부 로직이 변경되더라도 시스템의 외부 동작이 올바르다면 테스트는 계속 통과합니다.
- 더 나은 디자인: 단위 테스트는 시스템이 외부적으로 작동하는지 확인하는 데 도움을 줍니다. 내부 상호작용이 아닌 결과와 동작에 집중하는 것은 클래스나 함수의 공개 API를 테스트하는 데 더 잘 부합합니다.
- 불필요한 명세 피하기: Mock 기반의 테스트는 종종 불필요한 명세로 이어질 수 있습니다. 시스템의 정확성과 관련이 없는 것들을 테스트하는 경우가 있습니다. 예를 들어, 맞는 최종 결과가 생성되는 한 메소드가 몇 번 호출되는지는 중요할까요?

## 목 모킹 사용 시기(및 장소)

<div class="content-ad"></div>

위에 언급한 것처럼, 상호 작용을 검증하는 것이 중요한 구체적인 시나리오가 있습니다. 예를 들어 다음과 같은 경우에:

- 부수 효과가 중요한 경우: 예를 들어, 이메일이 전송되었거나 결제가 처리되었는지를 확인한다면, 목업은 시스템이 서비스에서 올바른 메서드를 호출했는지 확인하는 데 도움이 될 수 있습니다.
- 중복 방지: 목업을 사용하면 사용자에게 요금을 청구하는 것처럼 비용이 많이 드는 또는 중요한 작업이 한 번 이상 호출되지 않도록 보장할 수 있습니다.

그러나 이러한 시나리오는 순수한 단위 테스트에서는 비교적 드물습니다. 상호 작용을 검증하는 데는 여러 시스템 또는 구성 요소 간의 상호 작용이 더 중요한 통합 또는 종단 간 테스트가 더 적합한 경우가 많습니다.

# 결론

<div class="content-ad"></div>

목의 진정한 의도는 내부 상호작용을 검증하는 것이지만, 이는 단위 테스트에 취약성과 복잡성을 도입할 수 있습니다. 어떤 것이 어떻게 작동하는지 테스트하는 대신에 무엇을 생성하는지 테스트할 때, 테스트가 부서지기 쉽고 유지보수가 어려워질 위험이 있습니다.

대부분의 단위 테스트에서는 SUT의 최종 상태나 결과에 초점을 맞춘 스텁(stubs)과 페이크(fakes)를 사용하는 것이 더 효과적입니다. 특정 상호작용 자체가 시스템의 정확성에 중요할 때에만 목을 삼가야 합니다.

결국, "목 대신 페이크를 선호하라"는 것은 가장 중요한 부분에 집중하라는 요청입니다: 시스템의 외부 동작, 내부 작용이 아닌. 목 라이브러리를 사용하느냐 안 하느냐가 이 경우 주요 관심사는 아닙니다.

세 번째로 이 주제에 대해 쓸 필요가 없기를 바랍니다. 😅