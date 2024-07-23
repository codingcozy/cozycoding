---
title: "깔끔한 코드 작성 오류 처리 방법"
description: ""
coverImage: "/assets/img/2024-07-13-CleanCodeErrorHandling_0.png"
date: 2024-07-13 02:35
ogImage: 
  url: /assets/img/2024-07-13-CleanCodeErrorHandling_0.png
tag: Tech
originalTitle: "Clean Code: Error Handling"
link: "https://medium.com/gitconnected/clean-code-error-handling-629b0fd8ff99"
---


![2024-07-13-CleanCodeErrorHandling_0.png](/assets/img/2024-07-13-CleanCodeErrorHandling_0.png)

안녕하세요! 깨끗한 코드를 추구하는 여정 일곱 번째 장으로 오신 것을 환영합니다! 오늘은 오류 처리의 중요한 단계를 분석해 보겠습니다. 입력이 비정상적이거나 예기치 않은 경우가 있기 때문에, 오류를 효과적으로 처리하는 것은 코드베이스가 견고하게 유지되도록 하는 데 중요합니다.

시작하기 전에, 이 시리즈를 아직 따라오고 있지 않다면, 지금까지의 '깨끗한 코드' 장들을 확인해보세요. 이러한 기사들은 프로그래밍 기술을 향상시키고자 하는 모든 사람들에게 필수적인 독서입니다.

오류 처리는 소개가 필요 없을 정도로 중요하다는 것을 우리 모두 알고 있습니다. 그러나 주요 논리를 가리면 올바르게 처리된 것이 아닙니다. 오류 처리를 스타일과 명확함으로 처리하는 몇 가지 팁과 트릭에 대해 살펴봅시다. 이를 통해 코드가 견고하고 가독성 있게 유지되도록 합니다.

<div class="content-ad"></div>

## Return Codes 대신 예외를 사용하세요

에러 리턴 코드 대신 예외를 사용하면 코드의 명확성과 신뢰성을 높일 수 있습니다. -1 또는 0과 같은 숫자를 반환하여 에러를 신호로 보내면 호출자가 에러를 제대로 확인하는 것을 잊을 수 있는 문제가 발생할 수 있습니다. 함수 내에서 예외로 오류를 처리함으로써 호출자에게 명확한 로직을 제시할 수 있습니다.

간단한 예시:

```js
# 문제가 되는 예제

def divide(a, b):
    if b == 0:
        return -1  
    else:
        return a / b  

result = divide(10, 0)
if result == -1:
    print("에러: 0으로 나눌 수 없음")
else:
    print("결과:", result)
```

<div class="content-ad"></div>

이 예에서는 호출자가 오류를 감지하기 위해 반환 값을 확인해야 합니다. 이 간단한 경우에는 하나의 오류만 있는데, 여러 종류의 오류가 추가되면 혼란스럽고 관리하기 어려워질 것입니다!

```js
# 예외 처리가 개선된 예시

def divide(a, b):
    if b == 0:
        raise ValueError("0으로 나눌 수 없습니다.")
    else:
        return a / b  # 나눗셈 결과 반환


try:
    result = divide(10, 0)
    print("결과:", result)
except ValueError as e:
    print("오류 발생:", e)
```

이제 divide 함수는 0으로 나누기가 발생할 때 ValueError 예외를 발생시킵니다. 이렇게 하면 오류 처리가 명확해지고 호출자가 예외를 처리하도록 강제되어 가독성이 향상되고 놓친 오류 확인으로 인한 오류 가능성이 줄어듭니다.

이 방식으로 예외를 사용하면 코드가 보다 명확하고 오류 가능성이 낮아지며 코드베이스 전반에 걸쳐 오류 처리의 최상의 사례를 따릅니다.

<div class="content-ad"></div>

## null 반환하지 말기

오류 반환 코드를 피하는 것과 마찬가지로 결과가 없을 때 null (또는 Python의 None)을 반환하는 것은 호출자가 null 값 여부를 확인하는 것을 잊어 NullPointerException 또는 유사한 문제를 일으킬 수 있는 문제로 이어질 수 있습니다. null을 반환하는 대신 예외를 발생시키거나 결과의 부재를 명시적으로 나타내는 특수한 객체를 반환해야 합니다.

몇 가지 예제를 살펴보겠습니다:

```js
# 문제가 있는 예시

def find_item(items, target):
    for item in items:
        if item == target:
            return item
    return None

result = find_item([1, 2, 3], 4)
if result is None:
    print("아이템을 찾을 수 없습니다.")
else:
    print("찾은 아이템:", result)
```

<div class="content-ad"></div>

이 예제에서 호출자는 결과가 None인지 확인하여 대상 항목이 없음을 감지해야 합니다. 호출자가 이 확인을 수행하는 것을 잊으면 이 접근 방식은 오류가 발생할 수 있습니다.

```js
# 예외를 사용한 개선된 예제

def find_item(items, target):
    for item in items:
        if item == target:
            return item
    raise ValueError("항목을 찾을 수 없습니다")

try:
    result = find_item([1, 2, 3], 4)
    print("항목을 찾았습니다:", result)
except ValueError as e:
    print("오류 발생:", e)
```

이 버전에서 find_item 함수는 항목을 찾을 수 없을 때 ValueError를 발생시켜 오류 처리를 명시적으로 만들고 호출자가 예외를 처리하도록 합니다.

```js
# 특수 객체를 사용한 개선된 예제

class NotFound:
    def __repr__(self):
        return "<NotFound>"

NOT_FOUND = NotFound()

def find_item(items, target):
    for item in items:
        if item == target:
            return item
    return NOT_FOUND

result = find_item([1, 2, 3], 4)
if result is NOT_FOUND:
    print("항목을 찾을 수 없습니다")
else:
    print("항목을 찾았습니다:", result)
```

<div class="content-ad"></div>

In this version, the `find_item` function returns a special `NotFound` object when the item is not found. This approach makes it explicit when an item is not found and avoids the ambiguity of `None`.

Using exceptions or specialized objects instead of returning null not only improves code clarity and robustness but also ensures that error handling is explicit and less prone to being overlooked.

## Don’t pass null

Returning null is bad, but passing null is even worse! Passing null as an argument to a function often leads to `NullPointerException` errors, causing your application to crash unexpectedly. This approach can create hidden bugs that are hard to trace and debug. Instead of passing null you should either raise an exception or use assertions to ensure that your function receives valid arguments.

<div class="content-ad"></div>

예시:

```js
# 문제가 있는 예시

def process_data(data):
    if data is None:
        print("Error: data is None")
        return
    print("Processing data:", data)
```

이 예시에서는 process_data 함수에 None을 전달하면 함수 내에서 None 경우를 처리하는 추가적인 확인이 필요하며, 이는 코드를 혼란스럽게 하고 주요 로직을 가리게 할 수 있습니다.

```js
# 예외 처리를 추가한 향상된 예시

def process_data(data):
    if data is None:
        raise ValueError("잘못된 인수: data는 None일 수 없습니다.")
    print("Processing data:", data)

try:
    process_data(None)
except ValueError as e:
    print("에러:", e)
```


<div class="content-ad"></div>

이 버전에서는 data가 None 인 경우 process_data 함수가 ValueError를 발생시키므로 오류 처리가 명시적이며 호출자가 유효한 인수를 제공해야 합니다.

```python
# 개선된 예시로 단언문 사용

def process_data(data):
    assert data is not None, "Invalid argument: data cannot be None"
    print("데이터 처리 중:", data)

process_data(None)  
```

단언문을 사용하면 함수에 전달된 인수가 유효한지 강제할 수 있습니다. 이 접근 방식은 None이 허용되지 않음을 명확히하고 개발 중에 일찍 에러를 잡는 데 도움이 됩니다.

null 전달을 피하고 예외 또는 단언문을 사용함으로써 코드를 더 견고하고 에러를 줄이며 유지보수하기 쉬운 코드를 보장할 수 있습니다. 이러한 실천은 예기치 못한 충돌을 방지하고 오류 처리를 명확하게 만들어 애플리케이션의 신뢰성을 향상시킵니다.

<div class="content-ad"></div>

## 먼저 try-catch-finally 문 작성하기

코드를 작성할 때 먼저 try-catch-finally 문을 작성하는 것이 좋은 습관입니다. 이 구조는 try 블록 내에서 발생할 수 있는 예기치 못한 이벤트에 대비할 준비가 되어 있다는 것을 보장합니다. 예외를 명시적으로 처리함으로써, 잠재적인 오류를 관리할 준비가 되어 있다는 것을 코드에서 명확하게 표시할 수 있습니다.

try-catch 블록으로 시작하는 것은 또한 코드에서 기대하는 결과를 정의하도록 장려하며, 의도한 동작을 명확하게 설명할 수 있습니다. 또한, 일부러 예외를 유발하는 테스트를 포함시킴으로써, 오류 처리 접근 방식을 유효성 검사하여 다양한 시나리오에서 견고하고 신뢰할 수 있게 할 수 있습니다.

## 예외와 함께 맥락 제공하기

<div class="content-ad"></div>

모든 예외 상황은 오류의 원인을 정확히 파악할 수 있는 충분한 문맥을 제공해야 합니다. 정보를 제공하는 오류 메시지를 작성하고 의미 있는 이름을 사용하는 것이 중요합니다. 이는 전체의 절반이나 마찬가지입니다! 더 자세한 통찰을 얻으려면 이 주제에 대한 내 장을 참조해주세요.

## 호출자 요구에 따라 예외 정의하기

예외 처리를 설계할 때, 사용자 함수나 메서드(호출자)의 관점을 고려하는 것이 중요합니다. 목표는 무엇이 잘못되었는지에 대한 의미 있는 정보를 제공하면서 오류 처리 코드에서 불필요한 중복이나 모호함을 피하는 것입니다.

예외는 원본이나 유형(예: 프로그래밍 오류, 네트워크 문제)에 따라 정의하고 분류할 수 있습니다. 그러나 주요 관심사는 이러한 예외를 호출자가 어떻게 포착하고 처리하는지입니다. 모든 잠재적 문제에 대해 예외를 작성하는 대신, 실제로 필요한 예외를 정의하는 데 집중해야 합니다. 이 접근 방식은 특히 외부 제3자 API를 다룰 때 유용합니다. 이러한 경우에는 모든 예외를 포착할 필요가 없습니다. 애플리케이션에 필요한 예외만 포착하면 됩니다. 이렇게 함으로써, 각 예외를 잘 구조화되고 객체지향적인 방식으로 관리하는 래퍼를 구축할 수 있습니다.

<div class="content-ad"></div>

## 결론

이러한 목표들이 처음에는 상반된 것처럼 보일 수 있지만, 올바르게 다가가면 보완적입니다. 효과적인 오류 처리는 코드 명확성과 신뢰성을 유지하는 데 중요합니다. 오류 처리를 주요 논리와 분리된 관심사로 취급함으로써 예상치 못한 문제에 강력한 코드베이스를 작성할 수 있습니다. 예외를 사용하고 return 코드나 널 값 대신하는 명확하고 명시적인 오류 처리 기법을 강조하며 널 인수 전달을 피함으로써 코드가 신뢰성 있고 유지보수가 가능하도록 합니다. 요약하자면, 강력한 오류 처리를 우선시함으로써 깔끔한 코드 작성 여정에서 가독성과 견고성이라는 이중 목표를 달성하는 데 관건이 됩니다. 🚀

다음 장

“다음은 무엇일까요? 더 많은 통찰을 기대해 주세요!” 🔍

<div class="content-ad"></div>

이전 장

## 프로그래밍 서적 제휴 코너 📚💻🤝

나의 소유도 하고 있는 서적들로, 모든 개발자에게 강력히 추천하는 책들!

- Clean Code
- The Algorithm Design Manual
- Refactoring: Improving the Design of Existing Code