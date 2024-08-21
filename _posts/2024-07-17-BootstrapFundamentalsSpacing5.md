---
title: "부트스트랩 기초  Spacing5 이해하기"
description: ""
coverImage: "/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_0.png"
date: 2024-07-17 23:54
ogImage: 
  url: /assets/img/2024-07-17-BootstrapFundamentalsSpacing5_0.png
tag: Tech
originalTitle: "Bootstrap Fundamentals  Spacing 5"
link: "https://medium.com/@tomas-svojanovsky/bootstrap-fundamentals-spacing-5-339f901cb0c0"
isUpdated: true
---





![image](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_0.png)

Spacing is frequently used to create additional space in designs. Bootstrap offers various utility classes to simplify this process.

## Terminology

Consistent values are utilized in all examples. In Bootstrap, you can select values ranging from 1 to 5, each representing a specific rem value.


<div class="content-ad"></div>

- gap-1: 0.25rem
- gap-2: 0.5rem
- gap-3: 1rem
- gap-4: 1.5rem
- gap-5: 3rem

## Margin All

이 클래스들은 요소의 모든 면에 여백을 설정합니다.

```js
<div class="text-bg-dark m-1">Lorem, ipsum dolor.</div>
<div class="text-bg-dark m-2">Lorem, ipsum dolor.</div>
<div class="text-bg-dark m-3">Lorem, ipsum dolor.</div>
<div class="text-bg-dark m-4">Lorem, ipsum dolor.</div>
<div class="text-bg-dark m-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_1.png)

## 위 아래 여백

이 클래스들은 요소의 수평(위 아래) 여백을 설정합니다.

```js
<div class="text-bg-success my-1">Lorem, ipsum dolor.</div>
<div class="text-bg-success my-2">Lorem, ipsum dolor.</div>
<div class="text-bg-success my-3">Lorem, ipsum dolor.</div>
<div class="text-bg-success my-4">Lorem, ipsum dolor.</div>
<div class="text-bg-success my-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>


![Image](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_2.png)

## 좌우 여백

이 클래스들은 요소의 수직(위 아래) 여백을 설정합니다.

```js
<div class="text-bg-secondary mx-1">Lorem, ipsum dolor.</div>
<div class="text-bg-secondary mx-2">Lorem, ipsum dolor.</div>
<div class="text-bg-secondary mx-3">Lorem, ipsum dolor.</div>
<div class="text-bg-secondary mx-4">Lorem, ipsum dolor.</div>
<div class="text-bg-secondary mx-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>


![Margin Top](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_3.png)

## Margin Top

이 클래스들은 요소의 상단 여백을 설정합니다.

```js
<div class="text-bg-danger mt-1">Lorem, ipsum dolor.</div>
<div class="text-bg-danger mt-2">Lorem, ipsum dolor.</div>
<div class="text-bg-danger mt-3">Lorem, ipsum dolor.</div>
<div class="text-bg-danger mt-4">Lorem, ipsum dolor.</div>
<div class="text-bg-danger mt-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_4.png" />

## Margin Bottom

이 클래스들은 요소의 하단 여백을 설정합니다.

```js
<div class="text-bg-warning mb-1">Lorem, ipsum dolor.</div>
<div class="text-bg-warning mb-2">Lorem, ipsum dolor.</div>
<div class="text-bg-warning mb-3">Lorem, ipsum dolor.</div>
<div class="text-bg-warning mb-4">Lorem, ipsum dolor.</div>
<div class="text-bg-warning mb-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_5.png)

## 왼쪽 여백

이 클래스들은 요소의 왼쪽 여백(시작점)을 설정합니다.

```js
<div class="text-bg-info ms-1">Lorem, ipsum dolor.</div>
<div class="text-bg-info ms-2">Lorem, ipsum dolor.</div>
<div class="text-bg-info ms-3">Lorem, ipsum dolor.</div>
<div class="text-bg-info ms-4">Lorem, ipsum dolor.</div>
<div class="text-bg-info ms-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>


![Margin Right](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_6.png)

## 우측 여백

이러한 클래스는 요소의 오른쪽 여백을 설정합니다.

```js
<div class="text-bg-dark me-1">Lorem, ipsum dolor.</div>
<div class="text-bg-dark me-2">Lorem, ipsum dolor.</div>
<div class="text-bg-dark me-3">Lorem, ipsum dolor.</div>
<div class="text-bg-dark me-4">Lorem, ipsum dolor.</div>
<div class="text-bg-dark me-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_7.png" />

## Margin Auto

이 클래스들은 요소를 가운데 정렬하거나 지정된 방향을 기반으로 정렬하기 위해 다양한 방향으로 자동으로 마진을 설정합니다.

```js
<div class="text-bg-warning w-50 m-auto">Lorem, ipsum dolor.</div>
<div class="text-bg-warning w-50 my-auto">Lorem, ipsum dolor.</div>
<div class="text-bg-warning w-50 mx-auto">Lorem, ipsum dolor.</div>
<div class="text-bg-warning w-50 ms-auto">Lorem, ipsum dolor.</div>
<div class="text-bg-warning w-50 me-auto">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>

```html
<img src="/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_8.png" />

## Padding All

이러한 클래스는 요소의 모든 측면에 padding을 설정합니다.

<div class="text-bg-dark my-2 p-1">Lorem, ipsum dolor.</div>
<div class="text-bg-dark my-2 p-2">Lorem, ipsum dolor.</div>
<div class="text-bg-dark my-2 p-3">Lorem, ipsum dolor.</div>
<div class="text-bg-dark my-2 p-4">Lorem, ipsum dolor.</div>
<div class="text-bg-dark my-2 p-5">Lorem, ipsum dolor.</div>

<div class="content-ad"></div>

```
![이미지](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_9.png)

## 상단 및 하단 간격

이러한 클래스는 요소의 수직(상단 및 하단) 패딩을 설정합니다.

```js
<div class="text-bg-success my-2 py-1">Lorem, ipsum dolor.</div>
<div class="text-bg-success my-2 py-2">Lorem, ipsum dolor.</div>
<div class="text-bg-success my-2 py-3">Lorem, ipsum dolor.</div>
<div class="text-bg-success my-2 py-4">Lorem, ipsum dolor.</div>
<div class="text-bg-success my-2 py-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>


![Bootstrap Fundamentals Spacing](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_10.png)

## 좌우 여백

이 클래스들은 요소의 가로(왼쪽 및 오른쪽) 여백을 설정합니다.

```js
<div class="text-bg-danger my-2 px-1">Lorem, ipsum dolor.</div>
<div class="text-bg-danger my-2 px-2">Lorem, ipsum dolor.</div>
<div class="text-bg-danger my-2 px-3">Lorem, ipsum dolor.</div>
<div class="text-bg-danger my-2 px-4">Lorem, ipsum dolor.</div>
<div class="text-bg-danger my-2 px-5">Lorem, ipsum dolor.</div>
```


<div class="content-ad"></div>


![이미지](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_11.png)

## 위쪽 여백

이러한 클래스는 요소의 위쪽 여백을 설정합니다.

```js
<div class="text-bg-primary my-2 pt-1">Lorem, ipsum dolor.</div>
<div class="text-bg-primary my-2 pt-2">Lorem, ipsum dolor.</div>
<div class="text-bg-primary my-2 pt-3">Lorem, ipsum dolor.</div>
<div class="text-bg-primary my-2 pt-4">Lorem, ipsum dolor.</div>
<div class="text-bg-primary my-2 pt-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>


![image](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_12.png)

## Padding Bottom

이 클래스들은 요소의 하단 padding을 설정합니다.

```js
<div class="text-bg-info my-2 pb-1">Lorem, ipsum dolor.</div>
<div class="text-bg-info my-2 pb-2">Lorem, ipsum dolor.</div>
<div class="text-bg-info my-2 pb-3">Lorem, ipsum dolor.</div>
<div class="text-bg-info my-2 pb-4">Lorem, ipsum dolor.</div>
<div class="text-bg-info my-2 pb-5">Lorem, ipsum dolor.</div>
```


<div class="content-ad"></div>


![Bootstrap Fundamentals Spacing](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_13.png)

## 왼쪽 패딩

이 클래스들은 요소의 왼쪽 (시작) 패딩을 설정합니다.

```js
<div class="text-bg-warning my-2 ps-1">Lorem, ipsum dolor.</div>
<div class="text-bg-warning my-2 ps-2">Lorem, ipsum dolor.</div>
<div class="text-bg-warning my-2 ps-3">Lorem, ipsum dolor.</div>
<div class="text-bg-warning my-2 ps-4">Lorem, ipsum dolor.</div>
<div class="text-bg-warning my-2 ps-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_14.png" />

## 패딩 오른쪽

이 클래스들은 요소의 오른쪽(끝) 패딩을 설정합니다.

```js
<div class="text-bg-primary my-2 pe-1">Lorem, ipsum dolor.</div>
<div class="text-bg-primary my-2 pe-2">Lorem, ipsum dolor.</div>
<div class="text-bg-primary my-2 pe-3">Lorem, ipsum dolor.</div>
<div class="text-bg-primary my-2 pe-4">Lorem, ipsum dolor.</div>
<div class="text-bg-primary my-2 pe-5">Lorem, ipsum dolor.</div>
```

<div class="content-ad"></div>

```html
<img src="/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_15.png" />

## 스택

- .vstack 클래스는 요소를 수직으로 쌓을 때 사용됩니다.
- .hstack 클래스는 요소를 수평으로 쌓을 때 사용됩니다.

<div class="vstack gap-3">
    <div class="p-2 bg-info">첫 번째 항목</div>
    <div class="p-2 bg-info">두 번째 항목</div>
    <div class="p-2 bg-info">세 번째 항목</div>
</div>

<div class="hstack gap-5 mt-2">
    <div class="p-2 text-bg-success">첫 번째 항목</div>
    <div class="p-2 text-bg-success">두 번째 항목</div>
    <div class="p-2 text-bg-success">세 번째 항목</div>
</div>

<div class="content-ad"></div>

```
![Bootstrap Fundamentals Spacing](/assets/img/2024-07-17-BootstrapFundamentalsSpacing5_16.png)

![Medium Animation](https://miro.medium.com/v2/resize:fit:400/0*ODEffrlpBLjc4ITx.gif)
