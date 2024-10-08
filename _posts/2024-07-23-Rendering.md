---
title: "Nextjs 14 빠른 페이지 로딩을 위한 렌더링 개선 2024 최신 가이드"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:20
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Rendering"
link: "https://nextjs.org/docs/app/building-your-application/rendering"
isUpdated: true
---




# 렌더링

렌더링은 작성한 코드를 사용자 인터페이스로 변환합니다. React와 Next.js를 사용하면 코드 일부를 서버 또는 클라이언트에서 렌더링할 수 있는 하이브리드 웹 애플리케이션을 만들 수 있습니다. 이 섹션에서는 이러한 렌더링 환경, 전략 및 런타임 간의 차이를 이해하는 데 도움이 될 것입니다.

## 기본 개념

시작하기 전에 웹의 기본 개념 세 가지에 익숙해지는 것이 도움이 됩니다.

<div class="content-ad"></div>

- 어플리케이션 코드가 실행될 수 있는 환경: 서버 및 클라이언트.
- 사용자가 어플리케이션을 방문하거나 상호작용할 때 시작되는 요청-응답 라이프사이클.
- 서버와 클라이언트 코드를 분리하는 네트워크 경계.

### 렌더링 환경

웹 애플리케이션을 렌더링할 수 있는 두 가지 환경이 있습니다: 클라이언트와 서버.

![이미지](/assets/img/2024-07-23-Rendering_0.png)

<div class="content-ad"></div>

- 클라이언트는 사용자 장치의 브라우저를 가리키며, 애플리케이션 코드를 요청하기 위해 서버로 요청을 보내는 역할을 합니다. 그 후에 서버에서의 응답을 사용자 인터페이스로 변환합니다.
- 서버는 데이터 센터의 컴퓨터를 가리키며, 애플리케이션 코드를 저장하고 클라이언트로부터의 요청을 받아 적절한 응답을 보냅니다.

과거에는 서버와 클라이언트를 위한 코드를 작성할 때 서로 다른 언어(예: JavaScript, PHP)와 프레임워크를 사용해야 했습니다. React를 사용하면 동일한 언어인 JavaScript와 동일한 프레임워크(예: Next.js나 사용자가 선택한 프레임워크)를 사용할 수 있습니다. 이 유연성을 통해 문맥 전환 없이 두 환경 모두를 위한 코드를 원할하게 작성할 수 있습니다.

그러나 각 환경은 자체적인 능력과 제한사항을 가지고 있습니다. 따라서 서버와 클라이언트를 위해 작성하는 코드가 항상 동일하지는 않습니다. 특정 작업(예: 데이터 가져오기 또는 사용자 상태 관리)은 한 환경에서 다른 환경보다 더 적합할 수 있습니다.

이러한 차이를 이해하는 것은 React와 Next.js를 효과적으로 활용하는 데 중요합니다. 지금은 기반을 더욱 확립하기 위해 지금은 서버 및 클라이언트 컴포넌트 페이지에서 자세히 다룰 차이점과 사용 사례를 살펴봅시다.

<div class="content-ad"></div>

### 요청-응답 라이프사이클

대체로 말하자면, 모든 웹 사이트는 동일한 요청-응답 라이프사이클을 따릅니다:

- 사용자 작업: 사용자가 웹 응용 프로그램과 상호 작용합니다. 이는 링크를 클릭하거나 양식을 제출하거나 브라우저 주소 표시줄에 직접 URL을 입력하는 등의 작업일 수 있습니다.
- HTTP 요청: 클라이언트가 서버에 필요한 정보를 포함하는 HTTP 요청을 보냅니다. 요청되는 자원, 사용되는 방법(예: GET, POST) 및 필요한 추가 데이터에 대한 정보가 포함됩니다.
- 서버: 서버가 요청을 처리하고 적절한 자원으로 응답합니다. 이 프로세스는 경로 지정, 데이터 가져오기 등과 같은 여러 단계를 거칠 수 있습니다.
- HTTP 응답: 요청을 처리한 후, 서버가 클라이언트에 HTTP 응답을 보냅니다. 이 응답에는 상태 코드(요청이 성공했는지 여부를 클라이언트에게 알려주는 코드)와 요청된 자원(예: HTML, CSS, JavaScript, 정적 자산 등)이 포함됩니다.
- 클라이언트: 클라이언트는 사용자 인터페이스를 렌더링하기 위해 자원을 구문 분석합니다.
- 사용자 작업: 사용자 인터페이스가 렌더링되면, 사용자는 상호 작용할 수 있으며, 이 과정이 다시 시작됩니다.

하이브리드 웹 응용 프로그램을 구축하는 주요 부분은 라이프사이클에서 일을 어떻게 나눌지와 네트워크 경계를 어디에 둘지 결정하는 것입니다.

<div class="content-ad"></div>

### 네트워크 경계

웹 개발에서 네트워크 경계는 서로 다른 환경을 분리하는 개념적인 선입니다. 예를 들어, 클라이언트와 서버 사이, 또는 서버와 데이터 저장소 사이입니다.

React에서는 클라이언트-서버 네트워크 경계를 어디에 두어야 하는지 선택할 수 있습니다.  

배경에서 작업은 두 부분으로 나뉘어집니다: 클라이언트 모듈 그래프와 서버 모듈 그래프입니다. 서버 모듈 그래프에는 서버에서 렌더링되는 모든 구성 요소가 포함되고, 클라이언트 모듈 그래프에는 클라이언트에서 렌더링되는 모든 구성 요소가 포함됩니다.

<div class="content-ad"></div>

모듈 그래프를 응용 프로그램 파일 간의 의존성을 시각적으로 나타내는 것으로 생각하는 것이 도움이 될 수 있습니다.

React "use client" 규칙을 사용하여 경계를 정의할 수 있습니다. 또한 "use server" 규칙도 있어 React에게 서버에서 일부 계산 작업을 수행하도록 지시할 수 있습니다.

## 하이브리드 애플리케이션 구축

이러한 환경에서 작업할 때 응용 프로그램 코드의 흐름을 단방향으로 생각하는 것이 도움이 될 수 있습니다. 다시 말해서, 응답 중에 응용 프로그램 코드가 한 방향으로 흐른다는 것입니다: 서버에서 클라이언트로.

<div class="content-ad"></div>

클라이언트에서 서버에 접속해야 할 경우, 동일한 요청을 재사용하는 대신 새 요청을 서버에 보냅니다. 이렇게 하면 컴포넌트를 어디에 렌더링할지와 네트워크 경계를 설정하는 방법을 쉽게 이해할 수 있습니다.

실제로 이 모델은 개발자들이 서버에서 먼저 실행하려는 작업을 생각하고, 결과를 클라이언트에 보내서 애플리케이션을 상호작용할 수 있도록 만들도록 격려합니다.

이 개념은 클라이언트와 서버 컴포넌트를 동일한 컴포넌트 트리에 교차로 배치하는 방법을 살펴볼 때 더욱 명확해집니다.