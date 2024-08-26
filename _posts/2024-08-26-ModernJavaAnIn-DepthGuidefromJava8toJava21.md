---
title: "현대 자바 Java 8부터 Java 21까지 깊이 있는 안내"
description: ""
coverImage: "/assets/img/2024-08-26-ModernJavaAnIn-DepthGuidefromJava8toJava21_0.png"
date: 2024-08-26 18:45
ogImage: 
  url: /assets/img/2024-08-26-ModernJavaAnIn-DepthGuidefromJava8toJava21_0.png
tag: Tech
originalTitle: "Modern Java An In-Depth Guide from Java 8 to Java 21"
link: "https://medium.com/@akineralkan/modern-java-an-in-depth-guide-from-version-8-to-21-by-akiner-alkan-f89b50e13c72"
isUpdated: false
---


![이미지](/assets/img/2024-08-26-ModernJavaAnIn-DepthGuidefromJava8toJava21_0.png)

자바는 다재다능한 프로그래밍 언어로 전환의 여정을 거쳤습니다. Java 8부터 흥미로운 기능들이 개발자들의 코딩 방식을 변화시켰습니다. 함수를 더 잘 다룰 수 있는 간결한 람다 표현식부터 데이터 처리를 쉽게 만들어주는 Stream API까지, Java 8은 게임 체인저였습니다. 우리는 Java 21까지의 업데이트를 살펴볼 것입니다. 여기서 sealed 클래스는 클래스에 대한 더 많은 제어를 제공하며, records는 데이터 개체를 만드는 것을 아주 간단하게 만듭니다. 이 현대적인 Java 기능을 간단한 설명과 실용적인 예제로 살펴보는 여정에 참여해보세요. 이 글은 길고 내용이 많으므로, 편안한 읽기를 위해 여러 부분으로 나누어 읽는 것을 제안합니다. 또한 각 섹션 사이에 짧은 쉬는 시간을 갖는 것도 추천드립니다.

# Java 8

## 람다 표현식

<div class="content-ad"></div>

람다식은 익명 함수를 간결하게 표현하는 방법으로, 다른 함수에 인수로 전달하거나 함수형 프로그래밍 패러다임에서 사용될 수 있습니다. 별도의 메서드를 명시적으로 생성하지 않고 작은 인라인 함수를 정의할 수 있게 해줍니다. 람다식은 Java와 같은 함수형 프로그래밍을 지원하는 언어에서 흔히 사용됩니다.

```js
// 익명 내부 클래스를 사용한 전통적인 방식
  Runnable runnable = new Runnable() {
   @Override
   public void run() {
    System.out.println("Hello, world!");
   }
  };
```

```js
  // 람다식 사용
  Runnable lambdaRunnable = () -> {
   System.out.println("Hello, world!");
  };
```

## 스트림 API

<div class="content-ad"></div>

Stream API는 Java 8에서 소개된 기능으로, 데이터 컬렉션(예: 리스트, 배열)을 처리하는 더 함수적이고 선언적인 방법을 제공합니다. Stream을 사용하면 컬렉션의 요소에 대해 필터링, 매핑, 축소와 같은 다양한 작업을 더 간결하고 표현적인 방식으로 수행할 수 있습니다.

```java
List<String> fruits = Arrays.asList("apple", "banana", "orange", "grape", "kiwi");
```

```java
// Stream API를 활용하여 요소를 필터링하고 변환하는 예제
List<String> result = fruits.stream()
    .filter(fruit -> fruit.length() > 5)
    .map(String::toUpperCase)
    .collect(Collectors.toList());
System.out.println(result); // 출력: [BANANA, ORANGE]
```

Streams API로 많은 다양한 시나리오를 구현할 수 있습니다. 이 예제에서는 과일 목록에서 Stream을 생성하고, 5자 이상인 과일을 필터링하여 남은 요소를 대문자로 변환하고, 결과를 새 리스트로 수집합니다. Stream API를 사용하면 코드가 더 가독성이 높아지고 여러 작업을 연결할 수 있습니다.

<div class="content-ad"></div>

## 메소드 참조

메소드 참조는 메소드나 생성자를 참조하고 람다나 Stream API에서 사용하기 위한 단순한 구문을 제공하는 기능입니다. 이는 람다 식을 제공하는 대신 메소드의 동작을 복제하는 람다 표현식을 제공하는 대신 메소드의 이름으로 직접 참조할 수 있도록 함으로써 코드를 간단하게 만들어 줍니다.

```js
String[] words = {"apple", "banana", "cherry", "date", "elderberry"};
  
        // 명시적인 람다 식을 사용하여 정렬
        Arrays.sort(words, (a, b) -> a.compareToIgnoreCase(b));
```

```js
        // 메소드 참조를 사용하여 배열을 정렬
        Arrays.sort(words, String::compareToIgnoreCase);
```

<div class="content-ad"></div>

이 예제에서는 명시적 람다 표현식 `(a, b) -> a.compareToIgnoreCase(b)`이 배열을 정렬하는 데 사용됩니다. 두 가지 접근 방식 간의 비교를 보여주기 위해 이는 메소드 참조 버전과 함께 포함되어 있습니다.

## 기본 메소드

기본 메소드는 구현이 포함된 인터페이스 내에 정의된 메소드입니다. 기본 메소드를 사용하면 기존에 인터페이스를 구현하는 클래스와의 호환성을 유지하면서 인터페이스에 새로운 메소드를 추가할 수 있습니다. 기본 메소드는 모든 구현 클래스가 새로운 메소드에 대한 구현을 제공해야 하는 것을 강제하지 않고 인터페이스를 확장하는 방법을 제공합니다.

```java
interface Greeting {
   // 인터페이스의 기본 메소드
   default void greet() {
    System.out.println("인터페이스에서 안녕하세요!");
   }
}
```

<div class="content-ad"></div>

```js
  class Person implements Greeting {
   // 기본 greet() 메서드를 오버라이드할 필요가 없습니다.
  }
  
  public class Main {
   public static void main(String[] args) {
    Person person = new Person();
    person.greet(); // 기본 메서드 구현을 사용할 것입니다
   }
  }
```

이 예제에서 Greeting 인터페이스는 기본 메서드 greet()를 정의합니다. Person 클래스는 Greeting 인터페이스를 구현하지만 greet() 메서드의 자체 구현을 제공할 필요가 없습니다. 왜냐하면 기본 구현을 상속받기 때문입니다. person.greet() 호출은 인터페이스의 기본 메서드를 사용할 것입니다.

## 날짜 및 시간 API

Java 8에서 도입된 날짜 및 시간 API는 날짜 및 시간 관련 작업을 처리하는 포괄적인 방법을 제공하며 이전 java.util.Date 및 java.util.Calendar 클래스의 제한 사항과 복잡성을 해결합니다. 날짜, 시간, 기간, 지속 시간, 타임존 등을 나타내는 클래스가 포함되어 있습니다. 여기에는 Date and Time API를 사용하는 기본적인 개요와 예제가 포함되어 있습니다:

<div class="content-ad"></div>

```java
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
```

```java
public class DateTimeAPIExample {
   public static void main(String[] args) {
    // 현재 날짜, 시간 및 날짜-시간
    LocalDate currentDate = LocalDate.now();
    LocalTime currentTime = LocalTime.now();
    LocalDateTime currentDateTime = LocalDateTime.now();
    System.out.println("현재 날짜: " + currentDate);
    System.out.println("현재 시간: " + currentTime);
    System.out.println("현재 날짜-시간: " + currentDateTime);
    // 형식화 및 구문 분석
    DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
    String formattedDate = currentDate.format(dateFormatter);
    System.out.println("형식화된 날짜: " + formattedDate);
    String dateString = "2023-08-01";
    LocalDate parsedDate = LocalDate.parse(dateString, dateFormatter);
    System.out.println("구문 분석된 날짜: " + parsedDate);
   }
}
```

이 예제에서는 java.time 패키지에서 클래스를 가져와서 날짜, 시간 및 날짜-시간을 다루는 방법을 보여줍니다. LocalDate, LocalTime 및 LocalDateTime의 인스턴스를 생성한 후 DateTimeFormatter 클래스를 사용하여 날짜를 형식화하고 구문 분석하는 방법을 보여줍니다.

Java Date 및 Time API는 이전 대안들에 비해 더 직관적이고 변경 불가능하며 스레드 안전하게 설계되었습니다. 시간대, 시간 지속, 날짜 차이 처리를 위한 ZonedDateTime, Duration 및 Period와 같은 클래스를 제공합니다. 이 API는 이전의 날짜 및 시간 클래스에서 문제가되었던 시간 계산 및 일괄 시간 조정과 관련된 일반적인 문제를 피하는 데 도움을 줍니다.

<div class="content-ad"></div>

Baeldung 페이지를 찾아보면 구 차세대 API로 변환하는 자세한 예제를 확인할 수 있어요.

# 자바 9

## 자바 모듈 시스템

자바 9에서 소개된 자바 모듈 시스템은 자바 플랫폼에 중요한 변경 사항으로, 모듈식이면서 유지 보수성이 높은 애플리케이션을 만들 수 있게 해줘요. 이는 자바 생태계에서 문제가 되었던 강력한 캡슐화, 클래스 경로 오염 및 버전 충돌과 관련된 문제를 해결합니다.

<div class="content-ad"></div>

주요 개념:

모듈(Module): 모듈은 코드의 독립적인 단위로, 구현 세부 정보, 종속성을 캡슐화하고 명확한 API를 제공하는 것입니다. 모듈은 관련 있는 패키지와 자원들을 묶어서 높은 추상화 수준을 제공합니다.

모듈 디스크립터(module-info.java): 각 모듈은 module-info.java라는 파일에 지정된 모듈 디스크립터를 가지고 있습니다. 이 디스크립터는 모듈의 이름, 종속성, 내보낸 패키지 및 다른 모듈 관련 설정을 정의합니다.

모듈 경로(Module Path): 모듈 경로는 Java 어플리케이션의 종속성을 지정하는 새로운 방식입니다. 전통적인 클래스 경로(classpath)를 대체합니다. 모듈은 종속성과 명시적 요구 사항에 따라 해결되며, 이는 클래스 경로 충돌을 방지하는 데 도움이 됩니다.

<div class="content-ad"></div>

자바 모듈 시스템은 모듈 시스템 기능을 지원하기 위해 여러 새로운 키워드와 개념을 소개했습니다. 이러한 키워드는 모듈 선언을 정의하고 모듈 간의 관계, 서비스, 및 패키지를 지정하는 데 필수적입니다. 자바 모듈 시스템 내에서 모듈 간의 적절한 캡슐화, 의존성 관리, 그리고 격리를 보장하는 데 중요한 역할을 합니다. 모듈 시스템과 관련된 주요 키워드는 다음과 같습니다:


- module: 모듈을 선언하고 이름, 의존성, 그리고 기타 속성을 지정하는 데 사용됩니다. 모듈 선언은 module-info.java 파일에서 이루어집니다.

- requires: 모듈 의존성을 지정합니다. 다른 모듈을 필요로 하는 모듈은 해당 모듈의 노출된 패키지와 공개 유형에 엑세스할 수 있습니다.

- exports: 다른 모듈에 접근 가능한 패키지를 지정합니다. 노출된 패키지는 모듈의 공개 API를 정의합니다.

- opens: 패키지에 대한 반영적 액세스를 허용합니다. 특정 모듈에게 패키지의 비공개 멤버에 대한 런타임 액세스를 허용할 때 사용됩니다.

- uses: 모듈이 서비스를 사용한다는 것을 선언합니다. 이 키워드는 provides 키워드와 함께 사용하여 서비스 관계를 설정하는 데 사용됩니다.

- provides: 모듈이 Java 서비스 제공자 인터페이스(SPI)의 일부로 제공하는 서비스 구현을 지정합니다.

- with: provides 지시문에서 서비스 제공자 클래스를 선언할 때 사용할 서비스 로더 구현을 지정합니다.

- to: provides 지시문에서 여러 제공자가 정의된 경우에 서비스 제공자 클래스를 지정합니다.


자바 모듈 시스템에 대한 이해를 높이기 위한 예제를 만들어봅시다. 예제에서 고려할 사항은 다음과 같습니다:

<div class="content-ad"></div>


com.socialnetwork.core은 소셜 네트워킹 애플리케이션의 핵심 모듈을 나타냅니다.



com.socialnetwork.external은 외부 종속성을 포함하는 모듈을 나타냅니다.



com.socialnetwork.api은 귀하의 애플리케이션의 공개 API를 포함하는 패키지를 나타냅니다.



com.socialnetwork.notifications.reflective은 알림과 관련된 반사적 요소를 포함하는 패키지를 나타냅니다.


<div class="content-ad"></div>

```js
com.socialnetwork.notificationhandler은 알림을 반사적으로 처리하는 모듈을 나타냅니다.
```

```js
com.socialnetwork.UserNotificationService는 사용자 알림을 처리하기 위한 인터페이스입니다.
```

```js
com.socialnetwork.providers.DefaultNotificationServiceProvider 및 com.socialnetwork.providers.SecondaryNotificationServiceProvider는 UserNotificationService를 위한 서비스 제공자 클래스입니다.
```

```js
com.socialnetwork.providers.NotificationServiceLoader은 UserNotificationService 서비스를 위한 로더를 나타냅니다.
```

<div class="content-ad"></div>

```java
// 이것은 com.socialnetwork.core라는 새로운 모듈을 선언합니다.
module com.socialnetwork.core {
    // 이것은 코어 모듈이 외부 모듈을 종속성으로 필요로 한다는 것을 명시합니다.
    requires com.socialnetwork.external;
}
```

```java
// 이것은 코어 모듈에서 api 패키지를 내보내어 해당 공개 API를 다른 모듈에서 접근할 수 있도록 합니다.
exports com.socialnetwork.api;
// 이것은 reflective 패키지를 reflective 액세스를 위해 notificationhandler 모듈에 개방합니다.
// 이를 통해 후자 모듈이 reflection을 사용하여 비공개 멤버에 액세스할 수 있습니다.
opens com.socialnetwork.notifications.reflective to com.socialnetwork.notificationhandler;

// 이것은 코어 모듈이 UserNotificationService 서비스를 사용함을 나타냅니다.
uses com.socialnetwork.UserNotificationService;
// 이것은 UserNotificationService 인터페이스의 구현을 DefaultNotificationServiceProvider 및 SecondaryNotificationServiceProvider 클래스를 사용하여 제공합니다.
// 이는 여러 제공 업체가 UserNotificationService 서비스의 구현을 제공하는 관계를 설정합니다.
provides com.socialnetwork.UserNotificationService
    with com.socialnetwork.providers.DefaultNotificationServiceProvider
    to com.socialnetwork.providers.SecondaryNotificationServiceProvider;
}
```

이 module-info.java 스니펫은 com.socialnetwork.core 모듈을 위한 구조, 종속성, 내보내기, reflective 액세스, 서비스 제공자 및 서비스 소비자를 정의합니다. 이는 코어 모듈이 다른 모듈과 상호 작용하고 모듈화된 Java 애플리케이션에서 서비스를 제공하는 방법을 보여줍니다.

Java 9 모듈 시스템을 사용할 때 개발자는 암시적인 클래스패스 종속성이 아닌 모듈 간의 종속성과 상호 작용을 명시적으로 정의합니다. 모듈이 올바르게 캡슐화되고 종속성이 효과적으로 관리되는지 확인하기 위해 아키텍처를 테스트하는 것이 중요합니다. 이 구조를 테스트하고 모듈을 올바르게 처리하는지 확인하기 위해 ArchUnit을 아키텍처 테스트 프레임워크로 활용할 수 있습니다.


<div class="content-ad"></div>

## Try-with-resources

try-with-resources 문은 Java에서 사용되며 try 블록 내에서 사용되는 리소스를 자동으로 닫아줍니다. 이를 통해 예외가 발생해도 리소스가 올바르게 닫히도록 하여 리소스 관리를 간편화합니다. try-with-resources로 사용할 수 있는 리소스는 AutoCloseable 또는 java.io.Closeable 인터페이스를 구현해야 합니다.

Java 9에서는 try-with-resources 문이 개선되어 try 블록에서 선언된 최종 변수를 처리할 수 있게 되었습니다.

Java 9 이전:

<div class="content-ad"></div>

```js
새로운 Java 9 기능:
   
final Scanner scanner = new Scanner(new File("testRead.txt"));
final PrintWriter writer = new PrintWriter(new File("testWrite.txt"));
try (scanner; writer) { 
   // ...
  }

이 향상된 버전은 try 블록 내에서 리소스를 직접 선언할 수 있도록하여 코드를 간소화하고 가독성을 향상시키며 변수의 범위를 실제로 필요한 곳으로 줄입니다.

<div class="content-ad"></div>

## 비공개 인터페이스 메서드

비공개 인터페이스 메서드는 Java 9에서 도입된 기능으로, 인터페이스가 비공개 메서드를 가질 수 있게 합니다. 이러한 메서드는 주로 인터페이스 내부에서 사용되며, 코드 재사용과 가독성 향상을 위해 고안되었습니다. 이러한 메서드는 인터페이스를 구현하는 클래스나 인터페이스 외부의 다른 클래스에서 접근하거나 재정의할 수 없습니다.

public interface MyInterface {
  default void publicMethod() {
    privateMethod();
  }

  private void privateMethod() {
    System.out.println("인터페이스 내의 비공개 메서드입니다.");
  }
}

<div class="content-ad"></div>

# 자바 10

## 지역 변수 타입 추론

지역 변수 타입 추론, 보통 "var" 타입이라고 불리는 기능은 Java 10에서 소개된 기능으로, 명시적으로 타입을 지정하지 않고 지역 변수를 선언할 수 있게 해줍니다. 컴파일러는 할당된 값에 기반하여 변수의 타입을 추론합니다. 이 기능은 코드 가독성을 향상시키면서 강한 타이핑을 유지하도록 설계되었습니다.

public static void main(String[] args) {
   var name = "John Doe"; // String 타입 추론
   var age = 30; // int 타입 추론
   var salary = 50000.0; // double 타입 추론
   
   System.out.println("Name: " + name);
   System.out.println("Age: " + age);
   System.out.println("Salary: " + salary);
   
   // 컴파일 오류: var를 초기화해야 함
   // var uninitializedVar;
}

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해 주세요.

<div class="content-ad"></div>

로컬 변수 타입 추론(“var” 타입)은 Java의 람다 표현식에서도 사용할 수 있습니다. 이 기능은 Java 11에서 소개되었으며, var를 사용하여 람다 표현식 내에서 변수의 타입을 선언할 수 있습니다. 이를 통해 정적 타입의 장점을 유지하면서도 코드 가독성을 향상할 수 있습니다.

다음은 람다 표현식에서 로컬 변수 타입 추론을 사용하는 예제입니다.

List<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");
names.add("Charlie");

names.forEach((var name) -> {
  System.out.println("Hello, " + name);
});

<div class="content-ad"></div>

# 자바 12

## 문자열 들여쓰기와 변환

들여쓰기 메서드는 문자열의 각 줄에 들여쓰기를 추가하는 데 사용됩니다. 각 줄의 시작에 지정된 개수의 공백이 추가된 새로운 문자열 인스턴스를 반환합니다.

String original = "Hello\nWorld";
String indented = original.indent(4);
System.out.println(indented);
// 출력:
//     Hello
//     World

<div class="content-ad"></div>

`transform` 메서드는 문자열에 변환 함수를 적용하여 결과를 반환합니다. 이는 더 함수형 프로그래밍 스타일로 문자열에 연산을 수행하는 방법입니다.

String original = "Hello";
String transformed = original.transform(s -> s.toUpperCase());
System.out.println(transformed); // 출력: HELLO

## 파일 불일치

`Files.mismatch` 메서드는 Java의 `java.nio.file` 패키지의 일부이며, 두 파일의 내용을 비교하여 차이점을 확인하는 데 사용됩니다. 이는 두 파일의 내용을 전체적으로 읽지 않고도 두 파일이 다른 내용을 가지고 있는지를 효율적으로 결정하는 데 사용됩니다.

<div class="content-ad"></div>

Path filePath1 = Files.createTempFile("file1", ".txt");
Path filePath2 = Files.createTempFile("file2", ".txt");
Files.writeString(filePath1, "I love Java");
Files.writeString(filePath2, "I love Technology");

long mismatch = Files.mismatch(filePath1, filePath2); // It returns 7

이 코드에서 Files.mismatch 메서드는 filePath1과 filePath2로 참조된 파일의 내용을 비교하고 첫 번째로 다른 바이트의 위치를 반환합니다. 이 위치는 7번째 위치인 공백 문자일 것입니다. 이는 "I love Java"와 "I love Technology"의 내용이 일곱 번째 위치부터 다르기 때문입니다.

# Java 13

<div class="content-ad"></div>

## 텍스트 블록

텍스트 블록은 Java 13에서 미리보기 기능으로 소개되었으며 Java 15에서 Java 언어의 영구적인 일부로 만들어졌습니다. 연결 또는 이스케이프 문자가 필요하지 않는 다중 줄을 포함하는 문자열을 정의하는 보다 가독성 높고 자연스러운 방법을 제공합니다.

String name = """
                  ___        __  ___   __   __    __   _______  ______      
                 /   \\     |  |/  /  |  | |  \\ |  | |   ____||   _   \\     
                /  ^  \\    |  '  /   |  | |   \\|  | |  |__   |  |_)   |    
               /  /_\  \\   |    <    |  | |  . `  | |   __|  |        //     
              /  _____  \\  |  .  \   |  | |  |\\   | |  |____ |  |\\  \\__
             /__/     \__\\ |__|\\__\ |__| |__| \\__| |_______|| _| `._____|
                  """;

텍스트 블록은 SQL 쿼리, JSON, XML 및 가독성이 중요한 다른 종류의 서식이 지정된 텍스트를 작성하는 데 특히 유용합니다.

<div class="content-ad"></div>

# 자바 14

## Yield 키워드

자바 12에서는 스위치 문에 표현식을 도입하고 미리보기 기능으로 출시했습니다. 자바 13에서는 스위치 문에서 값을 반환하는 새로운 yield 구조를 추가했습니다. 자바 14에서는 스위치 표현식이 이제 표준 기능으로 지원됩니다.

public static String getDayType(String day) {
  var result = switch (day) {
    case "Monday", "Tuesday", "Wednesday", "Thursday", "Friday": yield "주중";
    case "Saturday", "Sunday": yield "주말";
    default: yield "잘못된 요일입니다.";
  };
  
  return result;
}

<div class="content-ad"></div>

# 자바 15

## 가비지 콜렉터 업데이트

Z 가비지 콜렉터는 자바 15까지 실험적인 기능이었습니다. 이는 저지연 및 높은 확장성을 갖춘 가비지 콜렉터입니다.

ZGC는 자바 11에서 실험적인 기능으로 소개되었는데, 개발자 커뮤니티에서 이를 너무 빨리 출시하기에는 너무 크다고 느꼈습니다. 그때부터 이 가비지 콜렉션에 많은 개선이 이루어졌습니다.

<div class="content-ad"></div>

ZGC는 매우 효율적이며 대용량 데이터 응용 프로그램(예: 머신 러닝 응용 프로그램)에서도 효율적으로 작동합니다. 가비지 컬렉션으로 인한 데이터 처리 중 장시간 일시 정지가 발생하지 않도록 보장합니다. Linux, Windows 및 MacOS를 지원합니다.

Shenandoah 저일시 정지 시간 가비지 수집기는 이제 실험 단계에서 벗어났습니다. JDK 12에 도입되었으며 Java 15부터는 표준 JDK의 일부입니다.

# Java 16

## instanceof에 대한 패턴 매칭

<div class="content-ad"></div>

자바 14에서 instanceof 연산자가 도입되어 타입 테스트 패턴을 미리보기 기능으로 제공하게 되었습니다. 타입 테스트 패턴은 단일 바인딩 변수로 타입을 지정하는 프레디케이트를 가지고 있습니다. 이 기능은 자바 15에서도 여전히 미리보기 기능으로 제공되고 있습니다. 자바 16에서는 이 기능이 이제 표준 배포의 일부로 포함되었습니다.

// 기존 instanceof
if (animal instanceof Cat) {
    Cat cat = (Cat) animal;
    cat.meow();
   // 다른 고양이 동작
}

// 현대적인 instanceof
if (animal instanceof Cat cat) {
    cat.meow();
}

## 레코드

<div class="content-ad"></div>

자바 14에서는 불변 데이터 객체를 쉽게 생성하기 위해 미리보기 기능으로 record 클래스 유형을 소개했습니다. 자바 15에서는 record 유형을 더 발전시켰습니다. 자바 16에서 record는 이제 JDK의 표준 기능입니다. Record를 정의하는 것은 불변 데이터 보유 객체를 간결하게 정의하는 방법입니다.

Record를 사용하지 않고

public final class Book {
   private final String title;
   private final String author;
   private final String isbn;

   public Book(String title, String author, String isbn) {
     this.title = title;
     this.author = author;
     this.isbn = isbn;
   }

   public String getTitle() {
     return title;
   }

   public String getAuthor() {
     return author;
   }

   public String getIsbn() {
     return isbn;
   }

   @Override
   public boolean equals(Object o) {
     // ...
   }

   @Override
   public int hashCode() {
     return Objects.hash(title, author, isbn);
   }
}

Record를 사용하여

<div class="content-ad"></div>

public record Book(String title, String author, String isbn) {}

# Java 17

## Sealed Class

시그네처 클래스는 자바 15에서 도입된 기능으로, 클래스 상속을 제어하고 특정 하위 클래스만 확장할 수 있도록 보장하는 데 사용됩니다. 시그네처 클래스에서 상속할 수 있는 클래스의 제한된 집합을 선언하는 방법을 제공하며, 다른 클래스에서의 확장을 방지합니다. 시그네처 클래스가 작동하는 기본적인 설명은 다음과 같습니다.

<div class="content-ad"></div>

public sealed class Shape 허가 Circle, Rectangle, Triangle, Square {...}

public sealed class Rectangle extends Shape 허가 TransparentRectangle, FilledRectangle {...}

이 예제에서 Shape 클래스는 sealed 키워드를 사용하여 sealed로 선언됩니다. permits 키워드를 사용하여 Shape에서 상속할 수 있는 클래스를 지정합니다.

//하위 클래스 정의:
public final class Circle extends Shape {...}

<div class="content-ad"></div>

public final class TransparentRectangle extends Rectangle {...}
public final class FilledRectangle extends Rectangle {...}
public non-sealed class Square extends Shape {...}

이 예시에서 Circle, Rectangle, Triangle 및 Square는 Shape의 허용된 하위 클래스로 명시적으로 표시됩니다. Square 클래스는 non-sealed로 선언되어 있어서 final이 아니며 현재 패키지 외부의 클래스에 의해 추가로 확장될 수 있습니다.

Sealed 클래스의 주요 기능

제어된 상속: Sealed 클래스를 사용하면 어떤 클래스가 해당 클래스를 확장할 수 있는지를 제어하여 원치 않는 하위 클래스를 방지할 수 있습니다.

<div class="content-ad"></div>

제한된 하위 클래스: permits 키워드는 sealed 클래스의 직접적인 하위 클래스로 허용되는 클래스 목록을 지정합니다.
열린 및 비-슬기로운 클래스: sealed 클래스는 열린 클래스 또는 비-슬기로운 클래스가 확장할 수 있도록 하여 클래스 계층 구조 설계에 대한 더 많은 유연성을 제공할 수 있습니다.
기본적으로 Final: sealed 클래스는 암시적으로 final이며, 명시적으로 허용되지 않는 한 직접적으로 인스턴스화하거나 하위 클래스화할 수 없습니다.
사용 사례: sealed 클래스는 하위 클래스의 수를 제한하고 클래스 계층 구조의 설계와 동작을 더 잘 제어하려는 경우에 유용합니다.

# 자바 21

## 가상 스레드

이전 스레딩 모델에서 Java의 스레드는 직접적으로 운영 체제(Operating System, OS) 스레드와 대응되어 OS 제약으로 인해 생성할 수 있는 스레드의 수에 제한이 있습니다. 전통적인 스레딩 모델에서 과도한 스레드 생성은 OS를 과도하게 부담시킬 수 있고 특히 수명이 짧은 스레드의 경우 높은 비용이 발생할 수 있습니다.
```

<div class="content-ad"></div>

자바 21은 가상 스레드를 도입하여 가상 스레드와 OS 스레드 간의 매핑을 제공하며, 이론적으로 제한 없는 가상 스레드 생성을 가능하게 합니다. 이 혁신은 기존 스레드 모델의 한계를 극복함으로써 고처리량 서버 응용 프로그램의 요구를 충족시키기 위해 많은 스레드를 생성할 수 있습니다. 가상 스레드를 사용하면 이전에 있던 스레드 생성에 대한 제한이 없어져, 서버 응용 프로그램에서 일반적으로 사용되는 요청 당 스레드 방식을 계속할 수 있게 됩니다.

가상 스레드를 사용하는 예제는 OS/플랫폼 스레드와 비교하여 가상 스레드의 사용법을 보여줍니다. ExecutorService를 사용하여 500,000개의 스레드를 각각 실행함으로써, JDK는 제한된 수의 캐리어 및 OS 스레드에서 효율적으로 실행을 관리하여 개발자가 동시성 코드를 간편하게 작성할 수 있습니다.

```js
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    IntStream.range(0, 500_000).forEach(i -> {
        executor.submit(() -> {
            Thread.sleep(Duration.ofSeconds(1));
            return i;
        });
    });
}  // executor.close() is called implicitly, and waits
```

## 순차 컬렉션

<div class="content-ad"></div>

명시된 만남 순서를 갖는 컬렉션에 대한 범용 상위 유형의 부재로 인해 Java의 컬렉션 프레임워크 내에서 반복되는 문제와 불만이 발생했습니다. 또한 첫 번째와 마지막 요소에 접근하는 일관된 메소드 부재와 역순으로 반복하는 방법이 불명확한 점이 지속적인 단점으로 작용했습니다.

만남 순서를 유지하는 List 인터페이스를 고려해 봅시다. 리스트에서 첫 번째 요소에 접근하는 것은 list.get(0)로 간단하지만, 마지막 요소에 접근하려면 list.get(list.size() - 1)이 필요합니다. 이 불일치는 개발자에게 어려움을 줄 뿐만 아니라 코드 유지 관리를 복잡하게 만듭니다.

Collection 인터페이스에서 예시로 언급된 이 불편함은 Map 인터페이스에서도 발생할 수 있습니다.

이 새로운 기능은 시퀀스 컬렉션, 시퀀스 세트 및 시퀀스 맵을 위한 세 가지 새로운 인터페이스를 도입합니다. 이들은 기존의 컬렉션 계층 구조에 추가됩니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-26-ModernJavaAnIn-DepthGuidefromJava8toJava21_1.png)

SequencedCollection 인터페이스는 reverse 메서드를 추가하여 등장 순서를 지원하며 첫 번째 및 마지막 요소를 가져올 수 있는 능력을 제공합니다. 더불어 SequencedMap 및 SequencedSet 인터페이스도 있습니다.

```js
interface SequencedCollection<E> extends Collection<E> {
    // 새로운 메서드
    SequencedCollection<E> reversed();
    // Deque로부터 상속된 메서드
    void addFirst(E);
    void addLast(E);
    E getFirst();
    E getLast();
    E removeFirst();
    E removeLast();
}
```

Sequenced Collections의 도입은 Java Collections에 대한 주목할만한 진보를 나타냅니다. 지정된 encounter order를 가진 컬렉션을 관리하는 표준화된 접근 방식이 간절히 필요했던 요구사항을 충족함으로써 Java는 개발자가 더 효율적이고 명확하게 작업할 수 있게 합니다. 이러한 새로운 인터페이스들은 보다 투명한 프레임워크와 예측 가능한 동작을 수립하여 코드를 보다 견고하고 이해하기 쉽게 만듭니다.


<div class="content-ad"></div>

## 문자열 템플릿

문자열 템플릿은 JDK 21에서 미리보기 기능으로 소개되었으며 Java에서 문자열 조작의 신뢰도와 사용성을 향상시키기 위해 고안되었습니다. 이는 템플릿 표현식을 만들어 문자열로 렌더링할 수 있는 메커니즘을 제공합니다. 이 기능은 종종 의도하지 않은 결과로 이어지는 주입과 같은 일반적인 문제를 방지하는 데 도움이 됩니다. 문자열 템플릿을 사용하면 개발자들은 문자열 템플릿 내에서 표현식을 작성하고 쉽게 원하는 출력을 생성할 수 있습니다. 예를 들어:

```js
String codingLanguage = "Java";
String sentence = "${codingLanguage} is awesome";
System.out.println(sentence);
```

```js
// 출력:
//   Java is awesome!
```

<div class="content-ad"></div>

## 레코드 패턴

자바 14에서 레코드의 도입으로 데이터 중심 클래스를 만드는 간소화된 방법이 제공되었습니다. 이를 통해 부가 코드 없이 데이터를 전달하는 데 집중할 수 있게 되었죠. 이는 코드를 단순화하고 가독성을 향상시키는 중요한 발전이었습니다.

이제 자바 21의 발전으로 레코드 패턴과 타입 패턴을 중첩시킬 수 있어 데이터 조작에 더 선언적이고 조립 가능한 접근 방식을 제공합니다. 이는 개발자가 데이터를 직관적이고 간결한 방식으로 탐색하고 처리할 수 있다는 의미입니다.

자바 21 이전에는 레코드에서 개별 값을 액세스하려면 전체 레코드를 분해해야 했기 때문에 장황한 코드가 될 수 있었습니다. 그러나 레코드 패턴과 타입 패턴의 도입으로 이 프로세스가 훨씬 간단해지고 효율적으로 이루어지게 되었습니다.

<div class="content-ad"></div>

여기 제공된 예제를 좀 더 자세히 살펴봅시다:

```js
record Address(String city, String apartment) {}
```

```js
static void printCityWithoutPatternMatching(Object obj) {
    if (obj instanceof Address a) {
        String city = a.city();
        System.out.println(city);
    }
}
static void printCityWithPatternMatching(Object obj) {
    if (obj instanceof Address(String city, String apartment)) {
        System.out.println(city);
    }
}
```

printCityWithoutPatternMatching 메서드에서는, Java 21 이전에는 도시에 액세스하기 위해 객체가 Address의 인스턴스인지 먼저 확인한 후, 액세서 메서드를 사용하여 도시를 검색해야 했습니다. 이 방법은 작동하지만 번잡하고 직관적이지 않을 수 있습니다.

<div class="content-ad"></div>

반면에, printCityWithPatternMatching 메서드는 Java 21에서 소개된 패턴 매칭의 강력함을 보여줍니다. 여기서 instanceof 확인은 조건문 안에서 직접적으로 패턴 매칭과 결합됩니다. 이로 인해 더 깔끔하고 간결한 코드가 생성되며, 도시가 명시적인 접근자 메서드 없이도 객체에서 직접 추출됩니다.

전반적으로, Java 21에서의 발전은 레코드 패턴(record patterns)과 타입 패턴(type patterns)의 도입을 통해 데이터 조작을 더욱 간단하게 만들어주며 우아하고 가독성이 좋은 코드를 촉진합니다. 이는 개발자들이 프로그램의 논리에 집중할 수 있도록 돕습니다. 보일러플레이트 코드에 갇힐 필요가 없어지죠.

# 결론

요약하자면, 우리는 Java 8부터 21까지의 변화를 통해 이 언어의 발전을 확인했습니다. 람다 표현식, 새로운 Time API, Stream API, sealed 클래스, 레코드 등과 같은 멋진 기능들을 보았습니다. 이러한 개선들 각각이 현대적인 코딩 문제를 해결하는 데 Java를 더 좋게 만들어주었습니다. 이 새로운 지식을 얻었으니, 이제 자신 있게 코딩 과제에 도전할 수 있을 것입니다. 기억하세요, Java는 항상 변화하고 성장하고 있으므로 최신 정보를 따라가면 더 많은 일을 할 수 있습니다. 코딩에 익숙한 사람이든 초보자든, Java는 멋진 것을 만드는 데 많은 기회를 제공합니다. 이 아이디어들이 여러분을 이끄는 길이 되어 Java 프로그래밍의 세계로 더 즐겁게 뛰어들어 보세요. 즐거운 코딩되세요!

<div class="content-ad"></div>

만약 이 기사가 흥미롭다고 느꼈다면, 다른 사람들과 함께 좋아요 표시하고 공유하는 것을 고려해주세요. 이를 통해 더 많은 사람들이 이 기사를 발견할 수 있게 도와주세요.

기술과 소프트웨어 개발에 대해 궁금하다면, 다른 기사들을 살펴보시기를 권장합니다. 코딩, 앱 생성, 최신 기술 트렌드에 대해 다루는 다양한 주제를 찾을 수 있을 것입니다. 경험 있는 개발자이든 막 시작한 개발자이든, 여기에는 모두를 위한 내용이 있습니다.

## 참고 자료

https://www.archunit.org/motivation

<div class="content-ad"></div>

[Java 21: Sequenced Collections](https://www.baeldung.com/java-21-sequenced-collections)

[Migrating to Java 8 Date Time API - Examples](https://www.baeldung.com/migrating-to-java-8-date-time-api#examples)