---
title: "Flask를 사용한 간단한 웹 애플리케이션 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-BuildingaSimpleWebApplicationwithFlask_0.png"
date: 2024-08-26 17:07
ogImage: 
  url: /assets/img/2024-08-26-BuildingaSimpleWebApplicationwithFlask_0.png
tag: Tech
originalTitle: "Building a Simple Web Application with Flask"
link: "https://medium.com/@adityapandeyadp/building-a-simple-web-application-with-flask-56c293d04a68"
isUpdated: true
updatedAt: 1724743673748
---


# 웹 앱을 만드는 일은 꽤 어렵지만 Flask를 사용하면 간단해집니다. 파이썬 프레임워크를 사용하면 간단하게 웹 애플리케이션을 만들 수 있습니다. 이 블로그에서는 Flask, HTML 및 CSS를 사용하여 간단한 웹 애플리케이션을 만드는 단계를 안내하겠습니다.

필수 준비물

- 기본적인 Python 이해
- HTML 및 CSS 작성 방법
- Flask
- Vs Code (또는 다른 IDE)

단계 1: 환경 설정하기

<div class="content-ad"></div>

패키지 버전이 다른 것으로 인해 문제가 발생하는 것을 원치 않기 때문에 가상 환경을 사용할 것입니다.

```shell
$ python3 -m venv myprojectenv
$ source myprojectenv/bin/activate # Windows일 경우: myprojectenv\Scripts\activate
```

다음으로 Flask를 설치해 보세요:

```shell
$ pip install Flask
```

<div class="content-ad"></div>

Step 2: 플라스크 앱 만들기

프로젝트 디렉토리에 새 파일 'app.py'를 만들고 다음 코드를 사용하세요

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
  return "첫 번째 플라스크 앱에 오신 것을 환영합니다!"

if __name__ == '__main__':
  app.run(debug=True, port=5000)
```

Step 3: 플라스크 앱 실행하기

<div class="content-ad"></div>

플라스크 앱을 실행하려면 터미널에서 다음 명령을 사용하세요:

```shell
$ python app.py
```

터미널에 링크가 표시됩니다. 해당 링크를 브라우저에서 열어서 초기 플라스크 앱을 사용할 수 있습니다.

단계 4: 템플릿 사용

<div class="content-ad"></div>

좀 더 복잡한 애플리케이션을 위해서는 HTML 템플릿을 사용하는 것이 좋습니다. 프로젝트 디렉토리에 templates 폴더를 생성하고 그 안에 home.html 파일을 만들어주세요:

```js
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Home</title>
</head>
<body>
<h1>Welcome to your first Flask app!</h1>
</body>
</html>
```

app.py 파일을 수정하여 render templates를 사용하도록 하겠습니다:

```js
from flask import render_template
@app.route('/')
def home():
  return render_template('home.html')
```

<div class="content-ad"></div>

### Step 5: HTML에 CSS 파일 추가하기

```js
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Home</title>
<link rel="stylesheet" href="{ url_for('static', filename='style.css') }">
</head>
<body>
<h1>Welcome to Your First Flask App!</h1>
</body>
</html>
```

HTML의 7번 줄에 위 코드를 추가하고 CSS에 링크를 생성해주세요.

프로젝트 디렉토리에 **static** 폴더를 생성해주세요. 그 안에 **style.css** 파일을 만들어주세요.

<div class="content-ad"></div>

```js
body {
  background-color: #f0f0f0;
  font-family: Arial, sans-serif;
}
h1 {
  color: #333;
  text-align: center;
  margin-top: 50px;
}
```

브라우저에서 웹 사이트를 새로고침하면 결과를 확인할 수 있어요!

<img src="/assets/img/2024-08-26-BuildingaSimpleWebApplicationwithFlask_0.png" />

축하해요! Flask를 사용하여 간단한 웹 애플리케이션을 만들었네요. 여기서부터는 더 많은 경로, 템플릿 추가, 심지어 데이터베이스와 연결하여 애플리케이션을 확장할 수 있어요. Flask는 이처럼 간단한 프로젝트부터 복잡한 애플리케이션까지 확장이 가능한 강력한 도구로, 웹 개발에 최적한 선택 중 하나에요.

<div class="content-ad"></div>

이제 Flask의 기능을 더 탐구하고 더 정교한 웹 애플리케이션을 만들어 보세요!