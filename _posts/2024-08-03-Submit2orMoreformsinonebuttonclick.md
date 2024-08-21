---
title: "한 번의 버튼 클릭으로 여러 개의 폼 제출하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 18:07
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Submit 2 or More forms in one button click"
link: "https://dev.to/armanrahman/submit-2-or-more-forms-in-one-button-click-30aj"
isUpdated: true
updatedAt: 1724246283839
---


귀하의 웹 페이지에 두 개의 양식이 다른 위치에 있지만 한 번에 두 양식을 제출하려면 다음을 사용하십시오 -

귀하의 HTML:

```js
<form id="form1">
   <!-- 양식 1 필드 -->
   <input type="text" name="field1" placeholder="필드 1">
</form>

<form id="form2">
    <!-- 양식 2 필드 -->
    <input type="text" name="field2" placeholder="필드 2">
</form>
```

JS:

<div class="content-ad"></div>

```js
window.addEventListener('load', function() {
            // 폼 데이터 가져오기
            let form1 = new FormData(document.getElementById('form1'));
            let form2 = new FormData(document.getElementById('form2'));

            // 두 폼 데이터를 URLSearchParams로 결합
            let urlParams = new URLSearchParams();
            form1.forEach((value, key) => {
                urlParams.append(key, value);
            });
            form2.forEach((value, key) => {
                urlParams.append(key, value);
            });

            // 결합된 폼 데이터가 포함된 새로운 URL로 리다이렉트
            window.location.href = '/submit-both-forms?' + urlParams.toString();
        });
```

이 방법은 해당 위치로 GET 요청을 보냅니다.