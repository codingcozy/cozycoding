---
title: "CSS 속성과 기능에 대해서 자세히 알고 있어야하는 이유"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 20:08
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "CSS In Details"
link: "https://dev.to/ashutoshsarangi/css-in-details-3hfk"
isUpdated: true
updatedAt: 1724743648941
---


우리 페이지에서 HTML이 어떻게 렌더링되는지

- 브라우저가 HTML을 로드합니다
- HTML을 DOM으로 변환합니다
- 연결된 자원을 가져옵니다
- 브라우저가 CSS를 구문분석합니다 (CSSOM)
- DOM과 CSSOM을 결합합니다
- UI가 그려집니다 (FCP) (첫 번째 콘텐츠 표시)

HTML 요소 유형

주로

<div class="content-ad"></div>

- 블록 수준
- 인라인

CSS 선택자: 

- 유형/속성 선택자
- 클래스 선택자
- ID 선택자
- 범용 선택자 (*)

CSS 상속

<div class="content-ad"></div>

부모체인에 직접 속성을 설정하지 않은 경우에는 상속이 발생합니다. 이 속성에 해당하는 값을 찾을 때까지 부모 체인이 횡단됩니다.

```js
<div class="body">
    <p>이 문단은 상속으로 파란색을 가지게 됩니다.</p>
</div>
<style>
.body{
    color: blue;
}
</style>
```

case 2

```js
<div class="body">
    <p>이 문단은 직접 명시로 빨간색을 가지게 됩니다.</p>
</div>
<style>
p {
color: red;
}
.body{
    color: blue;
}
</style>
```

<div class="content-ad"></div>

case 3

```js
<div class="body">
    <p>This is a Paragraph, but it will have the blue color due to strong Specification</p>
</div>
<style>
p {
    color: red;
}
.body p{
    color: blue;
}
</style>
```

CSS Specificity에 대해 어떻게 설명할까요?

- 브라우저가 어떤 CSS 선언을 적용해야 하는지 결정하는 알고리즘입니다.
- 각 선택자는 계산된 가중치를 가지고, 가장 구체적인 가중치가 적용됩니다. ID 선택자: 1 - 0 - 0, 클래스 선택자: 0 - 1 - 0, 타입 선택자: 0 - 0 - 1.

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경하세요.

<div class="content-ad"></div>

```js
<html>
<div>
<p></p>
</div>
</html>

<style>
html {
    font-size: 16px;
}

div {
font-size: 2em; //16px * 2 = 32px;
}

p {
font-size: 2em; // 32px * 2 = 64px
}
</style>
```

REM:- 루트 폰트 크기에 상대적입니다.

```js
<html>
<div>
<p></p>
</div>
</html>

<style>
html {
    font-size: 16px;
}

div {
font-size: 2rem; //16px * 2 = 32px;
}

p {
font-size: 2rem; // 16px * 2 = 32px
}
</style>
```

%:- % 계산

<div class="content-ad"></div>


```html
<div>
<p></p>
</div>
```

```css
html {
    font-size: 16px;
}

div {
font-size: 120%; //1.2*16 = 19.2px;
}

p {
font-size: 120%; // 1.2 * 19.2 = 23.04px
}
```

**CSS Combinators**

1. Descendent Selector (ul li a)

```html
<ul>
<li><a href='#'></a></li>
</ul>
```

<div class="content-ad"></div>

2. 자식 결합자 (직계 자손) (div.text ` p)

```js
<div>
<div class="text">
   <P>CSS가 여기 적용됩니다<P>
</div>
<div>
  <p>CSS가 적용되지 않습니다</p>
</div>
</div>
```

3. 인접 형제 결합자 (h1 + p)

Note:-

<div class="content-ad"></div>

- h1과 p 모두 동일한 부모 안에 있어야 하며, p는 h1 태그 바로 뒤에 위치해야 함

4. 일반 형제 결합자(p ~ code)

참고:

- 이들은 인접 형제와 같이 즉시 형제가 되어서는 안 됩니다. 그러나 형제가 있어야 합니다.
- 동일한 부모를 가져야 함

<div class="content-ad"></div>

블록 요소 수정자 아키텍처(BEM)

- 재사용 가능한 컴포넌트 및 코드 공유를 도와주는 디자인 방법론

다른 방법론

- OOCSS
- SMACSS
- SUITCVSS
- ATOMIC
- BEM

<div class="content-ad"></div>

블록

- 헤더
- 메뉴
- 입력란
- 체크박스 (독립적인 의미)

요소 (블록의 일부)

- 메뉴 항목
- 목록 항목
- 헤더 제목

<div class="content-ad"></div>

수정자

- 비활성화됨
- 강조됨
- 선택됨
- 노란색

.block__element--modifier 구문

```js
<form class=form>
   <input class='form__input'/>
   <input class="form__input form__input--disabled"/>
   <button class="form__button form__button--large"/>
</form>
```

<div class="content-ad"></div>

박스 모델:- (작업 중)

세부 정보에서 더 많은 정보를 추가해야 합니다.

참고: -

그리드 레이아웃 대 플렉스 레이아웃에 대한 별도의 기사가 있을 것입니다.