---
title: "플러터 웹 Flavors와 함께 Firebase 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-06-FlutterWebFirebasewithFlavors_0.png"
date: 2024-07-06 10:44
ogImage: 
  url: /assets/img/2024-07-06-FlutterWebFirebasewithFlavors_0.png
tag: Tech
originalTitle: "Flutter Web: Firebase with Flavors."
link: "https://medium.com/@m1nori/flutter-web-firebase-with-flavors-8b90055946d1"
---


안녕하세요! 이 글은 플러터 웹 앱에서 맛을 관리하는 방법에 대해 다룹니다.

예를 들어 Google Sign In을 구현하고 index.html 파일에서 Firebase 구성을 변경해야 하는 경우가 있다면 필요할 것입니다. Google Sign In을 구현해야 하는 경우 이 안내서를 사용해주세요.

Flutter 웹 프로젝트에서 플레이버에 따라 다른 index.html 파일을 만들려면 빌드 구성과 스크립팅의 조합을 사용하여 프로세스를 자동화할 수 있습니다. 다음은 단계별 안내서입니다:

# 1. 다른 index.html 파일 만들기:

<div class="content-ad"></div>

프로젝트 디렉토리에 각 플레이버에 대한 별도의 index.html 파일을 생성하세요.
웹 디렉토리의 주 파일인 index.html은 템플릿이나 기본 파일 역할을 해야 합니다. 예를 들어, web/ 디렉토리 안에 다음과 같은 3개의 파일이 있어야 합니다:

- index_dev.html
- index_prod.html
- index.html

/assets/img/2024-07-06-FlutterWebFirebasewithFlavors_0.png

올바른 설정을 갖춘 index_dev.html 파일의 예시:

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html>
<head>
   
    <base href="$FLUTTER_BASE_HREF">

    <meta charset="UTF-8">
    <meta content="IE=Edge" http-equiv="X-UA-Compatible">
    <link rel="apple-touch-icon" sizes="180x180" href="icons/apple-touch-icon.png">
    <link rel="icon" type="image/png" sizes="32x32" href="icons/favicon-32x32.png">
    <link rel="icon" type="image/png" sizes="16x16" href="icons/favicon-16x16.png">
    <link rel="manifest" href="icons/site.webmanifest">
    <link rel="mask-icon" href="icons/safari-pinned-tab.svg" color="#5bbad5">
    <meta name="msapplication-TileColor" content="#da532c">
    <meta name="theme-color" content="#ffffff">
    <meta name="msapplication-TileColor" content="#ffffff">
    <meta name="msapplication-TileImage" content="/ms-icon-144x144.png">
    <meta name="theme-color" content="#ffffff">
    <meta name="description" content="Test App.">

    <!-- Favicon -->
    <link rel="icon" type="image/pg" href="favicon/png"/>

    <title>Test App</title>
    <link rel="manifest" href="manifest.json">
</head>
<body>
<script src="https://www.gstatic.com/firebasejs/8.6.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.6.1/firebase-auth.js"></script>

<script>
    const firebaseConfig = {
      apiKey: "123",
      authDomain: "123",
      projectId: "123",
      storageBucket: "123",
      messagingSenderId: "123",
      appId: "123"
    };

    const app = initializeApp(firebaseConfig);
    const analytics = getAnalytics(app);

</script>
<script src="flutter_bootstrap.js" async></script>
</body>
</html>
```

노트(!) : `script src=”flutter_bootstrap.js” async``/script`라인은 body 태그 안에서 가장 마지막에 위치해야 합니다.

# 2. index.html로 올바른 파일을 복사하는 스크립트를 작성합니다.

프로젝트 루트 폴더에 scripts 폴더를 생성하고, 거기에 스크립트 파일을 넣었습니다.
저의 파일 scripts/copy_index.sh는 다음과 같습니다:

<div class="content-ad"></div>

```js
#!/bin/bash

FLAVOR=$1

if [ -z "$FLAVOR" ]; then
  echo "맛을 지정하지 않았어요. 기본 index.html을 사용합니다."
else
  echo "index_$FLAVOR.html을 index.html로 복사 중"
  cp web/index_$FLAVOR.html web/index.html
fi
```

# 3. 앱 실행 전 복사 스크립트를 실행할 스크립트 작성

예를 들어 scripts/run_dev.sh:
```js
./scripts/copy_index.sh dev
flutter run -d chrome --dart-define envFlavor=dev --flavor dev --web-port 5000
```

<div class="content-ad"></div>

스크립트/run_prod.sh :

```js
./scripts/copy_index.sh prod
flutter run -d chrome --dart-define envFlavor=prod --flavor prod --web-port 5000
```

/assets/img/2024-07-06-FlutterWebFirebasewithFlavors_1.png

# 4. 스크립트 실행

<div class="content-ad"></div>

터미널에서 다음 명령어를 실행해 주세요: zsh scripts/run_dev.sh
결과는 다음과 같이 나타납니다. 그리고 index_dev.html 파일이 변경될 것입니다:

/assets/img/2024-07-06-FlutterWebFirebasewithFlavors_2.png

Google Sign In의 자격 증명은 올바른 flavor 구성에서 가져온 것을 확인할 수 있습니다:

/assets/img/2024-07-06-FlutterWebFirebasewithFlavors_3.png

<div class="content-ad"></div>

다 했어요. 각각의 플레이버를 변경하기 전에 HTML 파일을 복사해두는 걸 잊지 마세요.