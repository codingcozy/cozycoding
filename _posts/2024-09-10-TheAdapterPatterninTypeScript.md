---
title: "TypeScript에서의 어댑터 패턴 이해와 활용법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-09-10 18:15
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "The Adapter Pattern in TypeScript"
link: "https://medium.com/itnext/the-adapter-pattern-in-typescript-c0fdb33183a5"
isUpdated: false
---


어댑터 패턴은 구조적 디자인 패턴으로, 호환되지 않는 인터페이스를 함께 작동하도록 하는 "래퍼" 또는 "어댑터"를 기존 클래스 주위에 제공함으로써 가능하게 합니다. 아래는 TypeScript에서 어댑터 패턴의 실제 예제인데, 기존 전자상거래 애플리케이션에 새로운 결제 게이트웨이를 통합하고자 할 때 사용됩니다.

# 시나리오:

기존 PayPalPaymentProcessor를 사용하여 결제를 처리하는 전자상거래 애플리케이션을 개발 중이라고 가정해보겠습니다. Stripe와 같은 다른 결제 제공업체를 지원하고자 하며, 새로운 결제 게이트웨이를 구현해야 하는 요구사항이 있습니다. 이를 StripePaymentProcessor라고 지칭하겠으나, 이 인터페이스는 기존 PayPalPaymentProcessor와 다릅니다. 새로운 결제 게이트웨이를 수용하기 위해 전체 애플리케이션을 변경하지 않으려면 어댑터 패턴을 사용할 수 있습니다.

# 단계 1: 대상 인터페이스 정의

<div class="content-ad"></div>

결제 처리기 인터페이스는 응용 프로그램이 이해하는 대상 인터페이스입니다.

```js
인터페이스 PaymentProcessor {
    pay(금액: number): void;
}
```

# 단계 2: 기존 PayPal 결제 처리기

이것은 응용 프로그램이 이미 사용 중인 기존 결제 처리기입니다. 보시다시피 PayPalPaymentProcessor는 우리의 PaymentProcessor 대상 인터페이스의 pay() 메서드를 구현하고 있습니다. 모든 결제 처리기 클래스가 구현할 것입니다.

<div class="content-ad"></div>

```js
class PayPalPaymentProcessor implements PaymentProcessor {
    public pay(amount: number): void {
        console.log(`PayPal을 사용하여 $${amount}을(를) 지불했습니다.`);
    }
}
```

# 단계 3: 새로운 Stripe 지불 처리기

이것은 통합하려는 새로운 지불 처리기입니다. 그러나 이는 PayPalPaymentProcessor에 없는 새로운 메서드인 makePayment()을 포함한 다른 인터페이스를 갖고 있습니다.

```js
class StripePaymentProcessor {
    public makePayment(amountInCents: number): void {
        console.log(`Stripe를 사용하여 $${amountInCents / 100}을(를) 지불했습니다.`);
    }
}
```

<div class="content-ad"></div>

# 단계 4: 어댑터 생성하기

우리 애플리케이션에서 이 작업을 수행하려면 어댑터를 구현해야 합니다. 이 새로운 어댑터 클래스는 대상 인터페이스(PaymentProcessor)를 구현하고 새로운 StripePaymentProcessor를 래핑합니다. 기본적으로 StripePaymentProcessor의 인터페이스를 애플리케이션이 이해하는 인터페이스로 변환합니다.

```js
class StripePaymentAdapter implements PaymentProcessor {
    private stripePaymentProcessor: StripePaymentProcessor;

    constructor(stripePaymentProcessor: StripePaymentProcessor) {
        this.stripePaymentProcessor = stripePaymentProcessor;
    }

    public pay(amount: number): void {
        // 달러를 센트로 변환하고 Stripe 결제 메소드 호출
        this.stripePaymentProcessor.makePayment(amount * 100);
    }
}
```

# 단계 5: 어플리케이션에서 어댑터 사용하기

<div class="content-ad"></div>

지금 애플리케이션은 기존 코드베이스를 변경하지 않고 어댑터를 통해 StripePaymentProcessor를 사용할 수 있습니다.

```js
// 애플리케이션 코드
const amountToPay = 100; // $100

// 기존 PayPal 결제 프로세서 사용
const paypalProcessor: PaymentProcessor = new PayPalPaymentProcessor();
paypalProcessor.pay(amountToPay);

// 어댑터를 통해 새로운 Stripe 결제 프로세서 사용
const stripeProcessor: PaymentProcessor = new StripePaymentAdapter(new StripePaymentProcessor());
stripeProcessor.pay(amountToPay);
```

결과:

```js
PayPal을 사용하여 $100을 지불했습니다.
Stripe를 사용하여 $100을 지불했습니다.
```

<div class="content-ad"></div>

# 중요한 포인트 이해하기

- PaymentProcessor 인터페이스는 두 결제 처리기가 준수해야 하는 대상 인터페이스입니다.
- PayPalPaymentProcessor 클래스는 이미이 인터페이스를 구현했습니다.
- StripePaymentProcessor 클래스는 다른 인터페이스를 갖고 있으므로 StripePaymentAdapter를 PaymentProcessor 인터페이스로 적응시킵니다.
- 어댑터 패턴 덕분에 응용 프로그램은 기존 코드를 수정하지 않고 두 결제 처리기를 모두 사용할 수 있습니다.

## 더 DRY하게 만들어봅시다

```js
function processPayment(paymentProcessor: PaymentProcessor, amount: number): void {
    paymentProcessor.pay(amount);
}

// 예시 사용법
const amountToPay = 100; // $100

// 기존 PayPal 결제 처리기 사용
const paypalProcessor: PaymentProcessor = new PayPalPaymentProcessor();
processPayment(paypalProcessor, amountToPay);

// 어댑터를 통해 새 Stripe 결제 처리기 사용
const stripeProcessor: PaymentProcessor = new StripePaymentAdapter(new StripePaymentProcessor());
processPayment(stripeProcessor, amountToPay);
```  

<div class="content-ad"></div>

애플리케이션에서 어댑터 패턴을 적용하는 실제 사용 사례가 있습니다. 보다 복잡한 코드에 통합되겠지만, 어댑터 패턴을 고려해야 할 시나리오에 대한 통찰력을 제공합니다.

어댑터 패턴을 사용하는 경우:

- 기존 코드: 기존 코드를 수정하지 않고 새로운 시스템 또는 라이브러리를 기존 코드베이스에 통합하는 경우.
- 제3자 라이브러리: 제3자 라이브러리나 API를 감싸서 애플리케이션과 호환되도록 만드는 경우.
- 다중 구현: 클라이언트 코드를 수정하지 않고 서비스 (예: 결제 처리기)의 여러 구현을 지원하는 경우.
- 단일 책임: 기존 클래스로부터 데이터 변환 또는 메서드 적응의 책임을 제어 하는 경우.

어댑터 패턴은 서로 다른 인터페이스 간의 간격을 줄이고 원활한 통합을 가능하게 하며 광범위한 리팩터링이 필요 없게 하는 데 도움이 됩니다.

<div class="content-ad"></div>

# 읽어 주셔서 감사합니다! 궁금한 점이 있으시면 댓글로 물어보세요.