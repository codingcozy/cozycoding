---
title: "Cubit  Freezed 강력한 BloC 패턴 옵션"
description: ""
coverImage: "/assets/img/2024-07-09-CubitFreezedPowerfulBloCpatternoption_0.png"
date: 2024-07-09 22:18
ogImage: 
  url: /assets/img/2024-07-09-CubitFreezedPowerfulBloCpatternoption_0.png
tag: Tech
originalTitle: "Cubit + Freezed: Powerful BloC pattern option🧊"
link: "https://medium.com/@caio.dev29/cubit-freezed-powerful-bloc-pattern-option-98e4f047b00c"
isUpdated: true
---




플러터 생태계에서는 다양한 컨텍스트 간 상태를 관리하는 방법이 많이 있어요. 이에 맞게 Cubit과 Freezed를 결합하여 새로운 접근법을 소개해 드리려고 해요. 이 방법은 코딩과 코드 생성을 향상시켜 생산성을 높일 수 있어요!

하지만 먼저, 계속하기 전에 몇 가지 중요한 사항을 먼저 알아보도록 할게요 :D

# BloC (Business Logic Component)

![Image](/assets/img/2024-07-09-CubitFreezedPowerfulBloCpatternoption_0.png)

<div class="content-ad"></div>

BLoC은 Flutter 애플리케이션에서 상태를 관리하는 데 사용되는 구조적인 패턴입니다. 이는 책임의 분리를 촉진하여 코드를 더 깔끔하고 모듈화하기 쉽게 합니다.

# Cubit: BLoC을 간단하게

![이미지](/assets/img/2024-07-09-CubitFreezedPowerfulBloCpatternoption_1.png)

Cubit은 BLoC보다 간단한 라이브러리로, Flutter 애플리케이션에서 상태를 관리하기 위해 설계되었습니다. BLoC가 이벤트-상태 패턴을 따른다면, Cubit은 이를 간소화하여 이벤트 없이 직접 상태 변경이 가능합니다.

<div class="content-ad"></div>

# Freezed

Freezed는 불변 클래스 및 Dart의 다른 일반적인 패턴, 예를 들어 조합 유형 생성에 도움을 주는 라이브러리입니다.

Freezed의 주요 장점 중 하나는 Cubit에서 Equatable 패키지가 필요하지 않게하고 자동으로 클래스의 `==` 및 `hashCode` 메서드를 생성한다는 것입니다.

# 이 강력한 조합을 활용하는 방법:

<div class="content-ad"></div>

먼저, 이 개발에서 사용할 종속 항목을 가져와야 합니다.

```js
name: your_app_name
description: 앱 설명

publish_to: 'none' 

version: 1.0.0+1

environment:
  sdk: '>=3.1.5 <4.0.0'

dependencies:
  flutter:
    sdk: flutter

  #HERE
  json_annotation: ^4.8.1
  freezed_annotation: ^2.4.1

dev_dependencies:
  flutter_test:
    sdk: flutter

  #HERE
  freezed: ^2.4.6
  build_runner: ^2.3.3

flutter:

  uses-material-design: true

  assets:
   - assets/
```

이제 Cubit과 Freezed를 결합하여 상태를 만들 수 있습니다. (제품 로딩 시뮬레이션을 할 것이지만, 여러분의 상황에 맞게 적응해 주세요).

```js
import 'package:your_app/data/products/products.dart';
import 'package:freezed_annotation/freezed_annotation.dart';

part 'products_state.freezed.dart';

@freezed
class ProductsState with _$ProductsState {
  const factory ProductsState.initial() = InitialProductsState;

  const factory ProductsState.loading() = LoadingProductsState;

  const factory ProductsState.error({required Object exception, required StackTrace stackTrace}) = ErrorProductsState;

  const factory ProductsState.success(List<Product>? products) = SuccessProductsState;

}
```

<div class="content-ad"></div>

먼저 Freezed가 생성할 파일을 지정해야 합니다. 우리 경우에는 `products_state.freezed.dart`가 될 것입니다.

`@freezed` 주석을 사용하여 Freezed에게 `ProductsState` 클래스의 코드를 생성하도록 지시해야 합니다.

처음에 코드에서 여러 오류가 보인다 해도 걱정하지 마세요. `ProductsState` 클래스에 정의된 팩토리들을 생성하기 위해 `build_runner`을 사용할 것이기 때문입니다. Freezed에 의해 주석이 달린 코드를 생성하기 위해 다음 명령어를 사용하세요:

```js
flutter pub run build_runner build
```

<div class="content-ad"></div>

알았어요! 마법이 작동해서 이제 상태 파일과 함께 새로운 `.freezed` 파일이 생성된 것을 알 수 있어요:

![이미지](/assets/img/2024-07-09-CubitFreezedPowerfulBloCpatternoption_2.png)

이제 생성된 상태에 기반한 Cubit을 만들 수 있어요.

여기 예시 코드를 확인해주세요:

```js
import 'dart:developer';

import 'package:dio/dio.dart';
import 'package:your_app/cubits/products_cubit/products_state.dart';
import 'package:your_app/data/products/products.dart';
import 'package:your_app/data/repositories/all_products/products_repository.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

class ProductsCubit extends Cubit<ProductsState> {
  ProductsCubit(this.repo) : super(const ProductsState.initial());

  final ProductsRepository repo;

  Future<void> getAllProducts() async {
    emit(const ProductsState.loading());

    try {
      final products = await repo.getAllProducts();

      emit(ProductsState.success(products));
    } catch (e, s) {
      emit(ProductsState.error(exception: e, stackTrace: s));
    }
  }
}
```

<div class="content-ad"></div>

UI에서 Cubit이라고 불리는 것을 변경하겠습니다:

```js
class ProductsPage extends StatefulWidget {
  @override
  _ProductsPageState createState() => _ProductsPageState();
}

class _ProductsPageState extends State<ProductsPage> {
  late ProductsCubit _productsCubit;

  @override
  void initState() {
    super.initState();
    _productsCubit = context.read<ProductsCubit>();
    _productsCubit.getAllProducts();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Products'),
      ),
      body: Column(
        children: [
          ElevatedButton(
            onPressed: () => _productsCubit.getAllProducts(),
            child: Text('Load Products'),
          ),
          Expanded(
            child: BlocBuilder<ProductsCubit, ProductsState>(
              builder: (context, state) {
                return state.when(
                  initial: () => Center(child: Text('데이터를 불러오려면 버튼을 누르세요')),
                  loading: () => Center(child: CircularProgressIndicator()),
                  error: (exception, stackTrace) => Center(child: Text('제품 불러오기 실패: $exception')),
                  success: (products) {
                    return ListView.builder(
                      itemCount: products.length,
                      itemBuilder: (context, index) {
                        final product = products[index];
                        return ListTile(
                          leading: Image.network(product.image),
                          title: Text(product.title),
                          subtitle: Text('\$${product.price}'),
                        );
                      },
                    );
                  },
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}
```

여기에 이 강력한 조합을 활용하는 간단한 예제가 있습니다. 특정 요구 사항에 맞게 조정하고 최선의 방법을 준수하도록 주의하세요.

고마워요, 좋은 코드로! 😎🤜🤛