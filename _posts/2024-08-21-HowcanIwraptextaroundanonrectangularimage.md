---
title: "비직사각형 이미지 주변으로 텍스트를 감싸는 방법."
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-21 16:58
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "How can I wrap text around a non rectangular image"
link: "https://medium.com/@fixitblog/solved-how-can-i-wrap-text-around-a-non-rectangular-image-d7f64254d1f5"
isUpdated: false
---


주어진 요소 안에서 텍스트를 비직사각형 이미지 주위로 감쌀 수 있을까요?

다양한 국가 지도 주위에 텍스트를 국가의 모양에 맞게 감싸고 싶은데, 국가의 테두리가 곧지 않더라도 텍스트가 항상 국가의 테두리로부터 동일한 거리를 유지하도록 하려고 합니다.

이것이 가능할까요?

# 해결 방법

<div class="content-ad"></div>

위의 내용을 친절한 톤으로 한국어로 번역해 드리겠습니다.

2011년 Tory Lawson이 작성한 블로그 글인 Wrapping text around non-rectangular shapes에 소개된 방법을 사용할 수 있습니다. 해당 방법은 div를 부유시켜 모양의 영역을 블록 처리하여 텍스트를 감쌀 수 있습니다:

## 단계 1: 감쌀 영역 정의

## 단계 2: 목록 생성

## 단계 3: 몇 개의 div 만들기

<div class="content-ad"></div>

```js
<body>
  <div id="wrapper">
     <div class="spacer"><img src="spacer.gif" width="301" height="10"></img></div>
     <div class="spacer"><img src="spacer.gif" width="311" height="10"></img></div>
     <div class="spacer"><img src="spacer.gif" width="318" height="10"></img></div>
  .
  .
  .
     <p>
     Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed vehicula tellus eget 
     magna dignissim a egestas odio luctus. Mauris interdum lorem sed augue venenatis 
     nec consequat magna tempor. Nullam blandit libero libero, eget posuere dolor. Vestibulum 
  .
  .
  .
     </p>
  </div>
</body>
```

```js
#wrapper { 
   width:634px;
   height:428px;
   display:block;
   background-image:url("headshot.jpg");
}
.spacer{
   display:block;
   float:right;
   clear:right;
}
p {
   display:inline;
   color:#FFF;
}
```

그래서 — 답은 "네 — 할 수 있어요" 입니다. 하지만 제가 아는 한 CSS의 "text-wrap" 옵션처럼 "쉬운" 방법은 없습니다.

답변자: Dave


<div class="content-ad"></div>

답변 확인자: Senaida (수정 도와주는 자원 봉사자)

이 답변은 stackoverflow에서 제공된 것이며 cc by-sa 2.5, cc by-sa 3.0 및 cc by-sa 4.0 라이선스에 따라 라이선스가 부여됩니다.