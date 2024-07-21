---
title: "Bootstrap 기본 버튼 이해하기 8"
description: ""
coverImage: "/assets/img/2024-07-21-BootstrapFundamentalsButtons8_0.png"
date: 2024-07-21 11:21
ogImage: 
  url: /assets/img/2024-07-21-BootstrapFundamentalsButtons8_0.png
tag: Tech
originalTitle: "Bootstrap Fundamentals  Buttons 8"
link: "https://medium.com/towardsdev/bootstrap-fundamentals-buttons-8-dca0e02ade4a"
---



![Button Styles](/assets/img/2024-07-21-BootstrapFundamentalsButtons8_0.png)

## Styles

다양한 색상의 멋진 버튼을 손쉽게 얻을 수 있습니다. 부트스트랩은 동일한 네이밍 컨벤션을 사용하므로 모든 프라이머리는 기본적으로 파란색이며, 석세스는 녹색 등등 입니다.

```js
<button class="btn btn-primary">Primary</button>
<button class="btn btn-secondary">Secondary</button>
<button class="btn btn-success">Success</button>
<button class="btn btn-danger">Danger</button>
<button class="btn btn-warning">Warning</button>
<button class="btn btn-info">Info</button>
<button class="btn btn-light">Light</button>
<button class="btn btn-dark">Dark</button>
<button class="btn btn-link">Link</button>
```

<div class="content-ad"></div>

```html
![Button Sizes](/assets/img/2024-07-21-BootstrapFundamentalsButtons8_1.png)

## Button Sizes

If needed, we can change the default size to a smaller button or a bigger one.

<button class="btn btn-primary">Regular</button>
<button class="btn btn-success btn-sm">Small</button>
<button class="btn btn-danger btn-lg">Large</button>

<div class="content-ad"></div>

<img src="/assets/img/2024-07-21-BootstrapFundamentalsButtons8_2.png" />

## 버튼 상태

버튼을 비활성화하거나 활성 상태로 설정하여 더욱 눈에 띄게 만들 수 있습니다.

<button class="btn btn-primary">일반</button>
<button class="btn btn-primary disabled">비활성화</button>
<button class="btn btn-primary active">활성화</button>

<div class="content-ad"></div>

<img src="/assets/img/2024-07-21-BootstrapFundamentalsButtons8_3.png" />

## Outline Buttons

이러한 종류의 버튼은 배경색이 없고 테두리 및 텍스트 색상은 변형에 의해 결정됩니다.

<button class="btn btn-outline-primary">주요</button>
<button class="btn btn-outline-secondary">보조</button>
<button class="btn btn-outline-success">성공</button>
<button class="btn btn-outline-danger">위험</button>
<button class="btn btn-outline-warning">경고</button>
<button class="btn btn-outline-info">정보</button>
<button class="btn btn-outline-light">밝음</button>
<button class="btn btn-outline-dark">어두움</button>

<div class="content-ad"></div>

아래는 버튼처럼 보이는 링크를 만들 수 있는 Bootstrap의 멋진 기능 중 하나입니다.

<a href="#" class="btn btn-primary">링크 버튼</a>
<a href="#" class="btn btn-primary btn-lg">링크 버튼</a>
<a href="#" class="btn btn-primary disabled">링크 버튼</a>

<div class="content-ad"></div>

```
![이미지](/assets/img/2024-07-21-BootstrapFundamentalsButtons8_5.png)

## 블록 버튼

.d-grid 클래스를 사용하여 버튼을 쉽게 세로로 배치할 수 있습니다.

```js
<div class="d-grid vstack gap-3">
    <button class="btn btn-primary">제출</button>
    <button class="btn btn-danger">취소</button>
</div>
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-21-BootstrapFundamentalsButtons8_6.png" />

## 버튼 그룹

버튼 그룹은 한 줄에 여러 개의 버튼을 그룹화하는 구성 요소입니다.

.btn-group 내의 버튼은 기본적으로 가로로 표시되어 하나의 연결된 그룹 버튼으로 보입니다. 이는 툴바 같은 요소를 만들거나 관련된 작업을 수행하는 버튼을 구성하는 데 유용합니다.

<div class="content-ad"></div>

```js
<div class="btn-group" role="group">
    <button class="btn btn-primary">왼쪽</button>
    <button class="btn btn-secondary">가운데</button>
    <button class="btn btn-primary">오른쪽</button>
</div>
```

<img src="/assets/img/2024-07-21-BootstrapFundamentalsButtons8_7.png" />

## 버튼 툴바

버튼 툴바는 버튼 그룹을 그룹화하여 여러 버튼 그룹을 포함하는 컨테이너입니다.

<div class="content-ad"></div>

`.btn-toolbar` 클래스를 사용하면 여러 개의 버튼 그룹을 수평 툴바에 배치할 수 있어서 관련 작업을 수행하는 버튼을 일관된 방식으로 구성하고 접근 가능한 인터페이스로 정리할 수 있습니다.

```js
<div class="btn-toolbar" role="toolbar">
    <div class="btn-group me-3" role="group">
        <button class="btn btn-primary">1</button>
        <button class="btn btn-secondary">2</button>
        <button class="btn btn-primary">3</button>
    </div>

    <div class="btn-group" role="group">
        <button class="btn btn-primary">4</button>
        <button class="btn btn-secondary">5</button>
        <button class="btn btn-primary">6</button>
    </div>
</div>
```

![버튼 톨바 이미지](/assets/img/2024-07-21-BootstrapFundamentalsButtons8_8.png)

## 드롭다운

<div class="content-ad"></div>

드롭다운은 링크 또는 작업 목록의 가시성을 토글할 수있는 구성 요소입니다.

클래스 .dropdown-toggle를 가진 버튼을 클릭하면 데이터-bs-toggle="dropdown" 속성이 드롭다운 메뉴를 표시하고 .dropdown-menu 요소 내의 링크를 표시합니다. 드롭다운 구성 요소는 링크나 작업 목록을 조직화하여 콤팩트하고 사용자 친화적인 방식으로 표시하는 데 유용합니다.

```js
<div class="dropdown">
    <button
      class="btn btn-secondary dropdown-toggle"
      data-bs-toggle="dropdown"
    >
      드롭다운 버튼
    </button>
    <ul class="dropdown-menu">
        <li><a href="#" class="dropdown-item">링크 1</a></li>
        <li><a href="#" class="dropdown-item">링크 2</a></li>
        <li><a href="#" class="dropdown-item">링크 3</a></li>
    </ul>
</div>
```

<img src="/assets/img/2024-07-21-BootstrapFundamentalsButtons8_9.png" />

<div class="content-ad"></div>

![image](https://miro.medium.com/v2/resize:fit:400/0*NKmjwgPqkxbQliac.gif)