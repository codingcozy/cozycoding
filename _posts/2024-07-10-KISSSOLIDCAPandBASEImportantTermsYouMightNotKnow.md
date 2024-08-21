---
title: "KISS, SOLID, CAP, 그리고 BASE 알아두면 좋은 중요한 용어들"
description: ""
coverImage: "/assets/img/2024-07-10-KISSSOLIDCAPandBASEImportantTermsYouMightNotKnow_0.png"
date: 2024-07-10 03:12
ogImage: 
  url: /assets/img/2024-07-10-KISSSOLIDCAPandBASEImportantTermsYouMightNotKnow_0.png
tag: Tech
originalTitle: "KISS, SOLID, CAP, and BASE: Important Terms You Might Not Know"
link: "https://medium.com/@techsuneel99/kiss-solid-cap-and-base-important-terms-you-might-not-know-dacccc07feb4"
isUpdated: true
---




소프트웨어 개발 및 시스템 디자인의 세계에서는 다양한 원칙과 개념이 개발자와 아키텍트들을 이끌어 견고하고 유지보수 가능하며 확장 가능한 시스템을 만들도록 안내합니다. 그 중에서도 KISS, SOLID, CAP, BASE가 기본 개념으로 두드러지며 모든 개발자가 이해해야 할 중요한 아이디어입니다. 본 문서에서는 각 개념을 깊이 있게 살펴보며 현실 시나리오에서의 중요성과 적용을 설명하겠습니다.

![KISS, SOLID, CAP, and BASE](/assets/img/2024-07-10-KISSSOLIDCAPandBASEImportantTermsYouMightNotKnow_0.png)

# KISS: Keep It Simple, Stupid

# KISS란 무엇인가요?

<div class="content-ad"></div>

KISS는 "Keep It Simple, Stupid" 또는 "Keep It Simple and Straightforward"의 약어입니다. 이 원칙은 설계와 실행에서의 간결함의 중요성을 강조합니다. 핵심 아이디어는 시스템이 복잡하게 만드는 대신 간단하게 유지할 때 가장 잘 작동한다는 것입니다. 간결함은 설계에서 주요 목표여야 하며 불필요한 복잡성은 피해야 합니다.

# KISS의 중요성

- 유지보수성: 간단한 시스템은 이해, 디버깅 및 유지가 쉽습니다.
- 신뢰성: 움직이는 부품이 적을수록 문제가 발생할 가능성이 적습니다.
- 효율성: 간단한 해결책은 종종 성능이 우수하고 리소스를 적게 사용합니다.
- 유연성: 간단한 설계는 변화하는 요구에 더 잘 적응할 수 있습니다.

# KISS가 실천된 예시

<div class="content-ad"></div>

한 가지 상황을 생각해 봅시다. 숫자 목록의 평균을 계산하는 함수를 구현해야 한다고 가정해 보겠습니다.

더 복잡한(Keep It Simple and Straightforward, KISS를 따르지 않는) 방식은 다음과 같을 수 있습니다:

```python
def calculate_average(numbers):
    total = 0
    count = 0
    for num in numbers:
        if isinstance(num, (int, float)):
            total += num
            count += 1
        else:
            try:
                num = float(num)
                total += num
                count += 1
            except ValueError:
                pass
    if count == 0:
        return None
    return total / count
```

이 함수는 다양한 예외 상황과 형 변환을 처리하려고 시도하여 필요 이상으로 복잡해 보입니다.

<div class="content-ad"></div>

A simple way to go about it would be:

```python
def calculate_average(numbers):
    if not numbers:
        return None
    return sum(numbers) / len(numbers)
```

This simplified version assumes that the input is a list of numbers, which is a reasonable assumption. It's concise, easy to grasp, and less likely to contain errors. If extra functionality is required, such as dealing with non-numeric inputs, it can be incorporated separately or managed when the function is invoked.

# Implementing KISS in Your Projects

<div class="content-ad"></div>

개발 작업에서 KISS를 적용하는 방법:

- 요구 사항을 충족하는 가장 간단한 해결책으로 시작하기.
- 일찍 최적화나 과도한 엔지니어링을 피하기.
- 복잡한 문제를 더 작고 간단한 부분으로 분해하기.
- 코드를 정기적으로 리팩터링하여 단순하고 깔끔하게 유지하기.
- 각 기능 또는 코드 라인의 필요성을 의심하기.

기억하세요, 목표는 가능한 한 단순하게 만드는 것이 아니라 효과적으로 목적을 달성하는 데 필요한 만큼 단순하게 만드는 것입니다.

# SOLID 원칙

<div class="content-ad"></div>

**SOLID**은 객체 지향 프로그래밍과 디자인의 다섯 가지 원칙을 나타내는 약어입니다. 이러한 원칙들이 함께 적용되면, 프로그래머가 시간이 지남에 따라 유지 보수 및 확장하기 쉬운 시스템을 만들 확률이 높아집니다.

**S — 단일 책임 원칙 (SRP)**

단일 책임 원칙은 클래스가 변경되어야 하는 이유가 하나여야 한다는 것을 말합니다. 다시 말해, 클래스는 하나의 작업 또는 책임만 가져야 합니다.

## SRP 예시

<div class="content-ad"></div>

사용자 데이터 및 인증을 처리하는 사용자 클래스에 대해 고려해보겠습니다:

```js
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
    
    def get_user_info(self):
        return f"{self.name} ({self.email})"
    
    def save_user_to_database(self):
        # 데이터베이스에 사용자 저장하는 코드
        pass
    
    def send_email(self, message):
        # 이메일 보내는 코드
        pass
```

이 클래스는 SRP를 위반하고 있습니다. 사용자 데이터, 데이터베이스 작업 및 이메일 기능에 책임을 지고 있습니다. SRP를 준수하기 위해 이를 각각의 별도 클래스로 분리할 수 있습니다:

```js
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
    
    def get_user_info(self):
        return f"{self.name} ({self.email})"

class UserDatabase:
    @staticmethod
    def save_user(user):
        # 데이터베이스에 사용자를 저장하는 코드
        pass

class EmailService:
    @staticmethod
    def send_email(user, message):
        # 이메일을 보내는 코드
        pass
```

<div class="content-ad"></div>

이제 각 클래스는 단일 책임을 갖고 있어 시스템이 더 모듈식이 되었고 유지보수가 더 쉬워졌어요.

# O — Open-Closed Principle (OCP)

개방-폐쇄 원칙(Open-Closed Principle)은 소프트웨어 개체(클래스, 모듈, 함수 등)는 확장에 대해 개방적이어야 하지만 수정에 대해서는 폐쇄적이어야 한다고 말해요. 이는 클래스의 동작을 수정하지 않고 확장할 수 있어야 한다는 의미예요.

## OCP 예시

<div class="content-ad"></div>

다양한 도형의 면적을 계산하는 시스템을 고려해보겠습니다:

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

class Circle:
    def __init__(self, radius):
        self.radius = radius

class AreaCalculator:
    def calculate_area(self, shape):
        if isinstance(shape, Rectangle):
            return shape.width * shape.height
        elif isinstance(shape, Circle):
            return 3.14 * shape.radius ** 2
        # 새로운 도형을 추가할 경우 이 클래스를 수정해야 합니다
```

새로운 도형을 추가할 때마다 AreaCalculator를 수정해야 하는 점은 OCP를 위반하고 있습니다. 여기 OCP를 준수하는 버전입니다:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def calculate_area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def calculate_area(self):
        return self.width * self.height

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def calculate_area(self):
        return 3.14 * self.radius ** 2

class AreaCalculator:
    def calculate_area(self, shape):
        return shape.calculate_area()
```

<div class="content-ad"></div>

이제 AreaCalculator 클래스의 수정 없이 새로운 모양을 추가할 수 있습니다.

## L — Liskov Substitution Principle (LSP)

리스코프 치환 원칙(Liskov Substitution Principle, LSP)은 슈퍼클래스의 객체를 서브클래스의 객체로 대체할 수 있어야 하며, 프로그램의 정확도에 영향을 주지 않아야 한다고 말합니다.

## LSP의 예시

<div class="content-ad"></div>

클래식한 사각형과 직사각형의 전형적인 예제를 살펴보겠습니다:

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def set_width(self, width):
        self.width = width
    
    def set_height(self, height):
        self.height = height
    
    def get_area(self):
        return self.width * self.height

class Square(Rectangle):
    def __init__(self, side):
        super().__init__(side, side)
    
    def set_width(self, width):
        self.width = width
        self.height = width
    
    def set_height(self, height):
        self.width = height
        self.height = height
```

이는 LSP를 위반합니다. 왜냐하면 Square를 Rectangle로 대체할 수 없으며, 프로그램의 동작을 변경해야하기 때문입니다. 더 나은 접근 방식은 다음과 같습니다:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def get_area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def get_area(self):
        return self.width * self.height

class Square(Shape):
    def __init__(self, side):
        self.side = side
    
    def get_area(self):
        return self.side ** 2
```

<div class="content-ad"></div>

지금은 Square이 Rectangle의 하위 클래스가 아니며 둘 다 Shape 인터페이스를 올바르게 구현했습니다.

# I — 인터페이스 분리 원칙 (ISP)

인터페이스 분리 원칙은 클라이언트가 사용하지 않는 메소드에 의존하도록 강제되어서는 안 된다고 말합니다. 즉, 하나의 범용 인터페이스보다 많은 클라이언트별 인터페이스가 더 나은 것입니다.

## ISP 예시

<div class="content-ad"></div>

다중 기능 프린터 인터페이스를 고려해보세요:

```js
class MultiFunctionPrinter:
    def print(self, document):
        pass
    
    def scan(self, document):
        pass
    
    def fax(self, document):
        pass

class OldPrinter(MultiFunctionPrinter):
    def print(self, document):
        # 실제 인쇄 코드
        pass
    
    def scan(self, document):
        raise NotImplementedError("스캔을 지원하지 않습니다")
    
    def fax(self, document):
        raise NotImplementedError("팩스를 지원하지 않습니다")
```

이것은 ISP를 위반합니다. 왜냐하면 OldPrinter가 지원하지 않는 메서드를 구현해야하기 때문입니다. 더 나은 접근 방식은 다음과 같습니다:

```js
class Printer:
    def print(self, document):
        pass

class Scanner:
    def scan(self, document):
        pass

class FaxMachine:
    def fax(self, document):
        pass

class MultiFunctionPrinter(Printer, Scanner, FaxMachine):
    def print(self, document):
        # 실제 인쇄 코드
        pass
    
    def scan(self, document):
        # 실제 스캔 코드
        pass
    
    def fax(self, document):
        # 실제 팩스 코드
        pass

class OldPrinter(Printer):
    def print(self, document):
        # 실제 인쇄 코드
        pass
```

<div class="content-ad"></div>

지금은 각 장치가 지원하는 인터페이스만 구현합니다.

# D — 의존성 역전 원칙(DIP)

의존성 역전 원칙은 고수준 모듈이 저수준 모듈에 의존해서는 안 된다는 것을 말합니다. 두 모듈 모두 추상화에 의존해야 합니다. 추상화는 세부사항에 의존해서는 안 되며, 세부사항은 추상화에 의존해야 합니다.

## DIP의 예제

<div class="content-ad"></div>

알림 시스템을 고려해보세요:

```js
class EmailNotifier:
    def send_notification(self, message):
        # 이메일을 보내는 코드
        pass

class NotificationService:
    def __init__(self):
        self.notifier = EmailNotifier()
    
    def send_notification(self, message):
        self.notifier.send_notification(message)
```

이 구조는 DIP를 위반합니다. 왜냐하면 고수준의 NotificationService가 저수준의 EmailNotifier에 의존하기 때문입니다. 더 나은 접근 방식은:

```js
from abc import ABC, abstractmethod

class Notifier(ABC):
    @abstractmethod
    def send_notification(self, message):
        pass

class EmailNotifier(Notifier):
    def send_notification(self, message):
        # 이메일을 보내는 코드
        pass

class SMSNotifier(Notifier):
    def send_notification(self, message):
        # SMS를 보내는 코드
        pass

class NotificationService:
    def __init__(self, notifier: Notifier):
        self.notifier = notifier
    
    def send_notification(self, message):
        self.notifier.send_notification(message)
```

<div class="content-ad"></div>

이제 NotificationService는 구체적인 구현이 아닌 추상화(Notifier)에 의존합니다.

# CAP 이론

CAP 이론 또는 Brewer의 이론으로도 알려진 CAP 이론은 분산 시스템에서의 기본 원리입니다. 이 이론은 분산 데이터 저장소가 다음 세 가지 중 두 가지 이상을 동시에 제공하는 것이 불가능하다는 것을 명시합니다:

- 일관성(Consistency): 모든 읽기 작업은 가장 최근의 쓰기 작업 또는 오류를 받습니다.
- 가용성(Availability): 모든 요청에는 (오류가 아닌) 응답이 제공되지만, 그것이 가장 최근의 쓰기를 포함한다는 보장은 없습니다.
- 분할 허용성(Partition tolerance): 시스템은 노드 간 네트워크에서 임의의 수의 메시지가 삭제(또는 지연)되더라도 계속 작동합니다.

<div class="content-ad"></div>

다시 말해, 네트워크 분할이 발생할 때, 일관성과 가용성 사이에서 선택해야 합니다.

# 예시를 통해 CAP 이해하기

세 개의 노드 A, B, C로 구성된 간단한 분산 키-값 저장소를 생각해 봅시다.

- 일관성과 분할 허용성(CP): 일관성과 분할 허용성을 우선시하는 시스템이 있다고 가정해 봅시다. 노드 A와 노드 B, C 사이에 네트워크 분할이 발생한 경우:

<div class="content-ad"></div>

- A 노드에 대한 쓰기 요청은 모든 접근 가능한 노드 간 일관성을 유지하기 위해 거부됩니다.
- B 및 C 노드로의 읽기 요청은 일관된 데이터를 반환하지만, A 노드가 사용 불가능해질 수 있습니다.

- 예시 구현:

```python
class CPSystem:
    def __init__(self):
        self.data = {}
        self.nodes = ['A', 'B', 'C']
      
    def write(self, key, value, node):
        if self.is_partition():
            return "Error: 네트워크 분할로 쓰기가 거부되었습니다"
        self.data[key] = value
        return "쓰기 성공"
      
    def read(self, key, node):
        if self.is_partition() and node == 'A':
            return "Error: 노드 사용 불가"
        return self.data.get(key, "키를 찾을 수 없음")
      
    def is_partition(self):
        # 네트워크 분할 모의
        return random.choice([True, False])
```

2. 가용성 및 분할 허용 (AP): 이제 가용성과 분할 허용을 우선시하는 시스템을 고려해 봅시다. 네트워크 분할의 경우:

<div class="content-ad"></div>

- 모든 노드에 대한 쓰기 요청이 수락됩니다.
- 모든 노드에 대한 읽기 요청은 응답을 반환하지만, 최신의 쓰기일 수는 없을 수 있습니다.

예시 구현:

```python
class APSystem:
    def __init__(self):
        self.data = {'A': {}, 'B': {}, 'C': {}}
    
    def write(self, key, value, node):
        self.data[node][key] = value
        return "쓰기 성공"
    
    def read(self, key, node):
        return self.data[node].get(key, "키를 찾을 수 없음")
    
    def sync(self):
        # 데이터를 주기적으로 동기화하는 데 사용될 것
        pass
```

3. 일관성과 가용성 (CA): 일관성과 가용성을 모두 제공하는 시스템은 파티션을 허용할 수 없습니다. 분산 시스템에서 네트워크 분할은 피할 수 없으므로 이 조합은 일반적으로 실무에서 불가능합니다.

<div class="content-ad"></div>

# 실세계 시스템에서 CAP의 영향

- 관계형 데이터베이스: 전통적인 관계형 데이터베이스인 PostgreSQL은 네트워크 분할이 발생할 때 일관성을 가용성보다 우선시하는 경우가 많다.
- NoSQL 데이터베이스: Cassandra나 DynamoDB와 같은 많은 NoSQL 데이터베이스는 종종 가용성을 일관성보다 우선시하며 최종 일관성 모델을 구현한다.
- 분산 캐시: Redis와 같은 시스템들은 종종 가용성과 분할 허용성을 우선시하며 데이터가 일부 상황에서 오래된 것으로 받아들일 수 있다.

CAP를 이해하면 시스템 디자이너가 분산 시스템에서 트레이드 오프에 대해 정보를 얻고, 응용프로그램의 구체적인 요구에 기반하여 올바른 균형을 선택할 수 있다.

# BASE: ACID의 대안

<div class="content-ad"></div>

BASE는 대규모 분산 시스템에서 전통적인 ACID(Atomicity, Consistency, Isolation, Durability) 속성 대신에 자주 사용되는 일련의 속성을 설명하는 약어입니다. BASE는 다음과 같습니다:

- **Basically Available(기본적으로 사용 가능):** 시스템은 가용성을 보장합니다.
- **Soft state(부드러운 상태):** 시스템의 상태는 입력이 없어도 시간이 지남에 따라 변경될 수 있습니다.
- **Eventual consistency(최종 일관성):** 시스템은 시간이 지남에 따라 일관성을 갖추게 되며, 해당 기간 동안 시스템이 입력을 받지 않는다면 일관성을 유지할 수 있습니다.

BASE는 일관성보다 가용성을 우선시하는 시스템에서 사용되며, CAP 이론의 AP 측면과 일치합니다.

# 예를 통해 BASE 이해하기

<div class="content-ad"></div>

분산형 소셜 미디어 애플리케이션을 고려해 봅시다. 여기서 사용자는 상태 업데이트를 게시하고 다른 사용자를 팔로우할 수 있습니다.

위 코드는 파이썬으로 작성된 분산 소셜 미디어 애플리케이션의 간단한 예시를 보여줍니다.

먼저, `User` 클래스와 `SocialMediaApp` 클래스를 정의하고, 사용자 생성, 상태 게시, 팔로우, 게시물 가져오기 및 노드 동기화와 같은 기능을 구현했습니다.

위 코드를 이용하면 다수의 노드를 시뮬레이션하여 사용자간 데이터를 동기화하고 관리할 수 있습니다.

아래는 사용 예시입니다:

```python
app = SocialMediaApp()
```

위에서 소개한 코드를 사용하여 분산형 소셜 미디어 앱을 만들고 각 사용자의 활동을 관리해보세요!

<div class="content-ad"></div>

```python
# 다른 노드에 사용자 만들기

app.create_user(1, "Alice")
app.create_user(2, "Bob")

# 상태 업데이트 게시

app.post_status(1, "Hello, world!", "A")
app.post_status(2, "Hi there!", "B")
```

<div class="content-ad"></div>

# 사용자 팔로우하기

```python
app.follow_user(2, 1, "C")
```

# 사용자의 게시물 가져오기 (노드마다 일관성이 없을 수 있음)

```python
print(app.get_user_posts(1, "A")) # ["Hello, world!"]
print(app.get_user_posts(1, "B")) # []
```

<div class="content-ad"></div>

# 노드 동기화

`app.sync_nodes()`

# 이제 데이터가 일관성 있어야 합니다

`print(app.get_user_posts(1, "B"))`  # ["Hello, world!"]

<div class="content-ad"></div>

이 예시에서는:

1. **기본적으로 이용 가능** (Basically Available): 시스템은 상태 업데이트와 팔로우 요청을 항상 수락하며, 처음에 한 노드에만 기록되어 있을지라도 수락합니다.

2. **소프트 상태** (Soft State): 시스템의 상태(사용자 게시물 및 팔로워)는 노드 간 업데이트의 비동기적 특성으로 인해 시간이 지남에 따라 변경될 수 있습니다.

3. **최종적 일관성** (Eventual Consistency): `sync_nodes()` 메서드는 모든 노드가 결국 동일한 데이터를 갖도록 보장하여 시간이 흐름에 따라 일관성을 달성합니다.

<div class="content-ad"></div>

이 BASE 방식은 즉각적인 일관성 대신 시스템이 높은 가용성과 분할 허용성을 유지할 수 있도록 합니다. 사용자는 즉시 가장 최신 정보를 볼 수 없을 수 있지만, 시스템은 결국 일관된 상태로 수렴할 것입니다.

# ACID와 BASE 비교

ACID 속성은 강력한 일관성이 필요한 시스템(예: 금융 거래)에 꼭 필요하지만, BASE는 가용성과 분할 허용성이 강력한 일관성보다 우선되는 대규모 분산 시스템에 더 적합합니다.

![KISS, SOLID, CAP, and BASE](/assets/img/2024-07-10-KISSSOLIDCAPandBASEImportantTermsYouMightNotKnow_1.png)

<div class="content-ad"></div>

# 결론

KISS, SOLID, CAP, 그리고 BASE를 이해하는 것은 복잡한 시스템에 작업하는 소프트웨어 개발자나 아키텍트에게 중요합니다. 이러한 원칙과 개념은 견고하고 유지보수 가능하며 확장 가능한 소프트웨어를 만들기 위한 프레임워크를 제공합니다:

- **KISS (Keep It Simple, Stupid)** 는 설계와 구현에서 불필요한 복잡성을 피하고 단순함을 추구하도록 상기시킵니다.

2. **SOLID 원칙**은 더 이해하기 쉽고 유연하며 유지보수하기 좋은 객체 지향적인 설계를 만들기 위해 우리를 안내합니다: — 단일 책임 원칙 — 개방 폐쇄 원칙 — 리스코프 치환 원칙 — 인터페이스 분리 원칙 — 의존성 역전 원칙

<div class="content-ad"></div>

3. **CAP 이론**은 분산 시스템에서 일관성(Consistency), 가용성(Availability), 그리고 분할 허용성(Partition Tolerance) 사이의 기본적인 트레이드오프를 이해하는 데 도움이 됩니다.

4. **BASE (Basically Available, Soft state, Eventual consistency)**는 강력한 일관성보다는 가용성과 분할 허용성을 우선시하는 시스템을 위한 ACID의 대안을 제공합니다.

이러한 원칙을 적용하고 이러한 개념을 이해함으로써, 개발자들은 시스템 디자인에 관한 정보를 바탕으로한 결정을 내릴 수 있으며, 보다 견고하고 효율적인 소프트웨어 솔루션을 이끌어 낼 수 있습니다. 그러나 이러한 것들은 엄격한 규칙이 아닌 지침이라는 것을 명심하세요. 중요한 것은 이러한 원칙을 이해하고 프로젝트의 구체적 요구사항과 제약 조건을 기반으로 신중하게 적용하는 것입니다. 소프트웨어 개발 과정에서 계속해서 나아가면서 이러한 원칙을 염두에 두되, 새로운 개념을 배우고 기술이 발전함에 따라 접근 방식을 조정할 수 있도록 열려 있어야 합니다. 소프트웨어 엔지니어링 분야는 끊임없이 변화하고 있으며, 배우고 적응하는 능력 역시 이러한 근간이해가 중요합니다.