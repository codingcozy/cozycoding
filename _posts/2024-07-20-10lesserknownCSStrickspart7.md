---
title: "알면 유용한, 잘 알려지지 않은 10가지 CSS 트릭 - Part 7"
description: ""
coverImage: "/assets/img/2024-07-20-10lesserknownCSStrickspart7_0.png"
date: 2024-07-20 11:21
ogImage: 
  url: /assets/img/2024-07-20-10lesserknownCSStrickspart7_0.png
tag: Tech
originalTitle: "10 lesser known CSS tricks part 7"
link: "https://medium.com/@creativebyte/10-lesser-known-css-tricks-part-7-4a6f3b17e97c"
isUpdated: true
---




<img src="/assets/img/2024-07-20-10lesserknownCSStrickspart7_0.png" />

안녕하세요! 10가지 잘 알려지지 않은 CSS 트릭 시리즈의 일곱 번째 시간입니다. 이미 알고 계신 분도 있겠지만, 처음 오신 분들에게... 안녕하세요! 이전 포스트를 아직 보지 못한 분들을 위해 10가지 잘 알려지지 않은 CSS 트릭을 준비했습니다. (지금까지 60가지를 소개했고, 이번 포스트 이후로는 70가지 트릭을 소개할 예정입니다.) 웹 디자인 게임을 향상시키는 데 도움이 될 것입니다.

## 01. 수직 텍스트를 위한 text-orientation

text-orientation 속성을 사용하여 텍스트를 수직으로 회전시킬 수 있습니다.

<div class="content-ad"></div>

```css
.vertical-text {
  text-orientation: upright;
}
```

## 02. 작은 대문자를 위한 font-variant

font-variant 속성을 사용하여 텍스트에 작은 대문자를 적용합니다.

```css
.small-caps {
  font-variant: small-caps;
}
```

<div class="content-ad"></div>

## 03. 백그라운드 분할에 대한 상자 장식 변경

여러 줄에 걸쳐 분할되는 요소의 백그라운드를 제어하려면 box-decoration-break를 사용하십시오.

```js
.split-background {
  box-decoration-break: clone;
}
```

## 04. 특정 포커스 스타일링을 위한 :focus-visible

<div class="content-ad"></div>

포커스된 요소에 스타일을 적용하되, 포커스가 마우스 클릭으로 제공되지 않을 때에만 적용하세요.

```js
input:focus-visible {
  outline: 2px solid blue;
}
```

## 05. 최적의 글꼴 렌더링을 위한 텍스트 렌더링

텍스트 렌더링 속성을 사용하여 텍스트 렌더링을 개선하세요.

<div class="content-ad"></div>

```css
.optimized-text {
  text-rendering: optimizeLegibility;
}
```

## 06. 초기 글자를 사용한 드롭 캡스

블록 수준 요소의 첫 글자를 초기 글자로 스타일링합니다.

```css
p::first-letter {
  font-size: 2em;
}
```

<div class="content-ad"></div>

## 07. 오브젝트를 넘어갈 때의 스크롤링 동작을 제어합니다.

스크롤 컨테이너의 경계를 넘어서 스크롤하는 경우 동작을 제어합니다.

```js
.scroll-container {
  overscroll-behavior: contain;
}
```

## 08. 수직 레이아웃을 위한 writing-mode

<div class="content-ad"></div>

웹 사이트 레이아웃을 변경하고 싶다면 Markdown 형식을 사용하면 표를 이해하기 더 쉬워질 거예요. 

<div class="content-ad"></div>

```js
::cue {
  color: blue;
}   
```

## 10. line-clamp for truncated multiline text

Limit the number of lines shown within an element with the line-clamp property.

```js
.truncated-text {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}  
```

<div class="content-ad"></div>

# 읽어주셔서 감사합니다

- 👏 만화를 50번 탭하여 기사를 공유하는 데 도움을 주세요
- 🌐 소셜 미디어에서 이야기를 공유하세요
- 🤝 Buymecoffee.com에서 한 잔 커피를 기부하세요
- 🔔 팔로우하기: Medium | X | LinkedIn
- 📝 월 5달러로 Medium 멤버십 프로그램에 가입하여, 저와 다른 작가들이 놀라운 작품을 만들어 나갈 수 있도록 도와주세요.