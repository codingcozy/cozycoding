---
title: "HTML, CSS, JavaScript로 비밀번호 생성 웹사이트 만드는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 19:56
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Build a Password Generator Website"
link: "https://dev.to/abhishekgurjar/build-a-password-generator-website-1o9g"
isUpdated: true
updatedAt: 1724743826749
---


## 소개

안녕하세요, 개발자 여러분! 제 새로운 프로젝트인 패스워드 생성기를 소개하게 되어 매우 기쁩니다. 이 도구는 안전하고 무작위한 패스워드를 손쉽게 생성할 수 있도록 설계되었습니다. 온라인 계정을 위한 강력한 패스워드가 필요하거나 JavaScript 기술을 연습하고 싶을 때, 이 패스워드 생성기는 여러분의 도구상자에 좋은 추가물이 될 것입니다.

## 프로젝트 개요

패스워드 생성기는 사용자가 다양한 구성으로 패스워드를 생성할 수 있는 웹 기반 응용 프로그램입니다. 패스워드 길이를 조정하고 소문자, 대문자, 숫자 및 기호와 같은 특정 문자 유형을 포함 또는 제외할 수 있습니다. 이 프로젝트는 JavaScript를 사용하여 동적 패스워드 생성기를 구축하는 방법을 소개하며, HTML과 CSS로 구축된 깔끔하고 사용자 친화적인 인터페이스가 함께 제공됩니다.

<div class="content-ad"></div>

## 특징

- 사용자 정의 가능한 비밀번호 길이: 슬라이더를 사용하여 생성된 비밀번호의 길이를 조절할 수 있습니다.
- 문자 유형 선택: 비밀번호에 포함할 문자 유형(소문자, 대문자, 숫자, 기호)을 선택할 수 있습니다.
- 생성 및 복사: 랜덤한 비밀번호를 생성하고 쉽게 클립보드에 복사할 수 있습니다.

## 사용된 기술

- HTML: 비밀번호 생성기의 구조와 레이아웃을 제공합니다.
- CSS: 현대적이고 반응형 디자인을 보장하기 위해 인터페이스를 스타일링합니다.
- JavaScript: 사용자 상호작용 및 비밀번호 복사를 포함한 비밀번호 생성 로직을 처리합니다.

<div class="content-ad"></div>

## 프로젝트 구조

프로젝트 구조에 대한 개요입니다:

```js
Password-Generator/
├── index.html
├── style.css
└── script.js
```

- index.html: 비밀번호 생성기에 대한 HTML 구조를 포함합니다.
- style.css: 매력적이고 반응형 디자인을 만들기 위한 CSS 스타일을 포함합니다.
- script.js: 비밀번호 생성 기능과 사용자 상호작용을 관리합니다.

<div class="content-ad"></div>

## 설치

프로젝트를 시작하려면 다음 단계를 따르세요:

- 저장소를 복제합니다:
```bash
git clone https://github.com/abhishekgurjar-in/Password-Generator.git
```
- 프로젝트 디렉토리를 엽니다:
```bash
cd Password-Generator
```
- 프로젝트를 실행합니다:
웹 브라우저에서 index.html 파일을 열어서 Password Generator를 사용하세요.

## 사용법

<div class="content-ad"></div>

- 웹 브라우저에서 웹 사이트를 열어주세요.
- 슬라이더를 사용하여 비밀번호 길이를 조절해주세요.
- 포함하고자 하는 문자 유형을 선택하거나 선택 해제해주세요.
- "비밀번호 생성" 버튼을 클릭하여 비밀번호를 생성해주세요.
- 복사 아이콘을 클릭하여 비밀번호를 클립 보드에 복사해주세요.

## 코드 설명

### HTML

index.html 파일은 비밀번호 생성기의 구조를 정의하며, 입력 필드, 컨트롤 및 디스플레이 영역을 포함합니다. 다음은 일부 코드 조각입니다:

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <link
      href="https://fonts.googleapis.com/icon?family=Material+Icons"
      rel="stylesheet"
    />
    <script src="script.js" defer></script>
    <title>Password Generator</title>
  </head>
  <body>
    <div class="container">
      <h1>Password Generator</h1>

      <div class="inputBox">
        <input type="text" class="passBox" id="passBox" disabled />
        <span class="material-icons" id="copyIcon">content_copy</span>
      </div>

      <input type="range" min="1" max="30" value="8" id="inputSlider" />

      <div class="row">
        <p>Password Length</p>
        <span id="sliderValue"></span>
      </div>

      <div class="row">
        <label for="lowercase">Include Lowercase Letters (a-z)</label>
        <input type="checkbox" name="lowercase" id="lowercase" checked/>
      </div>

      <div class="row">
        <label for="uppercase">Include Uppercase Letters (A-Z)</label>
        <input type="checkbox" name="uppercase" id="uppercase" checked/>
      </div>

      <div class="row">
        <label for="numbers">Include Numbers (0-9)</label>
        <input type="checkbox" name="numbers" id="numbers" checked/>
      </div>

      <div class="row">
        <label for="symbols">Include Symbols (@-*)</label>
        <input type="checkbox" name="symbols" id="symbols" checked/>
      </div>

      <button type="button" class="genBtn" id="genBtn">
        Generate Password
      </button>
    </div>
    <div class="footer">
      <p>Made with ❤️ by Abhishek Gurjar</p>
    </div>
  </body>
</html>
```

### CSS

The style.css file styles the Password Generator, making it visually appealing and responsive. Below are some key styles:

```js
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

body {
  max-width: 100vw;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #000;
  color: #fff;
  font-weight: 600;
  flex-direction: column;
}

.container {
  border: 0.5px solid #fff;
  border-radius: 10px;
  padding: 28px 32px;
  display: flex;
  flex-direction: column;
  background: transparent;
  box-shadow: 8px 8px 4px #909090, 8px 8px 0px #575757;
}

.container h1 {
  font-size: 1.4rem;
  margin-block: 8px;
}

.inputBox {
  position: relative;
}

.inputBox span {
  position: absolute;
  top: 16px;
  right: 6px;
  color: #000;
  font-size: 28px;
  cursor: pointer;
  z-index: 2;
}

.passBox {
  background-color: #fff;
  border: none;
  outline: none;
  padding: 10px;
  width: 300px;
  border-radius: 4px;
  font-size: 20px;
  margin-block: 8px;
  text-overflow: ellipsis;
}

.row {
  display: flex;
  margin-block: 8px;
}

.row p, .row label {
  flex-basis: 100%;
  font-size: 18px;
}

.row input[type="checkbox"] {
  width: 20px;
  height: 20px;
}

.genBtn {
  border: none;
  outline: none;
  background-color: #2c619e;
  padding: 12px 24px;
  color: #fff;
  font-size: 18px;
  margin-block: 8px;
  font-weight: 700;
  cursor: pointer;
  border-radius: 4px;
}

.genBtn:hover {
  background-color: #0066ff;
}

.footer {
  color: white;
  margin-top: 150px;
  text-align: center;
}

.footer p {
  font-size: 16px;
  font-weight: 200;
}
```

<div class="content-ad"></div>

### JavaScript

스크립트 파일인 script.js에는 비밀번호 생성 로직, 사용자 상호작용 관리 및 비밀번호를 클립보드에 복사하는 기능이 포함되어 있습니다. 아래는 코드 일부입니다:

```js
let inputSlider = document.getElementById("inputSlider");
let sliderValue = document.getElementById("sliderValue");
let passBox = document.getElementById("passBox");
let lowercase = document.getElementById("lowercase");
let uppercase = document.getElementById("uppercase");
let numbers = document.getElementById("numbers");
let symbols = document.getElementById("symbols");
let genBtn = document.getElementById("genBtn");
let copyIcon = document.getElementById("copyIcon");

// 입력 슬라이더 값 표시
sliderValue.textContent = inputSlider.value;
inputSlider.addEventListener("input", () => {
  sliderValue.textContent = inputSlider.value;
});

genBtn.addEventListener("click", () => {
  passBox.value = generatePassword();
});

let lowerChars = "abcdefghijklmnopqrstuvwxyz";
let upperChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
let allNumbers = "0123456789";
let allSymbols = "~!@#$%^&*";

// 비밀번호 생성 함수
function generatePassword() {
  let genPassword = "";
  let allChars = "";

  allChars += lowercase.checked ? lowerChars : "";
  allChars += uppercase.checked ? upperChars : "";
  allChars += numbers.checked ? allNumbers : "";
  allChars += symbols.checked ? allSymbols : "";

  if (allChars === "") {
    return genPassword;
  }

  let i = 1;
  while (i <= inputSlider.value) {
    genPassword += allChars.charAt(Math.floor(Math.random() * allChars.length));
    i++;
  }

  return genPassword;
}

copyIcon.addEventListener("click", () => {
  if (passBox.value !== "") {
    navigator.clipboard.writeText(passBox.value);
    copyIcon.innerText = "check";
    copyIcon.title = "비밀번호가 복사되었습니다.";

    setTimeout(() => {
      copyIcon.innerHTML = "content_copy";
      copyIcon.title = "";
    }, 3000);
  }
});
```

## 실시간 데모

<div class="content-ad"></div>

암호 생성기 프로젝트의 데모를 확인해보세요.

## 마무리

암호 생성기를 만드는 것은 나에게 즐거운 프로젝트였고 프론트엔드 개발 스킬을 적용할 수 있었습니다. 이 도구는 강력한 암호를 생성하는 데 유용하며 웹 개발 프로젝트에 좋은 추가물이 될 수 있습니다. 제가 도움이 되었듯이 여러분도 도움이 되길 바랍니다. 즐겁게 코딩해보세요!

## 크레딧

<div class="content-ad"></div>

이 프로젝트는 제 JavaScript 기술을 향상시키고 실용적인 웹 도구를 만들기 위한 여정의 일환으로 개발되었습니다.

## 작성자

- Abhishek Gurjar
GitHub 프로필
- GitHub 프로필