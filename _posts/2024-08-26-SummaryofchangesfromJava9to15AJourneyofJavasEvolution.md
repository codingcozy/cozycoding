---
title: "자바 9부터 15까지의 변화 요약 자바 발전 과정 정리"
description: ""
coverImage: "/assets/img/2024-08-26-SummaryofchangesfromJava9to15AJourneyofJavasEvolution_0.png"
date: 2024-08-26 18:49
ogImage: 
  url: /assets/img/2024-08-26-SummaryofchangesfromJava9to15AJourneyofJavasEvolution_0.png
tag: Tech
originalTitle: "Summary of changes from Java 9 to 15 A Journey of Javas Evolution"
link: "https://medium.com/@alankarrocks/summary-of-changes-from-java-8-to-15-a-journey-of-javas-evolution-de1e03ba2ace"
isUpdated: true
updatedAt: 1724742621815
---


<img src="/assets/img/2024-08-26-SummaryofchangesfromJava9to15AJourneyofJavasEvolution_0.png" />

자바는 현대 소프트웨어 개발의 중요한 요충지로서 지속적으로 진화해 왔습니다. 동적 기술 환경의 요구를 충족하기 위해 계속 발전해 왔습니다. Java 9부터 Java 11로의 여정은 특히 중요합니다. 이 기간에 발전된 기능들은 언어의 능력을 개선하고 성능을 향상시키며 개발 워크플로우를 간소화했습니다.

이 블로그에서는 Java 9부터 11까지의 주요 차이점을 탐구할 것입니다. 각 섹션에서는 특정 Java 버전에서 도입된 새로운 기능을 탐구합니다. 간결한 코드 예제로 이러한 기능들을 설명하고 이해를 높이기 위해 요약을 제공할 것입니다.

# Java 9: 모듈화와 그 이상

<div class="content-ad"></div>

주요 기능: Java 플랫폼 모듈 시스템 (JPMS)

```java
module com.mymodule {
    requires java.base;
    exports com.mymodule.api;
}
```

요약:

JPMS는 Java에 모듈성을 도입하여 개발자가 명시적 종속성을 갖는 자체 포함 단위로 코드를 캡슐화할 수 있게 했습니다. 이는 큰 프로젝트에 특히 유용한 더 나은 코드 구조화, 유지 관리 및 확장성을 촉진합니다.

<div class="content-ad"></div>

추가 Java 9 기능:

- JShell (Read-Eval-Print Loop): Java 코드 스니펫을 실시간으로 평가하는 대화식 도구로, 실험 및 학습을 돕습니다.
- 컬렉션 공장 메소드: List.of() 및 Set.of()와 같은 편리한 메소드를 사용하여 쉽게 불변 컬렉션을 생성할 수 있습니다.
- 개인 인터페이스 메소드: 인터페이스가 내부 구현 세부사항을 위한 개인 메소드를 정의할 수 있어 코드 구성을 개선합니다.
- HTTP/2 클라이언트: HTTP/2를 지원하는 새로운 HTTP 클라이언트로, 네트워크 통신의 성능과 효율성을 향상시켜줍니다.
- 프로세스 API 개선: 작업 체계 프로세스의 제어와 관리를 더 잘하는 프로세스 API 개선사항.

# Java 10: 지역 변수 유형 추론

주요 기능: var 키워드

<div class="content-ad"></div>

```javascript
var list = new ArrayList<String>();
list.add("Hello");
```

요약:

var는 로컬 변수의 타입 추론을 가능하게 하여 보일러플레이트 코드를 줄이고 가독성을 향상시킵니다. 컴파일러는 이니셜라이저 표현식으로부터 변수의 타입을 추론합니다.

기타 Java 10 기능:

<div class="content-ad"></div>

- 쓰레기 수집기 인터페이스: 새로운 GC 구현을 더 쉽게 통합할 수 있도록하는 표준화된 인터페이스.
- G1을 위한 병렬 Full GC: 전체 가비지 수집 작업을 병렬화하여 G1 가비지 수집기의 성능을 향상시킵니다.
- 애플리케이션 Class-Data Sharing: 여러 JVM 인스턴스 간에 일반적인 클래스 메타데이터를 공유함으로써 시작 시간과 메모리 풋프린트를 줄입니다.
- 쓰레드-로컬 핸드셰이크: 글로벌 VM 세이프포인트 없이 쓰레드에 콜백을 수행하는 메커니즘.

# 자바 11: 장기 지원 및 주요 향상

주요 기능:

- HTTP 클라이언트 (표준)
- 단일 파일 소스코드 프로그램
- 문자열 메소드 (isBlank, lines, repeat, strip)
- 엡실론 가비지 수집기
- 동적 클래스 파일 상수
- 람다 파라미터를 위한 지역 변수 구문

<div class="content-ad"></div>

```java
var client = HttpClient.newHttpClient();
var request = HttpRequest.newBuilder()
        .uri(URI.create("https://www.example.com"))
        .build();
var response = client.send(request, HttpResponse.BodyHandlers.ofString());
System.out.println(response.body());
```

요약:

Java 11은 장기 지원(LTS) 릴리스이며 지원 및 안정성이 확장됨을 나타냅니다. 표준화된 HTTP 클라이언트를 도입하며, 단일 파일 프로그램 실행을 최적화하고, 문자열 조작을 강화하며, Epsilon Garbage Collector와 성능 최적화를 추가했습니다.

추가 Java 11 기능:

<div class="content-ad"></div>

- 비행 레코더: JVM 및 애플리케이션 동작을 모니터링하고 프로파일링하기 위한 저부하 데이터 수집 프레임워크입니다.
- Nest-Based Access Control: 네스트 내의 클래스가 서로의 비공개 멤버에 액세스할 수 있도록 함으로써 캡슐화와 보안을 개선합니다.
- Low-Overhead Heap Profiling: 성능 저하를 최소화하면서 객체 할당 및 메모리 사용에 대한 상세한 정보를 제공합니다.

# Java 12: 스위치 표현식(미리 보기) 및 Shenandoah GC

주요 기능: 스위치 표현식(미리 보기)

```js
int dayOfWeek = 3;
String dayType = switch (dayOfWeek) {
    case 1, 7 -> "주말";
    case 2, 3, 4, 5, 6 -> "평일";
    default -> throw new IllegalArgumentException("유효하지 않은 요일: " + dayOfWeek);
};
System.out.println(dayType); // 출력: 평일
```

<div class="content-ad"></div>

요약:

스위치 표현식은 스위치 문을 더 간결하고 표현력있게 작성하는 방법을 제공하여 표현식으로 사용하고 값을 직접 반환할 수 있게 합니다.

기타 Java 12 기능:

- Shenandoah GC (실험적): 대규모 힙을 위해 설계된 저 지연 시간 가비지 컬렉터.
- Microbenchmark Suite: JVM 성능 측정용 마이크로벤치마크 모음.
- JVM Constants API: 주요 클래스 파일 및 런타임 아티팩트에 대한 명칭적 설명을 모델링하는 API로, 특히 상수 풀에서 로드할 수 있는 상수입니다.

<div class="content-ad"></div>

# Java 13: Text Blocks (미리보기) 및 ZGC 업그레이드

주요 기능: 텍스트 블록 (미리보기)

```js
String html = """
    <html>
        <body>
            <p>Hello, world!</p>
        </body>
    </html>
    """;
```

개요:

<div class="content-ad"></div>

텍스트 블록은 여러 줄의 문자열을 쉽게 생성할 수 있게 해주며, 포맷을 유지하고 이스케이프 시퀀스를 필요로 하지 않도록 도와줍니다.

Java 13의 추가 기능:

- ZGC 개선: Z 가비지 컬렉터의 개선 사항으로, 사용되지 않는 메모리를 해제하고 운영 체제로 반환합니다.
- 소켓 API 재구현: java.net.Socket 및 java.net.ServerSocket API의 새로운 구현으로, 성능 및 유지보수성이 개선됩니다.
- 동적 CDS 아카이브: 실행 중에 클래스 데이터 공유(CDS) 아카이브를 생성할 수 있어 시작 시간을 향상시킵니다.

# Java 14: instanceof에 대한 패턴 매칭(미리보기) 및 유용한 NullPointerExceptions

<div class="content-ad"></div>

키 기능: instanceof 패턴 매칭 (미리보기)

```js
if (obj instanceof String s) {
    System.out.println(s.length());
}
```

개요:

패턴 매칭은 instanceof 연산자를 개선하여 패턴 내에서 변수를 선언하고 조건부 블록에서 직접 사용할 수 있도록 하여 보일러플레이트 코드를 줄이는 기능을 제공합니다.

<div class="content-ad"></div>

부가적인 자바 14 기능:

- NullPointerExceptions에 대한 도움이 되는 정보: 정확한 null 참조 위치를 가리키는 더 유익한 NullPointerException을 제공합니다.
- Records (미리보기): 간결한 구문으로 데이터 집합을 모델링하는 새로운 유형의 클래스.
- Foreign-Memory Access API (Incubator): 안전하고 효율적으로 off-heap 메모리에 접근하기 위한 API.

# Java 15: Sealed Classes (미리보기) 및 Text Blocks (표준)

주요 기능:

<div class="content-ad"></div>

- 실裥 and-even정상 개발 다가 stay에서는하시ше읕toBeDefined 서방 cri형 Tables.

<div class="content-ad"></div>

추가된 Java 15 기능들:

- Hidden Classes: 런타임에서 클래스를 생성하는 프레임워크의 효율성을 개선합니다.
- Edwards-Curve Digital Signature Algorithm (EdDSA): 디지턈 서명을 위한 새로운 암호 알고리즘입니다.
- Foreign-Memory Access API (Second Incubator): 오프-힙 메모리에 접근하기 위한 API의 지속적인 개발입니다.

![이미지](/assets/img/2024-08-26-SummaryofchangesfromJava9to15AJourneyofJavasEvolution_1.png)

# 결론

<div class="content-ad"></div>

자바 9에서 15로의 진화는 언어에 중요한 발전을 가져와 개발자들에게 향상된 도구, 성능 향상, 그리고 보다 간편화된 개발 경험을 제공합니다. 이러한 주요 차이를 이해함으로써 여러분은 프로젝트에서 자바의 모든 잠재력을 활용할 수 있어 코드를 현대적이고 효율적이며 유지보수 가능하게 유지할 수 있습니다.

기술의 끝없는 변화 속에서 계속해서 배우는 것이 중요하다는 것을 기억하세요. 새로운 기능들을 받아들이고 실험해보며 자바 마스터로의 여정을 계속하십시오.