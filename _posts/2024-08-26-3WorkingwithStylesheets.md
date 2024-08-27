---
title: "웹사이트 CSS 스타일시트 작업 방법"
description: ""
coverImage: "/assets/img/2024-08-26-3WorkingwithStylesheets_0.png"
date: 2024-08-26 17:45
ogImage: 
  url: /assets/img/2024-08-26-3WorkingwithStylesheets_0.png
tag: Tech
originalTitle: "3. Working with Stylesheets"
link: "https://medium.com/@foghorn111/3-working-with-stylesheets-a0613c389f41"
isUpdated: true
updatedAt: 1724744138373
---


이전 기사 끝에서 어디에 있었는지 기억나시나요? 여기 그림이 있어요:

![Image 0](/assets/img/2024-08-26-3WorkingwithStylesheets_0.png)

이전에 보여드렸던 실제 프로토타입과 훨씬 다른 것 같죠? 여기 우리가 향하고 있는 방향의 그림입니다.

![Image 1](/assets/img/2024-08-26-3WorkingwithStylesheets_1.png)

<div class="content-ad"></div>

# 그래서 우리가 그림 1에서 그림 2로 가려면 어떻게 해야 하나요?

우리는 CSS(Cascading Stylesheet)를 사용해서 화면에 HTML에서 묘사하는 방법을 알려줄 거에요. 아마도 CSS가 뭔지 궁금해하실 거라 생각해서, 그냥 답부터 드릴게요. 그냥 또 하나의 코드 종류로, 웹 브라우저가 사용하는 방법을 안다고요. 현재 브라우저 회사들이 더 많은 스타일링 기능을 추가하려고 하면서 급속한 발전을 맛보이고 있어요. 새로운 기능이 구현될 때마다 이 문서를 최신 상태로 업데이트할게요.

## 코드에 Cascading Stylesheet를 어떻게 만들까요?

우리는 이번 시리즈의 시작점에 있기 때문에 우리 오래된 친구 "메모장.exe"를 방문할 거에요. 그 전에, 하지만 코드가 어디에 있는지 살펴볼까요?

<div class="content-ad"></div>

페이지를 논리적으로 그룹화하는 경향이 있어요. 전문 도구를 사용하면 더 쉽게 할 수 있지만, 지금은 그냥 새 폴더를 만들어서 유용한 이름을 지어주세요. (저는 폴더 이름을 "css"로 지었습니다. 이유는 명백하죠.) 그림 3을 참고해주세요.

![Working with Stylesheets](/assets/img/2024-08-26-3WorkingwithStylesheets_2.png)

이제 이 css 폴더 안에 새 파일을 저장해주세요. 파일 확장자로 ".css"를 꼭 붙여주세요. 그렇게 하면 웹 브라우저가 파일을 어떤 형식으로 처리해야 하는지 알 수 있어요. 이상한 형태의 파일을 만드는 또 다른 힌트입니다.

- 메모장에서 새 파일을 열어주세요
- 파일 메뉴를 클릭한 후 "다른 이름으로 저장"을 클릭해주세요
- "저장 유형" 드롭다운 메뉴를 모든 파일 (*.*)로 설정해주세요
- 스타일시트에 유용한 이름을 지어주세요. 그 이름의 끝에 ".css"가 붙어야 해요.

<div class="content-ad"></div>

<img src="/assets/img/2024-08-26-3WorkingwithStylesheets_3.png" />

# 브라우저가 스타일을 인식하게 하기

우리는 HTML 파일에 새로운 줄을 추가할 것입니다. 아래 코드에서 우리 웹 페이지 파일에서 6번째 줄에 있습니다. (내 파일명은 "workout prototype.html"입니다)

```js
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Workout Prototype</title>
    <link rel="stylesheet" href="css/prototype.css" />
</head>
<body>
    <div>2:43</div>
    <div>
        <button type="button" id="workoutButton">운동 시작</button>
        <button type="button" id="warmupButton">워밍업</button>
    </div>
...
```

<div class="content-ad"></div>

이것은 연관된 줄입니다:

```js
[스타일 시트](css/prototype.css)를 링크합니다.
```

이 줄이 의미하는 바는 다음과 같습니다:

- 웹페이지에 파일을 포함시킵니다. (`link /`)
- 해당 파일은 스타일 시트입니다 (rel="stylesheet")
- 해당 파일은 내 파일 근처의 폴더에 위치해 있습니다 (href="css/...")
- 해당 파일의 이름은 "prototype.css"입니다 (href="css/prototype.css")

<div class="content-ad"></div>

정말 강력한 작은 코드 줄이네요. 웹 브라우저는 제가 믿기 어려울 정도로 더 강력하게 만들어 줘요.

작업을 저장하시고 Cascading Stylesheet (CSS) 코드가 어떻게 작동하는지에 대해 조금 이야기해볼게요.

# CSS의 코딩 구조

Cascading Stylesheets는 기본적으로 규칙으로 이루어져 있고, 이러한 규칙에 대한 특별한 표기법이 있어요. 다음과 같아요:

<div class="content-ad"></div>

```js
<selector> {
  <attribute>: <value>;
}
```

## 세 가지 키: selector, attribute, value…

Selector란 말 그대로 웹페이지의 일부를 선택하는 코드 줄입니다. 예를 들어 selector를 사용하여 요소 유형을 선택할 수 있습니다.

body 태그 selector를 사용해봅시다. CSS 파일을 열고 다음 코드를 넣어보세요.

<div class="content-ad"></div>

```js
body {
  background-color: blue;
}
```

방금 무엇을 했는지 알아보았습니다. 웹 브라우저에게 문서의 본문을 파란색으로 만들라고 지시했습니다. 결과는 이렇습니다:

![이미지](/assets/img/2024-08-26-3WorkingwithStylesheets_4.png)

## 방금 쓴 CSS 코드에 대해 멋진 점 두 가지를 알아보겠습니다:

<div class="content-ad"></div>

- 우리는 3개의 특정 버튼 또는 다른 HTML 태그(element) 조합을 선택할 수 있는 멋진 선택기를 만들 수 있어요.
- 해당되는 경우 HTML 태그(element)의 모든 속성을 설정할 수 있고, 필요한 경우 떠올려서 만들어낼 수도 있어요.

위의 #1 예시에서 4번째 버튼만 선택하는 멋진 선택기를 살펴보겠어요. 나중에 더 쉬운 예시를 볼 거예요. 지금은 그 전에, 이전에 저장한 내용 위에 이 내용을 붙여넣기만 하면 돼요.

```js
body {
  background-color: white;
}
div > button:nth-of-type(4) {
  border-color: blue;
  border-width: 1px;
  border-style: solid;
  background-color: transparent;
  border-radius: 50%;
  color: blue;
  padding-block: .5rem;
  padding-inline: .75rem;
}
```

그리고 방금 한 작업의 사진이에요:

<div class="content-ad"></div>


<img src="/assets/img/2024-08-26-3WorkingwithStylesheets_5.png" />

방금 우리가 한 작업을 정리해 봅시다:

- body 'background-color: white;' — body의 배경이 다시 흰색으로 변경됩니다.
- div ` button:nth-of-type(4) — div 내부에 있는 4번째 버튼에 다음 스타일 규칙이 적용됩니다:

a. border-color: blue; — 테두리 색상 = 파란색


<div class="content-ad"></div>


b. border-width: 1px; — 픽셀 단위로 테두리 너비를 1로 설정합니다.

c. border-style: solid; — "Solid"(실선) 테두리 스타일을 설정합니다.

d. background-color: transparent; — 투명한 배경 색상을 설정합니다.

e. border-radius: 50%; — 테두리의 각 모퉁이를 50%의 호(arc)로 둥글게 만듭니다(높이와 너비의 50%에 해당).


<div class="content-ad"></div>

f. color: blue; — 버튼 안의 텍스트는 파란색이어야 합니다.

g. padding-block: .5rem; — 버튼 텍스트 위아래에 약간의 공간을 만듭니다 (숫자 "0"의 반 정도의 너비)

h. padding-inline: .75rem; — 버튼 텍스트 왼쪽과 오른쪽에 약간의 공간을 만듭니다 (숫자 "0"의 3/4 정도의 너비)

# 시작할 시간이 있습니다. 준비되셨나요?...

<div class="content-ad"></div>

우리 페이지에서 타이머를 포함하는 부분을 더 쉽게 선택할 수 있도록 만들어 보겠습니다:

HTML 파일에서 다음 줄을 변경하고 저장하세요.

```js
<div class="information rest-timer">2:43</div>
```

그런 다음 prototype.css로 이동하여 거기에 있는 모든 텍스트를 교체하세요. (이번이 마지막으로 처음부터 다시 시작할게요, 약속해요.)

<div class="content-ad"></div>

```js
.rest-timer {
  font-size: 10rem;
  text-align: center;
  color: hsla(232, 77%, 58%, 1);
}
```

방금 무엇을 했나요?

- div 태그에 몇 가지 클래스를 추가했습니다. (information & rest-timer). 나중에 유용하게 사용될 것입니다.
- .css 파일에서 클래스 이름이 "rest-timer"인 어떤 태그(요소)를 잡는 선택기를 사용했습니다. 클래스 이름으로 요소를 가져오려면 마침표(.)를 사용하고 클래스 이름을 씁니다. ".rest-timer"입니다.
- rest 타이머 요소 내의 텍스트는 브라우저의 기본 폰트 크기 (rem)의 10배여야 한다고 말했습니다. 10rem은 "0" 숫자의 10배 크기를 의미합니다.
- .rest-timer 클래스의 텍스트가 화면 가운데 정렬되어야 한다고 말했습니다.
- 함수를 사용하여 텍스트 색상을 설정했습니다. 함수 이름은 hsla이고 "색조, 채도, 밝기 및 알파(불투명도)"를 나타내는 것입니다. 거의 볼 수 있는 모든 색상을 표현하는 멋진 방법입니다.

## HSLA


<div class="content-ad"></div>

색조(Hue): 모든 기본 색상을 원 모양으로 배치하면 나침반에서 방향을 선택하는 것처럼 움직일 수 있어요. 우리는 북쪽에서 232도를 향해 돌아서라고 했어요. 그것은 파란색이에요.

채도(Saturation): 얼마나 많은 파란색을 사용해야 할까요? 모든 픽셀을 파란색으로 만들어야 할까요 아니면 일부만 파란색으로 바꿀까요? 우리는 픽셀 중 77%를 파란색으로 만들기로 했어요.

밝기(Lightness): 파란색을 얼마나 밝게 만들어야 할까요? 범위는 0%(가장 어둡게)부터 100%(흰색)까지에요. 모든 색상의 밝기를 높이면 결국 흰색이 됩니다. 우리는 진짜 검은색으로부터 파란색을 58% 밝게 해보기로 했어요.

알파(Alpha): 파란색은 얼마나 투명해야 할까요? 내 문자들을 얼마나 볼 수 있어야 할까요. 0이면 문자를 전혀 볼 수 없어요. 1이면 문자를 통과해서 볼 수 있어요.

<div class="content-ad"></div>

## 지금 우리 화면은 어떻게 보이나요?

![Screen](/assets/img/2024-08-26-3WorkingwithStylesheets_6.png)

# '운동 및 워밍업' 버튼 섹션

## HTML에 다른 클래스를 추가해 봅시다

<div class="content-ad"></div>

해당 버튼을 담고 있는 div를 수정해 주세요. 이제 다음과 같이 보이게 변경해 주세요:

```js
<div class="work-warm">
    <button type="button" id="workoutButton">Work Out</button>
    <button type="button" id="warmupButton">Warm Up</button>
</div>
```

## Flexbox 스타일과 display 속성.

우리는 이전의 css 프로그래밍 속성인 FlexBox를 사용할 것입니다. (보통 그리드를 사용하지만, 여기서는 약간 앞서서 진행하고 있어요.) 모든 요소에는 컨테이너 유형의 속성이 있습니다. DIV(division) 태그는 컨테이너 태그입니다. 다른 태그들을 담는 역할을 합니다. 그래서 DIV 태그에는 "display" 속성이 있습니다. 이 "display"의 기본 값은 항상 "block"입니다. 그런데 이는 꽤 제한적이에요.

<div class="content-ad"></div>

```js
.work-warm {
  display: flex;
  justify-content: center;
}

.work-warm > * {
  margin-inline: 2rem;
}
```

이 코드는 "Work Out" 및 "Warm Up" 버튼에 대해 어떤 작용을 하는 건가요? 아래 사진을 참고해주세요:

![Work Out and Warm Up Buttons](/assets/img/2024-08-26-3WorkingwithStylesheets_7.png)

# 가능한 가장 쉽게 버튼 스타일을 설정해 보겠습니다.

<div class="content-ad"></div>

요 구현에서는 간단한 CSS 선택자가 필요해요. 나중에 각 운동에서 4번째 버튼을 재정의할 거예요. 이번에는 클래스가 필요하지 않아요. 우리는 그냥 prototype.css 파일 맨 아래에 이 CSS 코드를 추가하면 돼요.

```js
button {
    background-color: hsla(232, 77%, 50%, 1);
    border: none;
    padding-block: .5rem;
    padding-inline: .75rem;
    color: hsla(232, 77%, 100%, 1);
    border-radius: 50%;
}

.work-warm > button {
    border-radius: 5px;
}
```

## 지금 우리 화면은 어디에요?

<img src="/assets/img/2024-08-26-3WorkingwithStylesheets_8.png" />

<div class="content-ad"></div>

# 파워하우스 레이아웃... display: grid

우리는 앞서 플렉스박스가 워크아웃과 워마합 버튼에서 멋진 작업을 했던 걸 봤어요. 음 CSS 그리드 레이아웃은 그만의 섬세함과 정밀함이 있어요. 정확히 더 나은 건 아니지만, 저는 이것을 사용하는 게 훨씬 편한 것 같아요. 이는 레이아웃에 테이블을 사용할 때의 원본 아이디어만큼 정확하지만 훨씬 간단한 방법으로 구현되었어요.

그러니 HTML을 다시 변경해볼까요? 운동 목록에 몇 가지 클래스를 추가하고 그들이 어떻게 작용하는지 볼 거에요.

```js
<div class="sets">
    <div class="exercise">스쿼트</div>
    <div class="summary">피라미드 세트 12-8 @ 45#-55#</div>
    <div class="button-group">
        <button type="button">12</button>
        <button type="button">10</button>
        <button type="button">8</button>
        <button type="button">+</button>
    </div>
    <div class="exercise">스탠딩 바벨 종아리 운동</div>
    <div class="summary">직선 세트 10 @ 45#</div>
    <div class="button-group">
        <button type="button">10</button>
        <button type="button">10</button>
        <button type="button">10</button>
        <button type="button">+</button>
    </div>
    <div class="exercise">윗몸 일으키기</div>
    <div class="summary">직선 세트 10회씩</div>
    <div class="button-group">
        <button type="button">10</button>
        <button type="button">10</button>
        <button type="button">10</button>
        <button type="button">+</button>
    </div>
</div>
```

<div class="content-ad"></div>

## 새로운 클래스들

우리의 최고 "div" 태그는 "sets"라는 클래스를 받게 됩니다. 이것이 그리드로 변환될 것입니다.

반복되는 세 가지 더 클래스가 있습니다:

- Exercise
- Summary
- button-group

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경해 주세요.

<div class="content-ad"></div>

## 이제 각 줄의 CSS 코드가 무슨 역할을 하는지 알아봅시다.

- display: grid — 이 코드는 브라우저에게 표와 비슷한 레이아웃을 예상하도록 알려줍니다.
- grid-template-columns: 7fr 5fr; — 이 코드는 이 섹션이 두 개의 열로 나뉘며, 왼쪽 열이 오른쪽 열보다 약간 더 크다는 것을 나타냅니다. "fr"은 "fractional unit"의 약어로, 이 코드는 "sets"라는 클래스가 두 개의 열을 가지고 있으며, 12개의 동일한 열을 두 개로 나누도록 지시합니다. 왼쪽 열은 이 중 7개를, 오른쪽 열은 이 중 5개를 차지하게 됩니다.
- grid-column: 1 / span 2; — 이 코드는 "button-group" 클래스를 가진 "div" 태그가 1열에서 시작하고 두 개의 열을 모두 커버하도록 지시합니다. (정말 멋지죠?)
- text-align: center; — 이것은 게으름을 부리기 위해 사용했습니다. justify 속성으로도 할 수 있었지만, text-align을 더 빨리 기억했어요. (그리고 작동했어요... 항상 긍정적인 일이죠.) 곧 justify/align 속성을 소개할게요.

이 코드로 놀아보세요. 내 방법이 정답이고 유일한 방법은 아니에요. 제 방법은 꽤 짧은 방법이지만, 제가 궁리할 수 있는만큼 똑똑한 수준입니다.

# 이제 메뉴도 그리드로 만들어봅시다.

<div class="content-ad"></div>

```js
<div class="main-menu">
    <img src="images/home.svg" />
    <img src="images/program.svg" />
    <img src="images/history.svg" />
    <img src="images/reports.svg" />
    <img src="images/settings.svg" />
    <p>Home</p>
    <p>Program</p>
    <p>History</p>
    <p>Reports</p>
    <p>Settings</p>
</div>
```

여기 스타일 규칙들이 있습니다.

```js
.main-menu {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    justify-items: center;
    align-content: center;
}
```

패턴을 보고 계십니까?

<div class="content-ad"></div>

다음은 수직 스타일링과 글꼴 크기에 대해 다룰 예정이에요.

내 GitHub인 "ElderCodeDragon"에서 코드를 비교해보는 것은 언제든지 환영해요.