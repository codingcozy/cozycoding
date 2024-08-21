---
title: "CSS Grid 마스터하기 레이아웃 쉽게 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-18-MasteringCSSGridLayoutsMadeEasy_0.png"
date: 2024-07-18 11:36
ogImage: 
  url: /assets/img/2024-07-18-MasteringCSSGridLayoutsMadeEasy_0.png
tag: Tech
originalTitle: "Mastering CSS Grid Layouts Made Easy"
link: "https://medium.com/@araujogabe1/mastering-css-grid-layouts-made-easy-38166c22c2be"
isUpdated: true
---




![CSS Grid](/assets/img/2024-07-18-MasteringCSSGridLayoutsMadeEasy_0.png)

복잡한 웹 레이아웃을 만드는 것은 웹 개발자들에게 전통적으로 어려운 작업이었습니다. 그러나 CSS Grid의 등장으로 세련되고 반응형 웹 레이아웃을 디자인하는 것이 더 직관적이고 간편해졌습니다. CSS Grid는 개발자가 외부 프레임워크나 복잡한 해결책 없이 CSS에서 직접 그리드 기반 디자인을 만들 수 있는 강력한 레이아웃 시스템입니다. 이 글에서는 CSS Grid의 기본 개념을 탐색하고 이를 활용하여 웹 레이아웃을 간단화하는 방법을 보여줄 것입니다.

# CSS Grid란 무엇인가요?

CSS Grid Layout은 웹을 위한 이차원 레이아웃 시스템인 "Grid"로 알려져 있습니다. 이를 사용하면 항목을 행과 열로 배치하여 Flexbox와 같은 1차원 레이아웃 시스템보다 더 많은 유연성을 제공합니다. Grid는 가로와 세로 차원 모두에 대해 정밀한 제어가 필요한 복잡한 레이아웃에 적합합니다.

<div class="content-ad"></div>

# CSS Grid의 기본 개념

코드를 시작하기 전에 CSS Grid의 몇 가지 기본 개념을 살펴보겠습니다:

- 그리드 컨테이너: 그리드 항목을 포함하는 부모 요소입니다. display: grid를 요소에 설정하여 그리드 컨테이너를 정의합니다.
- 그리드 항목: 그리드 컨테이너의 직속 자식요소입니다.
- 그리드 라인: 그리드를 나누는 수평 및 수직 라인입니다.
- 그리드 트랙: 그리드 라인 사이에 정의된 행 또는 열입니다.
- 그리드 셀: 그리드에서의 가장 작은 단위로, 행과 열의 교차점으로 정의됩니다.

# 간단한 그리드 생성하기

<div class="content-ad"></div>

간단한 예제를 시작해 보겠습니다. 세 개의 열과 두 개의 행으로 구성된 기본 그리드를 만들어보겠습니다.

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple CSS Grid</title>
    <style>
        .grid-container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-template-rows: repeat(2, 100px);
            gap: 10px;
            background-color: #f3f3f3;
            padding: 10px;
        }
        .grid-item {
            background-color: #4CAF50;
            color: white;
            padding: 20px;
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="grid-container">
        <div class="grid-item">1</div>
        <div class="grid-item">2</div>
        <div class="grid-item">3</div>
        <div class="grid-item">4</div>
        <div class="grid-item">5</div>
        <div class="grid-item">6</div>
    </div>
</body>
</html>
```

이 예제에서는 세 개의 열과 두 개의 행으로 구성된 그리드 컨테이너를 만듭니다. repeat 함수를 사용하여 반복되는 트랙을 간단히 정의할 수 있으며, 1fr 단위(분수 단위)는 각 열이 동일한 공간을 차지하게 합니다.

# 고급 그리드 기술

<div class="content-ad"></div>

이제 기본적인 이해를 얻었으니, CSS Grid를 무척 강력하게 만드는 몇 가지 고급 기술에 대해 알아보겠습니다.

## 그리드 템플릿 영역

그리드 템플릿 영역을 사용하면 그리드 내에서 영역을 정의하고 그리드 항목을 이러한 영역에 할당할 수 있습니다. 이 방법을 사용하면 HTML이 더 깨끗해지고 CSS가 더 읽기 쉬워질 수 있습니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grid Template Areas</title>
    <style>
        .grid-container {
            display: grid;
            grid-template-areas:
                'header header header'
                'sidebar main main'
                'footer footer footer';
            grid-gap: 10px;
            background-color: #f3f3f3;
            padding: 10px;
        }
        .header { grid-area: header; }
        .sidebar { grid-area: sidebar; }
        .main { grid-area: main; }
        .footer { grid-area: footer; }
        .grid-item {
            background-color: #4CAF50;
            color: white;
            padding: 20px;
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="grid-container">
        <div class="grid-item header">Header</div>
        <div class="grid-item sidebar">Sidebar</div>
        <div class="grid-item main">Main Content</div>
        <div class="grid-item footer">Footer</div>
    </div>
</body>
</html>
```

<div class="content-ad"></div>

여기서는 header, sidebar, main, 및 footer라는 이름의 영역을 정의합니다. 각 grid 항목은 grid-area 속성을 사용하여 해당 영역에 할당됩니다. 이 접근 방식은 복잡한 레이아웃을 생성하는 과정을 크게 간소화할 수 있습니다.

## 반응형 그리드

CSS Grid의 중요한 장점 중 하나는 쉽게 반응형 레이아웃을 만들 수 있는 능력입니다. 미디어 쿼리를 사용하여 viewport 크기에 기반한 그리드 구조를 변경할 수 있습니다.

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Grid</title>
    <style>
        .grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
            gap: 10px;
            background-color: #f3f3f3;
            padding: 10px;
        }
        .grid-item {
            background-color: #4CAF50;
            color: white;
            padding: 20px;
            text-align: center;
        }
        @media (min-width: 600px) {
            .grid-container {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            }
        }
        @media (min-width: 900px) {
            .grid-container {
                grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            }
        }
    </style>
</head>
<body>
    <div class="grid-container">
        <div class="grid-item">1</div>
        <div class="grid-item">2</div>
        <div class="grid-item">3</div>
        <div class="grid-item">4</div>
        <div class="grid-item">5</div>
        <div class="grid-item">6</div>
    </div>
</body>
</html>
```

<div class="content-ad"></div>

이 예시에서는 minmax와 auto-fill을 사용하여 반응형 그리드를 생성합니다. 뷰포트 너비에 따라 열의 수가 조정되어 레이아웃이 화면 크기에 항상 최적화됩니다.

# 실제 예시: 사진 갤러리

뷰포트 크기에 따라 레이아웃을 조정하는 사진 갤러리를 만들어 보겠습니다.

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Photo Gallery</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #fafafa;
            margin: 0;
            padding: 0;
        }
        .gallery {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 10px;
            padding: 10px;
        }
        .gallery-item {
            background-color: #eee;
            padding: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        .gallery-item img {
            max-width: 100%;
            height: auto;
            display: block;
        }
    </style>
</head>
<body>
    <div class="gallery">
        <div class="gallery-item"><img src="image1.jpg" alt="Image 1"></div>
        <div class="gallery-item"><img src="image2.jpg" alt="Image 2"></div>
        <div class="gallery-item"><img src="image3.jpg" alt="Image 3"></div>
        <div class="gallery-item"><img src="image4.jpg" alt="Image 4"></div>
        <div class="gallery-item"><img src="image5.jpg" alt="Image 5"></div>
        <div class="gallery-item"><img src="image6.jpg" alt="Image 6"></div>
    </div>
</body>
</html>
```

<div class="content-ad"></div>

이 갤러리에서는 사용 가능한 공간에 기반하여 한 행당 가능한 많은 항목을 맞추기 위해 그리드가 조정됩니다. 이를 통해 이 갤러리가 모바일 전화부터 대형 데스크톱 화면까지 모든 장치에서 멋지게 보이도록 보장합니다.

# 결론

CSS 그리드는 웹 개발자들에게 혁명을 일으키는 도구입니다. 최소한의 노력으로 복잡한 레이아웃을 만들기 위한 강력하고 유연한 방법을 제공합니다. 기본기를 마스터하고 고급 기술을 탐색함으로써 웹 디자인을 크게 향상시키고 사용자 경험을 향상시킬 수 있습니다. 간단한 웹 페이지에서 복잡한 응용프로그램을 만들고 있더라도 CSS 그리드는 반응형이며 시각적으로 매력적인 레이아웃을 만들 수 있도록 필요한 도구를 제공합니다.

다음 프로젝트에서 CSS 그리드를 활용하고 웹 개발에 불러오는 쉬움과 효율성을 경험해보세요. 즐거운 코딩하세요!