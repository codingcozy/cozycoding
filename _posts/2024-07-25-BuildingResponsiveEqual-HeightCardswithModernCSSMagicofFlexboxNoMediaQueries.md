---
title: "미디어 쿼리 없이 Flexbox로 반응형, 동일 높이 카드 만들기 모던 CSS의 마법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-25 11:40
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Building Responsive, Equal-Height Cards with Modern CSS Magic of Flexbox , No Media Queries"
link: "https://dev.to/jennavisions/building-responsive-equal-height-cards-with-modern-css-magic-of-flexbox-no-media-queries-2h0b"
isUpdated: true
---




## 목차

소개

우리의 목표는 무엇인가요?

시멘틱 HTML5를 사용하여 구조 빌딩

<div class="content-ad"></div>

모던 CSS를 사용하여 스타일 추가하기
- CSS 초기화
- Flexbox를 사용한 카드 레이아웃 디자인
- 카드 이미지 스타일링
- 카드 콘텐츠 스타일링
- 카드 버튼 스타일링
- 호버 효과 추가
- CSS 변수 활용

결론

### 소개

웹 개발자로서, 제품/프로젝트 쇼케이스, 사용자 프로필 또는 블로그 게시물을 위한 카드 컴포넌트를 만들어야 하는 경우가 많습니다. 카드는 어디서나 활용됩니다.

<div class="content-ad"></div>

과거에는 반응형 레이아웃을 만드는 것이 어려웠습니다. 하지만 현대 CSS 기술인 CSS Flexbox의 등장으로 이러한 디자인을 만드는 과정이 상당히 간단해지고 직관적으로 변했습니다.

Flexbox를 이용하면 반응형 레이아웃을 만드는 과정이 간단해집니다. 복잡한 미디어 쿼리를 사용하지 않고도 컨테이너 내 아이템을 쉽게 정렬하고 공간을 조절할 수 있습니다. 이는 정확한 브레이크포인트를 지정하지 않아도 각 디바이스 화면 크기와 방향에 맞춰 레이아웃을 아름답게 조정할 수 있다는 뜻입니다.

### 우리의 목표는 무엇인가요?

우리의 목표는 CSS Flexbox를 활용해 브레이크포인트에 의존하지 않고 같은 높이의 반응형 카드를 만드는 것입니다. 각 카드가 내용 길이에 관계 없이 동일한 높이를 유지하도록하여 다양한 화면 크기에 원활하게 적응하도록 할 것입니다.

<div class="content-ad"></div>

레이아웃을 구성하는 중요한 CSS 속성들이에요:

- display: flex
- align-items
- justify-content
- flex-grow

이제 CSS 플렉스박스의 매력을 탐험해봐요! 카드 구축하기!

### 시맨틱 HTML5를 이용한 구조 구축하기

<div class="content-ad"></div>

```js
<main class="card-container">
<!--   첫 번째 카드 -->
  <article class="card">
    <img src="https://placehold.co/320x240" width="320" height="240" alt="여기에 대체 이미지를 넣어주세요" class="card-image" loading="lazy">
    <h2 class="card-title">제목 1</h2>
    <p class="card-description">Lorem ipsum dolor sit amet consectetur, adipisicing elit. Quod, aliquid ex vel labore fugit dignissimos libero eos hic, fuga, vitae consequuntur quidem.</p>
    <button class="card-button" aria-label="제목 1에 대해 더 읽기">더 보기</button>
  </article>
  <!--   두 번째 카드 -->
  <article class="card">
    <img src="https://placehold.co/320x240" width="320" height="240"  alt="여기에 대체 이미지를 넣어주세요" class="card-image" loading="lazy">
    <h2 class="card-title">제목 2</h2>
    <p class="card-description">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Magnam aperiam consequuntur, saepe at repellat nobis.</p>
    <button class="card-button" aria-label="제목 2에 대해 더 읽기">더 보기</button>
  </article>
  <!--   세 번째 카드 -->
  <article class="card">
    <img src="https://placehold.co/320x240" width="320" height="240"  alt="여기에 대체 이미지를 넣어주세요" class="card-image" loading="lazy">
    <h2 class="card-title">제목 3</h2>
    <p class="card-description">Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolorum reprehenderit at cumque? Architecto numquam nam placeat suscipit!</p>
    <button class="card-button" aria-label="제목 3에 대해 더 읽기">더 보기</button>
  </article>
</main>
```

### 모던 CSS 사용하여 스타일 추가하기

#### CSS 초기화

```js
/* 기본 CSS 초기화 */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

<div class="content-ad"></div>

#### 본문

```js
/* 레이아웃이 페이지의 가로와 세로 중앙에 위치하도록 확실히 함 */
body {
    display: flex; /* 모든 카드를 수직 및 수평 중앙에 배치하기 위해 CSS 플렉스박스 사용 */
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    overflow-x: hidden; /* 수평 스크롤 방지 */
}
```

#### Flexbox를 사용한 Card 레이아웃 설계

```js
/* 카드들 */
.card-container {
    display: flex; /* 각 카드를 중앙에 표시하기 위해 CSS 플렉스박스 사용 */
    justify-content: center;
    align-items: stretch; /* 모든 카드의 높이를 동일하게 유지하기 위해 stretch 사용 */
    gap: 1.5625rem; /* 각 카드 사이의 간격 추가 */
    flex-wrap: wrap;
    padding: 1rem;
    max-width: 100%; /* 컨테이너가 뷰포트 너비를 초과하지 않도록 함 */
}

.card {
    display: flex;
    flex-direction: column;
    width: 20rem;
    background-color: #fff;
    box-shadow: 0 0.25rem 0.5rem rgba(0, 0, 0, 0.4);
    text-align: center;
    text-wrap: balance; /* 내용이 여러 줄에 고르게 분배되어 가독성이 더 좋아짐 */
    overflow: hidden;
}
```  

<div class="content-ad"></div>

### 카드 내부 내용 스타일링

#### 카드 이미지 스타일링

```js
.card-image {
    width: 100%;
    height: auto;
    object-fit: cover;
    margin-bottom: 0.85rem;
}
```

#### 카드 내용 스타일링

<div class="content-ad"></div>

```js
.card-title {
    font-size: 1.25rem;
    padding: 1rem;
    color: #3ca69f;
}

.card-description {
    flex-grow: 1; /* 사용 가능한 공간을 차지할 수 있도록 함으로써 콘텐츠의 길이에 관계없이 높이를 유지함 */
    padding: 0 1rem 1rem;
    font-size: 0.975rem;
    line-height: 1.5;
}
```

#### Card 버튼 스타일링

```js
/* Card 버튼 */
.card-button {
    align-self: center; /* 버튼을 가운데 정렬함 */
    margin: 1rem 0 3rem;
    padding: 0.625rem 1rem;
    font-size: 1rem;
    color: #ffffff;
    background-color: #3ca69f;
    border: none;
    border-radius: 0.3125rem;
    cursor: pointer;
}
```

#### Hover 효과 추가하기
```bash

```

<div class="content-ad"></div>

```js
.card {
    transition: 0.5s ease all;
}

.card-button {
    transition: 0.5s ease all;
}

/* cards hover effect */
.card:hover {
    background-color: #276662;
    color: #ffffff;
}

.card:hover > .card-button {
    background-color: #ffffff;
    color: #276662;
    font-weight: 700;
}

.card:hover > .card-title {
    color: #ffffff;
}
```

#### CSS 변수 사용하기

```js
/* 변수 선언 */
:root {
    --primary-color: #3ca69f;
    --secondary-color: #276662;
    --text-color: #ffffff;
    --shadow-color: rgba(0, 0, 0, 0.4);
    --border-radius: 0.3125rem;
    --spacing: 1rem;
    --transition-duration: 0.5초;
}
```

### 마무리

<div class="content-ad"></div>

#### 모두 함께 모으기

맨 위로 이동