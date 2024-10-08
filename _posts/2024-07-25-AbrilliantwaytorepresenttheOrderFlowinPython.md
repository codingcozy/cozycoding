---
title: "Python을 사용하여 주문 흐름을 효과적으로 표현하는 놀라운 방법"
description: ""
coverImage: "/assets/img/2024-07-25-AbrilliantwaytorepresenttheOrderFlowinPython_0.png"
date: 2024-07-25 11:50
ogImage: 
  url: /assets/img/2024-07-25-AbrilliantwaytorepresenttheOrderFlowinPython_0.png
tag: Tech
originalTitle: "A brilliant way to represent the Order Flow in Python"
link: "https://medium.com/@lu.battistoni/a-brilliant-way-to-represent-the-order-flow-in-python-fb96318e1070"
isUpdated: true
---




## 소개

이 기사에서는 Python에서 주문 플로우를 시각적인 방식으로 표현하는 통찰력 있는 방법을 보여드리겠습니다. 이 방법을 알고리즘 및 고빈도 거래, Álvaro Cartea(2015)를 읽다가 알게 되었고, 바로 매혹을 받았습니다. 이 표현은 주문 대장의 진화와 가격 변동을 직관적으로 설명해줍니다.

이 기사에서 사용한 Python 코드를 제공하겠습니다. 게다가, 기사 마지막에는 내 GitHub 저장소와 흥미로운 다른 기사들의 링크도 제공할 예정입니다.

표현 방법에 대해 깊이 알아보기 전에 간단한 질문을 해보겠습니다.

<div class="content-ad"></div>

## 주문 플로우란 무엇이며 왜 중요한가요?

주문부는 가격 수준에 따라 구성된 특정 금융 상품의 매수 및 매도 주문 목록입니다. 실시간으로 자산의 수요와 공급을 추적하는 데 사용됩니다. 부록에 포함된 가장 중요한 정보는 다음과 같습니다:

- 매수가: 구매자가 자산을 사려는 가격입니다. 매수가는 내림차순으로 나열되며, 가장 높은 매수가(최상의 매수)가 맨 위에 표시됩니다.
- 매도가: 판매자가 자산을 받아들일 의향이 있는 가격입니다. 매도가는 오름차순으로 나열되며, 가장 낮은 매도가(최상의 매도)가 맨 위에 표시됩니다.
- 주문 크기: 각 매수 및 매도가격에 대해 주문부는 그 가격에서 거래하려는 자산의 양을 보여줍니다.

주문부는 현재 시장 상태에 대한 통찰을 제공하여 참여자가 체계적인 거래 결정을 내릴 수 있도록 합니다(예: 주문부의 매수 및 매도 측면 간의 균형은 시장 센티먼트와 자산의 유동성을 나타낼 수 있습니다).

<div class="content-ad"></div>

주문 흐름(Order Flow)은 주문북(Order Book) 스냅샷의 연속입니다. 이를 통해 시장 역학이 시간에 따라 어떻게 변하는지 이해할 수 있고 이로부터 보안 가격이 미래에 어디에 있을지 알 수 있습니다. 주문북과 주문 흐름에 대한 심층적인 이해는 고빈도 거래에 접근하는 데 중요합니다.

## 주문북의 일반적인 표현

한 번 주문북을 본 적이 있다면, 중개업체 및 기타 소스에서 사용하는 보통의 표현은 다음과 같습니다:

![Order Flow](/assets/img/2024-07-25-AbrilliantwaytorepresenttheOrderFlowinPython_0.png)

<div class="content-ad"></div>

마음씨 좋은 개발자에요. 위에 말씀하신 내용을 한국어로 번역해드릴게요!

구매 주문(Bid)은 초록색으로, 판매 주문(Ask)은 빨간색으로 표시됩니다. 수준별 가격은 x축에, 거래량은 y축에 표시됩니다. 그래프는 각 가격 수준에서 Bid와 Ask의 누적 거래량을 나타냅니다.

가장 높은 Bid와 가장 낮은 Ask 사이의 흰 공간은 Bid-Ask Spread입니다. 점선은 Mid Price를 나타내며, 이는 Bid-Ask Spread의 중간 지점을 의미합니다. 일부 소스에서는 이 가격을 해당 증권의 이론적 효율가로 사용합니다.

실제로 이는 상당히 이해하기 쉬운 표현입니다. 수요와 공급 사이에 불균형이 있는 것을 볼 수 있고, 이 정보를 활용하여 미래 거래가 어디에서 발생할지에 대한 아이디어를 형성할 수 있습니다. 문제는 이것이 주문 흐름의 스냅샷이라는 것입니다. 이것은 단일 밀리초 스냅샷이며, 순간순간 주문이 매우 빠르게 변합니다. 이 정보는 매우 빠르게 오래되어 더 이상 유효하지 않게 됩니다.

그렇다면 주문 흐름의 표현은 어떻게 되나요? 일반적인 중개업체와 금융 사이트에서 흔히 볼 수 있는 표현들은 무엇인가요?

<div class="content-ad"></div>

보통 두 가지 종류의 표현을 찾을 수 있습니다. 주문 플로우 차트:

![Order Flow Chart](/assets/img/2024-07-25-AbrilliantwaytorepresenttheOrderFlowinPython_1.png)

이전과 마찬가지로 입찰 및 요청 주문이 있으며 거래량이 있습니다. 각 시간 단계마다 새로운 스냅샷이 그래프에 추가됩니다. 이 그래프를 유용하게 사용할 수도 있지만, 저는 그렇게 생각하지 않습니다.

또 다른 사용되는 플롯은 열지도입니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-25-AbrilliantwaytorepresenttheOrderFlowinPython_2.png" />

입찰(Bid), 요청(Asks) 및 그들의 양을 크기가 다른 공으로 나타냈습니다. 이것은 이전 그림보다 혼동이 적지만 실시간 거래에서 해독하기에는 여전히 꽤 복잡합니다.

## 대안 표현

이제 대안 표현을 보여드릴게요 (Cartea가 제안한 것):

<div class="content-ad"></div>

<img src="/assets/img/2024-07-25-AbrilliantwaytorepresenttheOrderFlowinPython_3.png" />

여기에는 보통 시간을 x 축에, 가격을 y 축에 표시했습니다. 시간은 어떤 것이든 나타낼 수 있으며, 우리는 이것들이 매 밀리초마다 촬영된 주문서의 스냅숏이라고 가정할 수 있습니다.

빨간색은 매수(Bid)를 나타내며, 각기 다른 가격 수준이 있습니다. 파란색은 매도(Ask)를 나타냅니다. 각 수준의 막대 그래프는 해당 수준의 거래량을 나타냅니다. 가장 높은 매수와 가장 낮은 매도 사이의 노란색 영역은 매수-매도 스프레드(Bid-Ask Spread)입니다.

주황색 선은 가격을 나타냅니다. 이는 마지막 거래가 발생한 가격입니다. 각 거래는 검은 원으로 나타내며, 판매 거래라면 빨간색, 매수 거래라면 초록색입니다. 원이 클수록 거래의 거래량이 더 큽니다.

<div class="content-ad"></div>

내 의견으로는, 이 표현은 이전 것들보다 훨씬 유용합니다. 시간이 지남에 따라 스프레드가 어떻게 변하는지, 가격이 어떻게 변하는지, 그리고 어디에 매수 또는 매도 압력이 있는지 시각화할 수 있습니다.

실시간으로 세 가지 다른 표현을 보는 상상을 해보세요. 그래프가 업데이트되고 새 값이 추가될 때, 어떤 표현이 더 유용하다고 생각하시나요? 세 번째 표현에 대한 생각은 무엇인가요? 이 그래프를 더 개선할 제안이 있으신가요?

여기서 그래프를 생성하는 코드와 Order Book 스냅샷을 올바른 형식으로 넣는 지침을 포함한 노트북을 찾으실 수 있습니다. 궁금한 점이 있으시면 언제든지 연락해 주세요!

## 더 많은 읽을거리

<div class="content-ad"></div>

주문서와 플로우에 대해 더 알고 싶다면 Cartea 책이나 Hasbrouck의 Empirical Market Microstructure을 읽어보는 것을 권장합니다.

이런 주제에 관심이 있다면, 저의 기사를 확인해보시기 바랍니다. 주문서에서 가치를 추출하는 방법을 지속적으로 연구하고 다루고 있습니다.

질문이나 제안이 있으면 언제든지 댓글을 남기거나 연락해 주세요 (내 연락처는 제 GitHub readme에서 찾을 수 있습니다).