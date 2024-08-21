---
title: "Java Stream API 중간 연산 탐색"
description: ""
coverImage: "/assets/img/2024-08-21-JavaStreamAPIExploringIntermediateOperations_0.png"
date: 2024-08-21 17:50
ogImage: 
  url: /assets/img/2024-08-21-JavaStreamAPIExploringIntermediateOperations_0.png
tag: Tech
originalTitle: "Java Stream API Exploring Intermediate Operations"
link: "https://medium.com/gitconnected/java-stream-api-exploring-intermediate-operations-a8fb7be4cac1"
isUpdated: false
---


## 중간 작업은 스트림 처리에서 중요하며, Stream API는 다양한 강력한 도구를 제공합니다. 이를 숙달하는 것은 모든 Java 개발자에게 필수적입니다. 함께 탐험해 봅시다.

![JavaStreamAPIExploringIntermediateOperations_0.png](/assets/img/2024-08-21-JavaStreamAPIExploringIntermediateOperations_0.png)

스트림 파이프라인이 무엇인지 정의하는 것부터 시작해봅시다. Java에서 스트림 파이프라인은 스트림에서 데이터를 처리하는 연산의 시퀀스입니다. 세 가지 주요 구성 요소로 구성됩니다:

- 소스: 스트림의 원본으로, 컬렉션, 배열, 생성기 함수 또는 I/O 채널과 같은 것입니다. 여기서 처리할 데이터가 나오는 곳입니다.
- 중간 작업: 스트림에서 데이터를 변환하거나 필터링하는 작업입니다. filter(), map(), sorted()와 같은 예가 있습니다. 중간 작업은 지연 실행되어 파이프라인이 최종 연산에 의해 실행될 때까지 처리를 수행하지 않습니다.
- 최종 연산: 파이프라인에서 마지막으로 실행되는 연산으로, 스트림 처리를 트리거합니다. forEach(), collect(), reduce()와 같은 예가 있습니다. 최종 연산이 호출되면 스트림이 소비되어 더 이상의 작업을 적용할 수 없습니다.

<div class="content-ad"></div>

지금, 스트림 중간 연산이 무엇인지 정의해 봅시다. 자바에서의 스트림 중간 연산은 스트림의 요소를 처리하고 다른 스트림을 생성하는 연산입니다. 이러한 연산은 스트림을 변환, 필터링, 검사 또는 수정하는 등의 작업을 수행하며 스트림을 소비하지 않습니다. 중간 연산의 주요 포인트는 중간 연산이 항상 스트림을 반환하여 추가 작업을 연결할 수 있도록 한다는 것입니다.

중간 연산의 주요 특징은 다음과 같습니다:

- 지연 평가: 중간 연산은 지연되어, 호출될 때 스트림의 요소를 실제로 처리하지 않습니다. 대신, 데이터에 적용될 연산 파이프라인을 빌드합니다. 이러한 파이프라인은 종단 연산이 호출될 때만 적용됩니다.
- 연결: 중간 연산이 스트림을 반환하기 때문에, 복잡한 처리 파이프라인을 구성하기 위해 연결할 수 있습니다. 이러한 연결은 명확하고 간결한 코드를 작성할 수 있게 해줍니다.
- 상태 없음 또는 상태 있는: 중간 연산은 각 요소가 독립적으로 처리되는 상태 없는 연산(map() 등)과 이전 요소에서 정보를 기억해야 하는 상태 있는 연산(distinct() 또는 sorted() 등)으로 나눌 수 있습니다.

다음 예시처럼 코드를 작성하면, 스트림을 실행하지 않을 것입니다. 왜냐하면 스트림 파이프라인이 종단 연산으로 끝나지 않았기 때문입니다:

<div class="content-ad"></div>

```js
stream()
  .intermediateOperation1()
  .intermediateOperation2()
  ...
  .intermediateOperationN()
```

방금 '끝나지 않는다'고 말했죠. 이름에서도 알 수 있듯이, 터미널 작업은 스트림을 종료합니다. 한 번 실행되면 스트림이 소비되고 재사용할 수 없습니다.

예를 들어, 이 코드도 컴파일되지 않습니다.

```js
stream()
  .intermediateOperation1()
  .terminalOperation1()
  .intermediateOperation2()

stream()
  .intermediateOperation1()
  .terminalOperation1()
  .terminalOperation2()
```

<div class="content-ad"></div>

터미널 연산이 스트림을 종료시키고 리스트와 같은 다른 형식으로 변환합니다. 스트림이 종료된 후에는 더 이상 중간 또는 터미널 연산을 호출할 수 없습니다. 왜냐하면 결과 객체가 더 이상 스트림이 아니기 때문입니다.

이 코드는 첫 번째 터미널 연산이 스트림을 닫아 더 이상 사용할 수 없게 만들기 때문에 예외를 발생시킵니다.

```js
Stream<T> stream = Stream.of(elemt1, element2)
                         .intermediateOperation1();

stream.terminalOperation(); -- stream is closed
stream.intermediateOperation2(); -- IllegalStateException
```

대부분의 경우, 스트림 파이프라인은 이 구조를 따릅니다:

<div class="content-ad"></div>

```js
stream()
  .intermediateOperation1()
  .intermediateOperation2()
  ...
  .intermediateOperationN()
  .terminalOperation()
```

하지만 파이프라인은 중간 작업들로 구성되어 첫 번째 예제처럼 구성될 수 있고, 최종 작업이 호출될 때에 실행됩니다.

## 상태 유지(stateful) vs. 상태 비유지(stateless) 작업

Java의 Stream API에서 작업은 상태 유지 및 상태 비유지의 두 가지 유형으로 분류할 수 있습니다. 이 둘의 차이를 이해하는 것은 Java에서 스트림을 효율적으로 사용하는 데 중요합니다.

<div class="content-ad"></div>

- Stateless 작업은 새로운 요소를 처리할 때 이전 요소의 상태를 유지하지 않습니다. 각 요소는 독립적으로 처리되며, 해당 작업은 이전 요소를 알 필요가 없습니다. 이로 인해 상태 정보를 유지할 필요가 없어 병렬로 효율적으로 처리될 수 있고 일반적으로 더 나은 성능을 보입니다.
- Stateful 작업은 스트림에서 요소를 처리하는 동안 상태 정보가 유지되어야 합니다. 요소를 처리하는 결과는 이전 요소에 의존할 수도 있고, 작업이 출력을 생성하기 전에 전체 스트림을 처리해야 할 수도 있습니다.

## limit 작업

limit 작업은 이름에서 알 수 있듯이, 스트림 파이프라인에서 처리되는 요소의 수를 제한합니다.

```js
Stream<T> limit(long maxSize);
```  

<div class="content-ad"></div>

다시 말해, 이는 매개변수로 지정된 요소 수만큼만 포함되도록 스트림을 잘라냅니다. 무한 및 유한 스트림 모두를 제한할 수 있습니다.

```js
List<Integer> example1 = Stream.iterate(1, n -> n + 1)
        .limit(3)
        .toList();
 // [1, 2, 3]
```

```js
List<Integer> example2 = Stream.of(1, 2, 3, 4, 5)
        .limit(3)
        .toList();
// [1, 2, 3]
```

이는 처리된 요소의 수를 추적해야 하기 때문에 상태 지닌 작업입니다. 스트림이 정렬되어 있을 때 병렬 스트림에 대해 더 비용이 많이 들 수 있습니다. 여러 스트림 세그먼트 간에 협력이 필요하여 올바른 요소 수가 전달되도록 보장하기 때문입니다.

<div class="content-ad"></div>

## 스킵 작업

스킵 작업은 이름에서 알 수 있듯이 스트림의 처음 몇 요소를 건너뛸 때 사용됩니다.

```js
Stream<T> skip(long n);
```

건너뛸 요소의 개수는 메소드에 전달된 매개변수에 의해 결정됩니다.

<div class="content-ad"></div>

```js
List<Integer> example3 = Stream.of(1, 2, 3, 4, 5)
        .skip(3)
        .toList();
// [4, 5]
```

스킵은 지나치는 요소를 추적해야 하므로 상태를 유지해야 하는 작업입니다. 특히 요소 순서를 유지하는 경우 병렬 처리 중에 더 비용이 많이 들 수 있습니다.

스킵과 리밋 작업을 동일한 파이프 라인에 결합하여 스트림의 크기를 정확히 정의할 수 있습니다.

```js
List<Integer> example4 = Stream.iterate(1, n -> n + 1)
        .skip(1)
        .limit(3)
        .toList();
// [2, 3, 4]
```

<div class="content-ad"></div>

## 중복 제거 작업

중복 제거 작업은 스트림에서 중복된 요소를 제거합니다.

```js
Stream<T> distinct();
```

다시 말해, 이 작업은 Object.equals() 메소드를 기반으로 한 유일한 요소만 포함하는 새로운 스트림을 반환합니다. 이 작업은 Object.hashCode()를 효율적으로 사용하여 요소를 추적하고 비교합니다. 따라서 비교되는 객체에는 equals()와 hashCode() 메소드가 올바르게 재정의되어 있어야 합니다.

<div class="content-ad"></div>

```js
List<Integer> example5 = Stream.of(1, 1, 2, 2, 3, 3)
        .distinct()
        .toList();
// [1, 2, 3]
```

이 작업은 이전에 만난 모든 요소를 추적하여 유일성을 보장해야 하므로 상태를 유지해야 하는 상태 유지 작업입니다. 특히 병렬 스트림에서 이 작업은 추가 오버헤드로 인해 자원을 많이 소비할 수 있습니다.

## sorted

sorted 메서드는 스트림의 요소를 정렬합니다. Stream 인터페이스는 이 메서드의 오버로드된 두 가지 버전을 제공합니다.

<div class="content-ad"></div>

```js
Stream<T> sorted();
```

이 버전은 인수를 취하지 않고 요소의 자연 순서를 사용하여 스트림을 정렬합니다. 예를 들어, 정수 스트림을 정렬하는 데 사용할 수 있습니다:

```js
List<Integer> example6 = Stream.of(2, 3, 5, 4, 1)
        .sorted()
        .toList();
// [1, 2, 3, 4, 5]
```

두 번째 버전은 Comparator를 인수로 받아 스트림에 대한 사용자 정의 순서를 정의할 수 있게 합니다.

<div class="content-ad"></div>

```js
Stream<T> sorted(Comparator<? super T> comparator);
```

이 메서드를 사용하여 역순으로 정렬할 수 있어요:

```js
List<Integer> example7 = Stream.of(2, 3, 5, 4, 1)
        .sorted(Comparator.reverseOrder())
        .toList();
// [5, 4, 3, 2, 1]
```

스트림에서 Comparator를 사용하는 방법을 자세히 알고 싶다면, 다양한 예제를 포함한 풍부한 강좌를 확인해보세요.

<div class="content-ad"></div>

sorted() 연산은 stateful 연산이며, 즉 정렬된 출력을 생성하기 전에 모든 요소를 처리해야 합니다. 이는 특히 병렬 스트림에서 사용할 때 리소스를 많이 소비하게 만듭니다.

## 순서 없는 연산

Java Stream API의 unordered() 메소드는 스트림에서 순서 제약 조건을 제거하는 데 사용됩니다. 기본적으로 몇몇 스트림, 특히 리스트와 같은 순서가 있는 원본에서 파생된 스트림은 요소의 순서를 유지합니다. 이러한 순서는 특히 병렬 처리에서 특정 연산의 성능에 영향을 줄 수 있습니다.

```js
S unordered();
```

<div class="content-ad"></div>

스트림에서 unordered()를 호출하면 요소가 처리되거나 생성되는 순서가 더 이상 중요하지 않다는 것을 나타냅니다. 특히 병렬 스트림을 사용할 때 효율적인 처리를 할 수 있게 해 줄 수 있습니다. 그 이유는 요소의 순서를 유지할 필요가 없어지기 때문입니다.

unordered()는 실제로 요소의 순서를 재정렬하지 않는다는 점을 주목해야 합니다. 이 메서드를 호출해도 이미 정렬되지 않은 스트림의 경우에는 영향이 없습니다.

스트림의 요소 순서를 고려하지 않고 처리하는 경우, 특히 병렬 스트림에서 unordered()를 사용하면 요소의 처리 방식에 있어서 더 유연해질 수 있어서 성능 향상에 도움이 될 수 있습니다.

```js
List<Integer> example8 = List.of(2, 3, 5, 4, 1)
        .stream()
        .unordered()
        .parallel() // 성능 향상 가능성 있음
        .toList();
```

<div class="content-ad"></div>

이 작업은 상태를 유지하지 않으며 스트림 요소 사이에 어떤 상태에도 의존하지 않습니다. 대신 요소의 순서를 무시해도 된다는 힌트를 제공합니다.

## peek 연산

peek 메서드는 Stream API의 흥미로운 연산 중 하나입니다.

```js
Stream<T> peek(Consumer<? super T> action);
```

<div class="content-ad"></div>

스트림의 요소를 수정하지 않고 관찰하는 것이 주된 목적입니다. 이는 동일한 요소를 가진 스트림을 반환하지만 각 요소에 대해 추가 동작을 수행하는 것으로, 매개변수로 전달된 Consumer에 의해 지정됩니다.

peek()는 디버깅에 유용할 수 있지만, 프로덕션 코드에 권장되지는 않습니다. 아래 예시에서는 peek를 사용하여 스트림의 각 요소를 정렬 전후에 인쇄하여 다른 단계에서 스트림의 상태에 대한 통찰력을 제공합니다.

```js
List<Integer> example9 = Stream.of(2, 3, 5, 4, 1)
        .peek(System.out::println)
        .sorted(Comparator.reverseOrder())
        .peek(System.out::println)
        .toList();
```

peek()는 스트림의 크기, 순서 또는 요소 유형을 변경하지 않는 상태 업무임을 유의해야 합니다.

<div class="content-ad"></div>

## 필터 작업

Stream API에서 가장 강력한 작업 중 하나인 필터() 작업을 소개합니다. 이 작업은 컬렉션 필터링하는 방법을 혁신적으로 변경했습니다. 불편한 if 문을 제거하고 코드를 더 간결하고 가독성 좋게 만들었습니다.

```js
Stream<T> filter(Predicate<? super T> predicate);
```

이 작업의 결과는 매개변수로 제공된 predicate와 일치하는 요소만 포함하는 스트림입니다. 아래 예시에서는 filter() 작업을 사용하여 스트림에서 홀수 숫자를 제거하였습니다.

<div class="content-ad"></div>

```java
List<Integer> example10 = Stream.of(1, 2, 3, 4, 5)
        .filter(n -> n % 2 == 0)
        .toList();
// [2, 4]
```

이것은 스트림 내의 요소 수를 줄일 수 있는 상태 없는 동작이지만, 그들의 순서나 유형을 변경하지는 않습니다.

## takeWhile 동작

takeWhile() 메서드는 Java의 Stream API에서 제공하는 강력한 도구입니다. 주어진 술어를 만족하는 한 스트림에서 요소를 취할 수 있게 합니다. 한 요소가 술어를 만족시키지 않으면, 동작이 중지되고, 심지어 해당 요소가 술어를 충족시킬 수 있더라도 더 이상의 요소는 처리되지 않습니다.

<div class="content-ad"></div>

```js
기본 Stream<T> takeWhile(Predicate<? super T> predicate) {...}
```

이 메소드는 단축 평가 방식으로 작동합니다. 즉, 요소가 조건을 충족하지 못하면 즉시 처리를 중지하고 더 이상의 요소를 조사하지 않습니다. 이 메소드는 스트림의 요소 순서를 존중하며, 프레디케이트가 더 이상 유효하지 않은 지점까지 요소 시퀀스를 처리하고 싶은 경우 유용합니다.

다음 예제를 통해 가장 잘 설명됩니다:

```js
List<Integer> example11 = Stream.of(1, 2, 3, 4, 5, 1)
        .takeWhile(n -> n < 4)
        .toList();
// [1, 2, 3]
```

<div class="content-ad"></div>

<img src="/assets/img/2024-08-21-JavaStreamAPIExploringIntermediateOperations_1.png" />

이 예시에서는 takeWhile() 메서드가 predicate인 `elements less than 4`을 사용합니다. 결과적으로 조건을 만족시키지 않는 요소인 4를 만나면 오퍼레이션이 중지됩니다. 4 이전의 모든 요소가 결과에 포함됩니다. 5 다음의 1은 조건을 만족하지만 takeWhile()은 조건이 더 이상 충족되지 않을 때 처리를 중단하므로 포함되지 않습니다.

다음은 첫 번째 요소에서 predicate를 충족하는 다른 예시를 살펴보겠습니다. 이 경우 빈 스트림이 생성됩니다.

```js
List<Integer> example12 = Stream.of(4, 1, 2, 3, 4, 5)
        .takeWhile(n -> n < 4)
        .toList();
// []
```

<div class="content-ad"></div>

<img src="/assets/img/2024-08-21-JavaStreamAPIExploringIntermediateOperations_2.png" />

takeWhile()은 요소를 술어식에 따라 처리하는 상태를 가지지 않는 작업이며 요소 간에 상태를 유지하지 않습니다.

스트림이 순서 없이 생성된 경우, takeWhile() 메서드의 결과는 결정론적이지 않을 수 있습니다. 아래 예제를 참조하세요:

```js
List<Integer> example13 = Set.of(1, 2, 3, 4, 5)
        .stream()
        .takeWhile(n -> n < 4)
        .toList();
// [] -- 첫 번째 실행
// [2, 3] -- 두 번째 실행
// [1, 2, 3] -- 세 번째 실행
```

<div class="content-ad"></div>

## "dropWhile" 작업

takeWhile() 작업의 대안인 dropWhile()을 알아봅시다. takeWhile()은 지정된 조건을 충족하는 경우에만 요소를 처리하는 반면, dropWhile()은 조건이 더 이상 충족되지 않을 때까지 요소를 삭제합니다. 이후 모든 후속 요소는 조건을 충족하더라도 결과에 포함됩니다.

```js
default Stream<T> dropWhile(Predicate<? super T> predicate) {...}
```

이 작업이 어떻게 작동하는지 이해하는 가장 좋은 방법은 예제를 사용해보는 것입니다.

<div class="content-ad"></div>

```js
List<Integer> example14 = Stream.of(1, 2, 3, 4, 5, 1)
        .dropWhile(n -> n < 4)
        .toList();
// [4, 5, 1]
```

![Java Stream API Exploring Intermediate Operations](/assets/img/2024-08-21-JavaStreamAPIExploringIntermediateOperations_3.png)

이 예제에서 dropWhile() 작업은 predicate `4보다 작은`을 만족하는 모든 요소를 제외합니다. 이 작업은 predicate을 만족하지 않는 첫 번째 요소인 숫자 4를 만나면 요소 제외를 중단합니다. 4 이후에도 predicate을 만족하는 또 다른 요소가 있더라도 이미 작업이 중단되었기 때문에 해당 요소는 제외되지 않습니다.

predicate이 맨 처음 요소에서 만족될 경우 요소가 제외되지 않습니다.

<div class="content-ad"></div>

```js
List<Integer> example15 = Stream.of( 4, 2, 3, 4, 5, 1)
        .dropWhile(n -> n < 4)
        .toList();
// [4, 2, 3, 4, 5, 1]
```

![Java Stream API Intermediate Operations](/assets/img/2024-08-21-JavaStreamAPIExploringIntermediateOperations_4.png)

dropWhile() 메서드는 요소를 술어와의 관계에 따라 평가하며 요소 사이에 상태를 유지하지 않는 상태가 없는 동작입니다. 스트림이 정렬되지 않은 경우, dropWhile() 메서드의 결과는 결정론적이 아닐 수 있습니다. 아래 예제를 참조해주세요:

```js
List<Integer> example16 = Set.of(1, 2, 3, 4, 5)
        .stream()
        .dropWhile(n -> n < 4)
        .toList();
// [5, 4] -- 첫 번째 실행
// [5, 4, 3, 2, 1] -- 두 번째 실행
// [4, 5] -- 세 번째 실행
```

<div class="content-ad"></div>

저는 takeWhile()과 dropWhile()을 한정 및 건너뛰기 작업과 유사한 것으로 생각해요. 다만, 이 둘은 술어(predicate)를 사용하는 것이 차이점입니다. 이 관점을 통해 그들의 동작을 더 명확하게 이해할 수 있어요.

## 병렬 작업

자바의 병렬 스트림은 여러 CPU 코어를 활용하여 데이터를 동시에 처리할 수 있게 해줘요. 큰 데이터셋을 다룰 때 작업을 빠르게 처리할 수 있도록, 스트림을 여러 부분으로 분할하고 병렬로 처리함으로써, 자바의 병렬 스트림은 파이프라인을 실행하는 데 걸리는 시간을 크게 줄일 수 있어요.

```javascript
S parallel();
```

<div class="content-ad"></div>

이 방법은 순차 스트림을 병렬로 변환합니다:

```js
List<Integer> example17 = Set.of(1, 2, 3, 4, 5)
        .stream()
        .parallel()
        .toList();
```

병렬 스트림은 성능을 향상시키는 Java의 강력한 도구입니다. 그러나 실행 순서, 부작용, 그리고 작업의 특성에 대한 신중한 고려 없이 사용해서는 안 됩니다.

## 순차 작업

<div class="content-ad"></div>

# 병렬 방식의 대응 방법은 순차 방식입니다:

```js
S sequential();
```

병렬 스트림을 순차적으로 바꿔줍니다.

```js
List<Integer> example18 = Set.of(1, 2, 3, 4, 5)
        .parallelStream()
        .sequential()
        .toList();
```

<div class="content-ad"></div>

Java Streams에서 작업할 때 연산의 순서는 성능에 상당한 영향을 미칠 수 있습니다. 여기 몇 가지 주요 고려 사항이 있습니다:

- 필터링 연산(filter(), takeWhile(), dropWhile())은 스트림 파이프라인에서 가능한 빨리 배치해야 합니다. 초기에 요소를 걸러내면 후속 연산이 처리해야 하는 요소 수가 줄어들어 성능 향상이 이루어집니다.
- 스트림에서 특정 수의 요소만 필요한 경우, limit()를 초기에 사용하면 특히 크거나 무한한 스트림에서 후속 연산의 작업 부담이 줄어들 수 있습니다.
- 정렬된(sorted()), 중복제거(distinct()), 건너뛰기(skip()), 그리고 제한(limit()) 연산은 정렬된 스트림에서 상태를 유지하는데 더 많은 연산이 필요할 수 있습니다. 특히 병렬 처리에서 이를 사용할 때는 사용하는 빈도와 상태 유지에 따른 비용을 고려해야 합니다.
- 요소의 순서가 중요하지 않은 경우, unordered() 메서드를 사용하여 병렬 스트림의 성능을 향상시킬 수 있습니다. 요소 처리 방식에 더 유연성을 부여하여 보다 효율적인 병렬 실행을 이끌어낼 수 있습니다.

<div class="content-ad"></div>

여기서 전체 코드를 찾을 수 있습니다:

## 결론

자바 프로그래머가 된다고 자신할 수 없는 것은 없습니다. Streams를 마스터하지 않은 채로. Stream API는 가장 어려운 작업조차 간단하게 만들 수 있는 다양한 중간 작업을 제공합니다. 이 튜토리얼에서 이러한 작업 중 대부분을 실제 예제와 함께 안내했습니다.

읽어 주셔서 감사합니다! 이 게시물이 마음에 들었다면 좋아요를 눌러 주시고 팔로우해주세요. 질문이나 제안 사항이 있으면 언제든지 댓글을 남기거나 LinkedIn 계정에서 연락해주세요.