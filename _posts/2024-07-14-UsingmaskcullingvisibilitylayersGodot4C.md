---
title: "Godot 4 C에서 마스크 컬링과 가시성 레이어 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-14-UsingmaskcullingvisibilitylayersGodot4C_0.png"
date: 2024-07-14 01:19
ogImage: 
  url: /assets/img/2024-07-14-UsingmaskcullingvisibilitylayersGodot4C_0.png
tag: Tech
originalTitle: "Using mask culling , visibility layers (Godot 4 C#)"
link: "https://medium.com/codex/using-mask-culling-visibility-layers-godot-4-c-7d3b8b0415d5"
isUpdated: true
---




게임 엔진에서 물체 렌더링은 핵심 시스템 중 하나입니다. 그런데 여러 해 동안, 게임 엔진 제작자들은 이 기본 기능을 다양한 옵션으로 강화하여 사용자가 자신들의 게임 렌더링을 더욱 효과적으로 제어할 수 있도록 했습니다.

특히, 대부분의 게임 제작 소프트웨어에서 사용 가능한 매우 일반적인 기능 중 하나는 가시성 레이어(visibility layers)입니다. 오늘은 이 도구가 왜 흥미로운지, 어떻게 노드에 대한 특정 가시성 레이어를 설정하는지, 인스펙터 패널과 C# 코드를 통해 어떻게 사용하는지, 그리고 카메라에서 일부 물체를 제거하는 방법을 살펴볼 것입니다.

중요한 참고 사항: 이 튜토리얼에서는 물리 레이어(physics layers)에 대해 다루지 않고 가시성 레이어에만 초점을 맞춥니다. 두 개념은 비슷하게 들리지만 서로 다르며, 목표도 다릅니다. 오늘은 가시성 레이어를 통해 물체를 표시하거나 숨기는 방법에 중점을 두겠습니다 :)

<div class="content-ad"></div>

우리는 항상 C#로 로직을 코딩할 것이므로 .NET이 활성화된 Godot 버전이 필요합니다.

물론, 이 데모의 데모 씬과 자산은 제 Github에서 확인하실 수 있습니다 🚀 그리고 나의 다른 Godot 튜토리얼과 함께.

또한, 이미지, 오디오 및 3D 모델 자산은 Kenney가 제작했습니다 🚀

이 튜토리얼은 비디오로도 제공됩니다 — 문서 버전은 아래에 있습니다:

<div class="content-ad"></div>

하지만, 지금 이야기해 놓은 모든 것을 갖추었으니, 이제 가볍게 들어가서 Godot과 C#에서 노드의 가시성 레이어를 설정하는 기본 사항을 알아보겠습니다!

# 왜 가시성 레이어를 조작해볼까요?

도구 자체에 들어가기 전에 먼저 가시성 레이어가 왜 흥미로운지 간단히 되짚어 봅시다. 게임 엔진에서와 마찬가지로, Godot은 다음과 같은 이유로 이 도구를 구현했습니다:

- 먼저, 가시성 레이어를 통해 다중 뷰 및 분할 화면과 같은 고급 효과를 만들 수 있습니다. 장면의 일부에만 영향을 미치는 부분 후처리, 게임 상태나 플레이어와의 상호작용에 따라 일부 요소를 동적으로 렌더링하는 등의 작업이 가능합니다.
- 둘째, 가시성 레이어는 조직화와 반복적인 디자인에 도움이 될 수도 있습니다. 서로 다른 레이어에 요소를 배치하고 조건부로 표시하거나 숨김으로써 복잡한 장면을 더 잘 준비하고 원하는 부분만 수정할 수 있게끔 할 수 있습니다.
- 마지막으로, 최적화와 디버깅 측면에서도 유용합니다. 사용하지 않는 요소들을 쉽게 끄고 렌더 작업 부하를 줄이거나 시각적 문제가 어디서 발생했는지 명확하게 구분할 수 있도록 도와줍니다.

<div class="content-ad"></div>

전체적으로 Visibility Layers는 우리에게 장면 렌더링을 보다 세밀하게 제어할 수 있는 효율적이고 잘 구성된 강력한 게임을 만들기 위한 멋진 도구입니다.

# Visibility 레이어 설정하기

먼저, 에디터에서 노드의 Inspector를 사용하여 노드에 직접적으로 Visibility 레이어를 사용하는 방법을 보겠습니다.

기억해 둬야 할 점은 이러한 Visibility 옵션이 많은 노드 유형에서 사용 가능하지만 모든 노드에 적용되는 것은 아닙니다. 더 구체적으로 말하자면, CanvasItem 또는 VisualInstance3D 유형을 사용하거나 해당 유형에서 파생된 노드에만 Visibility 레이어를 설정할 수 있다는 것입니다.

<div class="content-ad"></div>

보통 이 도구는 매우 기본적인 노드 타입인 Node 또는 Node3D 타입에서 제공되지 않는다.

하지만 CanvasItem이나 VisualInstance3D를 사용하고 계신다면, 그들의 Inspector에 Visibility Layer 또는 Layers라고 하는 속성이 있는데, 작은 숫자 표가 함께 표시됩니다:

![image](/assets/img/2024-07-14-UsingmaskcullingvisibilitylayersGodot4C_0.png)

이 셀들은 노드를 배치할 수 있는 모든 레이어를 나타내며, 1부터 20까지 입니다. 그리고 보시다시피, 셀을 클릭하여 간단히 레이어를 활성화 또는 비활성화할 수 있습니다:

<div class="content-ad"></div>

우리는 이 표를 사용하여 노드의 레이어를 매우 쉽게 변경하고 동시에 하나 이상의 레이어에 배치할 수 있습니다. 우리의 노드는 이 표에서 토글된 모든 레이어에 있게 됩니다.

예를 들어:

- 기본 설정인 이 구성은 노드가 가시성 레이어 번호 하나에만 있는 것을 의미합니다:

![이미지](/assets/img/2024-07-14-UsingmaskcullingvisibilitylayersGodot4C_1.png)

<div class="content-ad"></div>

- 이 경우는 레이어 번호 두에만 있다는 것을 의미합니다:

![Godot4C_2](/assets/img/2024-07-14-UsingmaskcullingvisibilitylayersGodot4C_2.png)

- 이 경우는 양쪽 레이어에 모두 있다는 것을 의미합니다:

![Godot4C_3](/assets/img/2024-07-14-UsingmaskcullingvisibilitylayersGodot4C_3.png)

<div class="content-ad"></div>

이제 기본적으로 Godot의 2D 및 3D 렌더 설정은 모든 레이어를 볼 수 있도록 되어 있습니다. 따라서 가시성 레이어를 변경해도 크게 영향을 미치지 않습니다.

다음 단계는 게임에서 해당 레이어를 표시하거나 숨기는 방법인 마스크 컬링을 사용하는 방법을 알아보는 것입니다. 이번에는 2D 및 3D 워크플로가 정확히 동일하지는 않습니다!

# 레이어를 사용하여 마스크 컬링 노드

그래서, 게임에서 가시성 레이어에 따라 조건부로 노드를 렌더링하는 것은 조금 복잡할 수 있습니다. 특히 이번에는 2D와 3D에서 동일하게 작동하지 않기 때문입니다.

<div class="content-ad"></div>

참고: 이는 Godot의 개발 방식에서 온 것인데, 먼저 2D 전용 렌더 엔진이 있었고, 그 후 몇 년 후에 새로운 3D 렌더 엔진이 나왔습니다(실제로 3D가 있긴 했지만 너무 사용하기 어려워서 누구나 2D만 사용했죠).

그래서 실제로 2D 노드 및 3D 노드를 관리하기 위해 두 개의 별도 탭이 필요하며, 각 엔진이 조금 더 최적화되어 있을 수도 있지만 때로는 사용 방법에 약간의 불일치가 있을 수도 있습니다. 그래서 요즘처럼 이런 일이 발생하죠.

하지만 어쨌든 — 가장 간단한 3D부터 시작해 봅시다.

## 3D에서 컬 마스크 사용하기

<div class="content-ad"></div>

요렇게 정말 간단한 3D 장면이 있다고 가정해봅시다. 여기에는 Camera3D 노드, 방향 빛, 그리고 흰색 구를 렌더링하는 MeshInstance3D가 있습니다.

또한 이 구가 가시성 레이어 번호 1이 아니라 레이어 번호 2에 있다고 가정해봅시다:

![3D scene](/assets/img/2024-07-14-UsingmaskcullingvisibilitylayersGodot4C_4.png)

이제, 미리보기 모드로 들어가서 카메라가 렌더링하는 것을 확인해 보더라도 우리는 여전히 구를 보게 됩니다.

<div class="content-ad"></div>

카메라 인스펙터 상단에 있는 칼 매스크 속성에서 각 셀이 모두 켜져 있기 때문입니다. 이는 기본적으로 우리 카메라가 모든 레이어를 "볼" 수 있다는 것을 의미합니다.

레이어 번호 두를 숨기려면, 두 번째 셀을 끄기만 하면 됩니다. 그러면 3D 카메라 미리보기에서 우리의 구가 사라진 것을 확인할 수 있어요 :)

그래서 이 칼 매스크 속성은 노드 가시성 레이어와 매우 유사한 표이며, 이는 Godot에게 이 Camera3D 노드가 렌더링을 위해 고려해야 하는 레이어들을 말해줍니다. 꽤 직관적이죠! :)

## 2D에서 칼 매스크 사용하기

<div class="content-ad"></div>

그러나 2D 작업에서는 약간 더 복잡한 문제가 발생합니다. 기본 2D 씬이 몇 개의 스프라이트와 함께 있는 경우 Camera2D 노드를 확인할 때 Cull Mask 속성이 없다는 것을 알 수 있습니다.

2D 작업 시에 Cull Mask 옵션은 실제로 카메라에 아니라 2D 요소가 속한 뷰포트에 사용할 수 있습니다. 실제로 Godot 문서의 뷰포트 API 참조 페이지를 보면 canvas_cull_mask 속성이 여기에 있다는 것을 알 수 있습니다.

그러나 상단으로 스크롤하면 이 뷰포트 노드 유형이 실제로 추상적인 것임을 알 수 있습니다. 이것은 즉, 직접 인스턴스화할 수 없음을 의미합니다.

따라서 Godot 편집기 내에서 Viewport 유형의 새 노드를 만들어 보려고 하면 회색으로 표시되어 생성할 수 없음을 알 수 있습니다.

<div class="content-ad"></div>

한편, 해당 노드의 직접 상속 형식인 SubViewport 노드 유형을 선택할 수 있습니다. 물론 Cull Mask 속성을 가지고 있습니다.

화면 크기에 맞게 SubViewport를 설정하고 씬 환경 설정을 제대로 맞추는 것은 오늘의 주제가 아닙니다. 그래서 이 부분은 무시하고 SubViewportContainer와 SubViewport 노드를 가지고 있다고 가정하겠습니다. 그리고 여기에 모든 2D 요소를 자식으로 넣었다고 가정하겠습니다.

💬 물론, 이에 대해 궁금하거나 이러한 노드를 사용하여 게임에서 분할 화면을 만드는 방법에 대해 알고 싶다면 댓글에서 말씀해주세요! ;)

어쨌든, 여기에서 제가 내 Sprite2D 노드와 Camera2D 노드를 모두 SubViewport 노드 아래에 넣었음을 보실 수 있습니다. 그리고 레이어 번호 1에 하나의 스프라이트를 두고, 다른 하나는 레이어 번호 2에 두었습니다:

<div class="content-ad"></div>

![UsingmaskcullingvisibilitylayersGodot4C_5.png](/assets/img/2024-07-14-UsingmaskcullingvisibilitylayersGodot4C_5.png)

Hey there! Let's dive into a cool feature in Godot. When you tweak the Canvas Cull Mask property on your SubViewport node, did you notice how the sprites magically appear and disappear in the editor viewport? It's because this time, culling is directly tied to the viewport, not just a camera. That's why Godot shows us the live results in the viewport. Pretty neat, right? 😉

Despite being a tad complex, Godot still rocks by letting us leverage visibility layers and mask culling both in 3D and 2D. This way, we can easily split our scene rendering into various parts. Think about creating multiple screens, hiding objects temporarily, adding post-processing to specific sections of the scene, and more! Give it a try!

<div class="content-ad"></div>

# 빠른 팁: 레이어 이름 설정하기

그런데, "Layer 1", "Layer 2"와 같은 표준적인 레이어 이름보다 좀 더 명확한 이름을 사용하고 싶다면, Project Settings 창으로 이동해서, 2D Render 또는 3D Render 섹션 중에 게임 유형에 맞는 섹션을 선택해보세요.

여기에서 다양한 입력란에 구체적인 이름을 입력할 수 있어요.

그러면 나중에 시각 레이어 테이블이나 컬 마스크 테이블로 되돌아올 때, 셀 위로 마우스를 가져가면, 이제 자신만의 명확한 이름이 표시돼 있음을 알게 될 거예요 :)

<div class="content-ad"></div>

오케이, 그리고 이 튜토리얼을 마무리하며, 빠르게 C# 코드를 사용하여 가시성 레이어를 얻거나 설정하는 또 다른 중요한 주제에 대해 얘기해보겠습니다...

# C# 코드를 통한 가시성 레이어 변경

자, 이제 우리가 Inspector 패널을 통해 노드의 가시성 레이어를 확인하고 변경하는 방법을 알았으니, C#에서의 동일한 기능을 살펴보겠습니다.

참고: 여기서 우리는 VisualInstance3D 노드에 중점을 두겠지만, 물론 CanvasItem에 대한 유사한 API를 얻을 수도 있습니다.

<div class="content-ad"></div>

그러면 이제 이전에 보았던 기본 3D 씬의 "Sphere"에 새로운 C# 스크립트, 예를 들어 VisibilityController라고 한다고 가정해 봅시다. _Process() 함수를 제거하고 몇 가지 API 호출을 테스트하기 위해 _Ready() 함수만 사용할 것입니다:

```csharp
using Godot;
using System;

public partial class VisibilityController : MeshInstance3D
{
    public override void _Ready()
    {}
}
```

MeshInstance3D로부터 파생된 노드이므로 VisualInstance3D에서 상속받은 Layers 속성을 쉽게 가져오거나 설정할 수 있다는 것을 알 수 있습니다.

디버그용으로 출력하고 씬을 실행해 보면:

<div class="content-ad"></div>

```csharp
using Godot;
using System;

public partial class VisibilityController : MeshInstance3D
{
    public override void _Ready()
    {
        GD.Print(Layers);
    }
}
```

그런 다음 콘솔에 "2"가 나타납니다. 이것은 레이어 번호 두에 해당하는 것으로 보입니다, 맞나요?

그러나 만약 우리의 구체를 레이어 번호 세 번으로 변경하고 게임을 재시작하면, 이번에는 3이 아니라 4가 출력됩니다!

그렇다면... 왜 그럴까요?


<div class="content-ad"></div>

게임 엔진에서와 마찬가지로, Godot의 마스크 레이어(오늘 학습 중인 가시성 레이어나 물리 레이어)는 비트마스크 시스템에 의존합니다. 이 시스템을 사용하면 레이어를 메모리에 더 효율적으로 저장하고 여러 레이어를 동시에 결합할 수 있습니다.

우리는 레이어의 값이 단순히 레이어의 정확한 번호와 동일하다고 말하는 대신, 이 레이어를 레이어 값에서 활성화 또는 비활성화할 비트의 위치로 사용합니다.

실제로 Layers 테이블에서도 해당 내용을 확인할 수 있습니다. 셀 위로 마우스를 올리면 레이어의 색인 아래에 이 레이어를 제어하는 비트의 위치와 관련 값이 표시됩니다.

만약 여러분이 2의 거듭제곱과 비트마스크에 대해 익숙하지 않다면, 그 핵심은 모든 2의 거듭제곱을 옆에 두고 0부터 번호를 매기는 것입니다. 마지막으로, 여러분의 비트마스크는 엔진에게 어떤 슬롯이 활성화되었고 어떤 것이 비활성화되었는지 알려주는 0과 1의 연속입니다. 왜냐하면 결국 마스크의 값은 마스크에서 활성화된 모든 2의 거듭제곱의 합이기 때문입니다.

<div class="content-ad"></div>

다시 말해, 한 번에 모든 세부 사항을 이해하지 못해도 괜찮아요. 기억해야 할 것은 VisualInstance3D의 Layers 속성에 새로운 값을 할당하려면 레이어 번호를 직접 사용할 수 없다는 점입니다.

```csharp
using Godot;
using System;

public partial class VisibilityController : MeshInstance3D
{
    public override void _Ready()
    {
        Layers = 3; // 레이어 3을 사용하지 말아야 합니다!
    }
}
```

대신, 해당하는 2의 거듭 제곱을 사용하거나, 노드에서 여러 레이어를 활성화하려면 2의 거듭 제곱의 합을 사용해야 합니다.

```csharp
using Godot;
using System;

public partial class VisibilityController : MeshInstance3D
{
    public override void _Ready()
    {
        Layers = 4; // 2^2 = 레이어 3
        Layers = 5; // 2^0 + 2^2 = 레이어 1 + 3
    }
}
```

<div class="content-ad"></div>

그렇다면, Godot은 우리에게 특정 노드의 가시성 레이어를 가져오거나 설정하는 데 사용하기 쉬운 API 세트를 제공해줍니다. 이 API들은 GetLayerMaskValue()와 SetLayerMaskValue()로 잘 알려져 있습니다.

일반적으로, 다음과 같이 몇 가지 출력을 하면:

```csharp
using Godot;
using System;

public partial class VisibilityController : MeshInstance3D
{
    public override void _Ready()
    {
        GD.Print($"레이어 1에 있나요? {GetLayerMaskValue(1)}");
        GD.Print($"레이어 2에 있나요? {GetLayerMaskValue(2)}");
        GD.Print($"레이어 3에 있나요? {GetLayerMaskValue(3)}");

        GD.Print(Layers);
    }
}
```

레이어 1, 2 또는 3에 노드가 있는지에 대한 true 또는 false 부울 값을 먼저 얻은 후, 해당 마스크 값이 하나의 정수로 표시됩니다. 여기서, 우리의 구체는 단지 레이어 번호 2에 있기 때문에, 두 번째 출력만 true를 반환하며, 마스크 값은 2^1 = 2입니다.

<div class="content-ad"></div>

하지만 SetLayerMaskValue()를 사용하여 레이어 번호 2를 비활성화하고 대신에 스피어를 레이어 1과 3에 놓는다면:

```csharp
using Godot;
using System;

public partial class VisibilityController : MeshInstance3D
{
    public override void _Ready()
    {
        SetLayerMaskValue(2, false); // 레이어 2에서 제거
        SetLayerMaskValue(1, true); // 레이어 1에 추가
        SetLayerMaskValue(3, true); // 레이어 3에 추가

        GD.Print($"레이어 1에 있나요? {GetLayerMaskValue(1)}");
        GD.Print($"레이어 2에 있나요? {GetLayerMaskValue(2)}");
        GD.Print($"레이어 3에 있나요? {GetLayerMaskValue(3)}");

        GD.Print(Layers);
    }
}
```

그러면 이제 출력 결과가 이 변경 사항을 반영하고 새 마스크 값은 2⁰ + 2² = 1 + 4 = 5이므로 5가 됩니다:

그리고 덧붙여 말하자면, 런타임 중에 인스펙터에서 가시성 레이어의 상태를 확인하고 싶다면, 가끔은 Godot 편집기로 이동하여 왼쪽 상단의 Scene 독에 가서 Remote 모드로 전환할 수 있다는 것을 잊지 마세요.

<div class="content-ad"></div>

이것은 씬의 런타임 계층 구조와 노드의 런타임 상태를 보여줍니다. 그럼 "Sphere" 객체를 계층에서 간단히 선택하면, 실제로 레이어 1과 3에 있는 것을 확인할 수 있습니다.

# 결론

어쨌든, 이렇게 됩니다: 이제 Godot에서 가시성 레이어를 사용하는 방법과 이러한 레이어를 통해 조건부로 객체를 렌더링하는 방법을 알게 되었습니다.

만약 이 튜토리얼을 즐겼다면, 기사에 박수를 치고 다음 튜토리얼을 놓치지 않도록 팔로우해주시고, 물론 배우고 싶은 Godot 트릭 아이디어를 댓글로 남겨주시기 바랍니다!

<div class="content-ad"></div>

언제나 읽어 주셔서 감사합니다. 건강하세요 :)

더 많은 콘텐츠와 많은 우수 작가들의 글을 보시려면 Medium 회원이 되어보세요! 회원 비용은 직접 읽는 작가들을 지원합니다.