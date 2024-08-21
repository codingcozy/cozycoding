---
title: "부트스트랩 기초 다크 모드 구현 방법 10"
description: ""
coverImage: "/assets/img/2024-07-22-BootstrapFundamentalsDarkMode10_0.png"
date: 2024-07-22 11:24
ogImage: 
  url: /assets/img/2024-07-22-BootstrapFundamentalsDarkMode10_0.png
tag: Tech
originalTitle: "Bootstrap Fundamentals  Dark Mode 10"
link: "https://medium.com/@tomas-svojanovsky/bootstrap-fundamentals-dark-mode-10-ec746978c7b4"
isUpdated: true
---




<img src="/assets/img/2024-07-22-BootstrapFundamentalsDarkMode10_0.png" />

## 다크 모드

많은 웹 사이트들이 사용자가 다크 모드나 라이트 모드를 설정할 수 있게 합니다. 부트스트랩은 이를 이해하고 원하는 모드를 선택할 수 있게 해줍니다.

부트스트랩을 사용하여 다크 모드를 설정하려면 data-bs-theme="dark"를 사용할 수 있습니다.

<div class="content-ad"></div>

```js
<!doctype html>
<html lang="en" data-bs-theme="dark">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap fundamentals</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
          integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">

    <style>
        /*background: var(--bs-primary);*/
        /*background: var(--bs-warning);*/
    </style>
</head>
<body>
<div class="container">
    <h1 class="mb-5">다크 모드</h1>

    <div class="card" style="width: 18rem;">
        <img src="https://imageio.forbes.com/specials-images/imageserve/5d35eacaf1176b0008974b54/0x0.jpg?format=jpg&crop=4560,2565,x790,y784,safe&height=900&width=1600&fit=bounds"
             class="card-img-top" alt="...">
        <div class="card-body">
            <h5 class="card-title">카드 제목</h5>
            <p class="card-text">카드 제목을 바탕으로 빠르게 예제 텍스트 작성 및 카드 콘텐츠의 대부분 형성합니다.</p>
            <a href="#" class="btn btn-primary">어딘가로 가기</a>
        </div>
    </div>

    <div class="card" style="width: 18rem;" data-bs-theme="light">
        <img src="https://imageio.forbes.com/specials-images/imageserve/5d35eacaf1176b0008974b54/0x0.jpg?format=jpg&crop=4560,2565,x790,y784,safe&height=900&width=1600&fit=bounds"
             class="card-img-top" alt="...">
        <div class="card-body">
            <h5 class="card-title">카드 제목</h5>
            <p class="card-text">카드 제목을 바탕으로 빠르게 예제 텍스트 작성 및 카드 콘텐츠의 대부분 형성합니다.</p>
            <a href="#" class="btn btn-primary">어딘가로 가기</a>
        </div>
    </div>

</div>
</body>
</html>
```

## 다크 카드

다크 모드에서 카드를 표시하는 데 별도의 조치가 필요하지 않다는 것을 확인할 수 있습니다.

```js
<div class="card" style="width: 18rem;">
    <img src="https://imageio.forbes.com/specials-images/imageserve/5d35eacaf1176b0008974b54/0x0.jpg?format=jpg&crop=4560,2565,x790,y784,safe&height=900&width=1600&fit=bounds"
         class="card-img-top" alt="...">
    <div class="card-body">
        <h5 class="card-title">카드 제목</h5>
        <p class="card-text">카드 제목을 바탕으로 빠르게 예제 텍스트 작성 및 카드 콘텐츠의 대부분 형성합니다.</p>
        <a href="#" class="btn btn-primary">어딘가로 가기</a>
    </div>
</div>
```

<div class="content-ad"></div>

## 화이트 카드

다크 모드를 사용할 때, data-bs-theme="light"를 사용하여 일부 요소를 라이트 모드로 변경할 수 있습니다.

```js
<div class="card" style="width: 18rem;" data-bs-theme="light">
    <img src="https://imageio.forbes.com/specials-images/imageserve/5d35eacaf1176b0008974b54/0x0.jpg?format=jpg&crop=4560,2565,x790,y784,safe&height=900&width=1600&fit=bounds"
         class="card-img-top" alt="...">
    <div class="card-body">
        <h5 class="card-title">카드 제목</h5>
        <p class="card-text">카드 제목에 대한 빠른 예제 텍스트 및 카드 콘텐츠의 대부분을 작성하세요.</p>
        <a href="#" class="btn btn-primary">어딘가로 이동</a>
    </div>
</div>
```

## 배경색 변경

<div class="content-ad"></div>

CSS 변수를 사용하여 배경색을 변경하는 것이 가능하며, Bootstrap이 제공하는 편의성을 활용할 수 있습니다.

```js
<style>
    body {
        background-color: var(--bs-primary);
        /*background-color: var(--bs-warning);*/
    }
</style>
```

![이미지1](/assets/img/2024-07-22-BootstrapFundamentalsDarkMode10_1.png)

![이미지2](https://miro.medium.com/v2/resize:fit:400/0*nSpT33ehx3SNqVFw.gif)