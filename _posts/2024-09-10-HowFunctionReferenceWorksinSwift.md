---
title: "Swift에서 함수 참조 작동 방식 함수 포인터와 클로저 explained"
description: ""
coverImage: "/assets/img/2024-09-10-HowFunctionReferenceWorksinSwift_0.png"
date: 2024-09-10 19:38
ogImage: 
  url: /assets/img/2024-09-10-HowFunctionReferenceWorksinSwift_0.png
tag: Tech
originalTitle: "How Function Reference Works in Swift"
link: "https://medium.com/iosplaybook/how-function-reference-works-in-swift-4158ceebb882"
isUpdated: false
---


모호한 함수 참조 해결 방법

![HowFunctionReferenceWorksinSwift](/assets/img/2024-09-10-HowFunctionReferenceWorksinSwift_0.png)

본 문서는 Swift에서 함수 시그니처가 어떻게 작동하는지 이해하고 있다고 가정합니다. 그렇지 않은 경우에는 계속하기 전에 Swift에서 함수 시그니처와 오버로딩에 대한 제 게시물을 먼저 읽어보시기를 권장합니다.

함수 참조는 괄호 앞부분인 함수의 이름으로 형성됩니다. 빠진 괄호가 함수 참조를 함수 호출과 구분짓는 요소입니다.

<div class="content-ad"></div>

Swift는 함수를 참조하는 데 모호함이 없으면 기능 참조를 허용합니다.

그러나 불확실한 상황에서는 Swift가 특정 함수를 참조하기 위해 함수의 전체 이름 또는 서명을 사용할 수 있도록 선택권을 제공합니다.

여기에 작동 방식이 나와 있습니다

# 문제

<div class="content-ad"></div>

```javascript
class MessageUser {
    
    func message(_ guestUser:String) {
        print("Welcome, \(guestUser)!")
    }
    
    func message() {
        print("Nice to see you again!")
    }
    
    func sendMessage() {
        var messenger = message // error: Ambiguous use of 'message'
    }
}
```

MessageUser 클래스에는 인사말을 출력하는 message 라는 두 함수와 sendMessage라는 함수가 있습니다.

sendMessage 내부에서 messenger 변수에 message를 할당하려고 시도합니다.

이 할당은 'message'의 모호함 사용으로 인해 오류를 발생시킵니다.

<div class="content-ad"></div>

Swift은 할당된 함수가 어떤 함수를 참조하는지 모르는 것 같아요.

우리가 원하는 함수를 정확하게 참조하려면 함수의 전체 이름 또는 서명을 사용해야 해요.

## 전체 이름 솔루션:

- 함수의 전체 이름은 괄호를 포함한 이름으로 구성되며 각 이름은 콜론으로 구분된 외부 매개변수의 이름을 포함해야 합니다.
- 쉼표와 공백은 사용할 수 없어요.
- 억제된 외부 매개변수 이름은 밑줄로 표시돼요.

<div class="content-ad"></div>

## 서명 솔루션:

- 함수의 이름(또는 전체 이름) 뒤에 키워드로 as가 오고 그 다음에 함수의 서명이 옵니다.

## 함수 전체 이름 및 서명 예시

```js
func helloWorld(_ greeting: String, country: String) {
    print("\(greeting), from \(country)")
}
```

<div class="content-ad"></div>

`helloWorld()` 함수는 두 개의 String 타입 매개변수를 가져와 아무것도 반환하지 않습니다.

함수 몸체에서는 greeting과 country 매개변수를 로컬 변수로 사용하여 보간된 문자열을 출력합니다.

```js
// helloWorld 전체 이름:
helloWorld(_:country:)
```

이것이 helloWorld()의 전체 이름입니다. suppressed external parameter name은 밑줄로 표시되고 매개변수 이름은 콜론으로 구분됩니다. 공백이나 쉼표가 없습니다.

<div class="content-ad"></div>

```js
// helloWorld 시그니처:
helloWorld(_:country:) as (String, String) -> ()

helloWorld as (String, String) -> ()
```

이것은 시그니처별로 함수를 참조할 수 있는 두 가지 방법입니다.

첫 번째 옵션은 전체 이름을 가지고 있고, 두 번째 옵션은 이름만 가지고 있습니다.

두 가지 옵션 모두 함수 뒤에 as 키워드와 함수 시그니처가 따릅니다.

<div class="content-ad"></div>

우리 코드를 고치는 두 가지 옵션을 살펴보죠.

# 전체 이름과 시그니처 솔루션

```js
class MessageUser {
    
    func message(_ guestUser:String) {
        print("Welcome, \(guestUser)!")
    }
    
    func message() {
        print("Nice to see you again!")
    }
    
    
    func sendMessage() {
        var messenger = message(_:)
    }
}
```

여기서 특정한 String 타입의 매개변수를 받는 함수를 참조하기 위해 fullName인 message(_:)을 사용했습니다.

<div class="content-ad"></div>

하지만 우리는 매개변수가 없는 메시지 기능을 참조할 때 전체 이름을 사용할 수 없습니다. 왜냐하면 전체 이름은 이름 그 자체이기 때문에 이로 인해 우리는 처음 문제로 다시 돌아가게 됩니다.

이 문제를 해결하기 위해 함수 시그니처 솔루션을 사용해야 합니다.

```js
class MessageUser {
    
    func message(_ guestUser:String) {
        print("환영합니다, \(guestUser)님!")
    }
    
    func message() {
        print("다시 뵙게 되어 반가워요!")
    }
    
    func sendMessage() {
        var messenger = message as () -> ()
    }
}
```

이제 함수 시그니처를 지정하여 매개변수가 없는 메시지 기능을 참조하고 있습니다.

<div class="content-ad"></div>

물론, 이름을 신중하게 선택해야 합니다. 하지만 함수에 관한 참조가 모호한 상황에 대처할 수 있는 해결책을 갖게 되셨군요.

이 기사가 마음에 드셨다면 구독하여 새로운 글을 지금 바로 이메일로 받아보세요!