---
title: "초보자를 위한 HTML 메타 태그 쉽게 이해하기 3"
description: ""
coverImage: "/assets/img/2024-07-26-HTMLforBeginnersMetatags3_0.png"
date: 2024-07-26 11:49
ogImage: 
  url: /assets/img/2024-07-26-HTMLforBeginnersMetatags3_0.png
tag: Tech
originalTitle: "HTML for Beginners Meta tags 3"
link: "https://medium.com/@tomas-svojanovsky/html-for-beginners-meta-tags-3-e3266808bf04"
---



![이미지](/assets/img/2024-07-26-HTMLforBeginnersMetatags3_0.png)

## Charset

이 요소는 HTML 문서의 문자 인코딩을 지정합니다.

```js
<meta charset="UTF-8"/>
```

<div class="content-ad"></div>

## 뷰포트

이 태그는 웹 페이지의 레이아웃 및 확대/축소를 다양한 디바이스에서 제어합니다, 특히 모바일 디바이스에서.

이 태그는 웹 페이지가 반응형으로 디자인되어 다양한 화면 크기 및 해상도에 맞게 조절되어 데스크톱부터 스마트폰 및 태블릿까지 다양한 디바이스에서 최적의 뷰잉 경험을 제공할 수 있도록 도와줍니다.

컨텐츠 속성의 width=device-width 부분은 뷰포트의 너비가 디바이스 화면의 너비와 동일하도록 지정합니다. 이는 웹 페이지가 디바이스 독립적인 픽셀 단위에서 화면의 너비와 일치하도록 함을 의미합니다.

<div class="content-ad"></div>

`<table>` 태그를 Markdown 형식으로 변경하십시오.

콘텐트 어트리뷰트의 initial-scale=1.0 부분은 페이지가 처음로드 될 때의 초기 확대 수준을 설정합니다. 초기 스케일이 1.0인 경우 페이지가 1:1 비율로 표시되며, 줌이 없음을 의미합니다.

```js
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

## 키워드

이 태그는 웹 페이지의 콘텐츠와 관련있는 키워드 목록을 지정하는 데 사용됩니다. 이러한 키워드는 검색 엔진이 페이지의 주제와 테마를 이해하는 데 도움이 될 수 있습니다.

<div class="content-ad"></div>


<meta name="keywords"…
