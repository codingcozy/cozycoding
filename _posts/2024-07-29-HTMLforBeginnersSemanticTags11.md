---
title: "초보자를 위한 HTML 시맨틱 태그 11"
description: ""
coverImage: "/assets/img/2024-07-29-HTMLforBeginnersSemanticTags11_0.png"
date: 2024-07-29 13:44
ogImage: 
  url: /assets/img/2024-07-29-HTMLforBeginnersSemanticTags11_0.png
tag: Tech
originalTitle: "HTML for Beginners Semantic Tags 11"
link: "https://medium.com/stackademic/html-for-beginners-semantic-tags-11-459094b14322"
---


![이미지](/assets/img/2024-07-29-HTMLforBeginnersSemanticTags11_0.png)

## 시멘틱 태그란 무엇인가요?

시멘틱 요소는 인간과 기계가 이해할 수 있는 방식으로 명확하게 의미를 설명하는 HTML 태그입니다. 이러한 요소들은 웹 콘텐츠의 구조를 정의하는 것뿐만 아니라 그 안에 포함된 콘텐츠의 역할과 중요성에 대한 정보를 제공합니다.

이를 통해 검색 엔진, 스크린 리더 및 다른 도구가 콘텐츠의 문맥을 이해하고 웹페이지의 전체 접근성 및 SEO(Search Engine Optimization)를 개선하는 데 도움이 됩니다.

<div class="content-ad"></div>

## 헤더

웹페이지의 헤더 섹션 또는 콘텐츠 섹션을 정의합니다. 일반적으로 사이트의 제목, 로고, 내비게이션 링크 등이 포함됩니다.

```js
<header>
    <h1>웹사이트 제목</h1>
    <nav>
        <ul>
            <li><a href="#home">홈</a></li>
            <li><a href="#about">소개</a></li>
            <li><a href="#contact">문의</a></li>
        </ul>
    </nav>
</header>
```

## 내비게이션

<div class="content-ad"></div>

탐색 링크 세트를 정의합니다.

```js
<nav>
    <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#services">Services</a></li>
        <li><a href="#contact">Contact</a></li>
    </ul>
</nav>
```

## 섹션