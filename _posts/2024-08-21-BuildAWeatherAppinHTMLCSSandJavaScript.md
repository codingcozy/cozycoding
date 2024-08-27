---
title: "HTML, CSS, JavaScript로 날씨 앱 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-BuildAWeatherAppinHTMLCSSandJavaScript_0.png"
date: 2024-08-21 18:47
ogImage: 
  url: /assets/img/2024-08-21-BuildAWeatherAppinHTMLCSSandJavaScript_0.png
tag: Tech
originalTitle: " Build A Weather App in HTML CSS and JavaScript"
link: "https://dev.to/codingnepal/build-a-weather-app-in-html-css-and-javascript-5e9e"
isUpdated: true
updatedAt: 1724246063284
---


<img src="/assets/img/2024-08-21-BuildAWeatherAppinHTMLCSSandJavaScript_0.png" />

날씨 앱을 만드는 것은 초보 웹 개발자들에게 훌륭한 프로젝트입니다. 이것은 기본적인 기술을 강화하는 데 도움이 되며 JavaScript에서 API의 실용적인 사용을 소개합니다. 외부 소스에서 데이터를 가져와 표시하는 작업을 통해 실전 경험을 쌓게 됩니다. 이러한 기술은 실제 응용 프로그램을 개발하는 데 필수적입니다.

이 블로그 포스트에서는 HTML, CSS, JavaScript를 사용하여 아름답고 인터랙티브한 날씨 앱을 만드는 방법을 안내해 드리겠습니다. 이 앱을 사용하면 사용자들이 임의의 도시의 날씨를 확인하거나 현재 위치를 사용하여 날씨 예보를 얻을 수 있습니다. 이 앱은 24시간 동안 실시간 날씨 업데이트와 예보를 제공할 것입니다.

이 안내서를 마치면 코딩 기술을 향상시킬 뿐만 아니라 구체적으로 사용하고 소개할 수 있는 실용적인 도구를 제공하는 완전히 기능적인 날씨 앱이 준비될 것입니다. 이것을 포트폴리오에 추가하거나 새로운 기술을 실험하더라도, 이 프로젝트는 당신의 웹 개발 여정에서 가치 있는 한 걸음입니다.

<div class="content-ad"></div>

## HTML 및 JavaScript를 사용한 날씨 앱 비디오 튜토리얼

위의 YouTube 비디오는 비디오 강좌를 선호하는 경우 훌륭한 자료입니다. 각 코드 라인을 설명하고 주석을 제공하여 당신의 날씨 앱 프로젝트를 따라가기 쉽게 만듭니다. 독해를 선호하거나 단계별 가이드가 필요한 경우에는 계속해서 이 게시물을 따라주십시오.

## HTML 및 JavaScript를 사용한 날씨 앱 구축 단계

HTML, CSS 및 JavaScript를 사용하여 기능적인 날씨 앱을 구축하려면 다음 간단한 단계별 지침을 따르십시오:

<div class="content-ad"></div>

- 마음에 드는 이름의 폴더를 만드세요. 예: weather-app.
- 그 안에 index.html, style.css 및 script.js와 같은 필수 파일을 만드세요.
- 아이콘 폴더를 다운로드하여 프로젝트 디렉토리에 넣으세요. 해당 폴더에는 날씨 상태에 필요한 아이콘이 포함되어 있습니다.

index.html 파일에는 날씨 앱 레이아웃을 구성하는 필수 HTML 마크업을 추가하세요. 이에는 도시 이름을 검색하는 입력란, 위치 버튼, 현재 날씨 및 시간별 예보를 표시하는 섹션이 모두 시멘틱 태그로 구조화되어야 합니다.

```js
<!DOCTYPE html>
<!-- Coding By CodingNepal - youtube.com/@codingnepal -->
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather App | CodingNepal</title>
  <!-- 아이콘을 위한 Google 폰트 링크 -->
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Rounded:opsz,wght,FILL,GRAD@24,400,0,0" />
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <!-- 검색 섹션 -->
    <div class="search-section">
      <div class="input-wrapper">
        <span class="material-symbols-rounded">search</span>
        <input type="search" placeholder="도시 이름을 입력하세요" class="search-input">
      </div>
      <button class="location-button">
        <span class="material-symbols-rounded">my_location</span>
      </button>
    </div>

    <!-- 결과 없음 메시지 -->
    <div class="no-results">
      <img src="icons/no-result.svg" alt="결과 없음" class="icon">
      <h3 class="title">문제가 발생했습니다!</h3>
      <p class="message">날씨 정보를 검색할 수 없습니다. 올바른 도시를 입력했는지 확인하거나 나중에 다시 시도하세요.</p>
    </div>

    <!-- 날씨 데이터 표시를 위한 컨테이너 -->
    <div class="weather-section">
      <div class="current-weather">
        <img src="icons/no-result.svg" class="weather-icon">
        <h2 class="temperature">00<span>°C</span></h2>
        <h5 class="description">날씨 설명</h5>
      </div>

      <!-- 시간별 날씨 예보 목록 -->
      <div class="hourly-weather">
        <ul class="weather-list"></ul>
      </div>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>
```

style.css 파일에는 날씨 앱을 스타일링하고 반응형 및 시각적으로 매력적인 디자인을 제공하는 CSS 코드를 추가하세요. 다양한 색상, 폰트 및 배경을 사용하여 인터페이스를 사용자 친화적이고 매력적으로 만들어보세요.

<div class="content-ad"></div>


/* 구글 폰트(몽세라트) 가져오기 */
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat", sans-serif;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: #5f41e4;
}

.container {
  flex-grow: 1;
  overflow: hidden;
  max-width: 425px;
  border-radius: 10px;
  position: relative;
  background: #fff;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
}

.search-section {
  display: flex;
  gap: 10px;
  padding: 25px;
  align-items: center;
}

... 중략 ...

// 주의: 날씨 앱은 날씨 API와 연결하기 전까지 작동하지 않습니다. WeatherAPI에서 무료 API 키를 등록하고 해당 키를 script.js 파일의 API_KEY 변수에 추가하세요. 이 키는 실시간 및 시간별 예보를 가져 올 수 있도록 앱에 권한을 부여합니다. 이 키는 다음과 같은 형식입니다: 5f2b6b5a24044bcdbbc103609241008.


<div class="content-ad"></div>

API 키를 코드에 추가하면 날씨 앱을 사용할 준비가 끝났어요. 브라우저에서 index.html 파일을 열고 도시 이름을 입력하거나 위치를 사용하여 날씨를 확인할 수 있어요.

## 결론과 마지막으로

요약하면, HTML, CSS 및 JavaScript로 날씨 앱을 만드는 것은 초보자가 핵심 웹 개발 기술을 향상시키고 API와 관련된 소중한 경험을 쌓는 좋은 방법이에요. 이 안내서를 따라가면 기능적이고 시각적으로 매력적인 날씨 앱을 만들어 포트폴리오에 완벽하게 추가할 수 있어요.

기술을 더 향상시키기 위해 환율 변환기, 이미지 검색 엔진, Gemini 챗봇 등 다른 관련 JavaScript 프로젝트를 살펴볼 수 있어요. 이러한 프로젝트마다 HTML 및 CSS에 대한 이해를 향상시키고 더 나은 스타일링 기술과 API 통합에 대한 실무 경험을 얻을 수 있어요.

<div class="content-ad"></div>

만약 날씨 앱을 개발하면서 문제가 발생하면, "다운로드" 버튼을 클릭하여 이 프로젝트의 소스 코드 파일을 다운로드할 수 있어요.

다운로드 코드 파일