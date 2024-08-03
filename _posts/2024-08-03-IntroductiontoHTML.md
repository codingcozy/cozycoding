---
title: "HTML 기초 완벽 가이드 웹 개발을 시작하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-IntroductiontoHTML_0.png"
date: 2024-08-03 19:21
ogImage: 
  url: /assets/img/2024-08-03-IntroductiontoHTML_0.png
tag: Tech
originalTitle: "Introduction to HTML"
link: "https://dev.to/sudhanshu_developer/introduction-to-html-25ji"
---


HTML은 웹 페이지를 만드는 데 사용되는 표준 언어입니다. 웹 페이지의 구조를 제공하고 텍스트, 이미지, 링크 등 다양한 요소를 포함할 수 있도록 개발자들에게 가능해줍니다. HTML 요소는 웹 페이지의 구성요소이며 문서의 내용과 구조를 정의합니다.

![HTML](/assets/img/2024-08-03-IntroductiontoHTML_0.png)

HTML 문서의 기본 구조

```html
<!DOCTYPE html>
<html>
<head>
  <title>Welcome to Sudhanshu Developer Page.</title>
</head>
<body>
  <h1>Welcome to Sudhanshu Developer</h1>
  <p>This is a Paragraph of text.</p>
</body>
</html>
```

<div class="content-ad"></div>

중요한 HTML 요소들

`html` 요소는 HTML 문서의 루트 요소입니다. 다른 모든 요소는 이 요소 내에 중첩됩니다.

```js
<!DOCTYPE html>
<html>
  <!-- 문서 내용을 이곳에 작성합니다 -->
</html>
```

`head` 요소에는 문서에 대한 메타 정보, 제목, 스크립트 및 스타일시트에 대한 링크가 포함됩니다.

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html>
<head>
  <title>Welcome to Sudhanshu Developer Page.</title>
</head>
<body>
  <!-- 여기에 본문 내용이 들어갑니다 -->
</body>
</html>
```

`title` 요소는 웹페이지 제목을 설정하며, 브라우저 탭에 표시됩니다.

```js
<!DOCTYPE html>
<html>
<head>
  <title>Welcome to Sudhanshu Developer Page.</title>
</head>
<body>
</body>
</html>
```

`link` 요소는 HTML 문서에 외부 리소스(예: 스타일시트)를 연결합니다.

<div class="content-ad"></div>


```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="styles.css">
  <title>Welcome to Sudhanshu Developer Page.</title>
</head>
<body>
</body>
</html>
```

`script` 요소는 JavaScript 코드를 포함하거나 참조합니다.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Welcome to Sudhanshu Developer Page.</title>
</head>
<body>
  <script src="script.js"></script>
</body>
</html>
```

`body` 요소에는 HTML 문서의 콘텐츠인 텍스트, 이미지 및 다른 요소가 포함됩니다.


<div class="content-ad"></div>


<!DOCTYPE html>
<html>
<head>
  <title>Welcome to Sudhanshu Developer Page.</title>
</head>
<body>
  <h1>Welcome to Sudhanshu Developer Page.</h1>
  <p>This is a paragraph of text.</p>
</body>
</html>


예시:

먼저 index.html 파일을 만들고 기본 코드를 작성한 후, 이 파일을 Google Chrome 또는 웹 브라우저에서 열어보세요.

index.html


<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />

    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>Welcome to Sudhanshu Developer Blog..!</title>
  </head>

  <body>
    <h1>Welcome to Sudhanshu Developer Blog..!</h1>
    <p>It is a basic HTML page </p>
    <p>Lorem Ipsum is simply dummy text of the printing and typesetting industry.</p>
  </body>
</html>
```

Output:

<img src="/assets/img/2024-08-03-IntroductiontoHTML_1.png" />
