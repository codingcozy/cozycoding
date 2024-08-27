---
title: "HTML, CSS, JavaScript로 슬라이더 웹사이트 만드는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 20:13
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Build a Testimonials Slider Website"
link: "https://dev.to/abhishekgurjar/build-a-testimonials-slider-website-3d61"
isUpdated: true
updatedAt: 1724743710487
---


## 소개

안녕하세요, 개발자 여러분! 제가 최신 프로젝트인 Testimonials Slider를 소개하게 되어 기쁩니다. 이 프로젝트는 JavaScript를 사용하여 인터랙티브하고 동적인 웹 구성 요소를 만드는 기술을 향상시키는 좋은 방법입니다. 여러분이 처음 시작하거나 포트폴리오에 새로운 기능을 추가하려는 경우, Testimonials Slider 프로젝트는 프론트엔드 개발에 더 깊이 매인 좋은 기회를 제공합니다.

## 프로젝트 개요

Testimonials Slider는 사용자가 다음 및 이전 버튼을 사용하여 다양한 추천사를 탐색할 수 있는 웹 기반 응용 프로그램입니다. 이 프로젝트는 인터랙티브한 사용자 인터페이스를 만드는 방법, JavaScript로 상태를 관리하는 방법, 그리고 부드러운 전환을 통해 사용자 경험을 향상하는 방법을 보여줍니다.

<div class="content-ad"></div>

## 기능

- 상호 작용형 추천 사례: 사용자는 내비게이션 버튼을 사용하여 여러 추천 사례를 탐색할 수 있습니다.
- 부드러운 전환 효과: 추천 사례가 부드러운 전환과 함께 변경되어 좋은 사용자 경험을 제공합니다.
- 반응형 디자인: 다양한 기기에서 일관된 시각적으로 매력적인 경험을 보장합니다.

## 사용된 기술

- HTML: 웹 페이지 및 추천 사례 요소를 구조화합니다.
- CSS: 깨끗하고 반응형 디자인을 보장하기 위해 사용자 인터페이스를 스타일링합니다.
- JavaScript: 추천 사례 탐색 및 사용자 상호작용을 관리합니다.

<div class="content-ad"></div>

## 프로젝트 구조

프로젝트 구조를 간단히 살펴보세요:

```js
Testimonials-Slider/
├── index.html
├── style.css
└── script.js
```

- index.html: Testimonials Slider의 HTML 구조를 포함합니다.
- style.css: 애플리케이션의 모습과 반응성을 향상시키는 CSS 스타일이 포함되어 있습니다.
- script.js: Testimonial 네비게이션 로직 및 사용자 상호작용을 관리합니다.

<div class="content-ad"></div>

## 설치

프로젝트를 시작하려면 다음 단계를 따르세요:

- 저장소를 복제합니다:
git clone https://github.com/abhishekgurjar-in/Testimonials-Slider.git
- 프로젝트 디렉토리를 엽니다:
cd Testimonials-Slider
- 프로젝트를 실행합니다:
테스티모니얼 슬라이더를 사용하려면 웹 브라우저에서 index.html 파일을 엽니다.
- 테스티모니얼 슬라이더를 사용하려면 웹 브라우저에서 index.html 파일을 엽니다.

## 사용법

<div class="content-ad"></div>

- 웹 브라우저에서 웹 사이트를 열어 주세요.
- 다양한 추천 사례를 확인하려면 "다음" 또는 "이전" 버튼을 클릭하여 탐색해주세요.
- 추천 사례를 이동하면서 부드러운 전환 효과를 즐겨보세요.

## 코드 설명

### HTML

index.html 파일은 추천 사례 슬라이더의 기본 구조를 제공합니다. 여기에는 추천 사례 내용과 탐색 버튼이 포함되어 있습니다. 다음은 코드 예시입니다:

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>테스트 모달</title>
    <link rel="stylesheet" href="style.css" />
    <script src="script.js" defer></script>
  </head>
  <body>
    <div class="container">
      <div class="box-1" id="testimonial-1">
        <div class="text">
          <h1>
            “가능한 가장 좋은 기반을 마련하려면이 코스를 추천합니다. 강사들이 다루는 내용은 놀라울 정도로 심층적입니다. 이제 전문적인 개발자로서 시작하는 데 대해 매우 자신감을 가집니다.”
          </h1>
          <div class="name">
            <h3>존 타크퍼</h3>
            <h4>초급 프론트엔드 개발자</h4>
          </div>
        </div>
        <div class="image">
          <img src="./images/image-john.jpg" alt="존의 추천서" />
          <div class="button">
            <img src="./images/icon-prev.svg" id="prev-1" alt="이전" />
            <img src="./images/icon-next.svg" id="next-1" alt="다음" />
          </div>
        </div>
      </div>
      <!-- 추가적인 추천서가 여기에 나타날 수 있습니다 -->
    </div>
    <div class="footer">
      <p>Abhishek Gurjar님에 의해 ❤️️ 제작되었습니다.</p>
    </div>
  </body>
</html>
```

### CSS

style.css 파일은 테스트 모달을 스타일링하여 현대적이고 사용자 친화적인 레이아웃을 제공합니다. 여기에 몇 가지 주요 스타일이 있습니다:

```js
* {
  box-sizing: border-box;
}

body {
  font-family: Inter, sans-serif;
  margin: 0;
  padding: 0;
}

.container {
  width: 100%;
  height: 90vh;
  background: url(./images/pattern-curve.svg) no-repeat fixed left bottom;
  display: flex;
  align-items: center;
  justify-content: center;
}

.box-1 {
  width: 70%;
  height: 70%;
  background-color: transparent;
  display: none; /* 초기에 모든 추천서 숨김 */
}

#testimonial-1 {
  display: flex; /* 첫 번째 추천서 표시 */
}

/* 추가 스타일 */
```

<div class="content-ad"></div>

### 자바스크립트

script.js 파일은 평가 및 사용자 상호 작용 처리를 위한 로직을 관리합니다. 다음은 일부 코드입니다:

```js
document.addEventListener("DOMContentLoaded", function () {
  const testimonials = document.querySelectorAll(".box-1");
  let currentIndex = 0;

  const showTestimonial = (index) => {
    testimonials.forEach((testimonial, i) => {
      testimonial.style.display = i === index ? "flex" : "none";
    });
  };

  document.getElementById("next-1").addEventListener("click", () => {
    currentIndex = (currentIndex + 1) % testimonials.length;
    showTestimonial(currentIndex);
  });

  document.getElementById("prev-1").addEventListener("click", () => {
    currentIndex = (currentIndex - 1 + testimonials.length) % testimonials.length;
    showTestimonial(currentIndex);
  });

  // 추가적인 자바스크립트 로직
});
```

## 실시간 데모

<div class="content-ad"></div>

위의 표를 마크다운 형식으로 바꿔주시면 됩니다.

<div class="content-ad"></div>

이 프로젝트는 웹 개발 학습 여정의 일환으로 개발되었습니다. 인터랙티브한 사용자 인터페이스를 만드는 데 중점을 두었습니다.

## 저자

- Abhishek Gurjar
  GitHub 프로필