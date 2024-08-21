---
title: "HTML, CSS, JavaScript로 Google Gemini 챗봇 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-BuildAGoogleGeminiChatbotwithHTMLCSSandJavaScript_0.png"
date: 2024-08-03 18:06
ogImage: 
  url: /assets/img/2024-08-03-BuildAGoogleGeminiChatbotwithHTMLCSSandJavaScript_0.png
tag: Tech
originalTitle: "Build A Google Gemini Chatbot with HTML CSS and JavaScript"
link: "https://dev.to/codingnepal/build-a-google-gemini-chatbot-with-html-css-and-javascript-434p"
isUpdated: true
updatedAt: 1724246462853
---



![Gemini Chatbot](/assets/img/2024-08-03-BuildAGoogleGeminiChatbotwithHTMLCSSandJavaScript_0.png)

Google Gemini나 ChatGPT와 같은 AI 챗봇은 기술과의 상호 작용 방식을 바꾸어 기계와의 대화를 거의 인간과 같이 만들었습니다. 초보 웹 개발자로서, 여러분은 자신만의 AI 챗봇을 만들어 본 적이 있습니까? 좋은 소식은 HTML, CSS 및 JavaScript를 사용하여 Google Gemini와 유사한 챗봇을 만들 수 있다는 것입니다.

익숙하지 않은 분들을 위해, Gemini는 Google이 개발한 고급 챗봇 모델로, ChatGPT와 유사합니다. 인공 지능을 사용하여 인간과 유사한 대답을 생성하며, 자연스러운 대화 능력으로 인기를 끌었습니다.

이 블로그 포스트에서는 HTML, CSS 및 JavaScript를 활용하여 Google Gemini 챗봇을 만드는 방법을 안내하겠습니다. 이 챗봇을 사용하면 사용자가 채팅할 수 있고, 대답을 복사하고, 밝은 테마와 어두운 테마를 전환할 수 있습니다. 또한, 테마와 채팅 기록은 브라우저의 로컬 저장소에 저장되어 페이지 새로 고침 후에도 유지되도록 보장됩니다.


<div class="content-ad"></div>

## HTML CSS & JavaScript로 Gemini 챗봇 만들기 비디오 튜토리얼

위의 YouTube 비디오는 비디오 튜토리얼을 선호하는 경우 좋은 자료입니다. 각 코드 라인을 설명하고 주석을 제공하여 Gemini 챗봇 복제 프로젝트를 함께 따라하기 쉽게 만들어 줍니다. 독서를 선호하거나 단계별 안내가 필요한 경우에는 계속해서 이 게시물을 따라 주세요.

## HTML & JavaScript로 Gemini 챗봇 만드는 단계

HTML, CSS 및 JavaScript를 사용하여 상호작용적이고 기능적인 Gemini 챗봇을 만드는 간단한 단계별 지침을 따라주세요.

<div class="content-ad"></div>

- 마음에 드는 이름의 폴더를 만들어주세요, 예를 들어, gemini-chatbot.
- 그 안에 index.html, style.css 및 script.js와 같은 필요한 파일들을 만들어주세요.
- 이미지 폴더를 다운로드하여 프로젝트 디렉토리에 넣어주세요. 이 폴더에는 이 챗봇 프로젝트에 필요한 로고가 포함되어 있습니다.

당신의 index.html 파일에는 Gemini 챗 레이아웃을 구조화하기 위한 필수 HTML 마크업을 추가해주세요. 인사 헤더, 제안 목록, 챗 섹션 및 입력 폼이 있는 구조물이 포함되어 있습니다. 모두 의미 있는 태그로 구성되어 있습니다.

```js
<!DOCTYPE html>
<!-- Coding By CodingNepal - www.codingnepalweb.com -->
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gemini Chatbot | CodingNepal</title>
  <!-- 아이콘을 위한 구글 폰트 링크 -->
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Rounded:opsz,wght,FILL,GRAD@24,400,0,0" />
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header class="header">
    <!-- 인사 헤더 -->
    <h1 class="title">안녕하세요!</h1>
    <p class="subtitle">오늘은 어떻게 도와드릴까요?</p>

    <!-- 제안 목록 -->
    <ul class="suggestion-list">
      <li class="suggestion">
        <h4 class="text">5명의 가장 친한 친구들과 $100 이하로 게임 밤을 계획해주세요.</h4>
        <span class="icon material-symbols-rounded">draw</span>
      </li>
      <li class="suggestion">
        <h4 class="text">대중 연설 기술을 향상시키는 가장 좋은 팁은 무엇인가요?</h4>
        <span class="icon material-symbols-rounded">lightbulb</span>
      </li>
      <li class="suggestion">
        <h4 class="text">웹 개발 최신 뉴스를 찾아주실 수 있나요?</h4>
        <span class="icon material-symbols-rounded">explore</span>
      </li>
      <li class="suggestion">
        <h4 class="text">배열의 모든 요소를 합산하는 JavaScript 코드를 작성해주세요.</h4>
        <span class="icon material-symbols-rounded">code</span>
      </li>
    </ul>
  </header>

  <!-- 챗 리스트 / 컨테이너 -->
  <div class="chat-list"></div>

  <!-- 입력 영역 -->
  <div class="typing-area">
    <form action="#" class="typing-form">
      <div class="input-wrapper">
        <input type="text" placeholder="여기에 프롬프트를 입력하세요" class="typing-input" required />
        <button id="send-message-button" class="icon material-symbols-rounded">send</button>
      </div>
      <div class="action-buttons">
        <span id="theme-toggle-button" class="icon material-symbols-rounded">light_mode</span>
        <span id="delete-chat-button" class="icon material-symbols-rounded">delete</span>
      </div>
    </form>
    <p class="disclaimer-text">
      Gemini는 사람들에 대한 정보를 포함한 부정확한 정보를 표시할 수 있습니다, 그러니 답변을 반드시 확인해주세요.
    </p>
  </div>

  <script src="script.js"></script>
</body>
</html>
```

style.css 파일에는 챗봇을 스타일링하는 CSS 코드를 추가해서 반응형이며 Gemini식 디자인을 줘보세요. 색상, 글꼴, 배경과 같은 다양한 CSS 속성을 실험하여 클론을 더 매력적으로 만들어보세요.

<div class="content-ad"></div>

```js
/* Google 폰트 Import - Poppins */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

:root {
  /* 다크 모드 색상 */
  --text-color: #E3E3E3;
  --subheading-color: #828282;
  --placeholder-color: #A6A6A6;
  --primary-color: #242424;
  --secondary-color: #383838;
  --secondary-hover-color: #444;
}

.light_mode {
  /* 라이트 모드 색상 */
  --text-color: #222;
  --subheading-color: #A0A0A0;
  --placeholder-color: #6C6C6C;
  --primary-color: #FFF;
  --secondary-color: #E9EEF6;
  --secondary-hover-color: #DBE1EA;
}

body {
  background: var(--primary-color);
}

.header, .chat-list .message, .typing-form {
  margin: 0 auto;
  max-width: 980px;
}

.header {
  margin-top: 6vh;
  padding: 1rem;
  overflow-x: hidden;
}

body.hide-header .header {
  margin: 0;
  display: none;
}

...
```

스크립트.js 파일에 채팅봇을 상호작용 가능하고 기능적으로 만드는 JavaScript 코드를 추가하세요. 이는 메시지를 보내고 받고, 라이트 모드와 다크 테마를 전환하고 채팅 기록을 관리하는 기능을 활성화하는 것을 포함합니다.

중요: Gemini API 키로 설정되기 전까지 채팅봇은 응답을 생성하려 할 수 없습니다. 이를 위해 스크립트.js 파일의 API_KEY 변수에 API 키를 추가하세요. Google AI Studio에서 무료 API 키를 얻을 수 있습니다. API 키는 다음과 같이 보일 것입니다: AIzaSyAtpnKGX14bTgmx0l_gQeatYvdWvY_wOTQ.

<div class="content-ad"></div>

API 키를 코드에 추가한 후, Gemini 챗봇과 대화를 시작할 준비가 되었습니다. 브라우저에서 index.html 파일을 열어 실제 작동하는 것을 확인해보세요!

## 결론과 마지막으로

HTML, CSS 및 JavaScript를 사용하여 Google Gemini 챗봇을 성공적으로 구축했습니다. 이러한 단계를 따라 사용자와 상호 작용하고 테마를 변경하며 로컬 저장소를 사용하여 채팅 기록을 저장할 수 있는 기능적인 챗봇을 개발했습니다.

이 프로젝트를 통해 웹 개발 기술을 향상시키는데 더불어 API를 통합하고 애플리케이션 상태를 관리하는 실용적인 경험을 쌓을 수 있습니다. 챗봇을 운용하며 추가 기능을 추가하거나 기능을 향상시켜 요구 사항을 더 잘 충족시킬 수 있는 방법을 탐색할 수 있습니다.

<div class="content-ad"></div>

만약 Gemini 챗봇을 구축하는 동안 어떤 문제가 발생하면, "다운로드" 버튼을 클릭하여 해당 프로젝트의 소스 코드 파일을 다운로드할 수 있습니다.

[코드 파일 다운로드하기](#)

만약 내 컨텐츠가 도움이 되었다면, 더 많은 컨텐츠를 만드는 데 지원하기 위해 커피 한 잔 사주시기를 고려해주세요.

![Gemini Chatbot](/assets/img/2024-08-03-BuildAGoogleGeminiChatbotwithHTMLCSSandJavaScript_1.png)