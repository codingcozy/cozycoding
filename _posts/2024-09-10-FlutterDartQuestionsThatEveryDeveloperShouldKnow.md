---
title: "모든 개발자가 알아야 할 Flutter와 Dart 질문들"
description: ""
coverImage: "/assets/img/2024-09-10-FlutterDartQuestionsThatEveryDeveloperShouldKnow_0.png"
date: 2024-09-10 19:52
ogImage: 
  url: /assets/img/2024-09-10-FlutterDartQuestionsThatEveryDeveloperShouldKnow_0.png
tag: Tech
originalTitle: "FlutterDart Questions That Every Developer Should Know."
link: "https://medium.com/stackademic/flutter-dart-questions-that-every-developer-should-know-364fe905dff4"
isUpdated: false
---


안녕하세요. 멋진 다트 문제 몇 가지를 다뤄볼 건데요, 이건 인터뷰 보기 전에 모두 알아두면 좋은 내용들입니다.

![이미지](/assets/img/2024-09-10-FlutterDartQuestionsThatEveryDeveloperShouldKnow_0.png)

# 안녕하세요 여러분,

첫 번째 질문으로 시작하겠습니다.

<div class="content-ad"></div>

Q1) 다음 프로그램의 출력을 예측하세요.

```js
void main() {
  int x = 1;
  fun(x);
  print(x);
}

void fun(int x) {
  x++;
}
```

만약 출력이 1이라고 생각하신다면, 올바른 길 위에 계신건가요! 다른 문제가 더 기다리고 있어요! 하지만 틀렸다고 생각하신 분들을 위해 걱정하지 마세요 — 다음 문제를 시도해보고, 함께 두 문제의 해결책을 논의하겠습니다.

![image](https://miro.medium.com/v2/resize:fit:956/1*1vzUG4Ui9xmWSpRrGDZ9Jw.gif)

<div class="content-ad"></div>


Q2) 다음 프로그램의 출력을 예측해보세요.

```js
void main() {
  List<int> arr = [1,2,3,4,5];
  fun1(arr);
  print(arr);
}

void fun(List arrayCopy){
  arrayCopy.add(6);
}
```

그렇다면 출력이 다음과 같다고 생각했다면

```js
[1,2,3,4,5]
```

<div class="content-ad"></div>

그러면 중요한 Dart 개념을 놓치고 있는지도 모릅니다. 그렇다면 이 출력물에 무엇이 잘못됐을까요? 질문 1에서 적용된 규칙에 따르면, 두 질문의 유일한 차이점은 질문 1에서 int 데이터 유형을 사용하고, 질문 2에서 List 데이터 유형을 사용했다는 것 뿐입니다. 그러나 출력물은 다음과 같습니다 -

```js
[1,2,3,4,5,6]
```

<img src="https://miro.medium.com/v2/resize:fit:996/1*4y1nDJWgOgV475U0JcQAXg.gif" />

<div class="content-ad"></div>

출력 결과의 비밀은 데이터 구조 유형 간의 차이에 있습니다. 구체적으로 기본 및 비 기본 데이터 구조입니다.

당신이 '어머나, 왜 그렇게 바보 같았을까?' 라고 생각하면 보너스 질문이 당신을 기다리고 있어요. 다른 분들은 해답을 함께 논의한 후에 질문에 도전해 보세요.

# 기본 데이터 구조

기본 데이터 구조는 Dart에 내장된 기본 유형입니다. 직접적으로 메모리에 저장되는 단일 값을 나타내며 직관적이고 간편합니다. 몇 가지 예시를 보여드리겠습니다:

<div class="content-ad"></div>

- int: 정수 값에 사용되며 1이나 42와 같은 값들이 포함됩니다.
- double: 3.14나 2.718과 같은 실수 값에 사용됩니다.
- bool: true나 false와 같은 부울 값들을 나타내는 데 사용됩니다.
- String: "Hello, World!"와 같이 문자열을 부호화하는 데 사용됩니다.

## 원시 데이터 구조 이외의 데이터 구조

원시 데이터 구조 이외의 데이터 구조들은 보다 복잡하며 Dart에서 기본 유형으로 직접 제공되지 않습니다. 이러한 데이터 구조들은 종종 원시 유형의 컬렉션이나 보다 복잡한 데이터 구조를 포함합니다. 예시로는 다음이 있습니다:

- List: [1, 2, 3]과 같이 순서가 있는 항목들의 컬렉션입니다.
- Set: '1, 2, 3'과 같이 순서가 없는 고유한 항목들의 컬렉션입니다.
- Map: '`key1`: `value1`, `key2`: `value2`'와 같이 키-값 쌍의 컬렉션입니다.

<div class="content-ad"></div>

답은 다음과 같은 주요 차이에 있습니다:

그래서 기본 데이터 구조는 즉 값에 의해 전달되고, 비 기본 데이터 구조는 참조에 의해 전달됩니다.

질문 1의 출력은 1입니다. 왜냐하면 이것은 함수 내에서 값에 의해 전달되는 기본 데이터 구조를 포함하기 때문입니다. 따라서 새 변수가 만들어져 함수로 전달되고, 이 새 변수의 값만 수정됩니다. 원래 값은 변경되지 않습니다.

그리고 질문 2의 출력은 [1, 2, 3, 4, 5, 6]입니다. 왜냐하면 이것은 함수 내에서 참조에 의해 전달되는 비 기본 데이터 구조를 포함하기 때문입니다. 결과적으로 동일한 변수의 참조가 함수로 전달되므로, 기존 변수의 값이 변경됩니다.

<div class="content-ad"></div>

# 보너스 문제

```js
void main() {
  List<int> arr = [1,2,3,4,5];
  fun1(arr);
  print(arr);
}

void fun(List<int> arrayCopy){
  List<int> arr =  [...arrayCopy];
  arr.add(6);
}
```

일단 생각해보세요.
출력은 [1, 2, 3, 4, 5] 입니다. 힌트 - 전개 연산자의 효과를 생각해 보세요. 전개 연산자에 대해 더 알고 싶다면 알려주세요.

즐거운 코딩하세요!!!

<div class="content-ad"></div>

Mohit Gupta
Flutter 개발자

# Stackademic 🎓

끝까지 읽어 주셔서 감사합니다. 떠나실 때:

- 작가를 칭찬하고 팔로우하기를 고려해 주세요! 👏
- 저희를 팔로우하기: X | LinkedIn | YouTube | Discord
- 다른 플랫폼 방문: In Plain English | CoFeed | Differ
- Stackademic.com에서 더 많은 콘텐츠 확인하기