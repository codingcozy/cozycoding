---
title: "Power Automate를 사용하여 나만의 폼을 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_0.png"
date: 2024-08-21 18:45
ogImage: 
  url: /assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_0.png
tag: Tech
originalTitle: "How to Create Your Own Form with Power Automate"
link: "https://dev.to/wyattdave/how-to-create-your-own-form-with-power-automate-4546"
isUpdated: true
updatedAt: 1724245790209
---


파워 오토메이트 개발자들 중 99%는 MS Forms 커넥터를 사용해본 경험이 있을 것 같아요. 정보 수집, 변환 및 SharePoint와 같은 다른 곳으로 이동하는 작업은 흔한 요구사항이며 Forms가 이를 잘 처리해줘요.

하지만 완벽하지 않아요. 세 가지 요소가 빠져있죠:
- 사전 정보 불러오기
- 외부 응답자에게 파일 업로드하기
- 테마 설정(폼 템플릿에만 제한됨)

하지만 이러한 제한사항을 피하는 방법이 있는데, 그것은 MS Forms의 우리만의 버전을 만드는 것이에요.

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

우리는 HTML 양식을 만들겠습니다(Javascript는 필요 없지만 원한다면 추가할 수 있습니다) 그리고 이후 Power Automate를 사용하여 해당 양식을 전송/처리할 것입니다.
다음과 같은 단계를 진행해야 합니다:

1. 양식 설정
2. 양식 전송
3. 양식 생성
4. 양식 처리

만들어질 양식은 다음과 같습니다:

- 응답자가 자신의 이름, 배송 날짜를 추가하고 주문 파일을 업로드하도록 요청합니다
- 양식에는 미리 생성된 주문 참조가 포함됩니다
- 응답은 처리할 때 한 번만 처리되도록 참조가 포함됩니다
- 간단한 인증이 포함되어 있습니다

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

그리고 흐름은 다음과 같을 겁니다:

- 이메일에 링크를 전송합니다.
- 양식 웹페이지를 생성합니다.
- 이미 처리되었는지 확인을 포함한 양식 응답을 처리합니다.

![이미지](/assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_0.png)

## 1. 양식 설정

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

JavaScript를 사용하지 않고 가능한 한 간단하게 유지하기 위해 HTML `form` 태그를 사용할 것입니다. 이 태그를 사용하면 태그 내의 모든 입력을 보낼 수 있습니다:

```js
<form action="{YOUR TRIGGER URL}" method="post" target="_blank">
        <label for="order">주문 참조:</label>
        <input type="text" id="order" name="order" value="#429"><br><br>
        <label for="name">이름:</label>
        <input type="text" id="name" name="name"><br><br>
        <label for="date">배송 날짜:</label>
        <input type="date" id="date" name="date"><br><br>
        <input type="submit" value="제출">
    </form>
```

위의 예시는 아래와 같이 응답을 보냅니다:

```js
{
  "$content-type": "application/x-www-form-urlencoded",
  "$content": "Zm5hbWU9RGF2aWQmbG5hbWU9V3lhdHQ=",
  "$formdata": [
    {
      "key": "name",
      "value": "David"
    },
    {
      "key": "date",
      "value": "2024-08-04"
    }
  ]
}
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

파일 업로드가 필요하지 않다면 당신이 필요한 것에 대해 좋은 것입니다. 그러나 파일을 업로드하려면 응답 유형을 application/x-www-form-urlencoded에서 multipart/form-data로 변경해야합니다. 이는 흐름 처리에 복잡성을 추가합니다.

마지막으로 주의해야 할 점은 `form` 태그에서 헤더를 설정할 수 없다는 것입니다. (JavaScript에서는 가능합니다.) 이로 인해 인증 방법이 제한됩니다. 하지만 해결책이 있습니다. 사전 입력 값을 추가하고 숨길 수 있습니다. 그럼 이 값은 폼 데이터와 함께 보내져서 트리거 조건으로 사용할 수 있습니다. 이것은 쉽게 볼 수 있기 때문에 이상적이지 않지만 기본 수준의 인증에 사용할 수 있습니다.

```js
<form action="{YOUR TRIGGER URL}" method="post" target="_blank" enctype="multipart/form-data">
        <input name="key" value="helloWorld" style="visibility:hidden" /><br>
        <label for="order">Order Ref:</label>
        <input type="text" id="order" name="order" value="#429"><br><br>
        <label for="name">Name:</label>
        <input type="text" id="name" name="name"><br><br>
        <label for="date">Delivery Date:</label>
        <input type="date" id="date" name="date"><br><br>
        <input name="file" type="file" /><br><br>
        <input type="submit" value="Submit">
    </form>
```

<img src="/assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_1.png" />

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

위의 텍스트를 친근한 톤으로 한국어로 번역해 드리겠습니다.

이 양식을 보시면 딱히 이쁘지 않다는 것을 알 수 있죠, 여기서 HTML과 CSS의 진정한 유연성이 발휘됩니다. 원하는 대로 양식을 디자인할 수 있고, 교육 정보, 링크, 사실 웹사이트에 있는 것이라면 모두 양식에 추가할 수 있습니다.

![양식 이미지](/assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_2.png)

무료 템플릿도 많이 있습니다. 저는 formbold.com에서 제 템플릿을 얻었는데, 그 외에도 수천 개가 있습니다.

## 2. 양식 보내기

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

답변자에게 양식을 받기 위해 이메일을 보낼 것입니다. 안타깝게도 모든 브라우저가 이메일에 양식을 직접 포함하는 것을 지원하지는 않습니다. 그래서 양식이 있는 웹 페이지에 링크를 보내줄 것입니다.

이 과정은 비교적 간단합니다. 이메일 본문에 있는 양식 URL의 고유 참조값을 업데이트하고, 그런 다음 이메일을 보냅니다.

고유 참조값은 우리가 전달하는 매개변수입니다. 이렇게 하면 이 매개변수를 사용하여 응답을 업데이트할 올바른 행을 찾을 수 있습니다. URL 매개변수는 `?param=value&param2=value` 형식을 따릅니다. 양식의 URL에 이미 매개변수가 있는 경우, `&param=value`를 URL에 추가합니다. 그런 다음 대체 표현식(replace expression)을 수행할 수 있습니다. (아래 예제의 경우 param은 ref입니다):

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
replace(triggerBody()['text'],'{paramRef}',string(outputs('Get_item')?['body/ID']))
```

테이블 태그를 Markdown 형식으로 변경했습니다.

## 3. 폼 생성하기

폼 링크를 전송했지만 실제로 양식을 어떻게 만들고 양식의 URL을 어디서 얻을까요?

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

제가 가장 좋아하는 트리거 중 하나인 `HTTP 요청이 수신되었을 때`를 사용하여 폼을 만들어보려고 합니다. 이를 통해 Power Automate가 기본적인 API처럼 작동하여 매개변수를 수신하고 응답을 보낼 수 있습니다. 재미있는 점은 HTML을 반환할 수 있으며, 브라우저에서 해당 URL을 사용하면 웹페이지처럼 HTML을 불러옵니다.

그래서 계획은 1단계에서 만든 것과 같이 폼을 사용하는 것입니다. 플로우는 GET 요청에 의해 트리거되며, ref 매개변수를 사용하여 SharePoint 목록에서 항목을 가져옵니다. 이는 이미 응답이 처리되었는지를 확인하기 위한 것입니다. 이미 처리된 응답이 있다면 `실패 - 이미 처리된 제출`을 반환하고, 그렇지 않으면 폼의 HTML을 보냅니다 (아래에서는 Get File Content 동작으로 가져옵니다. 폼을 사전에 채우려면 HTML에서 Get Item에서 가져온 값으로 또 다른 대체를 할 수 있습니다.


![이미지](/assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_4.png)

![이미지](/assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_5.png)


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

폼 응답 표현식

```js
replace(
    replace(
        body('Get_file_content_HTHML')
    ,
        '{ref}'
    ,
        outputs('Get_item')?['body/Title']
    )
,
    '{paremRef}'
,
    outputs('Get_item')?['body/ID']
)
```

폼 HTML

```js
<form action="{YOUR URL}&ref={paramRef}" method="POST" enctype="multipart/form-data">
      <input name="key" value="helloWorld" style="visibility:hidden" />
      <div class="formbold-input-flex">
        <input type="text" name="reference" id="name" value="{ref}" class="formbold-form-input"/>
        <label for="name" class="formbold-form-label"> 주문 참조 </label>
      </div>
      <div class="formbold-input-flex">
        <div>
          <input type="text" name="name" id="name" value="" class="formbold-form-input"/>
          <label for="name" class="formbold-form-label"> 이름 </label>
      </div>
      <div>
        <input type="date" name="deliveryDate" value="" class="formbold-form-input"/>
        <label for="delivery" class="formbold-form-label"> 배송 날짜 </label>
      </div>
    </div>        
      <div>
        <input name="file" type="file" />
      </div>
      <div>
      <button>메시지 보내기</button>
      </div>
    </form>
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

예시로 말씀드리면 이 데이터는 'paremRef'를 1로, 'ref'를 #429로 대체할 것입니다.

## 4. 양식 처리

마지막으로 양식 응답을 처리해야합니다. `HTTP 요청이 수신되었을 때` 또 다른 흐름을 사용합니다. 파일을 업로드해야 하는 요구사항 때문에 약간 복잡해지는 부분입니다. 간단한 JSON 객체 대신 다음과 같은 내용을 얻게 됩니다:

```js
{
  "$content-type": "multipart/form-data; boundary=----WebKitFormBoundarygVtkvZjZZZjbmNzg",
  "$content": "LS0tLS0tV2ViS2l0Rm9ybUJvdW5kYXJ5Z1Z0a3ZaalpaWmp==",
  "$multipart": [
    {
      "headers": {
        "Content-Disposition": "form-data; name=\"key\"",
        "Content-Length": "10"
      },
      "body": {
        "$content-type": "application/octet-stream",
        "$content": "aGVsbG9Xb3JsZA=="
      }
    },
    {
      "headers": {
        "Content-Disposition": "form-data; name=\"name\"",
        "Content-Length": "11"
      },
      "body": {
        "$content-type": "application/octet-stream",
        "$content": "RGF2aWQgV3lhdHQ="
      }
    },
    {
      "headers": {
        "Content-Disposition": "form-data; name=\"deliveryDate\"",
        "Content-Length": "10"
      },
      "body": {
        "$content-type": "application/octet-stream",
        "$content": "MjAyNC0wOC0yMg=="
      }
    },
    {
      "headers": {
        "Content-Disposition": "form-data; name=\"upload\"; filename=\"orderForm.csv\"",
        "Content-Type": "text/csv",
        "Content-Length": "5224"
      },
      "body": {
        "$content-type": "text/csv",
        "$content": "iVBORw0KGgoAAAANSUhEUgAAAMgAAADICAIAAAAiOjnJA"
      }
    }
  ]
}
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

인코딩되어 있어서 모든 값을 출력할 때 base64ToString으로 변환해야 합니다.

이는 $multipart 배열을 순환해야 한다는 것을 의미합니다. 인덱스는 각 값을 식별하는 방법이 됩니다 (또는 헤더/Content-Disposition 필드를 확인하는 스위치를 사용할 수도 있습니다. 여기에 질문이 포함됩니다).

그래서 나의 예제에서 배열 인덱스는 다음과 같습니다.

0 - 인증 키
1 - 주문 참조 (사전 작성된 것)
2 - 이름
3 - 배달 날짜
4 - 파일

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

다음 흐름을 따를 거에요:

![이미지1](/assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_6.png)

![이미지2](/assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_7.png)

![이미지3](/assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_8.png)

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

동봉된 파일은 이미 올바르게 인코딩되어 있으므로 base64로 변환할 필요가 없습니다.

```js
triggerBody()?['$multipart'][4]?['body']
```

<img src="/assets/img/2024-08-21-HowtoCreateYourOwnFormwithPowerAutomate_9.png" />

포스트 본문에 대한 스키마가 없다는 것을 확인할 수 있습니다. 이는 이상한 버그 때문에 발생한 것입니다. 어떤 이유에서인지 트리거가 스키마를 제거해야만 시작이 실패한다는 것이죠. 그래서 저는 플로우를 생성하기 위해 스키마를 사용(모든 매개변수를 쉽게 선택할 수 있도록 함)하고, 게시하기 전에는 제거합니다.

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

인증
인증은 트리거 조건입니다. 올바른 키 값이 없으면 플로우가 실행되지 않도록 지정하여 API가 스팸으로 사용되지 않도록 보장합니다.

```js
@equals(base64ToString(triggerBody()?['$multipart'][0]?['body']?['$content']),'helloWorld')
```

## 이 경우에는 내 암호는 helloWorld입니다.

그게 다에요. 우리는 동적으로 사전 입력된 형식을 가지고 있고, 외부 응답자로부터 파일을 업로드하며, 디자인에 제한이 없습니다.

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


![image](https://res.cloudinary.com/practicaldev/image/fetch/s--sPj7JS0N--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://i.imgur.com/tOpA9Yu.gif)

해결책과 HTML 템플릿은 여기에서 찾을 수 있어요. 만약 'HTTP 요청이 수신될 때' 트리거에 대해 더 알고 싶다면 여기를 확인해보세요. 그리고 어디까지 활용할 수 있는지 보고 싶다면 이전 블로그인 파워 오토메이트에서 Wordle(워들)를 만드는 방법을 확인해보세요.
