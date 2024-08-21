---
title: "오늘 좋게 보이는 5가지 이유"
description: ""
coverImage: "/assets/img/2024-07-02-Youlookgoodtoday_0.png"
date: 2024-07-02 22:32
ogImage: 
  url: /assets/img/2024-07-02-Youlookgoodtoday_0.png
tag: Tech
originalTitle: "You look good today!"
link: "https://medium.com/@gianlucasartori/you-look-good-today-5c0438fbe2ff"
isUpdated: true
---





**고지: 우리는 그것이 무엇인지는 상관하지 않습니다. 우리는 그것이 어떻게 보이는지에 중점을 둡니다. 왜냐하면 우리는 속을 수 있는 인간이기 때문입니다.**

회사들은 브랜드 아이덴티티에 투자합니다. 또한 브랜드 인지도에도 투자하는데, 이것은 기본적으로 회사 브랜드를 직원들에게 판매하는 것을 의미합니다. 이것은 단순히 한 가지 활동이 아니지만, 고객이 매일 사용하는 애플리케이션에 그들의 로고를 넣어 달라고 요청할 것입니다.

# 브랜드는 로고입니다

모든 회사에는 로고가 있습니다. 이 튜토리얼을 위해 우리는 Google에 로고를 요청하고 있습니다.

<div class="content-ad"></div>


![You Look Good Today 0](/assets/img/2024-07-02-Youlookgoodtoday_0.png)

![You Look Good Today 1](/assets/img/2024-07-02-Youlookgoodtoday_1.png)

We also need a background for the login screen. Let’s ask Google this one too.

![You Look Good Today 2](/assets/img/2024-07-02-Youlookgoodtoday_2.png)


<div class="content-ad"></div>


![You look good today](/assets/img/2024-07-02-Youlookgoodtoday_3.png)

```js
/demo/src/main/resources/deploy/public/brand
 - login-logo.png
 - login-background.jpg
 - logo.png
 - favicon.png
 - appicon.png
```

다음부터 애플리케이션을 처음으로 배포할 때마다, 이 파일들이 추출되어 다음 폴더에 추가될 것입니다: ~/demo/demo/tenants/DEFAULT/public/brand

우리가 한 노력의 결과를 보기 위해서는 이미 설치된 기본 브랜딩을 ~/demo/demo 폴더 아래서 제거해야 합니다.


<div class="content-ad"></div>

![You Look Good Today](/assets/img/2024-07-02-Youlookgoodtoday_4.png)

우리는 이렇게 무언가를 볼 수 있어야 해요. 멋있어 보이지만... 색을 고쳐야 해요.

# 브랜드는 색상입니다

Dueuno Elements 애플리케이션은 테넌트 속성 (시스템 관리 - 설정)에서 구성할 수 있습니다. 이는 각 테넌트가 다른 설정을 가질 수 있다는 것을 의미합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-02-Youlookgoodtoday_5.png)

각 Dueuno Elements 애플리케이션은 세 가지 다른 색상으로 구성될 수 있습니다:

- PRIMARY
기본 작업에 사용되는 색상. 주요 작업은 주 버튼, "생성"하는 버튼 또는 사용자가 해당 컨텍스트에서 중요한 것으로 보게 하려는 버튼입니다.
- SECONDARY
일반적인 작업을 수행하는 나머지 모든 버튼에 사용되는 색상입니다.
- TERTIARY
콘텐츠 및 양식 필드에 사용되는 색상입니다.

각 색상은 세 가지 값이 있습니다:


<div class="content-ad"></div>

- 배경 색상
요소의 배경에 사용되는 색상입니다.
- 배경색상 알파
투명도 지수입니다. 기본 색상이 평면 버전이 적절하지 않을 때 주요 색상에 음영을 입히는 데 사용됩니다.
- 텍스트 색상
텍스트에 사용되는 색상입니다.

우리는 애플리케이션을 설치할 때마다 수동으로 색상을 설정하고 싶지 않습니다. onInstall 메서드를 사용하여 테넌트 속성을 설정할 수 있습니다.

또한 로그인 폼 아래에 일부 복사를 추가하여 사용자가 우리가 누구인지 알 수 있도록 웹 사이트로 연결을 제공할 예정입니다.

```js
class BootStrap {

    ApplicationService applicationService
    TenantPropertyService tenantPropertyService

    ...

    def init = { servletContext ->

        applicationService.onInstall { String tenantId ->
            tenantPropertyService.setString('PRIMARY_BACKGROUND_COLOR', '#018B84')
            tenantPropertyService.setNumber('PRIMARY_BACKGROUND_COLOR_ALPHA', 0.25)
            tenantPropertyService.setString('LOGIN_COPY', '2024 &copy; <a href="https://my-company.com" target="_blank">My Company</a><br/>Made in Italy')
        }

    ...

}
```

<div class="content-ad"></div>

아래는 마크다운 형식으로 변환되었습니다.


![You look good today](/assets/img/2024-07-02-Youlookgoodtoday_6.png)

# 브랜드는 어디에나 있습니다

우리는 최종 사용자뿐만 아니라 모든 사람이 이 브랜드를 인식하도록 하고 싶어요! 그래서 우리는 우리의 로그에 배너를 추가하고 싶습니다.

애플리케이션이 재시작될 때마다 회사 이름을 나타내는 ASCII Art가 표시됩니다.


<div class="content-ad"></div>

아주 멋진 ASCII Art를 만드는 데 시간을 할애할 수 있어요. 저는 좀 게으르니까 다음 ASCII Text 생성기를 사용하겠어요: https://patorjk.com/software/taag

```js
 __  __          ____
|  \/  |_   _   / ___|___  _ __ ___  _ __   __ _ _ __  _   _
| |\/| | | | | | |   / _ \| '_ ` _ \| '_ \ / _` | '_ \| | | |
| |  | | |_| | | |__| (_) | | | | | | |_) | (_| | | | | |_| |
|_|  |_|\__, |  \____\___/|_| |_| |_| .__/ \__,_|_| |_|\__, |
        |___/                       |_|                |___/
```

```js
18:47:30.769 INFO  [restartedMain] o.s.boot.SpringApplication               : 
  __  __          ____
 |  \/  |_   _   / ___|___  _ __ ___  _ __   __ _ _ __  _   _
 | |\/| | | | | | |   / _ \| '_ ` _ \| '_ \ / _` | '_ \| | | |
 | |  | | |_| | | |__| (_) | | | | | | |_) | (_| | | | | |_| |
 |_|  |_|\__, |  \____\___/|_| |_| |_| .__/ \__,_|_| |_|\__, |
         |___/                       |_|                |___/


Spring Security Core 구성 중...
...Spring Security Core 구성 완료

18:47:36.979 INFO  [restartedMain] d.elements.core.ApplicationService       : 사용 가능한 언어 [en, it]
18:47:36.982 INFO  [restartedMain] d.elements.core.ApplicationService       : 
18:47:36.982 INFO  [restartedMain] d.elements.core.ApplicationService       : --------------------------------------------------------------------------------
18:47:36.982 INFO  [restartedMain] d.elements.core.ApplicationService       : 어플리케이션: 시작 중...
18:47:36.982 INFO  [restartedMain] d.elements.core.ApplicationService       : --------------------------------------------------------------------------------
18:47:36.987 INFO  [restartedMain] d.elements.core.ApplicationService       : 'dueuno.elements.core.beforeInit' 실행 중...
18:47:37.029 INFO  [restartedMain] d.elements.core.ApplicationService       : 'com.example.init' 실행 중...
18:47:37.030 INFO  [restartedMain] d.elements.core.ApplicationService       : 'dueuno.elements.core.afterInit' 실행 중...
18:47:37.042 INFO  [restartedMain] d.elements.core.ApplicationService       : --------------------------------------------------------------------------------
18:47:37.042 INFO  [restartedMain] d.elements.core.ApplicationService       : 어플리케이션: 시작됨.
18:47:37.042 INFO  [restartedMain] d.elements.core.ApplicationService       : --------------------------------------------------------------------------------
18:47:37.042 INFO  [restartedMain] d.elements.core.ApplicationService       : 
Grails 어플리케이션이 http://localhost:8080에서 개발 환경으로 실행 중입니다.
```

<div class="content-ad"></div>

# 결론

우리는 고객을 행복하게 만들었습니다. 그 의미는 우리도 행복하다는 것이죠. 더 필요한 게 무엇이 있을까요?

다음 기사에서는 데스크탑 컴퓨터, 태블릿, 휴대전화에서 Dueuno Elements 애플리케이션을 사용할 때 어떤 일이 벌어지는지 살펴볼 예정입니다.