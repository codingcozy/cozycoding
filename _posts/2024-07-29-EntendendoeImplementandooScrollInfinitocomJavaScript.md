---
title: "JavaScript로 무한 스크롤 이해하고 구현하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-29 13:41
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Entendendo e Implementando o Scroll Infinito com JavaScript"
link: "https://dev.to/josimar_canejo/entendendo-e-implementando-o-scroll-infinito-com-javascript-4526"
---


#### 무한 스크롤이란?

이미 알고 계시겠지만, 온라인 상점 같은 웹 사이트에서 페이지를 아래로 스크롤하면 제품들이 계속해서 나타나는 것을 본 적이 있을 겁니다. 또한 endless.horse의 경우, 간단해 보일 수 있지만 "무한 스크롤" 기능을 효과적으로 보여주는 훌륭한 예시입니다. 사이트에 접속하면 말이 보이지만 페이지를 아래로 스크롤하면 말의 다리가 무한정으로 자라나는 것을 발견할 수 있습니다. 결국 말의 발 부분에 도달할 수 없다는 농담입니다.

#### 무한 스크롤 구현하기

이 기능은 현대적인 개발에서 널리 사용됩니다. 무한 스크롤은 주요 소셜 미디어 플랫폼인 Twitter, Facebook, 특히 Instagram에서 활발하게 사용되며 페이지를 스크롤할수록 콘텐츠가 끊임없이 계속되는 것을 볼 수 있습니다.

<div class="content-ad"></div>

이 글에서는 이 기능을 기본적으로 구현하는 방법을 보여드리겠습니다. 그러나 이 접근 방식은 긴 쿼리 문제, 캐시 구현 및 프로덕션 애플리케이션에 필요한 다른 솔루션과 같은 문제를 다루지 않습니다. 그래도 이것은 이 리소스를 구현하는 방법을 이해하는 데 좋은 출발점입니다.

먼저, 무한 스크롤 기능을 추가할 위치를 결정하십시오. 글 게시물 목록인가요 아니면 이미지 목록인가요? 또한 데이터가 어디에서 가져올지 결정해야 합니다. 이 예제에서는 기본 API인 Random Fox API의 이미지를 사용할 것입니다.

HTML 파일을 생성하고 랜덤한 여우 이미지를 포함할 컨테이너를 만들어주세요.

```js
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Fox Images</title>
<link rel="stylesheet" href="styles.css">
</head>
<body>
<h1 class="header">Fox Images</h1>
<div class="container"></div>
<script src="script.js"></script>
</body>
</html>
```

<div class="content-ad"></div>

이 예시에서는 스타일 시트를 매우 간단하게 유지할 것입니다.

```js
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}

.header {
  font-size: 32px;
  color: black;
  text-align: center;
  font-family: Verdana, sans-serif;
}

img {
  width: 400px;
  height: 400px;
  margin: 4px;
  object-fit: cover;
}
```

컨테이너를 선택하고 Random Fox API의 URL을 가져옵니다. HTML에서 자바스크립트 파일과 CSS 파일을 링크하는 걸 잊지 마세요.

```js
const container = document.querySelector('.container');
const URL = "https://randomfox.ca/images/";

function getRandomNumber() {
  return Math.floor(Math.random() * 100);
}

function loadImages(numImages = 6) {
  let i = 0;
  while (i < numImages) {
    const img = document.createElement('img');
    img.src = `${URL}${getRandomNumber()}.jpg`;
    container.appendChild(img);
    i++;
  }
}

loadImages();
```

<div class="content-ad"></div>

무한 스크롤 기능을 구현하려면 다음과 같이 이벤트 리스너를 추가해주세요:

```js
window.addEventListener('scroll', () => {
  if (window.scrollY + window.innerHeight >= document.documentElement.scrollHeight) {
    loadImages();
  }
});
```

scrollY와 innerHeight를 더한 값이 scrollHeight보다 크거나 같으면 문서의 끝에 도달한 것이므로 더 많은 이미지를 불러와야 합니다.

이제 페이지가 무한 스크롤 기술로 작동해야 합니다. 여기 도움말을 위한 도전 과제가 있습니다: 다른 API를 사용하여이 프로젝트를 다시 만들어보고 항목을 더 멋지게 표시하기위한 스타일도 구현해보세요. 행운을 빕니다!