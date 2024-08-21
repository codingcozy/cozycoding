---
title: "나는 움직이는 것을 좋아해, 움직이는 것을 파트 1"
description: ""
coverImage: "/assets/img/2024-07-27-ILiketoMoveItMoveItPart1_0.png"
date: 2024-07-27 13:42
ogImage: 
  url: /assets/img/2024-07-27-ILiketoMoveItMoveItPart1_0.png
tag: Tech
originalTitle: "I Like to Move It, Move It Part 1"
link: "https://dev.to/nmiller15/i-like-to-move-it-move-it-part-1-3b10"
isUpdated: true
---




## CSS Display와 Positioning

![이미지](/assets/img/2024-07-27-ILiketoMoveItMoveItPart1_0.png)

이번 주에는 HTML과 CSS 시리즈에서 직접 코딩해 볼 차례입니다! 코드 작성하면서 배워 보세요! 이 코드펜 템플릿을 사용하여 따라해 보세요.

계정을 설정하고 이 템플릿을 복사한 후에는 이를 익숙해지게 하세요. HTML 요소는 어떻게 구조화되어 있나요? 사용된 익숙한 CSS 속성이 있나요? 편안해지면 수업을 진행하세요.

<div class="content-ad"></div>

## 문서 흐름

뭔가를 옮기기 전에, 프레임 안의 네 개의 정사각형을 살펴보세요. 프레임의 왼쪽 가장자리까지 서로 위에 쌓이도록 배치되어 있는 것을 알 수 있습니다.

이것은 스타일을 추가하지 않았을 때 문서 흐름에서 항목이 어떻게 나타나는지 보여줍니다. 브라우저는 좌측 상단부터 시작해서 항목을 아래로 쌓아 올립니다.

왜 이 블록들이 나란히 있지 않을까요? 그 이유는 이 블록들의 기본 표시 값이 block이기 때문입니다. 즉, 전체 너비를 차지한다는 뜻입니다. 따라서 빨간 정사각형의 콘텐츠 상자 너비는 25px이지만, 해당 컨테이너는 화면의 전체 너비를 차지합니다.

<div class="content-ad"></div>

기본 동작을 알아야 합니다. 요소가 어디에 "원하느냐"로 생각해보세요. 이제 우리의 역할은 그들을 우리가 원하는 위치로 유도하는 것입니다!

## 위치 지정

문서 흐름에서 항목들을 어떻게 이동시키나요? position 속성에 대해 이야기해봅시다. .red-box 클래스에 다음을 추가하세요:

```js
position: static;
```

<div class="content-ad"></div>

기다려! 아무것도 바뀌지 않았네요... 맞아요! static이 기본 값이에요. 다른 값들을 살펴봐요.

### Relative

position 값을 relative로 변경해보세요:

```js
position: relative;
```

<div class="content-ad"></div>

아직 변경이 안되었습니까? 다음을 추가해보세요:

```js
top: 100px;
```

갑자기, 우리의 빨간 블록이 100px 아래로 점프했습니다!

![](/assets/img/2024-07-27-ILiketoMoveItMoveItPart1_1.png)

<div class="content-ad"></div>

웹 브라우저는 상대 위치 지정을 사용할 때 객체를 기본 흐름에서의 위치를 기준으로 배치합니다.

### 절대 위치

추가된 속성을 제거하고 위치를 절대로 변경하세요:

```js
position: absolute;
```

<div class="content-ad"></div>

잠시만 기다려봐요… 파란 블록이 사라졌어요!

![image](/assets/img/2024-07-27-ILiketoMoveItMoveItPart1_2.png)

아니길래요? 빨간 블록을 절대 위치로 변경했더니, 문서의 일반적인 흐름에서 벗어났어요! 파란 블록은 여전히 있어요, 빨간 블록 아래에 있어요.

### 수정됨

<div class="content-ad"></div>

```js
position: fixed;
```

요소는 이제 문서 흐름에서 제거되고 뷰포트에서 고정되며 스크롤을 무시합니다.

블록이 프레임에 따라 위치하도록 되어 있지만, 값 할당을 시작하면 뷰포트에 대해 상대적으로 배치됩니다.

<div class="content-ad"></div>

### Sticky

고정된 속성의 특별한 변형 중 하나는 sticky입니다. 이것은 요소가 일반 흐름에서 위치하다가 해당 컨테이너가 지나가면 화면에 고정됩니다.

다음 코드를 .frame에서 주석 해제하여 동작을 확인하세요:

```js
height: 75px;
overflow: scroll;
```

<div class="content-ad"></div>

```css
.red-box {
  position: sticky;
  top: 0;
}
```

위로 스크롤해서 그것이 붙는 것을 확인해보세요!

<img src="/assets/img/2024-07-27-ILiketoMoveItMoveItPart1_3.png" />


<div class="content-ad"></div>

## 계속됩니다

오늘은 여기까지에요! 이번 주에 이 위치 지정 속성들과 계속 놀아보세요. 다음 주에는 display 속성에 대해 자세히 알아볼 거에요!