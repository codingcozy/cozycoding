---
title: "아름답게 웹 스크래핑하기 BeautifulSoup과 함께"
description: ""
coverImage: "/assets/img/2024-08-26-BasicsOfWebScrappingWithBeautifulSoup_0.png"
date: 2024-08-26 17:57
ogImage: 
  url: /assets/img/2024-08-26-BasicsOfWebScrappingWithBeautifulSoup_0.png
tag: Tech
originalTitle: "Basics Of Web Scrapping With BeautifulSoup"
link: "https://medium.com/@officialashharps/basics-of-web-scrapping-with-beautifulsoup-67e9ad6796c2"
isUpdated: false
---



<img src="/assets/img/2024-08-26-BasicsOfWebScrappingWithBeautifulSoup_0.png" />

BeautifulSoup는 Python에서 가장 간단하고 쉽게 배울 수 있는 웹 스크래핑 라이브러리 중 하나입니다.

다음 블로그는 BeautifulSoup의 핵심 주제 중 일부를 소개하며, 웹 스크래핑 분야로 진입하는 데 도움이 될 것입니다.

사전 요구 사항 - 기본적인 HTML


<div class="content-ad"></div>

# 1) 설치

Python에 BeautifulSoup 패키지를 설치하려면 터미널에서 다음 명령어를 사용할 수 있습니다.

```js
pip install beautifulsoup4
```

# 2) 웹 사이트에 연결하기

<div class="content-ad"></div>

웹 스크래핑의 첫 번째 단계는 코드 편집기에서 대상 웹 사이트와의 연결을 설정하여 웹 사이트의 HTML 콘텐츠를 가져오고 필요한 데이터를 추출하는 것입니다.

Python requests는 웹 사이트로 HTTP 요청을 보내는 데 사용되는 라이브러리입니다.

다음 명령어로 Requests를 설치하세요:

```js
pip install requests
```

<div class="content-ad"></div>

자료를 스크레이핑하기 위해서는 먼저 requests 라이브러리를 사용해서 해당 웹페이지의 콘텐츠를 추출해야 해요:

```js
url = "http://example.com"
response = requests.get(url)
html_content = response.text
```

웹페이지의 HTML 구조는 html_content 변수에 저장됩니다.

# 3) Beautifulsoup 객체 생성

<div class="content-ad"></div>

```js
soup = BeautifulSoup(html_content, 'html.parser')
```

BeautifulSoup 객체를 만드는 것은 데이터를 추출하는 핵심 단계 중 하나입니다.

여기서는 덧대 Beautifulsoup 오브젝트로부터 데이터를 추출하는데 필요한 순수 HTML 문자열을 Python 객체의 구조화된, 탐색 가능한 트리로 변환합니다. 이를 통해 문서에서 데이터를 손쉽게 검색, 수정, 추출할 수 있게 됩니다.

# 예시 HTML 코드:

<div class="content-ad"></div>

```javascript
<!DOCTYPE html>
<html>
<head>
    <title>예시 페이지</title>
</head>
<body>
    <div id="main-content" class="content">
        <h1>예시 페이지에 오신 것을 환영합니다</h1>
        <p>이것은 <a href="https://example.com">링크</a>가 있는 샘플 단락입니다.</p>
        <p class="description">이 단락은 특정 클래스를 가지고 있습니다.</p>
        <a href="https://another-example.com">또 다른 링크</a>
    </div>
    <footer id="footer">
        <p>&copy; 2024 Example</p>
    </footer>
</body>
</html>
```

이제 이 샘플 HTML 구조에서 몇 가지 데이터를 추출해보겠습니다:

## 1) 태그로 엘리먼트 찾기

이것은 데이터를 찾고 추출하는 가장 직접적인 방법 중 하나입니다.

<div class="content-ad"></div>

여기서는 a, p, h1 등과 같은 HTML 태그 내에서 검색을 수행합니다...

HTML:

```js
<title>Example Page</title>
```

코드:

<div class="content-ad"></div>

```js
title_tag = soup.find('title')
print(title_tag)  
print(title_tag.string)  
```

설명:

기본적으로, 우리는 이 경우 "title tag" 내의 데이터인 "Example Page"를 추출하고 있습니다.

## 2) CSS 클래스에 따라 요소 찾기

<div class="content-ad"></div>

HTML의 클래스는 요소에 CSS 스타일을 적용하는 데 사용되지만, 스크레이핑해야 하는 데이터를 식별하는 데도 사용할 수 있어요.

HTML:

코드:

```js
element = soup.find('p', class_='description')
print(element.string)
```

<div class="content-ad"></div>

해설:

이 HTML 구조에서 class = "description" 인 첫 번째 'p' 태그를 찾고 있습니다.

요소를 찾으면 html 요소 내부의 텍스트를 ".string"을 사용하여 추출합니다.

## 3) ID로 요소 찾기

<div class="content-ad"></div>

클래스와 유사하게, 우리는 필요한 데이터를 찾아 추출하기 위해 html에서 id 속성을 사용합니다.

HTML:

```js
<div id="main-content" class="content">
    <h1>예시 페이지에 오신 걸 환영합니다</h1>
    <p>이것은 <a href="https://example.com">링크</a>가 있는 샘플 단락입니다.</p>
</div>
```

코드:

<div class="content-ad"></div>

```js
element = soup.find(id='main-content')
print(element.string)
```

설명:

여기서는 id가 `main-content`인 첫 번째 html 태그를 찾아 내부의 텍스트를 추출하려고 합니다.

이 경우 출력은 다음과 같을 것입니다:

<div class="content-ad"></div>

"예제 페이지에 오신 것을 환영합니다

이곳은 링크가 있는 샘플 단락입니다"

## 4) 링크 찾기

HTML 요소에는 href와 같이 특정 링크나 URL이 저장된 속성이 있습니다.

<div class="content-ad"></div>

특정 속성을 갖는 특정 태그를 검색하여 해당 링크를 찾을 수 있어요!

# HTML:

```js
<a href="https://example.com">link</a>
```

코드:

<div class="content-ad"></div>

```js
link = soup.find('a')
print(link['href'])  
```

설명:

첫 번째 'a' 태그를 찾아 변수 link에 저장했습니다.

“a” 태그 내의 링크에 액세스하기 위해 link[‘href’]를 사용합니다. 여기서 href는 속성입니다.

<div class="content-ad"></div>

"위치를 찾을 수 있는 여러 요소

지금까지 우리가 필요한 특정 데이터를 찾는 데 `find` 메서드를 사용했다는 것을 이미 알아챘을 것입니다."

`https://example.com`

<div class="content-ad"></div>

그러나 "find" 메서드는 특정 태그의 첫 번째 발생을 찾을 때 사용됩니다.

특정 태그의 모든 발생을 찾아야 하는 경우는 어떻게 할까요?

그럴 때 "find_all" 메서드가 활약합니다.

예를 들어 설명해 드릴게요:

<div class="content-ad"></div>

## a) Find Method

샘플 HTML:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Sample Page</title>
</head>
<body>
    <p>This is the first paragraph.</p>
    <p>This is the second paragraph.</p>
    <p>This is the third paragraph.</p>
</body>
</html>
```

코드

<div class="content-ad"></div>


# Markdown

세부 설명:
이 코드는 `soup.find('p')`를 사용하여 HTML에서 첫 번째 단락을 찾고 `first_p` 변수에 저장합니다. 그런 다음 `print(first_p)`를 통해 첫 번째 단락 요소를 출력하고, `print(first_p.text)`를 통해 해당 요소의 텍스트 콘텐츠를 출력합니다.


<div class="content-ad"></div>

우리는 find 메소드를 사용하여 첫 번째 `p` 태그를 찾고, 해당 태그 내의 텍스트를 추출했어요.

## 2. Find All 메소드

코드:

```js
# 모든 <p> 태그 찾기
all_ps = soup.find_all('p')
for p in all_ps:
    print(p.string)
```

<div class="content-ad"></div>

출력:

```js
첫 번째 단락입니다.
두 번째 단락입니다.
세 번째 단락입니다.
```

설명:

모든 `p` 태그는 변수 "all_ps"에 리스트 형식으로 저장되어 있습니다.

<div class="content-ad"></div>

그럼 목록 내의 각 p 태그마다 for 루프를 사용하여 내부 텍스트를 ".string"을 사용하여 추출합니다.

# 텍스트 추출의 다양한 방법

- .get_text()
- .string
- .text

결론

<div class="content-ad"></div>

이것들은 웹 스크레이핑의 세계에서 시작할 수 있는 핵심 개념들입니다.

이제 연습할 시간이에요. 원하는 데이터를 추출하기 위해 간단한 웹페이지를 스크레이핑해보세요.

고맙습니다,

Muhammed Ashhar