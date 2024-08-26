---
title: "함께 배우는 CSS Clip-Path"
description: ""
coverImage: "/assets/img/2024-08-26-LearnwithmeCSSClip-Path_0.png"
date: 2024-08-26 17:48
ogImage: 
  url: /assets/img/2024-08-26-LearnwithmeCSSClip-Path_0.png
tag: Tech
originalTitle: "Learn with me CSS Clip-Path"
link: "https://medium.com/@ls.megha812/learn-with-me-css-clip-path-679bb60f027b"
isUpdated: false
---


CSS를 항상 어렵게 느꼈던 사람으로서, 올해는 CSS에 좀 더 편안해지기 위해 노력해왔어요. Epic Web Conference에 참석하고 커뮤니티의 멋진 엔지니어들을 만나면서 올 해 제 학습 여정을 Medium에 공유하게 돼서 흥겨웠어요.

이 글은 특히 CSS Clip-path 속성을 사용하여 웹 페이지의 배경에 재미있는 모양과 애니메이션을 만드는 방법에 대해 이야기합니다.

이 연습을 위해, 웹 페이지의 배경으로 고정된 노란색 모양을 만들고 싶었어요.

![이미지](/assets/img/2024-08-26-LearnwithmeCSSClip-Path_0.png)

<div class="content-ad"></div>

원하는 모양을 만들기 위해 순수한 CSS만 사용하고 싶었고, 그래서 clip-path 속성이 큰 도움이 되었습니다.

원하는 clip-path 속성을 직접 코딩하는 것도 가능하지만, 이 CSS clip-path 제작 도구는 원하는 모양의 값들을 도출하는 데 정말 놀랍습니다.

먼저, 원하는 배경의 "모퉁이" 수를 확인했습니다. 제 경우에는 4개이며, Clippy 웹사이트에서 "평행사변형" 옵션을 선택하고 원하는 모양을 만들기 시작했습니다.

더 복잡한 모양을 원한다면 "사용 정다각형" 섹션을 선택할 수도 있습니다.

<div class="content-ad"></div>

다음 CSS 코드로 비어 있는 div에 필요한 모양을 제공합니다.

```js
.customBackground {
  background-color: rgb(255 204 0);
  clip-path: polygon(23% 70%, 63% 69%, 100% 69%, 100% 100%, 80% 100%, 20% 100%, 0 100%, 0 89%);
  height: 100%;
  width: 100%;
}
```

div를 sticky 배경으로 설정하기

이제 모양을 배경 이미지로 설정하고 페이지 아래쪽에 고정하여 사용하려면, 새로 만든 div에 다음 CSS를 사용하세요.

<div class="content-ad"></div>

```css
.customBackground {
  background-color: rgb(255 204 0);
  clip-path: polygon(23% 70%, 63% 69%, 100% 69%, 100% 100%, 80% 100%, 20% 100%, 0 100%, 0 89%);
  z-index: -10;
  position: fixed;
  height: 100%;
  width: 100%;
  bottom: 0;
}
```

여기요! 새로운 고정된 배경이 동작 중입니다!

최종 결과
