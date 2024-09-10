---
title: "Java 람다 표현식에 대한 아마도 가장 이해하기 쉬운 설명"
description: ""
coverImage: "/assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_0.png"
date: 2024-09-10 18:48
ogImage: 
  url: /assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_0.png
tag: Tech
originalTitle: "Java Probably the Most Understandable Explanation of Lambda Expression Youll Ever See"
link: "https://medium.com/javarevisited/lambda-expression-in-java-439268b0ab97"
isUpdated: false
---



<img src="/assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_0.png" />

내 글은 누구나 열람할 수 있어요. 비회원 독자도 이 링크를 클릭하여 전체 글을 읽을 수 있어요.

# 1. 람다식이란?

자바 변수에 대해 "값"을 할당할 수 있고, 그 값을 활용할 수 있다는 걸 알고 있는 우리에게


<div class="content-ad"></div>

```java
int a = 1;
String s = "Hello";
System.out.println(s + a);
```

만약 자바 변수에 "코드 조각"을 할당하고 싶다면 어떻게 해야 합니까?

예를 들어, 오른쪽의 코드 블록을 codeBlock이라는 자바 변수에 할당하고 싶다면 어떻게 해야 합니까?

![이미지](/assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_1.png)


<div class="content-ad"></div>

자바 8 이전에는 이게 불가능했습니다. 그러나 자바 8 이후에 람다 기능을 사용하여 이를 할 수 있게 되었습니다. 직관적인 표현은 다음과 같습니다：

```java
codeBlock = public void doSomething(String s) {
        System.out.println(s);
}
```

물론, 이것은 매우 간결하게 쓸 수 있는 방법은 아닙니다. 따라서, 이 할당 작업을 더 우아하게 만들기 위해 몇 가지 불필요한 선언을 제거할 수 있습니다.

![이미지](/assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_2.png)

<div class="content-ad"></div>

이렇게 해서 '코드 조각'을 변수에 매우 우아하게 할당했습니다. 그리고 '이 코드 조각' 또는 '변수에 할당된 이 함수'은 람다 표현식입니다.

그러나 여기에는 여전히 하나의 질문이 남아 있습니다. 바로 변수 codeBlock의 타입이 무엇이어야 하는가인 것입니다.

Java 8에서 모든 람다 유형은 인터페이스이며, 람다 표현식 자체, 즉 '코드 조각'은 이 인터페이스의 구현이어야 합니다. 이것이 람다를 이해하는 데 중요한 포인트 중 하나라고 생각합니다. 요약하자면, 람다 표현식 자체가 인터페이스의 구현입니다. 이렇게 직접 말하면 여전히 약간 혼동스러울 수 있으니, 예제를 통해 계속 알아보도록 하겠습니다. 위의 codeBlock에 타입을 추가해 봅시다:

![이미지](/assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_3.png)

<div class="content-ad"></div>

이렇게 하나의 기능만 구현해야 하는 인터페이스를 "함수형 인터페이스"라고 합니다.

나중에 이 인터페이스에 인터페이스 함수를 추가하는 것을 방지하여, 여러 인터페이스 함수가 구현되어 "비 함수형 인터페이스"가 되는 것을 방지하기 위해, 이 인터페이스에 @FunctionalInterface 선언을 추가할 수 있습니다. 이렇게 하면 다른 사람들이 새로운 함수를 추가할 수 없습니다.

이렇게 하면 완전한 람다 표현식 선언을 얻을 수 있습니다.

![이미지](/assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_4.png)

<div class="content-ad"></div>


![Lambda Expression](/assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_5.png)

## 2. Lambda Expression의 기능은 무엇인가요?

가장 직관적인 효과는 코드를 극도로 간결하게 만드는 것입니다.

Lambda 표현식과 동일한 인터페이스의 전통적인 Java 구현을 비교할 수 있습니다:


<div class="content-ad"></div>

![Lambda Expression](/assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_6.png)

이 두 가지 작성 방법은 본질적으로 동등합니다. 그러나 분명히, Java 8의 작성 방법이 더 우아하고 간결합니다. 게다가 Lambda는 변수에 직접 할당될 수 있으므로 전통적인 Java에서는 인터페이스 구현과 초기화 정의를 명확히 해야 하는 반면, 함수에 Lambda를 직접 전달할 수 있습니다.

![Lambda Expression](/assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_7.png)

일부 경우에는 이 인터페이스 구현이 한 번만 사용되어야 합니다. 전통적인 Java 7은 InterfaceImpl을 구현하기 위해 "환경 오염" 인터페이스를 정의해야 합니다. 비교적 Java 8의 Lambda가 훨씬 깔끔해 보입니다.

<div class="content-ad"></div>

Lambda는 FunctionalInterface 라이브러리, forEach, stream(), 메서드 참조 및 기타 새로운 기능을 결합하여 코드를 더 간결하게 만듭니다!

이제 바로 예제로 넘어가보겠습니다.

Student의 정의와 List(Student)의 값이 주어진 것으로 가정합니다.

```java
@Getter
@AllArgsConstructor
public static class Student {
  private String name;
  private Integer age;
}

List<Student> students = Arrays.asList(
  new Student("Bob", 18),
  new Student("Ted", 17),
  new Student("Zeka", 18));
```

<div class="content-ad"></div>

자, 이제 students 목록에서 18세인 학생들의 이름을 모두 출력해야 합니다.

Lambda의 원본 쓰기 방법: 두 가지 함수형 인터페이스를 정의하고, 정적 함수를 정의한 후, 정적 함수를 호출하고 람다 표현식을 매개변수에 할당합니다.

```java
@FunctionalInterface
interface AgeMatcher {
  boolean match(Student student);
}

@FunctionalInterface
interface Executor {
  boolean execute(Student student);
}

public static void matchAndExecute(List<Student> students, AgeMatcher matcher, Executor executor) {
  for (Student student : students) {
    if (matcher.match(student)) {
      executor.execute(student);
    }
  }
}

public static void main(String[] args) {

        List<Student> students = Arrays.asList(
                new Student("Bob", 18),
                new Student("Ted", 17),
                new Student("Zeka", 18));

        matchAndExecute(students, 
                        s -> s.getAge() == 18, 
                        s -> System.out.println(s.getName()));
}
```

이 코드는 실제로 비교적 간결한데, 더 간결하게 할 수는 없을까요?

<div class="content-ad"></div>

물론, Java 8에는 많은 기능적 인터페이스를 정의하는 기능 인터페이스 패키지가 있습니다 (java.util.function (Java Platform SE 8)).

따라서 여기서 AgeMatcher와 Executor 두 개의 기능 인터페이스를 정의할 필요가 전혀 없습니다. Java 8 기능 인터페이스 패키지에서 그들의 인터페이스 쌍 때문에 Predicate(T)와 Consumer(T)를 사용할 수 있습니다. 실제로 그 정의는 AgeMatcher/Executor와 동일합니다.

![이미지](/assets/img/2024-09-10-JavaProbablytheMostUnderstandableExplanationofLambdaExpressionYoullEverSee_8.png)

단계 1: 단순화 — 기능 인터페이스 패키지 활용하기:

<div class="content-ad"></div>

```js
public static void matchAndExecute(List<Student> students, Predicate<Student> predicate, Consumer<Student> consumer) {
        for (Student student : students) {
            if (predicate.test(student)) {
                consumer.accept(student);
            }
        }
    }
```

matchAndExecute에 있는 for each 루프는 실제로 매우 귀찮습니다. 여기서는 Iterable의 forEach()를 사용할 수 있습니다. forEach() 자체가 Consumer(T) 매개변수를 받을 수 있습니다.

단계 2: 간단화 — foreach 루프를 Iterable.forEach()로 교체 :

```js
public static void matchAndExecute(List<Student> students, Predicate<Student> predicate, Consumer<Student> consumer) {
        students.forEach(s -> {
            if (predicate.test(s)) {
                consumer.accept(s);
            }
        });
    }
```

<div class="content-ad"></div>

matchAndExecute는 사실 List에서 수행되는 작업이므로 matchAndExecute를 제거하고 직접 stream() 기능을 사용할 수 있습니다. stream()의 여러 메서드는 Predicate(T) 및 Consumer(T)와 같은 매개변수를 허용합니다 (java.util.stream (Java Platform SE 8)). 위 내용을 이해하면 stream()을 매우 쉽게 이해할 수 있으며 추가 설명이 필요하지 않습니다.

3단계: 단순화 - 정적 함수 대신 stream() 사용:

```js
students.stream()
        .filter(s -> s.getAge() == 18)
        .forEach(s -> System.out.println(s.getName()));
```

원래의 람다 작성 방법과 비교하면 매우 간결합니다. 그러나 모든 학생의 정보를 출력하도록 변경하라고 요청하면 s 대신 `System.out.println(s);를 사용하여 메서드 참조를 통해 계속 단순화할 수 있습니다. 메서드 참조란 이미 작성된 다른 객체/클래스의 메서드로 람다 표현식을 대체하는 것입니다. 형식은 다음과 같습니다:

<div class="content-ad"></div>

<table> 태그를 Markdown 형식으로 변경하세요.

<div class="content-ad"></div>

아직 Java의 람다에 대해 논의하고 배울 부분이 있습니다. 예를 들어, 람다의 특성을 활용하여 병렬 처리를 수행하는 방법 등이 있습니다.

간략히 말하면, 개념을 전달하기 위해 일반적인 소개만 하고 있습니다. 인터넷에는 람다에 관한 많은 관련 자습서가 있으니 더 많이 읽고 연습해보세요. 시간이 지남에 따라 분명히 개선 사항이 있을 것입니다.

# 컬럼

아래 링크에서 자세히 알아보세요

<div class="content-ad"></div>

마지막으로, 만약 이 기사가 도움이 되었다면 👏좋아요와 팔로우를 눌러주세요. 감사합니다! ╰(*°▽°*)╯

저는 딜런입니다. 함께 더 나아가는 것을 기대하고 있어요. ❤️