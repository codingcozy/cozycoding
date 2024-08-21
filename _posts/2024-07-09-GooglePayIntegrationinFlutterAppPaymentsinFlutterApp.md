---
title: "Flutter 앱에 Google Pay 통합하는 방법  Flutter 앱에서 결제 기능 구현하기"
description: ""
coverImage: "/assets/img/2024-07-09-GooglePayIntegrationinFlutterAppPaymentsinFlutterApp_0.png"
date: 2024-07-09 22:11
ogImage: 
  url: /assets/img/2024-07-09-GooglePayIntegrationinFlutterAppPaymentsinFlutterApp_0.png
tag: Tech
originalTitle: "Google Pay Integration in Flutter App | Payments in Flutter App"
link: "https://medium.com/@sachin.dev2910/google-pay-integration-in-flutter-app-payments-in-flutter-app-c85f1c7e260e"
isUpdated: true
---




이 기사에서는 몇 가지 간단한 단계로 Google Pay를 Flutter 앱에 통합하는 방법을 알아볼 것입니다.

# 단계 1: 종속성 추가

먼저 pub.dev에 방문하여 pay 패키지를 검색하고 다음 종속성을 pubspec.yaml 파일에 추가하여 프로젝트에 추가하세요:

```yaml
dependencies:
  pay: ^2.0.0
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-GooglePayIntegrationinFlutterAppPaymentsinFlutterApp_0.png" />

# 단계 2: 비즈니스 계정 설정하기

- Google Pay 비즈니스 콘솔에 가입하고 계정을 생성하세요.
- 계정을 설정하고 비즈니스 프로필이 승인될 때까지 기다리세요.

<img src="/assets/img/2024-07-09-GooglePayIntegrationinFlutterAppPaymentsinFlutterApp_1.png" />

<div class="content-ad"></div>

# 단계 3: Gradle 구성 수정

앱 수준의 build.gradle 파일에서 minSdkVersion을 21로 변경하세요:

![이미지](/assets/img/2024-07-09-GooglePayIntegrationinFlutterAppPaymentsinFlutterApp_2.png)

# 단계 4: 결제 구성 파일 추가

<div class="content-ad"></div>

/assets 폴더에 gpay.json 파일을 생성하고 아래 내용을 넣어주세요:

```js
{
  "provider": "google_pay",
  "data": {
    "environment": "TEST",
    "apiVersion": 2,
    "apiVersionMinor": 0,
    "allowedPaymentMethods": [
      {
        "type": "CARD",
        "tokenizationSpecification": {
          "type": "PAYMENT_GATEWAY",
          "parameters": {
            "gateway": "example",
            "gatewayMerchantId": "gatewayMerchantId"
          }
        },
        "parameters": {
          "allowedCardNetworks": [
            "VISA",
            "MASTERCARD"
          ],
          "allowedAuthMethods": [
            "PAN_ONLY",
            "CRYPTOGRAM_3DS"
          ],
          "billingAddressRequired": true,
          "billingAddressParameters": {
            "format": "FULL",
            "phoneNumberRequired": true
          }
        }
      }
    ],
    "merchantInfo": {
      "merchantId": "본인의상세내용에따라수정",
      "merchantName": "본인의상세내용에따라수정"
    },
    "transactionInfo": {
      "countryCode": "US",
      "currencyCode": "USD"
    }
  }
}
```

merchantId 및 merchantName과 같은 필수 변경 사항을 수정해주세요.

# 단계 5: 결제 구성 초기화

<div class="content-ad"></div>

플러터 프로젝트에서 지불 구성을 초기화하세요:

```js
late final Future<PaymentConfiguration> _googlePayConfigFuture;

@override
void initState() {
  super.initState();
  _googlePayConfigFuture = PaymentConfiguration.fromAsset('gpay.json');
}
```

# 단계 6: 사용자 인터페이스 구현

Google Pay 버튼을 통합하기 위해 UI에 집중하세요. 아래는 완전한 코드입니다:

<div class="content-ad"></div>

```js
import 'package:flutter/material.dart';
import 'package:pay/pay.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  late final Future<PaymentConfiguration> _googlePayConfigFuture;

  @override
  void initState() {
    super.initState();
    _googlePayConfigFuture = PaymentConfiguration.fromAsset('gpay.json');
  }

  void onGooglePayResult(paymentResult) {
    debugPrint(paymentResult.toString());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: // 예시 지불 버튼은 자산을 사용하여 구성됩니다
          FutureBuilder<PaymentConfiguration>(
              future: _googlePayConfigFuture,
              builder: (context, snapshot) => snapshot.hasData
                  ? Center(
                    child: GooglePayButton(
                        paymentConfiguration: snapshot.data!,
                        paymentItems: const [
                          PaymentItem(
                            label: 'Total',
                            amount: '10.00',
                            status: PaymentItemStatus.final_price,
                          ),
                        ],
                        type: GooglePayButtonType.buy,
                        margin: const EdgeInsets.only(top: 15.0),
                        onPaymentResult: onGooglePayResult,
                        loadingIndicator: const Center(
                          child: CircularProgressIndicator(),
                        ),
                      ),
                  )
                  : const SizedBox.shrink()),
    );
  }
}
```

# Step 7: 폴더 구조

assets 폴더를 포함하고 있는 폴더 구조를 확인하고 pubspec.yaml 파일에 참조되었는지 확인하세요:

```js
flutter:
  assets:
    - assets/gpay.json
```

<div class="content-ad"></div>

# 결론:-

이러한 단계를 따르면 플러터 앱에 Google Pay를 쉽게 통합하여 사용자에게 원활한 결제 경험을 제공할 수 있습니다. 오류가 발생하면 댓글에서 자유롭게 질문해 주세요.

나에 대해:

저는 Sachin Choudhary입니다. 거의 2년간 플랫폼 전체에서 원활하고 효율적인 사용자 경험을 만드는 데 전념하는 열정적인 플러터 개발자입니다. 새로운 도전에 맞서고 혁신적인 아이디어를 현실로 구현하는 것을 즐깁니다. 귀하의 프로젝트를 성공으로 이끌 신뢰할 수 있는 플러터 개발자를 찾고 계시다면 함께 협업하여 귀하의 비전을 성취해 봅시다!

<div class="content-ad"></div>

✨ 제 LinkedIn 팔로우 해주세요

✨ 제 X 팔로우 해주세요