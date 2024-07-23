---
title: "API 요구 사항 분석하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-RequirementsAPIAnalysis_0.png"
date: 2024-07-07 23:49
ogImage: 
  url: /assets/img/2024-07-07-RequirementsAPIAnalysis_0.png
tag: Tech
originalTitle: "Requirements , API: Analysis"
link: "https://medium.com/analysts-corner/requirements-api-analysis-df62b5808fa2"
---


이전 장: [여기를 클릭하여 이전 장을 읽어보세요.](https://medium.com/analysts-corner/requirements-api-definitions-3f75a7308ae6)

이번 장에서는 먼저 API에 대한 요구 사항이 왜 필요한지 그리고 요구 사양 분류 프레임워크에 그 위치를 이해하게 될 것입니다. 그런 다음 요구 사항 엔지니어링 프로세스와 고려해야 하는 구체적인 측면을 살펴보겠습니다.

# API에 요구 사항이 필요한 이유는 무엇인가요?

API에 대한 요구 사항을 작성하는 이유에 대해 답해보겠습니다. 비즈니스 애널리스트들이 갑자기 API 요구 사항에 관심을 가지기 시작한 이유를 설명해보겠습니다. IT 업계에서 비즈니스 애널리스트로서 경력을 시작한 것이 10년 전이었는데, 당시에는 API 요구 사항을 일반적인 업무 범주로 고려하지 않았습니다. 그러나 요즘에는 BA, PO, PM의 채용 공고에서 API 지식을 요구하는 경우가 더 많이 보입니다. 그 이유에 대해 주관적으로 설명해보겠습니다.

<div class="content-ad"></div>

In the past, the main software architecture approach was deploying a monolithic application on-premise. This involved a tightly connected backend service that interacted with a desktop or web UI client. The backend was linked to various internal or external services, including functionalities like file uploads via FTP.

Business analysts primarily focused on defining business capabilities and how they were showcased on the UI, along with requirements for integrations like mappings and request logic. The intricate interactions between the backend, UI client, and integrated systems were handled by technical experts.

However, the landscape started shifting with the rise of Cloud, Software as a Service (SaaS), and Microservices Architecture (MSA) as the dominant trend in system design:

![RequirementsAPIAnalysis_0](/assets/img/2024-07-07-RequirementsAPIAnalysis_0.png)

<div class="content-ad"></div>

최근의 추세로 인해 시스템들은 이전보다 더 연결되어 있습니다. 일부 서비스는 API를 우선으로 채택하여, 사용 시 API를 통해서만 이용할 수 있다는 의미입니다. 따라서 API는 더 높은 요구사항을 가진 비즈니스 모델이 되었습니다. 이에 따라 이해관계자, 규정, 제한 사항 등을 고려하는 업무 분석가, 제품 소유주, 제품 관리자가 필요합니다.

웹, 모바일 애플리케이션, IoT 등을 포함한 API 클라이언트 수가 급증했습니다. 또한, 마이크로서비스는 시스템 내에서 서로 API를 통해 통신합니다. 따라서 수직으로나 수평으로, 외부 및 내부 클라이언트들이 각각의 이해관계자와 특정 요구 사항을 갖게 되었습니다.

또 다른 중요한 측면은 API가 시스템에 침투할 직접적인 방법을 제공하기 때문에 공격에 민감하다는 것입니다. 그러므로 API를 설계하고 구현하는 것 뿐만 아니라 그들의 보안도 확실히 해야 합니다.

요약하면, 클라우드, MSA, SaaS의 부상, 클라이언트 수의 증가, 높은 보안 위험, 그리고 속하는 산업 및 정부 규제에 의해 API의 "비즈니스화"가 진행되었으며, 모든 종류의 비즈니스 관계자들에게 흥미를 끄는 영역이 되었습니다.

<div class="content-ad"></div>

그것은 그 전환이 이루어진 상세한 설명은 아니지만, 제 관점에서는 그렇게 보입니다. 댓글에서 여러분의 생각을 듣고 싶습니다.

# 요구 사항 분류

BA 이론에 뛰어들기 전에, API 요구 사항이 기능적이지 않다는 것을 명확히해 봅시다. UI가 가장 가까운 비유입니다. 시스템 기능(함수)이 있고 사용자가 액세스할 수 있는 여러 가지 방법이 있습니다. 이전 챕터에서 API를 배포 채널로 확인했습니다. 공개 인터페이스인 UI, 명령줄 인터페이스(CLI), 챗봇 등에 대해 동일합니다.

API 디자인을 통해 기능 요구 사항을 표현할 수 있습니다. 그러나 API 자체는 시스템 기능의 직접적인 관심사가 아닙니다. UI 비유로 돌아가보면: 디자인 패턴, 사용할 구성 요소 또는 색 구성은 UX에 중요하지만 이를 기능적 요구 사항이라고 불리하지는 않습니다.

<div class="content-ad"></div>

예를 들어, "시스템은 X 항목 목록을 Y로 필터링하고 Z로 정렬하여 반환해야 합니다"는 기능 요구 사항입니다. 그러나 이 목록을 UI 및 API로 어떻게 반환하는지는 다를 수 있습니다. 하나의 프런트엔드만 해당 API 엔드포인트를 사용한다면 UI 측면에서 설명하는 것으로 충분합니다. 이 단일 클라이언트가 여기서 주요 이해관계자 역할을 합니다. 그러나 여러 클라이언트가 자신의 목적을 위해 목록을 얻고 싶어하는 경우, 그들과 의사 소통하고 필요한 것과 왜 필요한지에 대해 기대치를 맞추기 위해 추가 시간을 투자해야 합니다. 그 결과, UI에 목록 항목을 반환하는 방식이 상당히 달라질 수 있습니다.

좋아요, API 요구 사항은 기능적이 아니므로 어떤 유형의 요구 사항에 속하는지 여기서 살펴보겠습니다.

K. Wiegers와 C. Hokanson의 최근 "소프트웨어 요구 사항 핵심"에서는 "외부 인터페이스 요구 사항"을 정의합니다.

K. Wiegers와 J. Beatty의 클래식인 "소프트웨어 요구 사항" 3판에서는 비기능적 요구 사항과 인터페이스 간의 상호 연결을 강조합니다.

<div class="content-ad"></div>

ISO/IEC/IEEE 29148:2018 표준에 따르면 "시스템 및 소프트웨어 공학 - 수명주기 프로세스 - 요구공학"에서 인터페이스 요구 사항(5.2.8.3)이라는 요구 유형이 있습니다.

인터페이스 요구 사항은 비기능적이지 않습니다. 왜냐하면 인터페이스는 자체의 품질 속성인 보안, 호환성, 사용성, 규정 준수 등이 있기 때문입니다. 특정 능력의 품질 속성일 뿐만 아니라 인터페이스는 특정 능력의 품질 속성입니다. 그러나 별로인 인터페이스는 사용자 경험을 저해할 것입니다. UI든 API든 언급하고 있는 경우 사용자 경험이 분명히 나빠집니다.

또한 선택한 기술 및 디자인 패턴은 인터페이스에 제약을 함축합니다. 그러므로 소프트웨어 아키텍처도 분석 프로세스에 영향을 줍니다, 특히 API의 경우입니다. 예를 들어, API가 동기적인지 비동기적(이벤트 기반)인지는 아키텍처 및 사용된 프로토콜에 따라 달라집니다.

인터페이스 요구 사항(여기에는 UI 및 CLI도 포함됨)에서는 시스템이 소비자로 작동할 때 통합 요구 사항과 시스템이 생산자로 작동할 때 API 요구 사항을 분리하는 것이 합리적입니다(생산자 및 소비자에 대한 이전 부분 참조). 이 하위 유형들은 많은 공통점을 공유하지만 결과는 다릅니다. 후자의 결과는 운영 API 엔드포인트를 의미하고, 시스템 간의 작용은 첫 번째 결과입니다.

<div class="content-ad"></div>

# 요구 엔지니어링

IIBA의 BABOK v3(2015)은 API 또는 통합 요구사항을 분류하지 않습니다. 그러나 API 및 인터페이스에 대해 일반적으로 언급하는 인터페이스 분석 기법(10.24)에 대해 설명합니다. 이 분석은 다음과 같은 3 단계로 구성됩니다:

- 상호작용자들의 상호작용을 연구하여 필요한 인터페이스를 식별하고 이해하기 위한 준비 작업을 수행합니다.
- 각 향후 인터페이스의 기능을 설명하고 인터페이스의 유형 및 초기 디자인을 평가함으로써 식별을 진행합니다.
- 입력, 출력, 검증 등과 같은 인터페이스를 정의합니다.

요구 사양의 개발은 "전통적인" 비즈니스 분석 프로세스와 그리 다를 바 없습니다: 문제 인지, 분석, 명세 및 검증 단계가 포함됩니다. 현재 우리는 주로 첫 단계에 집중하고 있습니다. 명세 및 추가적인 검증은 특수성이 있지만, HTTP 프로토콜 기본 사항과 API 구조에 대해 자세히 탐구해야 할 것입니다. 이는 후속 장에서 다룰 예정입니다.

<div class="content-ad"></div>

# 시작점

여기, 고려해야 할 많은 질문들이 있습니다:

- 왜 이것이 필요할까요?

예를 들어, 우리는 시스템이 생성하는 데이터를 UI가 아닌 유료 공개 API를 통해 외부 제3자들이 얻을 수 있도록 하는 접근 지점을 만들고 싶어합니다. 요약하면, 새로운 배포 채널을 추가하고 수익화하고자 합니다.

<div class="content-ad"></div>

- 누구를 위해 이것을 하는 건가요?

예를 들면, 의료 데이터 중개업자들은 우리의 데이터를 활용하여 시장 예측 모델을 구축할 것입니다.

- 이것이 무엇을 할 건가요?

예를 들면, 지정 기간 동안의 구매 기록 세트를 반환합니다.

<div class="content-ad"></div>

- 그들은 어떻게 할 것인가요?

예를 들어, 토큰으로 승인하고 기업 ID와 시작 및 종료 날짜를 제공합니다.

# 전제 조건

다음 단계는 API 호출의 전제 조건을 정의하는 것입니다 — 인증과 권한 부여:

<div class="content-ad"></div>

- 인증은 클라이언트를 확인하고 API와 통신할 수 있도록 하는 것입니다.
- 권한은 특정 클라이언트가 특정 API 요청을 하고 특정 데이터를 볼 수 있도록 하는 것입니다.
- 또 다른 중요한 요점은 소비자가 호출을 하기 위해 필요한 모든 입력 데이터를 가지고 있는지입니다.
- 새로운 API 엔드포인트와 관련된 새 권한이 필요한지 또는 기존 권한을 재사용해야 하는지 이해해야 합니다.

## 기능적 공백

다른 중요한 질문은 시스템이 이미 그러한 기능을 제공하는지 여부입니다. 그렇다면 어떤 개선이 필요한지도 알아봐야 합니다.

현재 시스템 기능과 원하는 인터페이스 간에 개선이 필요한 경우, 그것은 해결책 요구 사항이 됩니다. 그래서 거기에는 전통적인 BA(비즈니스 분석가) 방법론이 사용되어야 합니다.

<div class="content-ad"></div>

예를 들어, 새로운 API 엔드포인트에 대한 새 검색 기준이 추가된 경우, 해당 기준이 검색 인덱스에 포함되어 있는지 확인해야 합니다. 그것은 이미 백엔드와 관련된 문제입니다.

해당 인터페이스를 통해 해결책을 설명하려면, 모든 측면을 충분히 다루기에는 충분하지 않습니다. 최소한의 요구는 새 API가 사용될 위치를 설명하는 데이터 모델로서 데이터 사전이나 entity relationship diagram과 함께 사용 사례를 포함하는 것입니다.

하나 이상의 API 엔드포인트가 사용될 사용 사례를 식별하고 설명하는 것이 과제입니다. 요즘에는 API가 매우 세분화되어 있기 때문에, 문맥 없이 단일 API는 큰 의미가 없습니다. 그래서 여러 Entity와 관련된 특정 동작이 포함된 시나리오는 하나 이상의 API 호출로 이루어질 것입니다.

# 계약 & 블랙 박스

<div class="content-ad"></div>

간단히 API를 살펴보면 다음과 같습니다.

![RequirementsAPIAnalysis_1](/assets/img/2024-07-07-RequirementsAPIAnalysis_1.png)

분석 결과는 입력/요청 구조와 결과/응답을 설명한 것입니다. 이것은 우리가 API 계약이라고도 하는 '블랙 박스'를 대표합니다.

- API 계약은 생산자와 소비자 간에 기대되는 입력과 결과에 대한 합의입니다.
- 계약은 엄격하게 형식화되어 있으며, 클라이언트는 API를 사용하기 시작할 때 이를 따르기로 동의합니다.
- API 생산자는 계약의 일관성을 유지하기 위한 책임을 집니다.
- API 계약을 어기면 법적 처벌을 받을 수 있습니다.
- API 계약은 구현 전에 정의될 수 있어 양쪽이 병행하여 작업할 수 있습니다.

<div class="content-ad"></div>

And here are two important points to keep in mind:

- The request/response model, also known as the API model, is not the same as the Data Model in a database. The API serves as a layer built on top of the database to facilitate interactions with external parties. The models may differ in names, the attributes exposed, and even in some data types.
- When creating a new API, it is crucial to maintain consistency with the existing system APIs. If a specific naming convention is already in place, it is advisable to adhere to it. This practice helps prevent potential confusion and reduces the cognitive load for users. 

API Models can be documented using:

- Formal methods such as data formats like XML and JSON, or specification formats like OpenAPI. OpenAPI is a commonly used formal specification format for HTTP APIs.
- Informal methods like an Excel spreadsheet or any text-based format. Informal formats often resemble OpenAPI but are presented in a more user-friendly manner.

<div class="content-ad"></div>

요청 로직은 검은 상자(유리 상자) 내부에서 무슨 일이 일어나는지를 다루는 부분을 말해요. 이를 다음과 같이 설명할 수 있어요:

- 비공식적으로(여러 단계로 구성된 텍스트 설명)
- 시각적으로: UML 활동 또는 순서도 등
- 다양한 명세 형식

요청 로직은 Entity 객체에 대한 간단한 CRUD 명령일 수도 있어요. 또한 몇 가지 내부 API가 관련된 정교한 집계 요청일 수도 있어요. 이 부분은 나중 장에서 다루게 되죠.

# 너무 순발력이 되지 마세요

<div class="content-ad"></div>

UI 작업 중에 변경 사항을 도입하는 것이 API보다 쉽습니다. 사용자 경험을 파괴할 위험이 있지만, 사용자가 새로운 것에 적응하는 데 필요한 인지 노력에 관한 것입니다. 그들은 변경 사항에 대해 돈을 지불하지 않습니다.

반면에 소비자들은 기존 API의 파괴적인 변경이나 새로운 API로의 이전에 적응하기 위해 대가를 지불합니다. 추가적인 통합 및 유지보수 노력이 필요합니다. 클라이언트 코드 수정부터 자동 테스트, 문서 업데이트까지 모든 것이 필요합니다.

따라서 모든 이 "반복적이면서 유연한(flexible)" 것은 API에 대해서는 그렇지 않습니다. 물론 API는 수명 주기 동안 진화합니다. 그러나 API 계약을 자주 파괴하는 것은 지속 가능성을 달성하는 좋은 방법이 아닙니다. 이를 완화하는 방법이 있지만, 모든 API 제공자의 인식 속에 꼭 있어야 합니다.

요구 사항 엔지니어링 관점에서, 분석은 특히 여러 소비자가 사용할 유료 공개 API인 경우 보다 철저히 수행되어야 합니다.

<div class="content-ad"></div>

# 주제에 대한 읽기

- API 요구 사항의 정의
- 비기능 요구 사항 및 API
- 변경 사항 및 하위 호환성
- IIBA 벨라루스의 "요구 사항 및 API" 웨비나 요약

다음 장: 구조 (작업 중)

원문 게시물: [https://ilyazakharau.com](https://ilyazakharau.com)