---
title: "HTML 입문자를 위한 가이드 Div와 Span 이해하기 10"
description: ""
coverImage: "/assets/img/2024-07-29-HTMLforBeginnersDivSpan10_0.png"
date: 2024-07-29 13:44
ogImage: 
  url: /assets/img/2024-07-29-HTMLforBeginnersDivSpan10_0.png
tag: Tech
originalTitle: "HTML for Beginners Div, Span 10"
link: "https://medium.com/@tomas-svojanovsky/html-for-beginners-div-span-10-8e1eff7f15c8"
isUpdated: true
---




![HTML for Beginners](/assets/img/2024-07-29-HTMLforBeginnersDivSpan10_0.png)

`div`과 `span` 요소는 가장 흔히 사용되는 HTML 요소 중 두 가지로, 보통 일반적인 컨테이너로 지칭됩니다. 다른 용도로는 스타일링을 위해 관련된 요소를 그룹화하거나 JavaScript가 DOM과 상호 작용하도록 하는 역할을 합니다.

## div

`div`는 블록 수준 요소로, 가능한 전체 너비를 차지하며 새로운 줄에서 시작합니다. 블록 수준 콘텐츠를 그룹화하는 데 사용됩니다.

<div class="content-ad"></div>

웹페이지의 섹션이나 구분을 만드는 데 자주 사용되며 더 나은 구성과 레이아웃 구조를 제공합니다.

`div` 요소는 CSS로 스타일을 지정하고 JavaScript로 조작하여 복잡한 레이아웃과 동적 동작을 만들 수 있습니다.

기타 블록 수준 또는 인라인 요소를 포함할 수 있어 HTML 문서를 구조화하기에 다재다능합니다.

```js
<div class="container">
    <h1>제목</h1>
    <p>이것은 div 안의 단락입니다.</p>
</div>
```

<div class="content-ad"></div>

## span

`span`은 인라인 요소로, 새 줄에서 시작하지 않고 필요한 만큼의 너비만 차지합니다. 인라인 콘텐츠를 그룹화하는 데 사용됩니다.

주로 다른 요소 내부의 텍스트나 콘텐츠 중 일부를 스타일링하는 데 사용되며 문서의 흐름을 깨지 않습니다.

`span`은 의미 있는 내용이나 구조적 구분이 필요하지 않을 때 주로 사용되며, 텍스트 일부에 스타일이나 JavaScript 효과를 적용하는 데 이상적입니다.

<div class="content-ad"></div>

```js
<p>This is a <span class="highlight">highlighted</span> word in a paragraph.</p>

<style>
  .highlight {
    color: #f9c282;    
  }
</style>
```