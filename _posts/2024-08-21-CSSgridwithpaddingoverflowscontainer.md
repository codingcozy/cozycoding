---
title: "CSS 그리드에서 컨테이너를 넘어가는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-21 16:56
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "CSS grid with padding overflows container"
link: "https://medium.com/@fixitblog/solved-css-grid-with-padding-overflows-container-24a1a4e4512e"
isUpdated: true
updatedAt: 1724246029733
---


친구 같이 이야기하는 것 같아서 기분이 좋아요.

Body의 너비가 100%인 푸터를 만들려고 해요. 푸터의 왼쪽에는 로고, 오른쪽에는 텍스트가 있기를 원해요. 그래서 CSS 그리드를 사용해보기로 결심했어요.

제가 이루고 싶은 것과 거의 똑같은 게 있어요:

```js
.footer {
  background-color: #2D1975;
  width: 100%;
  height: 350px;
  display: grid;
  grid-template-columns: 30% 70%;
}

.footerGridLogo {
  background-color: red;
}

.footerGridQuestions {
  background-color: green;
}
<div class="footer">
  <div class="footerGridLogo"></div>
  <div class="footerGridQuestions"></div>
</div>
```

하지만 로고가 왼쪽 가장자리에 너무 가깝지 않게 하기 위해 왼쪽에 조금의 패딩을 추가하고 싶어요. 10%정도로 하면 로고와 왼쪽 가장자리 사이에 간격이 생기겠죠. 그래서 10-25-65 비율이 되도록 해 보았어요. 그런데 그리드가 넘쳐 흐르는 문제가 발생했어요. 왜 그럴까요?

<div class="content-ad"></div>

문제 시연:

```js
.footer {
  background-color: #2D1975;
  width: 100%;
  height: 350px;
  display: grid;
  padding-left: 10%;
  grid-template-columns: 25% 65%;
}

.footerGridLogo {
  background-color: red;
}

.footerGridQuestions {
  background-color: green;
}
<div class="footer">
  <div class="footerGridLogo">
  </div>
  <div class="footerGridQuestions">
  </div>
</div>
```

다른 10% 그리드 항목을 패딩 대신 추가하는 것만으로 문제를 해결할 수 있다는 것을 깨달았습니다. 그러나 궁금증이 생겨서 패딩이 같은 방식으로 작동하지 않는 이유를 알고 싶습니다.

# 해결 방법

<div class="content-ad"></div>

먼저, 퍼센트 단위 대신 fr 단위를 사용해보세요. 이 단위는 남은 공간을 나타내며 고정된 길이(패딩과 같은)가 렌더링된 후에 계산됩니다.

따라서 이렇게 대신 해보세요:

```js
grid-template-columns: 1fr 2fr
```

<div class="content-ad"></div>


grid-template-columns: 3fr 7fr


자세한 내용은 다음과 같습니다:
- CSS 그리드 레이아웃에서 백분율과 fr 단위의 차이

두 번째로, 컨테이너에서 width: 100%를 제거하세요.

<div class="content-ad"></div>

```js
.footer {
  background-color: #2D1975;
  height: 350px;
  display: grid;
  padding-left: 10%;
  grid-template-columns: 25% 65%;
}
```

.footer를 display: grid로 설정하면(또는 유지하면) 블록 수준 요소가 됩니다. 이러한 요소는 자동으로 부모 요소의 전체 너비를 차지합니다.

그러나 블록 수준 요소에 width: 100%를 추가하면 패딩, 마진 및 테두리가 계산에 추가되어 오버플로우가 발생합니다. 그냥 제거해주세요.

마지막으로, 로고 자체에 패딩을 추가하고, 그리드 컨테이너나 그리드 항목에 패딩을 추가하지 않으면이 전체 문제를 피할 수 있습니다.

<div class="content-ad"></div>

위 내용을 한국어로 번역해드리겠습니다.

응답자 - Michael Benjamin

답변 확인자 - David Marino (수정자원봉사자)

이 응답은 스택 오버플로우에서 수집되었으며, cc by-sa 2.5, cc by-sa 3.0 및 cc by-sa 4.0 라이센스를 따릅니다.