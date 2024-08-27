---
title: "Core Data와 함께 데이터 모델링하는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-100DaysofiOSDevelopmentJourneyDay033_0.png"
date: 2024-08-26 19:30
ogImage: 
  url: /assets/img/2024-08-26-100DaysofiOSDevelopmentJourneyDay033_0.png
tag: Tech
originalTitle: "100 Days of iOS Development Journey  Day 033"
link: "https://medium.com/@ryccoatika/100-days-of-ios-development-journey-day-033-1c4ccdc4c30b"
isUpdated: true
updatedAt: 1724744167935
---


<img src="/assets/img/2024-08-26-100DaysofiOSDevelopmentJourneyDay033_0.png" />

제33일째, 우리는 간단한 뉴스 리더 앱을 만들 예정인 새 프로젝트에 착수합니다. 이 앱은 사용자가 원격 서버에서 가져온 최신 뉴스 기사를 둘러볼 수 있도록 해주며, 깔끔하고 직관적인 인터페이스에서 이를 표시합니다. 이 프로젝트에서는 앱의 구조를 조직하는 데 초점을 맞추며, 탐색을 위해 UITabBarController를 구현하고, Swift의 Codable 프로토콜을 사용하여 JSON 데이터를 구문 분석하는 것에 중점을 둘 것입니다.

이 프로젝트는 외부 소스에서 데이터를 처리하고 여러 뷰로 앱을 구성하는 데 좋은 입문 자료입니다. 끝날 때까지 기본적이지만 기능적인 앱을 만들어 더 고급 프로젝트의 기초를 마련하게 될 것입니다.

## 목차

<div class="content-ad"></div>

- 설정
- 기본 UI 생성: UITabBarController
- Codable 프로토콜을 사용한 JSON 구문 분석
- 요약
- 참고 자료

## 설정

XCode에서 새 iOS 프로젝트를 생성하고 "Project7"이라고 이름을 지정합니다.

## 기본 UI 생성: UITabBarController

<div class="content-ad"></div>

- ViewController 상속 업데이트: ViewController.swift 파일을 열고 클래스를 UIViewController 대신 UITableViewController에서 상속받도록 수정하세요.
- Storyboard 수정:
- Main.storyboard를 열고 기존 뷰 컨트롤러를 UITableViewController로 교체하세요.
- 이 UITableViewController를 초기 뷰 컨트롤러로 설정하세요.
- 속성 검사기에서 프로토타입 셀 식별자를 "Cell"로 설정하고 액세서리를 "Disclosure Indicator"로 변경하고 스타일을 "Subtitle"로 조정하세요.
- Navigation 및 Tab Bar 컨트롤러에 삽입:
- UITableViewController를 UINavigationController에 삽입하세요.
- 그런 다음 UINavigationController을 UITabBarController에 삽입하세요.
- 네비게이션 컨트롤러 씬 내의 "탭 바 항목"을 선택하고 시스템 항목을 "Custom"에서 "Most Recent"로 변경하세요.
- Storyboard ID 설정: 네비게이션 컨트롤러 씬에서 Identity Storyboard ID를 "NavController"로 설정하세요.
- 테이블 뷰 준비: ViewController.swift 파일을 열고 피드를 저장할 속성을 추가하세요:

```js
private var petitions = [String]()
```

- 테이블의 행 수를 정의하세요:

```js
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return petitions.count
}
```

<div class="content-ad"></div>

- 각 테이블 행에 대한 셀 뷰를 설정하십시오:

```js
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
    cell.textLabel?.text = "여기에 제목 입력"
    cell.detailTextLabel?.text = "여기에 부제목 입력"
    return cell
}
```

- 앱 실행: 이 시점에서 앱을 실행하면 하단 탭 바를 제외하고 테이블 뷰가 비어 있습니다. 다음 단계에서 테이블을 채웁니다.

## Codable 프로토콜을 사용한 JSON 파싱

<div class="content-ad"></div>

- 청원 구조 정의: Petition.swift라는 새 Swift 파일을 생성하고 JSON 구조와 일치하도록 구조체를 정의하세요:

```swift
import Foundation

struct Petition: Codable {
    var title: String
    var body: String
    var signatureCount: Int
}
```

- JSON 응답을 위한 래퍼 생성: JSON 응답은 results 키 하위에 중첩되어 있으므로, 래퍼 구조체를 정의하기 위해 Petitions.swift라는 다른 Swift 파일을 생성하세요:

```swift
import Foundation

struct Petitions: Codable {
    var results: [Petition]
}
```

<div class="content-ad"></div>

- 청원 배열 업데이트: ViewController.swift 파일의 청원 배열을 문자열 대신 Petition 객체를 저장할 수 있도록 수정하십시오:

```js
private var petitions = [Petition]()
```

- JSON 데이터 가져오기: viewDidLoad 메서드에서 JSON 데이터 가져오기를 구현하십시오:

```js
override func viewDidLoad() {
    super.viewDidLoad()
    
    if let url = URL(string: "https://www.hackingwithswift.com/samples/petitions-1.json") {
        if let data = try? Data(contentsOf: url) {
            parse(json: data)
        }
    }
}
```

<div class="content-ad"></div>

- JSON 데이터 구문 분석: JSON 데이터를 Swift 객체로 디코딩하는 파싱 메서드를 만들어보세요:

```swift
private func parse(json: Data) {
    let decoder = JSONDecoder()
    if let petitionsJson = try? decoder.decode(Petitions.self, from: json) {
        petitions = petitionsJson.results
        tableView.reloadData()
    }
}
```

- cellForRowAt 메서드 수정: petitions 배열에서 실제 데이터를 표시하도록 cellForRowAt 메서드를 수정하세요:

```swift
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
    let petition = petitions[indexPath.row]
    cell.textLabel?.text = petition.title
    cell.detailTextLabel?.text = petition.body
    return cell
}
```

<div class="content-ad"></div>

- 앱을 실행하면 이제 파싱된 JSON 데이터가 테이블 뷰에 표시됩니다.

![image](/assets/img/2024-08-26-100DaysofiOSDevelopmentJourneyDay033_1.png)

## 요약

우리는 기능적이고 사용자 친화적인 앱 인터페이스를 만드는 기초적인 측면을 탐구했습니다. 먼저 UITabBarController를 사용하여 기본 UI를 설정하여 서로 다른 뷰 간에 쉽게 이동할 수 있도록했습니다. 그런 다음, Codable 프로토콜을 사용하여 JSON 데이터를 구문 분석하는 데 초점을 맞춰 우리 앱에서 구조화된 데이터를 처리하고 표시하는 프로세스를 간소화했습니다.

<div class="content-ad"></div>

하루를 마무리하며, 우리 앱에서 데이터를 성공적으로 가져와서 표시하는 데 성공했습니다. 이로써 향후 보다 복잡한 데이터 조작을 위한 무대를 마련하고 더 많은 기능 향상을 기대할 수 있게 되었습니다. 이 날은 Swift에서 JSON 구문 분석과 UI 설정에 대한 개념을 강화하는 데 중요했으며, 앞으로의 고급 주제에 대비할 수 있도록 준비했습니다.

## 참고 자료

https://www.hackingwithswift.com/100/33