---
title: "자바스크립트로 핸드폰 연락처 읽어오는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-ReadPhoneContactswithJavaScript_0.png"
date: 2024-08-21 18:48
ogImage: 
  url: /assets/img/2024-08-21-ReadPhoneContactswithJavaScript_0.png
tag: Tech
originalTitle: "Read Phone Contacts with JavaScript"
link: "https://dev.to/alvaromontoro/read-phone-contacts-with-javascript-1j2"
isUpdated: true
updatedAt: 1724245421002
---


저자의 참고 사항: 이 문서에서 설명하는 기술과 프로세스는 실험적이며 일부 브라우저에서만 작동합니다. 작성 시점에서 Contact Picker API는 Android Chrome(버전 80 이상)과 iOS Safari(버전 14.5 이상, 그러나 플래그 설정 필요)에서만 지원됩니다. 기능을 확인하려면 제 웹사이트에서 실행 중인 데모를 확인할 수 있습니다.

전화나 태블릿의 연락처 목록에서 항목을 읽는 것은 일반적으로 네이티브 앱에만 제한되었습니다. 그러나 Contact Picker API를 사용하면 JavaScript를 사용하여 필요한 연락처 정보를 가져올 수 있습니다.

이 기능은 전화번호나 VoIP와 같은 연락처 정보가 필요한 앱, 알려진 사람을 찾고 싶은 소셜 네트워크, 또는 데이터를 확인하기 위해 애플리케이션을 전환하지 않고 양식 정보를 입력해야 하는 앱에서 흥미로울 수 있습니다.

API 및 장치에 따라 사용 가능한 속성을 제한할 수 있습니다. 개발자가 선택할 수 있는 다섯 가지 표준 속성이 있습니다.

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

- 이름
- 전화번호
- 이메일
- 주소
- 아이콘

여기서 복수형은 중요합니다. 연락처는 여러 전화번호, 이메일 또는 여러 주소를 가질 수 있습니다. 일관성을 위해 반환된 데이터는 항상 배열 내에 있을 것이며 값이 하나인 경우에도 그렇습니다. 이에 대해 나중에 자세히 설명하겠습니다.

## 개인 정보 보호 및 보안

전화에 저장된 연락처 정보에는 신중히 다루어야 할 민감한 정보가 포함될 수 있습니다. 따라서 고려해야 할 개인 정보 보호 및 보안 사항이 있습니다.

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

- Contact Picker API 코드는 최상위 레벨의 브라우징 컨텍스트에서 실행되어야 합니다. 이는 광고나 서드파티 플러그인과 같은 외부 코드가 사용자의 전화의 연락처 목록을 읽는 것을 방지합니다.
- Contact Picker API 코드는 사용자 제스처 이후에만 실행될 수 있습니다. 따라서 개발자들은 프로세스를 완전히 자동화할 수 없습니다. 사용자가 연락처를 읽도록 유도하기 위해 사용자가 반응해야 합니다.
- 사용자는 연락처 목록에 대한 액세스를 허용해야 합니다. 이 제한은 JS가 아닌 전화로부터 부과되었습니다. 사용자는 브라우저가 연락처에 액세스할 권한을 부여해야 합니다 (이미 부여되어 있지 않은 경우).

Contact Picker API를 사용하는 웹 사이트를 처음 사용할 때, 다음과 같은 메시지가 나타날 수 있습니다:

사용자가 "허용"을 탭할 때까지, 전화는 매번 팝업을 표시할 것입니다. Contact Picker API는 그 일이 발생할 때까지 실행되지 않습니다. 이것은 좋은 점입니다; 사용자가 적절한 권한을 부여하도록 보장하고 있습니다. 또한 한 번만 이루어진다는 것도 좋습니다; 페이지가 Contact Picker API 코드를 실행할 때마다 권한을 부여해야 한다면 불편할 것입니다.

## API와 코드

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

Contact Picker API는 두 가지 메소드만을 정의합니다:

- getProperties(): 기기에서 읽을 수 있는 속성 목록을 반환합니다. 정의에는 "address", "email", "icon" (이는 연락처 사진이 아닐 수 있습니다), "name", "tel" (전화)의 다섯 가지만 있지만, 기기가 모든 속성에 대한 액세스를 허용하지 않을 수 있습니다.
- select(): 연락처 팝업을 열고 사용자가 작업을 완료하면 선택한 것을 반환합니다. 두 개의 매개변수를 사용합니다: 읽을 속성 목록 및 옵션을 가진 선택적 객체.

두 메소드 모두 프로미스를 반환하지만, 앱의 일반적인 흐름을 차단하는 작업을 고려하여 이를 처리할 때는 async/await를 사용해야 합니다.

getProperties()를 무시하고 모든 속성을 직접 요청하는 것이 유혹적일 수 있습니다. 하지만 주의해야 할 점은, 요청한 속성 중 하나라도 사용할 수 없는 경우 select() 메소드에서 예외가 발생할 수 있습니다.

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

### 예시

Contact Picker API의 데모가 작동 중입니다. (여기에서 온라인 실행하거나 이 비디오를 시청하세요). API가 지원되면 연락처의 전화번호, 이름 및 이메일을 표시하는 버튼이 표시됩니다.

먼저, 버튼이 필요합니다. 개인 정보 및 보안 섹션에서 자세히 설명한대로 사용자 동작이 필요하기 때문에 API를 호출하기 전에 사용자 상호작용이 없으면 아무것도 트리거할 수 없습니다:

```js
<button onclick="getContactData()">연락처 데이터 표시</button>
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

getContactData() 함수에 주요 코드가 포함될 것입니다. 그 전에, Contact Picker API가 사용할 수 없는 경우 버튼을 표시하는 것이 어떤 의미일까요? API를 사용할 수 없는 경우 버튼을 숨겨봅시다. 더 좋은 방법은 기본적으로 버튼을 숨기고(API를 사용할 수 있는 경우만) 필요할 때만 표시하는 것입니다.

```js
// 브라우저가 Contact Picker API를 지원하는 경우에만 버튼 보이기
if ("contacts" in navigator) {
  document.querySelector("button").removeAttribute("hidden");
}
```

이제 버튼 로직이 준비되었으니 getContactData() 함수에 집중해 봅시다. 다음은 함수에 대한 주석이 달린 버전입니다:

```js
// modal 선택이 완료될 때까지 기다릴 것이기 때문에 비동기로 처리됩니다.
async function getContactData() {
  // 읽을 연락처 값들을 지정합니다.
  const props = ["tel", "name", "email"];

  // 모든 것을 try...catch 문으로 감싸 예외 처리를 합니다.
  try {
    // 네이티브 연락처 선택기를 엽니다 (권한 허용 후)
    const contacts = await navigator.contacts.select(props);

    // 네이티브 연락처 선택기가 닫힌 후에 실행됩니다.
    if (contacts.length) {
      // 데이터가 있는 경우 보여줍니다.
      alert("선택한 데이터: " + JSON.stringify(contacts));
    } else {
      // 데이터가 없는 경우 선택이 안 되었다고 표시합니다.
      alert("선택한 데이터가 없습니다");
    }
  } catch (ex) {
    // 오류가 발생하는 경우 오류 메시지를 보여줍니다.
    alert(ex.message);
  }
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

버튼이 이 기능을 트리거하고, 브라우저가 권한을 허용한 경우(이전 섹션의 스크린샷 참조), 연락처 모달이 나타납니다. 모달에는 URL에 있는 데이터를 읽는 방법, 반환할 데이터 및 선택할 연락처 목록과 함께 중요한 정보가 표시됩니다.

모달을 닫은 후, contacts 변수에 요청된 정보가 포함된 객체로 JSON 형식의 데이터가 저장됩니다(연락처 카드에서 사용할 수 없는 경우 비어 있을 수 있음).

예를 들어, 연락처로 자기 자신을 선택한 후의 결과는 다음과 같습니다(가짜 데이터):

```js
[
  {
    "address": [],
    "email": [ "alvarosemail@gmail.com" ],
    "icon": [],
    "name": [ "Alvaro Montoro" ],
    "tel": [ "555-555-5555", "555-123-4567" ]
  }
]
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

데이터에 아이콘이 포함되어 있다면 이미지를 나타내는 blob입니다. 주소가 포함되어 있다면 거리, 도시, 국가, 우편번호 등과 같은 더 복잡한 객체가 됩니다. 규격서에서 반환된 값들을 확인할 수 있습니다.

하지만 왜 하나의 연락처만 선택했는데 배열인가요? 여러 연락처를 선택할 수 있는 옵션이 있기 때문입니다!

### 여러 연락처 선택하기

하나 이상의 연락처를 선택하는 것이 가능합니다. 그렇게 하려면 navigator.contacts.select() 메서드에 이 옵션을 나타내는 두 번째 매개변수를 전달해야 합니다.

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
const props = ["tel", "address", "icon", "name", "email"];
// 하나의 옵션만 사용 가능: 여러 개 또는 하나만 읽기 (기본값)
const options = { multiple: true };

try {
  const contacts = await navigator.contacts.select(props, options);
  // ...
```

결과는 연락처 배열이므로 이 예제에서 나머지 코드는 동일합니다.

위의 코드는 내가 추가한 모든 주석 때문에 압박감을 주지만 여기에 저렇게 가볍게 주석을 다는 코드 예시가 있습니다. 눈치채시겠지만, 굉장히 간단합니다:

```js
async function getContactData() {
  if ("contacts" in navigator) {
    const props = await navigator.contacts.getProperties();
    const options = { multiple: true };

    try {
      const contacts = await navigator.contacts.select(props, options);

      if (contacts.length) {
        // 선택한 데이터 관리 코드
      } else {
        // 아무것도 선택되지 않았을 때 코드
      }
    } catch (ex) {
      // 오류가 발생했을 때 코드
    }
  }
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

웹 사이트에서 실행 중인 데모를 확인할 수 있어요. 걱정 마세요. 연락처 정보를 화면에 표시하는 것 외에는 아무 것도 안 해요. 그런데, 제가 믿음직하지 않다고 생각한다면 코드를 먼저 확인해 보세요.

## 결론: 개인 정보 보호가 해적 행위보다 중요합니다

연락처 정보는 개인을 식별할 수 있는 정보(PII)이며, 민감한 데이터에 필요한 모든 주의와 보안 조치를 취해야 합니다.

나라별로 변화하고 있는 법적 요구 사항에 대해 자세히 설명하지는 않겠지만, 민감한 데이터를 다룰 때 몇 가지 기본 가이드라인이 있어요:

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

- 다른 사람들의 프라이버시를 존중해주세요. 원치 않는 정보를 공유하게 강요하지 마세요.
- 데이터를 주의 깊게 다루고 안전하게 처리해주세요. 당신이 처리하는 데이터가 자신의 것이라면 편안할지 생각해봐주세요.
- 필요하지 않다면 데이터를 저장하지 마세요. 읽고 사용한 후에는 잊어버려주세요. 사용하지 않는 데이터를 저장하지 마세요.
- 필요한 데이터만 얻어주세요. 교활하거나 의심스러운 행동을 하지 마세요. 신뢰와 신임성을 쌓기 위해 필요한 것만 얻어주세요.

만약 웹 앱이 전화번호를 선택하는 동안 주소, 이름 또는 이메일을 읽어보려고 한다면, 저라면 허락을 거부하고 웹 사이트를 나갈 것입니다.

그래서 JavaScript와 연락처 선택기 API를 탐험하되, 항상 화면 뒤에 사람이 있다는 것을 기억하고, 그들이 공유하는 데이터가 잘못된 손에 빠진다면 위험할 수 있다는 것을 기억해주세요. 경솔하지 마세요.

만약 JavaScript에 관한 이 글을 즐기셨고 JS로 웹 API와 다른 것들을 테스트하고 싶다면, 이 다른 글도 확인해보세요:

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


![image](/assets/img/2024-08-21-ReadPhoneContactswithJavaScript_0.png)
