---
title: "상속보다 컴포지션을 선호해야 하는 이유 논란의 중심을 파헤치다"
description: ""
coverImage: "/assets/img/2024-07-01-CompositionOverInheritanceAControversialTake_0.png"
date: 2024-07-01 21:30
ogImage: 
  url: /assets/img/2024-07-01-CompositionOverInheritanceAControversialTake_0.png
tag: Tech
originalTitle: "Composition Over Inheritance; A Controversial Take"
link: "https://medium.com/@xainulabideen600/composition-over-inheritance-a-controversial-take-9570738b512c"
isUpdated: true
---





![Alt text](/assets/img/2024-07-01-CompositionOverInheritanceAControversialTake_0.png)

어느 좋은 날, 새로운 게임을 개발하는 데 몰두하고 있는 당신을 발견합니다. 마음은 상쾌하고 열정적입니다. 여러분은 MonoBehaviours를 빠르게 탐색하며 옛날에 작성한 FPS 컨트롤러와 함께 속속들이 진행되고 있는 프로젝트를 보고 있습니다. 최소한 여러분의 관점에서는 프로젝트가 올바른 방향으로 진행되고 있는 것 같습니다. 그러나 코드를 리팩토링하기 위해 준비할 때, 당신의 복잡한 게임 로직이나 정교한 수학적 표현 내에서가 아니라 게임 디자인 내에서 미묘하지만 익숙한 문제가 발생합니다.

여러분은 선택을 의심하기 시작합니다: “왜 여기에서 상속을 사용했을까?” 혹은 반대의 경우일 수도 있습니다. 사실, 그리고 잘 들어보세요, 모든 개발자는 새 프로젝트를 착수하거나 예전 저장소를 리팩토링할 때마다 이 결정과 마주치게 됩니다. 본 기사에서는 개발 초기 단계에서 직접 경험했던 문제 중 하나에 대해 다루고자 합니다: 상속 대 조립의 딜레마

기본 아이디어

<div class="content-ad"></div>

먼저 상속과 컴포지션의 기본적인 차이점에 대해 알아보겠습니다:

상속은 한 클래스가 다른 클래스를 확장할 때 발생하며, 기능을 상속하고 선택적으로 재정의하거나 자체 기능을 추가합니다.

컴포지션은 클래스가 인터페이스를 구현하고 해당 인터페이스에 정의된 각 메서드에 대한 자체 구현을 제공하는 것을 의미합니다.

상속은 파생된 클래스가 기본 클래스의 동작을 빌드하거나 수정하는 유기적인 개념으로 자주 인식됩니다. 반면에, 컴포지션은 클래스가 다른 클래스나 인터페이스로 구성되어 기능을 달성하는 더 선형적인 개념으로 여겨집니다.

<div class="content-ad"></div>

일부 어린이(파생 클래스)는 부모(베이스 클래스)와 크게 다를 수 있지만, 이러한 행동의 차이는 소프트웨어 아키텍처에 깊은 영향을 끼칠 수 있습니다.

실제로, 상속과 합성 중 어떤 방식을 선택하느냐에 따라 소프트웨어 시스템의 아키텍처에 큰 영향을 줄 수 있습니다.

큰 그림

당신과 같은 상황에서 어떤 접근 방식 - 상속 또는 합성 - 이 더 적합할지 알아보기 위해 한 가지 예시를 살펴보겠습니다. 최근에 저는 Selection System을 구현해야 하는 프로젝트에 참여했습니다. 이 시스템은 플레이어 앞의 객체를 강조하고 선택된 객체를 기반으로 다양한 동작을 트리거하는 액션 버튼을 활성화하는 것이 목표였습니다.

<div class="content-ad"></div>

우선, `Select`, `Deselect`, 그리고 `Take Action` 세 가지 메서드를 포함한 인터페이스를 만들었습니다.

```java
public interface ISelectable {
  public void Select();
  public void Deselect();
  public void TakeAction();
}
```

그러나 곧 Interface Segregation Principle을 위반한다는 것을 깨달았고, 이를 해결하기 위해 두 개의 인터페이스로 나누었습니다: ISelectable과 IAction. 각 인터페이스에는 각각의 메서드 세트가 포함되어 있었는데, ISelectable에는 `Select`와 `Deselect`가 있고, IAction에는 `Take Action`이 있었습니다.

```java
public interface ISelectable {
  public void Select();
  public void Deselect();
}

public interface IAction {
  public void TakeAction();
}
```

<div class="content-ad"></div>

예를 들어, 문을 선택하고 강조 표시한 후 열기 및 닫기와 같은 동작이 가능합니다. 모든 것이 순조롭게 진행되는 것처럼 보였지만 또 다른 문제가 발생했습니다. ISelectable 인터페이스를 구현하는 거의 모든 게임 오브젝트가 동일한 동작을 수행했습니다. 바로 아웃라인 셰이더에 대한 참조를 가져와 선택 시 활성화하는 것입니다.

이 중복성으로 인해 제가 이러한 목적으로 인터페이스를 사용하는 것에 대해 의문을 품었습니다. 이러한 설계를 위해 추상 클래스를 선택했을 수도 있습니다.

```csharp
public abstract class BaseSelect
{

  //일부 필드 및 속성

  public virtual void Select()
  {
    //기본 동작
  }

  public virtual void Deselect()
  {
    //기본 동작
  }
}
```

일부 게임 오브젝트가 선택 시 다르게 작동할 수 있지만, 윤곽선 대신 투명해지는 등 이러한 문제를 상속을 사용하여 해결할 수 있을까요? 놀랍게도, 그 대답은 '아니요'입니다!

<div class="content-ad"></div>

### 마법 같은 말

다수의 상담과 연구 끝에 유산의 주요 문제는 "물체가 무엇인가"라는 개념을 중심으로 구축되어 있는 반면, 구성은 "물체가 무엇을 할 수 있는가"를 중심으로 구축되어 있다는 것을 발견했습니다.

선택 시 문은 빨갛게 변할 수 있고, 유리는 윤곽을 가질 수 있으며, 무기는 선택 시 빛날 수 있습니다.

다른 관점으로 볼 때 유산은 "IS A" 관계를 구성하고, 구성은 "HAS A" 관계를 사용한다고 할 수 있지만, 이 해부학은 다양한 예시에서 양쪽 방식으로 표현될 수 있습니다. 예를 들어 남자이므로 나는 인간 클래스에 속한다(유산)고 할 수 있지만, 귀나 머리카락도 가지고 있다(구성)고 할 수 있습니다.

<div class="content-ad"></div>

Composition은 코드 재사용성을 장려하는데, 상속의 경우 대부분 어렵다고 말하기 어려운 부분이 있습니다.

요약

이제 우리 소프트웨어 프로젝트에서 따를 가장 좋은 방법은 무엇인가라는 질문이 제기됩니다. 대부분의 다른 소프트웨어 엔지니어들과 마찬가지로 저는 Composition을 더 선호합니다. 왜냐하면 이는 애플리케이션 아키텍처를 향상시키며 더 유연하며 유닛 테스트에서는 단순히 이길 수 없기 때문입니다. 그러나 더 빠른 구현이 필요할 때, 필드 및 메소드에 암시적으로 액세스 한정자를 제공하고 사용자 지정 생성자 및 소멸자를 작성하고 추상 클래스를 사용하는 경우가 있습니다. 문제는 여러 클래스에서 파생해야 하는 경우 발생합니다(모든 언어가 그것을 지원하는 것은 아닙니다). 상속을 일반적으로 참을 수 없는 이유는 미래를 예측하도록 강요하기 때문입니다. 인간은 항상 변화를 원하고 소프트웨어도 마찬가지이기 때문에 Composition은 코드 변경을 쉽게하고 소프트웨어에서 어떤 중단도없이 변화할 수 있게 합니다.