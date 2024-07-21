---
title: "웹 서비스 서버 사이드 콘텐츠와 클라이언트 사이드 콘텐츠의 비교"
description: ""
coverImage: "/assets/img/2024-07-21-WebservicesServer-sidevsClient-sidecontent_0.png"
date: 2024-07-21 11:23
ogImage: 
  url: /assets/img/2024-07-21-WebservicesServer-sidevsClient-sidecontent_0.png
tag: Tech
originalTitle: "Web services Server-side vs Client-side content"
link: "https://medium.com/@mnghamaty/web-services-server-side-vs-client-side-content-73fde9008f0c"
---



![웹서비스 서버 측 대 클라이언트 측 콘텐츠](/assets/img/2024-07-21-WebservicesServer-sidevsClient-sidecontent_0.png)

컴퓨터에게 클라이언트 웹 브라우저에 콘텐츠를 전달하는 방법은 수없이 많습니다. 이는 대부분 콘텐츠 유형, 제공하려는 보안 수준 및 서버와 클라이언트 간의 콘텐츠 교환과 관련된 기본 도구에 따라 달라집니다.

![웹서비스 서버 측 대 클라이언트 측 콘텐츠](/assets/img/2024-07-21-WebservicesServer-sidevsClient-sidecontent_1.png)

JinjaX를 사용하여 정적으로 컴파일된 HTML 페이지를 생성하고 JavaScript Custom Elements를 사용하여 캡슐화된 동적 콘텐츠를 보유하는 예시를 살펴보겠습니다.


<div class="content-ad"></div>

아래는 페이지에 사용자 지정 요소를 첨부하는 스크립트입니다.

```js
window.onload = () => {

  let holder = document.getElementById('main-content');
  let dc = DynamicClient(my_urls.dc_url);

  dc.refresh_content();
}
```