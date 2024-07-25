---
title: "초보자를 위한 HTML 기초 이해하기 1"
description: ""
coverImage: "/assets/img/2024-07-25-HTMLforBeginnersUnderstandingtheBasics1_0.png"
date: 2024-07-25 11:47
ogImage: 
  url: /assets/img/2024-07-25-HTMLforBeginnersUnderstandingtheBasics1_0.png
tag: Tech
originalTitle: "HTML for Beginners Understanding the Basics 1"
link: "https://medium.com/@tomas-svojanovsky/html-for-beginners-understanding-the-basics-1-ec6ef72505e1"
---


<img src="/assets/img/2024-07-25-HTMLforBeginnersUnderstandingtheBasics1_0.png" />

## HTML은 프로그래밍 언어가 아닙니다

HTML에서는 태그를 사용합니다. 우리는 원하는 어떤 태그든 사용할 수 있지만, 그것들은 아무 의미가 없을 수 있습니다. 언어에서  "개(dog)"라고 말하면, 모두가 네 다리, 꼬리 등을 떠올립니다. "블루블리(blublebli)"라고 말하면, 무언가를 말했지만, 아무 의미가 없습니다.

예를 들어, 어떻게 보이는지 살펴보겠습니다.

<div class="content-ad"></div>


<!-- html 태그 -->
<div>Hello from HTML</div>

<!-- 여전히 html 태그이지만, 브라우저는 우리가 무엇을 의미하는지 모름 -->
<dog>나는 개일까요?</dog>


![HTML Tags](/assets/img/2024-07-25-HTMLforBeginnersUnderstandingtheBasics1_1.png)

## 태그란 무엇인가요?

HTML에서 태그는 웹 페이지의 콘텐츠를 정의하고 구조화하는 데 사용됩니다. 이것들은 HTML 문서의 기본 빌딩 블록이며 제목, 문단, 링크, 이미지, 표, 양식 등과 같은 요소를 만드는 데 사용됩니다.


<div class="content-ad"></div>

일반적으로 HTML 태그는 각도 괄호(`< >`)로 감싸져 있으며 일반적으로 쌍으로 구성됩니다: 시작 태그와 종료 태그가 함께 있습니다.

## 쌍태그

쌍태그는 시작 태그와 종료 태그로 구성됩니다. 요소의 내용은 이 두 태그 사이에 배치됩니다. 종료 태그는 태그 이름 앞에 슬래시(/)가 붙습니다.

```js
<h1>This is a heading.</h1>
```

<div class="content-ad"></div>

## 매치되지 않는 태그

매치되지 않는 태그는 독립적이며 별도의 닫힌 태그가 없습니다. 이러한 태그에는 일반적으로 속성이 포함되어 있으며 자체를 하나의 태그로 닫을 수 있습니다.

일반적으로 슬래시가 없는 여는 태그만 사용하는 것이 일반적이지만, 슬래시를 사용하여 닫는 것도 여전히 유효하며, XHTML이나 호환성을 위해 종종 사용됩니다.

```js
<input type="text" name="username">
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-25-HTMLforBeginnersUnderstandingtheBasics1_2.png" />

## 속성

HTML의 속성은 요소에 대한 추가 정보를 제공하는 데 사용됩니다. 이러한 속성은 여는 태그 내에 지정되며 이름/값 쌍으로 제공됩니다. 속성은 HTML 요소의 속성 및 동작을 정의하고 기본 설정을 수정할 수 있습니다.

다음 섹션에서 몇 가지 속성을 살펴보도록 하겠습니다.

<div class="content-ad"></div>

## 클래스

HTML에서 클래스는 스타일링 및 스크립팅을 위해 요소를 그룹화하는 데 사용됩니다. 동일한 클래스를 여러 요소에 할당하여 한꺼번에 CSS 스타일을 쉽게 적용하고 JavaScript로 조작할 수 있습니다.

이를 통해 코드를 효율적으로 유지하고 조직화하여 유지 관리를 쉽게 할 수 있습니다.

```js
<div class="container">내용</div>
```

<div class="content-ad"></div>


[이미지](/assets/img/2024-07-25-HTMLforBeginnersUnderstandingtheBasics1_3.png)

## type, name, placeholder

여기서 재미있는 점이 나타납니다. 우리가 `div`를 문서에 넣으면 일반 텍스트를 볼 수 있지만 `input`을 넣으면 전체 입력 요소를 볼 수 있습니다.

HTML에는 많은 기능이 탑재되어 있습니다. 좋죠.


<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해보세요.

| 속성 | 설명 |
|------|------|
| type | 텍스트, 버튼 또는 스크립트 유형과 같은 요소의 유형을 지정합니다 |
| name | 제출 후 양식 데이터를 참조하는 데 사용되는 요소의 이름을 지정합니다 |
| placeholder | 필드에 입력할 수 있는 내용에 대한 힌트를 제공합니다 |

```js
<input type="text" name="username" placeholder="이름을 입력하세요">
```

<div class="content-ad"></div>


![HTML for Beginners](/assets/img/2024-07-25-HTMLforBeginnersUnderstandingtheBasics1_4.png)

![Medium GIF](https://miro.medium.com/v2/resize:fit:400/0*XxXC6dY-NeYAnCoo.gif)
