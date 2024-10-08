---
title: "오케스트라 vs 워크플로우 오케스트레이션 2024년 데이터 오케스트레이션 도구 비교"
description: ""
coverImage: "/assets/img/2024-07-13-OrchestravsWorkflowOrchestrationDataOrchestrationTools_0.png"
date: 2024-07-13 02:22
ogImage: 
  url: /assets/img/2024-07-13-OrchestravsWorkflowOrchestrationDataOrchestrationTools_0.png
tag: Tech
originalTitle: "Orchestra vs. Workflow Orchestration   Data Orchestration Tools"
link: "https://medium.com/@hugolu87/orchestra-vs-workflow-orchestration-data-orchestration-tools-d31587158f34"
isUpdated: true
---




# 오케스트라 플랫폼과 오픈 소스 데이터 오케스트레이션 도구 간의 유사점과 차이점에 대해 알아보세요.

## 소개

워크플로 오케스트레이션 도구는 수십 년 동안 존재해 왔습니다. 최근들어 이러한 도구들은 데이터 오케스트레이션 도구라고 불리기 시작했습니다.

과정을 트리거하고 모니터링하며 방향성 비순환 그래프(DAG) 또는 작업 그래프를 제어하는 것은 컴퓨터 자체가 탄생한 시기부터 존재해온 과제입니다. 종종, 이러한 종합적인 프로세스(오케스트레이션)는 소프트웨어 엔지니어링 및 최근에는 데이터 엔지니어링에서 여러 다양한 작업 부하에 필수적입니다.

<div class="content-ad"></div>

2014년 AirBNB의 Maxime Beauchamin에 의해 만들어진 Apache Airflow는 워크플로우 오케스트레이션에서 가장 큰 혁신이었습니다. Apache Airflow 또는 간단히 Airflow는 많은 데이터 팀에게 획기적인 순간이었습니다. 원래의 데이터 오케스트레이션 도구였죠.

시스템 관리자나 데이터 창고 아키텍트의 위장 아래서, 데이터 팀은 파이썬에서 작업의 그래프를 간단히 정의할 수 있었습니다. 회사에 큰 플랫폼 팀이 있고 클라우드에서 컴퓨터나 컴퓨터 클러스터를 유지할 수 있는 능력이 있다면, 복잡한 흐름을 쉽게 트리거하고 모니터링하는 것이 비교적 간단해졌습니다.

지금은 데이터 엔지니어링뿐만 아니라 분석 엔지니어링, 비즈니스 인텔리전스 및 데이터 과학의 등장을 보았습니다. 이제 주목해야 할 점은 이러한 학문들이 인공지능 기반 사용 사례에 파워로 활용될 수 있는 방법으로 전환되고 있다는 것입니다. 이 모든 것이 구체적으로 클라우드에서 일어나고 있습니다.

데이터 산업으로의 대규모 투자 유입도 본 것입니다. 데이터 전문가로서 우리는 이러한 것을 마음에 품고 안도해야 합니다. 벤처 캐피탈 투자의 훌륭한 머릿수들이 우리 분야에 수십억 달러를 투자하려는 것이다. 이것은 우리가 이루고자 하는 가치가 분명히 있다는 좋은 징후입니다.

<div class="content-ad"></div>

이에 따라 "Airflow 대안"이라 불리는 Workflow Orchestration 도구들이 등장했습니다. 이 도구들은 무료로 사용 및 배포할 수 있으며(Airflow와는 다르게 벤처 기업에 의해 지원되는 inherently tricky하고 지타적인 비즈니스 모델에 의해 지원됩니다), Airflow와 약간 다른 기능을 제공합니다. 또한 데이터 엔지니어들이 문제에 접근하는 방식에 영향을 미치고 있는 Data Build Tool인 "dbt"의 등장도 있습니다.

그럼에도 불구하고, 데이터 팀으로써 비즈니스 가치를 전달하는 능력과 더 최근에는 데이터 제품(Data as a Product으로도 알려짐)에 대한 능력이 줄어들었습니다. 데이터 팀에 대한 관심이 가치 센터가 아닌 비용 센터로써 증가함에 따라, 세계적인 경제 불황(2022년부터 현재까지)은 우리 직업 분야에서 파도처럼 일려의 감원을 강제하였습니다.

이에 우리는 다음과 같이 질문할 수밖에 없습니다 — Prefect, Dagster, Mage, Kestra 그리고 이제 우리 자신인 Orchestra와 같이 다음 세대 오픈 소스 데이터 Orchestration 도구들과 함께, 이 도구들에서 무엇이 빠져 있는 것인가? 왜 데이터 팀들은 여전히 가치를 창출하는 데 어려움을 겪고 있는 것일까요?

이 글에서는 Workflow Orchestration 도구들의 몇 가지 기본적인 특성과 그와 Orchestra 간의 기본적인 차이점에 대해 탐구해 보겠습니다. 글의 끝에 도달할 때쯤, 여러분은 비즈니스 가치를 창출하는 데 어떤 접근 방식이 여러분에게 적합한지에 대해 정보를 확보하게 될 것입니다.

<div class="content-ad"></div>

오픈 소스 워크플로 오케스트레이션 도구와 데이터 오케스트레이션 도구 사이에는 차이가 있지만, 오픈 소스 도구를 평가하는 맥락에서 "데이터 오케스트레이션"과 "워크플로 오케스트레이션"을 서로 바꿔 사용할 것입니다.

# 오픈 소스 워크플로 오케스트레이션 도구의 핵심 특징

## Git-컨트롤을 통한 DAG 정의

모든 오픈 소스 워크플로 오케스트레이션 도구는 사용자가 DAG를 구축할 수 있는 수단을 제공합니다. 일반적으로 이는 코드로 개발하는 것을 의미합니다. 어떤 도구는 CLI 도구(예: Astronomer는 Airflow 사용자를 위한 CLI 도구를 제공)를 제공하여 개발 프로세스를 간편화합니다. 다른 도구들은 VSCode 확장 프로그램을 갖추고 있습니다(Kestra 참조). 오픈 소스 워크플로 오케스트레이션 도구들은 사용자가 코드를 사용하여 개발하도록 강요하며, 따라서 Git-컨트롤 및 Git 제공업체와의 통합 또한 당연한 일로 여겨집니다.

<div class="content-ad"></div>

## 스케줄러

모든 오픈 소스 데이터 관리 도구는 스케줄러를 제공합니다. 스케줄러는 시간 및 DAG에 대해 인식하며 항상 실행 중인 서비스입니다. 이는 모든 워크플로의 루트 태스크를 트리거하고 데이터 팀에 대한 기본적인 인프라 구성 요소입니다. 또한 이벤트 기반 시스템에서 스케줄러의 필요성은 이러한 아키텍처의 "항상 스트리밍" 요소로 인해 줄어듭니다.

## DAG를 트리거하고 모니터링하며 유지 관리하기

모든 오픈 소스 워크플로 관리 도구는 물론 DAG를 트리거하고 모니터링할 수 있습니다. 그러나 많은 데이터 팀들이 소홀히 여기는 중요한 점이 있습니다. 바로 이 인프라의 유지 관리입니다.

<div class="content-ad"></div>

## Python 실행하기

모든 오픈 소스 워크플로 오케스트레이션 도구는 임의의 Python 코드를 실행하는 것을 지원합니다. 데이터 엔지니어로서, 이는 API에서 데이터를 페이징하거나 기본적인 데이터 변환을 수행하는 데 매우 도움이 됩니다.

그러나 이러한 기능은 많은 사람들에 의해 남용되고 있습니다. 임의의 Python을 실행하는 것은 사용 사례에 따라 까다로워질 수 있습니다. 예를 들어, 대용량 데이터 처리의 경우, 수 기가바이트 이상의 데이터를 처리하고 다루는 데 필요한 메모리와 계산 성능은 Airflow와 같은 도구가 갖춰야 하는 복잡한 인프라 요구 사항 때문에 처리하기 어려울 수 있습니다.

결과적으로, Airflow는 종종 두 가지 중요한 기능 — 강력한 Python 실행 및 전체 DAG의 트리거, 모니터링 및 상태 유지를 결합한 모노리식 애플리케이션으로 사용되었습니다. 이는 데이터 호수("데이터 호수 오케스트레이션")에서 매우 일반적입니다.

<div class="content-ad"></div>

우리 데이터 엔지니어로서 Airflow를 복잡하고 요구가 많은 워크플로우를 강인하게 실행하는 수단으로만 보는 것은 대단한 오류입니다. 이러한 논리 요소들은 분리되어야 합니다.

# 요약

이제의 오픈 소스 워크플로우 오케스트레이션 도구의 다섯 가지 기본 특성입니다.

- Git-컨트롤을 사용하여 DAG 정의
- 스케줄러
- DAG의 트리거, 모니터링 및 유지 관리
- Python 실행
- 사용자 인터페이스

<div class="content-ad"></div>

# Orchestra는 어떻게 비교되나요?

Orchestra는 데이터 조작, 데이터 관찰 및 데이터 운영을 통합하는 데이터 제어 패널입니다. 우리는 제어를 데이터 스택 내에서 임의의 작업을 실행하는 것으로 정의합니다. 이는 일정에 따라 작업을 트리거하거나 (또는 다른 유형의 트리거) 사용자가 파이프라인 실패로부터 복구하는 등 사용자 작업을 포함합니다. 또한 데이터 전문가로서 대부분의 사람들은 데이터 파이프라인에서 메타데이터를 가져오는 파이프라인을 작성하는 데 시간을 보냈습니다.

거의 모두가 데이터 에스테이트 전반에 걸친 대시보드 사용 방법을 설명하는 대시보드를 만들었습니다. AI가 중심에 오기 시작함에 따라, AI 응용프로그램 및 제품을 모니터링하는 것이 처음부터 그들을 구동하는 파이프라인을 구축하는 것만큼 중요한 작업이 되고 있습니다.

Orchestra 내에서 메타데이터는 주요한 역할을 합니다. 때로는 우리를 "스테로이드를 복용한 Airflow"로 설명하기도 했고, 이곳에서는 유사점과 차이점에 대해 자세히 살펴보고 이 설명이 정당화된 이유에 대해 알아볼 것입니다. 데이터 레이크 조정이든 데이터 웨어하우스 조정이든, 전체 구조의 꼭대기에 있는 것은 Orchestra입니다.

<div class="content-ad"></div>

# 유사성과 대등 비교

## Git-제어를 이용하여 DAG 정의하기

Orchestra는 Git-제어와 세련된 사용자 인터페이스를 결합하여 데이터 팀에게 최상의 장점을 제공합니다. .yml 파일과 Git-제어를 결합하여 Orchestra는 데이터 Orchestration에 선언적 접근을 채택하면서 UI 및 구성 요소 기반 DAG 빌더의 속도와 간편성을 제공합니다.

## 스케줄러

<div class="content-ad"></div>

악보에는 다양한 트리거 유형을 갖춘 완전히 기능이 갖추어진 스케줄러가 있습니다. 사용자는 파이프라인을 수동으로 트리거하거나 파이프라인에 의해 (서브플로우 생성) 또는 이벤트 기반 파이프라인에 대해 웹훅을 통해 트리거 할 수 있습니다. 물론 우리는 스케줄/크론 기반 트리거뿐만 아니라 완전한 시간대 지원을 포함한 기능을 제공합니다.

![OrchestravsWorkflowOrchestrationDataOrchestrationTools_0.png](/assets/img/2024-07-13-OrchestravsWorkflowOrchestrationDataOrchestrationTools_0.png)

## DAG의 트리거, 모니터링 및 유지 관리

<div class="content-ad"></div>

오케스트라는 물론 해당 기능을 제공하지만 몇 가지 중요한 추가 기능이 있습니다. Snowflake 및 Airbyte와 같은 다른 기술 요소들과의 통합의 깊이 때문에 작업이 실패하거나 성공할 때 이는 오케스트라에 반영됩니다. 더 이상 거짓 양성 및 거짓 음성이 발생하지 않습니다.

### Python 실행

우리는 현재 직접 파이썬 실행을 지원하지는 않습니다. 그러나 시간이 흐르면 기본 경량 파이썬 코드 실행을 지원할 것입니다. 대신, Airflow, Prefect, Render, EC2 또는 심지어 Databricks / Snowflake(컨테이너 서비스를 통해)와 같은 통합을 활용하여 효율적으로 파이썬을 실행할 수는 없을까요?

### 사용자 인터페이스

<div class="content-ad"></div>

물론, 저희도 사용자 인터페이스를 갖고 있습니다. 그러나 제어 원칙 때문에 훨씬 강력합니다. 사용자는 DAG가 어떻게 실행되고 있는지 보는 것뿐만 아니라 실패한 노드에서 다시 실행할 수도 있습니다. UI는 또한 데이터 파이프라인에서 생성된 완전한 메타데이터를 확인하는 역할을 합니다.

또한 Orchestra UI는 사용자가 자격 증명 / 연결과 같은 객체를 관리하고 물론, 파이프라인을 구축하는 데 도움을 줍니다.

# 차이점

Orchestra와 오픈 소스 데이터 Orchestration 도구 사이에는 몇 가지 주요 차이점이 있습니다. 첫 번째로 Orchestration은 우리의 가장 중요한 장비 중 하나에 불과하다는 점입니다. Orchestration은 단독 도구보다는 기능으로 더 많이 발전하고 있습니다. 시장에 있는 거의 모든 데이터 도구에는 어떤 종류의 Orchestrator가 내장되어 있습니다(아래 참조).

<div class="content-ad"></div>

Orchestra는 Orchestration을 사용하여 end-to-end metadata를 수집하고, 이를 활용해 Data Control Panel을 만들어 Data Teams가 필요로 하는 것을 제공합니다.

## Data Control Plane (AI 구동?)

Orchestra Control Plane은 Data Teams에게 세 가지 중요한 질문에 답합니다:

- 내 데이터 스택에서 무슨 일이 벌어지고 있나요?
- 내 데이터 스택에서 무엇이 고장나 있으며 그에 대해 어떻게 대처할 수 있나요?
- 내 데이터 스택에서 어떤 것이 작동 중이며 어떤 것이 비용이 들며 최적화할 수 있는 방법은 무엇인가요?

<div class="content-ad"></div>

이것이 가능한 이유는 오케스트레이션과 메타데이터를 결합한 통합 시스템만이 데이터 팀 — 엔지니어와 리더 모두에게 유용한 통찰을 제공할 수 있는 전체 맥락을 갖추고 있기 때문입니다. 우리는 이게 AI에 의해 가능한 것으로 생각해요!

## 완전한 관측 및 메타데이터

오픈 소스 워크플로 오케스트레이션 도구는 모듈식 아키텍처를 사용할 때 풍부한 메타데이터 저장소가 부족합니다. 런타임, 작업 상태 및 오류 메시지와 같은 기본 정보만 저장됩니다. 최근에는 dbt-core 통합으로 인해 Dagster와 같은 일부 제공업체가 자산 기반 계보를 제공할 수 있게 되었습니다(이것이 더 나은 메타데이터 집합을 가지고 있음). 그러나 많은 유용한 데이터 포인트가 빠져 있습니다:

- 변경된 행
- 데이터 품질 메트릭
- 비용
- 사용량(테이블이나 대시보드와 같은 최종 자산의 사용)
- 감사 로그(누가 무엇을 언제 하는지)
- 하위-Dag 메타데이터‍

<div class="content-ad"></div>

마지막 포인트는 특히 중요합니다. 오픈 소스 데이터 조정 도구가 다른 인프라의 작업을 트리거할 때, 해당 메타데이터가 완전히 손실됩니다.

Orchestra는 이 정보를 수집하도록 설계되었으며, 이를 통해 UI가 디버깅 및 데이터 스택 내에서 무슨 일이 일어나고 있는지에 대한 종단간 가시성을 얻기 위해 매우 풍부하고 강력해집니다.

## 종단 간 자산 기반 라인어지

Orchestra는 매번의 파이프라인 실행에 대해 자산 기반 라인어지 그래프를 생성할 수 있는 수준의 지능을 갖추고 있습니다. 이는 지나치게 복잡한 DAG를 보여주는 대신, 노드가 데이터 자산을 나타내고 데이터 품질 테스트와 같은 단계는 노드가 아닌 속성으로 UI에 렌더링됩니다.

<div class="content-ad"></div>

![Orchestra User Interface](/assets/img/2024-07-13-OrchestravsWorkflowOrchestrationDataOrchestrationTools_1.png)

## 강력한 사용자 인터페이스

사용자는 Orchestra UI 내에서 DAG(Directed Acyclic Graph)를 작성하고 연결을 관리할 수 있습니다. 이는 코드 저장소를 유지보수하거나 지속적 통합 및 개발을 구현하고 Orchestration 환경에 비밀을 삽입하는 방법에 대해 걱정할 필요 없이 매우 간단합니다.

사용자 인터페이스에는 end-to-end 계보 및 전체 스택 메타데이터를 비롯한 Observability 플랫폼의 많은 기능도 포함되어 있습니다.

<div class="content-ad"></div>

## 호스팅된 인프라

플랫폼 엔지니어링 팀을 구성하여 비싼 클러스터를 유지보수하거나 클라우드 컴포저나 MWAA와 같이 비싼 관리형 Airflow 솔루션을 구독하는 대신, Orchestra는 완전히 관리되는 SAAS 솔루션입니다. 이는 Fivetran이나 dbt Cloud가 데이터 분석 팀에게 비즈니스 가치 창출에 집중할 수 있도록 기반 인프라 구축을 대신 맡아줌으로써 개발 프로세스를 크게 가속화합니다.

## 고정가격

Orchestra는 실행할 수 있는 임의의 코드 대신 Orchestration에 집중함으로써 사용자들에게 파이프라인과 태스크 수에 기반한 고정가격을 제공할 수 있습니다. 이는 사용자가 성공적인 데이터 제품 및 AI 프로젝트 수에 따라 확장할 수 있도록 해주며, 불필요한 컴퓨팅 비용을 피할 수 있습니다. 이는 서버리스 기술을 활용하여 구현됩니다.

<div class="content-ad"></div>

## 아웃더박스 통합

에어플로우에서는 플러그인이라는 개념이 있습니다. 플러그인을 통해 누구든지 외부 소프트웨어의 작업을 트리거하고 모니터링하는 등 많은 작업을 수행하는 태스크를 만들 수 있습니다. 간단한 예로는 Fivetran 커넥터를 트리거하고 모니터링하는 것이 있습니다. Orchestra에서는 이를 한 단계 더 나아가 태스크가 사전 정의된 블록으로, 인프라나 SAAS의 작업을 실행하고 모니터링 및 최적화하는 부분입니다.

## Git-컨트롤과 사용자 인터페이스의 결합

오케스트라는 데이터 전문가들이 그래픽적이지만 선언적 인터페이스를 통해 신속하게 데이터 파이프라인을 구축할 수 있는 방법을 제공합니다. 이는 파이프라인을 빠르게 연결하는 것을 의미합니다. 데이터 리드나 아키텍트로써 엔지니어가 비즈니스 로직을 개발하고 그로 인해 Airflow를 통해 DAG를 배포하기 위해 두 배의 시간을 소비하는 것을 걱정할 필요가 없습니다. UI를 통해 이 프로세스를 매우 효과적으로 처리할 수 있습니다.

<div class="content-ad"></div>

## Orchestration-만

Orchestration만 사용하여 Orchestration과 Python 코드 실행을 혼합하지 않는 방식으로, Orchestra의 사용은 데이터 전문가들이 이미 모듈화된 데이터 스택을 고려하도록 장려합니다. 이는 무언가를 끝에서 끝까지 배포하는 머리 아픈 문제 없이 개발을 빠르게 만들어 주며, 작은 청크로 세분화할 수 있도록 허용함으로써 개발을 기본적으로 빠르게 만듭니다 (이로써 지속적 통합 또한 Airflow 저장소 등을 통해 너무 많이 시도하려고 하는 대신 도메인별로 수행되도록 허용됩니다).

# 결론

와아, 이제 많은 정보를 받았습니다! 데이터 스택의 제어 평면으로 Orchestra를 사용할 때 즐길 수 있는 많은 장점들이 있습니다. 다양한 인프라 구성 요소와 깊게 통합되어 있음으로써, 데이터 팀은 모든 일이 실제로 발생하고 한 곳에서 보이는 것을 확신하여 편히 쉬실 수 있습니다.

<div class="content-ad"></div>

Orchestra와 Orchestration-only / Observability-only 도구들 사이에는 많은 차이가 있습니다. 그 중요한 이유는 우리가 데이터 프로젝트를 효과적으로 제공하기 위해 필요한 도구들과 아키텍처를 통합, 유연하고 배우기 쉽게 만들었다고 믿기 때문입니다.

2024년에는 데이터 팀들이 직면한 가장 큰 어려움 중 하나가 가치를 입증하는 것입니다. 데이터 팀에 대한 사용량을 통해 가치와 비용을 통해 효율성을 어떻게 실현하는지를 증명하는 것은 더 이상 도전이 아닙니다.

이것은 데이터 및 AI 제품 개발에 대한 정말 혁신적인 접근 방식이며, 우리는 Orchestra를 백본으로 사용하여 여러분이 무엇을 더 빌드할지 기대할 수 없습니다! ⭐️

우리는 무료 티어를 발표했습니다. 여기에서 엔터프라이즈 베타에 가입할 수 있습니다.