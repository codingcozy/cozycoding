---
title: "Godot 4 C을 사용하여 애니메이션 3D 캐릭터 컨트롤러 설정하는 방법"
description: ""
coverImage: "/assets/img/2024-07-14-Settingupananimated3DcharactercontrollerGodot4C_0.png"
date: 2024-07-14 01:06
ogImage: 
  url: /assets/img/2024-07-14-Settingupananimated3DcharactercontrollerGodot4C_0.png
tag: Tech
originalTitle: "Setting up an animated 3D character controller (Godot 4 C#)"
link: "https://medium.com/codex/setting-up-an-animated-3d-character-controller-godot-4-c-ef1c9cd3ddcc"
---


비디오 게임은 다른 세계에 몸을 빼고 배회하는 것이 전부입니다. 3D 캐릭터를 만들어내는 것은 복잡할 수 있지만, 다음 Godot 게임을 위한 기본 캐릭터 컨트롤러를 만드는 것은 어렵지 않습니다!

이번 튜토리얼에서는 간단한 캐릭터 모델을 블렌더에서 준비하고 Adobe의 Mixamo 도구를 사용하여 애니메이션을 적용하는 방법을 배웁니다. 그리고 나서, 이 모델을 GLTF 파일로 Godot에 가져오고, 구성하고 해당 애니메이션을 설정하며 게임에서 이를 사용하기 위한 기본 C# 컨트롤러를 구현하는 방법을 살펴보겠습니다.

평소처럼, C#로 로직을 코딩할 것이므로 .NET이 활성화된 Godot 버전이 필요합니다.

물론, 이 예제의 데모 씬과 모든 에셋은 제 Github에서 확인할 수 있습니다 🚀 다른 Godot 튜토리얼들과 함께 :)

<div class="content-ad"></div>

Also, the assets are from Kenney’s library 🚀

The tutorial is also available as a video — the text version is provided below:

Now, with all that said, let’s dive in and discover how to import and set up an animated 3D character in Godot and C#!

Oh, and by the way, if you’re still new to Godot 4 and C# game development, feel free to check out my brand new "short-read" ebook: L’Almanach: Getting Started!

<div class="content-ad"></div>

![Godot tutorial](https://miro.medium.com/v2/resize:fit:720/1*hzC06eBz2gAUX8Q_anmOcQ.gif)

안녕하세요! 이 빠르고 간단한 가이드는 여러분에게 기초를 탐험하는 방법을 가르치고, C#로 프로그래밍하고, UI를 디자인하고, 심지어는 여러분이 스텝바이스텝으로 틱택토 게임을 만드는 방법을 알려줄 거에요 :)

그러니까 Godot 4/C# 여행을 100 페이지 미만의 분량으로 저렴한 가격에 시작하고 싶다면, Gumroad 페이지를 꼭 확인해보세요...

# 우리의 3D 캐릭터 모델 설정하기

<div class="content-ad"></div>

우선 Blender에 들어가서 오늘 우리가 살펴볼 3D 캐릭터를 살펴보겠습니다.

나는 여기서 Kenney의 라이브러리에서 기본 블록 모양의 캐릭터인 어드벤처러 스킨을 사용할 것이며, 마인크래프트와 비슷한 아바타이며 이미 애니메이션을 위해 뼈대가 구성되어 있습니다.

일반적으로 포즈 모드로 전환하여 뼈를 번역하거나 회전하면 몸이 따라 움직이는 것을 볼 수 있습니다... 그래서 우리는 사실상 이렇게 조금씩 캐릭터를 애니메이션화할 수 있습니다.

하지만 물론, 이것은 꽤 오랜 시간이 소요될 것입니다... 그리고 또한 내가 개인적으로는 좋은 애니메이터 기술을 가져야 한다는 것을 요구합니다 ;)

<div class="content-ad"></div>

So instead, a very cool solution is to use a great tool by Adobe, called Mixamo.

Mixamo is a free pose and animation library that you can utilize to access a wide variety of animations on your models. It is compatible with most game engines, and specifically with Godot.

By default, it will display animations on one of the built-in 3D characters. However, you can always import your own model, usually in the FBX format.

<div class="content-ad"></div>

그리고 오른쪽 미리보기에 직접 바로 바뀐 것을 확인할 수 있습니다:

이제는 원하는 모든 애니메이션을 선택하고 확인하며 다운로드할 수 있는 검색 도구와 라이브러리를 이용할 수 있어요 :)

예를 들어, 이 강좌를 위해 3D 캐릭터를 위한 몇 가지 전형적인 움직임을 다운로드했어요: 정지 자세, 달리기 애니메이션, 떨어지는 애니메이션, 심지를 다듬는 움직임까지 모두요.

이 모든 것들을 Mixamo에서 FBX 형식으로 내보내고 피부 옵션도 활성화하여 내보냈어요:

<div class="content-ad"></div>

런 애니메이션에서는, 내 리그가 게임 엔진에서 애니메이션과 독립적으로 이동하도록 하기 위해 In Place 옵션을 선택했어요:
오케이! 이제, 우리는 Blender로 돌아가서 비 애니메이션 캐릭터를 삭제하고 새 FBX 파일 중 하나로 대체해서 캐릭터의 몸통과 애니메이션을 가져와야 할 거예요:
Blender 장면의 전역 타임라인을 시작하면, 우리가 Mixamo에서 미리보기 및 다운로드한 이 아이들 포즈에 따라 캐릭터가 움직이는 걸 볼 수 있어요.
이제, 캐릭터를 자립시키고 Godot과 같은 게임 엔진에서 사용하기 쉽게 하기 위해, 다른 애니메이션도 다시 가져와서 모두 이 참조 암체어 객체에 여러 작업으로 둘 것이에요.

<div class="content-ad"></div>

먼저, 캐릭터에 대해 실제로 가지고 있는 액션들을 파악하려면 타임라인에서 도프 시트로 패널을 전환해야 합니다.

그럼 왼쪽의 드롭다운을 사용하여 액션 편집기를 표시해 봅시다.

지금은 이름을 쉽게 알아볼 수 있는 "idle"로 바꿀 수 있는 무척 읽기 어려운 단일 액션이 있는 것을 알 수 있습니다.

이제 우리의 다른 Mixamo 애니메이션을 추가하려면 다른 FBX 파일을 가져와야 하는데, Blender가 자동으로 이러한 액션들을 첫 번째 액션 리스트에 추가해 줍니다.

<div class="content-ad"></div>

그러니까, 각각의 이름을 바꿔주면 무엇이 무엇인지 쉽게 알 수 있게 됩니다.

그리고 모든 다른 아마처들의 계층 구조들을 삭제할 수 있도록 모든 복사된 애니메이션을 복사한 아마처들을 선택한 후에 삭제할 수 있습니다.

결국, 작은 모험가 캐릭터의 단 하나의 복사본만 있는데도 이 릭을 위해 모든 애니메이션을 재생할 수 있게되었죠. 마지막 단계는 이 아마처와 그 하위 계층 구조 모두를 선택해서, 모든 자식들과 함께 GLTF로 내보내서 Godot에서 사용할 수 있도록 하는 것입니다. 제 경우에는 장면에 기본 조명과 카메라도 포함되어 있기 때문에 선택해서 내보내기만 해야 합니다.

그리고 이제, 우리는 그냥 이 파일을 Godot 프로젝트로 끌어다 놓기만 하면 되죠 — 그리고 3D 장면에서 우리의 모험가를 구성하고 사용할 준비가 됩니다!

<div class="content-ad"></div>

# 캐릭터의 애니메이터 준비하기

자, 이제 캐릭터 모델이 준비되고 가져와졌으니, 실제로 Godot에서 애니메이션을 어떻게 사용하는지 살펴봅시다 :)

GLTF 모델을 두 번 클릭하면, 블렌더에서 이전에 보았던 메시와 모든 뼈대가 있는 스켈레톤, 그리고 맨 아래에 각각의 액션이 있는 AnimationPlayer 노드가 있는 것을 확인할 수 있습니다.

사실, 여기서 기회가 있으니 애니메이션 중 일부를 루프하도록 Godot에게 알려보겠습니다. 구체적으로는 떨어지는, 대기하는, 그리고 달리는 애니메이션을 선형 모드로 루프하도록 설정해야 합니다. 이 오른쪽 드롭다운에서 설정하세요:

<div class="content-ad"></div>

이제 단순히 모델을 다시 가져와서(팝업 패널 하단의 Reimport 버튼으로) 작업을 시작할 준비가 된 것이죠 :)

얼마 전에 2D 캐릭터 컨트롤러를 다룰 때, 애니메이션된 스프라이트를 사용하여 애니메이션을 재생하는 방법을 보았습니다. 그리고 최근에 또 다른 에피소드에서, 3D 상자의 뚜껑을 회전시키고 이동시키는 AnimationPlayer 노드를 사용하는 방법에 대해 논의했습니다.

Godot 문서에 명시된 대로, AnimationPlayer 노드는 많은 상황에 적응할 수 있는 매우 강력한 도구입니다… 하지만 모든 상황에 적합한 것은 아닙니다. 특히, 여러 사전 제작된 애니메이션을 섞어서 릭을 애니메이트하는 것과 같이 여러 애니메이션을 똑똑하게 조합하려면 흥미로운 도구인 AnimationTree로 이동하는 것이 좋습니다.

AnimationTree 노드는 자체적으로 작동하지 않습니다: 실제로 애니메이션 데이터를 정의하고 저장하는 데 항상 AnimationPlayer 노드가 필요합니다. 그러나 이는 코드의 변수에 따라 애니메이션을 혼합, 순서대로 재생하거나 조건부로 재생하는 편리한 방법입니다. 그래서 일반적으로 3D 캐릭터를 애니메이트하기 위해서 매우 흥미로운 도구입니다 ;)

<div class="content-ad"></div>

일반적으로 AnimationTree 노드를 사용할 때의 기술은 전체 3D 씬을 만들고 GLTF 애니메이션된 3D 모델을 넣은 다음, AnimationTree 노드를 이 모델 옆에 루트 노드 아래에 추가하는 것입니다.

마지막으로, GLTF 가져온 파일 내의 AnimationPlayer 노드에 액세스하려면, 3D 모델 인스턴스를 마우스 오른쪽 버튼으로 클릭하여 편집 가능한 자식 속성을 켠 다음, AnimationTree의 Inspector에서 할당하면 됩니다.

자, 이제 AnimationTree가 정확히 무엇을 할 수 있는지 결정해야 합니다. Tree Root 드롭다운을 살펴보면 AnimationTree를 사용하는 다양한 방법이 있음을 알 수 있습니다:

- 연관된 AnimationPlayer에서 특정 클립을 참조하고 재생할 수 있는 기본 애니메이션 노드가 있습니다.
- 블렌딩 노드는 여러 애니메이션을 조합하고 캐릭터의 스켈레톤에 적용할 양을 제어하는 멋진 방법입니다. 이를 사용하여 Godot에게 애니메이션의 영향을 뼈대의 일부에만 적용하도록 지시하면 몸통의 움직임을 완전히 분리하여 아바타가 달리고 어딘가를 향해 조준할 수 있게 할 수 있습니다.
- 하지만 여기서 우리는 세 번째 옵션, 캐릭터 컨트롤러에 대해 매우 직관적이고 코드 로직과 잘 맞는 StateMachine 기반의 AnimationTree를 선택할 것입니다.

<div class="content-ad"></div>

이제 상태 머신의 개념이 익숙하지 않다면, FSM에 관한 이전 튜토리얼 시리즈를 꼭 확인해보시기를 권장합니다.

참고: 이번 에피소드는 코드 로직에 FSM 디자인 패턴을 사용하는 내용이지만, 상태, 전환, 그리고 행동을 더 직관적인 청크로 분할하는 기본 개념은 애니메이션에도 똑같이 적용됩니다 ;)

AnimationTree 노드로 새로운 상태 머신 루트 노드를 생성하면, Godot은 화면 하단에 Animation Tree 패널이 자동으로 열리는데, 현재 시작과 끝 사각형을 포함하고 있습니다.

이러한 사각형은 트리 내의 노드이며, 여기서는 상태 머신의 상태들과 같습니다. 정말 멋진 점은 이 패널에서 우클릭하여 새 노드를 추가하고 연관된 AnimationPlayer 노드 중 하나의 애니메이션 클립을 선택할 수 있다는 것입니다. 일반적으로, 지금은 "idle"에서 시작해보죠 ;)

<div class="content-ad"></div>

새로운 상태를 상태 머신에 추가했으니 이제 AnimationTree 노드의 Inspector에서 해당 상태를 활성화하면 캐릭터가 이 클립을 재생하고 3D 뷰포트에서 어떻게 보이는지 즉시 미리 볼 수 있어요.

그런데 게임이 시작될 때 이 애니메이션을 기본으로 재생하고 싶다면, 시작 블록과 새로운 idle 블록 사이에 전환을 만들어야 해요. 패널 상단의 전환 도구로 전환 툴로 전환하고, Start 사각형에서 idle로 클릭하여 드래그하면 됩니다.

이제 게임이 시작되면 Godot이 자동으로 시작 블록을 실행하게 되고, 이 전환을 통해 상태 머신이 idle 상태를 기본값으로 사용하게 됩니다. 그리고 이 상태는 AnimationPlayer 노드의 idle 애니메이션과 연결되어 있기 때문에 게임이 실행될 때 캐릭터는 이 포즈로 기본 표시될 거에요.

좋아요, 이제 진짜 중요한 질문은: 어떻게 상태 머신에게 다른 애니메이션을 재생하도록 지시할까요? 그리고 사실, 이 애니메이션을 적절한 시간에 재생하도록 어떻게 지시할 수 있을까요?

<div class="content-ad"></div>

예를 들어, 만약 우리가 우리의 달리는 애니메이션을 위한 또 다른 상태 블록을 추가하고, 휴식 상태와 이 새로운 상태 간의 새로운 전이를 만든다면... 상태 기계는 즉시 그로 전환될 것입니다!

그럼, 어떻게 하면 기계가 휴식 상태를 건너뛰지 않고 즉시 뛰기 상태로 가는 것을 막을 수 있을까요? 어떻게 하면 캐릭터가 움직이기 시작할 때까지 기다리도록 할 수 있을까요?

# 조건부 상태 전이 생성하기

음, 지금은 전환들이 너무 간단해서 뛰기 상태 블록으로 자동 진행하는 것을 막기에 충분하지 않습니다.

<div class="content-ad"></div>

기본적으로, 전환을 생성할 때 Godot은 즉시 이동되어야한다고 가정하므로 스켈레톤의 현재 상태(및 연결된 애니메이션)을 직접 업데이트합니다. 즉, 첫 번째 애니메이션을 심지어 보지 못합니다:이 트리 전체의 동작을 확인하려면 시작 블록의 재생 아이콘을 클릭하여 캐릭터가 즉시 달리기를 시작하는 것을 볼 수 있습니다.

전이를 즉시 트리거하는 것을 방지하려면, 우리의 아이들 상태와 런 상태 사이의 전환 라인을 선택 도구를 사용하고 클릭하여 액세스할 수 있는 인스펙터를 살펴봐야합니다.

이제 우리가 할 수있는 두 가지가 있습니다:

- 첫째, 스위치 섹션에서이 전환의 스위치 모드를 동기화 또는 끝으로 변경할 수 있습니다.
동기화는 변경을 즉시 트리거하지만, 첫 번째 애니메이션을 중단 한 프레임에서 두 번째 애니메이션을 재생하도록하므로 우리가 여기에서 원하는 바는 아닙니다.
종료시에는 Godot이 아이들 애니메이션을 완전히 재생 한 후에 런 애니메이션으로 전환 할 때까지 기다린다는 것을 의미합니다. 여기에서 ... 아이들 애니메이션을 루프로 만들었기 때문에 결코 끝나지 않을 것입니다! 게다가 여전히 좋지 않습니다. 우리는 아바타가 이동 할 때까지 아이들 상태를 유지하고 싶습니다.

<div class="content-ad"></div>

- 두 번째 요령에 대해 이야기해보겠습니다: Advance Condition 또는 Expression 사용

Advance 섹션을 열게 되면, 세 가지 프로퍼티가 포함되어 있는 것을 볼 수 있습니다:

- 맨 위에, 전환 효과를 활성화할지 여부를 결정하는 드롭다운이 있습니다. 우리는 기본값인 Auto로 남겨둘 수 있습니다.
- 그 아래에는 Condition이라는 빈 텍스트 필드가 있습니다.
- 마지막으로 Expression이라는 텍스트 영역이 있습니다.

이름에서 알 수 있듯이, 이것들은 전환을 조건부로 트리거하는 가장 강력한 두 가지 방법이며, 기본적으로 다음과 같이 작동합니다...

<div class="content-ad"></div>

The Condition field allows you to set a parameter for your animation tree that can be controlled through your code, and it should be a boolean value. When this condition is true, the tree will trigger the transition; otherwise, it will stay in its current state.

Another approach is using the Expression property: instead of defining and setting a parameter, you can directly write an expression here that evaluates to a boolean. This expression can utilize any properties exposed in the script attached to the Animation Expression Base Node object within the AnimationTree node.

For instance, let's say we want our character to transition from idle to running when the character's velocity is not null. To accomplish this:

- Initially, we must designate our root "Player" node as the Animation Expression Base Node.

<div class="content-ad"></div>

![image](/assets/img/2024-07-14-Settingupananimated3DcharactercontrollerGodot4C_0.png)

- 그런 다음 전환을 다시 선택하고 표현 필드에 다음과 같은 공식을 작성합니다: velocity.length() ` 0 (속도 벡터가 null이 아닌지 확인하려면):

중요한 점은 우리는 이 표현식에서 C#을 사용할 수 없다는 것을 명심해야 합니다. 그래서, 이번에만 GDScript를 이용해서 작은 상자 안에 그렸던 것이죠 — 그래서 저는 title이 있는 C# 동등물(Length()) 대신에 소문자로 length()를 작성했습니다.:)

물론 속도가 null이면 실행에서 대기로 돌아가는 반대 전환이 있는 다른 표현식도 작성할 수 있습니다:

<div class="content-ad"></div>

그리고 이제 마침내 캐릭터에 이 속도 변수를 실제로 만들어야 합니다!

그러니까, 우리는 실제로 루트 노드의 유형을 기본 Node3D에서 CharacterBody3D로 변경하고 캡슐 충돌체를 제공해야 합니다:

참고: 이전 에피소드에서 논의했던 바와 같이 CharacterBody 노드(2D 또는 3D)는 객체를 물리학을 사용하여 코드에서 이동시키는 데 가장 적합한 도구입니다.

그런 다음, 그 위에 'Player'라는 새로운 스크립트를 만들어봅시다:

<div class="content-ad"></div>

거기에서 Godot가 우리를 위해 CharacterBody3D 템플릿을 멋지게 설정해 두었죠 :)

그래서 스크립트를 만들고 IDE에서 열면 이미 많은 코드를 공부할 수 있어요!

```csharp
using Godot;
using System;

public partial class Player : CharacterBody3D {
    public const float Speed = 5.0f;
    public const float JumpVelocity = 4.5f;

    // 프로젝트 설정에서 중력을 가져와 RigidBody 노드와 동기화합니다.
    public float gravity = ProjectSettings.GetSetting("physics/3d/default_gravity").AsSingle();

    public override void _PhysicsProcess(double delta) {
        Vector3 velocity = Velocity;

        // 중력 추가
        if (!IsOnFloor())
            velocity.Y -= gravity * (float)delta;

        // 점프 처리
        if (Input.IsActionJustPressed("ui_accept") && IsOnFloor())
            velocity.Y = JumpVelocity;

        // 입력 방향 가져오고 이동/감속 처리
        // UI 동작을 사용자 지정 게임플레이 동작으로 대체해야 합니다.
        Vector2 inputDir = Input.GetVector("ui_left", "ui_right", "ui_up", "ui_down");
        Vector3 direction = (Transform.Basis * new Vector3(inputDir.X, 0, inputDir.Y)).Normalized();
        if (direction != Vector3.Zero) {
            velocity.X = direction.X * Speed;
            velocity.Z = direction.Z * Speed;
        } else {
            velocity.X = Mathf.MoveToward(Velocity.X, 0, Speed);
            velocity.Z = Mathf.MoveToward(Velocity.Z, 0, Speed);
        }

        Velocity = velocity;
        MoveAndSlide();
    }
}
```

지금 캐릭터 이동에 익숙하지 않다면, 예전에 2D 캐릭터 컨트롤러 에피소드에서 논의한 내용을 다시 참고해 보세요.

<div class="content-ad"></div>

기본 아이디어는 각 프레임마다 _PhysicsProcess() 내장 후크에서 물리를 처리하는 경우, 사용자 입력에 따라 속도를 업데이트하고 그에 따라 객체를 이동시키는 것입니다.

보다 구체적으로 말하면: 캐릭터가 공중에 있을 때는 중력을 적용하여 땅으로 떨어지도록합니다. 그런 다음, 가능한 점프 입력을 확인하여 위로 올라 가도록합니다. 그런 다음, 내장된 Godot 입력 액션을 사용하여 캐릭터를 수평으로 이동시킵니다. 마지막으로, MoveAndSlide() 호출은 실제로 이 프레임에 대한 이러한 새로 계산된 속도를 사용하여 캐릭터를 이동시킵니다.

이 코드는 꽤 멋지며 실행하면 이미 앞으로, 뒤로, 점프 및 좌우로 이동할 수 있음을 알 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*NOBMTcwdrREjHEs8LbWGfA.gif)

<div class="content-ad"></div>

그리고 놀라운 소식입니다! 이제 만일 우리가 속도 변수를 클래스 자체로 추출하고 [Export] 속성으로 노출하고, 그리고 _Ready() 함수에서 AnimationTree 노드에 대한 참조를 가져와(scene 시작시 활성화되는지 확인하기 위해):

```js
public partial class Player : CharacterBody3D {
    // ...

    [Export] public Vector3 velocity = Velocity;

    public override void _Ready() {
        GetNode<AnimationTree>("AnimationTree").Active = true;
    }

    public override void _PhysicsProcess(double delta) {
        velocity = Velocity;

        // ...
    }
}
```

그럼 게임을 실행해보면... 자동으로 애니메이션이 실행됩니다!

![애니메이션](https://miro.medium.com/v2/resize:fit:1400/1*8JZEHB91VPiq24kzmmb9FA.gif)

<div class="content-ad"></div>

그럼 좋아요 — 우리 캐릭터는 이제 수평으로 이동할 때까지 가만히 서 있고, 그러다가 뛰기 시작하며, 움직임을 멈추면 다시 가만히 서 있는 걸로 돌아갑니다. 참 멋지죠 :)

물론, 모든 애니메이션에 정확히 맞지는 않기 때문에 이제 몇 가지 논의할 사항이 더 있습니다…

# 애니메이션된 3D 캐릭터 컨트롤러 구현

좋아요, 이제 캐릭터 컨트롤러가 전진하거나 후진하고, 점프할 수 있으며, 좌우로 이동할 수 있게 되었습니다. 이제 좌우로 이동하는 것도 멋질 수는 있지만, 여기서는 좌우 이동 애니메이션이 없고, 게다가 저는 옆으로 미끄러지는 대신 캐릭터가 회전하길 원합니다.

<div class="content-ad"></div>

그래서 마지막 파트에서는 Godot의 3D 캐릭터 컨트롤러 템플릿을 수정하여 캐릭터가 수직 축 주위로 회전하고 필요할 때 떨어지는 애니메이션을 제대로 얻을 수 있도록 상태 머신에서 땅을 올바르게 확인하는 것으로 바꿀 것입니다.

먼저, 좌우 이동을 회전으로 변경하는 것은 실제로 그리 어렵지 않습니다.

기본적으로, Godot의 GetVector() 입력 컨버터를 사용하여 4개의 입력 방향을 취하고 일치하는 2D 벡터를 결정하는 대신 X 및 Y 입력 방향을 분리할 것입니다. 이렇게 하면 X 방향을 회전에 사용하고 Y 방향은 전진 또는 후진 운동에 사용할 것입니다.

이 작업을 하려면, 단순히 GetAxis() 입력을 사용하여 turnStrength와 moveStrength를 얻는 것이 필요합니다:

<div class="content-ad"></div>


public override void _PhysicsProcess(double delta) {
    // ...

    // 입력 방향을 가져오고 이동/감속을 처리합니다.
    // 좋은 습관으로 UI 동작을 사용자 지정 게임플레이 동작으로 대체해야 합니다.
    float turnStrength = Input.GetAxis("ui_left", "ui_right");
    float moveStrength = Input.GetAxis("ui_up", "ui_down");

    Vector3 direction = (Transform.Basis * new Vector3(0, 0, moveStrength)).Normalized();
    // ...
}


그리고 우리의 방향 벡터 계산에서, inputDir 변수를 로컬 전방 벡터(Z 좌표)의 moveStrength로 대체해준다면, 수직 입력 축이 앞뒤 이동에만 영향을 미치도록 할 수 있습니다:


public override void _PhysicsProcess(double delta) {
    // ...

    // 입력 방향을 가져오고 이동/감속을 처리합니다.
    // 좋은 습관으로 UI 동작을 사용자 지정 게임플레이 동작으로 대체해야 합니다.
    float turnStrength = Input.GetAxis("ui_left", "ui_right");
    float moveStrength = Input.GetAxis("ui_up", "ui_down");

    Vector3 direction = (Transform.Basis * new Vector3(0, 0, moveStrength)).Normalized();
    // ...
}


또한, turnStrength 입력에 따라 캐릭터가 왼쪽 또는 오른쪽으로 회전하도록 하고 일정한 회전 속도를 설정하기 위해 RotateY() 내장 메서드를 사용할 수 있습니다:


<div class="content-ad"></div>

```csharp
public partial class Player : CharacterBody3D {
    // ...

    public const float RotationVelocity = 3.5f;

    public override void _PhysicsProcess(double delta) {
        // ...
    
        // Get the input direction and handle the movement/deceleration.
        // As good practice, you should replace UI actions with custom gameplay actions.
        float turnStrength = Input.GetAxis("ui_left", "ui_right");
        float moveStrength = Input.GetAxis("ui_up", "ui_down");
    
        RotateY(-Mathf.DegToRad(turnStrength * RotationVelocity));
    
        Vector3 direction = (Transform.basis * new Vector3(0, 0, moveStrength)).Normalized();
        // ...
    }
}
```

Moreover, it's recommended to create custom input actions to replace Godot’s UI-related actions from the default template. Let's define actions for left, right, forward, backward movements, jumping, and punching:

![animated 3D character controller image](/assets/img/2024-07-14-Settingupananimated3DcharactercontrollerGodot4C_1.png)

Then, update the script to use these custom actions, like this:

<div class="content-ad"></div>

```cs
public override void _PhysicsProcess(double delta) {
    Vector3 velocity = Velocity;

    // 중력을 추가합니다.
    if (!IsOnFloor())
        velocity.Y -= gravity * (float)delta;

    // 점프 처리.
    if (Input.IsActionJustPressed("jump") && IsOnFloor())
        velocity.Y = JumpVelocity;

    // 입력 방향을 받아 이동/감속을 처리합니다.
    // 좋은 습관으로 UI 액션을 사용자 지정 게임플레이 액션으로 대체해야 합니다.
    float turnStrength = Input.GetAxis("left", "right");
    float moveStrength = Input.GetAxis("forward", "backwards");

    RotateY(-Mathf.DegToRad(turnStrength * RotationVelocity));

    Vector3 direction = (Transform.basis * new Vector3(0, 0, moveStrength)).Normalized();    if (direction != Vector3.Zero)
    // ...
}
```

여기서 배워본 내용을 확인해보세요! 게임을 다시 실행하면 이제 플레이어가 왼쪽 또는 오른쪽 키를 누를 때 캐릭터가 회전하고, 위 또는 아래 키를 누를 때 앞이나 뒤로 이동하며, 애니메이션도 자동으로 업데이트됩니다 ;)

![example](https://miro.medium.com/v2/resize:fit:1400/1*ghe8bm0KOBVb0ojvCVjDjw.gif)

다음 단계는 캐릭터가 발 아래 땅이 사라질 때 떨어지는 애니메이션으로 전환되도록 하는 것입니다.

<div class="content-ad"></div>

그러니까, 먼저 상태 기계에 이것을 추가해 봅시다:


![이미지](/assets/img/2024-07-14-Settingupananimated3DcharactercontrollerGodot4C_2.png)


그런 다음, idle 상태에서 이 새로운 떨어지는 상태로 전환을 만들어야 하고 아바타가 바닥에 없을 때에만 트리거되도록 설정해야 합니다.

트랜지션 표현식의 정말 멋진 점은, 소스 객체로 CharacterBody3D 노드를 사용하고 있기 때문에 내장 is_on_floor() 메소드를 직접 사용할 수 있고 정상 작동한다는 것입니다! :)

<div class="content-ad"></div>

(다시 말하지만, 미리 준비된 표현은 GDScript로 작성해야 하며 C# 대신 여기서는 스네이크 케이스 버전을 사용하고 있습니다…)

그런 다음 캐릭터가 바닥에 있는 경우를 역 전환하는 것도 물론 가능하며, 해당 속도가 현재 null인지도 확인해야 합니다:

만약 null이 아니라면, 그럼에도 불구하고 케릭터의 러닝 상태로 전환해야 합니다. 그래서 마지막으로 이 새로운 전환 라운드를 완성하기 위해 러닝 상태와 떨어지는 상태 사이에 링크를 추가할 수 있습니다. 캐릭터가 바닥에 없는지 여부를 확인하는 동일한 확인 절차를 한 방향으로 재사용하되, 다른 방향으로는 해당 속도가 null이 아닌지 확인해야 합니다:

이 시점에서 게임을 다시 실행하면, 점프해서 바닥에 있지 않을 때 바로 캐릭터가 떨어지는 애니메이션으로 전환되고, 그 후에는 우리가 걷고 있는지 여부에 따라 쉬는 애니메이션 또는 러닝 애니메이션 중 하나로 돌아갑니다! 게다가 멋진 점은 언덕에서 떨어지기만 하더라도 작동한다는 것이며, 이는 어떤 점프 상태와도 관련이 없기 때문에 아주 멋집니다 :)

<div class="content-ad"></div>

![AnimationTree](https://miro.medium.com/v2/resize:fit:1400/1*ZL4zntj8_-WtD_SdrC0MuA.gif)

이 시스템의 멋진 점은 코드를 업데이트하지 않아도이 새로운 애니메이션과 조건부 전환을 AnimationTree에 추가할 수 있다는 것입니다. 적절한 소스 노드를 준비했다면, 일반적으로 올바른 유형을 선택함으로써 몇 초만에 고급 검사를 즉시 설정할 수 있습니다.

이제 이 튜토리얼을 마무리하기 위해 조건부 전환을 수행하는 다른 기법의 예제를 빨리 살펴 보겠습니다. 고급 조건을 사용합니다.

예를 들어, 마지막 동작 인 펀치를 상태 머신에 통합하고 싶다고 가정해 봅시다. 목표는 땅 위에 있을 때 펀치 입력 동작을 트리거하는지 확인하고, 그 경우 캐릭터의 움직임을 중단하고 애니메이션을 재생한 다음, 완료되면 다시 대기 상태로 돌아가는 것입니다.

<div class="content-ad"></div>

AnimationTree 요소에서 바라보면 설정하는 것이 꽤 쉽습니다. 우리는 새 애니메이션을 새 상태로 추가하고, 유휴 및 실행 상태에서 해당 상태로 전환을 생성하며, 애니메이션이 종료될 때 펀치에서 유휴 상태로 반대 전환을 만들면 됩니다.

이를 위해 앞에서 본 At End Switch Mode를 사용할 수 있습니다.

그러나 펀치 상태로의 전환을 위해 이번에는 식(expression)을 사용하는 것이 조금 지나치다고 생각될 수 있습니다.

왜냐하면 사실 우리는 스크립트에서 부울 값을 만들어야 하며, 만약 액션을 트리거한다면 _PhysicsProcess() 메서드에서 해당 값을 설정한 다음, 이 값을 그냥 전달하는 식(expression)에 사용해야하기 때문입니다.

<div class="content-ad"></div>

단일 true/false 플래그에 대해 약간 복잡하죠. 여기서는 예를 들어 "punch"와 같은 진행 조건으로 이 플래그를 정의하는 것이 더 쉬울 겁니다. 그런 다음 스크립트에서 이를 설정하면 되겠죠.

(심지어 에디터에서 특정 설정을 시험하고 애니메이션 상태의 올바른 순서가 트리거되는지 확인하기 위해 토글하여 활성화 또는 비활성화할 수도 있습니다 :) )

<div class="content-ad"></div>

하지만 어쨌든, 우리는 펀치 로직을 처리해 보겠습니다. 바닥에 있고 펀치 액션을 방금 실행했다면, 임시로 "punched" 변수를 true로 설정할 겁니다:

```js
public override void _PhysicsProcess(double delta) {
    velocity = Velocity;
    bool punched = false;
    
    // 중력 추가
    if (!IsOnFloor())
        velocity.Y -= gravity * (float)delta;
    else {
        // 점프 처리
        if (Input.IsActionJustPressed("jump"))
            velocity.Y = JumpVelocity;

        // 펀치 처리
        if (Input.IsActionJustPressed("punch"))
            punched = true;
    }

    // ...
}
```

이 임시 플래그를 AnimationTree 노드의 조건 매개변수 값으로 할당할 것입니다. 이 노드를 참조하고, 해당 노드의 매개변수/조건/펀치 변수를 설정합니다:

```js
public partial class Player : CharacterBody3D {
    // ...

    private AnimationTree _anim;

    public override void _Ready() {
        _anim = GetNode<AnimationTree>("AnimationTree");
        _anim.Active = true;
    }

    public override void _PhysicsProcess(double delta) {
        velocity = Velocity;
        bool punched = false;
        
        // 중력 추가
        if (!IsOnFloor())
            velocity.Y -= gravity * (float)delta;
        else {
            // 점프 처리
            if (Input.IsActionJustPressed("jump"))
                velocity.Y = JumpVelocity;
    
            // 펀치 처리
            if (Input.IsActionJustPressed("punch"))
                punched = true;
            _anim.Set("parameters/conditions/punch", punched);
        }
        // ...
    }
}
```

<div class="content-ad"></div>

마침내, 우리가 펀치하는 동안 움직임을 중단시킬 수 있는 멋진 속임수를 사용할 수 있습니다. 여기서 우리는 상태 기계의 현재 상태에 직접 액세스하는 방법을 사용할 것이며, 이 때 "펀치"인지 확인합니다.

이를 위해 AnimationTree 노드의 재생 매개변수에 액세스하여 현재 노드를 확인할 수 있습니다. 그런 다음, 캐릭터의 속도를 재설정하고 _PhysicsProcess() 메서드의 나머지 로직을 건너뛸 것입니다:

```js
public partial class Player : CharacterBody3D {
    // ...

    private AnimationTree _anim;
    private AnimationNodeStateMachinePlayback _animPlayback;

    public override void _Ready() {
        _anim = GetNode<AnimationTree>("AnimationTree");
        _animPlayback = (AnimationNodeStateMachinePlayback) _anim.Get("parameters/playback");
        _anim.Active = true;
    }

    public override void _PhysicsProcess(double delta) {
        velocity = Velocity;
        bool punched = false;
        
        // 중력 추가.
        if (!IsOnFloor())
            velocity.Y -= gravity * (float)delta;
        else { ... }

        // 펀치? 움직임 중지.
        if (_animPlayback.GetCurrentNode() == "punch") {
            velocity = Vector3.Zero;
            Velocity = velocity;
            return;
        }
        // ...
    }
}
```

여기까지입니다! 이제 우리는 사용자 지정 입력 작업으로 움직이고 회전할 수 있는 기본 3D 캐릭터 컨트롤러를 성공적으로 구현했으며, 이러한 움직임에 맞는 다양한 애니메이션을 재생할 수 있습니다. :)

<div class="content-ad"></div>

![image](https://miro.medium.com/v2/resize:fit:1400/1*VueVso9i0IdPZGBzMfb9wQ.gif)

# 결론

그래서 — 여기 다 되었어요: Blender에서 간단한 3D 아바타를 설정하고 Mixamo를 통해 애니메이션을 부여한 다음 이를 사용하여 Godot에서 물리 기반 이동과 애니메이션을 하는 3D 플레이어 아바타를 만드는 방법을 알게 되었어요!

만약 이 튜토리얼을 즐겼다면, 기사에 박수를 치거나 다음 글을 놓치지 않기 위해 저를 팔로우해주시고, 물론 배우고 싶은 Godot의 트릭 아이디어를 댓글로 남겨주셔도 괜찮아요!

<div class="content-ad"></div>

언제나 읽어주셔서 감사합니다! 건강하세요 :)

더 많은 콘텐츠를 읽으시려면 중요한 많은 작가들의 기사들을 Medium에서 보실 수 있습니다. 회원이 되어 작가들을 지원해주세요.