---
title: "HTML 문자열로 DOM 요소 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_0.png"
date: 2024-08-03 18:14
ogImage: 
  url: /assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_0.png
tag: Tech
originalTitle: "How to Create DOM Elements from HTML Strings Multiple Methods"
link: "https://dev.to/zacharylee/how-to-create-dom-elements-from-html-strings-multiple-methods-5fk"
isUpdated: true
updatedAt: 1724246374973
---


간단한 동적 HTML DOM 요소를 만들어야 할 때 HTML 문자열 템플릿을 사용할 수 있어요. JavaScript에서 이를 달성하는 다양한 방법이 있지만 고려해야 할 점이 있을 수 있어요.

## innerHTML

![이미지](/assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_0.png)

```js
const container = document.createElement('div');
container.innerHTML = `<div>Hello World</div>`;
const node = container.firstElementChild;
```

<div class="content-ad"></div>

이것은 가장 일반적인 방법 중 하나입니다. 컨테이너 요소의 innerHTML에 HTML 문자열을 삽입한 다음 생성된 DOM 노드에 액세스합니다.

그러나 이는 유효한 HTML 노드, 즉 표준 HTML 구조에서 허용되는 요소들만 처리할 수 있습니다. 예를 들어 `tr` 요소를 `div`에 삽입하려고 하면 노드가 생성되지 않습니다.

또한, 이 방법은 HTML 문자열에서 스크립트를 실행하지 않으며, 신뢰할 수 없는 내용을 처리할 때 안전합니다.

![이미지](/assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_1.png)

<div class="content-ad"></div>

## innerHTML + template Element

![HowtoCreateDOMElementsfromHTMLStringsMultipleMethods](/assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_2.png)

```js
const template = document.createElement('template');
template.innerHTML = `<div>Hello World</div>`;
const node = template.content.firstElementChild;
```

Using the `template` tag is great because it allows you to include various HTML structures without content restrictions, such as table-related elements like `tr` and `td`.

<div class="content-ad"></div>


아래는 Markdown 형식으로 변경한 내용입니다.

![이미지](/assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_3.png)

## insertAdjacentHTML

![이미지](/assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_4.png)

```js
const container = document.createElement("div");
container.insertAdjacentHTML("afterbegin", `<div>Hello World</div>`);
const node = container.firstElementChild;
```

<div class="content-ad"></div>

이 방법은 innerHTML과 비슷하며 유효한 HTML 노드만 다룰 수 있습니다. 또한 스크립트를 실행하지 않습니다.

## DOMParser

![이미지](/assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_5.png)

```js
const node = new DOMParser().parseFromString(`<div>Hello World</div>`, "text/html").body.firstElementChild;
```

<div class="content-ad"></div>

이 방법은 전체 HTML 문서를 만들어 문자열을 구문 분석 한 후에 문서에서 노드를 추출합니다. 이는 다른 솔루션보다 상대적으로 느리며 사용을 권장하지 않습니다.

유효한 HTML 노드만 처리할 수 있으며 스크립트를 실행하지 않습니다.

## Range.createContextualFragment

![이미지](/assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_6.png)

<div class="content-ad"></div>

```js
const node = document.createRange().createContextualFragment(`<div>Hello World</div>`).firstElementChild;
```

이 방법은 아마도 가장 간단한 방법입니다. 또한 기본적으로 유효한 HTML 노드만 처리할 수 있습니다. 그러나 이를 피하기 위해 컨텍스트를 설정할 수 있습니다.

![image](/assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_7.png)

HTML 문자열에서 스크립트를 실행하므로 DOMPurify와 같은 클리너를 사용하여 신뢰할 수 없는 콘텐츠를 다룰 때 특히 주의해야 합니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-08-03-HowtoCreateDOMElementsfromHTMLStringsMultipleMethods_8.png" />

## 결론

다양한 경우에 따라서, 여러분에게 가장 적합한 방법을 선택해야 할 수도 있어요.

만약 이 글이 도움이 되었다면, 구독해 주시면 감사하겠어요. 저는 매일 웹 개발 업데이트에 관한 최신 뉴스를 보내드립니다. 지원해 주셔서 감사합니다!