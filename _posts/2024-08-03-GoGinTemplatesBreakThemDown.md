---
title: "Go Gin 템플릿 세세히 파헤치기"
description: ""
coverImage: "/assets/img/2024-08-03-GoGinTemplatesBreakThemDown_0.png"
date: 2024-08-03 19:21
ogImage: 
  url: /assets/img/2024-08-03-GoGinTemplatesBreakThemDown_0.png
tag: Tech
originalTitle: "Go Gin Templates Break Them Down"
link: "https://dev.to/ossan/go-gin-templates-break-them-down-4mob"
---


## 오늘의 문제

안녕하세요! 오늘의 문제는 한 HTML 템플릿을 다른 HTML에 임베드하는 방법입니다. 이렇게 함으로써 코드의 재사용성을 높이고 중복을 줄일 수 있습니다. 두 개의 HTML 페이지를 렌더링해야 한다고 가정해 봅시다. 이 두 페이지는 중간 부분만 다를 뿐 모두 동일합니다. 헤더, 푸터, 그리고 두 페이지 각각의 정보를 담은 본문 두 개를 가지고 있으면 어떨까요? 이미 적용 방법을 알아보셨을 것입니다. 전형적인 사용 사례이기 때문이죠. 이 내용을 자세히 다루는 것이 가치가 있다고 생각합니다. Go와 Gin 웹 프레임워크를 활용해 이를 어떻게 구현할 수 있는지 알아봅시다.

### HTML 템플릿이란 무엇인가요?

코드에 대한 설명으로 바로 들어가기 전에 HTML 템플릿에 대해 간단히 이야기해보겠습니다. HTML 템플릿은 나중에 제공되는 값과 HTML 태그를 결합할 수 있게 해줍니다. Gin은 HTML 템플릿을 관리하기 위해 Go 표준 라이브러리에 속하는 html/template 패키지의 사용을 강화합니다. 더 많은 정보는 여기에서 확인할 수 있습니다. 이제 해결책을 제시하는 것으로 시작해봅시다 🚀.

<div class="content-ad"></div>

## 해결책

먼저 HTML 템플릿을 살펴보고 코드에서 이를 호출하는 방법과 몇 가지 테스트를 실행하는 방법을 알아보겠습니다. 우리가 가지고 싶어하는 최종 HTML 페이지를 제시해 봅시다:

![페이지](/assets/img/2024-08-03-GoGinTemplatesBreakThemDown_0.png)

헤더와 푸터가 동일한 것을 볼 수 있습니다. 그러나 "코어" 부분은 요청된 페이지에 따라 다릅니다.

<div class="content-ad"></div>

### HTML 템플릿

템플릿은 모두 templates 폴더에 있습니다. 파일 목록은 다음과 같습니다:

- header.tmpl
- page1.tmpl
- page2.tmpl
- footer.tmpl

먼저 정적인 것들을 다루어 보겠습니다.

<div class="content-ad"></div>

#### header.tmpl 파일

내용은 다음과 같습니다:

```js
{define "header" }
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <title>{ .title }</title>
</head>
<body>
 <h1>BASELINE</h1>
 <div id="content">
{end}
```

여기에 주의할 사항 몇 가지가 있습니다. 모든 템플릿 코드는 템플릿의 시작 및 끝 정의 사이에 있습니다. 이 경우에는 'define "header" '와 'end'가 경계입니다. 그런 다음, 우리는 `title` 태그 내에서 title이라는 매개변수를 사용하여 이 템플릿을 자식 템플릿에서 호출할 때 매핑할 수 있습니다. 나중에 자세히 알아보겠습니다.

<div class="content-ad"></div>

#### `footer.tmpl` 파일

여기서 내용은 더욱 간단합니다:

```js
{define "footer"}
</div>
<p>FooterInfo</p>
</body>
</html>
{end}
```

푸터 템플릿은 항상 일관되게 나타나는 모든 페이지의 마지막 부분입니다.

<div class="content-ad"></div>

### 페이지1.tmpl 파일

이제 매운 파트 🌶️. 페이지1.tmpl 파일에서 생명을 바꾸는 HTML 파트. 내용은 다음과 같습니다:

```js
{template "header" . }
<h2>페이지 1에 오신 것을 환영합니다</h2>
<p>이것은 페이지 1의 내용입니다.</p>
{template "footer"}
```

여기서 언급할 가치 있는 몇 가지 사항이 있습니다. 먼저, 'template "header" . '라는 줄을 사용하여 header 템플릿에 대한 참조를 추가합니다.

<div class="content-ad"></div>

헤더 템플릿(header.tmpl)과 page1.tmpl 파일에서 사용된 이름 간에는 1:1 관계가 있습니다 (예: 두 파일 모두 header를 사용합니다).

점(.)은 모든 매개변수를 파이프라인을 통해 전달하는 것을 의미합니다. 이 방법 덕분에 "Page 1 IS HERE" 값을 page1.tmpl 파일로 전달할 수 있습니다. 이 파일은 다시 해당 값을 사용하는 헤더 템플릿(header template)으로 전달합니다.

그런 다음, 해당 페이지에 특정 콘텐츠를 입력하고 일관성을 유지하기 위해 푸터 템플릿(footer template)을 참조합니다 🎇.

이제 Go 코드를 살펴보겠습니다.

<div class="content-ad"></div>

### 고 코드

여기서는 모든 것을 묶어 HTTP 엔드포인트에 응답할 수 있어야 합니다. 데모를 위해 전체 코드는 main.go 파일에 있습니다. 먼저 코드를 소개할 텐데, 그 다음에 관련 섹션을 모두 설명할게요.

```js
package main

import (
    "fmt"
    "net/http"
    "os"

    "github.com/gin-gonic/gin"
)

func main() {
    gin.SetMode(gin.DebugMode)
    r := gin.Default()

    // 템플릿 로드
    r.LoadHTMLGlob("templates/*.tmpl")

    r.GET("/page1", func(c *gin.Context) {
        c.HTML(http.StatusOK, "page1.tmpl", gin.H{
            // 이 값은 "header.tmpl" 파일로 전달됩니다
            "title": "페이지 1이 여기에 있습니다",
        })
    })

    r.GET("/page2", func(c *gin.Context) {
        c.HTML(http.StatusOK, "page2.tmpl", gin.H{
            "title": "페이지 2가 여기에 있습니다",
        })
    })

    if err := r.Run(":8080"); err != nil {
        fmt.Fprintf(os.Stderr, "서버를 실행할 수 없음: %v\n", err)
        os.Exit(1)
    }
}
```

가장 눈에 띄는 것은 r.LoadHTMLGlob("templates/*.tmpl") 호출입니다. 이는 특정 디렉토리에 일치하는 모든 템플릿을 로드합니다. 완성도를 위해 사용하는 것 외에도 원하는 파일들을 지정하는 r.LoadHTMLFiles()을 사용할 수도 있습니다.

<div class="content-ad"></div>

그럼, 우리는 라우터를 조작하여 /page1 및 /page2 주소로 오는 GET HTTP 요청에 응답하게 만들었습니다. HandlerFunc 본문에서는 c.HTML 메서드를 사용하여 다음을 설정할 수 있었습니다:

- HTTP 응답 상태 코드
- 이 유형의 요청에 사용할 템플릿
- 템플릿에 전달할 선택적 매개변수로 템플릿을 더 동적으로 만들 수 있습니다

매개변수로 gin.H 유형을 사용하여 맵[string] interface{}에 기초한 유연성을 가질 수 있었습니다.
마지막으로 서버를 시작하고 발생할 수 있는 오류를 잡았습니다. 🕵️

## 한 번 시도해보세요

<div class="content-ad"></div>

이제 모든 것이 예상대로 작동하는지 확인해봅시다. go run . 명령어를 실행하여 서버가 정상적으로 시작되었는지 확인합니다. 테스트하기 위해 curl 명령어를 사용할 것입니다 (머신에 설치해야 할 수도 있습니다. 더 자세한 정보는 여기를 참고하세요):

```js
curl http://localhost:8080/page1
```

셸에 출력된 내용은 다음과 같아야 합니다:

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page 1 IS HERE</title>
</head>
<body>
    <h1>BASELINE</h1>
    <div id="content">

<h2>Welcome to Page 1</h2>
<p>This is the content of Page 1.</p>

</div>
<p>FooterInfo</p>
</body>
</html>
```

<div class="content-ad"></div>

당신은 선택한 웹 브라우저에서도 직접 시도해볼 수 있어요. 어떤 방법을 선택하든 결과는 같을 거예요.

## 이제 끝

이 블로그 글은 HTML 템플릿에 대한 완전한 설명이 아닙니다. 아래 자료를 확인하는 걸 적극 권장해요:

- https://gin-gonic.com/docs/examples/html-rendering/
- https://www.digitalocean.com/community/tutorials/how-to-use-templates-in-go
- https://pkg.go.dev/html/template

<div class="content-ad"></div>

어떤 피드백이라도 매우 환영합니다.

떠나시기 전에, 만일 특정 주제에 관심이 있고 그것을 다뤄주길 원하신다면 저에게 연락해 주시기를 적극 권장합니다. 제가 그 주제를 선정하고 도움이 되는 콘텐츠를 제공하기 위해 최선을 다할 것을 약속합니다.

지원해 주셔서 정말 감사드리며, 다음에 또 만나요 👋