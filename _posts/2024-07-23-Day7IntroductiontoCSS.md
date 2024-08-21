---
title: "Day 7 CSS 입문 - 기초부터 배우는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-23 21:30
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Day 7 Introduction to CSS"
link: "https://dev.to/dipakahirav/day-7-introduction-to-css-2n2k"
isUpdated: true
---




HTML 및 CSS 마스터하기 여정 7일차에 오신 것을 환영합니다! 오늘은 CSS(계층화 스타일 시트)를 소개할 것입니다. CSS는 HTML 콘텐츠를 스타일링하는 데 사용되는 언어입니다. 이 글을 마치면 CSS의 기본 개념을 이해하고 웹 페이지에 스타일을 적용하는 방법을 이해할 수 있을 것입니다.

제 유튜브 채널을 구독해주시면 채널을 지원하고 웹 개발 튜토리얼을 더 많이 받을 수 있습니다.

#### CSS란?

CSS는 Cascading Style Sheets의 약자입니다. 이는 HTML로 작성된 문서의 표현을 기술하는 스타일시트 언어입니다. CSS는 웹 페이지의 레이아웃, 색상, 글꼴 및 전반적인 외관을 제어합니다.

<div class="content-ad"></div>

#### HTML에 CSS 포함하는 방법

HTML 문서에 CSS를 포함하는 세 가지 방법이 있습니다:

- 인라인 CSS: HTML 태그 내에서 스타일 속성을 사용하는 방법.
- 내부 CSS: HTML 문서의 `head` 섹션 내에서 `style` 태그를 사용하는 방법.
- 외부 CSS: `link` 태그를 사용하여 외부 CSS 파일에 링크하는 방법.

각 방법을 알아보겠습니다.

<div class="content-ad"></div>

1. 인라인 CSS:

```js
   <p style="color: blue; font-size: 16px;">이것은 인라인 스타일이 적용된 단락입니다.</p>
```

2. 내부 CSS:

```js
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>내부 CSS 예제</title>
       <style>
           p {
               color: blue;
               font-size: 16px;
           }
       </style>
   </head>
   <body>
       <p>이것은 내부 스타일이 적용된 단락입니다.</p>
   </body>
   </html>
```

<div class="content-ad"></div>

3. 외부 CSS:

다음 내용으로 styles.css 파일을 생성하세요:

```css
   p {
       color: blue;
       font-size: 16px;
   }
```

HTML 문서에서 외부 CSS 파일을 연결하세요:

<div class="content-ad"></div>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>External CSS Example</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <p>This is an externally-styled paragraph.</p>
</body>
</html>


#### CSS Syntax

CSS는 선택자(selectors)와 선언(declarations)으로 구성됩니다. 선택자는 스타일을 적용하려는 HTML 요소를 가리키며, 선언은 스타일 속성과 값을 정의합니다.


selector {
    property: value;
}


<div class="content-ad"></div>

예를 들어:

```js
p {
    color: blue;
    font-size: 16px;
}
```

### 기본 CSS 선택자

1.Element Selector: 특정 유형의 모든 요소를 대상으로합니다.

<div class="content-ad"></div>

```css
 p {
     color: blue;
 }
```

2. Class 선택자: 특정 클래스 속성을 가진 요소를 대상으로 합니다. 점(.) 다음에 클래스 이름을 사용하세요.

```css
 .highlight {
     background-color: yellow;
 }
```

```html
 <p class="highlight">이것은 강조된 단락입니다.</p>
```

<div class="content-ad"></div>

3.ID 선택자: 특정 ID 속성을 가진 단일 요소를 대상으로합니다. 해시(#) 다음에 ID 이름을 사용하십시오.

```js
#unique {
   font-weight: bold;
}
```

```js
<p id="unique">이것은 유일하게 스타일이 적용된 문단입니다.</p>
```

#### CSS를 사용하여 텍스트 스타일링하기

<div class="content-ad"></div>

여기 일반 텍스트 스타일링 속성 몇 가지가 있습니다:

1. 색상: 텍스트 색상을 설정합니다.

```js
   p {
       color: red;
   }
```

2. 글꼴 패밀리: 텍스트의 글꼴을 설정합니다.

<div class="content-ad"></div>

```js
   p {
       font-family: Arial, sans-serif;
   }
```

3. 글꼴 크기: 텍스트의 크기를 설정합니다.

```js
   p {
       font-size: 18px;
   }
```

4. 텍스트 정렬: 텍스트를 정렬합니다.

<div class="content-ad"></div>

```css
   p {
       text-align: center;
   }
```

#### 스타일이 적용된 HTML 문서 만들기

CSS 스타일이 적용된 HTML 문서를 만들어봅시다:

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS 스타일링 예제</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
        }
        h1 {
            color: navy;
            text-align: center;
        }
        p {
            color: gray;
            font-size: 16px;
        }
        .highlight {
            background-color: yellow;
        }
        #unique {
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>CSS 스타일링에 오신 것을 환영합니다.</h1>
    <p>내부 CSS로 스타일이 적용된 문단입니다.</p>
    <p class="highlight">강조된 문단입니다.</p>
    <p id="unique">특별히 스타일이 적용된 문단입니다.</p>
</body>
</html>
```

<div class="content-ad"></div>

#### 요약

본 블로그 포스트에서는 CSS를 소개하고 HTML 콘텐츠에 적용하는 방법을 알아보았습니다. HTML에 CSS를 포함하는 세 가지 방법, CSS의 기본 구문, 일반 선택자 및 기본 텍스트 스타일링 속성에 대해 다뤘습니다.

## 시리즈 색인

CSS의 박스 모델과 레이아웃 속성을 더 자세히 살펴볼 Day 8을 기대해주세요. 즐거운 코딩 되세요!

<div class="content-ad"></div>

더 많은 웹 개발 자습서와 팁을 보려면 저를 팔로우해 주세요. 아래 댓글이나 질문을 자유롭게 남겨 주세요!

### 팔로우 및 구독:

- 웹사이트: Dipak Ahirav
- 이메일: dipaksahirav@gmail.com
- YouTube: devDive with Dipak
- LinkedIn: Dipak Ahirav