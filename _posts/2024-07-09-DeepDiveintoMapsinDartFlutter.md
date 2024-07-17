---
title: "Dart와 Flutter의 맵Map 깊이 파헤치기"
description: ""
coverImage: "/assets/img/2024-07-09-DeepDiveintoMapsinDartFlutter_0.png"
date: 2024-07-09 22:49
ogImage: 
  url: /assets/img/2024-07-09-DeepDiveintoMapsinDartFlutter_0.png
tag: Tech
originalTitle: "Deep Dive into Maps in Dart|Flutter"
link: "https://medium.com/@mohamed-abdo/deep-dive-into-maps-in-dart-flutter-511cd1f13849"
---


`<img src="/assets/img/2024-07-09-DeepDiveintoMapsinDartFlutter_0.png" />`

다트의 Map은 key-value 쌍의 모음입니다. 각 키를 정확히 하나의 값에 매핑합니다. Map을 반복할 수 있습니다.

# 다트/플러터에서 값으로 Map을 초기화합니다

데이터 유형이 `Map<dynamic, dynamic>`이거나 `Map` 또는 `var`로 설정됩니다.

<div class="content-ad"></div>

맵의 "키 데이터 형식, 값 데이터 형식" 표를 변경하려면 다음과 같이 합니다.


Map<String, int> map = {'zero': 0, 'one': 1, 'two': 2};

Map map = {'zero': 0, 'one': 1, 'two': 2};

var map = {'zero': 0, 'one': 1, 'two': 2};


"Map.fromIterable()" 생성자를 사용하여 반복 가능한 요소에서 새 맵을 만드는 방법입니다. 이 생성자는 Iterable과 각 요소에서 키와 값을 추출하는 두 개의 함수를 가져옵니다.


List<int> numbers = [0, 1, 2];

Map<String, int> map = Map.fromIterable(numbers,
    key: (k) => 'key ' + k.toString(), value: (v) => v);

// {key 0: 0, key 1: 1, key 2: 2}


<div class="content-ad"></div>

반복 가능한 객체로부터 새 맵을 만들려면 "Map.fromIterables()" 생성자를 사용하세요. 이 생성자는 각 리스트에서 키와 값을 추출하기 위해 두 개의 리스트를 사용합니다.

```js
List<String> letters = ['zero', 'one', 'two'];
List<int> numbers = [0, 1, 2];

Map<String, int> map = Map.fromIterables(letters, numbers);
// {zero: 0, one: 1, two: 2}
```

맵을 설정하여 업데이트할 수 없게 만들기

```js
Map map = Map.unmodifiable({1: 'one', 2: 'two'});
// {1: one, 2: two}
```

<div class="content-ad"></div>

만약 `Map[0]='zero';`와 같이 수정하려고 하면 오류가 발생합니다.

```js
Unhandled exception:
Unsupported operation: Cannot modify unmodifiable map
```

맵이 특정 키를 포함하는지 확인하는 방법은 다음과 같습니다. 이 방법은 맵이 지정된 키를 포함하면 true를 반환하고 그렇지 않으면 false를 반환합니다.

```js
Map map = { 0: 'zero', 1: 'one', 2: 'two'};
map.containsKey(1) //true
map.containsKey(3) //false
```

<div class="content-ad"></div>

# Dart/Flutter에서 key에 따라 맵 값을 업데이트하는 방법

값을 업데이트하는 세 가지 방법이 있어요

- 대괄호 [].
- update() 메서드.
- 키가 없을 때 새 객체/엔트리를 추가하려면 ifAbsent 매개변수를 사용한 update() 메서드.

```js
Map map = {0: 'zero', 1: 'one', 2: 'two'};

map[0] = 'ZERO';
print(map); // {0: ZERO, 1: one, 2: two}

map.update(1, (v) {
  return 'ONE';
});
print(map); // {0: ZERO, 1: ONE, 2: two}

map.update(3, (v) => 'THREE', ifAbsent: () => 'three');
print(map); // {0: ZERO, 1: ONE, 2: two, 3: three}
```

<div class="content-ad"></div>

# 맵 컬렉션 if 및 컬렉션 for

컬렉션 if 및 컬렉션 for를 사용하면 조건문 (if)과 반복문 (for)을 사용하여 동적으로 맵을 생성할 수 있습니다.

```js
var visable = true;
var map = {0: 'zero', 1: 'one', if (visable) 2: 'two'};
print(map); // {0: 'zero', 1: 'one', 2: 'two'}

var map2 = {for (int i = 0; i < 3; i++) i: i + 1};
print(map2); // {0: 1, 1: 2, 2: 3}
```

# 맵에 항목 추가하기

<div class="content-ad"></div>

맵에 항목(키-값 쌍)을 추가하는 방법은 다음 두 가지입니다:

- 대괄호 [] 사용
- putIfAbsent() 메서드 호출

```js
Map map = {1: 'one', 2: 'two'};

map[0] = 'zero';
print(map); // {1: one, 2: two, 0: zero}

map.putIfAbsent(3, () => 'three'); 
print(map); // {1: one, 2: two, 0: zero, 3: three}
```

# 맵에서 객체 제거

<div class="content-ad"></div>

세 가지 방법을 보여드리겠습니다:

- remove() 메서드를 사용하여 키-값 쌍을 키로 제거하는 방법을 보여드립니다.
- removeWhere() 메서드를 사용하여 조건을 만족하는 모든 항목을 제거하는 방법을 보여드립니다.
- clear() 메서드를 사용하여 Map에서 모든 항목을 제거하는 방법을 보여드립니다.

```js
Map map = { 0: 'zero', 1: 'one', 2: 'two'};

map.remove(0);
print(map); // {1: one, 2: two}

map.removeWhere((k, v) => v.startsWith('o'));
print(map); // {2: two}

map.clear();
print(map); // {}
```

# 여러 개의 Map을 병합하기

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경해주세요.

<div class="content-ad"></div>

```js
NoSuchMethodError: The getter 'keys' was called on null.
```

이를 해결하려면 널을 다루기 위해 널-안전 확장 연산자 `...?`를 사용합니다. 이 연산자는 하나의 추가 `?` 기호만으로 널인 Map을 자동으로 확인합니다:

```js
var combinedMap = {...?map1, ...?map2};
```

# Map을 순회하기


<div class="content-ad"></div>

다트 Map을 순회하는 방법에는 두 가지가 있어요:

- forEach() 메서드.
- entries 속성과 forEach() 메서드.

```js
Map map = {1: 'one', 2: 'two', 3: 'three'};

map.forEach((k, v) {
  print('{ key: $k, value: $v }');
});

map.entries.forEach((e) {
  print('{ key: ${e.key}, value: ${e.value} }');
});
```

Map 속성과 forEach() 메서드를 사용하여 키 또는 값에 대해 순회할 수 있어요.

<div class="content-ad"></div>

```js
map.keys.forEach((k) => print(k));
map.values.forEach((v) => print(v));
```

# 값으로 키 찾기

Dart Map은 키로 값을 가져오는 것을 지원합니다. 그러면 반대는 어떨까요?
keys 속성과 List의 firstWhere() 메서드를 사용하여 지정된 값으로 키를 가져올 수 있습니다.

```js
Map map = {1: 'one', 2: 'two', 3: 'three'};
var key = map.keys.firstWhere((k) => map[k] == 'two', orElse: () => null);
print(key);
// 2
```

<div class="content-ad"></div>

# Dart/Flutter에서 Map 정렬하기

이 예제는 Map을 값으로 정렬하는 방법을 보여줍니다. Map `fromEntries()` 메서드와 List `sort()` 메서드를 사용합니다.

```js
Map map = {1: 'one', 2: 'two', 3: 'three', 4: 'four', 5: 'five'};
var sortedMap = Map.fromEntries(
    map.entries.toList()
    ..sort((e1, e2) => e1.value.compareTo(e2.value)));
    
print(sortedMap); // {5: five, 4: four, 1: one, 3: three, 2: two}
```

다음 단계로 키(Key)로 Map을 정렬할 수 있습니다:

<div class="content-ad"></div>

- 'entries' 속성을 사용하여 Map의 모든 항목을 가져옵니다.
- 'toList()' 메서드를 사용하여 Entries를 List로 변환합니다.
- 'sort()' 메서드와 비교 함수(항목의 키에 대해)를 사용하여 List를 정렬합니다.
- 'fromEntries()' 메서드를 사용하여 정렬된 List를 Map으로 변환합니다.

```js
Map map = {3: 'three', 1: 'one', 4: 'four', 5: 'five', 2: 'two'};
var sortedByKeyMap = Map.fromEntries(
    map.entries.toList()..sort((e1, e2) => e1.key.compareTo(e2.key)));
print(sortedByKeyMap); // {1: one, 2: two, 3: three, 4: four, 5: five}
```

다음 단계로 값에 따라 Map를 정렬할 수 있습니다:

- 'entries' 속성을 사용하여 Map의 모든 항목을 가져옵니다.
- 'toList()' 메서드를 사용하여 Entries를 List로 변환합니다.
- 항목의 값에 대한 'sort()' 메서드와 비교 함수를 사용하여 List를 정렬합니다.
- 'fromEntries()' 메서드를 사용하여 정렬된 List를 Map으로 변환합니다.

<div class="content-ad"></div>

```js
Map map = {3: 'three', 1: 'one', 4: 'four', 5: 'five', 2: 'two'};
var sortedByValueMap = Map.fromEntries(
    map.entries.toList()..sort((e1, e2) => e1.value.compareTo(e2.value)));
print(sortedByValueMap); // {5: five, 4: four, 1: one, 3: three, 2: two}
```

우리는 SplayTreeMap을 사용하여 Map을 정렬할 수 있습니다. SplayTreeMap은 자체적으로 균형 잡힌 이진 트리에 기반한 맵 유형으로, 키를 자동으로 정렬된 순서로 반복합니다.

키는 생성자나 from() 메서드에서 전달된 비교 함수를 사용하여 비교됩니다. 비교 함수를 지정하지 않으면 객체는 비교 가능하다고 가정되며 Comparable.compareTo() 메서드를 사용하여 비교됩니다.

```js
Map map = {3: 'three', 1: 'one', 4: 'four', 5: 'five', 2: 'two'};
var sortedByKeyMap =
    new SplayTreeMap<int, String>.from(map, (k1, k2) => k1.compareTo(k2));
/* 동일한 결과 */
// sortedByKeyMap = new SplayTreeMap<int, String>.from(map);
print(sortedByKeyMap); // {1: one, 2: two, 3: three, 4: four, 5: five}
```  

<div class="content-ad"></div>

만약 SplayTreeMap을 값에 따라 정렬된 맵을 사용하고 싶다면, compare 함수를 바꾸면 됩니다.

```js
Map map = {3: 'three', 1: 'one', 4: 'four', 5: 'five', 2: 'two'};
var sortedByValueMap = new SplayTreeMap<int, String>.from(
    map, (k1, k2) => map[k1].compareTo(map[k2]));
print(sortedByValueMap); // {5: five, 4: four, 1: one, 3: three, 2: two}
```

# Map 변환하기

우리는 map() 메서드를 사용하여 모든 항목이 변환된 새로운 Map을 얻을 수 있습니다.

<div class="content-ad"></div>

예를 들어, 아래 코드는 모든 항목의 키와 대문자 값을 변경합니다.

```js
Map map = {1: 'one', 2: 'two', 3: 'three'};
var transformedMap = map.map((k, v) {
  return MapEntry('($k)', v.toUpperCase());
});
print(transformedMap); // {(1): ONE, (2): TWO, (3): THREE}
```

이 자습서에서는 Dart Map의 중요한 포인트 몇 가지를 배웠습니다. Map를 생성하고 초기화하는 방법, Map에 항목을 추가, 업데이트 및 삭제하는 방법, Map를 결합하는 방법, Map를 반복하는 방법, Map를 정렬하고 변형하는 방법 등을 다루었습니다.

이 블로그가 마음에 들었기를 바라고, 플러터를 시작하는 데 도움이 되었기를 기대합니다! 재미있었다면 손뼉 버튼을 꾹 눌러주시고 아래에 댓글을 남겨주세요.

<div class="content-ad"></div>

베즈코더의 튜토리얼 도움에 감사드립니다.

다음에 만나요.