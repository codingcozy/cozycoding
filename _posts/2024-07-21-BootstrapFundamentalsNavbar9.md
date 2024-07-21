---
title: "기초부터 배우는 Bootstrap  네비게이션 바 만들기 9"
description: ""
coverImage: "/assets/img/2024-07-21-BootstrapFundamentalsNavbar9_0.png"
date: 2024-07-21 11:22
ogImage: 
  url: /assets/img/2024-07-21-BootstrapFundamentalsNavbar9_0.png
tag: Tech
originalTitle: "Bootstrap Fundamentals  Navbar 9"
link: "https://medium.com/@tomas-svojanovsky/bootstrap-fundamentals-navbar-9-54187ff0f604"
---


<img src="/assets/img/2024-07-21-BootstrapFundamentalsNavbar9_0.png" />

## 간단한 바

`.navbar-expand-md` 클래스로 일반 메뉴를 표시할 뷰포트 너비 및 햄버거 메뉴를 표시할 때를 제어할 수 있습니다.

```js
<nav class="navbar navbar-expand-md bg-light">
<div class="container">
<a href="#" class="navbar-brand">Navbar</a>
<button
  type="button"
  data-bs-toggle="collapse"
  data-bs-target="#navbarNav"
  class="navbar-toggler"
>
  <span class="navbar-toggler-icon"></span>
</button>

<div class="collapse navbar-collapse" id="navbarNav">
<ul class="navbar-nav ms-auto">
<li class="nav-item">
<a href="#" class="nav-link active">Home</a>
</li>
<li class="nav-item">
<a href="#about" class="nav-link">About</a>
</li>
<li class="nav-item">
<a href="#" class="nav-link">Services</a>
</li>
<li class="nav-item">
<a href="#" class="nav-link">Contact</a>
</li>
</ul>
</div>
</div>
</nav>
```

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-21-BootstrapFundamentalsNavbar9_1.png)

## 색상 변형

nav 요소에 클래스를 추가하는 것도 가능합니다. 예를 들어, bg-primary를 추가하면 배경색을 빠르게 파란색으로 변경할 수 있습니다.

```js
<nav class="navbar navbar-expand-md bg-primary navbar-dark">
  <div class="container">
    <a href="#" class="navbar-brand">Navbar</a>
    <button
      type="button"
      data-bs-toggle="collapse"
      data-bs-target="#navbarNav"
      class="navbar-toggler"
    >
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item">
          <a href="#" class="nav-link active">Home</a>
        </li>
        <li class="nav-item">
          <a href="#" class="nav-link">About</a>
        </li>
        <li class="nav-item">
          <a href="#" class="nav-link">Services</a>
        </li>
        <li class="nav-item">
          <a href="#" class="nav-link">Contact</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```


<div class="content-ad"></div>


![](/assets/img/2024-07-21-BootstrapFundamentalsNavbar9_2.png)

## 상단 고정

고정 상단 클래스는 스크롤과 상관없이 뷰포트 상단에 고정된 내비게이션 바를 만드는 데 사용됩니다.

```js
<nav class="navbar navbar-expand-md bg-dark navbar-dark fixed-top">
  <div class="container">
    <a href="#" class="navbar-brand">Navbar</a>
    <button
      type="button"
      data-bs-toggle="collapse"
      data-bs-target="#navbarNav"
      class="navbar-toggler"
    >
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item">
          <a href="#" class="nav-link active">Home</a>
        </li>
        <li class="nav-item">
          <a href="#about" class="nav-link">About</a>
        </li>
        <li class="nav-item">
          <a href="#" class="nav-link">Services</a>
        </li>
        <li class="nav-item">
          <a href="#" class="nav-link">Contact</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-21-BootstrapFundamentalsNavbar9_3.png" />

## 네비게이션 바에서의 폼

부트스트랩에서 네비게이션 바에서의 폼은 네비게이션 바 내에 표준 HTML 폼 요소를 통합하여 작동합니다. 이를 통해 검색 상자 또는 다른 폼 입력을 네비게이션 영역 내에 직접 포함할 수 있습니다.

기억하세요, 다른 일반 요소들과 똑같은 방법으로 이 작업을 수행할 수 있습니다.

<div class="content-ad"></div>


# 네비게이션 바


<nav class="navbar navbar-expand-md bg-warning">
  <div class="container">
    <a href="#" class="navbar-brand">Navbar</a>
    <form class="d-flex" role="search">
      <input type="text" class="form-control me-2" />
      <button class="btn btn-dark">Search</button>
    </form>
  </div>
</nav>


이미지
![화면 캡쳐](/assets/img/2024-07-21-BootstrapFundamentalsNavbar9_4.png)


이미지
![애니메이션](https://miro.medium.com/v2/resize:fit:400/0*vxGO68y7ufKlz9Tl.gif)
