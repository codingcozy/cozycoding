---
title: "HTML과 CSS에서 Div 중앙에 배치하는 5가지 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-28 13:40
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Different Ways to Center a Div in HTML and CSS"
link: "https://dev.to/muhammedshamal/different-ways-to-center-a-div-in-html-and-css-47df"
---


제 LinkedIn 팔로우해주세요
제 Github에서 팔로우해주세요

클릭하여 읽기

지루한 섹션 없이 코딩으로 바로 이동할 수 있어요!

### 1. 플렉스박스 사용하기

<div class="content-ad"></div>

플렉스박스는 요소들을 가로와 세로로 쉽게 가운데 정렬할 수 있는 강력한 레이아웃 도구입니다.

#### 예시:

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Center Div with Flexbox</title>
    <style>
        .container {
            display: flex;
            justify-content: center; /* 가로 중앙 정렬 */
            align-items: center;    /* 세로 중앙 정렬 */
            height: 100vh;          /* 전체 뷰포트 높이 */
        }

        .centered-div {
            width: 200px;
            height: 200px;
            background-color: lightblue;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="centered-div">Flexbox로 가운데 정렬</div>
    </div>
</body>
</html>
```

### 2. 그리드 사용

<div class="content-ad"></div>

CSS 그리드는 요소를 쉽게 가운데 정렬할 수 있는 또 다른 강력한 레이아웃 시스템입니다.

#### 예시:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Center Div with Grid</title>
    <style>
        .container {
            display: grid;
            place-items: center; /* 수평 및 수직 가운데 정렬 */
            height: 100vh;       /* 화면 전체 높이 */
        }

        .centered-div {
            width: 200px;
            height: 200px;
            background-color: lightcoral;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="centered-div">CSS 그리드로 가운데 정렬</div>
    </div>
</body>
</html>
```

### 3. 절대 위치와 변형을 이용하기

<div class="content-ad"></div>

이 방법은 div를 절대적으로 배치하고 변형(transform)을 사용하여 가운데 정렬하는 방법입니다.

#### 예시:

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Center Div with Absolute Positioning</title>
    <style>
        .container {
            position: relative;
            height: 100vh; /* 전체 화면 높이 */
        }

        .centered-div {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 200px;
            height: 200px;
            background-color: lightgreen;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="centered-div">Centered with Absolute Positioning</div>
    </div>
</body>
</html>
```

### 4. margin auto 사용하기

<div class="content-ad"></div>

margin: auto를 지정된 너비를 가진 요소에 설정하면 수평으로 가운데 정렬할 수 있습니다.

#### 예시:

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Center Div with Margin Auto</title>
    <style>
        .container {
            width: 100%;
            height: 100vh; /* 전체 뷰포트 높이 */
            display: flex;
            align-items: center; /* 수직 중앙 정렬 */
        }

        .centered-div {
            margin: 0 auto; /* 수평 중앙 정렬 */
            width: 200px;
            height: 200px;
            background-color: lightcoral;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="centered-div">Centered with Margin Auto</div>
    </div>
</body>
</html>
```

### 5. 테이블 디스플레이 사용

<div class="content-ad"></div>

이 방법은 요소를 가운데 정렬하는 데 display: table 및 display: table-cell을 사용합니다.

#### 예시:

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Center Div with Table Display</title>
    <style>
        .container {
            display: table;
            width: 100%;
            height: 100vh; /* 전체 뷰포트 높이 */
        }

        .centered-div {
            display: table-cell;
            vertical-align: middle; /* 수직 중앙 정렬 */
            text-align: center;     /* 수평 중앙 정렬 */
        }

        .inner-div {
            display: inline-block;
            width: 200px;
            height: 200px;
            background-color: lightpink;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="centered-div">
            <div class="inner-div">Centered with Table Display</div>
        </div>
    </div>
</body>
</html>
```

잘가
코딩하세요!