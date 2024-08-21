---
title: "swift 데이터 추상화 하는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-SwiftDataIntroduction_0.png"
date: 2024-08-21 18:24
ogImage: 
  url: /assets/img/2024-08-21-SwiftDataIntroduction_0.png
tag: Tech
originalTitle: "Swift Data  Introduction"
link: "https://medium.com/@ashishyadav_16301/swift-data-introduction-996deb05422d"
isUpdated: true
updatedAt: 1724245303338
---


<img src="/assets/img/2024-08-21-SwiftDataIntroduction_0.png" />

애플은 2023년 SwiftData를 iOS 개발의 일환으로 소개했습니다. 이를 통해 선언적 코딩을 통해 데이터 지속성을 제공하며, UIKit이 데이터 저장을 위해 Core Data를 사용하는 것과 유사하게, SwiftData는 애플리케이션의 관리 및 저장 프로세스를 간소화합니다.

- SwiftData는 자체적으로 CoreData 기초 위에 구축된 프레임워크입니다.
- 모델, 관계 및 마이그레이션을 정의하기 위한 선언적 방법을 제공합니다.
- Swift Concurrency 기능을 제공하여 데이터 작업을 비동기적으로 수행할 수 있습니다.
- 여러 기기로 클라우드와 데이터를 동기화하는 클라우드 동기화 기능을 제공합니다.

SwiftData의 주요 부분은 다음과 같습니다:

<div class="content-ad"></div>

- 모델
- 컨테이너
- 컨텍스트

# 1. 모델

모델은 저장소에 저장해야 하는 데이터입니다.

```js
@모델
최종 클래스 할 일 항목 {
    @속성(.고유) 렛 title: 문자열
    렛 timeStaamp: 문자열
    렛 isCompleted: 부울
    렛 isCritical: 부울
    
    이닛(title: 문자열, timeStaamp: 문자열, isCompleted: 부울, isCritical: 부울) {
        이.self.title = title
        이.self.timeStaamp = timeStaamp
        이.self.isCompleted = isCompleted
        이.self.isCritical = isCritical
    }
}
```

<div class="content-ad"></div>

여기서 @Model은 ToDoItem Model이 SwiftData model임을 나타내는 Macro입니다. 또한, DB에서 고유하게 만들기 위해 title에 unique 속성을 추가해주세요.

# 2. 컨테이너

장치에 데이터를 저장할 때 컨테이너라고 불리는 저장 공간이 필요합니다.

모델 컨테이너는 앱의 WindowGroup이 호출될 때 초기화됩니다.

<div class="content-ad"></div>

```swift
struct SwiftData_ExampleApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
                .modelContainer(for: ToDoItem.self)
        }
    }
}
```

# 3. Context

그 역할은 생성, 삭제 및 수정된 모든 객체를 추적하여 모든 모델이 나중에 컨테이너에 저장될 수 있도록 합니다.

```swift
struct ContentView: View {
    
    @Environment(\.modelContext) var context
    
    var body: some View {
        VStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundStyle(.tint)
            Text("Hello, world!")
        }
        .padding()
    }
}
```

<div class="content-ad"></div>

@Environment property wrapper는 SwiftUI의 미리 정의된 키들과 함께 작동합니다.

다음 SwiftData의 부분이 곧 발표될 예정이에요!

조급해하지 마세요.