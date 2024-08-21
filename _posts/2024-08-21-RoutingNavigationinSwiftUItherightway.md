---
title: "SwiftUI에서의 라우팅과 네비게이션 방법 정리"
description: ""
coverImage: "/assets/img/2024-08-21-RoutingNavigationinSwiftUItherightway_0.png"
date: 2024-08-21 18:27
ogImage: 
  url: /assets/img/2024-08-21-RoutingNavigationinSwiftUItherightway_0.png
tag: Tech
originalTitle: "Routing , Navigation in SwiftUI  the right way"
link: "https://medium.com/@blorenzop/routing-navigation-in-swiftui-f1f8ff818937"
isUpdated: true
updatedAt: 1724245395157
---


## 애플리케이션의 탐색을 처리하는 간단하고 가벼운 라우팅 레이어를 만드는 방법을 배워보세요.

우리 모두가 동의할 것은 탐색이 우리 애플리케이션의 가장 중요한 부분 중 하나라는 것입니다. 좋은 라우팅 시스템을 갖는 것은 앱을 확장하고 새로운 기능을 추가할 때 큰 차이를 만들 수 있습니다.

Navigation은 SwiftUI가 출시된 첫날부터 아킬레스건이었습니다. 뷰에 매우 결합된 라우팅 시스템은 복잡한 애플리케이션을 개발할 때 일을 더 어렵게 만들었습니다.

다행히도, iOS 16부터 UIKit에서처럼 뷰 스택을 유지하고 뷰를 쉽게 푸시하고 팝할 수 있는 새로운 탐색 API가 제공됩니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-08-21-RoutingNavigationinSwiftUItherightway_0.png)

# 새로운 네비게이션 API

우리 집에 있는 다양한 방을 보여주기 위한 간단한 애플리케이션을 만들어 봅시다. 우리가 표시하려는 방에 대한 정보를 저장할 모델이 필요합니다. 이 경우, 방의 이름과 해당 방의 이미지를 표시할 것입니다.

또한, 방의 정보를 보여주는 뷰가 필요합니다.

<div class="content-ad"></div>

저희 애플리케이션에 필요한 마지막 부분은 보여줄 모든 방 목록의 뷰입니다. 사용자가 목록에서 항목을 누르면 RoomDetail 뷰로 이동할 것입니다.

이 작업을 수행하는 다양한 옵션을 살펴봅시다.

## 내비게이션 링크

사용자를 새로운 뷰로 이동시키는 가장 간단한 방법은 NavigationLink를 사용하는 것입니다. RoomInfo 배열을 사용할 예정이므로, 배열에 있는 모든 RoomInfo 인스턴스마다 destination이 될 init(_:destination:) 메서드와 navigationDestination(for:destination:) 수정자를 사용할 수 있습니다.

<div class="content-ad"></div>

navigationDestination 수정자는 우리가 목록에 표시하는 각 항목을 해당 데이터 값과 링크하는 역할을 합니다. 따라서 사용자가 목록 항목 중 하나를 탭하면 해당 목록 항목의 RoomInfo 인스턴스를 캡처합니다.

![이미지](https://miro.medium.com/v2/resize:fit:462/1*G04epGt-vTLS956eb_Jo1w.gif)

## 네비게이션 경로

이 시점에서, 우리는 단순한 마스터-세부 시나리오에서 어떻게 탐색하는지 알고 있습니다. 우리가 탐색하려는 뷰가 같은 경우(예: RoomDetail), 어떻게 탐색할지 알고 있습니다. 그러나, 네비게이션 스택으로 푸시하려는 서로 다른 유형을 처리하려면 새로운 래퍼인 NavigationPath를 사용해야 합니다. NavigationPath가 하는 일은 어떤 Hashable 타입이든 스택에 푸시할 수 있게 해주는 것입니다.

<div class="content-ad"></div>

메인 뷰에 새로운 버튼을 추가하려고 한다고 가정해 봅시다. 사용자가 해당 버튼을 탭하면 모든 미술 작품이 포함된 새로운 목록 뷰가 표시됩니다. NavigationPath를 사용하여이를 쉽게 구현할 수 있습니다.

참고로 NavigationPath가 처리하는 다른 유형에 따라 필요에 따라 navigationDestination 수정자를 원하는 만큼 추가할 수 있습니다.

<div class="content-ad"></div>

새 API가 어떻게 작동하는지 이미 확인했으니, 이제 우리 애플리케이션에 사용자 정의 라우팅 레이어를 만들어볼 생각을 시작해봐요.

우선 가장 중요한 것은 네비게이션 경로를 저장할 새 클래스를 만들어두어야 합니다.

이게 우리 라우팅 클래스에 필요한 전부에요. Destination 열거형 내에서 가능한 모든 네비게이션 대상을 정의할 수 있어요. 저희는 샘플 앱을 변경하고 거실 보기 또는 침실 보기로 이동할 수 있는 두 개의 버튼을 추가할 예정입니다.

그래서 준비가 되면, 정의된 목적지 중 어디로든 이동하려면 navigate(to) 함수를 호출하기만 하면 됩니다. 이전 보기로 이동하려면 navigateBack() 함수를 호출하면 되고, 루트 보기로 이동하려면 navigateToRoot() 함수를 호출하면 됩니다.

<div class="content-ad"></div>

그럼, 애플리케이션의 진입점으로부터 Route 인스턴스를 생성하고 모든 뷰에서 사용될 환경 객체로 주입해야 합니다.

이전에 사용한 navigationDestination 수정자를 볼 수 있듯이, 목적지에 따라 올바른 뷰를 생성하기 위해 사용합니다. 마지막으로, 뷰에서 라우터 인스턴스를 가져와 필요할 때마다 사용해야 합니다.

<img src="https://miro.medium.com/v2/resize:fit:462/1*wRmlp16QfJEjD80eKrdMsw.gif" />

질문이 있으시면 언제든지 메시지를 남겨주세요! 🙂

<div class="content-ad"></div>

- 🤓 iOS 개발 팁과 통찰력에 관한 정기적인 콘텐츠를 위해 Twitter에서 저와 함께하세요
- 🚀 제 GitHub를 확인해보세요. 거기서 내 예제 프로젝트를 모두 공유하고 있어요