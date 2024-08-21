---
title: "웹사이트에서 그라디언트 텍스트 사용하는 5가지 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-26 11:45
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Gradient text for your website in 5 methods"
link: "https://dev.to/axorax/gradient-text-for-your-website-in-5-methods-4fj9"
isUpdated: true
---




이 글에서는 CSS를 사용하여 그라데이션 텍스트를 만드는 몇 가지 방법을 소개하겠습니다. CSS를 사용하지 않는 방법도 포함되어 있습니다. 이미 한 가지 방법에 익숙할 수도 있습니다.

메서드 - 1, 2 및 3에 대한 HTML

```js
<h1>Sub to Axorax on YT!</h1>
```

## 메서드 - 1 (CSS)

<div class="content-ad"></div>

```css
h1 {
  background: -webkit-linear-gradient(#e28bfc, #8bb8fc);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

## 방법 - 2 (CSS)

```css
h1 {
  background: linear-gradient(#e28bfc, #8bb8fc);
  background-clip: text;
  color: transparent;
}
```

## 방법 - 3 (CSS clip-path)

<div class="content-ad"></div>

```js
h1 {
  position: relative;
  display: inline-block;
}

h1::before {
  content: "YouTube에서 Axorax를 구독하세요!";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(#e28bfc, #8bb8fc);
  -webkit-background-clip: text;
  color: transparent;
  clip-path: text;
}
```

## 방법 - 4 (SVG)

```js
<svg width="100%" height="100%">
  <defs>
    <linearGradient id="gradient" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" style="stop-color:#e28bfc; stop-opacity:1" />
      <stop offset="100%" style="stop-color:#8bb8fc; stop-opacity:1" />
    </linearGradient>
  </defs>
  <text x="10" y="50" font-size="72" fill="url(#gradient)">그라데이션 텍스트</text>
</svg>
```

## 방법 - 5 (HTML canvas)

<div class="content-ad"></div>


종말`스타일
`를`마크다운
``로`변경해보세요!