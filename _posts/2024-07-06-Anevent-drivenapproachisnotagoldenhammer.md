---
title: "이벤트 주도 접근 방식의 한계와 대안은"
description: ""
coverImage: "/assets/img/2024-07-06-Anevent-drivenapproachisnotagoldenhammer_0.png"
date: 2024-07-06 03:40
ogImage: 
  url: /assets/img/2024-07-06-Anevent-drivenapproachisnotagoldenhammer_0.png
tag: Tech
originalTitle: "An event-driven approach is not a golden hammer"
link: "https://medium.com/gitconnected/an-event-driven-approach-is-not-a-golden-hammer-b1b9265ec7d6"
isUpdated: true
---





![Event-driven architecture](/assets/img/2024-07-06-Anevent-drivenapproachisnotagoldenhammer_0.png)

이벤트 주도 아키텍처(EDA)는 시스템 구성 요소가 이벤트의 생성, 감지 및 사용을 통해 통신하고 작동하는 설계 패러다임입니다. EDA에서 이벤트는 시스템 내에서 상태 변경이나 발생에 중대한 변화를 나타내며, 구성 요소(이벤트 생성자)는 이러한 이벤트를 생성하여 상태 변경이나 작업을 신호하기 위해 사용합니다. 다른 구성 요소(이벤트 소비자)는 이러한 이벤트에 응답하여 발생한 이벤트에 따라 추가 처리 또는 작업을 트리거합니다. 이 아키텍처는 구성 요소가 비동기적으로 상호 작용하고 독립적으로 동작함으로써 느슨한 결합, 확장 가능성 및 유연성을 장려합니다.

비동기 이벤트 주도 아키텍처(EDA)는 많은 장점을 제공하지만, 일반적인 해결책은 아닙니다. REST API를 사용하는 것이 더 적합하거나 유리한 상황도 있습니다. 먼저 즉각적인 응답이 필요할 때 필요합니다. 또한 특정 API 엔드포인트를 호출할 때 명령 접근 방식은 비동기적인 경우에도 유용할 수 있습니다(예: 202 Accepted 응답 상태를 사용하여 비동기 동작을 모방한다).

몇 년 전 분산 시스템을 리팩토링할 때, 이벤트 발행-구독 패턴(pub-sub)을 어디서나 사용하여 통신을 재설계하고 싶었습니다. 많이 들어와서 이것이 서비스의 결합 해제와 독립성을 증진시킨다고 들었습니다. 그러나, pub-sub는 API 구현의 기술적인 방법일 뿐이며, 묶인 컨텍스트 간의 논리적인 관계에 대한 것이야말로 주요해야 합니다.

<div class="content-ad"></div>

EDA(이벤트 중심 아키텍처) 이벤트 생성기는 일반적으로 서비스 간의 계약을 소유합니다. 이 소유권은 이벤트의 성격에서 비롯됩니다: 이벤트는 생성기의 도메인 내에서 중요한 상태 변화나 발생을 알리기 때문입니다.

여러 유형의 이벤트를 많이 발행하고 소비자도 많은 서비스가 있는 경우를 가정해 봅시다. 또한, 계속해서 변경되는 이유는 반복적으로 개발하는 핵심 도메인이기 때문입니다. 기존 이벤트의 필드를 수정하고 새로운 이벤트를 추가하는 경우가 많습니다. 문제점은 무엇일까요? 각 이벤트 변경은 각 소비자에서 업데이트되어야 합니다. 발행자와 소비자를 소유하는 경우 크게 걱정할 필요는 없습니다(하지만 문제를 일으킬 수 있습니다). 그러나 반면에, 두 계약의 각각을 유지하는 별도 팀이 있다면 많은 노력, 조정 및 의사 소통 부담이 발생할 것입니다.

테이블 예약 사용 사례에 대해 생각해 봅시다. 테이블 예약시 예약한 사람에게 알림을 보내야 합니다. 두 가지 옵션을 고려해 봅시다:

- TableReserved 이벤트는 ReservationService가 발행하고 NotificationService가 소비합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-06-Anevent-drivenapproachisnotagoldenhammer_1.png)

- 이메일/SMS 등을 보내는 끝점을 제공하는 NotificationService REST API

![이미지](/assets/img/2024-07-06-Anevent-drivenapproachisnotagoldenhammer_2.png)

이 단계에서 두 옵션 모두 직관적으로 보입니다. 중요한 점을 주목해주세요: 이벤트 방식의 경우 ReservationService는 계약 생성기입니다. 반면에 REST API 방식의 경우 ReservationService는 계약 소비자입니다. 이는 향후 개발에 영향을 미치는 중요한 관계의 차이입니다.


<div class="content-ad"></div>

비즈니스로부터 새로운 요구 사항이 들어왔을 때, 예약을 취소하는 것은 고객에게 알림 메시지를 보내야 한다는 것입니다. 이러한 경우 이벤트 접근 방식을 사용한다면 ReservationService에 새 이벤트인 TableReservationCancelled을 추가하고 NotificationService 쪽에서 이벤트를 처리해야 합니다. 따라서 두 서비스를 변경해야 합니다. 반면에 REST API 접근 방식은 ReservationService에서만 변경이 필요합니다. 물론, NotificationService API가 제목, 텍스트, 이메일 주소 등과 같이 안정적인 매개변수를 사용한다고 가정합니다. 그러나 이는 매우 중요한 사항입니다. REST API 접근 방식은 한쪽이 안정적이고 범용적인 경우 유리할 수 있습니다. 범용 서브도메인의 다른 예는 검색 엔진이나 보고서가 있습니다. 안정성 때문에 일반적인 바운디드 컨텍스트는 관계의 상류쪽이 되어 계약을 제공해야 합니다. 상류/하류 관계에 대해 더 알고 싶다면 DDD 콘텍스트 맵 도구를 권장합니다.

결론적으로, 솔루션을 구현하기 전에 다음 질문을 하면 좋은 아이디어입니다:

- 관계의 어느 구성 요소가 미래에 변화하지 않을까?
- 우리에게 범용 서브도메인이 있는가?
- 순환 종속성이 있는가(두 구성 요소가 서로 다른 부분을 강제하는 경우)? 때로는 두 구성 요소가 서로에게 소비되는 이벤트를 게시할 때 발생하는 경우가 있습니다. 이는 일반적으로 디자인의 문제입니다.
- A 구성 요소가 B 구성 요소에 대해 알아야 하는가? 우리 예제에서는 NotificationService가 테이블 예약 개념에 대해 알아야만 하는가?

이벤트 주도 아키텍처는 마이크로서비스를 디자인하는 데 매우 좋은 패턴이지만 때로는 유연해야 하며 다른 접근 방식과 혼합해야 할 수 있습니다.