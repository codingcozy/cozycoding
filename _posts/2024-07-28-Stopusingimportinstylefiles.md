---
title: "스타일 파일에서 import 사용을 중단해야 하는 이유"
description: ""
coverImage: "/assets/img/2024-07-28-Stopusingimportinstylefiles_0.png"
date: 2024-07-28 13:45
ogImage: 
  url: /assets/img/2024-07-28-Stopusingimportinstylefiles_0.png
tag: Tech
originalTitle: "Stop using import in style files"
link: "https://medium.com/javascript-in-plain-english/stop-using-import-in-style-files-8a4b5950bb3f"
---


![이미지](/assets/img/2024-07-28-Stopusingimportinstylefiles_0.png)

카스케이딩 스타일 시트(CSS)는 HTML 요소의 스타일을 정의하는 스타일시트 언어로, 폰트 크기, 폰트, 색상 등을 설정할 수 있습니다. SCSS (Sassy CSS)는 CSS 위에 구축된 전처리기 스크립트 언어입니다. SCSS는 확장 가능한 스타일시트를 쉽게 작성할 수 있도록 하는 고급 Sass 구문입니다. SCSS는 변수, 중첩, 믹스인, 함수와 같은 CSS에 없는 기능과 기능을 제공합니다.

대규모 프로젝트에서 작업할 때, 단일 스타일 파일을 관리하고 수정하는 것은 어려울 수 있습니다. 스타일을 사용 목적에 따라 별도의 파일로 분할하여 보다 쉽게 만들 수 있습니다. 현재 작업 파일에 다른 파일에서 스타일을 추가해야 하는 경우, @import를 사용하여 간단히 가져올 수 있습니다. 그러나 여러분이 알지 못할 다른 옵션이 있습니다. 바로 @use입니다.

둘 다 동일한 작업을 수행할 수 있다면 언제 어떻게 사용해야 할지 궁금할 수 있습니다. 이 기사에서 그 차이를 배울 수 있습니다.

<div class="content-ad"></div>

## @use 대비 @import

사용 방법은 동일하지만, 유일한 차이점은 멤버를 로드하고 처리하는 방식입니다.

이 @import는 현재 가져오는 파일에서 모든 것을 전역적으로 접근할 수 있게 만듭니다. 이는 변수와 믹스인이 어디에 있는지 추적하기 어렵게 만듭니다...