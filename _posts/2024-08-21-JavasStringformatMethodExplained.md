---
title: "Java String.format() 메서드 사용 방법과 예시"
description: ""
coverImage: "/assets/img/2024-08-21-JavasStringformatMethodExplained_0.png"
date: 2024-08-21 17:48
ogImage: 
  url: /assets/img/2024-08-21-JavasStringformatMethodExplained_0.png
tag: Tech
originalTitle: "Javas String.format() Method Explained"
link: "https://medium.com/@AlexanderObregon/javas-string-format-method-explained-82707214c953"
isUpdated: false
---


<img src="/assets/img/2024-08-21-JavasStringformatMethodExplained_0.png" />

# 소개

Java의 String.format() 메소드는 개발자가 미리 정의된 형식에 변수 값을 삽입하여 동적 문자열을 생성할 수 있는 강력한 도구입니다. C 프로그래밍 언어에서 가져온 이 메소드는 숫자, 날짜 및 기타 객체를 포함한 다양한 유형의 데이터로 문자열을 구성하는 유연한 방법을 제공합니다. String.format()의 사용 방법을 이해하면 복잡한 문자열 작업을 다룰 때 코드의 가독성과 유지 관리성을 향상시킬 수 있습니다.

# String.format() 기본사항

<div class="content-ad"></div>

Java의 String.format() 메서드는 java.lang 패키지의 일부이며, 모든 Java 프로그램에 자동으로 import됩니다. 이 메서드는 형식을 지정한 다음 해당 형식에 하나 이상의 인수를 삽입하여 형식화된 문자열을 생성하는 방법을 제공합니다. 이 메서드는 제공된 형식과 인수를 기반으로 구성된 문자열을 반환합니다.

## 구문

String.format() 메서드의 기본 구문은 다음과 같습니다:

```js
String formattedString = String.format(String format, Object... args);
```

<div class="content-ad"></div>

- 형식: 텍스트와 형식 지정자를 포함하는 형식 문자열입니다.
- args: 형식 지정자에 해당하는 인수들입니다.

간단한 예제를 보여드리겠습니다:

```js
int value = 42;
String result = String.format("The value is %d.", value);
System.out.println(result); // 결과: The value is 42.
```

이 예제에서 %d는 형식 지정자로, String.format() 메서드에 정수 값 42를 문자열에 삽입하도록 지시합니다.

<div class="content-ad"></div>

# 형식 지정자 이해하기

형식 지정자는 String.format() 메서드 내에서 중요한 구성 요소로, 해당 인수가 출력 문자열에서 어떻게 표시되어야 하는지를 결정하는 자리 표시자 역할을 합니다. 각 형식 지정자는 % 문자로 시작하고, 해당 인수의 형식을 설명하는 문자 또는 문자 시퀀스로 구성됩니다.

## 기본 형식 지정자

다음은 Java에서 가장 일반적으로 사용되는 형식 지정자 중 일부입니다:

<div class="content-ad"></div>

- %d: 정수를 십진수로 형식화하는 데 사용됩니다. 예를 들어, 문자열에 정수를 삽입하려면 %d를 사용해야 합니다.

```js
int age = 25;
String formatted = String.format("나는 %d살이다.", age);
System.out.println(formatted); // 출력: 나는 25살이다.
```

- %f: 부동 소수점 숫자를 형식화하는 데 사용됩니다. %f는 기본적으로 소수점 뒤에 여섯 자리를 표시하지만 이를 조정할 수 있습니다.

```js
double price = 19.99;
String formatted = String.format("가격은 $%.2f", price);
System.out.println(formatted); // 출력: 가격은 $19.99
```

<div class="content-ad"></div>

- %s: 이 스펙별 formatter는 문자열을 형식화합니다. 특히 텍스트를 삽입하거나 여러 문자열을 결합해야 할 때 유용합니다.

```js
String firstName = "Alex";
String lastName = "Obregon";
String formatted = String.format("안녕, %s %s!", firstName, lastName);
System.out.println(formatted); // 출력: 안녕, Alex Obregon!
```

- %c: 이 스펙별 formatter는 단일 문자를 형식화하는 데 사용됩니다. 문자열 내에 문자를 포함해야 하는 상황에 이상적입니다.

```js
char letter = 'A';
String formatted = String.format("첫 번째 글자는 %c입니다.", letter);
System.out.println(formatted); // 출력: 첫 번째 글자는 A입니다.
```

<div class="content-ad"></div>

- %b: 이 스페셜 포맷터는 boolean 값 (true 또는 false)을 포맷하는 데 사용됩니다. boolean 값을 문자열로 변환합니다.

```js
boolean isJavaFun = true;
String formatted = String.format("Java가 재미있습니까? %b", isJavaFun);
System.out.println(formatted); // 출력: Java가 재미있습니까? true
```

- %x: 이 스페셜 포맷터는 정수를 16진수로 변환하여 포맷합니다. 16진수 표현이 일반적인 프로그래밍 컨텍스트에서 유용합니다.

```js
int value = 255;
String formatted = String.format("16진수 값: %x", value);
System.out.println(formatted); // 출력: 16진수 값: ff
```

<div class="content-ad"></div>

- %e: 이 서식 지정자는 부동 소수점 숫자를 과학적 표기법으로 서식화하는 데 사용됩니다.

```js
double number = 12345.6789;
String formatted = String.format("과학적 표기법: %e", number);
System.out.println(formatted); // 출력: 과학적 표기법: 1.234568e+04
```

## 플래그

플래그는 형식 지정자의 동작을 수정하는 데 추가할 수 있는 선택적 요소입니다. 플래그는 % 문자 바로 뒤에 포맷 지정자 앞에 배치됩니다.

<div class="content-ad"></div>

- + (플러스 기호): 이 플래그는 숫자 값에 양수 일지라도 부호(+ 또는 -)를 포함하도록 강제합니다.

```js
int positiveNumber = 42;
int negativeNumber = -42;
String formatted = String.format("양수: %+d, 음수: %+d", positiveNumber, negativeNumber);
System.out.println(formatted); // 출력: 양수: +42, 음수: -42
```

- 0 (제로 패딩): 이 플래그는 숫자를 공백 대신 제로로 채웁니다. 일정한 너비를 갖도록 숫자를 사용하는 데 자주 사용됩니다.

```js
int number = 7;
String formatted = String.format("%04d", number);
System.out.println(formatted); // 출력: 0007
```

<div class="content-ad"></div>

- , (쉼표): 이 플래그는 숫자에 그룹화 구분자(쉼표)를 추가하여 큰 숫자를 가독성 있게 포맷하는 데 유용합니다.

```js
int largeNumber = 1000000;
String formatted = String.format("%,d", largeNumber);
System.out.println(formatted); // 출력: 1,000,000
```

- - (왼쪽 정렬): 이 플래그는 출력을 지정된 너비 내에서 왼쪽으로 정렬하며 필요한 경우 오른쪽에 공백으로 채웁니다.

```js
String word = "Java";
String formatted = String.format("%-10s is cool.", word);
System.out.println(formatted); // 출력: Java       is cool.
```

<div class="content-ad"></div>

## 너비

너비는 출력에 쓰여야 하는 최소 글자 수를 지정합니다. 인수의 문자열 표현이 너비보다 짧으면 공백(또는 제로 플래그가 사용된 경우 0)으로 채워집니다.

- 최소 너비: 너비를 지정함으로써 출력이 적어도 해당 많은 글자를 차지하도록합니다.

```js
int value = 42;
String formatted = String.format("%5d", value);
System.out.println(formatted); // 출력: "   42"
```

<div class="content-ad"></div>

이 예제에서는 정수 42가 총 다섯 글자를 차지하도록 세 칸의 공백으로 패딩되었습니다.

## 정밀도

정밀도는 부동 소수점 숫자의 소수점 이하 자릿수를 제어하거나 문자열에서 사용할 최대 글자 수를 제어하는 데 사용됩니다.

- 부동 소수점 정밀도: 소수점 이하에 표시되는 자릿수를 제어합니다.

<div class="content-ad"></div>

```java
double pi = 3.141592653589793;
String formatted = String.format("%.4f", pi);
System.out.println(formatted); // 출력: 3.1416
```

여기서 소수점 이하 4자리까지 표시되는 것을 의미합니다.

- 문자열 정밀도: 이는 문자열에서 가져온 문자의 수를 제한합니다.

```java
String text = "Hello, World!";
String formatted = String.format("%.5s", text);
System.out.println(formatted); // 출력: Hello
```

<div class="content-ad"></div>

여기서는 문자열의 처음 다섯 글자만 출력됩니다.

## 인수 색인

어떤 경우에는 형식 문자열 내에서 다른 위치에서 인수를 재사용하고 싶을 수 있습니다. 인수 색인을 사용하면 특정 위치에서 어떤 인수를 사용해야 하는지 지정할 수 있습니다.

- 인수 색인: 이는 형식 지정자 앞에 숫자와 $ 기호를 넣어 나타냅니다.

<div class="content-ad"></div>

```java
String name = "Kaitlyn";
int age = 30;
String formatted = String.format("%2$s is %1$d years old.", age, name);
System.out.println(formatted); // Output: Kaitlyn is 30 years old.
```

이 예시에서 2$는 두 번째 인자인 이름이 먼저 사용되어야 함을 나타내고, 1$은 첫 번째 인자인 나이가 두 번째로 사용되어야 함을 나타냅니다.

## 플래그, 너비, 정밀도 결합

이러한 요소들을 결합하여 출력을 정확하게 제어하는 복잡한 형식 지정자를 만들 수 있습니다.

<div class="content-ad"></div>

```js
double balance = 12345.6789;
String formatted = String.format("Balance: $%,.2f", balance);
System.out.println(formatted); // Output: Balance: $12,345.68
```

이 예제에서 %,.2f는 쉼표 플래그, 정밀도 및 f 서식 지정자의 조합으로, 잘 서식이 지정된 통화 값이 생성됩니다.

# String.format()의 실용적인 예제

String.format() 메서드의 다양성은 다양한 데이터 유형 및 서식 지정 요구 사항을 처리할 수 있는 능력을 드러냅니다. 이 섹션에서는 숫자, 날짜 형식화 및 구조화된 보고서 작성을 포함한 다양한 시나리오에 대해 String.format()을 효과적으로 사용하는 방법을 보여주는 여러 실용적인 예제를 탐색하겠습니다.

<div class="content-ad"></div>

## 숫자 형식 지정

String.format()의 가장 일반적인 사용 사례 중 하나는 숫자를 다양한 방식으로 형식화하는 것입니다. 숫자 출력을 특정 형식으로 제시해야 하는 애플리케이션에서 특히 유용합니다.

- 선행 제로로 형식화: 숫자 식별자를 표시할 때, 일정한 너비를 유지하기 위해 숫자를 선행 제로로 채우는 것이 종종 필요합니다.

```js
int productId = 7;
String formatted = String.format("제품 ID: %05d", productId);
System.out.println(formatted); // 출력: 제품 ID: 00007
```

<div class="content-ad"></div>

여기서 %05d는 정수가 총 다섯 글자 너비로 0으로 채워지도록합니다.

- 백분율 형식: 일부 상황에서는 숫자를 백분율로 표시해야 할 수도 있습니다. String.format() 메서드를 사용하면 백분율 기호를 쉽게 추가하고 소수점 이하 자릿수를 제어할 수 있습니다.

```js
double successRate = 0.9235;
String formatted = String.format("성공률: %.2f%%", successRate * 100);
System.out.println(formatted); // 출력: 성공률: 92.35%
```

%.2f 서식 지정자는 소수점 이하 두 자리로 숫자를 포맷하고 그 뒤에 백분율 기호가 옵니다.

<div class="content-ad"></div>

- 큰 숫자 형식 지정: 대규모 숫자를 다룰 때, 천 단위 구분 기호로 쉼표를 추가하면 가독성이 크게 향상됩니다.

```js
long population = 7834000000L;
String formatted = String.format("세계 인구: %,d", population);
System.out.println(formatted); // 출력: 세계 인구: 7,834,000,000
```

%,d 서식 지정자는 숫자에 쉼표를 추가하여 읽기 쉽게 만듭니다.

## 날짜 형식 지정

<div class="content-ad"></div>

날짜 형식을 포맷하는 데 String.format()이 뛰어난 기능을 발휘하는 또 다른 영역은 요구 사항에 따라 다양한 날짜 및 시간 형식을 허용한다는 점입니다.

- ISO 형식으로 날짜 표시: ISO 형식(YYYY-MM-DD)은 많은 응용 프로그램에서 날짜를 나타내는 표준 방법입니다.

```java
LocalDate date = LocalDate.of(2024, 8, 11);
String formatted = String.format("Date: %tF", date);
System.out.println(formatted); // 출력: Date: 2024-08-11
```

%tF 지정자는 날짜를 ISO 8601 형식인 yyyy-MM-dd로 포맷합니다.

<div class="content-ad"></div>

- 24시간 형식으로 시간 표시: 24시간 형식으로 시간을 표시해야 하는 애플리케이션에는 String.format()을 사용하면 간편하게 처리할 수 있습니다.

```js
LocalTime time = LocalTime.of(14, 30, 45);
String formatted = String.format("시간: %tT", time);
System.out.println(formatted); // 결과: 시간: 14:30:45
```

위 예시에서 %tT 서식 지정자는 시간을 HH:mm:ss 형식으로 포맷합니다. 여기서 HH는 24시간 형식의 시간을 나타냅니다.

- 사용자 정의 날짜 및 시간 형식: 날짜와 시간을 사용자 정의 형식으로 결합하는 것은 여러 개의 형식 지정자를 사용하여 쉽게 수행할 수 있습니다.

<div class="content-ad"></div>

```java
LocalDateTime dateTime = LocalDateTime.of(2024, 8, 11, 14, 30, 45);
String formatted = String.format("이벤트 날짜와 시간: %1$tY년 %1$tm월 %1$td일 %1$tI시 %1$tM분 %1$tp", dateTime);
System.out.println(formatted); // 출력: 이벤트 날짜와 시간: 2024년 08월 11일 02시 30분 오후
```

이 예시는 동일한 인수를 여러 번 참조하기 위해 %1$를 사용하여 날짜와 시간의 복잡한 형식을 만드는 방법을 보여줍니다.

## 구조화된 보고서 생성하기

String.format()은 또한 보고서나 로그와 같은 구조화된 텍스트 출력을 생성하는 데 중요한 역할을 할 수 있습니다. 다양한 형식 지정자를 결합함으로써 명확하고 일관된 구조를 만들 수 있습니다.


<div class="content-ad"></div>

- 탭ular 레포트 생성: 간단한 레포트를 생성해야 할 때를 상상해봅시다. 다양한 아이템과 가격을 나타내는 간단한 보고서를 생성해야 하는 경우입니다.

```js
String item1 = "노트북";
double price1 = 999.99;
String item2 = "모니터";
double price2 = 199.49;
String item3 = "키보드";
double price3 = 49.99;

String report = String.format(
    "%-10s %10s%n%-10s %10.2f%n%-10s %10.2f%n%-10s %10.2f%n", 
    "아이템", "가격", 
    item1, price1, 
    item2, price2, 
    item3, price3
);

System.out.println(report);
``` 

Output:

```js
아이템       가격
노트북     999.99
모니터     199.49
키보드     49.99
```

<div class="content-ad"></div>

이 예제에서 %-10s 서식 지정자는 항목 이름을 10자 필드 내에서 왼쪽 정렬하고, %10.2f는 가격을 10자 필드 내에서 오른쪽 정렬하여 소수점 둘째 자리까지 표시합니다.

- 로그 항목 생성: 로깅이 필요한 애플리케이션의 경우 String.format()을 사용하여 일관된 형식의 로그 항목을 생성할 수 있습니다.

```java
String logLevel = "INFO";
String message = "시스템 시작이 완료되었습니다.";
LocalDateTime timestamp = LocalDateTime.now();

String logEntry = String.format("[%1$tF %1$tT] [%2$s] %3$s", timestamp, logLevel, message);
System.out.println(logEntry);
```

출력:

<div class="content-ad"></div>

```js
[2024-08-15 14:30:45] [INFO] System startup complete.
```

이 로그 항목 형식에는 타임스탬프, 로그 레벨 및 메시지가 모두 명확하고 일관된 방식으로 포맷되어 있습니다.

- 금융 데이터 처리: 금융 응용 프로그램은 특히 금액 값 처리 시에는 정확한 포맷팅이 필요합니다.

```js
double revenue = 1250349.75;
double expenses = 987654.32;
double profit = revenue - expenses;

String financialReport = String.format(
    "Revenue: $%,.2f%nExpenses: $%,.2f%nProfit: $%,.2f", 
    revenue, expenses, profit
);
System.out.println(financialReport);
```

<div class="content-ad"></div>

출력:

```js
수익: $1,250,349.75
지출: $987,654.32
이익: $262,695.43
```

이 예시는 String.format()을 사용하여 금융 수치를 형식화하는 방법을 보여줍니다. 쉼표를 추가하고 소수점 자릿수를 제어할 수 있습니다.

# 결론

<div class="content-ad"></div>

자바의 String.format() 메소드는 동적이고 잘 형식화된 문자열을 만드는 프로세스를 간소화하는 유연한 도구입니다. 숫자, 날짜를 형식화하거나 복잡한 보고서를 작성할 때, String.format()은 강력하고 유연한 방법을 제공하여 문자열 조작 요구 사항을 처리할 수 있습니다. 형식 지정자를 정확히 이해하고 활용함으로써, 여러분의 애플리케이션에서 다양한 형식 요구 사항을 충족하는 명확하고 가독성 좋은 유지보수 가능한 코드를 생성할 수 있습니다.

- 자바의 형식 지정자
- 자바 날짜와 시간 API

읽어 주셔서 감사합니다! 본 문서가 도움이 되었다면 하이라이팅, 클랩, 응답 또는 트위터/X를 통해 연락하는 것을 고려해 주시면 매우 감사하며, 이렇게 유지되는 무료 컨텐츠에 도움이 됩니다!