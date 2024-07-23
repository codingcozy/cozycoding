---
title: "Unity3D를 사용하여 데드 존 구현 및 객체를 따라가면서 카메라 위치 조정하는 방법"
description: ""
coverImage: "/assets/img/2024-07-02-HowtoimplementadeadzoneandadjustthecameraspositionwhilefollowinganobjectusingUnity3D_0.png"
date: 2024-07-02 23:26
ogImage: 
  url: /assets/img/2024-07-02-HowtoimplementadeadzoneandadjustthecameraspositionwhilefollowinganobjectusingUnity3D_0.png
tag: Tech
originalTitle: "How to implement a dead zone and adjust the cameras position while following an object using Unity3D"
link: "https://medium.com/@joemoceri/how-to-implement-a-dead-zone-and-adjust-the-cameras-position-while-following-an-object-using-91a76eca940f"
---


소개

Unity3D 및 PixelPerfectCamera를 사용하면 약간의 노력을 기울이면 데드 존을 달성하고 카메라가 어디에 어떻게 움직이고 배치될지를 지정할 수 있습니다.

설정

Unity 버전 2020.3.2f1

<div class="content-ad"></div>

Pixel Perfect Camera 1.47

Code

The project we're using can be found [here](https://github.com/joemoceri/cave-adventure-demo) which is using the above versions.

<div class="content-ad"></div>

![Camera Operations Gif](https://miro.medium.com/v2/resize:fit:1400/1*5pV4WogmM77woCQ5Pr620g.gif)

위의 GIF는 카메라 작업이 어떻게 작동하는지 보여줍니다. 카메라는 먼저 데드 존을 허용하며 시작합니다. 데드 존이란 화면에서 카메라가 따라다니지 않는 영역으로, 좌우 경계에 도달할 때까지는 따라오지 않다가 경계에 도달하면 따라다니지만 경계에 머무릅니다. 이러한 경계는 플레이어의 위치가 아닌 화면을 기준으로 하며, 플레이어 주변을 충분히 볼 수 있는 공간을 제공하는 것이 목적입니다.

캐릭터가 환경을 이동할 때, 카메라는 특정한 표시 공간을 제공하기 위해 위치를 조정합니다. 특정 영역을 숨기거나 플레이어의 주의를 중점적으로 둔 특정 영역에 초점을 맞추는 데 유용합니다. 이를 달성하기 위해 PixelPerfectCamera와 일부 카메라 오버라이드 및 논리를 사용할 것입니다.

코드

<div class="content-ad"></div>

플레이어를 따라가는 카메라는 아래 코드처럼 FixedUpdate 안에서 물리 연산과 함께 실행됩니다.

```js
public void FollowTarget(CaveGuyModel model)
{
    // orthographic size를 원하는 크기로 설정합니다
    Cam.orthographicSize = (Screen.height / 100f) / 4f;

    var x = CameraTransform.position.x;
    var y = CameraTransform.position.y;

    var offsetX = (Offset.x * Mathf.Sign(model.CaveGuyTransform.localScale.x)); // scale은 방향을 결정하며, 부호에 따라 암묵적인 데드존을 생성합니다

    // 플레이어의 뷰포트 위치 가져오기
    var caveGuyViewportPosition = Camera.main.WorldToViewportPoint(model.CaveGuyTransform.position);

    // x 오프셋을 고려한 업데이트된 x 위치 가져오기
    var newX = Mathf.MoveTowards(CameraTransform.position.x, model.CaveGuyTransform.position.x + offsetX, SmoothTimeX);
    
    // 만약 데드존 안에 없다면, 데드존을 확인합니다
    if (!FollowTargetX)
    {
        // 플레이어를 화면의 중간 50% 내에 유지
        if(caveGuyViewportPosition.x <= 0.25f || model.CaveGuyTransform.position.x < LastCaveGuyXPositionLeft) // 동일 방향 변경마다 스냅되지 않도록 equals는 피합니다
        {
            FollowTargetX = true;
        }
        else if(caveGuyViewportPosition.x >= 0.75f || model.CaveGuyTransform.position.x > LastCaveGuyXPositionRight) // 위와 동일
        {
            FollowTargetX = true;
        }
    }
    
    // 데드존 경계에 있다면, x를 업데이트합니다
    if (FollowTargetX) // 새로운 x 변환을 적용합니다
    {
        x = newX;
    }

    // 카메라 오버라이드가 Y 좌표를 조정해야 하는지 확인합니다
    if (FollowTargetY) // 새로운 y 변환을 적용합니다
    {
        y = Mathf.MoveTowards(CameraTransform.position.y, NewY, SmoothTimeY);
    }


    CameraTransform.position = new Vector3(x, y, CameraTransform.position.z); // 마지막으로, 카메라 위치를 업데이트합니다
}
```

플레이어가 자유롭게 이동할 수 있는 화면의 중간 50%를 데드존으로 처리합니다. 경계에 닿으면 카메라가 따라가도록 변경됩니다. Y 좌표 조정도 동일하게 처리합니다.

위와 같은 클래스로 카메라 오버라이드 트리거를 사용하여 카메라의 동작을 수정합니다.

<div class="content-ad"></div>

```js
namespace CaveAdventure.CameraOverride
{
    using CaveGuy;
    using UnityEngine;

    public class CameraOverrideEventListener : MonoBehaviour
    {
        // 다른 개체에 상대적인 카메라의 Y 위치를 조정합니다.
        public bool SetCameraOffsetYRelativeToObject = true;

        // 수동 x 오프셋 설정
        public bool SetCameraOffsetX;

        // 수동 y 오프셋 설정
        public bool SetCameraOffsetY;
        
        // 사용할 오프셋 값
        public Vector2 NewOffset;

        // 사용할 상대 개체의 y 위치
        public GameObject RelativeObject;

        // 어떤 것이 더 나은지 확신할 수 없습니다. 남아있을지 또는 들어갈지는 나중에 결정하겠습니다.
        void OnTriggerStay2D(Collider2D collider)
        {
            var bll = collider.GetComponentInParent<CaveGuyBll>();
            if (bll != null)
            {
                if (SetCameraOffsetYRelativeToObject)
                    bll.SetYPosition(RelativeObject.transform.position);

                if (SetCameraOffsetX)
                    bll.SetCameraOffset(NewOffset.x, null);

                if (SetCameraOffsetY)
                    bll.SetCameraOffset(null, NewOffset.y);
            }
        }
    }
}
```

```js
// bll 
public void SetCameraOffset(float? x, float? y)
{
    CameraService.SetCameraOffset(x, y);
}

public void SetYPosition(Vector2? relativeTo)
{
    CameraService.SetYPosition(Model, relativeTo);
}

public void ChangeXDirection(float xAxis)
{
    if (xAxis != 0) // 멈추었을 때 방향 유지
    {
        var scale = Model.CaveGuyTransform.localScale;
        Model.CaveGuyTransform.localScale = new Vector3(Mathf.Abs(scale.x) * Mathf.Sign(xAxis), scale.y, scale.z); // 스프라이트 뒤집기
        if (Mathf.Sign(xAxis) != Mathf.Sign(scale.x)) // x 방향을 변경할 때 카메라 데드존 활성화
        {
            CameraService.ActivateDeadzoneX(Model);
        }
    }
}
```

```js
// camera service
public void SetCameraOffset(float? x, float? y)
{
    Offset = new Vector2(x ?? Offset.x, y ?? Offset.y);
}

public void SetYPosition(CaveGuyModel model, Vector2? relativeTo)
{
    NewY = Offset.y;

    // relativeTo가 있는 경우 사용
    if (relativeTo != null)
    {
        NewY += relativeTo.Value.y;
    }
    
    // 실행 중인 경우 코루틴 중지
    if(UnfollowYCouroutine != null)
        StopCoroutine(UnfollowYCouroutine);

    // 원하는 y 위치에 도달할 때까지 팔로우를 시작
    UnfollowYCouroutine = StartCoroutine(UnfollowYAfterReset());
}

private IEnumerator UnfollowYAfterReset()
{
    // 고정 업데이트를 기다림
    yield return new WaitForFixedUpdate();
    
    // 카메라 Y가 변경될 수 있도록 플래그 설정
    while(CameraTransform.position.y != NewY)
    {
        FollowTargetY = true;
        yield return null;
    }
    FollowTargetY = false;
}

public void ActivateDeadzoneX(CaveGuyModel model)
{
    FollowTargetX = false;
    if ((int)Mathf.Sign(model.CaveGuyTransform.localScale.x) == 1)
        LastCaveGuyXPositionLeft = model.CaveGuyTransform.position.x;
    else
        LastCaveGuyXPositionRight = model.CaveGuyTransform.position.x;
}
```

위에 있는 트리거는 편집기에서 다음과 같이 설정되어 있습니다.


<div class="content-ad"></div>

![How to Implement a Dead Zone and Adjust the Camera's Position While Following an Object Using Unity3D](/assets/img/2024-07-02-HowtoimplementadeadzoneandadjustthecameraspositionwhilefollowinganobjectusingUnity3D_0.png)

환경을 걸을 때 설정에 따라 위치가 조정됩니다.

X 오프셋을 변경하면 데드 존 동작이 변경되어 시작 부분(맨 왼쪽)이나 끝 부분(맨 오른쪽)에 위치하게 됩니다. Y 오프셋은 화면을 위쪽이나 아래쪽으로 더 볼 수 있게 해줍니다.

방향을 바꿀 때 데드 존이 활성화되어 카메라가 X 축에서 플레이어를 더이상 따라가지 않을 것입니다.

<div class="content-ad"></div>

GitHub 저장소

위 코드의 전체 프로젝트와 사용 예제를 확인하려면 GitHub 저장소를 확인해보세요.

Pixel Perfect Camera

[Pixel Perfect Camera](https://forum.unity.com/threads/released-pixel-perfect-camera.416141/)

<div class="content-ad"></div>

브라우저에서 WebGL을 사용하여 이것을 플레이해보세요!

[https://joemoceri.github.io/cave-adventure-demo/](https://joemoceri.github.io/cave-adventure-demo/)