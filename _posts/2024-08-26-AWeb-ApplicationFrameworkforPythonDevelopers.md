---
title: "파이썬 개발자를 위한 웹 애플리케이션 프레임워크 Django, Flask 등 정리"
description: ""
coverImage: "/assets/img/2024-08-26-AWeb-ApplicationFrameworkforPythonDevelopers_0.png"
date: 2024-08-26 17:25
ogImage: 
  url: /assets/img/2024-08-26-AWeb-ApplicationFrameworkforPythonDevelopers_0.png
tag: Tech
originalTitle: "A Web-Application Framework for Python Developers"
link: "https://medium.com/datadriveninvestor/a-web-application-framework-for-python-developers-0e8d5ad3be7a"
isUpdated: true
updatedAt: 1724744046200
---


## FastHTML 소개와 예제

![이미지](/assets/img/2024-08-26-AWeb-ApplicationFrameworkforPythonDevelopers_0.png)

안녕하세요! AI 애플리케이션을 위한 웹 프론트엔드를 개발하는 Python 개발자 여러분. 저도 Python 개발자이며, 웹 프론트엔드 기술에 대한 능력 부족으로 항상 어려움을 겪었습니다. 사내 웹 프론트엔드 개발자들은 가끔만 도와 줄 뿐이었고, 제 지식은 개발에 부족함이 있었습니다.

Streamlit 패키지가 인기를 얻기 시작하면서 Python으로 애플리케이션을 개발할 수 있는 기회를 얻게 되었습니다. 그러나 Streamlit으로 개발한 애플리케이션은 실제 웹 애플리케이션 같지 않았고, 보다 복잡한 애플리케이션에 대해서는 이상적인 선택이 아니었습니다.

<div class="content-ad"></div>

그래서 새로운 프레임워크인 "FastHTML"가 좋은 소식이었어요. 그래서 즉시 살펴보았죠. 이 기사와 이어지는 글에서는 첫 경험들을 소개하면서 샘플 애플리케이션과 함께 소개하고 싶어요. FastAPI 프레임워크로 Python에서 REST 인터페이스를 개발하는 데 사용되는 FastHTML이란 이름의 유사성은 우연이 아니라 의도적이에요. 시스템을 설계할 때 FastAPI에서 큰 영감을 받았답니다.

## FastHTML에 대한 정보

FastHTML의 문서는 https://docs.fastht.ml/에서 찾을 수 있으며, 오픈 소스 프로젝트의 소스 코드와 예제는 https://github.com/AnswerDotAI/fasthtml에서 확인할 수 있어요. 이 프레임워크는 매우 가볍고 잘 맞는 매우 가벼운 패키지에 기초하여 성능 범위를 실현해요. 필수 기술은 AJAX 기능을 제공하는 HTMX이며, 웹 소켓 등이 포함돼 있어요. 미니멀한 PicoCSS가 CSS 프레임워크로 사용돼요.

## 샘플 애플리케이션

<div class="content-ad"></div>

아래에서는 FastHTML의 핵심 구성 요소를 예제 응용 프로그램을 사용하여 소개하겠습니다. 샘플 응용 프로그램은 간단한 사용자 인증을 실현하고 로그인에 성공한 후 시작 페이지를 표시합니다. 샘플 응용 프로그램의 코드를 단계별로 살펴보겠습니다.

```js
from fasthtml.common import *
import os
from hmac import compare_digest
from dotenv import load_dotenv

load_dotenv()

userpw = os.environ.get("PW")
```

첫 번째 줄은 FastHTML 함수를 가져오고, 다른 가져오기 문은 환경 변수를 처리하는 데 필요합니다. 응용 프로그램을 간단하게 유지하려면 인증에 사용되는 비밀번호를 환경 변수에 저장합니다. 비밀번호를 확인하기 위해 compare_digest를 사용합니다.

```js
login_redir = RedirectResponse('/login', status_code=303)
def before(req, sess):
    auth = req.scope['auth'] = sess.get('auth', None)
    if not auth: return login_redir
bware = Beforeware(before, skip=[r'/favicon\.ico', r'/static/.*', r'.*\.css', '/login'])
```

<div class="content-ad"></div>

각 페이지를로드하기 전에 시스템이 사용자가 로그인했는지 확인합니다. 그렇지 않으면 사용자가 로그인 페이지로 리디렉트됩니다.

```js
app = FastHTML(
    before=bware,
    hdrs=( picolink, Style(':root { --pico-font-size: 100%; }'))
    )
```

FastHTML 애플리케이션이 정의되었으며 이전에 정의한 사용자 확인 함수가 통합되었습니다. 머리글에 PicoCSS 및 기본값을 확장하거나 덮어 쓰는 클래스가 추가되었습니다.

```js
rt = app.route

@rt("/login")
def get():
    frm = Form(
        Input(id='name', placeholder='Name'),
        Input(id='pwd', type='password', placeholder='Password'),
        Button('login'),
        action='/login', method='post')
    return Titled("Login", frm)

@rt("/login")
def post(login:Login, sess):
    if not login.name or not login.pwd: return login_redir
    if not compare_digest(userpw.encode("utf-8"), login.pwd.encode("utf-8")):
        return login_redir
    sess['auth'] = login.name
    return RedirectResponse('/', status_code=303)

@app.get("/logout")
def logout(sess):
    del sess['auth']
    return login_redir
```

<div class="content-ad"></div>

개별 라우트의 Post 및 Get 호출을 위한 함수가 정의되었습니다. 간단한 예제에서는 로그인 및 로그아웃을 위한 라우트가 있습니다. 사용자 이름과 비밀번호 필드로 구성된 간단한 양식이 지정되며, 환경 변수에 저장된 비밀번호("PW")가 입력되었는지 확인합니다. 사용자 이름은 예시로 임의로 지정됩니다.

로그아웃 라우트에서는 세션에 저장된 인증이 삭제되며 사용자가 로그아웃됩니다.

```js
# 메인 페이지
@rt("/")
def get(auth):
    title = f"{auth}님이 로그인함"
    top = Grid(H1(title), Div(A('로그아웃', href='/logout'), 
      style='text-align: right'))
    return Title(title), Main(top, P("일부 내용"), cls='container')

serve()
```

메인 페이지 라우트에서 사용자 이름과 일부 다소 가짜 콘텐츠가 표시됩니다. 이 예제는 FastHTML이 응답으로 해당 태그를 생성하는 각 HTML 태그용 함수를 제공한다는 것을 보여줍니다. grid 함수는 간단한 그리드 레이아웃을 위해 사용됩니다. 마지막 줄은 FastHTML 서버를 시작합니다.

<div class="content-ad"></div>

로그인에 성공한 후 시작 페이지의 콘텐츠가 표시됩니다. 이 예제에서는 제목, 로그아웃 옵션 및 페이지 콘텐츠가 포함됩니다. 아래 그림을 참조하세요.

![start page content](/assets/img/2024-08-26-AWeb-ApplicationFrameworkforPythonDevelopers_1.png)

완전한 소스 코드는 다음과 같습니다:

```js
from fasthtml.common import *
import os
from hmac import compare_digest
from dotenv import load_dotenv

load_dotenv()

userpw = os.environ.get("PW")

login_redir = RedirectResponse('/login', status_code=303)
def before(req, sess):
    auth = req.scope['auth'] = sess.get('auth', None)
    if not auth: return login_redir
bware = Beforeware(before, skip=[r'/favicon\.ico', r'/static/.*', r'.*\.css', '/login'])

app = FastHTML(
    before=bware,
    hdrs=( picolink, Style(':root { --pico-font-size: 100%; }'))
    )

rt = app.route

@rt("/login")
def get():
    frm = Form(
        Input(id='name', placeholder='Name'),
        Input(id='pwd', type='password', placeholder='Password'),
        Button('login'),
        action='/login', method='post')
    return Titled("Login", frm)

@rt("/login")
def post(login:Login, sess):
    if not login.name or not login.pwd: return login_redir
    if not compare_digest(userpw.encode("utf-8"), login.pwd.encode("utf-8")): 
      return login_redir
    sess['auth'] = login.name
    return RedirectResponse('/', status_code=303)

@app.get("/logout")
def logout(sess):
    del sess['auth']
    return login_redir

# Main page
@rt("/")
def get(auth):
    title = f"{auth}님이 로그인 중"
    top = Grid(H1(title), Div(A('logout', href='/logout'), style='text-align: right'))
    return Title(title), Main(top, P("내용"), cls='container')

serve()
```

<div class="content-ad"></div>

이 예제는 FastHTML의 기본 구조와 몇 가지 기본 개념을 보여줍니다. 코드가 main.py 파일에 있다면 Python main.py로 시작할 수 있습니다.

DataDrivenInvestor.com에서 저희를 방문해주세요.

DDIntel을 여기서 구독하세요.

크리에이터 생태계에 여기서 참여하세요.

<div class="content-ad"></div>

DDI 공식 텔레그램 채널: [링크](https://t.me/+tafUp6ecEys4YjQ1)

LinkedIn, Twitter, YouTube, 그리고 Facebook에서 팔로우해 주세요.