---
title: "스위프트에서 함수 오버로드 시 함수 참조 동작 방식 알아보기"
description: ""
coverImage: "/assets/img/2024-09-10-HowFunctionReferenceWorksWhenaFunctionisOverloadedinSwift_0.png"
date: 2024-09-10 19:38
ogImage: 
  url: /assets/img/2024-09-10-HowFunctionReferenceWorksWhenaFunctionisOverloadedinSwift_0.png
tag: Tech
originalTitle: "How Function Reference Works When a Function is Overloaded in Swift"
link: "https://medium.com/iosplaybook/how-function-reference-works-when-a-function-is-overloaded-in-swift-c6fca8ee9430"
isUpdated: false
---


힌트: 함수의 유형을 변경합니다.

![Change the table tag to Markdown format](/assets/img/2024-09-10-HowFunctionReferenceWorksWhenaFunctionisOverloadedinSwift_0.png)

이 문서는 Swift에서 함수 참조, 함수 시그니처 및 오버로딩이 작동하는 방법을 이해한다고 가정합니다. 아직 모르신다면, 다음 내용을 읽는 것을 권장합니다.

Swift에서 함수 시그니처 및 오버로딩

<div class="content-ad"></div>

스위프트에서 함수 참조 작동 방식

이전 글에서 모호한 함수 참조를 해결하는 두 가지 해결책을 제시했습니다. 이번 글은 그것의 연장선이지만, 여기서는 함수가 오버로드된 상황에서 함수 참조 문제를 다루고 있습니다.

오버로드된 함수에 접근하려고 할 때, 전체 이름 솔루션은 함수의 전체 이름이 동일하기 때문에 오류가 발생합니다. 이러한 경우 함수 시그니처 솔루션을 구현해야 합니다.

# 오버로드된 함수 참조하기

<div class="content-ad"></div>

```swift
class Greeting{
    
    func greet() {
    }
    
    func greet(_ hello: String) {
        print("\(hello), Adam")
    }
    
    func greet(_ greeted: Bool) {
        if greeted {
            print("Welcome back!")
        }
    }
    
    func sendGreeting() {
        let userGreeting = greet(_:) as (Bool) -> ()
    }
}
```

여기서 'Greeting' 클래스에는 greet라는 세 가지 함수가 있으며 두 개가 오버로드되었습니다.

함수 full-name greet(_:)을 사용하면 모호성 오류가 발생합니다. Swift는 greet(_:)라는 동일한 이름을 공유하는 두 함수 중 어떤 함수를 참조해야 하는지 알 수 없습니다.


<div class="content-ad"></div>

이 오류를 해결하려면 함수의 전체 이름을 함수의 서명으로 교체하십시오.

만약 이 글이 마음에 드셨다면, 구독해주시고 제가 발행할 때 바로 이메일을 받아보세요!