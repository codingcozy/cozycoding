---
title: "HTML과 CSS로 간단한 로더 2가지 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-23-TwoSimpleLoaderswithHTMLandCSS_0.png"
date: 2024-07-23 21:17
ogImage: 
  url: /assets/img/2024-07-23-TwoSimpleLoaderswithHTMLandCSS_0.png
tag: Tech
originalTitle: "Two Simple Loaders with HTML and CSS"
link: "https://medium.com/gitconnected/two-simple-loaders-with-html-and-css-49d25b932a25"
---


로더는 웹 애플리케이션의 중요한 부분으로, 로딩하거나 처리하는 동안 사용자에게 피드백을 제공합니다.

![로더 이미지](/assets/img/2024-07-23-TwoSimpleLoaderswithHTMLandCSS_0.png)

그들은 여러 목적을 제공합니다:

- 사용자 경험 향상: 로더는 사용자에게 작업 진행 상황(콘텐츠 로딩, 폼 제출, 데이터 처리 등)에 대한 정보를 제공합니다. 로더가 없으면 애플리케이션이 반응이 없는 것으로 인식될 수 있어 사용자들이 답답해할 수 있습니다.
- 속도 인식: 로더는 배경에서 어떤 일이 일어나고 있는 것을 나타내어, 로딩 시간이 변하지 않더라도 속도를 인식시킵니다. 이는 애플리케이션이 빠르고 사용자에게 더 반응적으로 보이도록 만듭니다.
- 기대 관리: 로더는 사용자 요청에 애플리케이션이 활발히 작업 중임을 나타내어, 사용자들이 애플리케이션이 멈춘 것이나 충돌한 것으로 착각하는 것을 방지합니다.
- 전문성과 세련된 완성도: 로더를 포함함으로써 웹 애플리케이션의 디자인에서 세부 사항과 전문성에 대한 주의를 보여줍니다. 이는 개발자들이 스무스하고 직관적인 사용자 경험을 제공하고자 애플리케이션에 신경쓴다는 것을 보여줍니다.
- 접근성 고려: 로더는 시각적 단서 외에 청각적이나 촉각적인 피드백을 제공함으로써 장애를 가진 사용자들에게 혜택을 줄 수 있습니다. 이를 통해 사용자의 능력에 관계없이 모든 사용자들이 애플리케이션 상태에 대해 정보를 받을 수 있습니다.

<div class="content-ad"></div>

요약하자면, 로더는 웹 애플리케이션의 필수 요소로, 긍정적인 사용자 경험에 기여하며 지각된 성능을 향상시키고, 기대를 관리하며 디자인 및 개발에서 전문성을 나타냅니다.

이 기사에서는 HTML 및 CSS만을 사용하여 만들 수 있는 두 가지 간단한 사용자 지정 로더를 소개할 것입니다.

# 로더 1

![로더 1](https://miro.medium.com/v2/resize:fit:400/1*PEz9ciOVPE3gu0llZgFulQ.gif)

<div class="content-ad"></div>

```html
<head>
    <link rel="stylesheet" href="loader.css">
</head>

<body>
    <div class="loader-wrapper">
        <div class="loader">
            <div class="loader loader-inner"></div>
        </div>
    </div>
</body>

```

```css
.loader-wrapper {
    width: 100px;
    height: 100px;
}

.loader {
    box-sizing: border-box;
    width: 100%;
    height: 100%;
    border: 10px solid #162534;
    border-top-color: #4bc8eb;
    border-bottom-color: #EB6E4B;
    border-radius: 50%;
    animation: rotate 5s linear infinite;
}

.loader-inner {
    border-top-color: #36f372;
    border-bottom-color: #F336B7;
    animation-duration: 2.5s;
}

@keyframes rotate {

    0% {
        transform: scale(1) rotate(360deg);
    }

    50% {
        transform: scale(.8) rotate(-360deg);
    }
}
```

# Loader 2

![Loader Animation](https://miro.medium.com/v2/resize:fit:400/1*22qPBeV8QZUTFGolR7VhRw.gif)


<div class="content-ad"></div>

```js
<html>

<head>
    <link rel="stylesheet" href="loader.css">
</head>

<body>
    <div class="loader">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
    </div>
</body>

</html>
```

```js
* {
    margin: 0;
    padding: 0;
}

body {
    background-color: #f5f5f5;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    width: 100vh;
}

.loader {
    width: 100px;
    height: 100px;
    position: relative;
}

.loader div {
    position: absolute;
    width: 35%;
    height: 35%;
    border-radius: 5px;
    animation: load 2s infinite ease-in-out;
}

.loader div:nth-of-type(1) {
    background-color: #B22727;
}

.loader div:nth-of-type(2) {
    background-color: #EE5007;
    animation-delay: 0.5s;
}

.loader div:nth-of-type(3) {
    background-color: #F8CB2E;
    animation-delay: 1s;
}

.loader div:nth-of-type(4) {
    background-color: #006E7F;
    animation-delay: 1.5s;
}

@keyframes load {
    0% {
        transform: translate(0%);
        border-radius: 50%;
    }

    25% {
        transform: translate(200%);
        border-radius: 50%;
    }

    50% {
        transform: translate(200%, 200%);
        border-radius: 0%;
    }

    75% {
        transform: translate(0%, 200%);
        rotate: (-45deg);
        border-radius: 0%;
    }

    100% {
        transform: translate(0%);
        border-radius: 50%;
    }
}
```

이 웹사이트 https://uiball.com/ldrs/ 에서도 다양한 스윗로더를 찾을 수 있어요. 여러 예시 중에서 선택하고 쉽게 사용할 수 있어요.
글 전체를 읽어주셔서 감사합니다. 지루하지 않길 바라요.



<div class="content-ad"></div>

위 코드(로더 1)는 이 GitHub 저장소에서 찾을 수 있어요.

위 코드(로더 2)는 이 GitHub 저장소에서 찾을 수 있어요.

다음은 무엇일까요? 제 이야기를 가장 먼저 읽으려면 Medium에서 저를 팔로우해주세요.