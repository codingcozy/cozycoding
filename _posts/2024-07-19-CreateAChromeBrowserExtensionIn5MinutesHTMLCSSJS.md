---
title: "5분 만에 만드는 크롬 브라우저 확장 프로그램  HTML, CSS, JS 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-19-CreateAChromeBrowserExtensionIn5MinutesHTMLCSSJS_0.png"
date: 2024-07-19 13:21
ogImage: 
  url: /assets/img/2024-07-19-CreateAChromeBrowserExtensionIn5MinutesHTMLCSSJS_0.png
tag: Tech
originalTitle: "Create A Chrome Browser Extension In 5 Minutes  HTML, CSS, JS"
link: "https://medium.com/codex/create-a-chrome-browser-extension-in-5-minutes-html-css-js-90f88a259769"
---


HTML, CSS 및 JavaScript의 기본 지식이 있다면 브라우저 확장 프로그램을 만들 수 있어요.

# 브라우저 확장 프로그램 만들기

먼저 확장 프로그램 파일을 저장할 hi-ext라는 새 폴더를 만들어주세요. 그리고 폴더 hi-ext 안에 manifest.json이라는 파일을 만들어주세요.

```json
// hi-ext/manifest.json

{
  "manifest_version": 3,
  "name": "Hi 확장 프로그램",
  "description": "기본 Hi 확장 프로그램",
  "version": "1.0",
  "action": {
    "default_popup": "hi.html",
    "default_icon": "hi_extensions.png"
  }
}
```

<div class="content-ad"></div>

이 manifest.json 파일은 확장 프로그램의 기본 구성 정보를 설정하며 브라우저에 의해 올바르게 로드될 것입니다:

- manifest_version — manifest 파일의 버전을 설정합니다. 이 값은 3으로 설정해야 합니다.
- name, description, version — 원하는 값을 할당합니다.
- action — 확장 프로그램의 동작 아이콘 및 동작 아이콘을 클릭했을 때 나타날 팝업을 보여줄 HTML 페이지를 선언합니다.

Freepik에서 다운로드한 무료 PNG 아이콘을 찾으실 수 있습니다. 그 이름을 hi_extensions.png로 변경하고, hi-ext 내에 저장해주세요.

hi-ext 내에 hi.html 파일을 생성하고 아래의 HTML 코드를 추가해주세요:

<div class="content-ad"></div>

```js
<!-- hi-ext/hi.html -->
<html>
  <body>
    <h1>Hi 확장 프로그램</h1>
  </body>
</html>
```

이 시점에서 사용자가 브라우저에서 해당 아이콘을 클릭하면 확장 프로그램이 팝업됩니다.

## 브라우저 확장 프로그램 테스트

개발 목적으로 브라우저에서 빠르게 확장 프로그램을 테스트하고 싶습니다.

<div class="content-ad"></div>

로컬로 로드하여 Chrome에서 테스트할 수 있습니다:

- Chrome Extensions 페이지로 이동합니다. 예: chrome://extensions. (설계상 chrome:// URL은 링크될 수 없습니다.)
- 오른쪽 상단의 토글 스위치를 클릭하여 개발자 모드를 활성화합니다.
- '압축 해제된 확장 프로그램을 로드합니다' 버튼을 클릭하고 확장 프로그램 'hi-ext'를 생성한 폴더를 선택합니다.

<img src="/assets/img/2024-07-19-CreateAChromeBrowserExtensionIn5MinutesHTMLCSSJS_0.png" />

이 단계에서는 아래와 같이 확장 프로그램이 표시됩니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-19-CreateAChromeBrowserExtensionIn5MinutesHTMLCSSJS_1.png" />

## 핀하기!

기본적으로 확장 프로그램을 로컬로 로드하면 확장 메뉴에 나타납니다. 확장 프로그램을 툴바에 고정하여 개발 중에 확장 프로그램에 빠르게 액세스할 수 있습니다.

확장 프로그램을 고정하면 개발 중에 테스트해보기에 유용합니다.

<div class="content-ad"></div>

아래와 같이 열립니다:

![이미지](/assets/img/2024-07-19-CreateAChromeBrowserExtensionIn5MinutesHTMLCSSJS_3.png)

멋져요! 작동합니다!

<div class="content-ad"></div>

## 자바스크립트 추가

지금까지 기본 작동하는 익스텐션을 만들었습니다. hi-ext 폴더 안에 popup.js 파일을 추가하여 초기 console.log를 넣을 수 있습니다.

```js
// hi-ext/popup.js
console.log("This popup is saying Hi!");
```

그리고 HTML 파일을 업데이트하여 해당 JavaScript 파일을 참조하세요.

<div class="content-ad"></div>


<!-- hi-ext/hi.html -->
<html>

<body>
    <h1>확장프로그램 안내</h1>
    <script src="popup.js"></script>
</body>

</html>


콘솔에 로그를 확인하려면 다음을 수행하세요:

- 팝업을 엽니다.
- 팝업에서 마우스 오른쪽 버튼을 클릭합니다.
- "검사"를 선택합니다.
- 개발자 도구에서 콘솔 패널로 이동합니다.

<img src="/assets/img/2024-07-19-CreateAChromeBrowserExtensionIn5MinutesHTMLCSSJS_4.png" />


<div class="content-ad"></div>

여기에 기본 Chrome 브라우저 익스텐션이 있어요.

# 당신을 격려하는 브라우저 익스텐션

다음 단계에서는 격려하는 명언을 제공하는 Chrome 브라우저 익스텐션을 만듭니다.

## HTML

<div class="content-ad"></div>

위의 코드를 `hi.html`에 추가하여 시작합니다.

```js
<!-- hi-ext/hi.html -->
<html>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>모티베이션 명언</title>
<link rel="stylesheet" href="styles.css">

<body>
    <h1>모티베이션 명언</h1>
    <div class="quote-container">
        <button id="motivateMe">나를 동기부여해줘</button>
        <div id="quote"></div>
    </div>
    <script src="popup.js"></script>
</body>

</html>
```

## CSS

보시다시피, 아직 생성하지 않은 `styles.css` 파일을 참조하고 있습니다.

<div class="content-ad"></div>

안녕하세요! 아래의 CSS를 styles.css 파일로 작성해주세요.

```css
body {
  font-family: Arial, sans-serif;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 500px;
  width: 300px;
  background-color: #f0f0f0;
}

.quote-container {
  text-align: center;
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

button {
  margin: 5px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

#quote {
  margin-top: 20px;
  font-size: 18px;
  font-style: italic;
}
```

## JavaScript

팝업 창에 인용구와 아래의 JavaScript 코드를 포함시키기 위해 popup.js를 업데이트해주세요.

<div class="content-ad"></div>

```js
// popup.js

const quotes = [
  {
    quote: "위대한 일을 하는 유일한 방법은 자신이 하는 일을 사랑하는 것입니다.",
    author: "스티브 잡스",
  },
  {
    quote: "나무를 심을 가장 좋은 때는 20년 전이었습니다. 두 번째로 좋은 때는 지금입니다.",
    author: "중국 속담",
  },
  {
    quote: "당신의 시간은 제한되어 있으니 타인의 삶을 살아낭비하지 마세요.",
    author: "스티브 잡스",
  },
  {
    quote: "내일을 꿈꾸는 것의 유일한 제한은 오늘의 의심입니다.",
    author: "프랭클린 D. 루즈벨트",
  },
  {
    quote: "미래는 자신의 꿈의 아름다움을 믿는 사람에게 속합니다.",
    author: "일리노이 로즈벨트",
  },
  {
    quote: "중요한 것은 당신이 천천히 가는 속도가 아니라 멈추지 않는 것입니다.",
    author: "공자",
  },
  {
    quote: "당신이 항상 원해온 모든 것은 두려움의 반대편에 있습니다.",
    author: "조지 애다이어",
  },
  {
    quote: "성공은 최종적이지 않고, 실패는 죽음이 아닙니다. 계속하는 용기가 중요합니다.",
    author: "윈스턴 처칠",
  },
  {
    quote: "할 수 있다고 믿으면, 당신의 절반에 이미 도달한 것입니다.",
    author: "테오도어 루즈벨트",
  },
  {
    quote: "당신이 하는 일이 차이를 만든다고 생각하면 그래요.",
    author: "윌리엄 제임스",
  },
];

document.getElementById("motivateMe").addEventListener("click", function () {
  const randomIndex = Math.floor(Math.random() * quotes.length);
  const randomQuote = quotes[randomIndex];
  document.getElementById(
    "quote"
  ).innerHTML = `"${randomQuote.quote}" - ${randomQuote.author}`;
});
```

그리고 확장 기능에서 첫 번째 동기부여 명언을 소개합니다:

<img src="/assets/img/2024-07-19-CreateAChromeBrowserExtensionIn5MinutesHTMLCSSJS_5.png" />

그래서 계속해서 나아가세요!