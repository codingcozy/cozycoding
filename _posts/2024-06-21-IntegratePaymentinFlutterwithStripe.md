---
title: "Flutter에서 Stripe를 사용하여 결제 통합하는 방법"
description: ""
coverImage: "/assets/img/2024-06-21-IntegratePaymentinFlutterwithStripe_0.png"
date: 2024-06-21 22:19
ogImage: 
  url: /assets/img/2024-06-21-IntegratePaymentinFlutterwithStripe_0.png
tag: Tech
originalTitle: "Integrate Payment in Flutter with Stripe"
link: "https://medium.com/@Ikay_codes/integrate-payment-in-flutter-with-stripe-13e96fdc2e9e"
isUpdated: true
---





![이미지](/assets/img/2024-06-21-IntegratePaymentinFlutterwithStripe_0.png)

안녕하세요 여러분, 제가 최근에 글을 올린 지 꽤 오랜 시간이 지났죠. 지난 몇 달 동안 저는 많은 새로운 경험들로 가득했고, 그 모든 것을 해내려고 노력했습니다. 하지만 글을 오랫동안 쓰지 않았다는 것을 깨달았고, 이번 11월에 적어도 두 개의 글을 쓰기로 다짐하고 앞으로는 더 꾸준해지기로 결심했습니다. 제가 이를 통해 모든 독자들에게 책임감을 느끼도록 허락해주세요.

오늘의 글은 플러터(Flutter)에서 Stripe를 이용한 결제 처리에 관한 것입니다. 최근에 어느 회사에서 나에게 모바일 앱의 여러 기능을 매우 짧은 기간 내에 작업하도록 계약을 맺었습니다. 그 기능 중 하나가 결제 기능이었고, 물론 그들은 Stripe를 사용하길 원했습니다. 저는 반면에 플러터에 Stripe를 통합한 경험이 전혀 없었지만, 도전에서 도망가는 걸 절대 못 볼 것 같아 이 일을 받아들이고 조사에 착수했습니다. 이 지식을 여러 곳에서 모았지만, 이 글을 최대한 정보성 있게 만드는 데 노력하겠습니다. 이 주제에 대한 답변을 찾기 위해 다른 곳을 더 이상 찾아다니지 않아도 되도록 하겠습니다. 긴 이야기는 여기까지, 이제 시작해보죠.

## Stripe:

<div class="content-ad"></div>

먼저, 이 멋진 결제 플랫폼인 Stripe에 대해 간단히 소개할게요. Stripe는 Shopify, Instacart 같은 수백만 개의 기업에서 사용하는 결제 게이트웨이로, Google, Amazon과 같은 대규모 기술 기업도 사용하고 있어요. 이들은 전 세계 기업 및 그들의 고객들이 결제를 받고 보내는 간단하고 편리한 프로세스를 제공해요. 가상 및 실물 카드 발급, 반복 결제 처리, 재무에 대한 지능적이고 다이어그래틱한 분석 및 보고서, 송장 생성 등과 같은 추가 서비스도 제공하고 있어요.

Stripe를 개발자로서 흥미롭게 느낀 점 중 하나는 다양한 스택에 쉽게 통합되며 이를 지원하는 라이브러리, 그리고 Apple 및 Google 페이와 같은 네이티브 결제를 지원한다는 것이에요.

## 시작하기

시작하려면, 먼저 Stripe에서 API 키를 생성해야 해요. 이를 위해 Stripe 계정을 생성해야 해요. 그러고 나서 대시보드에 로그인하여 테스트 모드를 활성화하고 통합 및 테스트를 위해 개발자 `API Keys`로 이동하여 API 키(공개 및 비밀 키)를 확인하세요.

<div class="content-ad"></div>


![이미지](/assets/img/2024-06-21-IntegratePaymentinFlutterwithStripe_1.png)

## 환경 설정하기

의존성에 다음 패키지들을 추가하세요:

```yaml
flutter_stripe: ^5.0.0
flutter_dotenv: ^5.0.2
http: ^0.13.5
```

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해주세요.

<div class="content-ad"></div>


![이미지](/assets/img/2024-06-21-IntegratePaymentinFlutterwithStripe_2.png)

Stripe 비밀 키를 .env 파일에 다음과 같이 넣어주세요:

```js
STRIPE_SECRET = sk_test_39Aops3ZvLRexxxxxxxxxxx
```

pubspec.yaml에 .env 경로를 자산에 추가해주세요.


<div class="content-ad"></div>


assets:
- assets/.env


귀하의 .env 파일을 .gitignore에 추가하십시오.


#Dotenv
.env


main.dart


<div class="content-ad"></div>

저희 앱은 단순한 홈 페이지와 결제 모달이 표시되는 버튼이 있는 페이지가 있어요.

이곳은 우리의 결제 기능입니다:

세 가지 간단한 단계로 나누어 보겠습니다.

1단계: 결제 의도 생성 - 우리는 지불할 금액과 통화를 정의하는 createPaymentIntent 함수로 지불 의도를 만들어 시작합니다.

<div class="content-ad"></div>

우리는 화폐와 금액을 100으로 곱한 후 Flutter_stripe에 의해 double로 변환될 때 값이 유지되도록 몸체를 포함한 Stripe에 POST 요청을 보냅니다. 그에 대한 응답으로 Stripe는 결제 의도를 다시 보내줍니다. STEP 2에서 이를 사용하여 우리의 지불 시트를 초기화할 것입니다.

## STEP 2: 지불 시트 초기화

7~15행에서 makePayment 함수에서 지불 시트를 초기화합니다. 여기에는 우리가 카드 세부 정보를 기입하고 지불할 지불 시트 모달을 생성할 것입니다.

우리는 이전 단계에서 결제 의도로부터 얻은 client_secret를 전달합니다. 여기에서는 스타일을 포함한 다양한 매개변수에 액세스할 수 있습니다. 이를 통해 우리는 어두운 테마를 선택할 수 있습니다!

<div class="content-ad"></div>

## 단계 3: 결제 시트 표시하기

마지막 단계는 모달 시트를 표시하는 것입니다.

이것으로 세 번째이자 마지막 단계가 끝났습니다.

팁: 결제가 성공하였을 때 또는 결제 처리 중에 오류가 발생한 경우와 같이 사용자에게 추가 응답을 알림 대화상자 형식으로 추가할 수 있습니다.

<div class="content-ad"></div>

끝났어요! 플러터 애플리케이션에 Stripe 통합이 완료되었습니다.

앱을 론칭할 준비가 되셨다면 대시보드에서 테스트 모드에서 라이브 모드로 전환하고 Stripe의 지시에 따라 진행하면 됩니다.

질문이나 의견이 있으시면 언제든지 알려주세요 💬, 이 기사가 유용했다면 👏👏을 눌러주시기를 잊지 마세요.

여기서 전체 소스 코드를 구할 수 있어요.