---
title: "1분 만에 웹사이트 클론하고 수정하는 방법"
description: ""
coverImage: "/assets/img/2024-07-27-CloneAnyWebsiteandEditAnythinginJust1Minute_0.png"
date: 2024-07-27 13:41
ogImage: 
  url: /assets/img/2024-07-27-CloneAnyWebsiteandEditAnythinginJust1Minute_0.png
tag: Tech
originalTitle: "Clone Any Website and Edit Anything in Just 1 Minute"
link: "https://dev.to/kooboo/clone-any-website-and-edit-anything-in-just-1-minute-lo"
isUpdated: true
---




웹 사이트, 전자 상거래, 및 애플리케이션 개발을 위한 강력한 도구를 개발했습니다. 내 의견으로는 이 도구가 WordPress보다 훨씬 뛰어나다고 생각합니다. 커뮤니티로부터 피드백을 기대하고 있습니다.

이것은 이 도구를 소개하는 시리즈 기사 중 첫 번째입니다

# 시작하기

Github 또는 저희 웹사이트에서 다운로드하고 클릭하여 실행하세요.

<div class="content-ad"></div>

https://www.github.com/kooboo/kooboo

https://www.kooboo.com/downloads

설치가 필요하지 않습니다. 계정을 등록하고 로그인하세요.

![이미지](/assets/img/2024-07-27-CloneAnyWebsiteandEditAnythinginJust1Minute_0.png)

<div class="content-ad"></div>

# 웹 사이트를 복제하고 직접 내용을 편집해보세요

위 화면에서 "새 사이트"를 클릭하고 원하는 URL을 입력하세요

![이미지](/assets/img/2024-07-27-CloneAnyWebsiteandEditAnythinginJust1Minute_1.png)

"Clone 시작"을 클릭하세요

<div class="content-ad"></div>

<img src="/assets/img/2024-07-27-CloneAnyWebsiteandEditAnythinginJust1Minute_2.png" />

1분 또는 2분 안에 원본 웹사이트의 완벽한 클론을 만들고 직접 모든 내용을 편집할 수 있습니다.

<img src="/assets/img/2024-07-27-CloneAnyWebsiteandEditAnythinginJust1Minute_3.png" />

# 인라인 편집

<div class="content-ad"></div>

Kooboo는 정적 또는 동적 콘텐츠, 텍스트, 이미지 또는 스타일 시트를 인라인으로 편집할 수 있습니다.

이 기능을 사용하려면 위의 스크린샷에 나와 있는대로 "관리"로 이동하여 왼쪽 메뉴에서 "페이지"를 클릭한 다음 페이지를 선택하고 "인라인 편집"을 클릭하십시오.

이제 아무 곳이나 클릭하여 내용을 직접 변경할 수 있는 인라인 편집 모드에 있습니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-27-CloneAnyWebsiteandEditAnythinginJust1Minute_5.png)

# 소스 코드 IDE 편집

Kooboo 개발 모드는 개발, 디버깅 및 배포를 위한 완전 기능을 갖춘 웹 IDE에 액세스할 수 있습니다.

![이미지](/assets/img/2024-07-27-CloneAnyWebsiteandEditAnythinginJust1Minute_6.png)

<div class="content-ad"></div>

이 템플릿 엔진은 VueJs 구문과 완전히 호환되며, env=server 태그를 사용하여 서버 측에서 JavaScript를 실행할 수 있습니다.

아래 예시 코드는 데이터베이스에서 콘텐츠 목록을 읽어와 리스트로 표시합니다.

```js
<script env="server">
    var services = k.content.service.all()
</script>
<div env="server" v-for="service in services">
    <span>{service.title}</span>
</div>
```

# DOM 트리 편집

<div class="content-ad"></div>

Kooboo를 사용하면 전체 웹사이트를 단일 Dom 트리처럼 편집할 수 있어요. 변경 사항은 동일한 Dom 구조를 가진 여러 페이지에 업데이트됩니다.

![이미지](/assets/img/2024-07-27-CloneAnyWebsiteandEditAnythinginJust1Minute_7.png)