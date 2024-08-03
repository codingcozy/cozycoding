---
title: "2024년에 그리드는 구식이 될까 CSS 트렌드 분석"
description: ""
coverImage: "/assets/img/2024-08-03-CSSEn2024lesGridscesthasbeen_0.png"
date: 2024-08-03 19:23
ogImage: 
  url: /assets/img/2024-08-03-CSSEn2024lesGridscesthasbeen_0.png
tag: Tech
originalTitle: "CSS  En 2024 les Grids cest has been "
link: "https://dev.to/younup/css-en-2024-les-grids-cest-has-been--lmc"
---


## 소개

2024년에는 모두가 반응형 디스플레이가 무엇인지 알고 있습니다. 다양한 화면(스마트폰, 태블릿, 데스크톱 컴퓨터 등)에 대해 최적의 미학적 및 인간공학적 디스플레이를 위해 사용 가능한 공간을 지능적으로 활용하는 것입니다.

그러나 기본 원리로 돌아가서, 오늘날 CSS를 사용하여 어떻게 반응형 디자인을 구현할까요?

이를 위해 CSS는 다음과 같은 주요 도구를 제공했습니다:

<div class="content-ad"></div>

- 그리드(그리고 서브 그리드 😯) !
- 플렉스박스
- 그리고 미디어 쿼리

실제로, 많은 개발자들이 그리드(역사적으로는 옛 것)를 완전히 포기하고 플렉스박스를 선호합니다. 플렉스박스는 매우 유사해 보이지만(조금) 더 현대적입니다.

하지만 : 그렇다면 그리드가 아직 존재하는 이유는 무엇일까요? 우리는 좀 낡은 도구를 보고 있는 것이 아닐까요?

그리드를 플렉스박스와 비교하기 위해, 각각의 기능들에 대해 다시 살펴보는 것을 제안합니다. 언제 어떻게 사용해야 하는지를 포함하여. 그리드가 우리가 생각했던 것 이상으로 제공할 가능성이 있음을 알게 될 것입니다!

<div class="content-ad"></div>

## 플렉스박스

아마도 이미 알고 계시겠지만 Flexbox는 Grid와 비교해서 훨씬 유연한 정렬 시스템을 제공합니다. 하지만 원하는 요소들을 배열할 때 주축 하나에만 집중하도록 특별히 설계되어 있습니다:

```js
<div class="container">
  <div class="column col1">컬럼 1</div>
  <div class="column col2">컬럼 2</div>
  <div class="column col3">컬럼 3</div>
  <div class="column col4">컬럼 4</div>
</div>
```

```js
.container {
  display: flex; /* 플렉스박스 활성화 */
  flex-direction: row; /* 주 축 방향 선택 */
  justify-content: center; /* 주축을 기준으로 요소들 가운데 정렬 */

  width: 100%;
  background-color: #fff9e6;
}

.column {
  background-color: #fbbe00;
  width: 100px;
  height: 50px;
  color: white;
}
```

<div class="content-ad"></div>

다음과 같이 변합니다:

![이미지](/assets/img/2024-08-03-CSSEn2024lesGridscesthasbeen_0.png)

사실, Flexbox는 사전에 열의 수를 정하지 않고도 단일 축에 여러 요소를 정렬하는 데 이상적입니다.

그러나 예를 들어 .container가 더 많은 공간을 차지하는 경우에도 보조 축(우리의 경우에는 수직 🙂)에 대한 정렬을 설정할 수 있습니다:

<div class="content-ad"></div>

```js
.container {
  /* ... */

  height: 100vh; /* 뷰포트 전체 높이 차지 */
  align-items: center; /* 부수축 방향 가운데 정렬 */

  justify-content: space-between; /* (주축 정렬의 다른 예) */

  /* ... */
}
```

![이미지](/assets/img/2024-08-03-CSSEn2024lesGridscesthasbeen_1.png)

...그리고 요소가 한 줄에 모두 맞지 않을 경우 하나 이상의 추가 행을 만들 수 있도록 flex-wrap 속성을 사용할 수 있습니다:

```js
.container {
  /* ... */
  flex-wrap: wrap;
  /* ... */
}
```

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-03-CSSEn2024lesGridscesthasbeen_2.png)

마지막 예제를 수행할 때 4번과 5번 요소에 서로 다른 크기를 의도적으로 적용했음을 주목할 것입니다. 그러면 flexbox가 여러 행에 걸쳐 동일한 열 정렬을 보장하지 않음을 확인할 수 있습니다 😕. Flexbox는 요소들을 서로 뒤에 표시하는 것으로 충분히 동작했습니다. 이는 고려해야 할 제한 사항입니다.

마지막으로 Flexbox에 대한 이 모든 정보를 마치기 위해, 반응형 "사용 가능한 공간을 지능적으로 활용하기" 문제에 대응하기 위해 자식 요소에 놀랄만한 flex 속성을 언급하겠습니다:

```js
.container {
  display: flex;
  flex-direction: row;
  align-items: center;
  height: 100vh;
  gap: 5px; /* (요소 간 간격) */

  width: 100%;
  background-color: #fff9e6;
}

.column {
  background-color: #fbbe00;
  color: white;
}

.col2 {
  flex: 1; /* 공간 차지하는 능력이 작음 */
}

.col3 {
  flex: 2; /* 공간 차지하는 능력이 중간 */
}

.col4 {
  flex: 3; /* 공간 차지하는 능력이 큼 */
}
```

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-03-CSSEn2024lesGridscesthasbeen_3.png)

La propriété flex nous permet donc de simplifier la répartition et le partage de l'espace disponible entre les éléments, ici en suivant un coefficient.

Mais attention à ne pas tout confondre : il s'agit bien ici d'étirer les éléments pour occuper l'espace, et non de leur attribuer une ou plusieurs colonnes ! On ne peut donc pas non plus espérer garantir un alignement, par exemple, entre plusieurs flexboxs sur une même colonne ! ... Ça, c'est le boulot des Grids !😋

## Les Grids


<div class="content-ad"></div>

### 전통적인 사용법

가끔 "잊혀지거나" 다른 CSS의 다양한 발전으로 말씀드렸던 것과 같이, 그리드 시스템은 2D 그리드를 선언하여 자식 요소를 배치할 수 있는 시스템을 의미합니다. 이 그리드의 각 셀은 뷰포트 크기 및/또는 요소 크기에 따라 동적으로 확장됩니다.

그리드 시스템은 예를 들어 다음과 같은 구조를 가진 페이지의 기본 레이아웃을 정의하는 데 이상적입니다:

```js
<body>
  <header>
    <h1>헤더</h1>
  </header>
  <aside>
    <h2>사이드바</h2>
  </aside>
  <main>
    <h2>메인 제목</h2>
    <p>내용 내용 내용...</p>
  </main>
</body>
```

<div class="content-ad"></div>

일반적으로 grid-template-areas를 사용하여 그리드를 생성할 수 있습니다. 이는 상당히 시각적이라고 할 수 있어요:

```js
body {
  display: grid; /* 그리드 활성화 */
  grid-template-areas: /* 그리드 영역 정의 */
    "header header header header"
    "sidebar content content content";

  grid-template-columns: repeat(4, 25%); /* 4개 열의 너비 분배 */
  grid-template-rows: 30% 70%; /* 2개 행의 높이 분배 */

  margin: 0; /* (그리드를 페이지에 적용하기 위한...) */
  height: 100vh; /* (...전체 높이 할당) */
}

header {
  grid-area: header; /* 'header' 영역에 할당 */
  background-color: #fbbe00;
  color: white;
}

aside {
  grid-area: sidebar; /* 'sidebar' 영역에 할당 */
  background-color: #fff9e6;
}

main {
  grid-area: content; /* 'content' 영역에 할당 */
  background-color: white;
}
```

<img src="/assets/img/2024-08-03-CSSEn2024lesGridscesthasbeen_4.png" />

그래서 grid-template-areas 속성을 사용하여 열과 행의 그룹을 정의하고 명시적 이름을 붙일 수 있어요(가급적이면 명료하게요! 😊). 그런 다음 자식 요소는 grid-area 속성을 통해 이러한 이름이 지정된 영역에 할당될 수 있어요.

<div class="content-ad"></div>

### 자동 흐름

Grid 시스템이 너무 "엄격"하다고 느낀다면 🤖, 요소의 수와 관계없이 레이아웃을 자동으로 구성할 수도 있습니다. 이는 flexbox와 비슷한 개념입니다:

```js
body {
  display: grid; /* 그리드 활성화 */

  grid-auto-flow: column; /* <== 컬럼에 대한 자동 분배 */

  margin: 0; /* (그리드를 확장하기 위해... */
  height: 100vh; /* ...페이지 전체에 적용합니다) */
}
```

<img src="/assets/img/2024-08-03-CSSEn2024lesGridscesthasbeen_5.png" />

<div class="content-ad"></div>

그러나 주의할 점은 예를 들어 grid-template을 사용하여 열과 행의 크기를 지정하면 그리드가 요소들의 나타나는 순서를 변경할 수 있음을 언급해야 합니다. 이는 템플릿의 셀에 먼저 들어갈 수 있는 요소를 선호하는 것이 목적입니다. 이는 경험이 적은 사용자들에게는 다소 혼란스러울 수 있습니다.

기억하세요: 그리드는 2차원 배치를 다룹니다! 플렉스박스와는 달리 주요 축이 없습니다!

### 하위 그리드

지난 몇 년간 주목받고 있는 속성! (cf State of CSS 2023).

<div class="content-ad"></div>

가정해 봅시다. 사이트의 기본 열을 정의했다고 해보죠 (방금 우리가 grid-template-areas와 함께 맨 처음에 본 것처럼). 콘텐츠를 계속 추가함으로써 HTML 계층 구조가 커지기 시작했습니다.

그러면 어떻게 하면 부모 사이트의 정의된 열에 맞게 n 번째 하위 자식 태그를 정렬할 수 있을까요, 마치 Material Design 2의 레이아웃처럼요?

여기서 subgrid가 등장합니다! subgrid를 사용하면 직계 부모의 템플릿에 참조할 수 있도록 해줍니다 (따라서 매 단계마다 subgrid를 전달해야 합니다):

```js
main {
  grid-area: content; /* 그리드의 'content' 영역에 할당 */

  display: grid;
  grid-template-columns: subgrid; /* <== 직계 부모의 열 템플릿 상속 */

  background-color: white;
}
```

<div class="content-ad"></div>

부모 열과 완벽하게 정렬된 "content" 부분의 3개 열을 찾을 수 있게 해줍니다. 그것이 최고네요 😎!

![이미지](/assets/img/2024-08-03-CSSEn2024lesGridscesthasbeen_6.png)

(보라색 영역은 정렬을 강조하기 위해 추가되었습니다)

## 결론

<div class="content-ad"></div>

그리드는 어떻게 되었나요?

우리가 볼 수 있었듯이, 그리드가 사용 중지되지 않았다면 그에는 좋은 이유가 있습니다!

그리드는 웹 페이지의 기본 영역을 정의하고 2차원 정렬, 행 간격 또는 열 간격을 보장하는 데 이상적입니다.

한편, 플렉스박스는 단일 축에서 간단하게 정렬하거나 가변 요소의 수에 대한 사용 가능한 공간을 공유하는 데 이상적이며, 이는 두 번째 수준의 기능으로 볼 수 있습니다.

<div class="content-ad"></div>

결국 중요한 것은 레시피를 만들듯이 적당한 비율을 맞추는 것입니다:

- 기초를 위한 그리드
- 페이지의 하위 영역을 위한 몇 가지 서브그리드
- 부가적이고/또는 동적 정렬을 위한 플렉스박스
- ... 그리고 지원하는 디바이스에 맞게 차원과 열 수를 조정하기 위한 미디어 쿼리까지 

좋은 도구로 응담형을 만들기 위한 변명이 필요 없겠네요! 🙂