---
title: "JavaScript DOM 조작하기 요소 선택하는 방법 2"
description: ""
coverImage: "/assets/img/2024-07-23-JavaScriptDOMManipulationSelectingElements2_0.png"
date: 2024-07-23 21:37
ogImage: 
  url: /assets/img/2024-07-23-JavaScriptDOMManipulationSelectingElements2_0.png
tag: Tech
originalTitle: "JavaScript DOM Manipulation Selecting Elements 2"
link: "https://medium.com/javascript-in-plain-english/javascript-dom-manipulation-selecting-elements-2-3285860ec8db"
---



![이미지](/assets/img/2024-07-23-JavaScriptDOMManipulationSelectingElements2_0.png)

## HTML 구조

우리는 나중에 사용할 HTML 구조를 정의하는 것으로 시작하겠습니다.

```js
<div class="container">
  <div class="item item-1">아이템 1</div>
  <div class="item item-2">아이템 2</div>
  <div class="item item-3">아이템 3</div>
</div>

<button id="submit-button">제출</button>
```

<div class="content-ad"></div>

## getElementById

getElementById 메서드는 JavaScript의 document 객체의 메서드로, 지정된 id 속성을 가진 요소를 반환합니다.

이 메서드는 DOM에서 하나의 요소를 선택하는 가장 흔한 방법 중 하나입니다.

```js
const submitButton = document.getElementById("submit-button");
console.log(submitButton); // <button id="submit-button">
```

<div class="content-ad"></div>

## querySelector

querySelector 메서드는 JavaScript에서 document의 객체 메서드로, 지정된 CSS 선택자와 일치하는 문서 내 첫 번째 요소를 반환합니다.

getElementById보다 유연한 이 메서드는 더 복잡한 선택을 가능하게 합니다.

```js
const item = document.querySelector(".item-1");
console.log(item); // <div class="item item-1">
```

<div class="content-ad"></div>

## querySelectorAll

querySelectorAll 메소드는 JavaScript의 document 객체의 메소드로, 지정된 CSS 선택자와 일치하는 문서의 모든 요소를 포함하는 정적 NodeList를 반환합니다.

```js
const items = document.querySelectorAll(".item");
console.log(items); // NodeList(3) [ div.item.item-1, div.item.item-2, div.item.item-1 ]
```

## getElementsByClassName

<div class="content-ad"></div>

getElementByClassName 메서드는 JavaScript에서 document의 객체 메서드로, 지정된 클래스의 모든 요소를 포함하는 라이브 HTMLCollection을 반환합니다.

```js
const items = document.getElementsByClassName("item");
console.log(items); // HTMLCollection { 0: div.item.item-1, 1: div.item.item-2, 2: div.item.item-1, length: 3 }
```

![이미지](https://miro.medium.com/v2/resize:fit:400/0*Nfky0UWEViKrwjcp.gif)

# 간단하게 설명 🚀

<div class="content-ad"></div>

플레인 영어 커뮤니티에 참여해 주셔서 감사합니다! 다음에 가시기 전에:

- 작가를 박수 치고 팔로우로 지지해 주세요 👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 만나보기