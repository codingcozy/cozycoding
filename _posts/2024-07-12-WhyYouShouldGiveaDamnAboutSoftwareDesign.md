---
title: " 소프트웨어 디자인에 신경 써야 하는 이유"
description: ""
coverImage: "/assets/img/2024-07-12-WhyYouShouldGiveaDamnAboutSoftwareDesign_0.png"
date: 2024-07-12 22:36
ogImage: 
  url: /assets/img/2024-07-12-WhyYouShouldGiveaDamnAboutSoftwareDesign_0.png
tag: Tech
originalTitle: "Why You Should Give a Damn About Software Design"
link: "https://medium.com/gitconnected/why-you-should-give-a-damn-about-software-design-2a6ddbf3c242"
isUpdated: true
---





<img src="/assets/img/2024-07-12-WhyYouShouldGiveaDamnAboutSoftwareDesign_0.png" />

소프트웨어 엔지니어링을 혐오스럽게 만드는 것은 두 가지가 있습니다. 나쁜 팀 문화와 나쁜 디자인으로 인한 기술 부채입니다. 코드를 이해하지 못해서 내 개인 프로젝트를 버린 횟수를 알 수 없어요. 매번 어지러운 WET 코드 뭉치와 함께 작업하는 것이 얼마나 답답한지.

정말 속이 메스꺼워요.

나쁜 코드를 쓰는 것은 그냥 인간의 본성 중 일부에 불과합니다. 때로는 우리는 어떤 것을 잊어버리고, 새로운 아이디어가 스며들어오고, 그러다가 다른 일에 새로 모해져 버리기도 합니다. 소프트웨어 프로젝트의 구조를 전혀 생각하지 않고 큰 기술 부채로 이어질 수 있는 작은 경고 신호를 무시하는 것입니다.


<div class="content-ad"></div>

이 문서는 새로 오신 개발자와 엔지니어들이 소프트웨어 디자인 패턴을 고려해야 하는 이유를 이해하는 데 도움이 되도록 제작된 오버뷰입니다.

# 내 깨달음

전통적이지 않은 배경을 가진 개발자로써 소프트웨어 엔지니어링의 "왜"를 이해하는 것은 어려울 수 있습니다. 어떤 어플리케이션 작동 원리에 대해 질문했을 때, 아주 좋은 설명을 받았지만 왜 특정 솔루션이 사용되는지 이해할 필요도 있었던 기억이 납니다.

그 외의 질문들도 있을 수 있습니다...

<div class="content-ad"></div>

프로젝트가 이렇게 구성된 이유가 뭐에요?

왜 클래스를 이렇게 작성했어요?

디지털 헬스케어 기관에서 일할 때, 엔지니어들과 직접 협업하는 기회가 있었어요. 그들은 팩토리 디자인 패턴을 사용했고, 이는 디자인 패턴에 매우 새로운 엔지니어들에게 좋은 시작점이라고 생각해요.

또한, 나중에 부끄러워하지 않기 위해 질문을 하기도 해요. 질문을 하는 것은 가장 좋은 학습 방법이에요. 왜냐하면 아무도 당신이 모르는 것을 알지 못하기 때문이거든.

<div class="content-ad"></div>

# "지금은 전혀 이해가 안 가지만 완전히 이해했다고 생각했어요!"

나는 나의 매니저가 첫 번째로 Factory 디자인 패턴을 설명한 후에 이렇게 말했다. 이 패턴으로 코딩에 익숙해지고 나서, 개발 속도가 빨라지고, 테스트와 디버깅이 덜 괴로워졌으며, 더 적은 양의 코드로 기능을 추가하거나 삭제할 수 있었습니다. 좋은 소프트웨어 디자인은 개발자들이 복잡한 문제를 더 작고 관리하기 쉬운 조각들로 나누도록 돕습니다.

좋은 소프트웨어 디자인은 문제가 발생하기 전에 해결합니다.

단일 구성 요소 애플리케이션 및 소규모 프로젝트의 경우, 소프트웨어 디자인은 실제로 문제가 되지 않습니다.

<div class="content-ad"></div>

대규모 팀으로 일하거나 대규모 프로젝트에서 작업하기 시작할 때 이해하기 어려운 몇 가지 문제가 있습니다. 마치 누군가가 그들의 차량이 약 12만~15만 마일에서 문제를 겪기 시작할 때 합성 오일이 엔진에 얼마나 더 좋은지 보지 못하는 것처럼요.

# 팩토리 디자인 패턴

팩토리 디자인 패턴은 객체를 더 유연하고 제어된 방식으로 생성할 수 있도록 하는 프로그래밍 개념입니다.

가게를 위해 많은 제품을 만들어야 하는데, 각 객체는 일부 조건에 따라 다르게 생성됩니다. 예를 들어 자동차를 만들고 있다고 상상해보세요. 모든 자동차는 적어도 4개의 바퀴, 연료 탱크, 엔진 등을 필요로 할 것이지만, 각 자동차는 고유한 색상, 모양, 연도, 모델을 갖게 됩니다. 각 자동차를 완전히 처음부터 만드는 대신, 어떻게 각 자동차를 설계해야 하는지 정확히 결정하는 청사진을 만들 수 있습니다.

<div class="content-ad"></div>

계속해서 처음부터 다시 시작할 필요 없어요.

공장에는 몇 가지 매개변수를 입력받아 그 매개변수에 따라 적절한 객체를 생성하고 반환하는 메서드가 있어요. 이렇게 하면 여러 객체를 쉽게 생성할 수 있고, 전체 프로그램을 변경하는 대신 공장의 메서드를 변경하여 객체 생성 방식을 변경할 수 있어요.

아래 예시를 살펴봅시다.

```js
// Product 객체를 위한 인터페이스 정의
interface Product {
  name: string;
  price: number;
}

// Product 인터페이스를 구현하는 구체적인 Product 클래스 정의
class Book implements Product {
  public name: string;
  public price: number;
  
  constructor(name: string, price: number) {
    this.name = name;
    this.price = price;
  }
}

class Shirt implements Product {
  public name: string;
  public price: number;
  
  constructor(name: string, price: number) {
    this.name = name;
    this.price = price;
  }
}

// Product 객체를 생성하는 팩토리 클래스 정의
class ProductFactory {
  public createProduct(type: string, name: string, price: number): Product {
    if (type === 'book') {
      return new Book(name, price);
    } else if (type === 'shirt') {
      return new Shirt(name, price);
    } else {
      throw new Error('잘못된 제품 유형입니다.');
    }
  }
}

// 사용 예시
const factory = new ProductFactory();

const book = factory.createProduct('book', '반지의 제왕', 20.99);
const shirt = factory.createProduct('shirt', '파란 티셔츠', 12.99);

console.log(book);
console.log(shirt);
```

<div class="content-ad"></div>

우선 이름과 가격을 포함하는 인터페이스를 선언했습니다. 각 제품은 이 두 변수만 가지게 될 것입니다. ProductFactory는 이러한 제품의 범주를 처리하고 모든 제품이 `createProduct` 메서드의 첫 번째 인수로 전달되도록 만드는 역할을 합니다.

여기서 Factory 디자인 패턴의 잘못된 구현이 있습니다.

```js
class Product {
  public name: string;
  public price: number;

  constructor(name: string, price: number) {
    this.name = name;
    this.price = price;
  }
}

class ProductFactory {
  public createProduct(type: string, name: string, price: number): Product {
    if (type === 'book') {
      return new Product(name, price);
    } else if (type === 'shirt') {
      return new Product(name, price);
    } else {
      throw new Error('Invalid product type.');
    }
  }
}

// 사용 예시
const factory = new ProductFactory();

const book = factory.createProduct('book', '반지의 제왕', 20.99);
const shirt = factory.createProduct('shirt', '파란 티셔츠', 12.99);

console.log(book);
console.log(shirt);
```

createProduct 메서드가 타입을 처리하고 있지만 프로그램이 나중에 제품이 어떤 범주에 속하는지 결정할 수 있는 방법이 없다는 것을 알 수 있습니다.

<div class="content-ad"></div>

productFactory에 추가 클래스를 추가해도 모든 클래스가 단일 목적을 가져야 한다는 규칙을 어격히게 위반하게 될 것입니다. 지금은 큰 문제처럼 보이지 않을 수 있지만, 이 애플리케이션이 성장함에 따라 하나의 변경으로 프로젝트 전체의 여러 모듈에 영향을 미쳐 유지보수가 매우 어려워질 수 있습니다.

# 마치며

소프트웨어 디자인은 애플리케이션을 확장하고 나중에 추가 기능을 추가할 때 필요한 중요한 단계입니다. 또한 깔끔하고 조직적으로 유지하여 나중에 예상치 못한 문제에 부딪히지 않도록 도와줍니다.

# 더 많은 조언이나 도움이 필요하신가요?

<div class="content-ad"></div>

안녕하세요! 전체 스택 엔지니어, 커리어 코치, 그리고 비즈니스가 개발 비용을 절약하도록 도와주는 컨설턴트 Chris입니다. 또한, 엔지니어들이 취업을 할 수 있는 소프트 스킬을 갖출 수 있도록 지원합니다.

제 뉴스레터를 구독해보세요!

LinkedIn에서 저와 연락을 유지하세요.
개인 블로그