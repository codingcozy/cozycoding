---
title: "플라스크(Flask)를 이용한 메뉴 프로젝트 만드는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 17:05
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Project Documentation For menu project using flask"
link: "https://medium.com/@bhaibhupesh10/project-documentation-flask-google-search-application-7fa0adfc0053"
isUpdated: true
updatedAt: 1724742694297
---


## 개요:

이 Flask 애플리케이션은 사용자가 웹 인터페이스에서 직접 Google 검색을 수행할 수 있도록 합니다. 백엔드에는 Flask 프레임워크를 사용하고, HTTP 요청을 보내기 위해 requests를 사용하며, Google 검색 결과의 HTML 내용을 파싱하기 위해 BeautifulSoup를 사용합니다. 이 앱은 검색 결과를 반환하고 웹페이지에 표시합니다.

## 폴더 구조:

```js
project_directory/
│
├── templates/
│   └── index.html       # 웹 인터페이스용 HTML 파일
│
└── server.py            # 메인 Flask 애플리케이션 파일
```

<div class="content-ad"></div>

## 의존성:

- Flask: 루트 처리 및 HTML 템플릿 렌더링을 다루는 웹 프레임워크입니다.
- requests: 외부 사이트(Google 검색)로 HTTP 요청을 보내는 라이브러리입니다.
- BeautifulSoup: 검색 결과에서 관련 정보를 추출하는 HTML 파싱 라이브러리입니다.

## 설치:

- 가상 환경 없이 전역적으로 의존성 설치하기:

<div class="content-ad"></div>

- pip install flask requests beautifulsoup4

- 가상 환경을 사용하여 종속성 설치하기(권장 사항):

- 가상 환경 생성:
- python -m venv venv

## 코드 설명:

<div class="content-ad"></div>

server.py:

- 라이브러리 가져오기:

- Flask: 웹 애플리케이션을 만드는 데 사용됩니다.
- request: HTTP 요청에서 쿼리 매개변수에 액세스합니다.
- render_template: HTML 템플릿을 렌더링합니다.
- jsonify: Python 사전을 JSON으로 변환합니다.
- requests: Google에 HTTP 요청을 보냅니다.
- BeautifulSoup: Google의 검색 결과의 HTML 내용을 구문 분석합니다.

- google_search 함수:

<div class="content-ad"></div>

- 입력: 쿼리 (검색어).
- 처리:
  - 검색 쿼리로 Google에 GET 요청을 보냅니다.
  - BeautifulSoup을 사용하여 응답을 파싱하여 검색 결과의 제목, 링크 및 스니펫을 추출합니다.
- 출력: 검색 결과 목록 (제목, 링크, 스니펫).

- 홈 경로 (/):

  - 사용자가 검색 쿼리를 입력할 수 있는 홈페이지 (index.html)를 렌더링합니다.

- 검색 경로 (/search):

<div class="content-ad"></div>

- 입력: 쿼리 매개변수 쿼리.
- 처리: 사용자가 제공한 쿼리로 google_search 함수를 호출합니다.
- 출력: JSON 응답으로 검색 결과를 반환합니다.

- 애플리케이션 시작:

- 스크립트를 직접 실행하면 Flask 개발 서버가 시작되어 애플리케이션에 액세스할 수 있게됩니다.

index.html:

<div class="content-ad"></div>

- 양식:

  - 사용자가 검색어를 입력할 수 있는 입력 필드가 포함되어 있습니다.
  - JavaScript (AJAX)를 사용하여 /search 엔드포인트로 양식을 제출합니다.

- JavaScript:

  - 양식 제출을 캡처하고 Flask 백엔드에서 검색 결과를 가져와 새로 고침 없이 페이지에 표시합니다.

<div class="content-ad"></div>

## 어플리케이션 실행 방법:

- Flask 서버를 실행하려면 다음을 실행하세요: 

```bash
python server.py
```

- 웹 브라우저를 열고 http://127.0.0.1:5000/ 주소로 이동하여 어플리케이션에 접속하세요.
- 검색어를 입력한 후 "검색"을 클릭하면 결과가 페이지에 표시됩니다.

<div class="content-ad"></div>

# 여러 작업 추가하기:

만약 이 애플리케이션의 기능을 확장하여 더 많은 작업(예: 다양한 유형의 검색, 데이터 처리 또는 기타 기능)을 추가하고 싶다면, 다음 단계를 따르세요:

## 1. 새 경로 추가:

다른 작업을 처리하기 위해 새 경로를 추가할 수 있습니다. 예를 들어 날씨 정보를 가져오는 작업을 추가하려면 새 경로를 만드세요:

<div class="content-ad"></div>

```python
@app.route('/weather', methods=['GET'])
def get_weather():
    city = request.args.get('city')
    # 날씨 정보를 가져오는 코드
    weather_data = fetch_weather(city)
    return jsonify(weather_data)
```

## 2. 새 작업을 위한 함수 생성:

google_search와 유사한 작업을 수행하는 함수를 정의하세요. 예를 들어:

```python
def fetch_weather(city):
    # 날씨 서비스에 API 호출
    response = requests.get(f'https://api.weatherapi.com/v1/current.json?key=YOUR_API_KEY&q={city}')
    if response.status_code == 200:
        return response.json()
    else:
        return {'error': '날씨 데이터를 가져올 수 없습니다'}
```

<div class="content-ad"></div>

## 3. 프론트엔드 수정:

index.html을 업데이트하여 새 작업을 위한 폼을 포함시킵니다. 예를 들어, 날씨 조회를 처리하는 새로운 폼을 추가하세요:

```js
<form id="weatherForm">
    <input type="text" id="city" name="city" placeholder="도시를 입력하세요">
    <button type="submit">날씨 가져오기</button>
</form>
<div id="weatherResults"></div>
```

```js
<script>
    document.getElementById('weatherForm').addEventListener('submit', function(e) {
        e.preventDefault();
        const city = document.getElementById('city').value;
        fetch(`/weather?city=${city}`)
            .then(response => response.json())
            .then(data => {
                let resultsDiv = document.getElementById('weatherResults');
                resultsDiv.innerHTML = `<p>온도: ${data.temp}°C</p>`;
            });
    });
</script>
```

<div class="content-ad"></div>

## 4. 추가 템플릿 추가하기 (선택사항):

새로운 작업이 다른 HTML 템플릿이 필요하다면, templates 폴더에 추가 HTML 파일을 생성하고 render_template 함수를 사용하여 렌더링할 수 있습니다.

```js
@app.route('/new_task')
def new_task():
    return render_template('new_task.html')
```

# 결론:

<div class="content-ad"></div>

이 문서는 Flask 애플리케이션을 설정, 실행 및 확장하는 방법을 안내합니다. 새로운 라우트와 함수를 만들고 전면 사용자 인터페이스를 필요에 맞게 수정하여 여러 작업을 추가할 수 있습니다.