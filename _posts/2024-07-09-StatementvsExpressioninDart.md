---
title: "Dart에서 Statement와 Expression의 차이점 이해하기"
description: ""
coverImage: "/assets/img/2024-07-09-StatementvsExpressioninDart_0.png"
date: 2024-07-09 22:13
ogImage: 
  url: /assets/img/2024-07-09-StatementvsExpressioninDart_0.png
tag: Tech
originalTitle: "Statement vs. Expression in Dart"
link: "https://medium.com/gitconnected/statement-vs-expression-in-dart-b9af4a3b525d"
---



![Statement vs Expression in Dart](/assets/img/2024-07-09-StatementvsExpressioninDart_0.png)

프로그래밍 언어의 기초 요소 중 하나는 명령문으로, 특정 작업이나 동작을 수행하는 코드 단위를 나타냅니다. 또 다른 중요한 요소는 표현식으로, 값을 생성하는 코드 단위를 표현합니다. 이 글에서는 명령문과 표현식 사이의 차이를 살펴보며 프로그래밍 기초와 Dart 프로그래밍 언어의 구문을 더욱 확고히 할 것입니다.

# 소개

먼저, 작성하는 모든 코드가 명령문인지 표현식인지에 동의합시다. 맞아요 – 한 단어부터 완전한 함수나 클래스까지 모든 코드는 이 두 범주 중 하나에 속합니다.


<div class="content-ad"></div>

# 문장

문장은 여러 표현식, 연산 및 다른 문장으로 구성된 코드로, 여러분의 코드를 끝내는 코드입니다(; 또는 ') 그리고 한 줄 또는 코드 블록을 끝내는 모든 내용입니다. 예를 들어, 아래의 코드는 변수에 값을 선언하고 할당하는 작업을 수행하며, 줄 끝에 ; 이 있어서 문장입니다:

```js
final x = 5;
```

다른 예로는 if 문이 있습니다. 이는 코드 블록을 :으로 끝내는 예입니다.

<div class="content-ad"></div>

```js
if (x == 5) {
 print("hi");
}
```

위의 `if` 문 안에는 다른 문장이 있습니다: `print("hi");`, 이 문장은 세미콜론으로 코드 라인을 끝냅니다.

# 표현식

위에서 볼 수 있듯이, 모든 코드 라인 또는 블록은 문장입니다. 그렇다면 표현식은 무엇일까요? 표현식은 값을 가지거나 반환하는 모든 코드입니다. 예를 들어:

<div class="content-ad"></div>

```js
마지막 x = 5;
```

여기서 5는 값이 있기 때문에 표현식입니다. 다른 예제를 살펴보겠습니다:

```js
if (x == 5) {
 print("hi");
}
```

위 코드에서 6개의 표현식과 2개의 문이 있습니다:
- x는 메모리에 값이 있기 때문에 표현식입니다.
- 5도 표현식입니다.
- x == 5는 부울 값이 반환되기 때문에 표현식입니다.
- "hi"는 표현식입니다.
- print 함수 호출도 표현식입니다. 함수 호출은 값을 반환하기 때문입니다 (우리의 경우 void).
- print 자체도 표현식입니다.

<div class="content-ad"></div>

```js
print("hi") 라고 완전한 라인을 출력하는 것은 ;으로 끝나기 때문에 문장입니다. if 문도 문장입니다.

간단히 말해서, 변수에 저장할 수 있는 모든 것은 표현식입니다:

const x = print("hi");

위 코드는 print 함수(또는 다른 함수)를 호출하는 것이 표현식이기 때문에 유효합니다.

<div class="content-ad"></div>

아래 예시를 참고해주세요:

최종 y = if (x == 5) {
 console.log("안녕");
}

위의 코드는 잘못되었습니다. if는 식이 아니기 때문에 오류가 발생합니다.

// 올바른 코드
최종 hii = (String x, String b) {
 console.log(x);
 console.log(b);
 return x;
};
// 잘못된 코드
최종 x = String hii2(String x, String b) {
 console.log(x);
 console.log(b);
 return x;
};

<div class="content-ad"></div>

첫 번째 함수는 이름이 없는 함수인 함수 표현식을 사용했기 때문에 올바릅니다. 두 번째 함수는 함수 이름이 있는 함수 선언문이기 때문에 잘못되었습니다.

# 표현식 위치

위의 예제에서는 코드를 올바르게 만들기 위해 채워져야 하는 위치가 있습니다. 예를 들어 `if` 조건문에서는 코드를 유효하게 만들기 위해 괄호 안에 불리언 표현식을 제공해야 합니다. = 기호 뒤도 표현식 위치이며, switch case 내부, 함수에 매개변수를 전달할 때 등 더 많은 위치가 있습니다.

# 흔한 함정

<div class="content-ad"></div>

1. 값을 할당하기 위해 if 문을 표현식으로 사용:

const x = 5;
// 잘못된 사용, `if`는 문이기 때문에 변수에 할당될 수 없습니다
final y = if (x == 5) 'hi';
// 올바른 사용
final String? y;
if (x == 5)
 y = 'hi';
// 올바른 사용
final y2 = (x == 5) ? 'hi' : null;

2. 매개변수 전달을 위해 if 문 사용:

const secure = true;
// 잘못된 사용
print(if (secure) 'secure' else 'insecure');
// 올바른 사용
print(secure ? 'secure' : 'insecure');

<div class="content-ad"></div>

3. 스위치 값 반환 또는 할당:

const grade = 'A';
// 잘못된 부분, 스위치 문을 변수에 할당하려고 시도했습니다
final numberGrade = switch (grade) {
 case 'A' => return 100;
 case 'B' => return 90;
 case 'C' => return 80;
 case 'D' => return 70;
};
// 스위치 문 사용 및 후에 할당을 수행한 올바른 방법
final int numberGrade2;
switch (grade) {
 case 'A':
   numberGrade2 = 100;
 case 'B':
   numberGrade2 = 90;
 break;
   case 'C':
 numberGrade2 = 80;
 break;
   case 'D':
 numberGrade2 = 70;
 default:
   numberGrade2 = 60;
}
// 스위치 표현식 사용 및 직접 할당을 수행한 올바른 방법
final numberGrade3 = switch (grade) {
 'A' => 100,
 'B' => 90,
 'C' => 80,
 'D' => 70,
 _ => 60,
};

4. 함수 표현식, 함수 문:

// 올바른 코드 (이름 없이 함수를 표현식으로 작성해야 합니다)
final hii = (String x, String b) {
 print(x);
 print(b);
 return x;
};
// 잘못된 코드 (이름이 있는 함수는 문입니다)
final x = String hii2(String x, String b) {
 print(x);
 print(b);
 return x;
};

<div class="content-ad"></div>

5. collection if 및 collection for:

이들은 리스트, 맵 또는 세트 내에서만 사용됩니다. 세미콜론 없이 if와 for과 동일하게 작동하며 한 개의 표현식을 반환할 수 있습니다 (문장이 아님).

const y = 3;

final list = [
 5,
 4,
 if (y == 3) 2,
 if (y == 1) 1 else if (y == 2) 2 else 3,
 for (int i = 0; i < 10; i++) i,
];

final map = {
 'a': 1,
 'b': 2,
 if (y == 3) 'c': 3,
 if (y == 1) 'd': 1 else if (y == 2) 'e': 2 else 'f': 3,
 for (int i = 0; i < 10; i++) i: '1',
};

final set = {
 1,
 2,
 if (y == 3) 3,
 if (y == 1) 1 else if (y == 2) 2 else 3,
 for (int i = 0; i < 10; i++) i,
};

6. 화살표 함수는 표현식을 반환해야합니다.

<div class="content-ad"></div>

// 잘못된 방법: 화살표 함수에서 문을 사용함
var multiply = (a, b) => {return a * b}; // 에러
// 올바른 방법: 화살표 함수에서 표현식 사용
var multiply = (a, b) => a * b;

# 팁

값을 계산하고 매개변수로 전달하거나 변수에 저장해야 하는 상황에서 여러 문을 사용해야 하지만 먼저 작성하고 저장하고 싶지 않을 때는 표현식 함수를 정의하고 표현식 내에서 바로 호출할 수 있습니다:

const a = 1;
const b = 2;
final x = () {
 if (a == 1) return 1;
 if (b == 2) return 2;
 return a + b;
}();
print(x); // 1
print(() {
 if (a == 1) return 1;
 if (b == 2) return 2;
 return a + b;
}()); // 1

<div class="content-ad"></div>

# 결론

문장과 표현식의 차이를 이해하는 것은 Dart나 다른 프로그래밍 언어를 습득하는 데 중요합니다. 문장은 작업을 수행하고 일반적으로 세미콜론이나 중괄호로 끝나지만, 표현식은 값을 평가하고 값이 예상되는 곳 어디에서든 사용할 수 있습니다. 이러한 구조를 인식하고 올바르게 활용하면 더 효율적이고 정확한 코드를 작성할 수 있으며, 일반적인 함정을 피하고 Dart의 강력한 기능을 활용할 수 있습니다. 이 기본 개념을 깊이 이해하기 위해 계속 연습하고 다양한 시나리오를 실험해 보세요.