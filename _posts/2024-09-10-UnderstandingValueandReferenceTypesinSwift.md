---
title: "스위프트에서 값 타입과 참조 타입 이해하기"
description: ""
coverImage: "/assets/img/2024-09-10-UnderstandingValueandReferenceTypesinSwift_0.png"
date: 2024-09-10 19:37
ogImage: 
  url: /assets/img/2024-09-10-UnderstandingValueandReferenceTypesinSwift_0.png
tag: Tech
originalTitle: "Understanding Value and Reference Types in Swift"
link: "https://medium.com/iosplaybook/understanding-value-and-reference-types-in-swift-c4de28340097"
isUpdated: false
---


알아두어야 할 점

![Image](/assets/img/2024-09-10-UnderstandingValueandReferenceTypesinSwift_0.png)

Swift에서 가장 중요한 개념 중 하나는 값 타입과 참조 타입의 차이를 이해하는 것입니다. 각각의 타입은 고유한 동작을 갖고 있으며, 이 차이를 이해하는 것은 효율적이고 예측 가능한 코드를 작성하는 데 중요합니다.

이 글에서는 값 타입과 참조 타입 간의 세세한 차이를 살펴보겠습니다. 이를 통해 더 명확한 결정을 내릴 수 있습니다.

<div class="content-ad"></div>

# 데이터 공유 방법

값이나 참조를 통해 전달할 때, 본질적으로 데이터를 공유하고 있습니다. 그럼 데이터를 어떻게 공유하는지가 중요한 문제가 됩니다.

예를 들어, 이 글의 헤더 이미지를 작업하는 중에 디자인에 대한 피드백을 받고 싶다면, 쉽게 이미지를 친구와 공유하여 의견을 얻을 수 있습니다.

여기서 헤더 이미지를 공유하는 두 가지 방법이 있습니다.

<div class="content-ad"></div>

- 친구에게 JPG 이미지를 이메일로 보내주세요.
- 친구에게 Canva 템플릿 링크를 보내주세요.

옵션 1의 경우, 친구는 자기 자신의 사본을 가지고 있고, 이메일을 삭제해도 원본을 여전히 가지고 있을 수 있어요.

옵션 2의 경우, 친구가 실수로 이미지를 삭제하면 영구적으로 사라질 수 있어요.

이것이 값 타입과 참조 타입의 기본 컨셉입니다. 값 타입은 헤더 이미지의 사본이며, 참조 타입은 헤더 이미지의 위치를 가리키는 Canva URL 링크입니다.

<div class="content-ad"></div>

# 값 타입

Swift의 값 타입은 구조체, 열거형 및 튜플입니다. 함수에 값을 전달하거나 변수나 상수에 할당할 때, Swift는 해당 값의 복사본을 만듭니다.

```js
struct Content {
    var title: String
}

var myContent = Content(title: "Understanding Value and Reference Types in Swift")

var myContentCopy = myContent

myContentCopy.title = "Lessons in Value and Reference Types"

print(myContent.title) // Understanding Value and Reference Types in Swift

print(myContentCopy.title) // Lessons in Value and Reference Types
```

여기서 Content라는 구조체가 생성되었으며, title이라는 하나의 속성이 있습니다.

<div class="content-ad"></div>

내용은 내용의 제목으로 초기화되고 변수 myContent에 할당됩니다.

myContent를 변수 myContentCopy에 할당하는 것은 원래 인스턴스를 새 인스턴스로 복사합니다.

두 인스턴스가 다르기 때문에 myContentCopy의 텍스트를 변경해도 myContent가 변경되지 않습니다.

이것은 내 친구에게 내 내용을 보내는 것과 유사합니다. 그녀는 자신의 사본에 대해 원하는 대로 할 수 있고 원본은 변경되지 않습니다.

<div class="content-ad"></div>

하지만 참조 유형은 다르다.

# 참조 유형

```swift
class Content {
    var title: String
    
    init(title: String) {
        self.title = title
    }
}

var myContent = Content(title: "Swift에서 값 타입과 참조 타입 이해하기")

var myContentCopy = myContent

myContentCopy.title = "Swift에서 값 타입과 참조 타입의 수업"

print(myContent.title) // Swift에서 값 타입과 참조 타입의 수업

print(myContentCopy.title) // Swift에서 값 타입과 참조 타입의 수업
```

이 코드는 구조체와 비슷하지만 클래스로 선언되었으며 init 메서드가 있습니다.

<div class="content-ad"></div>

내용 복사본(myContentCopy)의 제목을 변경하면 원래 인스턴스도 변경됩니다.

참조 유형의 경우, myContentCopy를 myContent에 할당하면 할당되는 것은 인스턴스에 대한 참조입니다. 복사본이 생성되지 않으며, 대신 myContentCopy와 myContent가 동일한 공간 메모리를 가리키게 됩니다; 이전의 URL 예제와 같습니다.

두 인스턴스가 같은 것을 참조하기 때문에 myContentCopy의 제목을 변경하면 이를 공유하는 모든 것이 업데이트되며, 이 경우에는 myContent가 해당됩니다.

Swift에서 값 유형과 참조 유형의 차이를 이해하면 견고하고 효율적이며 안전한 코드를 작성하는 데 기본적입니다. 필요에 따라 적절한 유형을 선택함으로써 응용 프로그램이 의도대로 동작하고 예측 가능하며 관리 가능한 데이터 상호 작용을 보장할 수 있습니다.