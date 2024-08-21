---
title: "Bootstrap 기본  사이징, 테두리 및 그림자 이해하기 6"
description: ""
coverImage: "/assets/img/2024-07-19-BootstrapFundamentalsSizingBordersandShadows6_0.png"
date: 2024-07-19 11:21
ogImage: 
  url: /assets/img/2024-07-19-BootstrapFundamentalsSizingBordersandShadows6_0.png
tag: Tech
originalTitle: "Bootstrap Fundamentals  Sizing, Borders and Shadows 6"
link: "https://medium.com/@tomas-svojanovsky/bootstrap-fundamentals-sizing-borders-and-shadows-6-5eb971b96db4"
isUpdated: true
---





![이미지](/assets/img/2024-07-19-BootstrapFundamentalsSizingBordersandShadows6_0.png)

## 너비

이러한 클래스들은 div의 너비를 부모 컨테이너의 %로 설정합니다.

- 너비 25%: 부모 컨테이너의 너비의 25%인 녹색 상자
- 너비 50%: 부모 컨테이너의 너비의 50%인 녹색 상자
- 너비 75%: 부모 컨테이너의 너비의 75%인 녹색 상자
- 너비 100%: 부모 컨테이너의 너비의 100%인 녹색 상자
- 너비 자동: 내용에 따라 자동으로 조정되는 너비를 가진 녹색 상자


<div class="content-ad"></div>


<div class="bg-success text-white p-3 w-25">Width 25%</div>
<div class="bg-success text-white p-3 w-50">Width 50%</div>
<div class="bg-success text-white p-3 w-75">Width 75%</div>
<div class="bg-success text-white p-3 w-100">Width 100%</div>
<div class="bg-success text-white p-3 w-auto">Width Auto</div>


![Image](/assets/img/2024-07-19-BootstrapFundamentalsSizingBordersandShadows6_1.png)

## 높이

이러한 클래스는 부모 컨테이너 높이의 %를 설정합니다.

<div class="content-ad"></div>

- 높이 25%: 높이가 75px 인 파란색 상자
- 높이 50%: 높이가 150px 인 파란색 상자
- 높이 75%: 높이가 225px 인 파란색 상자
- 높이 100%: 높이가 300px 인 파란색 상자
- 높이 자동: 내용에 따라 자동으로 조정되는 높이를 가진 파란색 상자

```js
<div style="height: 300px; border: 1px solid #333">
    <div class="bg-primary text-white d-inline-block h-25">높이 25%</div>
    <div class="bg-primary text-white d-inline-block h-50">높이 50%</div>
    <div class="bg-primary text-white d-inline-block h-75">높이 75%</div>
    <div class="bg-primary text-white d-inline-block h-100">
      높이 100%
    </div>
    <div class="bg-primary text-white d-inline-block h-auto">
      높이 자동
    </div>
</div>
```

<img src="/assets/img/2024-07-19-BootstrapFundamentalsSizingBordersandShadows6_2.png" />

## 테두리

<div class="content-ad"></div>

제공된 코드는 다른 테두리 스타일을 가진 div 요소 시리즈를 생성합니다.

- 보통: 여백, 하단 여백, 연한 배경색 및 모든 면에 테두리가 있는 상자
- 상단 테두리: 여백, 하단 여백, 연한 배경색 및 위쪽에만 테두리가 있는 상자
- 하단 테두리: 여백, 하단 여백, 연한 배경색 및 아래쪽에만 테두리가 있는 상자
- 왼쪽 테두리: 여백, 하단 여백, 연한 배경색 및 왼쪽에만 테두리가 있는 상자
- 오른쪽 테두리: 여백, 하단 여백, 연한 배경색 및 오른쪽에만 테두리가 있는 상자

```js
<div class="p-3 mb-2 bg-light border">보통</div>
<div class="p-3 mb-2 bg-light border-top">상단 테두리</div>
<div class="p-3 mb-2 bg-light border-bottom">하단 테두리</div>
<div class="p-3 mb-2 bg-light border-start">오른쪽 테두리</div>
<div class="p-3 mb-2 bg-light border-end">왼쪽 테두리</div>
```

<img src="/assets/img/2024-07-19-BootstrapFundamentalsSizingBordersandShadows6_3.png" />

<div class="content-ad"></div>

## 테두리 색상

부트스트랩은 다양한 테두리 색상 클래스를 제공하여 요소의 테두리를 쉽게 사용자 정의할 수 있도록 합니다.

```js
<div class="p-3 mb-2 bg-light border border-primary">Primary</div>
<div class="p-3 mb-2 bg-light border border-secondary">Secondary</div>
<div class="p-3 mb-2 bg-light border border-success">Success</div>
<div class="p-3 mb-2 bg-light border border-info">Info</div>
<div class="p-3 mb-2 bg-light border border-warning">Warning</div>
<div class="p-3 mb-2 bg-light border border-danger">Danger</div>
<div class="p-3 mb-2 bg-light border border-light">Light</div>
<div class="p-3 mb-2 bg-light border border-dark">Dark</div>
<div class="p-3 mb-2 bg-light border border-white">White</div>
```

![테두리 색상](/assets/img/2024-07-19-BootstrapFundamentalsSizingBordersandShadows6_4.png)

<div class="content-ad"></div>

## 테두리 반경

테두리 반경 클래스를 사용하면 요소에 둥근 모서리를 쉽게 추가할 수 있어 디자인에 부드럽고 현대적인 느낌을 줄 수 있어요.

```js
<div class="p-3 mb-2 bg-light border rounded">둥근</div>
<div class="p-3 mb-2 bg-light border rounded-top">위 둥근</div>
<div class="p-3 mb-2 bg-light border rounded-bottom">아래 둥근</div>
<div class="p-3 mb-2 bg-light border rounded-end">오른쪽 둥근</div>
<div class="p-3 mb-2 bg-light border rounded-start">왼쪽 둥근</div>
<div class="p-3 mb-2 bg-light w-25 border rounded-circle">원형 둥근</div>
<div class="p-3 mb-2 bg-light border rounded-1">둥근 1</div>
<div class="p-3 mb-2 bg-light border rounded-2">둥근 2</div>
<div class="p-3 mb-2 bg-light border rounded-3">둥근 3</div>
<div class="p-3 mb-2 bg-light border rounded-4">둥근 4</div>
<div class="p-3 mb-2 bg-light border rounded-5">둥근 5</div>
```

<img src="/assets/img/2024-07-19-BootstrapFundamentalsSizingBordersandShadows6_5.png" />

<div class="content-ad"></div>

## 그림자

그림자는 웹 페이지의 시각적 매력과 사용성을 크게 향상시킬 수 있는 필수 디자인 요소입니다. 그림자는 깊이를 제공하고 요소들을 두드러지게 만들며 다른 구성 요소들 사이에 계층 구조를 만들어줍니다.

```js
<div class="p-3 mb-5 shadow-none">그림자 없음</div>
<div class="p-3 mb-5 shadow-sm">작은 그림자</div>
<div class="p-3 mb-5 shadow">일반 그림자</div>
<div class="p-3 mb-5 shadow-lg">큰 그림자</div>
```

![그림자](/assets/img/2024-07-19-BootstrapFundamentalsSizingBordersandShadows6_6.png)

<div class="content-ad"></div>


![image](https://miro.medium.com/v2/resize:fit:400/0*3mkjC-rhBla8JYjr.gif)
