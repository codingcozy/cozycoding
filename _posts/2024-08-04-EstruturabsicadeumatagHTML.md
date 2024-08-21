---
title: "HTML 태그의 기본 구조 정리"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-04 19:35
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Estrutura bsica de uma tag HTML"
link: "https://dev.to/mikedsousa/estrutura-basica-de-uma-tag-html-39ab"
isUpdated: true
updatedAt: 1724246248337
---


나는 개발자입니다. 위의 텍스트를 친근한 어조로 한국어로 번역해 드리겠습니다.

"나는 개발자입니다. 위의 텍스트를 친근한 어조로 한국어로 번역해 드리겠습니다."

마크다운 형식으로 표 태그를 변경하겠습니다.

<div class="content-ad"></div>

태그 이름은 HTML 태그를 볼 때 처음으로 나타나는 부분입니다. 어떤 종류의 요소가 생성되는지를 정의합니다. 예를 들어, p라는 태그는 문단을 나타내고, a 태그는 다른 웹 페이지로 연결되는 링크를 나타냅니다.

```js
<p>문단 예시</p>
<a href="http://example.com">링크 예시</a>
```

- 속성

속성은 HTML 요소에 대한 추가 정보를 제공합니다. 속성은 항상 여는 태그에 지정되며 이름과 값으로 구성되며 등호 기호로 구분됩니다. 예를 들어:

<div class="content-ad"></div>


[Visite nosso site](https://www.example.com){:target="_blank"}


Aqui, href는 링크 대상을 지정하는 속성이고, target="_blank"는 링크가 새 탭이나 창에서 열리도록 지시하는 속성입니다. class, id, src, alt(이미지의 대체 텍스트)와 같은 흔한 속성 외에도 특정 HTML 요소에는 고유한 속성이 있어 추가 기능을 제공합니다. 이러한 속성은 특정 태그 유형에 고유하며 특정 동작과 스타일을 제공합니다.

type (input 요소에서) — 폼에서 입력 유형을 정의하며, 텍스트, 비밀번호, 버튼 등이 있습니다.


<div class="content-ad"></div>

```js
<input type="text" placeholder="이름을 입력하세요">
```

disabled (버튼, 입력란에서 사용) — 해당 요소가 비활성화되어 상호작용할 수 없음을 나타냅니다.

```js
<button disabled>비활성화된 버튼</button>
```

- 내용

<div class="content-ad"></div>

내용은 열고 닫는 태그 사이에 들어가는 내용입니다. 이는 브라우저에서 표시되는 정보나 텍스트입니다. 예를 들어:

```js
<p>이것은 문단의 내용입니다.</p>
```

일부 태그의 경우, 자체적으로 완전한 내용을 가지고 있지만, 이 내용은 속성으로만 표현됩니다. 예를 들어, 이미지를 위한 img 태그:

```js
<img src="이미지.jpg" alt="이미지 설명">
```

<div class="content-ad"></div>

여기서 이미지 자체가 src(이미지 경로) 및 alt(대체 텍스트)로 나타낸 내용입니다.

HTML 태그의 구조와 구성 요소를 이해하는 것은 효율적인 방식으로 웹페이지를 만드는 데 중요합니다. 태그 이름, 속성 및 내용은 각 페이지의 부분이 렌더링되고 작동하는 방식을 정의하는 기본 요소입니다.