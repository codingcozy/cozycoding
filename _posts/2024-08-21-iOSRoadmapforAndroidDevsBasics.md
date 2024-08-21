---
title: "안드로이드 개발자를 위한 iOS 로드맵 기초부터"
description: ""
coverImage: "/assets/img/2024-08-21-iOSRoadmapforAndroidDevsBasics_0.png"
date: 2024-08-21 18:25
ogImage: 
  url: /assets/img/2024-08-21-iOSRoadmapforAndroidDevsBasics_0.png
tag: Tech
originalTitle: "iOS Roadmap for Android Devs Basics"
link: "https://medium.com/@rohitky/ios-roadmap-for-android-devs-basics-b7bc385e3bec"
isUpdated: false
---


![이미지](/assets/img/2024-08-21-iOSRoadmapforAndroidDevsBasics_0.png)

코틀린 멀티 플랫폼 모바일(KMM)이 인기를 얻으면서, 안드로이드와 iOS 모두에 코딩할 수 있는 개발자에 대한 수요가 증가하고 있습니다. 이 안내서에서는 이 두 플랫폼에 대한 개발하는 방법의 유사점과 차이점을 다룰 것입니다. 각 주제에 대한 개요를 제공하고, 더 자세한 부분은 이어서 다룰 예정입니다.

시스템별 디자인

안드로이드 앱은 파티션 방식을 사용하여 개발됩니다. 앱을 단편과 활동으로 나눠서 구성합니다. 활동은 앱 내 단일 화면을 나타내며, 여러 화면으로 구성된 프로젝트는 다수의 활동을 관리해야 합니다. 각 활동은 프래그먼트를 포함하고 있으며, 이는 사용자 인터페이스의 부분으로 다른 활동 간을 이동하거나 값 입력, 새로운 화면을 열 때 사용됩니다.

<div class="content-ad"></div>

iOS 애플리케이션 아키텍처는 뷰 컨트롤러를 기반으로 합니다. 페이지뷰, 탭, 스플릿 뷰 컨트롤러와 같은 다양한 유형의 뷰 컨트롤러가 앱 개발에서 사용됩니다. 뷰 컨트롤러는 전체 화면 또는 일부를 관리할 수 있습니다.

## OOPS

Kotlin과 Swift는 클래스, 객체, 상속, 다형성, 캡슐화, 추상화를 포함한 OOP 패러다임을 지원하는 현대적이고 정적 타입의 프로그래밍 언어입니다.

Kotlin

<div class="content-ad"></div>

```js
// 클래스 및 객체
class Person(val name: String)
val person = Person("John")

// 상속
open class Animal
class Dog : Animal()

// 다형성
open class Animal {
    open fun makeSound() {
        println("Animal sound")
    }
}
class Dog : Animal() {
    override fun makeSound() {
        println("Bark")
    }
}
```

Swift

```js
// 클래스 및 객체
class Person {
    var name: String
    init(name: String) {
        self.name = name
    }
}
let person = Person(name: "John")

// 상속
class Animal {}
class Dog: Animal {}

// 다형성
class Animal {
    func makeSound() {
        print("Animal sound")
    }
}
class Dog: Animal {
    override func makeSound() {
        print("Bark")
    }
}
```

## 접근 제어자


<div class="content-ad"></div>

<img src="/assets/img/2024-08-21-iOSRoadmapforAndroidDevsBasics_1.png" />

참고사항:

- Swift에는 Kotlin의 protected 수정자와 직접적인 등가 요소가 없습니다.
- Kotlin에는 Swift의 file-private에 직접적인 등가 요소가 없지만, 유사한 동작을 위해 단일 파일 모듈 내에서 internal 레벨이 사용됩니다.

## 널 안전성

<div class="content-ad"></div>

```kotlin
// 코틀린
var name: String? = null // 널 가능형

// 스위프트
var name: String? = nil // 옵셔널 타입
```

- 코틀린은 널 포인터 예외를 방지하기 위해 내장된 널 안전성을 제공하며, ?를 사용하여 명시적으로 널 가능성을 표시해야 합니다.
- 스위프트는 옵셔널을 사용하여 널 가능성을 처리하며, ?와 !를 사용하여 옵셔널과 암시적으로 해제된 옵셔널 타입을 표시합니다.

## 확장:

```kotlin
// 코틀린
fun String.addExclamation() = this + "!"

// 스위프트
extension String {
    func addExclamation() -> String {
        return self + "!"
    }
}
```

<div class="content-ad"></div>

두 언어 모두 기존 클래스에 기능을 추가하기 위한 확장을 지원합니다. 문법은 유사하지만 다른 키워드를 사용합니다.

## Views

```js
// SWIFTUI
struct ContentView: View {
    @State private var count = 0

    var body: some View {
        VStack {
            Text("Count: \(count)")
            Button("Increment") {
                count += 1
            }
        }
    }
}

// Jetpack Compose
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Column {
        Text("Count: $count")
        Button(onClick = { count += 1 }) {
            Text("Increment")
        }
    }
}
```

SwiftUI와 Jetpack Compose는 UI를 설명하기 위한 선언적 문법을 사용하여 상태를 반영합니다. 상태를 관리하고 상태가 변경될 때 UI를 업데이트하는 메커니즘을 제공합니다.

<div class="content-ad"></div>

## 상태 관리

상태 관리는 반응성이 있고 상호작용이 가능한 UI를 구축하는 데 중요한 측면입니다. Jetpack Compose(안드로이드)와 SwiftUI(iOS) 모두 각각의 프레임워크 내에서 상태를 관리하는 메커니즘을 제공합니다. 아래에서는 두 플랫폼의 상태 관리 접근 방식을 비교 및 대조할 것입니다.

![image](/assets/img/2024-08-21-iOSRoadmapforAndroidDevsBasics_2.png)

안드로이드에서 ViewModel은 화면 회전과 같은 구성 변경을 통해 유지됩니다. 그러나 iOS에서 SwiftUI를 사용할 때는 안드로이드의 ViewModel에 직접적인 동등물이 없습니다. 구성 변경을 통해 상태를 유지하기 위해 @StateObject, @ObservedObject, @EnvironmentObject을 사용할 수 있습니다.

<div class="content-ad"></div>

## 내비게이션

iOS 내비게이션: 코디네이터 패턴은 멀티모듈 iOS 프로젝트에서 내비게이션에 일반적으로 사용되며, 코디네이터에 내비게이션 책임을 위임함으로써 관심사를 깔끔하게 분리합니다.

```js
// 피처 A 코디네이터
class FeatureACoordinator: ObservableObject, Coordinator {
    @Published var navigationPath = NavigationPath()
    func start() -> some View {
        NavigationStack(path: $navigationPath) {
            FeatureAView()
                .environmentObject(self)
        }
    }
  
    func navigateToFeatureB() {
        navigationPath.append(FeatureBView())
    }
}
```

- 뷰에서 내비게이션 설정: SwiftUI 뷰를 사용하고 코디네이터와 바인딩하세요.

<div class="content-ad"></div>

```swift
struct MainView: View {
    @EnvironmentObject var coordinator: MainCoordinator

    var body: some View {
        VStack {
            Text("Main View")
            Button("Go to Feature A") {
                coordinator.navigateToFeatureA()
            }
        }
        .navigationTitle("Main")
    }
}
```

iOS에서는 멀티 모듈 프로젝트를 위해 Coordinator Pattern과 함께 Flow Stacks를 사용할 수도 있습니다. 이는 더 나은 조직화와 관심사 분리를 제공하는 고급 네비게이션 아키텍처입니다.

안드로이드에서는 Android Navigation Component를 사용하여 일반적으로 네비게이션이 처리됩니다. 이 컴포넌트는 앱에서 모든 네비게이션 상호작용을 처리하는 프레임워크를 제공하며, 프래그먼트 트랜잭션, 딥 링킹 등을 포함합니다.

```js
// nav_graph.xml에 네비게이션 설정
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    app:startDestination="@id/mainFragment">

    <fragment
        android:id="@+id/mainFragment"
        android:name="com.example.MainFragment"
        android:label="Main"
        tools:layout="@layout/fragment_main">
        <action
            android:id="@+id/action_mainFragment_to_featureAFragment"
            app:destination="@id/featureAFragment" />
    </fragment>

    <fragment
        android:id="@+id/featureAFragment"
        android:name="com.example.FeatureAFragment"
        android:label="Feature A"
        tools:layout="@layout/fragment_feature_a" />
</navigation>

// MainFragment.kt
class MainFragment : Fragment(R.layout.fragment_main) {
    private val navController by lazy { findNavController() }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        view.findViewById<Button>(R.id.buttonNavigateToFeatureA).setOnClickListener {
            navController.navigate(R.id.action_mainFragment_to_featureAFragment)
        }
    }
}

// FeatureAFragment.kt
class FeatureAFragment : Fragment(R.layout.fragment_feature_a)
```

<div class="content-ad"></div>

안녕하세요! 위의 내용을 번역해보겠습니다.

안드로이드 실행 유형:

- 표준: 새 인스턴스가 백 스택에 추가됩니다.
- SingleTop: 맨 위에 있는 인스턴스를 재사용합니다.
- SingleTask: 기존 인스턴스를 앞으로 가져와 다른 인스턴스를 지웁니다.
- SingleInstance: 액티비티가 자체 작업에서 독립적으로 실행됩니다.

이러한 실행 유형은 백 스택 동작을 관리하고 복잡한 애플리케이션에서 적절한 탐색 흐름을 보장하는 데 도움이 됩니다.

## 다중 모듈

<div class="content-ad"></div>

- 안드로이드는 Gradle을 사용하여 멀티모듈 프로젝트를 관리합니다.

```js
// 프로젝트 구조
MyApplication/
├── app/
├── network/
├── featureA/
├── featureB/
└── settings.gradle

// settings.gradle
include ':app', ':network', ':featureA', ':featureB'

// app/build.gradle
dependencies {
    implementation project(':network')
    implementation project(':featureA')
}
```

- iOS는 프로젝트 관리를 위해 Xcode를 사용하며 의존성으로 Swift Package Manager(SPM)을 활용할 수 있습니다.

```js
// 프로젝트 구조
MyApplication/
├── MyApp/
├── NetworkFramework/
├── FeatureAFramework/
└── Package.swift

// Package.swift
import PackageDescription

let package = Package(
    name: "MyApp",
    dependencies: [
        .package(path: "./NetworkFramework"),
        .package(path: "./FeatureAFramework")
    ],
    targets: [
        .target(
            name: "MyApp",
            dependencies: ["NetworkFramework", "FeatureAFramework"]),
    ]
)
```

<div class="content-ad"></div>

- CocoaPods와 Carthage은 SPM 외에도 iOS에서 인기있는 의존성 관리자입니다.

```js
// Podfile
target 'MyApp' do
  pod 'Alamofire', '~> 5.4'
end
```

## 테스트 유형

Android와 iOS 모두 앱 신뢰성과 성능을 보장하기 위해 다양한 테스트 방법을 제공합니다. 아래는 각 플랫폼에 사용되는 주요 테스트 유형을 비교한 것입니다.

<div class="content-ad"></div>

아래는 iOS 개발로의 여정을 시작하는 간단한 개요입니다. 후속 파트에서는 각 주제를 자세히 다룰 것입니다.

감사합니다 🚀