---
title: " 수직적 분할 더 나은 NET 프로젝트를 위한 핵심 방법"
description: ""
coverImage: "/assets/img/2024-07-14-VerticalSlicingTheKeytoBetterNETProjects_0.png"
date: 2024-07-14 00:35
ogImage: 
  url: /assets/img/2024-07-14-VerticalSlicingTheKeytoBetterNETProjects_0.png
tag: Tech
originalTitle: "🚀 Vertical Slicing: The Key to Better .NET Projects!"
link: "https://medium.com/@kmorpex/vertical-slicing-the-key-to-better-net-projects-991c1c757393"
isUpdated: true
---




![2024-07-14-VerticalSlicingTheKeytoBetterNETProjects_0](/assets/img/2024-07-14-VerticalSlicingTheKeytoBetterNETProjects_0.png)

프로젝트가 또래 높이난 스파게티 그릇만큼 난잡하다면 걱정하지 마세요. 수직 슬라이싱은 코드베이스를 잘 구성된, 쉽게 다룰 수 있는 걸작으로 전환하는 마법의 비법일지도 모르겠습니다. 준비됐나요? 함께 해봐요!

## 문제: 전통적인 프로젝트 구조

먼저, .NET 프로젝트를 구성하는 전통적인 방식에 대해 이야기해 봅시다. 이것은 모든 것이 유형별로 구분되어 정리된 옷장과 같습니다 — 셔츠, 바지, 신발 등이 있습니다. 깔끔하지만 옷장에서 옷이나 기능을 조합할 때는 맞는 조각을 찾기 위해 장소를 돌아다녀야하는 불편함이 있습니다. 이 전통적인 접근 방식은 매끄러운 응용프로그램을 구축하는 대신 퍼즐을 맞추는 것처럼 느껴지기도 합니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-14-VerticalSlicingTheKeytoBetterNETProjects_1.png" />

요즘 .NET 프로젝트를 위한 폴더 구성을 살펴보겠습니다. 계층 구조를 사용하는 전형적인 설정입니다:

```js
🔗 DeliveryDash.API
├── 📁 Database
├── 📁 DTOs
├── 📁 Entities
│   ├── #️⃣ Order.cs
│   ├── #️⃣ Customer.cs
│   └── #️⃣ ...
├── 📁 Controllers
│    ├── #️⃣ OrderController.cs
│    ├── #️⃣ CustomerController.cs
│    └── #️⃣ ...
├── 📁 Commands
│    ├── #️⃣ CreateOrderCommand.cs
│    ├── #️⃣ RegisterCustomerCommand.cs
│    └── #️⃣ ...
├── 📁 Handlers
│    ├── #️⃣ CreateOrderCommandHandler.cs
│    ├── #️⃣ RegisterCustomerCommandHandler.cs
│    └── #️⃣ ...
├── 📁 Middleware
├── 📄 appsettings.json
├── 📄 appsettings.Development.json
└── #️⃣ Program.cs
```

이 설정에서 코드의 서로 다른 부분 — Controllers, Commands, Handlers — 모두 각자의 폴더에 있습니다. 그런 방식은 어떤 면에서는 타당하지만, 프로젝트가 커질수록 혼란스러워질 수 있습니다. 여러 부분에 흩어진 기능을 추적하는 것이 어려워져 개발이 살짝 복잡해질 수 있습니다.

<div class="content-ad"></div>

## 해결책: 수직 슬라이싱

이제, 유형별이 아닌 의상별로 옷장을 정리했다고 상상해보세요. 특정 의상을 위해 필요한 모든 것이 한 자리에 모여 있을 것입니다. 이것이 수직 슬라이싱이 코드 베이스에 하는 일입니다. 기술적 레이어로 구성하는 대신 기능별로 구성합니다. 각 기능 슬라이스에는 해당 기능에 필요한 모든 것이 포함되어 있어야 합니다 — 컨트롤러, 커맨드, 핸들러, 모델 등이 모두 제 위치에 있습니다.

![VerticalSlicing](/assets/img/2024-07-14-VerticalSlicingTheKeytoBetterNETProjects_2.png)

예를 들어 설명하면서 조금 더 자세히 알아보겠습니다. 상품 관리 기능을 사용하여 수직 슬라이싱이 어떻게 작동하는지 보여드리겠습니다.

<div class="content-ad"></div>

## 수직 슬라이싱을 통한 폴더 구조

API를 사용하기 쉽게 만들기 위해 REPR 패턴을 사용할 수 있습니다. 이는 Request-EndPoint-Response의 약자입니다. 이것은 수직으로 슬라이싱하는 데 좋습니다. 이를 위해 MediatR 라이브러리를 사용할 수 있습니다.

REPR은 웹 API 엔드포인트가 세 가지 부분으로 구성되어야 한다고 말합니다:
요청, 엔드포인트, 응답

수직 슬라이싱을 사용하면 폴더 구조가 이와 같이 나타날 수 있습니다. Features 폴더를 볼 수 있습니다. 이 폴더에는 수직 슬라이스가 포함되어 있습니다. 각 수직 슬라이스는 단일 API 요청 또는 사용 사례에 해당합니다.

<div class="content-ad"></div>


🔗 DeliveryDash.API
├── 📁 Database
├── 📁 Entities
│   ├── #️⃣ Order.cs
│   ├── #️⃣ Customer.cs
│   └── #️⃣ ...
├── 📁 Features
│   ├── 📁 Orders
│   │   ├── 📁 CreateOrder
│   │   │   ├── #️⃣ CreateOrderCommand.cs
│   │   │   ├── #️⃣ CreateOrderCommandHandler.cs
│   │   │   ├── #️⃣ CreateOrderCommandValidator.cs
│   │   │   └── #️⃣ CreateOrderEndpoint.cs
│   │   └── 📁 GetOrder
│   │       ├── #️⃣ OrderResponse.cs
│   │       ├── #️⃣ GetOrderEndpoint.cs
│   │       ├── #️⃣ GetOrderQuery.cs
│   │       └── #️⃣ GetOrderQueryHandler.cs
│   ├── 📁 Customers
│   │   └── 📁 RegisterCustomer
│   │       ├── #️⃣ RegisterCustomerCommand.cs
│   │       ├── #️⃣ RegisterCustomerCommandHandler.cs
│   │       ├── #️⃣ RegisterCustomerCommandValidator.cs
│   │       └── #️⃣ RegisterCustomerEndpoint.cs
│   └── 📁 ...
├── 📁 Middleware
├── 📄 appsettings.json
├── 📄 appsettings.Development.json
└── #️⃣ Program.cs


## 수직 슬라이싱의 장점

- 기능 중심의 조직: 특정 기능과 관련된 모든 것을 한 곳에 유지합니다. 이렇게 하면 해당 기능에 대한 코드를 찾고 관리하기 쉬워집니다.
- 유지보수가 쉬움: 앱의 한 부분을 변경해도 나머지에 영향을 미치지 않습니다. 그래서 예상치 못한 문제의 가능성이 줄어듭니다.
- 협업 강화: 다른 기능에 작업하는 팀이 서로 방해받지 않습니다. 이것은 병렬 개발을 가능하게 하고 병합 충돌 가능성을 줄입니다.
- 개발 간소화: 수직 슬라이싱을 사용하면 새로운 기능을 추가하거나 기존 기능을 변경하는 것이 쉽습니다. 각 층이 목적을 가진 별도의 부분인 집을 짓는 것과 같습니다.
- 확장성 개선: 앱이 커질 때, 수직 슬라이싱을 사용하면 관련 코드를 함께 유지하므로 관리하기 쉬워집니다. 이렇게 하면 앱이 성장할수록 더 많은 기능과 기능을 처리하기 쉬워집니다.
- 테스트 단순화: 테스트가 보다 목표를 정확히 하고 처리하기 쉽습니다. 기능이 분리되어 있기 때문에 다른 모든 코드에 빠져들지 않고 각각을 개별적으로 테스트할 수 있습니다.

## 고려해야 할 도전과제


<div class="content-ad"></div>

- 시작하기: 수직 슬라이싱 아키텍처를 설정하려면 신중히 계획해야 합니다. 기능을 논리적으로 그룹화하여 구성해야 합니다.
- 코드 중복: 수직 슬라이싱은 특히 여러 기능이 유사한 구성 요소를 공유하는 경우 중복을 야기할 수 있습니다. 공유된 로직을 신중하게 관리하여 중복을 피하는 것이 중요합니다.
- 학습 곡선: 팀이 전통적인 계층 구조 방식을 사용하는 경우, 수직 슬라이싱으로 전환하는 데는 시간이 걸릴 수 있습니다. 교육이나 문서 작성을 통해 전환을 더 원할하게 만들 수 있습니다.
- 소규모 프로젝트의 추가 부담: 작은 프로젝트의 경우, 수직 슬라이싱이 필요하지 않을 수 있습니다. 이는 코드를 격리하고 구성하는 장점이 더 큰, 더 복잡한 응용 프로그램에서 더 중요하기 때문입니다.

# 결론

수직 슬라이싱은 .NET 프로젝트를 조직하는 훌륭한 방법입니다. 기술적인 계층을 생각하기보다는 기능을 고려합니다. 관련된 구성 요소를 함께 그룹화하여 더 간단하고 관리하기 쉬운 코드베이스를 만듭니다. 새로운 프로젝트와 기존 프로젝트 모두에 좋은 선택이며, 명확성과 효율성을 제공합니다.

![image](https://miro.medium.com/v2/resize:fit:1400/0*W7yiDurUIGn7jOKJ.gif)

<div class="content-ad"></div>

👏 만약 이 내용이 도움이 되었다면 클랩(clap)을 눌러 주시면 감사하겠습니다 (버튼을 누른 채로 더 많이 클랩할 수 있습니다). 댓글에 귀하의 생각과 제안을 남겨주시면 이 주제에 대해 계속 토론할 수 있습니다.

읽어 주셔서 감사합니다...