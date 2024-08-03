---
title: "HTML 초보자를 위한 가이드 팝오버, 프로그레스, 다이얼로그 사용 방법 18"
description: ""
coverImage: "/assets/img/2024-08-03-HTMLforBeginnersPopoverProgressDialog18_0.png"
date: 2024-08-03 19:45
ogImage: 
  url: /assets/img/2024-08-03-HTMLforBeginnersPopoverProgressDialog18_0.png
tag: Tech
originalTitle: "HTML for Beginners Popover, Progress, Dialog 18"
link: "https://medium.com/dev-genius/html-for-beginners-popover-progress-dialog-18-32c61876d3d9"
---


![이미지](/assets/img/2024-08-03-HTMLforBeginnersPopoverProgressDialog18_0.png)

## 팝오버

HTML의 팝오버 속성은 상호작용 팝오버 요소를 만드는 방법을 제공하는 속성입니다. 이 팝오버는 사용자 상호작용에 의해 트리거되어 추가 정보나 상호작용 콘텐츠를 표시할 수 있습니다. 이 기능은 JavaScript에 크게 의존하지 않고 팝오버를 만들고 관리하는 과정을 단순화하기 위해 설계되었습니다.

```js
<button popovertarget="target-1">팝오버 열기</button>

<div popover id="target-1">멋진 텍스트</div>
```

<div class="content-ad"></div>

"Open Popover" 버튼을 클릭해 보세요. 버튼 아래에 텍스트가 나타날 거에요.

![이미지](/assets/img/2024-08-03-HTMLforBeginnersPopoverProgressDialog18_1.png)

## 상세 정보

HTML에서 `details` 요소는 사용자가 추가 콘텐츠를 드러내거나 숨길 수 있는 대화식 위젯을 만드는 데 사용됩니다. `summary` 요소는 `details` 요소의 요약 또는 레이블 역할을 합니다.

<div class="content-ad"></div>


<details>
    <summary>로렘 입숨이란 무엇인가요?</summary>
    <p>로렘 입숨은 인쇄 및 조판 산업의 더미 텍스트입니다.</p>
</details>


## 진행 상황

HTML의 `progress` 요소는 다운로드, 파일 업로드 또는 설치 프로세스와 같은 작업의 완료 상태를 나타냅니다.

- max: 진행률 표시 막대의 최대 값이 지정됩니다. 기본 값은 1입니다.
- value: 작업의 어느 정도가 완료되었는지를 나타냅니다. 이 값은 0부터 최대 값 사이여야 합니다.

<div class="content-ad"></div>

```js
<label for="file">파일 진행 상황:</label>
<progress id="file" max="100" value="70">70%</progress>
```

![이미지](/assets/img/2024-08-03-HTMLforBeginnersPopoverProgressDialog18_2.png)

## Meter

```js
<style>
    meter::-webkit-meter-optimum-value {
      background-color: violet;
  }
</style>
    
<label for="fuel">연료:</label>
<meter
  id="fuel"
  value="90"
  min="0"
  max="100"
  low="30"
  high="90"
  optimal="80"
>
  50/100
</meter>
```  

<div class="content-ad"></div>

<img src="/assets/img/2024-08-03-HTMLforBeginnersPopoverProgressDialog18_3.png" />

## 대화 상자

HTML의 팝오버 속성은 상호작용 가능한 팝오버 요소를 만들 수 있는 속성입니다. 이러한 팝오버는 버튼 클릭과 같은 사용자 상호작용을 트리거로 추가 정보나 상호작용 콘텐츠를 표시할 수 있습니다. 이 기능은 자바스크립트에 크게 의존하지 않고 팝오버를 만들고 관리하는 프로세스를 간소화하기 위해 설계되었습니다.

```js
<button id="showDialog">대화 상자 열기</button>
    
<dialog id="myDialog">
    <p>이것은 대화 상자입니다.</p>
    <button id="closeDialog">닫기</button>
</dialog>

<script>
    const dialog = document.getElementById('myDialog');
    const showDialogButton = document.getElementById('showDialog');
    const closeDialogButton = document.getElementById('closeDialog');

    showDialogButton.addEventListener('click', () => {
        dialog.showModal(); // 대화 상자를 모달로 엽니다
    });

    closeDialogButton.addEventListener('click', () => {
        dialog.close(); // 대화 상자를 닫습니다
    });
</script>
```

<div class="content-ad"></div>

아래의 표 태그를 Markdown 형식으로 변경해보세요.

<img src="/assets/img/2024-08-03-HTMLforBeginnersPopoverProgressDialog18_4.png" />

<img src="https://miro.medium.com/v2/resize:fit:400/0*GZIAz8GVUbQsHdjQ.gif" />