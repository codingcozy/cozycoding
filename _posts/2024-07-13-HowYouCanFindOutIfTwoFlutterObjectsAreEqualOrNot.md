---
title: "두 개의 Flutter 객체가 같은지 확인하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-HowYouCanFindOutIfTwoFlutterObjectsAreEqualOrNot_0.png"
date: 2024-07-13 21:32
ogImage: 
  url: /assets/img/2024-07-13-HowYouCanFindOutIfTwoFlutterObjectsAreEqualOrNot_0.png
tag: Tech
originalTitle: "How You Can Find Out If Two Flutter Objects Are Equal Or Not"
link: "https://medium.com/gitconnected/how-you-can-find-out-if-two-flutter-objects-are-equal-or-not-6271c62a2807"
---



![스크린샷](/assets/img/2024-07-13-HowYouCanFindOutIfTwoFlutterObjectsAreEqualOrNot_0.png)

객체의 동등성 비교는 모든 종류의 앱에서 중요합니다. 중복된 객체는 종종 원하지 않습니다. 이 기사에서는 두 개의 Flutter 객체가 동일한지 여부를 확인하는 방법을 보여드리겠습니다. 다른 프로그래밍 언어에서와 마찬가지로 Dart에서도 동일합니다. 이 글에서는 두 가지 비교 방법을 소개합니다. Dart에서 제공하는 내장 메서드를 사용한 긴 방법과 외부 패키지를 사용하지만 덜 세심한 방법입니다.

# 내장 메서드로 비교하기

간단한 식별자, 이름 및 이메일 속성을 갖는 간단한 클래스가 있다고 가정해 보겠습니다. Dart 언어에서 제공하는 옵션을 사용할 때 두 가지 메서드를 재정의해야 합니다:


<div class="content-ad"></div>

- 등호 연산자 == 
- 그리고 메소드 hashCode

다음은 코드 예시입니다:

```js
class Person {
  final int id;
  final String name;
  final String email;

  Person(this.id, this.name, this.email);

  // 등호 연산자 오버라이드
  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    if (other is! Person) return false;
    return id == other.id &&
           name == other.name &&
           email == other.email;
  }

  // hashCode 메소드 오버라이드
  @override
  int get hashCode => id.hashCode ^ name.hashCode ^ email.hashCode;
}
```

자, 이 코드에서 무슨 일이 일어나는지 알아보겠습니다.

<div class="content-ad"></div>

먼저, 연산자 ==를 재정의합니다. 이 코드는 두 객체가 동일한 참조를 공유하는지를 결정하기 위해 동일한 함수를 사용합니다. 그렇지 않으면 타입 확인이 수행됩니다. 타입이 일치하는 경우 모든 속성 값이 비교됩니다.

두 번째 단계는 hashCode를 재정의하는 것입니다. 이렇게 함으로써 속성 값이 동일한 객체는 동일한 해시 값이 반환되어 동일하다고 간주됩니다. 또한 HashMap 또는 HashSet와 같은 해시 기반 컬렉션에서 효율적인 조회에 유용합니다.

이 절차는 직관적이지만 속성이 많아지면 상당히 많은 양의 코드가 생성될 수 있습니다. 두 번째 방법은 이 문제를 해결해 줍니다.

# equatable 패키지를 사용하여 비교하기

<div class="content-ad"></div>

패키지 equatable을 사용하면 저가 위에서 설명한 모든 것을 처리해주고 여러 줄의 코드를 절약할 수 있습니다. 객체를 Equatable로 파생시키고 비교에 포함하려는 속성을 지정해야 하는 것만 필요합니다.

다음은 예시입니다:

```js
import 'package:equatable/equatable.dart';

class Person extends Equatable {
  final int id;
  final String name;
  final String email;

  Person(this.id, this.name, this.email);

  @override
  List<Object?> get props => [id, name, email];
}
```

props getter는 객체를 비교할 때 고려해야 하는 모든 속성을 반환합니다.

<div class="content-ad"></div>

가끔은 데이터베이스에서 고유 식별자를 가진 객체가 있습니다. 이 경우 id 속성만 비교하고 나머지는 건너뛰는 것이 충분하게 됩니다.

이 접근 방식은 동일한 결과를 얻기 위해 훨씬 적은 코드 줄이 필요합니다. 그러나 (또다른) 제3자 패키지에 의존해야 한다는 단점이 있습니다.

# 보너스: 빠르게 데이터 클래스 생성하기

Dart Quicktype은 JSON을 깔끔한 Dart 객체로 변환해주는 훌륭한 서비스입니다. 필요에 맞게 클래스를 정확히 만들 수 있는 많은 구성 옵션이 있습니다. 또한 강력히 추천하는 Equatable 옵션이 있습니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-HowYouCanFindOutIfTwoFlutterObjectsAreEqualOrNot_1.png)

당연히, 여러분이 선호하는 인공지능 모델도 비슷한 작업을 할 수 있지만, 제 경험상 대부분의 경우 수동 편집이 필요합니다. 가능한 한 자주 사용하는 것이 저의 해결책입니다.

# 결론

본 문서에서는 두 가지 방법을 소개하여 두 개의 Flutter 객체가 동일한지 여부를 확인하는 방법을 소개했습니다. 3rd party 패키지에 의존하지 않고 직접 작업할 수도 있고, equatable 패키지를 사용하여 많은 코드 줄을 절약할 수도 있습니다. 두 가지 방법 모두 잘 작동할 것입니다.

<div class="content-ad"></div>

💡 잠깐만요! 제 뉴스레터에 가입하면 간단한 플러터 팁을 받을 수 있어요! Firebase나 Flutter에 대해 내 소형 ebook으로 배우세요. 그리고 제 샵에 방문해서 디지털 제품, 도구 및 무료 자료를 확인해보세요!