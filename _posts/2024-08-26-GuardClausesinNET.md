---
title: ".NET에서의 가드 절차 코드 가독성과 안정성을 높이는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-GuardClausesinNET_0.png"
date: 2024-08-26 19:28
ogImage: 
  url: /assets/img/2024-08-26-GuardClausesinNET_0.png
tag: Tech
originalTitle: "Guard Clauses in .NET"
link: "https://medium.com/codenx/guard-clauses-in-net-d6b70dc0c46c"
isUpdated: false
---


## 더 깔끔한 .NET 코드를 위해

가드 절은 당신이 작성한 메서드에 제공된 인수가 유효한지 확인한 후에 실제 비즈니스 로직을 계속하기 전에 확실하게 하는데에 간단하면서도 강력한 도구입니다. 이는 유명한 "화살표 코드" (중첩 if문)를 방지하고 코드를 더 깔끔하고 유지 보수가 쉽게 만들어줍니다.

이 글에서는 .NET에서 사용되는 다양한 일반적인 유효성 검사 시나리오를 효과적으로 처리하기 위해 사용자 정의 가드 절을 어떻게 생성하는지 살펴볼 것입니다.

# 가드 절이란?

<div class="content-ad"></div>

가드 절은 함수의 시작 부분에 배치된 조건 체크로, 입력 매개변수를 유효성 검사하는 역할을 합니다. 이 조건이 충족되지 않으면 함수가 조기에 종료되며, 종종 예외를 throw하여 이후 로직을 실행하지 않습니다. 이 접근 방식은 코드를 깔끔하게 유지하고 깊은 중첩을 방지하는 데 도움이 됩니다.

## 용어 '가드 절' 이해: 궁금증 해결

Q: 왜 '가드' 절이라고 부르는 건가요?

A: '가드' 절이라는 용어는 이 '가드'가 어떤 역할을 하는지 나타내기 때문에 사용됩니다. 이는 잘못된 데이터가 전달되지 않도록 메서드의 무결성을 '보호'하며, 뒤이어 나오는 로직을 보호합니다. 이 접근 방식은 유효한 입력만 허용하여 코드의 하단에서의 에러나 예상치 못한 동작을 방지합니다.

<div class="content-ad"></div>

Q: 이러한 절을 "가드(guard)" 대신 "유효성 검사(validate)"라고 부를 수 있나요?

A: "유효성 검사(validate)"로도 이름을 지을 수 있지만, "가드(guard)"라는 용어는 특히 무언가 나쁜 일이 일어나기 전에 예방하는 개념을 명확히 전달합니다. "유효성 검사(validate)"는 주로 정확성을 확인하는 것을 의미하지만, "가드(guard)"는 조건이 충족되지 않을 때 예외를 발생시키는 등의 행동을 함께 함을 시사합니다. 이러한 방어적인 행위에는 "가드"라는 용어가 더 적합합니다.

## 가드 절 없는 예제

이제 비어 있지 않은 이름과 양수인 나이가 필요한 Student 클래스를 만드는 시나리오를 살펴보겠습니다.

<div class="content-ad"></div>

```js
public class Student
{
    public string Name { get; private set; }
    public int Age { get; private set; }

    public Student(string name, int age)
    {
        if (string.IsNullOrWhiteSpace(name))
        {
            throw new ArgumentException("Name cannot be null, empty, or whitespace.", nameof(name));
        }

        if (age <= 0)
        {
            throw new ArgumentOutOfRangeException(nameof(age), "Age must be a positive integer.");
        }

        Name = name;
        Age = age;
    }
}
```

이 접근 방식은 작동하지만 여러 메서드나 클래스에 유사한 확인을 추가해야 하는 경우 불편할 수 있습니다. 이것이 사용자 정의 가드 절을 사용하는 곳입니다.

# 사용자 정의 가드 절 생성

코드를 더 재사용 가능하고 유지 관리 가능하게 만들기 위해 일반적인 유효성 검사에 대해 메서드가 포함된 정적 Ensure 클래스를 만들 수 있습니다. 이를 통해 반복적인 유효성 검사 로직을 한 줄의 코드로 대체할 수 있게 됩니다.


<div class="content-ad"></div>

```js
public static class Ensure
{
    public static void NotNullOrWhiteSpace(string value, string parameterName)
    {
        if (string.IsNullOrWhiteSpace(value))
        {
            throw new ArgumentException($"{parameterName} cannot be null, empty, or whitespace.", parameterName);
        }
    }

    public static void Positive(int value, string parameterName)
    {
        if (value <= 0)
        {
            throw new ArgumentOutOfRangeException(parameterName, $"{parameterName} must be a positive integer.");
        }
    }
}
```

## 학생 클래스 리팩터링

사용자 정의 가드 절을 추가한 후에는 학생 클래스를 이러한 메서드를 사용하도록 리팩터링할 수 있습니다.

**<img src="/assets/img/2024-08-26-GuardClausesinNET_0.png" />**

<div class="content-ad"></div>

## 가드 절을 사용하는 장점

- 가독성 향상: 유효성 검사에 의한 주요 논리가 가려지지 않습니다.
- 재사용성: 공통 유효성 검사 논리가 중앙에 집중되어 업데이트 또는 확장이 쉬워집니다.
- 유지보수성: 유효성 검사 논리 변경을 한 곳에서 수행하여 버그 발생 위험이 줄어듭니다.

## 가드 절 확장

Ensure 클래스를 확장하여 null 개체 확인, 범위 유효성 검사, 비어 있지 않은 컬렉션 확인 등 다른 일반적인 유효성 검사 시나리오를 처리할 수 있습니다.

<div class="content-ad"></div>

여기는 범위를 유효성 검사하기 위해 Ensure 클래스를 확장하는 예제입니다.

```js
public static class Ensure
{
    public static void InRange(int value, int min, int max, string parameterName)
    {
        if (value < min || value > max)
        {
            throw new ArgumentOutOfRangeException(parameterName, $"{parameterName} must be between {min} and {max}.");
        }
    }
}
```

공통 유효성 검사 로직을 재사용 가능한 메서드로 추상화함으로써, 더 깔끔하고 유지보수하기 쉬운 코드를 작성할 수 있습니다. 같은 유효성 검사 논리를 반복해서 작성해야 할 때는 사용자 정의 가드 절을 만들어 코드베이스를 간편하게 만드는 것을 고려해보세요.

이 정보가 유익했기를 바라며. 🌟 즐거운 배움의 여정이 되길 바랍니다!

<div class="content-ad"></div>

📚 이와 같은 통찰을 더 보시려면 Merwan Chinta 님을 👏 팔로우 👉 해보세요!