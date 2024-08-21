---
title: "Dart 프로그래밍 언어에서 Class와 Mixin 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-07-ClassandmixininDartprogramminglanguage_0.png"
date: 2024-07-07 13:14
ogImage: 
  url: /assets/img/2024-07-07-ClassandmixininDartprogramminglanguage_0.png
tag: Tech
originalTitle: "Class and mixin in Dart programming language"
link: "https://medium.com/@programmingpassion/class-and-mixin-in-dart-programming-language-fb55d4f8b048"
isUpdated: true
---





다트 프로그래밍 언어는 가장 정교한 클래스 시스템을 가지고 있습니다. 따라서 다트에서 클래스를 사용하는 방법에 대한 다양한 기능을 제공합니다.

아래는 다트 언어의 클래스 표입니다. 이를 살펴보면 얼마나 복잡한지 감을 잡을 수 있을 것입니다.

![다트 언어의 클래스 표](/assets/img/2024-07-07-ClassandmixininDartprogramminglanguage_0.png)

이 표를 어떻게 이해해야 할까요? 표를 분석하고 규칙을 찾아보면 이해하기가 수월해질 것입니다.

<div class="content-ad"></div>

# 클래스

Dart의 클래스는 클래스 또는 인터페이스로 사용할 수 있습니다. 자식 클래스는 부모 클래스를 파생하거나 클래스를 인터페이스로 구현할 수 있습니다.

# 믹스인

믹스인은 확장과 비슷하지만 어떤 클래스에서도 공유하고 재사용할 수 있습니다. 믹스인은 인터페이스 또는 믹스인으로 사용할 수 있습니다.

<div class="content-ad"></div>

# 믹스인 클래스

믹스인 클래스는 믹스인과 클래스의 결합체입니다. 이는 믹스인 클래스가 다중 목적 클래스, 인터페이스 또는 믹스인으로 작용할 수 있다는 것을 의미합니다.

# 혼란스럽고 압도되는

이는 대다수에게 혼란스러워 보입니다.

<div class="content-ad"></div>

더불어, 앞에서 언급한 대로 Dart에서는 클래스와 믹스인이 이중 선택 옵션이 될 수 있는 상황이 있습니다.

게다가, 클래스와 믹스인의 조합은 모두, 클래스, 인터페이스, 믹스인의 삼중 옵션이 될 수 있습니다.

# 기본(base)과 인터페이스(interface)로 간단하게 만들기

모든 것을 명확하고 명료하게 만들기 위해, 기본(base)과 인터페이스가 구원을 주게 됩니다.

<div class="content-ad"></div>

기본 클래스를 사용하여 클래스를 정의할 수 있어요.

인터페이스 클래스를 사용하여 인터페이스를 정의할 수 있어요.

그리고 베이스 믹스인을 사용하여 믹스인을 정의할 수 있어요.

# 추상

<div class="content-ad"></div>

추상 클래스를 형성하기 위해서는 항상 클래스와 함께 사용해야 하며 다른 것은 아무 것도 아닙니다.

abstract를 사용함으로써, 추상 클래스는 추상 클래스 내에서 구체적인 구현 없이 메서드를 제공하는 옵션을 제공합니다. 더 나아가, 추상 클래스로는 인스턴스를 만들지 않을 것입니다.

abstract는 인터페이스 클래스, 베이스 클래스에도 적용할 수 있으며, 이러한 경우에는 해당 클래스의 인스턴스를 생성하고 싶지 않을 때 사용할 수 있습니다.

# final class

<div class="content-ad"></div>

최종 클래스는 상속 없이 일반 기본 클래스와 같은 역할을합니다. 이 부분은 매우 직관적입니다.

## 최종 추상 클래스

최종 추상 클래스는 최종 클래스와 동일하지만 abstract가 있는 경우 거의 인스턴스화를 허용하지 않습니다.

## sealed 클래스

<div class="content-ad"></div>

sealed 클래스는 완전한 추상 클래스와 전체 논리를 지원하는 것과 같습니다.

# 결론

Dart 프로그래밍 언어에서 클래스와 mixin은 OOP를 적용하는 여러 가지 옵션을 제공합니다. 그러나 저는 개인적으로 복잡하다고 느낍니다.

모든 것을 섞어서 자유롭게 사용할 수도 있고, 모든 역할을 명확하게 만들어 의심하는 것도 가능합니다.