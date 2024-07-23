---
title: "자바스크립트 DOM 조작 DOM 트래버싱 - 활용 예제 3"
description: ""
coverImage: "/assets/img/2024-07-23-JavaScriptDOMManipulationTraversingtheDOM3_0.png"
date: 2024-07-23 21:34
ogImage: 
  url: /assets/img/2024-07-23-JavaScriptDOMManipulationTraversingtheDOM3_0.png
tag: Tech
originalTitle: "JavaScript DOM Manipulation Traversing the DOM 3"
link: "https://medium.com/javascript-in-plain-english/javascript-dom-manipulation-traversing-the-dom-3-b39f33459e9c"
---


<img src="/assets/img/2024-07-23-JavaScriptDOMManipulationTraversingtheDOM3_0.png" />

## HTML 구조

나중에 사용할 HTML 구조를 정의하는 것부터 시작하겠습니다.

```js
<div class="container">
  <!-- comment -->
  <div class="item item-1">Item 1</div>
  <div class="item item-2">Item 2</div>
  <div class="item item-3">Item 3</div>
</div>
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-23-JavaScriptDOMManipulationTraversingtheDOM3_1.png" />

## children

우리가 하는 건 테이블 태그를 마크다운 형식으로 바꾸는거야. 

```js
const container = document.querySelector(".container");
console.log(container.children); // HTMLCollection(3) [div.item.item-1, div.item.item-2, div.item.item-1]
```

<div class="content-ad"></div>

## firstElementChild

첫 번째 자식 요소에 접근할 때 firstElementChild 속성을 사용할 수 있어요. 주의할 점은 주석은 건너뛴다는 거에요.

```js
const container = document.querySelector(".container");
console.log(container.firstElementChild); // <div class="item item-1">Item 1</div>
```

## lastElementChild

<div class="content-ad"></div>

부모 요소의 마지막 요소에는 `lastElementChild` 속성을 사용하여 접근할 수 있습니다.

```js
const container = document.querySelector(".container");
console.log(container.lastElementChild); // <div class="item item-3">Item 3</div>
```

## parentElement

어떤 아이템에 있을 때 `parent` 속성을 사용하여 쉽게 부모로 돌아갈 수 있습니다.

<div class="content-ad"></div>

```js
const item = document.querySelector(".item-2");
console.log(item.parentElement); // <div class="container"></div>
```

## nextElementSibling

자식 요소 내부에서 다음 자식에 접근하려면 nextElementSibling 속성을 사용할 수 있습니다.

```js
const item = document.querySelector(".item-2");
console.log(item.nextElementSibling); // <div class="item item-3">Item 3</div>
```

<div class="content-ad"></div>

## previousElementSibling

이전 예제와 비슷한 상황에서 previousElementSibling 속성을 사용하여 이전 자식에 접근할 수 있습니다.

```js
const item = document.querySelector(".item-2");
console.log(item.previousElementSibling); // <div class="item item-1">Item 1</div>
```

## childNodes

<div class="content-ad"></div>

이전에 본 children보다 더 많은 것을 반환합니다; 텍스트와 주석도 반환됩니다.

```js
const container = document.querySelector(".container");
console.log(container.childNodes); // NodeList(9) [text, comment, text, div.item.item-1, text, div.item.item-2, text, div.item.item-3, text]
```

## firstChild

첫 번째 child 요소로는 item-1 클래스가 있는 div를 받을 것으로 기대하지만, 대신 텍스트 노드를 받습니다. div를 얻으려면 firstElementChild를 사용해야 합니다.

<div class="content-ad"></div>

```js
const container = document.querySelector(".container");
console.log(container.lastChild); // #text
```

## lastChild

마지막 자식을 액세스하려면 요소가 아닌 텍스트 노드나 코멘트와 같은 다른 것이 반환될 수 있다는 것을 인식해야 합니다. 요소를 얻으려면 lastElementChild를 사용하세요.

여기서 닫는 div 태그 앞에 새 줄이 있어 텍스트 노드도 가져올 것입니다.

<div class="content-ad"></div>

```js
const container = document.querySelector(".container");
console.log(container.lastChild); // #text
```

## nextSibling

다음 형제에 접근하려면 구조에 따라 요소 또는 텍스트 노드를 가져올 수 있다는 점을 인지해야 합니다.”

```js
const item = document.querySelector(".item-1");
console.log(item.nextSibling); // #text
```

<div class="content-ad"></div>

구조를 약간 변경하면 두 번째 div를 얻을 수 있습니다.

```js
<div class="container">
  <!-- 주석 -->
  <div class="item item-1">아이템 1</div><div class="item item-2">아이템 2</div>
  <div class="item item-3">아이템 3</div>
</div>

<script>
  const item = document.querySelector(".item-1");
  console.log(item.nextSibling); // div.item.item-2
</script>
```

## previousSibling

previousSibling에도 nextSibling와 같은 원리가 적용됩니다.

<div class="content-ad"></div>

```js
const item = document.querySelector(".item-1");
console.log(item.previousSibling); // #text
```

![In Plain English](https://miro.medium.com/v2/resize:fit:400/0*C0LGWZGWFTRANQTO.gif)

# 평문으로 보는 영어 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

<div class="content-ad"></div>

- 작가를 박수 치고 팔로우 ️👏️️해 주세요
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 확인하세요