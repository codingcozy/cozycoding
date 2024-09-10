---
title: "CSS와 HTML을 사용하여 Glassmorphism 효과를 만드는 방법"
description: ""
coverImage: "/assets/img/2024-09-10-HowtoCreateGlassmorphismEffectusingCSSandHTML_0.png"
date: 2024-09-10 17:40
ogImage: 
  url: /assets/img/2024-09-10-HowtoCreateGlassmorphismEffectusingCSSandHTML_0.png
tag: Tech
originalTitle: "How to Create Glassmorphism Effect using CSS and HTML"
link: "https://medium.com/@infodigit67/how-to-create-glassmorphism-effect-using-css-and-html-8415025d9bdc"
isUpdated: false
---


글래스모피즘은 상대적으로 새로운 트렌드로 떠오르고 있는 기능으로, 사이트에 신선하고 독특한 느낌을 주기 위해 새로 디자인된 웹사이트에서 자주 선호됩니다. Behance와 dribble과 같은 서비스에서 더욱 인기를 얻고 있습니다.

이 기능을 이용하면 사이트의 요소들이 투명하거나 담화된 유리처럼 보이게 할 수 있습니다. 또한 생생하거나 연한 컬러와 가벼운 테두리를 사용해 독특한 웹 디자인을 구현할 수 있습니다.

이 간결하고 통찰력있는 단계별 튜토리얼에서는 처음부터 Glassmorphism이라 불리는 유리와 같은 룩과 느낌을 어떻게 만들 수 있는지 살펴볼 것입니다. 최종 사용자 인터페이스는 아래 이미지와 같이 보일 것입니다. 또한 당신의 취향에 맞는 다른 값들로도 튜토리얼을 따라와도 괜찮습니다.

![이미지](/assets/img/2024-09-10-HowtoCreateGlassmorphismEffectusingCSSandHTML_0.png)

<div class="content-ad"></div>

계속 진행하기 전에 CSS에서 유리와 같은 느낌을 만드는 것은 실제로 HTML 요소의 다양한 층의 색상을 조작하는 것입니다. 이렇게 하면 브라우저가 렌더링할 때 눈이 그것을 다른 곳에 놓인 유리 조각으로 인식하고 텍스트가 적힌 것처럼 보입니다.

# 단계 1

우리는 일반 HTML을 사용하여 프로젝트의 뼈대를 만드는 것으로 시작합니다. 코드 스니펫에 나와 있는 대로 3개의 div를 만들 것입니다: container, card, 그리고 중첩된 콘텐츠를 위한 하나 더.

```js
<div class="container">

<div class="card">

<div class="content">

<h2>01</h2>

<h3>SERVICES</h3>

<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo

</p>

<a href="">Read More...</a>

</div>

</div>

</div>
```

<div class="content-ad"></div>

이 코드는 필요한 만큼 복제하여 세 가지 다른 카드를 만들 수 있습니다.

## 참고

저희 튜토리얼의 주요 내용은 HTML이 아닌 CSS에 있으므로 색상 조작에 직접 뛰어들어 봅시다.

# 단계 2

<div class="content-ad"></div>

```css
import url('https://fonts.googleapis.com/css?family=Open+Sans&display=swap');

*

{

margin: 0;

padding: 0;

box-sizing: border-box;

font-family: 'poppin', sans-serif;

}

body {

display: flex;

justify-content: center;

align-items: center;

min-height: 100vh;

background: #161623;

}
``` 

# Step 3

프로젝트에서 두 개의 원을 생성하세요.

<div class="content-ad"></div>


```js
body::before {

content: "";

position: absolute;

top: 0;

left: 0;

width: 100%;

height: 100%;

background: linear-gradient(#2196f3, #e91e63);

clip-path: circle(20% at 10% 10%);

}

body::after {

content: "";

position: absolute;

top: 0;

left: 0;

width: 100%;

height: 100%;

background: linear-gradient(#2196f3, #e91e63);

clip-path: circle(20% at 10% 10%);

}
```

## 참고

이 원들은 앞 단계에서 볼 것과 같은 흐림 효과의 차이를 보여주기 위해 배경으로 만들어졌습니다.

위 스니펫에서 가장 중요한 점은 clip-path 속성의 사용으로 원을 생성하고, 원 함수에 전달된 백분율 값으로 위치를 조정하며, 또한 linear-gradient의 사용으로 단색보다 더 나은 유리와 같은 반사를 제공합니다.


<div class="content-ad"></div>

# 단계 4

그라디언트를 변경하고 각 원의 위치를 조절하기 위해 circle 함수를 사용하여 놀 수 있어요.

# 단계 5

컨테이너를 스타일링하여 콘텐츠를 원하는 위치로 배치하세요. 저는 아래와 같은 값들을 사용할 거에요.

<div class="content-ad"></div>


.container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  max-width: 1200%;
  flex-wrap: wrap;
  z-index: 1;
}


컨테이너 내부에 있는 카드를 스타일링해주세요. 원하는 값은 아래에 나와 있습니다. 원하시는 스타일을 얻기 위해 사용자 정의 값들을 자유롭게 사용해주세요.

## 참고

원하는 효과를 얻기 위해 box-shadow, 테두리 및 backdrop-filter를 사용해주세요. 백드롭 필터는 카드를 흐릿하게 만들어서 콘텐츠를 유리를 통해 바라보는 것처럼 보이게 합니다. 테두리는 카드를 3D 모양으로 만들어 작은 유리 조각처럼 보이게 하고, 박스 그림자는 카드를 배경으로부터 구분해 주어 그림자 효과를 줍니다. 다시 한번 말씀드리지만, 이 값들을 재미있게 조합하여 원하는 효과를 얻을 수 있습니다.


<div class="content-ad"></div>

## 참고

테두리는 카드 요소 주위가 아니라 위 및 왼쪽에만 배치됩니다.

```js
.container .card {

position: relative;

width: 280%;

height: 400%;

margin: 30px;

box-shadow: 20px 20px 50px rgba(0,0, 0, 0.5);

border-radius: 5px;

backkground: rgba(255, 255, 255, 0.1);

overflow: hidden;

display: flex;

justify-content: center;

align-items: center;

border-top: 1px solid rgba(255, 255, 255, 0.5);

border-left: 1px solid rgba(255, 255, 255, 0.5);

backdrop-filter: blur(5px);

}
```

이 시점에서 이미 유리같은 프로젝트를 만들었어야 합니다. 하지만 여기서 멈출 이유가 없습니다. 카드로 들어가서 내용을 하나씩 수정하여 더 나은 외관을 갖도록 해봅시다. 아래 코드 스니펫을 확인해보세요.

<div class="content-ad"></div>

```css
.container .card .content {
    padding: 20px;
    text-align: center;
    transition: 0.5s;
}

.container .card .content h2 {
    position: absolute;
    top: -30px;
    right: 30px;
    font-size: 6em;
    color: rgba(255, 255, 255, 0.99);
    pointer-events: none;
}

.container .card .content h3 {
    font-size: 1.8em;
    color: #fff;
    z-index: 1;
}
```

여기 있어요, 이해하셨죠?

사이트를 구축할 때 어떤 디자인 기능을 사용하더라도, 사이트에 다른 유형의 디자인 요소를 결합하여 독특하면서도 일관된 디자인을 만들 수 있습니다. 코드를 필요에 맞게 수정하여 어떻게 작동하는지 더 잘 이해하고 고유한 CSS 효과를 만들어보세요.

이 기능을 어떻게 적용했는지 저희에게 알려주세요. 마지막까지 함께해 주셔서 감사합니다. 함께 고생하셨습니다.

<div class="content-ad"></div>

# 쉽게 말해 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 작가를 클랩하고 팔로우해 주세요 👏️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기