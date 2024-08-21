---
title: "로컬 Python 애플리케이션에서 이벤트 구동 시스템 설계 방법"
description: ""
coverImage: "/assets/img/2024-07-14-DesigningEvent-DrivenSystemsinLocalPythonApplications_0.png"
date: 2024-07-14 00:50
ogImage: 
  url: /assets/img/2024-07-14-DesigningEvent-DrivenSystemsinLocalPythonApplications_0.png
tag: Tech
originalTitle: "Designing Event-Driven Systems in Local Python Applications"
link: "https://medium.com/gitconnected/designing-event-driven-systems-in-local-python-applications-8abf7544d7e4"
isUpdated: true
---




![Event-Driven System](/assets/img/2024-07-14-DesigningEventDrivenSystemsinLocalPythonApplications_0.png)

안녕하세요! 암호화폐 전문가 여러분👋

이벤트 기반 소프트웨어 아키텍처를 사용하면 코드를 더 깔끔하게 작성할 수 있습니다. 대부분의 소프트웨어 엔지니어들은 JavaScript의 keypress, Tkinter의 bind 또는 PyQt/PySide의 keyPressEvent와 같은 프론트엔드 프레임워크/언어에서 제공하는 사전 정의된 이벤트 시스템을 알아두고 있을 것입니다. 아마도 장고 신호(Django Signals)에 대해 들어봤거나 분산 시스템과 관련하여 아파치 카프카(Apache Kafka)나 구글 클라우드 Pub/Sub을 알고 있을 것입니다.

하지만 이벤트 기반 소프트웨어 아키텍처는 그렇게 복잡하지 않습니다.

이벤트 기반 시스템의 구성 요소는 간단합니다: 이벤트가 있고, 이벤트를 생성하는 프로듀서가 있으며, 이벤트를 소비하는 컨슈머가 있습니다. 그런 다음 이벤트 레지스트리라고 불리는 코드가 필요합니다. 이 코드는 프로듀서에서 이벤트를 컨슈머로 전달합니다.

<div class="content-ad"></div>

# 이벤트 기반 소프트웨어 구축

최근에는 Python을 사용하여 그래픽 사용자 인터페이스 개발에 발을 담그기 위해 flitz 파일 관리자를 개발 중입니다. Tkinter 프레임워크로 시작했지만 적합하지 않다는 것을 깨달았습니다. Tkinter와 강하게 결합되어 있어 Qt로 변경하는 것이 어려워졌습니다.

![Designing Event-Driven Systems in Local Python Applications](/assets/img/2024-07-14-DesigningEvent-DrivenSystemsinLocalPythonApplications_1.png)

예를 들어, URL 창을 통해 현재 경로를 변경할 때 목록에 나열된 문서를 변경하고 싶습니다. Flitz에는 “위” 버튼도 있습니다. 클릭하면 URL 창에 표시된 현재 경로가 변경되며 세부 창에 표시된 문서도 변경됩니다.

<div class="content-ad"></div>

개념적으로 코드는 다음과 같이 보일 수 있습니다:

```python
class FileExplorer:
    def __init__(self):
        current_path = Path(".")

        # UI 위젯 표시
        details_view = DetailsView(self)
        url_bar = UrlBar(self)
        up_button = UpButton(self)


class UrlBar:
    def __init__(self, root):
        self.root = root
        self.text = str(self.root.current_path)

    def on_keypress_enter(self):
        # 경로가 변경되었음을 애플리케이션에 알림
        self.root.current_path = Path(self.text)

        # 추가 작업 트리거
        self.root.details_view.refresh()
        self.refresh()

    def refresh(self):
        self.text = str(self.root.current_path)


class UpButton:
    def __init__(self, root):
        self.root = root

    def on_press(self):
        # 경로가 변경되었음을 애플리케이션에 알림
        self.root.current_path = self.root.current_path.parent

        # 추가 작업 트리거
        self.root.details_view.refresh()
        self.root.url_bar.refresh()


class DetailsView:
    def refresh():
        """root.current_path에서 파일을 표시합니다."""
```

UpButton.on_press 및 UrlBar.on_keypress_enter에 로직 중복이 있습니다. 이러한 부분은 애플리케이션이 커질수록 유지 보수가 어려워집니다. 어느 순간 누군가가 애플리케이션의 GUI 위젯 중 하나를 업데이트/새로고침하는 것을 잊게 될 수 있습니다.

대신, set_current_path 함수를 정의할 수 있습니다:

<div class="content-ad"></div>


이미지 태그를 Markdown 형식으로 변경해주세요.

```js
class FileExplorer:
    def __init__(self):
        current_path = Path(".")

        # UI 위젯 표시
        self.details_view = DetailsView(self)
        self.url_bar = UrlBar(self)
        self.up_button = UpButton(self)

    def set_current_path(self, current_path):
        self.current_path = current_path
        self.details_view.refresh()
        self.url_bar.refresh()


class UrlBar:
    def __init__(self, root):
        self.root = root
        self.text = str(self.root.current_path)

    def on_keypress_enter(self):
        self.root.set_current_path(Path(self.text))


class UpButton:
    def __init__(self, root):
        self.root = root

    def on_press(self):
        self.root.set_current_path(self.root.current_path.parent)


class DetailsView:
    def refresh():
        """루트 .current_path에서 파일을 표시합니다."""  
```

다음으로 개선하고자 하는 부분은 코드의 지역성입니다. 즉, 한 구성 요소는 모든 코드를 한 곳에 가지고 있어야 합니다. set_current_path에 관련없는 여러 줄이있으면 이러한 구성 요소를 변경하거나 올바른 작업을 수행하는지 검사하기가 더 어려워집니다. 즉, set_current_path에서 어디를 건드려야 하는 구성 요소가 있는지 코드를 확인하지 않고는 어떻게 알 수 있습니까?

```js
from pathlib import Path

# events.py
class Event:
    def __init__(self, name):
        self.name = name
        self.listeners = []

    def consumed_by(self, listener):
        self.listeners.append(listener)

    def produce(self):
        for listener in self.listeners:
            listener()

current_path_changed = Event('current_path_changed')

# Remaining code:
class FileExplorer:
    def __init__(self):
        self.current_path = Path(".")
        self.event_registry = {}  # 구성 요소 리스너를 보관하는 이벤트 레지스트리

        # UI 위젯 표시
        self.details_view = DetailsView(self)
        self.url_bar = UrlBar(self)
        self.up_button = UpButton(self)

    def set_current_path(self, current_path):
        self.current_path = current_path
        current_path_changed.produce()


class UrlBar:
    def __init__(self, root):
        self.root = root
        self.text = str(self.root.current_path)
        current_path_changed.consumed_by(self.refresh)

    def on_keypress_enter(self):
        self.root.set_current_path(Path(self.text))

    def refresh(self):
        self = self.root.url_bar
        self.text = str(self.root.current_path)
        print(f"UrlBar refreshed")

class UpButton:
    def __init__(self, root):
        self.root = root

    def on_press(self):
        self.root.set_current_path(self.root.current_path.parent.resolve())


class DetailsView:
    def __init__(self, root):
        self.root = root
        current_path_changed.consumed_by(self.refresh)

    def refresh(self):
        """루트 .current_path에서 파일을 표시합니다."""
        print(f"현재 경로에서 파일을 표시하는 세부 정보 뷰를 새로고침합니다 {self.root.current_path}")

# 사용 예시:
explorer = FileExplorer()
explorer.up_button.on_press()
```

blink 같은 라이브러리도 사용할 수 있습니다.


<div class="content-ad"></div>

그게 다야. 이벤트 기반 시스템을 보셨습니다 🎉

이 경우 레지스트리는 모든 이벤트 객체입니다.

# 간단하죠 — 근데 왜 아파치 카프카가 필요할까요?

카프카는 대규모 분산 시스템에서 사용됩니다. 특히 네트워크가 지연과 에러를 가지고 있다는 것을 의미합니다. 소비자들은 이동하고, 생산자들도 마찬가지입니다.

<div class="content-ad"></div>

# 알겠어요, 하지만 왜 blinker가 필요할까요?

- 멀티스레딩
- 소비자 연결 해제
- 연결된 데이터: 이 간단한 예에서는 이벤트가 어떤 데이터와 함께 오지 않았습니다. 이는 많은 사용 사례에서 다릅니다.

# 용어 — 같은 개념을 나타내는 여러 용어들

이벤트와 시그널은 같은 것입니다. 마찬가지로 함께 속하는 몇 가지 쌍이 있습니다:

<div class="content-ad"></div>


Event  | Action        | Follow-up   | Comment
---------------------------------------------------------------------------
이벤트 | 게시자        | 구독자        | Apache Kafka, Google Pub/Sub
이벤트 | 프로듀서      | 컨슈머      | RabbitMQ, Spring Cloud Stream
이벤트 | 디스패치      | 리스너        | JavaScript
시그널 | 보내는 사람 | 받는 사람  | Blinker


# 이벤트 주도 시스템의 함정들

- 순서: 소비자의 실행 순서가 중요하지 않도록 이벤트 주도 시스템을 설계해야 합니다.
- 무한 루프: 이벤트 A가 이벤트 B를 일으키고, 이벤트 B가 이벤트 A를 호출할 수 있습니다.

# 요약


<div class="content-ad"></div>

- **무엇**: 이벤트 기반 시스템은 이벤트, 해당 이벤트를 생성하는 코드 부분, 그리고 해당 이벤트를 소비하는 다른 부분으로 구성됩니다.
- **왜**: 발신자와 수신자를 분리하는 것이 유용합니다. 이는 유지 관리성을 높입니다.
- **어떻게**: 이벤트 기반 소프트웨어를 구현하는 가장 간단한 방법은 신호용 전역 객체를 사용하는 것입니다. 이 신호는 송신자에 의해 사용되며 자동으로 소비자를 호출합니다. Blinker와 같은 라이브러리가 도움이 될 수 있습니다. 분산 시스템에 있다면 상황은 좀 더 복잡해집니다.