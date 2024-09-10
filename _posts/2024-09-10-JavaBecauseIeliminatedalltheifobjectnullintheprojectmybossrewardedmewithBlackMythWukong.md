---
title: "Java object  null 조건문들을 모두 제거해서 보너스로 검은 신화 웨콩을 받았어"
description: ""
coverImage: "/assets/img/2024-09-10-JavaBecauseIeliminatedalltheifobjectnullintheprojectmybossrewardedmewithBlackMythWukong_0.png"
date: 2024-09-10 18:46
ogImage: 
  url: /assets/img/2024-09-10-JavaBecauseIeliminatedalltheifobjectnullintheprojectmybossrewardedmewithBlackMythWukong_0.png
tag: Tech
originalTitle: "Java Because I eliminated all the if (object  null) in the project, my boss rewarded me with Black Myth Wukong"
link: "https://medium.com/gitconnected/java-because-i-eliminated-all-the-if-object-null-in-the-project-my-boss-rewarded-me-with-29b316c32329"
isUpdated: false
---


<img src="/assets/img/2024-09-10-JavaBecauseIeliminatedalltheifobjectnullintheprojectmybossrewardedmewithBlackMythWukong_0.png" />

요즘 친구가 새 회사에 입사했어요. 프로젝트에는 객체가 null 인지 직접 판단하는 코드가 많았지만, 친구는 Optional을 사용하여 최적화했고 상사로부터 칭찬을 받았어요.

자세히 소개하겠습니다.

이전 글에서는 자바의 람다 표현식을 소개했었죠. 이번 글에서는 람다 표현식과 Optional을 결합하여 자바가 null을 더 우아하게 처리하는 방법을 소개하겠습니다.

<div class="content-ad"></div>

학생 객체와 학생 객체에 대한 Optional 래퍼가 있는 상황을 가정해 봅시다:

```java
public class OptionalTest {

    public static void main(String[] args) {
        Student student = new Student("Bob", 18);
        Optional<Student> studentOpt = Optional.ofNullable(student);
    }

    @Getter
    @AllArgsConstructor
    public static class Student {
        private String name;
        private Integer age;
    }
}
```

만약 람다와 함께 사용되지 않는다면 Optional은 원래 수고스러운 null 체크를 간단하게 만들어줄 수 없습니다. 아래와 같이 작성하는 것이 그 예시입니다:

```java
// 메서드 1 작성
if (student == null) {
  return UNKNOWN_STUDENT;
} else {
  return student;
}

// 메서드 2도 메서드 1만큼 나쁘게 작성됩니다
if (!studentOpt.isPresent()) {
  return UNKNOWN_STUDENT;
} else {
  return studentOpt.get();
}
```

<div class="content-ad"></div>

Optional을 람다와 함께 사용할 때에만 Optional의 진정한 힘을 발휘할 수 있습니다!

이제 자바8의 람다 + Optional과 전통적인 자바 간의 널 처리 차이 네 가지를 비교해보겠습니다.

상황 1 — 존재하는 경우 실행

```java
// if
if (student != null) {
  System.out.println(student);
}

// Optional
studentOpt.ifPresent(System.out::println);
```

<div class="content-ad"></div>

Case 2 — 존재하면 반환하고, 그렇지 않으면 오류를 반환하거나 예외를 throw합니다.

```js
// if
if (student == null) {
  return UNKNOWN_STUDENT;   // 또는 예외 throw
} else {
  return student;
}

// Optional
return studentOpt.orElse(UNKNOWN_STUDENT);
return studentOpt.orElseThrow(RuntimeException::new);
```

Case 3 — 존재하면 반환됩니다. 그렇지 않으면 함수로 생성합니다.

```js
// if
if (student == null) {
  return UNKNOWN_STUDENT;
} else {
  return generateWithFunction();
}

// Optional
return studentOpt.orElseGet(() -> generateWithFunction());
```

<div class="content-ad"></div>

사례 4 - 연속적인 널 체크

```js
public class OptionalCase {

    public static void main(String[] args) {
        Student student = new Student("Bob", 18,
                new Street("Main Street", new City("Los Angeles", new State("CA"))));

        System.out.println(getStateFromJava7(student));
        System.out.println(getStateFromJava8(student));
    }

    private static String getStateFromJava7(Student student) {
        // Java 7
        if (student != null) {
            Street street = student.getAddress();
            if (street != null) {
                City city = street.getCity();
                if (city != null) {
                    State state = city.getState();
                    if (state != null) {
                        String stateName = state.getName();
                        if (stateName != null) {
                            return stateName;
                        }
                        return "unknown";
                    }
                    return "unknown";
                }
                return "unknown";
            }
            return "unknown";
        }
        return "unknown";
    }

    private static String getStateFromJava8(Student student) {
        Optional<Student> studentOpt = Optional.ofNullable(student);
        // Java 8
        return studentOpt
                .map(Student::getAddress)
                .map(Street::getCity)
                .map(City::getState)
                .map(State::getName)
                .orElse("unknown");
    }

    @Getter
    @AllArgsConstructor
    static class Student {
        private String name;
        private Integer age;
        private Street address;
    }

    @Getter
    @AllArgsConstructor
    static class Street {
        private String name;
        private City city;
    }

    @Getter
    @AllArgsConstructor
    static class City {
        private String name;
        private State state;
    }

    @Getter
    @AllArgsConstructor
    static class State {
        private String name;
    }
}
```

위의 네 가지 상황에서 Optional + Lambda를 사용하면 ifElse 블록을 훨씬 적게 작성할 수 있음을 명확히 알 수 있습니다. 특히 상황 4의 경우, 전통적인 Java 작성 방법은 장황하고 이해하기 어렵지만, Optional + Lambda는 새롭고 명확하며 간결합니다.

Java Lambda와 Stream의 더 많은 사용 사례를 보려면 제 컬럼을 읽어보세요.

<div class="content-ad"></div>

다른 계속해서 업데이트되는 코너도 읽어보시는 걸 추천해요!