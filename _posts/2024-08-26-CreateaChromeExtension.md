---
title: "크롬 익스텐션 만드는 방법과 예제 코드"
description: ""
coverImage: "/assets/img/2024-08-26-CreateaChromeExtension_0.png"
date: 2024-08-26 17:06
ogImage: 
  url: /assets/img/2024-08-26-CreateaChromeExtension_0.png
tag: Tech
originalTitle: "Create a Chrome Extension"
link: "https://medium.com/@theriyasharma24/create-a-chrome-extension-1a77c4fc16e7"
isUpdated: false
---



![Chrome Extension](/assets/img/2024-08-26-CreateaChromeExtension_0.png)

크롬 익스텐션을 만든 적이 있나요? 저와 같이 새로운 기술 도구에 뛰어드는 것이 항상 흥미로운 분이라면, 나만의 크롬 익스텐션을 만드는 것은 여러분의 브라우징 경험을 향상시키거나 다른 사람들이 사용하는 것을 도와줄 수 있는 보상적인 방법일 것입니다. 이 안내서에서는 크롬 익스텐션을 만드는 기본 사항을 안내하고 참 쉬운 예제를 제공할 것입니다: 모든 새로운(빈) 탭을 닫는 크롬 익스텐션입니다.

## 크롬 익스텐션이란?

크롬 익스텐션은 브라우징 경험을 사용자 지정하는 작은 소프트웨어 프로그램입니다. 크롬의 기능과 동작을 개별적으로 필요에 맞게 조정할 수 있게 해주며, HTML, JavaScript, CSS와 같은 웹 기술을 사용하여 만들어져 웹 개발자들이 모두 사용할 수 있습니다.


<div class="content-ad"></div>

## 기본 사항

크롬 확장프로그램을 만들려면 세 가지 주요 파일이 필요합니다:

1. 매니페스트 파일 (manifest.json): 이 파일은 확장프로그램에 대한 메타데이터입니다. 크롬에 확장프로그램, 버전, 필요한 권한 및 기타 중요한 정보에 대해 알려줍니다.

2. HTML 파일: 확장프로그램에 사용자 인터페이스(UI)가 있다면 HTML과 CSS를 사용하여 디자인합니다.

3. JavaScript 파일: 이 파일에는 확장프로그램의 동작을 제어하는 확장프로그램의 로직이 포함되어 있습니다.

<div class="content-ad"></div>

## 예시: 모든 새 탭을 닫기 위한 Chrome 확장 프로그램 만들기

기술적인 개념을 이해하기 위해 항상 예시를 참조해요. 예를 들어, 종종 너무 많은 탭을 열고 하나의 클릭으로 모든 새 탭을 닫을 수 있는 쉬운 방법이 있었으면 좋겠다고 생각해봅시다. 우리는 그것을 수행하는 확장 프로그램을 만들어볼 거에요!

단계 1: 매니페스트 파일 설정

컴퓨터에 새 폴더를 만들고 manifest.json이라는 파일을 추가하세요. 아래는 어떻게 보일 수 있는지에 대한 기본적인 예시입니다:

<div class="content-ad"></div>

```js
{
 "manifest_version": 3,
 "name": "Close All New Tabs",
 "version": "1.0",
 "description": "열려진 모든 새 탭을 닫습니다.",
 "permissions": ["tabs"],
 "background": {
 "service_worker": "background.js"
 },
 "action": {
 "default_popup": "popup.html"
 }
}
```

## 단계 2: 백그라운드 스크립트 생성하기

동일한 폴더에 `background.js`라는 파일을 만드십시오. 이 스크립트는 탭을 닫는 기능을 관리할 것입니다:

```js
chrome.tabs.query({}, function(tabs) {
 for (let tab of tabs) {
 if (tab.url === 'chrome://newtab/') {
 chrome.tabs.remove(tab.id);
 }
 }
});
```

<div class="content-ad"></div>

## 단계 3: UI 만들기

수동으로 탭 닫기 작업을 트리거하는 버튼을 제공하려면 popup.html 파일을 만드세요:

```js
<!DOCTYPE html>
<html>
 <head>
 <style>
 body { font-family: Arial, sans-serif; padding: 10px; }
 button { padding: 10px; background-color: #f44336; color: white; border: none; cursor: pointer; }
 </style>
 </head>
 <body>
 <button id=”close-tabs”>새 탭 닫기</button>
 <script src=”popup.js”></script>
 </body>
</html>
```

그리고 버튼 클릭을 처리하는 popup.js를 만드세요:

<div class="content-ad"></div>

```js
document.getElementById('close-tabs').addEventListener('click', () => {
  chrome.tabs.query({}, function(tabs) {
    for (let tab of tabs) {
      if (tab.url === 'chrome://newtab/') {
        chrome.tabs.remove(tab.id);
      }
    }
  });
});
```

## 단계 4: Chrome에서 확장 프로그램로 로드하기

1. Chrome을 열고 chrome://extensions/로 이동합니다.
2. 오른쪽 상단의 "개발자 모드"를 활성화합니다.
3. "압축 해제된 확장 프로그램을 로드"를 클릭하고 확장 프로그램 파일이 있는 폴더를 선택합니다.

이제 확장 프로그램을 사용할 준비가 되었습니다! 한 번의 클릭으로 모든 새 탭을 닫아 브라우징 경험을 효율적으로 만들어보세요.

<div class="content-ad"></div>

# Chrome 확장 프로그램을 게시하세요

크롬 확장 프로그램을 만드는 데 많은 노력을 기울인 후, 다음으로 진행할 흥미로운 단계는 Chrome 웹 스토어에 게시하여 전 세계와 공유하는 것입니다. 수백만 명의 사용자에게 확장 프로그램을 제공하려면 다음 빠른 안내서를 따라보세요.

## 단계 1: 확장 프로그램 준비

게시하기 전에, 확장 프로그램이 대중에게 준비되었는지 확인하세요:

<div class="content-ad"></div>

- 철저한 테스트: 확장 기능이 버그 없이 정상적으로 작동하는지 확인하세요. 안정성을 보장하기 위해 다양한 시나리오에서 테스트하세요.
- 에셋 생성: 몇 가지 그래픽이 필요합니다:
  - 128x128 픽셀 아이콘: 스토어에서 당신의 확장 기능을 대표합니다.
  - 스크린샷 (1280x800 또는 640x400 픽셀): 당신의 확장 기능이 작동 중인 clear한 이미지를 제공하세요. 사용자들이 확장 기능이 무엇을 하는지 이해하는 데 도움이 됩니다.
  - 프로모션 이미지 (옵션): 확장 기능을 시각적으로 홍보하기 위한 440x280 픽셀 이미지.

3. 자세한 설명 작성: 확장 기능이 무엇을 하는지 명확히 설명하고, 사용자에게 어떤 이점을 제공하는지 설명하며, 발견성을 향상시키는 데 도움이 되는 관련 키워드를 포함하세요.

## 단계 2: 개발자 계정 생성

<div class="content-ad"></div>

확장 프로그램을 출판하려면 개발자 계정이 필요합니다:

- Chrome 웹 스토어 개발자 대시보드로 이동하세요.
- Google 계정으로 로그인하세요.
- 개발자 계약에 동의하세요.
- $5의 일회성 등록 비용을 지불하세요.

## 단계 3: 확장 프로그램 업로드하기

- 확장 프로그램 압축하기: 확장 프로그램 파일(매니페스트, HTML, CSS, JS 파일 및 이미지 포함)을 .zip 파일로 묶어주세요.
- 개발자 대시보드로 이동: 계정 설정이 완료되면 대시보드에서 "새 항목 추가"를 클릭하세요.
- Zip 파일 업로드하기: 확장 프로그램의 .zip 파일을 선택하고 업로드하세요.

<div class="content-ad"></div>

## 단계 4: 세부 정보 입력

확장 프로그램을 업로드하면 추가 정보를 입력하라는 프롬프트가 표시됩니다:

- 이름 및 설명: 확장 프로그램의 이름과 간결하고 매력적인 설명을 입력하세요.
- 카테고리: 확장 프로그램에 가장 적합한 카테고리를 선택하세요.
- 언어: 확장 프로그램의 기본 언어를 선택하세요.
- 스크린샷 및 아이콘: 미리 준비한 스크린샷과 아이콘을 업로드하세요.
- 권한: 확장 프로그램이 필요로 하는 권한을 검토하고 설명하세요.

## 단계 5: 심사를 위해 제출

<div class="content-ad"></div>

세부 정보를 모두 입력한 후:

- 저장 및 게시: 모든 것에 만족하셨으면 "게시"를 클릭하세요. 휴! 여기서 일이 끝났어요. 구글은 여러분이 작성한 확장 프로그램이 가이드라인을 충족하는지 확인할 것입니다.
- 심사 과정: 심사 과정은 몇 시간에서 몇 일이 걸릴 수 있습니다. 확장 프로그램이 승인되고 게시되면 이메일을 받게 됩니다. (제 경우엔 게시되는 데 하루가 걸렸어요!)

## 단계 6: 확장 프로그램 관리하기

게시한 후, 여러분의 확장 프로그램은 누구나 설치할 수 있는 크롬 웹 스토어에서 이용할 수 있습니다. 개발자 대시보드를 통해 업데이트를 관리하고 성능을 추적하며 사용자 피드백에 응답할 수 있습니다.

<div class="content-ad"></div>

## 나의 첫 번째 크롬 익스텐션

저는 최근에 처음으로 크롬 익스텐션을 출시했어요. 그것은 간단한 단일 탭 정리 도구로, 모든 열린 탭을 닫고 그 링크를 하나의 목록에 저장합니다. 이제 브라우저가 최대 90% 적은 메모리 공간을 차지하며, 원하는대로 모든 탭 또는 개별 탭을 쉽게 다시로드할 수 있어요.

여기서 확인하고 리뷰도 남겨주세요 - Single Tab Organizer.

## 결론

<div class="content-ad"></div>

크롬 익스텐션을 만드는 것은 웹 개발 기술을 향상시키고 자신과 다른 사람들에게 유용한 도구를 제공하는 환상적인 방법입니다. 작게 시작하고 실험해보면 곧 강력한 브라우저 익스텐션 세트를 만들어 세상과 공유할 수 있을 거예요!

여기까지 읽어주셨다면, 이 기사에 👏을 꼭 눌러 주세요.

즐거운 코딩하세요! ✨😁