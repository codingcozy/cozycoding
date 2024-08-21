---
title: "자바 스트림의 흐름을 따라가는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-FollowingtheflowofaJavaStream_0.png"
date: 2024-08-21 17:47
ogImage: 
  url: /assets/img/2024-08-21-FollowingtheflowofaJavaStream_0.png
tag: Tech
originalTitle: "Following the flow of a Java Stream"
link: "https://medium.com/@donraab/following-the-flow-of-a-java-stream-0bb617e3074f"
isUpdated: false
---


Java Stream에서 여러 번의 지연 작업을 시각화하는 방법입니다.

![Java Stream Flow](/assets/img/2024-08-21-FollowingtheflowofaJavaStream_0.png)

# Eager는 쉬우나, Lazy는 미로처럼

몇 년 전에, Stream 작업 이름 (filter, map, reduce)에 따라 이어지는 즉시 작업과 지연 작업 패턴의 차이점을 설명하는 블로그를 작성했습니다. Java Stream의 peek 메서드와 System.out.println을 활용하여, 지연 반복 패턴의 가장 어려운 부분인 실행 순서와 흐름을 설명했습니다.

<div class="content-ad"></div>

peek를 System.out.println과 함께 사용하는 것이 블로그에는 괜찮지만, 미래 유지보수자들이 코드를 실행하고 출력을 확인할 필요 없이 테스트에서 어떤 방식으로 작동하는지 인코딩하고 이해할 수 있는 방법이 있습니다.

# 출력하지 말고 어서트(assert)하자!

다음은 내가 상기 블로그에 작성한 코드와 유사한 코드인데, 여기서 Stream의 각 단계에 사용된 입력을 이해하기 위해 peek를 사용했습니다.

```js
@Test
public void lazyFilterMapReducePrint()
{
    List<Integer> list =
            List.of(1, 2, 3, 4, 5);

    Optional<String> lazy = list.stream()
            .peek(i -> System.out.println("filter: " + i))
            .filter(each -> each % 2 == 0)
            .peek(i -> System.out.println("map: " + i))
            .map(String::valueOf)
            .peek(i -> System.out.println("reduce: " + i))
            .reduce(String::concat);

    Assertions.assertEquals("24", lazy.orElse(""));
}
```

<div class="content-ad"></div>

이 코드 자체로는 Stream 파이프라인의 실행 순서에 대해 아무것도 알려주지 않습니다. 출력을 보려면 코드를 실행해야 합니다. 다음은 이 코드를 실행했을 때의 출력입니다.

```js
filter: 1
filter: 2
map: 2
reduce: 2
filter: 3
filter: 4
map: 4
reduce: 4
filter: 5
```

peek 코드를 출력에서 요소를 추가하는 List로 변환하고, 그것을 예상한 순서대로 정렬된 List와 비교하도록 할 수 있습니다.

```js
@Test
public void lazyFilterMapReduceAssert()
{
    List<Integer> list =
            List.of(1, 2, 3, 4, 5);

    MutableList<String> order =
            Lists.mutable.empty();

    Optional<String> lazy = list.stream()
            .peek(i -> order.add("filter: " + i))
            .filter(each -> each % 2 == 0)
            .peek(i -> order.add("map: " + i))
            .map(String::valueOf)
            .peek(i -> order.add("reduce: " + i))
            .reduce(String::concat);

    List<String> expectedOrder = List.of(
            "filter: 1",
            "filter: 2",
            "map: 2",
            "reduce: 2",
            "filter: 3",
            "filter: 4",
            "map: 4",
            "reduce: 4",
            "filter: 5");

    Assertions.assertEquals(expectedOrder, order);
    Assertions.assertEquals("24", lazy.orElse(""));
}
```

<div class="content-ad"></div>

이 코드에서는 정수 값 목록을 가져와서 먼저 Predicate에 대해 평가한 다음 짝수인지 테스트하고 필터링합니다. 숫자 2와 4가 테스트되면 즉시 다음 단계로 이동하여 String으로 매핑되고 마지막으로 합쳐져 결합된 String으로 반환됩니다. 

위의 코드의 eager 버전은 출력을 어설션으로 하는 코드입니다. 이 코드는 Eclipse Collections 유형과 eager 메서드를 사용하는데, 표준 Java Collection Framework에는 동등한 eager 동작이 없기 때문입니다. Eclipse Collections에서 Stream peek 메서드에 해당하는 것은 tap이라고 합니다.

```js
@Test
public void eagerSelectCollectReduceAssert()
{
    ImmutableList<Integer> list =
            Lists.immutable.of(1, 2, 3, 4, 5);

    MutableList<String> order =
            Lists.mutable.empty();

    Optional<String> eager = list
            .tap(i -> order.add("select: " + i))
            .select(each -> each % 2 == 0)
            .tap(i -> order.add("collect: " + i))
            .collect(String::valueOf)
            .tap(i -> order.add("reduce: " + i))
            .reduce(String::concat);

    List<String> expectedOrder = List.of(
            "select: 1",
            "select: 2",
            "select: 3",
            "select: 4",
            "select: 5",
            "collect: 2",
            "collect: 4",
            "reduce: 2",
            "reduce: 4");

    Assertions.assertEquals(expectedOrder, order);
    Assertions.assertEquals("24", eager.orElse(""));
}
```

유사한 기능을 하는 코드의 eager 버전은 먼저 모든 짝수를 선택(필터링)한 다음, 선택된 요소들을 String으로 변환하여 수집(map)하고, 마지막으로 이러한 요소를 최종 출력물로 줄입니다. 출력 순서는 eager 케이스에서의 메서드 호출 순서와 일치합니다. 이는 코드를 이해하고 추론하기 쉽게 만듭니다.

<div class="content-ad"></div>

Eclipse Collections에서의 게으른(Lazy) 동등 코드는 Java Stream 코드와 동일한 실행 순서를 가질 것입니다. 다음은 Eclipse Collections에서 LazyIterable 유형을 사용하여 동등한 코드입니다.

```js
@Test
public void lazySelectCollectReduceAssert()
{
    ImmutableList<Integer> list =
            Lists.immutable.of(1, 2, 3, 4, 5);

    MutableList<String> order =
            Lists.mutable.empty();

    Optional<String> lazy = list.asLazy()
            .tap(i -> order.add("select: " + i))
            .select(each -> each % 2 == 0)
            .tap(i -> order.add("collect: " + i))
            .collect(String::valueOf)
            .tap(i -> order.add("reduce: " + i))
            .reduce(String::concat);

    List<String> expectedOrder = List.of(
            "select: 1",
            "select: 2",
            "collect: 2",
            "reduce: 2",
            "select: 3",
            "select: 4",
            "collect: 4",
            "reduce: 4",
            "select: 5");

    Assertions.assertEquals(expectedOrder, order);
    Assertions.assertEquals("24", lazy.orElse(""));
}
```

# IntelliJ가 도와줍니다

Java Stream 흐름을 이해하는 문제는 충분히 일반적하다는 사실로 인해 JetBrains의 멋진 개발자들이 우리를 돕기 위해 Java Stream을 위한 특별한 디버깅 기능을 개발했습니다.

<div class="content-ad"></div>

이 도구의 훌륭한 점은 Stream의 여러 지점을 엿보기 위해 호출을 살펴볼 필요가 없다는 것입니다. 우리의 코드는 다음과 같이 간단하게 유지될 수 있습니다.

```js
@Test
public void lazyFilterMapReduce()
{
    ImmutableList<Integer> list =
            Lists.immutable.of(1, 2, 3, 4, 5);

    Optional<String> lazy = list.stream()
            .filter(each -> each % 2 == 0)
            .map(String::valueOf)
            .reduce(String::concat);

    Assertions.assertEquals("24", lazy.orElse(""));
}
```

## 디버깅 도구 사용하기

코드에 중단점을 설정하고 디버그 모드에서 코드를 실행합니다. 디버깅 탭에 "Trace Current Stream Chain"이라는 말풍선 도움 텍스트가 있는 이 버튼을 찾아봅니다.

<div class="content-ad"></div>


![JavaStream](/assets/img/2024-08-21-FollowingtheflowofaJavaStream_1.png)

스트림을 보는 두 가지 모드가 있습니다. 분할 모드에서는 스트림의 각 단계를 볼 수 있습니다.

먼저 filter를 살펴보겠습니다.

다음으로 map을 살펴보겠습니다.


<div class="content-ad"></div>

드디어 reduce를 살펴봅시다.

split 모드를 사용하는 것은 여러 단계로 이루어진 것 같아 조금 귀찮습니다. 따라서 한 번에 모든 단계를 보여주는 flat 모드로 전환하겠습니다.

이 뷰는 Stream 파이프라인의 각 단계 후에 입력이 어떻게 변하는지 보여줍니다. 하지만 실행 순서는 보여주지 않습니다. peek 접근 방식을 사용하면 어떤 종류의 출력을 통해 실행 순서를 확인할 수 있습니다.

# 최종 생각

<div class="content-ad"></div>

이 블로그가 도움이 되었으면 좋겠어요. 자바 스트림에서 peek를 사용하거나 이클립스 컬렉션에서 tap을 사용하여 게으른 코드의 흐름을 이해하는 데 도움을 받을 수 있어요. 자바 스트림에서는 또한 IntelliJ의 스트림 전용 디버깅 도구를 사용하여 파이프라인의 각 단계에서 입력과 출력을 분석할 수 있는 장점이 있어요. 이클립스 컬렉션의 LazyIterable 유형을 위한 유사한 디버깅 도구를 누군가 개발해주면 좋을 것 같죠.

읽어 주셔서 감사해요!

저는 이클립스 컬렉션 OSS 프로젝트의 창시자이자 공헌자입니다. 해당 프로젝트는 이클립스 재단에서 관리되고 있어요. 이클립스 컬렉션은 기여를 환영합니다.