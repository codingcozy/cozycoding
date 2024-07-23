---
title: "자바스크립트의 DOM 조작 방법 요소 교체와 삭제 5번째 방법"
description: ""
coverImage: "/assets/img/2024-07-23-JavaScriptDOMManipulationReplacingDeletingElements5_0.png"
date: 2024-07-23 21:35
ogImage: 
  url: /assets/img/2024-07-23-JavaScriptDOMManipulationReplacingDeletingElements5_0.png
tag: Tech
originalTitle: "JavaScript DOM Manipulation Replacing, Deleting Elements 5"
link: "https://medium.com/@tomas-svojanovsky/javascript-dom-manipulation-replacing-deleting-elements-5-098f4b3a8f13"
---


![image](/assets/img/2024-07-23-JavaScriptDOMManipulationReplacingDeletingElements5_0.png)

## HTML 구조

나중에 사용할 HTML 구조를 정의하는 것부터 시작하겠습니다.

```js
<ul>
  <li class="item item-1">아이템 1</li>
  <li class="item item-2">아이템 2</li>
  <li class="item item-3">아이템 3</li>
</ul>
```

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-23-JavaScriptDOMManipulationReplacingDeletingElements5_1.png)

## 요소 교체하기

예를 들어, replaceWith 메서드를 사용하여 첫 번째 li를 다른 li로 교체할 수 있습니다.

```js
const firstLi = document.querySelector(".item-1");

const newLi = document.createElement("li");
newLi.className = "item item-1";
newLi.innerText = "새로운 아이템 1";

firstLi.replaceWith(newLi);
```

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-23-JavaScriptDOMManipulationReplacingDeletingElements5_2.png)

## OuterHTML

replaceWith 대신 outerHTML을 사용할 수도 있습니다.

```js
const firstLi = document.querySelector(".item-1");

const newLi = document.createElement("li");
newLi.className = "item item-1";
newLi.innerText = "새로운 아이템 1";

firstLi.outerHTML = newLi.outerHTML;
```


<div class="content-ad"></div>


![Image](/assets/img/2024-07-23-JavaScriptDOMManipulationReplacingDeletingElements5_3.png)

## InnerHTML

만약 li 요소의 내용 부분만 바꾸고 싶다면, innerHTML을 사용할 수 있어요.

```js
const firstLi = document.querySelector(".item-1");

firstLi.innerHTML = "<div class='new-item'>안녕, 나는 div야</div>";
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-23-JavaScriptDOMManipulationReplacingDeletingElements5_4.png" />

## 삭제하기

만약 요소를 삭제하고 싶다면, 문서에서 해당 요소를 선택하고 remove 메서드를 호출해야 합니다.

```js
const firstLi = document.querySelector(".item-1");

firstLi.remove();
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-23-JavaScriptDOMManipulationReplacingDeletingElements5_5.png" />

## 자식 노드 제거

removeChild 메서드는 부모 노드의 DOM에서 자식 노드를 제거합니다. 부모 및 자식 엘리먼트에 모두 접근해야합니다.

이번에는 다른 HTML을 사용할 예정입니다.

<div class="content-ad"></div>

```js
<div id="parentElement">
    <p id="childElement">나는 자식 요소입니다</p>
</div>
```

위 코드를 실행한 후, 1초 후에 "나는 자식"에서 "이것은 자식 요소입니다"로 내용이 변경되고, 그런 다음 내용이 제거됩니다.

```js
function wait(time) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve("ok");
    }, time);
  })
}

async function main() {
  await wait(1000);
  // remove를 사용
  let child = document.getElementById("childElement");
  child.remove(); // 자식 요소를 직접 제거합니다.

  await wait(1000);
  
  // 증명을 위해 요소를 다시 추가
  let parent = document.getElementById("parentElement");
  let newChild = document.createElement("p");
  newChild.id = "childElement";
  newChild.textContent = "이것은 자식 요소입니다";
  parent.appendChild(newChild);

  await wait(1000);
  
  // removeChild를 사용
  child = document.getElementById("childElement");
  parent.removeChild(child); // 부모 요소를 사용하여 자식 요소를 제거합니다.
}

main();
```

<img src="https://miro.medium.com/v2/resize:fit:400/0*-AkfuiXj_KJahBiH.gif" />
