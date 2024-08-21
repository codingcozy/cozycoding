---
title: "HTML과 TailwindCSS로 반응형 포트폴리오 웹사이트 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-BuildaResponsivePortfolioWebsitewithHTMLandTailwindCSSABeginnersGuide_0.png"
date: 2024-08-21 16:57
ogImage: 
  url: /assets/img/2024-08-21-BuildaResponsivePortfolioWebsitewithHTMLandTailwindCSSABeginnersGuide_0.png
tag: Tech
originalTitle: "Build a Responsive Portfolio Website with HTML and TailwindCSS A Beginners Guide"
link: "https://medium.com/javascript-in-plain-english/build-a-responsive-portfolio-website-with-html-and-tailwindcss-a-beginners-guide-22d7b5759b47"
isUpdated: true
updatedAt: 1724246053987
---


<img src="/assets/img/2024-08-21-BuildaResponsivePortfolioWebsitewithHTMLandTailwindCSSABeginnersGuide_0.png" />

# 소개:

현재의 디지털 시대에서 개발자, 디자이너 및 크리에이터들에게 잘 디자인된 포트폴리오 웹사이트를 갖는 것은 매우 중요합니다. 이는 여러분의 온라인 비즈니스 카드로서 여러분의 기술, 프로젝트 및 경험을 잠재적인 고객이나 고용주에게 쇼케이스합니다. 포트폴리오를 만들기 위해 다양한 도구와 플랫폼이 있지만, HTML 및 TailwindCSS를 사용하여 스크래치부터 직접 만드는 것은 디자인과 기능에 대한 완전한 제어를 제공합니다. 이 기사에서는 여러분을 단계별로 안내하여 반응형이고 시각적으로 매력적인 포트폴리오 웹사이트를 만드는 과정을 안내하겠습니다.

# TailwindCSS를 사용하는 이유?

<div class="content-ad"></div>

TailwindCSS는 웹사이트를 빠르고 효율적으로 디자인할 수 있게 해주는 유틸리티 중심의 CSS 프레임워크입니다. 전통적인 CSS 프레임워크와 달리 Tailwind는 미리 정의된 구성 요소나 테마가 없습니다. 대신, 사용자 정의 디자인을 만들기 위해 혼합 및 일치시킬 수 있는 저수준의 유틸리티 클래스를 제공합니다. 이는 매우 유연하며 독특하고 개인적인 포트폴리오 웹사이트를 만들기에 이상적입니다.

# 요구 사항:

튜토리얼에 들어가기 전에 HTML 및 CSS에 대한 기본적인 이해가 필요합니다. TailwindCSS에 익숙하다면 좋지만 필수는 아닙니다. 필요한 모든 것을 다룰 것이기 때문입니다.

# 완전한 블로그를 읽기를 원하지 않는다면 비디오 튜토리얼 이용하기

<div class="content-ad"></div>

# 단계 1: 프로젝트 설정하기

먼저 프로젝트 디렉토리를 설정하고 TailwindCSS를 포함해보겠습니다. 컴퓨터에 새 폴더를 만들고 터미널을 통해 해당 폴더로 이동하세요. 폴더 안에 index.html 파일을 생성하여 HTML 코드를 작성할 것입니다.

다음으로 TailwindCSS를 설치해야 합니다. 이 작업은 HTML 파일의 head 섹션에 TailwindCDN을 포함시킴으로써 가장 쉽게 할 수 있습니다:

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <script src="https://cdn.tailwindcss.com"></script>
    <script src="./tailwind.config.js"></script>

    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap"
      rel="stylesheet"
    />

    <link rel="stylesheet" href="./index.css" />

    <title>Portfolio</title>
</head>
<body>
    <!-- 여기에 내용이 들어갑니다 -->
</body>
</html>
```

<div class="content-ad"></div>

프로젝트에서 별도의 설정 없이 TailwindCSS를 직접 사용할 수 있습니다.

# 단계 3: 섹션 추가하기

기본 구조가 마련되었으므로 프로젝트 방문자에게 귀하에 대한 전체 정보를 제공하는 중요한 섹션을 추가해 봅시다.
TailwindCSS를 사용하면 HTML 요소에 직접 유틸리티 클래스를 추가하여 웹사이트를 쉽게 스타일링할 수 있습니다.

# 헤더 스타일링:

<div class="content-ad"></div>

```js
<header class="bg-secondary pt-5 pb-10 lg:pb-0">
  <nav
    class="container px-5 md:mx-auto flex flex-wrap items-center justify-between gap-5"
  >
    <img src="./assets/logo.svg" alt="포트폴리오 로고" />

    <label for="menu-toggle" class="cursor-pointer lg:hidden block">
      <svg
        class="fill-current text-gray-900"
        xmlns="http://www.w3.org/2000/svg"
        width="20"
        height="20"
        viewBox="0 0 20 20"
      >
        <title>menu</title>
        <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"></path>
      </svg>
    </label>
    <input class="hidden" type="checkbox" id="메뉴-토글" />

    <ul
      id="메뉴"
      class="hidden lg:flex w-full lg:w-auto flex-wrap items-center gap-10"
    >
      <li class="py-1 lg:py-0">홈</li>
      <li class="py-1 lg:py-0">소개</li>
      <li class="py-1 lg:py-0">블로그</li>
      <li class="py-1 lg:py-0">프로젝트</li>
      <li class="py-1 lg:py-0">비디오</li>
      <li
        class="h-10 w-10 mt-3 md:mt-0 rounded-full bg-primary text-white text-lg flex items-center justify-center"
      >
        <ion-icon name="mail"></ion-icon>
      </li>
    </ul>
  </nav>

  <!--  영웅 섹션-->
</header>
```

# 영웅 섹션:

이 섹션에는 이름 및 간단한 설명과 같은 당신에 대한 간략한 정보가 포함될 것입니다.


<div class="content-ad"></div>

```js
<div class="container px-5 md:mx-auto flex items-center gap-10 mt-10">
  <div class="w-full lg:w-1/2">
    <h3 class="font-normal mb-3 text-2xl">안녕하세요,</h3>
    <h1 class="font-bold text-5xl lg:text-8xl my-3">저는 가지입니다</h1>
    <p class="mt-5 mb-10 md:text-lg">
      사용자 친화적인 경험을 만들기에 주안점을 둔 개발자입니다. 훌륭한 소프트웨어를 만드는 것에 열정적입니다.
    </p>

    <a
      href="#"
      target="_blank"
      class="bg-primary shadow-md text-white px-5 py-2 rounded my-10"
    >
      이력서 다운로드
    </a>
  </div>

  <div class="hidden lg:block lg:w-1/2">
    <img src="./assets/character.svg" alt="가지의 아바타" />
  </div>
</div>
```

# 경험 및 기술 스택:

방문자들에게 귀하의 경험 및 사용 중인 기술 스택에 대해 알려드리는 섹션입니다.

```js
<section class="bg-primary py-5 text-white">
  <div
    class="container px-5 md:mx-auto grid grid-cols-1 md:grid-cols-3 gap-5 md:gap-10"
  >
    <div class="flex items-center gap-5">
      <h1 class="text-6xl lg:text-9xl font-bold">10</h1>
      <h2 class="text-2xl lg:text-3xl">
        경력<br />
        10년
      </h2>
    </div>
    <div
      class="col-span-2 border-t pt-8 mt-5 md:border-t-0 md:pt-0 md:mt-0 md:justify-self-end"
    >
      <h3 class="text-2xl lg:text-3xl mb-5">사용 중인 기술 스택</h3>
      <div class="flex gap-5 flex-wrap text-4xl md:text-3xl lg:text-6xl">
        <ion-icon name="logo-react"></ion-icon>
        <ion-icon name="logo-javascript"></ion-icon>
        <ion-icon name="logo-nodejs"></ion-icon>
        <ion-icon name="logo-angular"></ion-icon>
        <ion-icon name="logo-html5"></ion-icon>
        <ion-icon name="logo-css3"></ion-icon>
        <ion-icon name="logo-sass"></ion-icon>
        <ion-icon name="logo-wordpress"></ion-icon>
        <ion-icon name="logo-github"></ion-icon>
      </div>
    </div>
  </div>
</section>
```

<div class="content-ad"></div>

# 최신 블로그:

이 섹션에서는 최신 블로그 중 일부를 소개할 예정이에요

```js
<section class="container md:mx-auto px-5 py-10">
  <div class="flex items-end flex-wrap gap-10 justify-between">
    <div>
      <h1 class="text-3xl font-semibold">최신 블로그</h1>
      <p class="text-gray-500 font-light">
        소프트웨어 개발에 관련된 다양한 주제에 대해 블로그를 작성합니다
      </p>
    </div>
    <a href="#" class="underline text-primary font-normal"
      >더 많은 블로그 탐색하기</a
    >
  </div>
  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5 mt-10">
    <!-- 블로그 1-->
    <div>
      <img
        src="./assets/blog-1.png"
        alt="웹 개발자가 디지털 마케팅을 배워야하는 이유"
        class="w-full object-cover shadow-lg"
      />
      <h1 class="text-xl font-semibold mt-5">
        왜 웹 개발자는 디지털 마케팅을 배워야 하는가
      </h1>
      <p class="text-gray-400 font-light mt-2">가지 칸 | 2021년 6월 7일</p>
      <p class="font-light mt-4">
        웹 개발은 많은 재능 있는 개인들이 최상위 자리를 노리는 경쟁이 치열한 산업입니다. 경쟁 상대들을 앞지르고 싶다면 이 글을 읽어보세요.
      </p>
    </div>

    <!-- 블로그 2-->
    <!-- 블로그 3-->
  </div>
</section>
```

# 단계 4: 다른 섹션 추가하기

<div class="content-ad"></div>

모든 관련 섹션 추가

```js
<section class="bg-dark-green py-10 text-white">
  <div class="container md:mx-auto px-5">
    <h1 class="text-2xl md:text-3xl lg:text-4xl font-semibold">
      최신 블로그 및 비디오 알림 받기
    </h1>
    <div class="grid grid-cols-1 md:grid-cols-3 mt-4 gap-2">
      <input
        type="text"
        placeholder="이메일 주소 입력"
        required
        class="bg-transparent text-md border border-primary w-full col-span-2 px-5 py-2"
      />
      <button class="bg-primary px-5 py-2 shadow-md rounded">
        뉴스레터 구독하기
      </button>
    </div>
  </div>
</section>

<section class="container md:mx-auto px-5 py-16">
  <h1 class="text-center text-3xl font-semibold mb-5 text-3xl">
    소셜 미디어로 연락하기
  </h1>
  <div
    class="flex flex-wrap items-center justify-center gap-5 text-primary text-4xl lg:text-5xl"
  >
    <ion-icon name="logo-linkedin"></ion-icon>
    <ion-icon name="logo-youtube"></ion-icon>
    <ion-icon name="logo-twitter"></ion-icon>
    <ion-icon name="logo-instagram"></ion-icon>
    <ion-icon name="logo-facebook"></ion-icon>
  </div>
</section>

<footer class="bg-primary text-white py-3 text-center px-auto">
  <p>저작권 2024 | Code With Ghazi</p>
</footer>
```

# 단계 5: 포트폴리오 배포하기

포트폴리오를 완성했다면, 이제 세계가 볼 수 있도록 배포할 시간입니다! GitHub Pages, Netlify, Vercel을 포함한 여러 가지 방법으로 정적 웹사이트를 배포할 수 있습니다. 이러한 서비스들은 간편한 배포 옵션을 제공하며 종종 몇 번의 클릭으로 완료할 수 있습니다.

<div class="content-ad"></div>

# 결론:

축하합니다! HTML과 TailwindCSS를 사용하여 반응형 및 사용자 정의 가능한 포트폴리오 웹사이트를 성공적으로 만드셨습니다. 이 웹사이트는 여러분이 기술과 포트폴리오가 성장함에 따라 확장하고 맞춤화할 수 있는 견고한 기반 역할을 할 것입니다. TailwindCSS의 유틸리티 중심 접근 방식은 독특한 스타일에 맞는 디자인을 효율적으로 만들 수 있도록 도와줍니다. 이제 여러분의 포트폴리오를 세상과 공유해보세요!

만약 이 안내서가 도움이 되었다면, 더 심층적인 안내와 포트폴리오를 사용자 정의하는 추가 팁을 얻기 위해 전체 비디오 튜토리얼을 확인해보세요.

즐거운 코딩하세요! 🎉

<div class="content-ad"></div>

제가 올려준 콘텐츠를 더 많이 보시려면 팔로우해주세요:
LinkedIn: https://www.linkedin.com/in/ghazi-khan/
Twitter: https://twitter.com/ghazikhan205
YouTube: http://www.youtube.com/@codewithghazi

# 쉽게 설명한 영어로 🚀

쉽게 설명한 영어 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

- 꼭 박수를 치시고 작가를 팔로우해주세요 ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기