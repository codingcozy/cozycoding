---
title: "HTML과 CSS로 처음 만드는 반응형 웹사이트 제작 가이드"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 18:17
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Building Your First Responsive Website with HTML and CSS"
link: "https://dev.to/egbo2255/building-your-first-responsive-website-with-html-and-css-32eh"
---


반응형 웹사이트를 만드는 것은 프런트엔드 개발자에게 필수적인 기술입니다. 반응형 웹사이트는 장치와 화면 크기에 따라 레이아웃과 콘텐츠를 조정하여 모든 장치에서 훌륭한 사용자 경험을 보장합니다. 이 기사에서는 HTML과 CSS를 사용하여 기본적인 반응형 웹사이트를 만드는 과정을 안내해 드리겠습니다.

### 준비물

시작하기 전에 HTML과 CSS에 대한 기본적인 이해가 필요합니다. CSS Flexbox와 미디어 쿼리에 익숙하다면 좋을 것입니다.

### 단계 1: 프로젝트 설정하기

<div class="content-ad"></div>

새 프로젝트 폴더를 만들고 다음 파일들을 추가하세요:

- index.html: 메인 HTML 파일입니다.
- styles.css: 웹 사이트를 스타일링하는 CSS 파일입니다.

### 단계 2: HTML 구조화

index.html을 열고 다음과 같이 기본 HTML 구조를 추가하세요. 내용은 무엇이든 됩니다:

<div class="content-ad"></div>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Website</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>나의 반응형 웹사이트</h1>
        <nav>
            <ul>
                <li><a href="#home">홈</a></li>
                <li><a href="#about">소개</a></li>
                <li><a href="#services">서비스</a></li>
                <li><a href="#contact">연락처</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <section id="home">
            <h2>나의 웹사이트에 오신 것을 환영합니다</h2>
            <p>이것은 HTML과 CSS로 만든 간단한 반응형 웹사이트입니다.</p>
        </section>
        <section id="about">
            <h2>회사 소개</h2>
            <p>우리는 훌륭한 웹 개발 서비스를 제공합니다.</p>
        </section>
        <section id="services">
            <h2>우리의 서비스</h2>
            <p>다양한 웹 개발 서비스를 제공합니다.</p>
        </section>
        <section id="contact">
            <h2>연락처</h2>
            <p>문의 사항이 있으시면 언제든지 연락해 주세요.</p>
        </section>
    </main>
    <footer>
        <p>&copy; 2024 My Responsive Website</p>
    </footer>
</body>
</html>


### 단계 3: 웹사이트 스타일링하기

이제 styles.css 파일을 열어 몇 가지 기본 스타일을 추가해보세요:

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
}

header {
    background: #333;
    color: #fff;
    padding: 1rem 0;
}

header h1 {
    text-align: center;
}

nav ul {
    display: flex;
    justify-content: center;
    list-style: none;
}

nav ul li {
    margin: 0 1rem;
}

nav ul li a {
    color: #fff;
    text-decoration: none;
}

main {
    padding: 2rem;
}

section {
    margin-bottom: 2rem;
}

footer {
    background: #333;
    color: #fff;
    text-align: center;
    padding: 1rem 0;
    position: fixed;
    width: 100%;
    bottom: 0;
}
```

<div class="content-ad"></div>

### 단계 4: 반응형 웹사이트 만들기

웹사이트를 반응형으로 만들기 위해 미디어 쿼리를 사용할 거에요. 이를 통해 화면 크기에 따라 다른 스타일을 적용할 수 있어요. styles.css에 다음 미디어 쿼리를 추가해 주세요:

```js
@media (max-width: 768px) {
    nav ul {
        flex-direction: column;
        align-items: center;
    }

    nav ul li {
        margin: 0.5rem 0;
    }

    main {
        padding: 1rem;
    }
}

@media (max-width: 480px) {
    header h1 {
        font-size: 1.5rem;
    }

    nav ul li {
        margin: 0.25rem 0;
    }

    main {
        padding: 0.5rem;
    }
}
```

### 단계 5: 웹사이트 테스트하기

<div class="content-ad"></div>

웹 브라우저에서 index.html 파일을 열어서 브라우저 창 크기를 조절해보세요. 화면 크기에 따라 레이아웃이 자동으로 조정되는 것을 확인할 수 있을 거에요. 네비게이션 메뉴가 수직으로 쌓이고 화면 너비가 줄어들면 콘텐츠 주변의 간격이 줄어드는 것을 볼 수 있을 거에요.

### 마지막으로

이렇게 HTML과 CSS를 사용하여 간단한 반응형 웹사이트를 만들어 보았어요! 이 예제는 웹 페이지 구조화 기본과 미디어 쿼리를 사용하여 반응형 디자인을 만드는 방법을 다룹니다. 더 많은 경험을 쌓으면 CSS 그리드, 플렉스박스, 반응형 이미지와 같은 고급 기술을 통해 더 복잡하고 동적인 레이아웃을 만들어볼 수 있어요.

계속해서 기대해주세요!!!