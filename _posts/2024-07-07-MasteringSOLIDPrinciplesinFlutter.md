---
title: "Flutter에서 SOLID 원칙 마스터하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-MasteringSOLIDPrinciplesinFlutter_0.png"
date: 2024-07-07 22:23
ogImage: 
  url: /assets/img/2024-07-07-MasteringSOLIDPrinciplesinFlutter_0.png
tag: Tech
originalTitle: "Mastering SOLID Principles in Flutter"
link: "https://medium.com/gitconnected/mastering-solid-principles-in-flutter-30cdaaa5475b"
---


<img src="/assets/img/2024-07-07-MasteringSOLIDPrinciplesinFlutter_0.png" />

안녕하세요 여러분!

오랜만이에요 (두 년 동안) 제가 Medium에 글을 쓰지 않았네요. 몇 개의 글을 초안으로 작성했지만 발행하지 않았어요 (곧 발행할 예정이에요). 지난 두 년 동안, Mobile 앱을 위한 CICD 구성, Fastlane, Microservices, 앱 보안 등 대부분의 것들을 배웠어요. SOLID 원칙도 그 중 하나였어요. 이 글에서는 SOLID 원칙이 무엇인지, 그리고 Flutter 프로젝트에 어떻게 적용하는지 예를 들어 살펴보겠어요.

# SOLID 원칙이란 무엇인가요?

<div class="content-ad"></div>

SOLID 원칙은 객체 지향 프로그래밍의 다섯 가지 디자인 원칙으로, 개발자가 확장 가능한 소프트웨어 시스템을 만들고 유지보수하는 데 도움을 줍니다. 이것은 2000년에 유명한 Uncle Bob(컴퓨터 과학자 로버트 J. 마틴)에 의해 처음 소개되었습니다. 하지만 SOLID 약어는 나중에 Michael Feathers에 의해 소개되었습니다.

약어를 먼저 이해해 봅시다.

## S.O.L.I.D.

S - 단일 책임 원칙

<div class="content-ad"></div>


O — 개방 폐쇄 원칙

L — 리스코프 치환 원칙

I — 인터페이스 분리 원칙

D — 의존성 역전 원칙


<div class="content-ad"></div>

# 단일 책임 원칙 (SRP)

단일 책임 원칙 (SRP)은 클래스나 모듈이 변경되어야 하는 이유가 하나뿐이어야 한다는 원칙을 말합니다.

이제 Flutter에 어떻게 적용할 수 있는지 알아볼까요?

예를 들어, 회사에서 전자 상거래 앱을 개발 중이라고 가정해봅시다. 상품 목록을 표시하는 기능을 개발할 수 있습니다. 코드가 단일 책임 원칙을 따르도록 하기 위해 관심사를 다른 클래스로 분리하는 방법을 확인하고 싶을 것입니다.

<div class="content-ad"></div>

먼저, 하나의 제품을 나타내는 Product 클래스를 생성할 것입니다:

```js
class Product {
  final String name;
  final double price;
  final String description;

  Product({required this.name, required this.price, required this.description});
}
```

다음으로, 제품을 가져오고 관리하는 책임이 있는 ProductRepository 클래스를 생성할 것입니다:

```js
class ProductRepository {
  // 제품을 가져오고 추가 및 업데이트하는 메서드들
}
```

<div class="content-ad"></div>

이제 제품 목록을 표시하는 ProductListScreen 위젯을 생성해 봅시다:

```js
class ProductListScreen extends StatelessWidget {
  // 제품 목록을 표시하는 UI 코드
}
```

이 예시에서:

- Product 클래스는 속성을 포함한 단일 제품을 나타냅니다.
- ProductRepository 클래스는 제품을 관리하고 가져오는 역할을 합니다.
- ProductListScreen 위젯은 제품 목록을 표시하는 역할을 합니다.

<div class="content-ad"></div>

서로 다른 클래스(Product, ProductRepository 및 ProductListScreen)로 역할을 분리함으로써 우리는 단일 책임 원칙을 따릅니다. 각 클래스에는 명확하고 구분된 책임이 있어 코드가 더 모듈화되고 유지보수가 쉽고 이해하기 쉬워지며 테스트하기도 쉬워집니다.

# 개방/폐쇄 원칙 (OCP)

OCP는 클래스가 확장에 대해 열려 있어야 하지만 수정에 대해 폐쇄되어야 한다고 말합니다.

상기 원칙을 전자상거래 앱에 적용해 봅시다. 제품 목록을 표시한 후에 이제 특정 카테고리로 제품을 필터링할 수 있는 새로운 기능을 도입해야 한다는 작업을 받았습니다.

<div class="content-ad"></div>

제품 목록 화면 위젯을 직접 수정하는 대신 OCP를 위반하는 것을 피하기 위해 추상화와 상속 개념을 사용하여 기능을 확장할 것입니다.

먼저 ProductFilter라는 추상 클래스를 만들겠습니다:

```js
abstract class ProductFilter {
  List<Product> applyFilter(List<Product> products);
}
```

다음으로, 카테고리별 제품 필터링을위한 ProductFilter의 구체적인 구현을 만들겠습니다:

<div class="content-ad"></div>

```js
class CategoryFilter extends ProductFilter {

  @override
  List<Product> applyFilter(List<Product> products) {
    // 조건에 맞게 제품 필터링
  }
}
```

이제 ProductListScreen 위젯을 수정하여 ProductFilter 매개변수를 받을 수 있도록 합시다:

```js
class ProductListScreen extends StatelessWidget {
  final ProductRepository productRepository;
  final ProductFilter? productFilter;

  const ProductListScreen({Key? key, required this.productRepository, this.productFilter}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    productRepository.fetchProducts();
    List<Product> filteredProducts = productFilter != null
        ? productFilter!.applyFilter(productRepository.products)
        : productRepository.products;
    
    // UI 렌더링
  }
}
```

이러한 변경으로 기존의 ProductListScreen 코드를 수정하지 않고 제품 목록의 기능을 확장할 수 있습니다. 예를 들어 제품의 가용성에 따라 새로운 필터를 생성할 수 있습니다.


<div class="content-ad"></div>

```js
class AvailabilityFilter extends ProductFilter {
  final bool available;

  AvailabilityFilter(this.available);

  @override
  List<Product> applyFilter(List<Product> products) {
    // Filtering condition for available products applied here
  }
}
```

그러면 ProductListScreen에서 코드를 수정하지 않고 이 새로운 필터를 사용할 수 있습니다.

```js
ProductListScreen(
  productRepository: productRepository,
  productFilter: AvailabilityFilter(true), // 사용 가능한 제품만 표시
);
```

이 접근 방식은 ProductListScreen의 소스 코드를 수정하지 않고도 해당 기능을 확장할 수 있어 Open/Closed Principle을 준수합니다. ProductFilter라는 추상화를 도입하여 ProductListScreen의 기존 구현을 변경하지 않고 새로운 필터(예: CategoryFilter 및 AvailabilityFilter)를 추가할 수 있습니다.


<div class="content-ad"></div>

# 리스코프 치환 원칙 (LSP)

LSP는 상위 클래스의 객체를 하위 클래스의 객체로 교체할 수 있도록 하면 프로그램의 정확성에 영향을 미치지 않아야 한다는 것을 말합니다.

우리는 제품의 하위 클래스를 생성하고 제품 클래스와 상호 교환할 수 있음을 보장함으로써 LSP를 증명할 수 있습니다.

```js
class DiscountedProduct extends Product {
  final double discount;

  DiscountedProduct({
    required String name,
    required double price,
    required String description,
    required this.discount,
  }) : super(name: name, price: price, description: description);
}
```

<div class="content-ad"></div>

이 하위 클래스에서는 Product를 상속하고 추가적인 속성인 할인(discount)을 추가합니다.

이제 ProductListScreen 위젯을 업데이트하여 Product 및 DiscountedProduct 인스턴스를 모두 처리할 수 있도록 합니다:

```js
import 'package:flutter/material.dart';

class ProductListScreen extends StatelessWidget {
  final ProductRepository productRepository;
  final ProductFilter? productFilter;

  const ProductListScreen({Key? key, required this.productRepository, this.productFilter}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    productRepository.fetchProducts();
    List<Product> filteredProducts = productFilter != null
        ? productFilter!.applyFilter(productRepository.products)
        : productRepository.products;

    return Scaffold(
      appBar: AppBar(
        title: Text('Product List'),
      ),
      body: ListView.builder(
        itemCount: filteredProducts.length,
        itemBuilder: (context, index) {
          final product = filteredProducts[index];
          return ListTile(
            title: Text(product.name),
            subtitle: Text('${product.price.toStringAsFixed(2)}\n${product.description}'),
            trailing: Text(
              product is DiscountedProduct
                  ? 'Discounted'
                  : 'Regular', // DiscountedProduct일 경우 '할인', 그 외에는 '일반'을 표시합니다
            ),
          );
        },
      ),
    );
  }
}
```

이 업데이트된 ProductListScreen에서는 제품이 DiscountedProduct의 인스턴스인지 확인하고 해당 내용을 표시하도록 추가 확인을 포함했습니다(예: DiscountedProduct인 경우 UI에서 "할인"을 표시합니다). 이를 통해 Liskov Substitution Principle을 충족하는 ProductListScreen에서 DiscountedProduct를 Product와 상호 교환하여 사용할 수 있음을 보여줍니다.

<div class="content-ad"></div>

# Interface Segregation Principle (ISP)

ISP는 클라이언트가 사용하지 않는 인터페이스에 의존하지 않아도 된다는 것을 말합니다.

음악 플레이어를 예로 들어보겠습니다. 두 가지 기능이 있습니다: 음악 재생과 음악 정지입니다. 추가로 Spotify가 하는 것과 유사하게 오프라인에서 음악 재생을 지원합니다.

다음은 코드에서 어떻게 보일지에 대한 내용입니다:

<div class="content-ad"></div>

```js
// 음악 플레이어 인터페이스인 MusicPlayer를 정의한다.
abstract class MusicPlayer {
  void playMusic();
  void stopMusic();
}

// 인터넷에서 음악을 재생하는 OnlineMusicPlayer를 구현한다.
class OnlineMusicPlayer implements MusicPlayer {
  @override
  void playMusic() {
    print("인터넷에서 음악을 재생합니다");
  }

  @override
  void stopMusic() {
    print("인터넷에서 음악을 중지합니다");
  }
}

// 로컬 저장소에서 음악을 재생하는 OfflineMusicPlayer를 구현한다.
class OfflineMusicPlayer implements MusicPlayer {
  @override
  void playMusic() {
    print("로컬 저장소에서 음악을 재생합니다");
  }

  @override
  void stopMusic() {
    print("로컬 저장소에서 음악을 중지합니다");
  }
}
```

이제 이 음악 플레이어 클래스들의 구체적인 구현에 의존하지 않고 사용하는 Flutter 위젯을 생성해보겠습니다:

```js
import 'package:flutter/material.dart';

class MusicPlayerWidget extends StatelessWidget {
  final MusicPlayer musicPlayer;

  MusicPlayerWidget(this.musicPlayer);

  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        ElevatedButton(
          onPressed: () {
            musicPlayer.playMusic();
          },
          child: Text('음악 재생'),
        ),
        ElevatedButton(
          onPressed: () {
            musicPlayer.stopMusic();
          },
          child: Text('음악 정지'),
        ),
      ],
    );
  }
}
```

이제 OnlineMusicPlayer와 OfflineMusicPlayer의 인스턴스와 함께 MusicPlayerWidget을 사용할 수 있습니다:

<div class="content-ad"></div>

```js
void main() {
  runApp(MaterialApp(
    home: Scaffold(
      appBar: AppBar(title: Text('음악 플레이어 예시')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('온라인 음악 플레이어'),
            MusicPlayerWidget(OnlineMusicPlayer()),
            SizedBox(height: 20),
            Text('오프라인 음악 플레이어'),
            MusicPlayerWidget(OfflineMusicPlayer()),
          ],
        ),
      ),
    ),
  ));
}
```

이 예시는 `MusicPlayerWidget`이 OnlineMusicPlayer와 OfflineMusicPlayer와 같은 일반적인 인터페이스(MusicPlayer)를 통해 상호작용할 수 있도록 하여 Interface Segregation Principle을 준수합니다.

# DIP(Dependency Inversion Principle)

DIP는 고수준 모듈이 저수준 모듈에 의존하지 않아야 하며, 둘 다 추상화에 의존해야 한다고 말합니다. Flutter에서 DIP를 적용하는 방법은 다음과 같습니다:

<div class="content-ad"></div>

먼저, 데이터베이스 서비스를 나타내는 추상 클래스 DatabaseService를 생성합시다. 이 클래스에는 데이터베이스에서 데이터를 가져오는 fetchData 메서드가 포함됩니다.

```js
abstract class DatabaseService {
  Future<String> fetchData();
}
```

다음으로, 이 인터페이스의 두 구체적인 구현인 LocalDatabaseService와 RemoteDatabaseService를 만들어봅시다.

```js
class LocalDatabaseService implements DatabaseService {
  @override
  Future<String> fetchData() async {
    // 로컬 데이터베이스에서 데이터 가져오기
  }
}

class RemoteDatabaseService implements DatabaseService {
  @override
  Future<String> fetchData() async {
    // 원격 데이터베이스에서 데이터 가져오기
  }
}
```

<div class="content-ad"></div>

이제 DatabaseService 구현체 대신 추상화인 DatabaseService에 의존하는 DataManager 클래스를 생성해 봅시다.

```js
class DataManager {
  final DatabaseService databaseService;

  DataManager(this.databaseService);

  Future<void> fetchData() async {
    final data = await databaseService.fetchData();
  }
}
```

마지막으로, DataManager를 다양한 DatabaseService 구현과 함께 사용할 수 있습니다:

```js
void main() {
  final localService = LocalDatabaseService();
  final remoteService = RemoteDatabaseService();

  final localDataManager = DataManager(localService);
  final remoteDataManager = DataManager(remoteService);

  localDataManager.fetchData(); 
  remoteDataManager.fetchData();
}
```

<div class="content-ad"></div>

이 예제에서 DataManager는 구체적인 구현(LocalDatabaseService 및 RemoteDatabaseService)에 의존하지 않습니다. 대신, Dependency Inversion Principle을 따라 추상화 된 DatabaseService에 의존합니다. 이는 코드를 더 유연하게 만들어주며, 높은 수준의 DataManager 클래스를 수정하지 않고도 구현을 쉽게 교체할 수 있게 합니다.

# 요약

이러한 SOLID 원칙을 준수함으로써, 더 유지보수 가능하고 유연하며 확장 가능한 Flutter 앱을 작성할 수 있습니다. 이러한 원칙은 코드를 깨끗하고 효율적으로 만들어주며, Flutter 개발을 배우는 학생들에게 가치 있는 기술을 제공합니다.

# 가기 전에

<div class="content-ad"></div>

내 글을 읽어주셔서 감사합니다. SOLID 개념이 잘 이해되셨으면 다음 플러터 프로젝트에 적용해보세요.

그리고, 아, 알고 계셨나요? 박수 버튼을 최대 50번 클릭할 수 있다고요! 이 글이 마음에 드신다면 둥근 박수를 보내주세요. 여러분의 박수를 진심으로 환영합니다!

질문이 있으시다면 LinkedIn, Twitter, Instagram에서 연락해주세요.

즐거운 코딩하시길 :)