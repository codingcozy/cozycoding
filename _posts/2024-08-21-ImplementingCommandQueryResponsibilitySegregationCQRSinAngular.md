---
title: "Angular에서 Command Query Responsibility Segregation (CQRS) 구현하기"
description: ""
coverImage: "/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_0.png"
date: 2024-08-21 17:32
ogImage: 
  url: /assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_0.png
tag: Tech
originalTitle: "Implementing Command Query Responsibility Segregation (CQRS) in Angular"
link: "https://medium.com/@Charles_Matte/implementing-command-query-responsibility-segregation-cqrs-in-angular-4def3409fa0f"
isUpdated: true
updatedAt: 1724245776422
---


프론트엔드 세계는 다양한 JavaScript 프레임워크나 라이브러리로 가득차 있지만, 각각이 독특한 전문성을 제공합니다. 원하는 프레임워크나 라이브러리를 선택하더라도 코드를 작성할 때 검증된 디자인 원칙을 지키는 데 집중하지 않으면 안 됩니다. 이러한 함정에 여러 차례 빠져 본 경험이 있습니다.

저는 Angular에 아직은 입문자지만, 프로페셔널하게 약 1년간 사용하고 있습니다. Angular가 개발자들이 간편하게 OOP 원칙을 프론트엔드에 구현할 수 있도록 해주어 이 프레임워크를 사랑하게 되었습니다. 올바른 아키텍처와 디자인 패턴을 갖춘 Angular 애플리케이션은 아름다운 코드베이스로 성장할 수 있습니다.

하지만 장시간 고민하게 된 것 중 하나는, 비즈니스 로직과 서비스 클래스를 UI 컴포넌트로 넘기지 않으면 어떻게 하는가였습니다. Angular로 중요한 것을 구축하려면 리액티브 프로그래밍 패러다임을 피할 수 없습니다. 매우 강력하지만 RxJs는 때로 상당히 장황해질 수도 있습니다. Angular에서는 UI 컴포넌트가 여러 옵저버블을 구독하도록 만들고 있습니다. 그 옵저버블이 일부 데이터를 반환하면, 데이터를 변환하고 비즈니스 로직을 여기저기 추가하기가 지나치게 쉬워집니다. 규칙적인 프로그래머는 이러한 일이 최소화될 수 있지만, 저는 그렇지 않습니다. UI 컴포넌트에 비즈니스 로직을 추가하면 즉시 살기 편해지는데, 그럴 때는 각종 내용을 담았습니다. 그러고 보면, 어처구니없이 복잡해진 UI 컴포넌트가 우리할머니의 스파게티처럼 보이게 됩니다.

그래서 제가 컴포넌트의 책임 사항 사이에 강한 경계를 그리는 것을 선호합니다. UI 컴포넌트는 HTTP 요청을 직접 구독하지 않아야 합니다. 그 결과도 처리하면 안 되며, 데이터가 어디에서 오는지도 알 필요가 없습니다.

<div class="content-ad"></div>

디자인 패턴을 계속 찾아보다가 "멍청한" 컴포넌트와 "스마트" 컴포넌트 사이에 강력한 경계를 그리는 데 도움이 되는 패턴을 찾았어요. 결국, .NET 세계에서 인기 있는 패턴인 Command Query Responsibility Segregation 또는 CQRS에서 영감을 얻어 깔끔하고 만족스러운 해결책을 찾았어요.

## UI 컴포넌트에서 Query 및 Command 컴포넌트 분리하기

참고: 다음 섹션에서 사용된 코드는 여기에서 찾을 수 있어요.

첫 번째 단계는 인터페이스를 사용하여 UI 컴포넌트와 스마트 컴포넌트를 분리하는 것이에요. 스마트 컴포넌트는 데이터를 가져오는 쿼리 컴포넌트 또는 데이터를 보내는 명령 컴포넌트가 될 수 있어요.

<div class="content-ad"></div>

레시피는 꽤 간단합니다. 쿼리 컴포넌트 및 명령 컴포넌트를 위한 두 가지 인터페이스를 만듭니다. 각 인터페이스는 제네릭 타입을 사용하고 퍼사드를 포함합니다.

![이미지 1](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_0.png)

선택 사항: 이러한 인터페이스를 구현하는 추상 클래스를 선언하고 실제 컴포넌트가 추상 컴포넌트를 확장하도록 할 수 있습니다. 이렇게하면 동일한 유형의 컴포넌트 간에 재사용성을 가질 수 있습니다. 예를 들어, 공유 UI 로직을 추상 클래스 내에 배치하여 실제 구성 요소가 상속하도록 하는 것을 좋아합니다.

![이미지 2](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_1.png)

<div class="content-ad"></div>

## 패키지 추가

퍼사드의 역할은 컴포넌트의 데이터 관련 로직을 조율하는 것입니다. MVC 팬들을 위해 언급하자면, 이것은 컴포넌트를 위한 일종의 컨트롤러라고 생각합니다.

먼저 명령 퍼사드 인터페이스를 살펴봅시다:

![Command Facade Interface](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_2.png)

<div class="content-ad"></div>

명령 게이트웨이에는 세 가지 메서드가 포함되어야 합니다: (1) 명령, (2) 명령을 실행하는 역할을 하는 실행 함수, (3) 수신한 데이터를 처리하는 콜백 함수입니다.

선택 사항: 데이터 가져오기 및 전송을 위해 리포지토리 패턴을 강제하는 기본 게이트웨이 인터페이스를 선언하는 것을 좋아합니다. 이를 통해 데이터 레이어가 게이트웨이로 넘쳐나가는 것을 방지할 수 있습니다. 이것이 ICommandFacade가 IFacade 인터페이스를 확장하는 이유입니다.

![ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_3](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_3.png)

여기에 명령 게이트웨이의 전체 구현이 있습니다. 거기에서 사용된 일부 메서드는 부모 클래스(BaseFacade)에 선언되어 있어서 쿼리 게이트웨이와 함께 재사용할 수 있습니다.

<div class="content-ad"></div>


![Implementing Command Query Responsibility Segregation CQRS in Angular 4](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_4.png)

![Implementing Command Query Responsibility Segregation CQRS in Angular 5](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_5.png)

조금 복잡해 보일 수 있지만, 참아주세요. 이 인터페이스와 추상 클래스를 작성하면 다시 확장하거나 다시 열 필요가 없습니다. SOLID 원칙을 준수하는 데 훨씬 쉽게 만들어 줍니다.

## 모든 것을 함께 놓아보기


<div class="content-ad"></div>

친절한 톤으로 번역하였습니다:

모든 것이 어떻게 결합되는지 확인하기 위해 구체적인 예제를 살펴보겠습니다. 다음은 새 블로그 게시물을 만드는 UI를 처리하는 컴포넌트입니다:

![이미지](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_6.png)

이 컴포넌트는 단 하나의 책임을 가지고 있습니다: 버튼을 표시하고 클릭 이벤트에 반응하는 것뿐입니다. 비즈니스 로직, 데이터 조작, 아무것도 없습니다. 오직 순수하고 깔끔한 UI만 있습니다. 다른 모든 부분은 퍼사드에서 처리됩니다:

![이미지](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_7.png)

<div class="content-ad"></div>

한 번 더 빠된 지면에 주목해보세요. 명령을 조율하는 데 완벽한 일을 하고 있습니다. 다른 것에 대해 알지도, 관심도 없죠.

쿼리 구성 요소는 같은 패턴을 따릅니다:

![](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_8.png)

![](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_9.png)

<div class="content-ad"></div>

여기서 유일한 차이점은 추상 QueryComponent 클래스 내에서 쿼리 실행 호출을 배치했다는 것입니다. 필수 사항은 아니지만 이렇게 하면 내 구체적인 쿼리 구성 요소가 초기화될 때 자동으로 쿼리를 실행합니다. 사용자 작업에 연결해야 하는 경우에는 실행을 구체 구성 요소로 다시 이동할 수 있습니다.

![image](/assets/img/2024-08-21-ImplementingCommandQueryResponsibilitySegregationCQRSinAngular_10.png)

## 마지막으로

이 패턴을 실제 제품 세팅에서 사용해왔는데, 지금까지 잘 작동하고 있습니다. 코드 스니펫 이상의 것이 필요한 경우에는 코드 저장소인 angular-cqrs-example을 확인하시기를 추천합니다.