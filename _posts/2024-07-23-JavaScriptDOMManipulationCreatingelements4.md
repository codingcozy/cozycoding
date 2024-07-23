---
title: "JavaScript DOM 조작 요소 생성 방법 4"
description: ""
coverImage: "/assets/img/2024-07-23-JavaScriptDOMManipulationCreatingelements4_0.png"
date: 2024-07-23 21:36
ogImage: 
  url: /assets/img/2024-07-23-JavaScriptDOMManipulationCreatingelements4_0.png
tag: Tech
originalTitle: "JavaScript DOM Manipulation Creating elements 4"
link: "https://medium.com/@tomas-svojanovsky/javascript-dom-manipulation-creating-elements-4-9a26b57c72ed"
---



![JavaScript DOM Manipulation Creating Elements](/assets/img/2024-07-23-JavaScriptDOMManipulationCreatingelements4_0.png)

만약 HTML 요소를 생성하려면 createElement를 사용할 수 있습니다. 그런 다음 className 및 setAttribute와 같은 여러 가지를 호출할 수 있습니다. 새 요소를 문서에 추가하려면 새 요소를 삽입하려는 요소를 선택하고 appendChild 메서드를 호출해야 합니다.

요소에 텍스트를 삽입하려면 innerText를 사용하거나 직접 textNode를 생성할 수도 있습니다.

```js
const div = document.createElement("div");
div.className = "container";
div.setAttribute("title", "This is container");

// div.innerText = "Hello from container";

const textNode = document.createTextNode("Hello from container");
div.appendChild(textNode);

document.body.appendChild(textNode);
``` 


<div class="content-ad"></div>


![image](/assets/img/2024-07-23-JavaScriptDOMManipulationCreatingelements4_1.png)

## createTextNode

createTextNode 메서드는 주어진 텍스트를 포함하는 새로운 텍스트 노드를 생성하는 방법입니다. 그런 다음 appendChild 또는 insertBefore와 같은 메서드를 사용하여 이 노드를 DOM의 어떤 요소에도 연결할 수 있습니다.

## innerText


<div class="content-ad"></div>

innerText은 요소의 텍스트 내용을 설정하거나 반환하는 속성입니다.

## innerHTML

```js
const div = document.createElement("div");
div.className = "container";

div.innerHTML = `
    <div class="item item-1">Item 1</div>
    <div class="item item-2">Item 2</div>
    <div class="item item-3">Item 3</div>
`;

document.body.appendChild(div);
```

![이미지](/assets/img/2024-07-23-JavaScriptDOMManipulationCreatingelements4_2.png)

<div class="content-ad"></div>

## innerHTML x createElement

createElement은 지정된 유형의 새 요소를 생성하는 방법입니다. 이 요소는 appendChild, insertBefore 또는 replaceChild와 같은 메서드를 사용하여 다른 요소에 명시적으로 추가할 때까지 DOM 내의 다른 요소와 독립적입니다.

innerHTML은 주어진 요소의 HTML 콘텐츠를 설정하거나 가져올 수 있게 하는 속성입니다. innerHTML에 값을 할당하면 해당 요소의 모든 기존 내용을 새 HTML 코드로 대체할 수 있으며, 이는 다른 요소, 텍스트 또는 스크립트를 포함할 수 있습니다.

```js
<div id="section">
  
</div>

<script>
  const newElement = document.createElement("p");
  newElement.textContent = "This is a new paragraph";
  const element = document.getElementById("section");
  element.appendChild(newElement);
</script>
```

<div class="content-ad"></div>


![Image 1](/assets/img/2024-07-23-JavaScriptDOMManipulationCreatingelements4_3.png)

![Image 2](https://miro.medium.com/v2/resize:fit:400/0*kU6jJ0LPe_adkLX2.gif)
