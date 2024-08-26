---
title: "CSS와 JavaScript를 사용하여 다크 모드로 웹사이트 전환하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 20:07
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "How to Switch Your Website to Dark Mode Using CSS and JavaScript"
link: "https://dev.to/msarabi/how-to-switch-your-website-to-dark-mode-using-css-and-javascript-670"
isUpdated: false
---


## 소개

다크 모드는 어두운 배경에 밝은 텍스트와 요소를 사용하는 디스플레이 설정입니다. 디자인이 멋있어 보이고 여러 가지 실용적인 장점을 제공하기 때문에 인기를 얻고 있습니다.

다크 모드의 장점은 다음과 같습니다:

- 눈 노이 증소: 특히 어두운 환경에서는 밝은 빛을 화면에서 방출하는 양을 줄이기 때문에, 눈에 덜 스트레스를 주는 경우가 많습니다.
- OLED 화면에서 배터리 수명 개선: OLED(유기 발광 다이오드) 화면에서는 다크 모드로 사용할 때 검은 색 픽셀이 사실상 꺼져 있어서 밝은 색상을 표시할 때 보다 적은 전력을 소비하여 배터리 수명을 연장할 수 있습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이 튜토리얼에서는 CSS와 JavaScript를 사용하여 웹 사이트를 다크 모드로 전환하는 방법을 다룰 것입니다. 우리는 간단한 밝은 테마의 웹 페이지 템플릿으로 시작하여 사용자가 부드럽게 라이트 및 다크 테마 간에 전환할 수 있는 토글 가능한 라이트/다크 모드를 가진 웹 사이트로 변형할 것입니다.

## 프로젝트 설정

간단한 밝은 테마의 웹 페이지 템플릿부터 시작해봅시다. 그런 다음 사용자가 부드럽게 라이트 및 다크 테마 간에 전환할 수 있는 토글 가능한 라이트/다크 모드를 가진 웹 사이트로 변형할 것입니다.

## 다크 모드 스타일 구현

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 색상 선택하기

먼저 사용할 모든 색상을 나열하고 각 색상에 대한 다크 테마 버전을 선택하십시오. 아래 표에는 페이지의 모든 색상과 해당 다크 버전이 나열되어 있습니다.

### 사용자 정의 변수

그런 다음 CSS 변수를 사용하여 해당 변수로 본문에 다크 및 라이트 클래스를 만듭니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
body.dark {
    --body-bg: #121212;
    --primary-text: #e0e0e0;
    --header-footer-bg: #1f1f1f;
    --header-footer-text: #ffffff;
    --section-bg: #1f1f1f;
    --secondary-text: #1e90ff;
    --shadow: rgba(0, 0, 0, 0.2);
}

body.light {
    --body-bg: #f4f4f4;
    --primary-text: #333333;
    --header-footer-bg: #333333;
    --header-footer-text: #ffffff;
    --section-bg: #ffffff;
    --secondary-text: #006baf;
    --shadow: rgba(0, 0, 0, 0.1);
}
```

CSS 변수에 대해 'Using CSS custom properties'에서 자세히 읽을 수 있어요. 기본적으로 이들은 문서 내에서 재사용할 값을 저장하는 데 사용되는 두 개의 대시 (--)를 사용하여 정의된 엔티티입니다. CSS 변수를 사용하면 변경 사항이 자동으로 업데이트되어 유지 관리가 쉬워집니다.

### 동적 요소 색상

var() CSS 함수를 사용하여 CSS 변수의 값을 삽입합니다. 이 방법으로 하면 색상을 동적으로 변경하고 문서 전체에 걸쳐 변경 사항을 반영하는 하나의 변수를 업데이트할 수 있습니다. 수동으로 각 요소를 변경하는 대신 변경 사항이 전체 문서에 자동으로 적용됩니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

다음은 nav 요소와 그 자식 요소의 예시입니다.

```js
body {
  background-color: var(--body-bg);
  color: var(--primary-text);
}

header, footer {
  background-color: var(--header-footer-bg);
  color: var(--header-footer-text);
}

article {
  background-color: var(--section-bg);
  box-shadow: 0 4px 8px var(--shadow);
}

a {
  color: var(--secondary-text);
}
```

### JavaScript로 라이트/다크 모드 전환하기

이제 body의 클래스를 dark 또는 light로 변경하여 테마를 전환할 수 있습니다. 먼저, 헤더에 버튼을 추가하고 해당 클릭 이벤트에 changeTheme() 함수를 설정하세요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

여기 웹 사이트 테마를 변경할 수 있는 버튼이 있습니다. 

마크다운 형식으로 테이블 태그를 변경해보겠습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

아래의 코드 펜에서 해당 튜토리얼의 코드를 확인할 수 있어요

## 다음 단계

더불어, 사용자의 테마 선호도를 로컬 스토리지에 저장할 수도 있어요. changeTheme() 함수를 업데이트하여 선택한 테마를 저장하고 페이지를 로드할 때 해당 테마를 확인하면, 사용자의 선택이 기억되고 다음 방문 시에 자동으로 적용됩니다.

```js
function changeTheme() {
    if (document.body.classList.contains('light')) {
        document.body.classList.remove('light');
        document.body.classList.add('dark');
    } else {
        document.body.classList.remove('dark');
        document.body.classList.add('light');
    }

    // Save the current theme in localStorage
    const theme = document.body.classList.contains('dark') ? 'dark' : 'light';
    localStorage.setItem('theme', theme);
}

document.addEventListener('DOMContentLoaded', function () {
    // Get the saved theme from localStorage when the page loads
    const savedTheme = localStorage.getItem('theme') || 'light';
    document.body.classList.add(savedTheme);
});
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

어두운 테마에서 table 태그를 Markdown 형식으로 변경하면 브라우저가 스타일을 변경해야 하는 일부 요소의 처리를 보장할 수 있습니다.

```js
body.dark {
  color-scheme: dark;
}
```

## 결론

결론적으로, 웹 사이트에 어두운 모드를 추가하면 눈의 스트레스를 줄이고 OLED 스크린에서 배터리 수명을 연장하여 사용자 경험을 향상시킬 수 있습니다. 본 안내를 따라 CSS와 JavaScript를 사용하여 쉽게 라이트/다크 모드 토글을 설정할 수 있습니다. 당신의 디자인에 맞게 어두운 모드를 사용자 정의하세요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

아래 댓글란에 구현 내용을 공유하거나 질문을 하세요.