---
title: "Dart 클래스 선언 마스터하기 final, sealed, abstract, interface, mixin 등을 완벽하게 이해하기"
description: ""
coverImage: "/assets/img/2024-07-09-MasteringClassDeclarationsinDartfinalsealedabstractinterfacemixinandMore_0.png"
date: 2024-07-09 22:26
ogImage: 
  url: /assets/img/2024-07-09-MasteringClassDeclarationsinDartfinalsealedabstractinterfacemixinandMore_0.png
tag: Tech
originalTitle: "Mastering Class Declarations in Dart: final, sealed, abstract, interface, mixin, and More"
link: "https://medium.com/@gautam007/mastering-class-declarations-in-dart-final-sealed-abstract-interface-mixin-and-more-059f555d718e"
isUpdated: true
---




## 다트에서 final, sealed, abstract, interface, mixin 등의 키워드를 사용하여 견고하고 유지보수 가능한 코드를 작성하는 방법을 배워보세요.

플러터 개발에 사용되는 다트는 클래스를 선언하고 관리하기 위한 다양한 키워드를 제공합니다. 이러한 키워드는 클래스의 행동, 제약 조건, 구조를 정의하는 데 도움을 주어 코드의 가독성과 유지보수성을 향상시킵니다. 이 안내서에서는 final, sealed, abstract, interface 등의 클래스 선언에 사용할 수 있는 여러 키워드를 다양하게 살펴보겠습니다.

![Mastering Class Declarations in Dart](/assets/img/2024-07-09-MasteringClassDeclarationsinDartfinalsealedabstractinterfacemixinandMore_0.png)

# 1. final – 변경할 수 없는 클래스

<div class="content-ad"></div>

최종 키워드는 변수와 함께 자주 사용되지만, 클래스에도 불변성을 보장하기 위해 사용될 수 있습니다. 클래스와 함께 사용하면 인스턴스의 수정이 생성된 후에 방지됩니다.

```js
final class Point {
   final int x;
   final int y;

   Point(this.x, this.y);
}

void main() {
   final point = Point(1, 2);
   // point.x = 3; // 오류: 클래스 'Point'에 대한 setter 'x'가 정의되지 않았습니다.
}
```

## 2. sealed – 클래스 확장 제한하기

Dart 2.17부터 Dart는 제한된 상속을 제공하기 위해 sealed 클래스를 소개했습니다. sealed 클래스는 동일한 라이브러리 내의 클래스에 의해서만 확장될 수 있습니다.

<div class="content-ad"></div>

```js
sealed class Shape {
  void draw();
}

class Circle extends Shape {
  @override
  void draw() {
    print("Drawing a circle");
  }
}

class Square extends Shape {
  @override
  void draw() {
    print("Drawing a square");
  }
}

void main() {
  Shape circle = Circle();
  circle.draw(); // 출력: 원 그리기

  Shape square = Square();
  square.draw(); // 출력: 사각형 그리기
}
```

# 3. abstract – 추상 클래스

추상 키워드는 추상 클래스를 정의할 때 사용되며, 추상 클래스는 직접적으로 인스턴스화할 수 없습니다. 추상 클래스는 주로 서브클래스에서 구현되어야 하는 메소드를 가진 베이스 클래스로 사용됩니다.

```js
abstract class Animal {
  void makeSound(); // 추상 메소드
}

class Dog extends Animal {
  @override
  void makeSound() {
    print("왈왈");
  }
}

void main() {
  Dog dog = Dog();
  dog.makeSound(); // 출력: 왈왈
}
```

<div class="content-ad"></div>

# 4. 인터페이스 - 인터페이스 구현하기

Dart에는 명시적 인터페이스 키워드가 없습니다. 대신, 모든 클래스는 암묵적으로 인터페이스를 정의하며, implements 키워드를 사용하여 모든 클래스의 인터페이스를 구현할 수 있습니다. 이는 특정 계약을 준수하는 클래스를 보장하는 데 유용합니다.

```js
interface class Flyable {
  void fly() {}
}

class Bird implements Flyable {
  @override
  void fly() {
    print("새가 날아갑니다");
  }
}

void main() {
  Bird bird = Bird();
  bird.fly(); // 출력: 새가 날아갑니다
}
```

# 5. 믹스인 - 재사용 가능한 코드 블록

<div class="content-ad"></div>

다트(Dart)의 믹신(Mixins)을 사용하면 클래스의 코드를 여러 클래스 계층구조에서 재사용할 수 있습니다. mixin 키워드를 사용하여 선언하며, 상속을 사용하지 않고 클래스에 기능을 추가하는 데 사용할 수 있습니다.

```js
mixin Swimmer {
  void swim() {
    print("수영 중");
  }
}

class Fish with Swimmer {}

void main() {
  Fish fish = Fish();
  fish.swim(); // 출력: 수영 중
}
```

# 6. extension – 기능 확장

extension 키워드를 사용하면 기존 클래스에 새로운 기능을 추가할 수 있습니다. 이는 제어하지 않는 클래스에 유틸리티 메서드를 추가하는 데 유용합니다.

<div class="content-ad"></div>

```js
extension StringExtensions on String {
  String reverse() {
    return split('').reversed.join('');
  }
}

void main() {
  String str = "hello";
  print(str.reverse()); // 결과: olleh
}
```

# 7. enum – Enumerations

enum 키워드는 열거형이라고도 불리는 상수 값의 컬렉션을 정의하는 데 사용됩니다. Enum은 고정된 수의 옵션을 나타내는 데 유용합니다.

```js
enum Color { red, green, blue }

void main() {
  Color favoriteColor = Color.blue;

  switch (favoriteColor) {
    case Color.red:
      print("빨강");
      break;
    case Color.green:
      print("초록");
      break;
    case Color.blue:
      print("파랑");
      break;
  }
}
```

<div class="content-ad"></div>

# 실용적인 사용 사례와 팁

## final 클래스 사용 시기:

- final 클래스를 사용하여 변경이 불가능한 객체를 만들어라. 객체는 생성 후 상태가 변경되지 않도록 하여 데이터 무결성을 보장하라.

## sealed 클래스 사용 시기:

<div class="content-ad"></div>

- 특정 클래스의 확장을 제한하기 위해 보통 동일한 라이브러리 내에서 엄격하게 제어된 계층 구조를 위해 sealed 클래스를 사용하세요.

## abstract 클래스를 사용해야 하는 경우:

- 서브클래스에서 구현되어야 하는 공통 메서드가 있는 베이스 클래스에 대해 abstract 클래스를 사용하세요.

## 인터페이스 구현을 사용해야 하는 경우:

<div class="content-ad"></div>

- 여러 가지 관련 없는 클래스 계층 구조를 다룰 때 특정 계약을 준수하도록 클래스를 구현하도록 합니다.

## mixin을 사용해야 하는 경우:

- 클래스 계층을 만들지 않고 여러 클래스 간에 기능을 공유할 때 mixin을 사용합니다.

## extension을 사용해야 하는 경우:

<div class="content-ad"></div>

- 기존 클래스에 새로운 메소드를 추가하여 기능을 향상시킬 때는 확장을 사용하세요. 이렇게 하면 원본 클래스를 변경하지 않고도 기능을 보다 효율적으로 확장할 수 있습니다.

## 열거형(enum)을 사용하는 경우:

- 관련 상수 집합을 정의하고 코드의 가독성과 안전성을 높일 때는 열거형을 사용하세요.

# 결론

<div class="content-ad"></div>

Dart의 클래스 선언 키워드를 이해하고 효과적으로 사용하면 더 견고하고 유지보수가 용이하며 확장 가능한 코드를 작성할 수 있습니다. 이러한 키워드를 적절히 활용하여 Dart 애플리케이션의 기능성, 가독성 및 구조를 향상시킬 수 있습니다. 즐거운 코딩하세요!