---
title: "swift에서 커스텀 연산자 사용하기"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 19:29
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Understanding Custom Operators in Swift."
link: "https://medium.com/@harshaag99/understanding-custom-operators-in-swift-e4ce3f1f9c39"
isUpdated: true
updatedAt: 1724742488153
---


이전에 자신만의 수학 기호를 발명하여 코드를 더 읽기 쉽게 만들었으면 좋겠다고 생각해 본 적이 있나요? 요리를 할 때 전통적인 칼 대신에 자신이 가장 좋아하는 채소를 잘라내기 위해 완벽하게 형성된 도구를 만든다고 상상해 보세요. Swift에서의 사용자 정의 연산자는 그 특별한 도구처럼 — 코드에 적합한 작업을 위해 자신만의 기호를 정의할 수 있게 해줍니다.

# 연산자란 무엇인가요?

간단히 말해서, 연산자는 컴파일러에게 특정 작업을 수행하라고 알려주는 기호입니다. 이전에 +가 더하기를, -가 빼기를 나타내는 연산자를 보았을 것입니다. Swift에는 많은 내장 연산자가 함께 제공되지만, 사용자가 직접 만들 수도 있습니다!

# 왜 사용자 정의 연산자를 만들게 되나요?

<div class="content-ad"></div>

가끔 표준 연산자로는 원하는 작업을 완전히 표현할 수 없는 경우가 있습니다. 계산기 앱을 개발 중이라고 상상해보세요. 두 숫자의 평균을 빠르게 구하는 특별한 기호가 필요할 때가 있습니다. 함수를 작성할 수도 있지만, 그 외에도 특별한 연산자가 있으면 좋지 않을까요? 그래서 사용자 정의 연산자가 필요한 것입니다.

# 사용자 정의 연산자 생성

간단한 예제로 시작해보죠: 두 숫자의 평균을 구하는 연산자를 만들어봅시다. 이를 위해 `<>` 기호를 사용할 것입니다.

```js
infix operator <>: AdditionPrecedence

func <>(left: Int, right: Int) -> Int {
    return (left + right) / 2
}
```

<div class="content-ad"></div>

# 쪼개어서 설명하기

- 중위 연산자: 중위 연산자는 두 값 사이에 배치됩니다 (예: 5 + 3에서의 +). 중위 키워드는 Swift에게 새로운 연산자가 두 값 사이에 위치해야 한다고 알려줍니다.
- 연산자 심볼: ``를 우리의 심볼로 선택했습니다. 거의 모든 비알파벳 문자 조합을 사용할 수 있습니다.
- 우선순위 그룹: AdditionPrecedence는 우리의 연산자가 표현식이 어떻게 평가되는지에 대한 규칙에서 + 연산자를 따를 것을 의미합니다.
- 함수: 함수는 우리의 연산자가 무엇을 하는지 정의합니다. 여기서는 두 숫자를 더하고 그 후에 2로 나눠서 평균값을 구합니다.

# 현실 세계 비유

사용자 정의 연산자는 키보드의 사용자 정의 바로 가기와 같습니다. 동작을 수행하기 위해 여러 키를 누르는 대신, 정확히 원하는 동작을 수행하는 고유한 키 조합을 만들어 시간과 노력을 절약할 수 있습니다.

<div class="content-ad"></div>

# 오픈 예외 및 실패 시나리오

- 과용: 커스텀 연산자는 강력하지만 너무 많이 사용하면 코드를 읽기 어렵게 만들 수 있습니다. 너무 많은 커스텀 도구를 주방에 만드는 것과 같습니다 - 결국 혼란스럽고 혼란스럽게 됩니다.
- 모호성: 두 커스텀 연산자가 유사한 기호나 우선 순위를 가지면 혼란과 버그로 이어질 수 있습니다. 예를 들어 평균을 나타내는 ``와 다른 연산을 나타내는 ``와 같은 경우, 어떤 것이 무엇을 하는지 기억하기 어려울 수 있습니다.
- 호환성: 커스텀 연산자는 코드베이스에 고유하므로 설명 없이 코드를 읽는 다른 사람은 이해하기 어려울 수 있습니다. 다른 사람도 이해할 수 없는 새로운 단어를 발명하는 것과 같습니다 - 사전 없이는 의사소통이 중단됩니다.
- 실패 예시: 커스텀 연산자를 생성하되 우선 순위 그룹을 지정하지 않거나 내장 연산자와 충돌하는 그룹을 선택하는 것을 잊어버린다고 상상해보세요. 이로 인해 복잡한 표현식에서 예기치 않은 결과가 발생할 수 있습니다. 예를 들어, 다음은 오류가 발생하거나 잘못된 출력을 생성할 수 있습니다:

```js
infix operator <> // 우선순위 그룹 없음
func <>(left: Int, right: Int) -> Int {
    return (left + right) / 2
}
```

우선 순위를 정의하지 않으면 Swift는 이 연산자를 사용하여 표현식을 평가하는 방법을 모를 수 있으며, 프로그램이 실패하거나 예측할 수 없게 작동할 수 있습니다.

<div class="content-ad"></div>

행복한 코딩!