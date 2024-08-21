---
title: "초보자를 위한 HTML 링크 사용법 가이드 파트 7"
description: ""
coverImage: "/assets/img/2024-07-29-HTMLforBeginnersLinks7_0.png"
date: 2024-07-29 13:45
ogImage: 
  url: /assets/img/2024-07-29-HTMLforBeginnersLinks7_0.png
tag: Tech
originalTitle: "HTML for Beginners Links 7"
link: "https://medium.com/@tomas-svojanovsky/html-for-beginners-links-7-0361a966e2c7"
isUpdated: true
---





<img src="/assets/img/2024-07-29-HTMLforBeginnersLinks7_0.png" />

## External link

The `a` tag is used to create links. The href attribute specifies the URL the link points to. An external link leads to another web page outside the current website.

```js
<a> 태그는 링크를 생성하는 데 사용됩니다. href 속성은 링크가 가리키는 URL을 지정합니다. 외부 링크는 현재 웹 사이트 외부의 다른 웹 페이지로 연결합니다.
```

<div class="content-ad"></div>

`a` 태그에서 target 속성은 링크가 열리는 방식을 지정합니다. _blank 값은 링크를 새 창이나 탭에서 엽니다.

```js
<a href="https://www.example.com" target="_blank">새 창에서 Example.com 열기</a>
```

## 상대 링크

상대 링크는 동일한 웹사이트 또는 서버에 있는 파일의 상대 경로를 사용합니다. 동일한 사이트의 페이지 간에 링크를 만드는 데 유용합니다.

<div class="content-ad"></div>


[Contact Us](/contact.html)


## Anchor link

앵커 링크는 동일한 페이지의 특정 부분으로 이동합니다. 이는 긴 문서의 특정 섹션으로 빠르게 이동하는 데 사용됩니다.


[Section 1로 이동](#section1)


<div style="height: 1000px;"></div> <!-- 이 div는 스크롤링을 위한 공간을 추가하는 데 사용됩니다 -->

<div id="section1">
    <h2>Section 1</h2>
    <p>이 문서의 Section 1입니다.</p>
</div>


<div class="content-ad"></div>

## 이메일 링크

이메일 링크는 클릭하면 사용자의 기본 이메일 클라이언트가 지정된 이메일 주소로 새 이메일 초안을 열어 줍니다.

```js
[이메일 보내기](mailto:example@example.com)
```