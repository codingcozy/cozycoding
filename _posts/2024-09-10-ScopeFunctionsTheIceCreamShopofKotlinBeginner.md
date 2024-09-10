---
title: "코틀린의 아이스크림 가게 초보자를 위한 스코프 함수"
description: ""
coverImage: "/assets/img/2024-09-10-ScopeFunctionsTheIceCreamShopofKotlinBeginner_0.png"
date: 2024-09-10 19:18
ogImage: 
  url: /assets/img/2024-09-10-ScopeFunctionsTheIceCreamShopofKotlinBeginner_0.png
tag: Tech
originalTitle: "Scope Functions The Ice Cream Shop of Kotlin Beginner"
link: "https://medium.com/@harmanpreet.khera/scope-functions-the-ice-cream-shop-of-kotlin-f8d24bcd9be1"
isUpdated: false
---


Kotlin에서 올바른 scope 함수를 선택하는 것은 완벽한 아이스크림 맛을 고르는 것 같은 느낌일 수 있어요. 다양한 옵션이 있지만 모두 맛있어요! 핵심은 당신의 욕망(또는 사용 사례)과 적절한 scope 함수를 매치하는 것이에요. 여기 재미있는 안내서가 있어요. let, run, with, apply, 그리고 also로 달콤한 let, run, with, apply, 그리고 also 세계를 탐험하며 코드를 선물같이 만들어 보세요!

![이미지](/assets/img/2024-09-10-ScopeFunctionsTheIceCreamShopofKotlinBeginner_0.png)

# 1. let

let은 민트 초콜릿 칩 아이스크림 같은 느낌이에요 — 상쾌하고 시원해요! 호출 체인의 결과에 대해 작업을 수행할 수 있게 해주어 널 가능 객체를 다룰 때 완벽해요.

<div class="content-ad"></div>

사용법:

```js
val result = someNullableValue?.let {
    // 'it'에서 작업 수행
    it.toUpperCase()
}
```

# 2. run

run은 록키로드 아이스크림처럼 가득한 재료들로 가득 차 있어요! 객체의 세계로 뛰어들어 구성 및 계산을 수행한 다음 결과를 반환합니다. 풍부하고 맛있는 코딩 경험을 원할 때 완벽한 방법이에요!

<div class="content-ad"></div>

사용법:

```js
val result = someObject.run {
    // 'this'에서 동작 수행
    this.someMethod()
    "결과"
}
```

### 3. with

with은 클래식 바닐라 스쿱처럼 간단하면서 다재다능합니다. 객체의 세계로 뛰어들어 설정할 수 있지만, 확장 함수가 아닙니다. 간단하고 꾸밈이 없는 설정을 원할 때 완벽합니다!

<div class="content-ad"></div>

사용법:

```js
val result = with(someObject) {
    // 'this'에 대해 작업 수행
    this.someMethod()
    "결과"
}
```

## 4. apply

apply는 쿠키 반죽이랑 같아요 — 모든 재료를 섞어서 같은 맛있는 반죽을 얻어요! 객체를 구성하고 그대로 반환하기에 완벽한데, 주로 객체 초기화에 사용돼요.

<div class="content-ad"></div>

사용 예시:

```js
val result = someObject.apply {
    // 'this'에서 작업 수행
    this.someProperty = "value"
}
```

## 5. also

`also`는 당신이 좋아하는 아이스크림 위에 뿌려지는 스프링클과 같습니다. 약간의 재미를 더해줍니다! 객체에 추가적인 작업을 수행하고 동일한 객체를 반환합니다. 따라서 즐거운 부작용을 위한 완벽한 도구입니다.

<div class="content-ad"></div>

사용법:

```js
val result = someObject.also {
    // 'it'을 사용하여 작업 수행
    it.someMethod()
}
```

# 코딩 선데이에 스코프 함수가 뿌려지는 이유가 여기 있습니다! 😋

- 객체 구성: 객체의 이름을 반복하지 않고 객체를 구성할 수 있습니다.

<div class="content-ad"></div>

```js
val person = Person().apply {
    name = "John"
    age = 30
}
```

- Nullable Handling: They provide a way to handle nullable objects more gracefully.

```js
val nameLength = person?.let {
    it.name.length
}
```

- Code Readability: They make the code more readable by reducing boilerplate code.
- Chaining Operations: They enable chaining multiple operations on an object.

<div class="content-ad"></div>

```js
val result = listOf(1, 2, 3).map { it * 2 }.filter { it > 2 }.also {
    println("필터링된 목록: $it")
}
```

- 로컬 컨텍스트: 변수들에 대한 일시적인 범위를 제공하여 변수 이름 충돌 위험을 줄입니다.

```js
val result = with(person) {
    "이름: $name, 나이: $age"
}
```

# 요약

<div class="content-ad"></div>

`let`: 널 가능한 객체에 작업을 수행하는 데 사용합니다.  
`run`: 객체 구성 및 계산에 사용합니다.  
`with`: 객체 구성에 사용합니다.  
`apply`: 객체 초기화에 사용합니다.  
`also`: 부작용에 사용합니다.  

각 스코프 함수는 고유한 아이스크림 맛과 같아요 — 적절한 함수를 선택하면 코드가 마치 선물 같고 부드러워집니다!  

새로운 글을 게시할 때 알림을 받고 싶다면 여기에 구독하세요.