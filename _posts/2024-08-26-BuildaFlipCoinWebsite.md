---
title: "ReactJS로 동전 뒤집기 웹사이트 만드는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 19:58
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Build a Flip Coin Website"
link: "https://dev.to/abhishekgurjar/build-a-flip-coin-website-4e9n"
isUpdated: true
updatedAt: 1724743920839
---


## 소개

안녕하세요, 개발자 여러분! 제가 최근에 개발한 새로운 프로젝트를 소개하게 되어 기쁩니다: Flip Coin 애플리케이션입니다. 이 간단하지만 재미있는 프로젝트를 통해 전통적인 동전 던지기를 시뮬레이션할 수 있어요. 이는 의사 결정을 내릴 때나 재미를 위해 완벽한 기회가 될 거에요. 이는 HTML, CSS, JavaScript를 사용한 대화형 웹 애플리케이션을 만드는 훌륭한 예시입니다.

## 프로젝트 개요

Flip Coin은 동전 던지기를 시뮬레이션하는 웹 기반 애플리케이션입니다. 사용자들은 버튼을 클릭하여 동전을 던질 수 있으며, 결과는 화면에 표시됩니다. 이 프로젝트는 기본적인 웹 개발 기술을 보여주며, 프론트엔드 스킬을 연습할 수 있는 실용적인 방법을 제공합니다.

<div class="content-ad"></div>

## 기능

- 동전 던지기 시뮬레이션: 버튼을 클릭하여 동전을 던져서 앞면 또는 뒷면으로 떨어지는지 확인할 수 있습니다.
- 시각적 피드백: 동전의 결과는 간단한 애니메이션과 함께 표시되어 사용자 경험을 향상시킵니다.
- 반응형 디자인: 어플리케이션은 다양한 기기에서 잘 작동하도록 설계되었습니다.

## 사용된 기술

- HTML: Flip Coin 애플리케이션의 구조를 제공합니다.
- CSS: 애플리케이션을 스타일링하고 시각적으로 매력적인 경험을 위해 애니메이션을 추가합니다.
- JavaScript: 동전 던지기 로직을 구현하고 사용자 상호작용을 처리합니다.

<div class="content-ad"></div>

## 프로젝트 구조

프로젝트 구조에 대한 개요입니다:

```js
Flip-Coin/
├── index.html
├── style.css
└── script.js
```

- index.html: Flip Coin 애플리케이션의 HTML 구조가 포함되어 있습니다.
- style.css: 깔끔하고 상호작용적인 디자인을 위한 CSS 스타일이 포함되어 있습니다.
- script.js: 동전 던지기 로직 및 사용자 상호작용을 관리합니다.

<div class="content-ad"></div>

## 설치

프로젝트를 시작하려면 다음 단계를 따르세요:

- 리포지토리 복제:
git clone https://github.com/abhishekgurjar-in/Flip-Coin.git
- 프로젝트 디렉토리 열기:
cd Flip-Coin
- 프로젝트 실행:
Flip Coin 애플리케이션을 사용하려면 웹 브라우저에서 index.html 파일을 엽니다.

## 사용법

<div class="content-ad"></div>

- 웹 브라우저에서 웹 사이트를 열어주세요.
- 동전을 뒤집기 위해 "Flip Coin" 버튼을 클릭해주세요.
- 화면에서 동전이 앞면 또는 뒷면에 떨어졌는지 확인할 수 있습니다.

## 코드 설명

### HTML

index.html 파일은 Flip Coin 애플리케이션의 구조를 정의합니다. 버튼과 결과를 표시하는 영역이 포함되어 있습니다. 아래는 일부 코드입니다:

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Flip Coin</title>
    <link rel="stylesheet" href="style.css" />
    <script src="./script.js" defer></script>
  </head>
  <body>
    <div id="main">
      <div id="logo_image"></div>
      <div class="container">
        <p>클릭으로 운명 뒤집기</p>
        <div class="stats">
          <p id="heads-count">앞면: 0</p>
          <p id="tails-count">뒷면: 0</p>
        </div>
        <div class="coin">
          <div class="heads">
            <img
              src="https://raw.githubusercontent.com/AsmrProg-YT/100-days-of-javascript/c82f3949ec4ba9503c875fc0fa7faa4a71053db7/Day%20%2307%20-%20Flip%20a%20Coin%20Game/heads.svg"
              alt="앞면"
            />
          </div>
          <div class="tails">
            <img
              src="https://raw.githubusercontent.com/AsmrProg-YT/100-days-of-javascript/c82f3949ec4ba9503c875fc0fa7faa4a71053db7/Day%20%2307%20-%20Flip%20a%20Coin%20Game/tails.svg"
              alt="뒷면"
            />
          </div>
        </div>
        <div id="buttons">
          <button id="flip-button">동전 던지기</button>
          <button id="reset-button">초기화</button>
          <audio id="flip-sound">
            <source src="./assets/coin-flip-88793.mp3" type="audio/mpeg" />
            오디오 요소를 지원하지 않는 브라우저입니다.
          </audio>
        </div>
      </div>
    </div>
    <div class="footer">
      <p>Made with ❤️ by Abhishek Gurjar</p>
    </div>
  </body>
</html>
```

### CSS

style.css 파일은 Flip Coin 애플리케이션을 스타일링하여 동전 던지기에 간단한 애니메이션을 추가합니다. 아래는 일부 주요 스타일입니다:

```js
@import url("https://fonts.googleapis.com/css2?family=Rubik:wght@300;400;500;600;700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Rubik", sans-serif;
}

body {
  height: 100%;
  width: 100%;
  background-color: #072ac8;
}

#main {
  display: flex;
  justify-content: center;
  width: 100%;
  height: 90vh;
}

#logo_image {
  width: 250px;
  height: 100px;
  background: url(./assets/original-68bc1d89ca3ea0450d8ca9f3a1403d42-removebg-preview.png);
  background-position: center;
  background-size: cover;
  align-items: center;
  justify-content: center;
}

.container {
  background: #a2d6f9;
  width: 700px;
  height: 500px;
  padding: 80px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 15px 30px 35px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  -webkit-perspective: 300px;
  perspective: 300px;
}

.container p {
  text-align: center;
  font-size: 20px;
}

.stats {
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: #101020;
  font-weight: 500;
  line-height: 50px;
  font-size: 20px;
}

.coin {
  height: 150px;
  width: 150px;
  position: relative;
  margin: 50px auto;
  -webkit-transform-style: preserve-3d;
  transform-style: preserve-3d;
}

.tails {
  transform: rotateX(180deg);
}

.buttons {
  display: flex;
  justify-content: center;
  align-items: center;
}

.coin img {
  width: 145px;
}

heads,
.tails {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
}

button {
  width: 260px;
  padding: 10px 0;
  border: 2.5px solid black;
  font-size: 22px;
  border-radius: 5px;
  cursor: pointer;
}

#flip-button {
  background: #072ac8;
  color: white;
}

#flip-button:disabled {
  background-color: #e1e0ee;
  color: #101020;
  border-color: #e1e0ee;
}

#reset-button {
  background: #fff;
  color: #072ac8;
}

.footer {
  margin: 20px;
  text-align: center;
  color: white;
}

@keyframes spin-tails {
  0% {
    transform: rotateX(0);
  }
  100% {
    transform: rotateX(1980deg);
  }
}

@keyframes spin-heads {
  0% {
    transform: rotateX(0);
  }
  100% {
    transform: rotateX(2160deg);
  }
}
```

<div class="content-ad"></div>

### 자바스크립트

script.js 파일에는 동전을 던지고 결과를 표시하는 논리가 포함되어 있습니다. 아래는 코드 일부입니다:

```js
let tails = 0;
let heads = 0; // 헤드 변수 정의 추가
let coin = document.querySelector(".coin");
let flipBtn = document.querySelector("#flip-button");
let resetBtn = document.querySelector("#reset-button");
let flipSound = document.querySelector("#flip-sound");

flipBtn.addEventListener("click", () => {
    flipSound.currentTime = 0; 
    flipSound.play();

    let i = Math.floor(Math.random() * 2);
    coin.style.animation = "none";

    if (i) {
        setTimeout(() => {
            coin.style.animation = "spin-heads 3s forwards";
        }, 100);
        heads++;
    } else {
        setTimeout(() => {
            coin.style.animation = "spin-tails 3s forwards";
        }, 100);
        tails++;
    }
    setTimeout(updateStatus, 3000);
    disableButton();
});

function updateStatus() {
    document.querySelector("#heads-count").textContent = `Heads: ${heads}`;
    document.querySelector("#tails-count").textContent = `Tails: ${tails}`;
}

function disableButton() {
    flipBtn.disabled = true;
    setTimeout(() => {
        flipBtn.disabled = false;
    }, 3000);
}

resetBtn.addEventListener("click", () => {
    coin.style.animation = "none"; // 오타 수정: "aniamtion"을 "animation"으로
    heads = 0;
    tails = 0;
    updateStatus();
});
```

## 실시간 데모

<div class="content-ad"></div>

플립 코인 프로젝트의 라이브 데모를 확인할 수 있어요.

## 결론

플립 코인 애플리케이션을 만들면서 재미있고 교육적인 경험이었어요. 이 간단한 프로젝트는 HTML, CSS 및 JavaScript를 사용하여 상호 작용 웹 요소를 만드는 방법을 보여줍니다. 유용하게 활용하시고 즐겁게 실험해보세요. 즐거운 코딩하세요!

## 제작자

<div class="content-ad"></div>

이 프로젝트는 실용적이고 인터랙티브한 웹 어플리케이션을 개발함으로써 내 프론트엔드 개발 스킬을 향상시키는 지속적인 여정의 일환으로 개발되었습니다.

## 작성자

- Abhishek Gurjar
GitHub 프로필
- GitHub 프로필