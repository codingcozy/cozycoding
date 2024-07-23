---
title: "자바에서 입력값 읽는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-ReadingInputsinJava_0.png"
date: 2024-07-07 03:22
ogImage: 
  url: /assets/img/2024-07-07-ReadingInputsinJava_0.png
tag: Tech
originalTitle: "Reading Inputs in Java"
link: "https://medium.com/@nimakarimian/reading-inputs-in-java-3ea2168d9516"
---


자바에서 입력을 읽는 것은 파이썬보다 조금 더 복잡합니다. 파이썬에 익숙하다면 이렇게 입력을 읽을 수 있다는 것을 알고 있을 것입니다:

```js
user_name = input("여기에 사용자 이름을 입력하세요: ")
```

# InputStream에서 읽기

자바에서의 상황은 완전히 다릅니다. 자바의 표준 입력 스트림에서 입력을 읽으려면 먼저 Scanner를 생성해야 합니다. 자바에서의 Scanner는 입력을 읽는 과정을 간단화해주며 유용한 메서드들을 제공합니다. 이러한 메서드를 사용하면 사용자의 입력을 더 작은 데이터 조각으로 나누어주고 이 조각들을 유용한 형식으로 변환할 수 있습니다.

<div class="content-ad"></div>

- 숫자 (정수, 부동 소수점, 롱 등)
- 부울 (false, true)
- 문자열

```java
Scanner in = new Scanner(System.in);
```

위의 코드에서 볼 수 있듯이, 우리는 scanner 객체를 만들고 "System.in"과 바인딩했습니다. Scanner 클래스는 사용자가 입력한 다양한 유형의 데이터를 읽을 수 있는 여러 메서드를 제공합니다. 여기에 일반적인 메서드 몇 가지가 있습니다:

- nextLine(): 사용자가 입력한 전체 텍스트 줄을 읽습니다 (공백 포함).
- nextInt(): 사용자가 입력한 다음 정수 값을 읽습니다.
- nextDouble(): 다음 더블 정밀도 부동 소수점 값을 읽습니다.
- nextBoolean(): 사용자가 입력한 다음 불리언 값 (true 또는 false)을 읽습니다.

<div class="content-ad"></div>

# 자바 Scanner 자세히 알아보기

먼저 새 Scanner 객체 변수를 만들어야 합니다.

```java
Scanner input = new Scanner(System.in);
```

## 문자열 입력 읽기

<div class="content-ad"></div>

`Scanner.nextLine()` 메소드는 입력의 다음 줄을 읽습니다.

```java
String firstName = input.nextLine();
String lastName = input.nextLine();
```

주의:

이 메소드는 줄 구분자를 찾기 위해 입력을 계속 검색하기 때문에 줄 구분자가 없는 경우 건너뛴 줄을 찾기 위해 모든 입력을 버퍼링할 수 있습니다.

<div class="content-ad"></div>

예를 들어, nextLine() 메서드를 nextInt() 메서드 바로 뒤에 사용하는 경우, nextInt()가 정수 토큰을 읽기 때문에 그 줄의 정수 입력에 대한 마지막 개행 문자가 여전히 입력 버퍼에 대기 중이며, 다음 nextLine()은 정수 줄의 나머지를 읽게 됩니다 (이 경우에는 비어 있습니다).

## 숫자 입력 읽기

다행히도 Java에서 InputStream에서 직접 입력을 읽을 때는 TypeCastings와 변환의 귀찮음 없이 입력을 바로 원하는 형식으로 읽을 수 있습니다.

```java
int age = input.nextInt(); //int
float weight = input.nextFloat(); // float
long bankAccount = input.nextLong(); //Long Int 
double bankBalance = input.nextDouble(); //double floating precision 
```

<div class="content-ad"></div>

안내:

이전에 말한 대로, Java에서 숫자 입력을 읽은 후에 그 뒤에 줄을 읽고 싶다면 nextLine() 메서드를 사용해야 합니다.

```js
long bankAccount = input.nextLong(); //Long Int
input.nextLine();
String bankName = input.nextLine();
```

## 불리언 값 읽기

<div class="content-ad"></div>

이미지 태그를 Markdown 형식으로 변경해주세요.


boolean isActive = input.nextBoolean();


## Scanner 닫기


input.close();


<div class="content-ad"></div>

InputStream을 통해 입력을 읽은 후 Scanner를 닫는 것은 좋은 프로그래밍 관행입니다. Scanner는 시스템 리소스인 기본 입력 스트림(System.in)과 같은 것을 사용합니다. Scanner를 명시적으로 닫으면 이러한 리소스가 해제되어 프로그램의 다른 부분이나 시스템 자체가 필요한 경우 사용할 수 있습니다. Scanner를 닫지 않으면 리소스 누출로 이어질 수 있습니다. 이는 Scanner에 할당된 리소스가 더 이상 사용되지 않더라도 계속 점유되는 것을 의미합니다.

# 비밀번호 입력

클라이언트로부터 비밀번호를 읽어들이는 경우 Scanner를 사용해서는 안 됩니다. 왜냐하면 그렇게 입력이 누구에게나 보이기 때문입니다. 입력에서 비밀번호를 읽고자 한다면 Console 클래스를 사용하는 것을 고려해야 합니다.

## Console 클래스

<div class="content-ad"></div>

```java
Console console = System.console();
char[] pass = console.readPassword("비밀번호 입력");
System.out.println(pass);
```

보안을 위해 비밀번호는 배열로 반환됩니다. 방금 읽은 비밀번호를 처리한 후 배열을 비워야 하거나 더미 값으로 비밀번호 배열을 대체해야 합니다. 기억해야 할 것 중 하나는 `System.console()`이 항상 사용 가능한 것은 아니며, null을 반환하는 경우를 처리해야 합니다.

![ReadingInputsinJava](/assets/img/2024-07-07-ReadingInputsinJava_0.png)

Java - Spring에 관한 다양한 사실과 지식을 더 알고 싶다면 제 블로그를 확인해주세요.


<div class="content-ad"></div>

## 관련 기사:

- 자바에서 파일 읽기
- 자바에서 파일 쓰기
- String vs StringBuilder — 불변 vs 가변