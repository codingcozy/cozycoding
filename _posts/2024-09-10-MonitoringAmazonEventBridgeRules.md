---
title: "아마존 이벤트브릿지 규칙 모니터링"
description: ""
coverImage: "/assets/img/2024-09-10-MonitoringAmazonEventBridgeRules_0.png"
date: 2024-09-10 18:54
ogImage: 
  url: /assets/img/2024-09-10-MonitoringAmazonEventBridgeRules_0.png
tag: Tech
originalTitle: "Monitoring Amazon EventBridge Rules"
link: "https://medium.com/towards-data-science/monitoring-amazon-eventbridge-rules-127434c58984"
isUpdated: false
---


클라우드워치 이벤트에서 유래한 아마존 웹 서비스(AWS)는 2019년 7월에 이벤트 기반 아키텍처를 위한 서버리스 오퍼링인 이벤트브릿지를 출시했습니다. 이제 이벤트브릿지는 여러 구성 요소로 구성되어 있습니다:

- 이벤트 프로듀서와 컨슈머를 통합하는 룰이 있는 버스(브로커).
- 일회성 또는 반복적인 작업의 완전 관리된 호출을 위한 스케줄링.
- 프로듀서와 컨슈머 간의 관리되는 통합을 제공하는 파이프.

이벤트브릿지가 어떻게 작동하는지 설명하는 온라인 자료가 많이 있습니다. 제품 문서는 유용한 시작점 역할을 합니다. 이 게시물에서는 특히 이벤트브릿지 룰의 모니터링에 대해 자세히 살펴보겠습니다.

![이미지](/assets/img/2024-09-10-MonitoringAmazonEventBridgeRules_0.png)

<div class="content-ad"></div>

우리는 메트릭이 어떻게 작동하는지에 대한 개요로 시작하고, 그 후에 현재 Rule 메트릭 제공 사항을 탐색하고 그 한계를 확인할 것입니다. Rule 메트릭을 넓은 범위로 다룬 후에는 고객에게 추가 가치를 제공할 수 있는 제안된 개선 사항으로 마무리하겠습니다.

# Rule 메트릭이 작동하는 방식

다른 AWS 서비스와 일관성 있게, EventBridge Rules은 성능을 관찰하기 위한 CloudWatch 메트릭을 제공합니다. 일부 메트릭은 하나 이상의 차원으로 분해될 수 있어 특정 리소스의 동작을 검토할 수 있습니다. Rules은 세 가지 차원을 제공합니다:

- EventBusName: Rule의 소스 이벤트 버스의 이름.
- EventSourceName: 이벤트 소스의 이름 (Partner Rules 전용).
- RuleName: Rule의 이름.

<div class="content-ad"></div>

각 메트릭의 지원되는 차원은 "EventBridge metrics" 테이블의 "Dimensions" 아래에 나열됩니다. 그러나 작성 시점에서 이 테이블은 실제 경험과 일관성이 없었기 때문에 CloudWatch를 직접 사용하여 사용 가능한 각 메트릭의 차원을 파악하는 것이 유용할 것입니다. 나중에 다룰 FailedInvocations를 살펴보겠지만, 예를 들어 문서화된 차원에는 RuleName만 나열되어 있으며 Region 및 Namespace과 같은 기본값도 함께 언급되었습니다. 그러나 아래 스크린샷에서도 보여지는 것처럼, 왼쪽에 my-event-bus로 설정된 EventBusName도 확인할 수 있습니다.

![image](/assets/img/2024-09-10-MonitoringAmazonEventBridgeRules_1.png)

# 트래픽 양 관찰

트래픽 양과 관련된 여러 메트릭이 있으며, 이를 공유할 가치가 있습니다.

<div class="content-ad"></div>

- MatchedEvents: Rule 필터링 기준과 일치하는 이벤트 수입니다.
- TriggeredRules: 일치하는 이벤트에 의해 트리거된 Rule(s)의 수입니다.
- ThrottledRules: Rule(s)이 얼마나 자주 지연되어 호출되었는지를 보여줍니다.

많은 사용 사례에서 MatchedEvents와 TriggeredRules는 서로 교차 사용될 수 있습니다. 모든 또는 특정 Rule(s)의 트래픽 양을 모니터링하기를 원할 때 둘 다 적용됩니다. ThrottledRules는 계정 전체의 Rule 호출에 대한 제한이 깨졌음을 보여줍니다. 이 한계는 "초당 트랜잭션으로 제한되는 호출 제한"에서 조정할 수 있습니다.

Rule이 일치하는 이벤트에 의해 트리거되었을 때 목표 소비자를 호출해야 합니다. 상기 메트릭 중 어느 것도 목표 호출을 포함하지 않습니다. 이로 인해 다음과 같은 몇 가지 탐구할 가치 있는 질문이 제기됩니다:

- Rules는 호출 실패를 어떻게 처리합니까?
- 어떤 메트릭을 사용하여 호출 실패를 추적할 수 있습니까?
- 성공적인 호출을 어떻게 관찰합니까?

<div class="content-ad"></div>

# 실패한 호출 처리

어떤 경우에는 규칙이 대상을 호출하지 못하는 경우 호출을 다시 시도할 수 있습니다. 예를 들어, 네트워크 문제로 인한 실패는 다시 시도되지만 권한 부족 오류는 다시 시도되지 않습니다.

재시도가 필요한 경우, 규칙은 대상 소비자의 재시도 정책을 준수합니다. 재시도 횟수와 간격은 최대 185회와 24시간까지 가능합니다. 그러나 모든 대상이 구성 가능한 재시도 정책을 제공하는 것은 아닙니다. 예를 들어, 이벤트브릿지 이벤트 버스 대상의 재시도 정책은 조절할 수 없습니다.

![MonitoringAmazonEventBridgeRules_2](/assets/img/2024-09-10-MonitoringAmazonEventBridgeRules_2.png)

<div class="content-ad"></div>

# Invocation Failures 모니터링

Rule이 대상을 호출하는 데 실패할 때 (해당되는 경우 재시도 포함), 이벤트를 전달하는 데 영구적으로 실패했다고 말합니다. 이러한 경우를 검토하기 위해 앞에서 언급한 FailedInvocations 메트릭을 활용할 수 있습니다. 이는 영구적인 실패만을 표시하며 다시 시도할 실패는 고려되지 않습니다.

다시 시도할 실패는 관찰하기가 더 어렵습니다. 2023년 11월에 EventBridge가 RetryInvocationAttempts를 포함한 여러 새로운 메트릭을 도입했습니다. "대상 호출이 다시 시도된 횟수"를 정확히 제공하는 이 메트릭은 우리가 필요한 것을 제공하는 것처럼 보입니다. 그러나 이는 차원을 지원하지 않으므로 개별 Bus 또는 Rule마다 다시 시도를 검토할 수 없습니다. 단지 전체 AWS 계정 전체의 다시 시도 횟수만 볼 수 있습니다. 단일 계정에서 많은 Rule을 사용하는 경우에는 특히 가치가 없습니다.

작성 시점에서는 다시 시도할 실패를 추적하는 명확한 방법이 없습니다. Rule 다시 시도 정책을 구성할 수 없는 경우에는 특히 문제가 됩니다. 즉, Rule이 최대 24시간까지 다시 시도할 수 있고 이 간격을 줄일 수 없는 경우, 우리는 하류 소비자의 메트릭에 의존하지 않고 24시간 동안 실패를 쉽게 관찰할 수 없을 수 있습니다.

<div class="content-ad"></div>

# 성공적인 실행 추적

실패 사항을 다시 시도하는 것을 보려고 노력할지도 모르지만, 성공적인 Rule 호출을 모니터링하기 위해 기존 메트릭을 사용할 수 있지만, 조금의 작업이 필요합니다. Metric SuccessfulInvocationAttempts가 이상적으로 보일 수 있지만, RetryInvocationAttempts와 동일한 문제가 있습니다: 특정 Rule 이름으로 이 메트릭을 분류하는 데 사용할 수 있는 차원이 현재 작성 시점에서 없기 때문에, 귀하의 계정이 하나의 Rule만 있는 경우를 제외하고는 제한된 가치를 제공합니다.

![이미지](/assets/img/2024-09-10-MonitoringAmazonEventBridgeRules_3.png)

다음으로 성공 및 영구적으로 실패한 호출을 모두 세는 Invocations로 전환합니다. 후자의 카운트를 제외하려면, CloudWatch 메트릭 수학을 활용하여 FailedInvocations을 Invocations에서 빼고 결과를 성공적인 호출의 카운트로 취할 수 있습니다.

<div class="content-ad"></div>

아직 다루지 않은 몇 가지 기타 호출 메트릭이 있습니다. 이에는 InvocationAttempts 및 InvocationsCreated가 포함되는데, 다시 말해서 특정 Buses 또는 Rules로 필터링할 수 없으므로 그 값은 별다른 가치가 없습니다. 또한 AWS에서 다음과 같이 정의하는 DeadLetterInvocations도 있습니다:

이 메트릭은 추가 설명이 필요합니다.

## Dead Letter Invocations

Rules에서는 대상이 영구적으로 호출되지 못하는 이벤트에 대한 SQS Dead Letter Queue 통합을 지원합니다. 따라서 DeadLetterInvocations는 실패한 이벤트가 얼마나 자주 큐로 전송되는지를 보여줄까요? 처음 생각과는 달리, 그렇지 않습니다. 대신 InvocationsSentToDlq 및 InvocationsFailedToBeSentToDlq가 DLQ 통합 동작을 모니터링하는 데 사용됩니다.

<div class="content-ad"></div>

아마도 이름으로는 명확하지 않을 수 있지만, DeadLetterInvocations는 Rule이 자체를 호출할 때 강조됩니다. 이를 설명하기 위해 다음 예를 살펴보세요:

- 이벤트 버스 my-event-bus가 Rule인 my-rule로 설정됩니다.
- my-rule은 모든 이벤트에 대해 람다 함수 my-function을 호출하도록 구성됩니다.
- my-function은 EventBridge SDK를 사용하여 이벤트를 my-event-bus에 넣습니다.

![이미지](/assets/img/2024-09-10-MonitoringAmazonEventBridgeRules_4.png)

우리는 무한 루프에 빠졌어요! my-function은 자신이 호출된 이벤트 버스에 이벤트를 넣습니다. 개입이 없으면 Rule이 무기한 실행되어 AWS 계정 소유자에게 큰 비용을 발생시킬 수 있습니다. AWS는 서비스 간 루프를 감지하고 자동으로 Rule을 비활성화하여 DeadLetterInvocations 메트릭을 생성합니다.

<div class="content-ad"></div>

# 메트릭 개선 제안

이 글에서 우리는 Rule 메트릭을 전체적으로 다루지는 않았습니다. PutEvents 및 PutPartnerEvents API 요청을 통해 더 탐구할 가치 있는 다양한 옵션이 있지만, 블로그 포스트의 범위를 벗어났습니다.

우리가 다룬 내용을 기반으로, EventBridge는 많은 유용한 메트릭을 제공하지만, 더 많은 개선 여지가 있습니다. 특히 계정 전체 숫자만 제공하는 메트릭에 대한 차원(Dimensions) 지원은 환영받을 만합니다. 예를 들어, Dimensions로 EventBusName 및 RuleName이 RetryInvocationAttempts에 대해 구체적인 규칙을 사용하여 재시도될 호출을 검토하는 데 매우 가치가 있을 수 있습니다. 재시도 정책 간격동안 규칙당 메트릭이 없는 것은 프로덕션에서 규칙을 활용하는 시간이 중요한 서비스에서 심각한 고려사항이어야 합니다.

또 다른 반복되는 주제는 메트릭 명명입니다. 명명하기 어렵습니다! 이는 좀 더 주관적이지만, 고려해야 할 몇 가지 사례가 있습니다. 먼저, 호출은 InvocationAttempts와 혼동될 수 있습니다. 이 두 메트릭 간에 차이점이 명확하지 않습니다. 아마도 전자를 InvocationAttemptsWithoutRetry 또는 비슷한 이름으로 변경하는 것이 더 나을 수 있습니다. 왜냐하면 이는 성공과 영구 실패를 다루고 있기 때문일 수 있습니다. 둘째, DeadLetterInvocations의 이름을 재검토하는 것도 좋은 아이디어일 수 있습니다. Lambda가 재귀 검출 출시를 하면서, 이 메트릭을 RecursiveInvocationAttempts와 같이 명명할 수도 있고 또는 DiscardedInvocationAttempts와 같이 더 일반적인 이름을 사용할 수도 있을 것입니다.

<div class="content-ad"></div>

EventBridge는 훌륭한 서비스이지만 Rules를 관찰하는 방법을 개선할 여지가 남아 있습니다. 위 제안을 참고하여 궁금한 사항이나 더 탐구해 볼 제안 사항이 있으면 언제든지 연락해 주세요. Rules와 밀접하게 작업하시는 경우, 관찰 가능성과 개선이 가능한 부분에 대한 여러분의 생각을 듣는 게 좋을 것 같습니다. 이 블로그 글을 읽어주셔서 감사합니다. 참고용으로 아래 추가 독서 자료를 공유해 드렸습니다.

# 추가 독서 자료

- Amazon에 의한 Amazon EventBridge 모니터링.
- Amazon에 의한 Rule 모니터링을 위한 모범 사례.
- Amazon에 의한 EventBridge용 향상된 CloudWatch 지표.