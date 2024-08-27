---
title: "Java Stream API Find와 Match로 터미널 연산 하는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-JavaStreamAPIMasteringTerminalOperationswithFindandMatch_0.png"
date: 2024-08-26 18:51
ogImage: 
  url: /assets/img/2024-08-26-JavaStreamAPIMasteringTerminalOperationswithFindandMatch_0.png
tag: Tech
originalTitle: "Java Stream API Mastering Terminal Operations with Find and Match"
link: "https://medium.com/gitconnected/java-stream-api-mastering-terminal-operations-with-find-and-match-1c178ae33c89"
isUpdated: true
updatedAt: 1724743420065
---


## 요소 스트림을 처리하는 일반적인 이유 중 하나는 특정 항목을 찾거나 일치시키는 것입니다. Stream API는 이를 위해 설계된 다양한 메서드를 제공합니다. 이 튜토리얼에서는 실제 예제와 함께 이러한 작업을 수행하는 방법에 대해 안내하겠습니다.

![image](/assets/img/2024-08-26-JavaStreamAPIMasteringTerminalOperationswithFindandMatch_0.png)

스트림 작업은 중간 작업과 최종 작업으로 나뉩니다. 최종 작업은 스트림을 소비하고 결과를 생성합니다. 이 결과는 새로운 컬렉션, 모든 요소의 결합 값 또는 일부 요소의 값일 수 있습니다. 이 튜토리얼에서 다루어질 모든 작업은 최종 작업입니다.

중간 작업에 대한 자세한 내용은 여기에서 찾을 수 있습니다:

<div class="content-ad"></div>

## 첫 번째 항목 찾기 작업

주어진 스트림에서 첫 번째 요소를 검색해야 할 때, 다음 메서드를 사용합니다:

```js
Optional<T> findFirst();
```

이 메서드는 요소를 직접 반환하는 대신에 Optional 객체 내에 감싸줍니다. 스트림이 비어있는 경우에는 빈 Optional을 반환하여 NullPointerException의 위험 없이 안전하게 검사 및 사용이 가능합니다.

<div class="content-ad"></div>

```java
Optional<Object> 예제1 = Stream.empty()
        .findFirst();
// Optional.empty
```

findFirst 자체는 NullPointerException을 throw하지 않지만, 스트림의 첫 번째 요소가 null이면 Optional로 래핑되어 적절히 처리되지 않으면 NullPointerException이 발생할 수 있습니다. null 값을 피하기 위해 findFirst 또는 다른 작업을 적용하기 전에 filter를 사용하여 스트림에서 null 요소를 제외하십시오.

```java
Optional<Integer> 예제2 = Stream.of(null, 2, 3)
        .filter(Objects::nonNull) // filter를 사용하지 않으면 NPE가 발생합니다
        .findFirst();
// Optional[2]
```

이 방법은 정의된 경우 스트림의 순서를 준수합니다.


<div class="content-ad"></div>

```js
Optional<Integer> example3 = Stream.of(1, 2, 3)
        .findFirst();
// Optional[1]
```

스트림에 순서가 정의되지 않은 경우 이 메소드를 사용할 때 주의해야 합니다. 아래 예시에서는 Set에서 스트림을 생성하는데, 알다시피 Set은 어떤 순서도 유지하지 않습니다. 결과적으로 findFirst 메소드의 결과는 예측할 수 없습니다.

```js
Optional<Integer> example4 = Set.of(1, 2, 3).stream()
        .findFirst();
// Optional[1] 또는 Optional[2] 또는 Optional[3]
```

## Find Any 연산

<div class="content-ad"></div>

findAny은 스트림에서 임의의 요소를 반환하며 스트림이 정렬되어 있든 아니든 예측할 수 없는 결과를 생성할 수 있습니다. 정확한 요소가 일관되지 않게 반환되는 경우, 병렬 처리를 위한 유연성을 제공합니다.

```js
Optional<T> findAny();
```

스트림 구현은 데이터를 무작위로 선택하지 않으므로, 많은 경우 - 특히 순차적으로 실행하는 경우 - findAny로 반환되는 요소는 첫 번째 요소로 보일 수 있습니다.

```js
Optional<Integer> example5 = Stream.of(1, 2, 3)
        .findAny();
// Optional[1]
```

<div class="content-ad"></div>

두 메소드 모두 short-circuiting 작업이며, 스트림 처리는 요소가 발견되는 즉시 중지됩니다.

## Any Match 작업

anyMatch 메소드는 스트림에서 전달된 매개변수로 전달된 술어와 일치하는 요소가 있는지를 확인합니다:

```js
boolean anyMatch(Predicate<? super T> predicate);
```

<div class="content-ad"></div>

이렇게 사용할 수 있어요:

```js
boolean example6 = Stream.of(1, 2, 3)
        .anyMatch(n -> n == 1);
// true

boolean example7 = Stream.of(2, 3)
        .anyMatch(n -> n == 1);
// false

boolean example8 = Stream.<Integer>empty()
        .anyMatch(n -> n == 1);
// false
```

## 모두 일치하는 작업

스트림의 모든 요소가 특정 조건과 일치하는지 확인하려면 allMatch 메서드를 사용합니다.

<div class="content-ad"></div>

```js
boolean allMatch(Predicate<? super T> predicate);
```

다양한 상황에서의 작동 방식은 다음과 같습니다:

```js
boolean example9 = Stream.of(1, 1, 1)
        .allMatch(n -> n == 1);
// true

boolean example10 = Stream.of(1, 1, 2)
        .allMatch(n -> n == 1);
// false

boolean example11 = Stream.of(2, 2, 2)
        .allMatch(n -> n == 1);
// false

boolean example12 = Stream.<Integer>empty()
        .allMatch(n -> n == 1);
// true
```

스트림이 비어있을 때 메소드는 true를 반환한다는 점을 강조하는 게 중요합니다.

<div class="content-ad"></div>

## None Match 연산

만약 스트림이 지정된 조건과 일치하지 않는 요소만 포함하도록 하려면, 다음 메서드를 사용합니다:

```js
boolean noneMatch(Predicate<? super T> predicate);
```

어떻게 작동하는지 확인해보겠습니다:

<div class="content-ad"></div>

```java
boolean example13 = Stream.of(2, 2, 2)
        .noneMatch(n -> n == 1);
// true

boolean example14 = Stream.of(1, 1, 2)
        .noneMatch(n -> n == 1);
// false

boolean example15 = Stream.of(1, 1, 1)
        .noneMatch(n -> n == 1);
// false

boolean example16 = Stream.<Integer>empty()
        .noneMatch(n -> n == 1);
// true
```

이 메소드는 빈 스트림에 대해 호출되면 true를 반환합니다.

anyMatch, allMatch, noneMatch 세 가지 match 메소드는 모두 쇼트서킷 연산이며, 결과를 일찍 판단할 수 있으면 스트림의 모든 요소를 평가하지 않을 수 있습니다.

IntStream, LongStream 및 DoubleStream 인터페이스는 모두 제시된 메소드를 제공하며, 일반 Stream 인터페이스에서와 동일하게 작동합니다.


<div class="content-ad"></div>

위의 코드는 여기서 확인할 수 있습니다:

## 결론

이 자습서에서 다룬 방법들은 코드를 보다 간결하고 명확하게 만드는 데 도움을 줍니다. 일반적으로 간단하지만, 이러한 방법 중 일부는 특정 주의 사항이 딸려 있습니다. 이 자습서가 여러분이 가지고 있을 수 있는 의문을 해소해 드렸기를 바랍니다.

읽어 주셔서 감사합니다! 이 글이 마음에 드셨다면 좋아요와 팔로우를 눌러 주세요. 궁금한 점이나 제안 사항이 있으면 언제든 댓글을 남기거나 LinkedIn 계정을 통해 연락해 주세요.