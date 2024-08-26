---
title: "CSS만으로도 가능합니다"
description: ""
coverImage: "/assets/img/2024-08-26-NoJSrequiredyoucandothiswithCSS_0.png"
date: 2024-08-26 17:16
ogImage: 
  url: /assets/img/2024-08-26-NoJSrequiredyoucandothiswithCSS_0.png
tag: Tech
originalTitle: "No JS required  you can do this with CSS"
link: "https://medium.com/@bogdanfromkyiv/no-js-required-you-can-do-this-with-css-e4635e40502c"
isUpdated: false
---


사이트에서 JS 코드를 최대한 적게 사용하는 것이 성능면에서 더 좋습니다.

왜냐하면 여러 가지 이유가 있기 때문입니다:

- JS를 적게 사용하면 TBT도 적어집니다.
- CSS는 JS보다 파싱하고 실행하는 속도가 빠릅니다.
- JS를 사용하면 레이아웃이 다시 렌더링되지만 CSS의 경우 그렇지 않습니다.

마지막으로 CSS에 오류가 있다고 해도 페이지 로딩을 막지 않습니다. 그러나 JS 코드에 오류가 있을 경우 페이지 전체가 정상적으로 작동하지 않을 수 있습니다.

<div class="content-ad"></div>

고지! 저는 당신에게 JS 사용을 그만두라고 말하고 있지 않아요 😅

하지만 JS를 CSS로 쉽게 대체할 수 있는 경우에는 대체해 주세요.

CSS만 사용하여 사이트에 유용한 기능 몇 가지를 살펴보세요.

## 호버 시 하위 메뉴

<div class="content-ad"></div>

간단한 것부터 시작해봐요. 서브 메뉴를 만들 때 JS를 사용할 필요는 없어요. 우리 모두 :hover 으로 알려진 가상 클래스를 사용할 수 있으니 망설이지 마세요.

다음은 간단한 메뉴 마크업입니다:

```js
<nav>
  <ul>
    <li><a href="#">Home</a></li>
    <li><a href="#">About</a></li>
    <li><a href="#">Blog 🠋</a>
      <ul>
        <li><a href="#">News</a></li>
        <li><a href="#">Interviews</a>
        <li><a href="#">Travel</a></li>
        <li><a href="#">Art</a></li>
        <li><a href="#">Tech</a></li>
        </li>
      </ul>
    </li>
    <li><a href="#">Contacts</a></li>
  </ul>
</nav>
```

일반적으로 서브 메뉴를 숨기기 위해 사람들은 display: none; 방법을 자주 사용해요.

<div class="content-ad"></div>

저는 호버 효과를 부드럽게 만들기 위해 투명도(opacity)와 포인터 이벤트(pointer-events) CSS 속성을 결합해서 사용하는 것을 선호합니다 (브라우저는 아직 display CSS 속성을 애니메이션화 할 수 없습니다). 포인터 이벤트 속성 pointer-events: none;은 하위 메뉴가 표시될 때까지 (상위 요소에 호버될 때까지) 하위 메뉴를 클릭할 수 없게 만듭니다.

```js
/* 하위 메뉴의 부모 요소에 상대적인 위치를 설정하는 것을 잊지 마세요 */
nav > ul > li {
  position: relative;
}
/* 하위 메뉴는 기본적으로 숨겨져 있습니다 */
nav > ul ul {
  position: absolute;
  opacity: 0;
  pointer-events: none;
  translate: 0 25%;
  transition: all 250ms cubic-bezier(.33,.65,.67,.81);
}
/* 호버 시 표시되도록 만듭니다 */
nav > ul > li:hover > ul {
  opacity: 1;
  pointer-events: all;
  translate: 0 0;
}
```

그리고 최종 결과는 다음과 같습니다:

# 탭형 콘텐츠

<div class="content-ad"></div>

`label`과 `input type=”radio”`을 결합하면 마법이 일어날 수 있어요! `label`을 클릭하면 브라우저가 자동으로 해당 입력란으로 초점을 이동시키죠. 라디오 입력란의 경우, 선택될 거예요.

레이아웃은 간단해요: 입력-라벨 요소와 관련 있는 콘텐츠의 div 요소를 여러 개 만들어요.

```js
<!-- 라디오 버튼 -->
<input type="radio" id="tab1" name="tabs" checked>
<label for="tab1">탭 1</label>

<input type="radio" id="tab2" name="tabs">
<label for="tab2">탭 2</label>

<input type="radio" id="tab3" name="tabs">
<label for="tab3">탭 3</label>

<!-- 콘텐츠 -->
<div class="tab-content" id="content1">
  <h2>탭 1 콘텐츠</h2>
  <p>첫 번째 탭 콘텐츠에요.</p>
</div>

<div class="tab-content" id="content2">
  <h2>탭 2 콘텐츠</h2>
  <p>두 번째 탭 콘텐츠에요.</p>
</div>

<div class="tab-content" id="content3">
  <h2>탭 3 콘텐츠</h2>
  <p>세 번째 탭 콘텐츠에요.</p>
</div>
```

그럼 CSS의 아름다움을 즐기세요!

<div class="content-ad"></div>


/* 라디오 버튼 숨기기 */
input[type="radio"] {
  display: none;
}

/* 레이블을 탭으로 스타일 지정 */
label {
  display: inline-block;
  padding: 10px 20px;
  margin: 0;
  cursor: pointer;
  background-color: #eee;
  border: 1px solid #ccc;
  border-bottom: none;
  transition: background-color 0.3s ease;
}

/* 활성화된 탭의 스타일 지정 */
input[type="radio"]:checked + label {
  background-color: #fff;
  border-bottom: 1px solid #fff;
  font-weight: bold;
}

/* 콘텐츠 영역 스타일 지정 */
.tab-content {
  display: none;
  padding: 20px;
  border: 1px solid #ccc;
  border-top: none;
  background-color: #fff;
}

/* 활성화된 콘텐츠 표시 */
#tab1:checked ~ #content1,
#tab2:checked ~ #content2,
#tab3:checked ~ #content3 {
  display: block;
}


CSS에는 확인란(라디오 버튼 및 체크박스 모두)에 대한 기본 :checked 선택자가 있습니다.

그리고 순서 형제 결합자로 공식적으로 묘사되는 ~ (물결표) 덕분에 콘텐트 블록의 가시성을 전환할 수 있습니다. 유일한 요구 사항은 콘텐츠 div가 라디오 입력창 뒤에 배치되어야 한다는 것입니다. 이것이 최종 결과입니다:


<div class="content-ad"></div>

## Snap 블록

데스크톱에서 YouTube Shorts를 스크롤해 본 적이 있나요? 각 휠 스크롤 후 다음 비디오가 페이지 상단에 바로 붙습니다:

![Snap block](/assets/img/2024-08-26-NoJSrequiredyoucandothiswithCSS_0.png)

만약 이게 복잡한 JS 코드라고 생각하신다면 — 그것은 틀렸어요! 이것은 scroll-snap이라는 내장 CSS 기능입니다.

<div class="content-ad"></div>

먼저, 부모 컨테이너에 대해 scroll-snap-type을 설정해야 합니다. YouTube Shorts처럼 작동하도록 설정하려면 y mandatory로 설정해야 합니다.

```js
.scroll-container {
  display: flex;
  flex-direction: column;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
  width: 300px;
  height: 80%;
  background-color: #ddd;
  padding: 20px;
  box-sizing: border-box;
  scroll-padding: 10px;
}
```

이는 스크롤 컨테이너가 수직 축에서만 스냅되며 현재 스크롤되지 않았을 때 콘텐츠가 스냅 위치로 이동해야 한다는 것을 의미합니다. 이 속성에 대한 각 가능한 값에 대한 상세한 설명을 위해 MDN 문서를 확인하십시오.

그리고 스크롤된 각 항목에 대해 해당 스냅 영역 내에서 항목의 스냅 위치를 제공해야 합니다. 다시 말해, 항목을 맨 위 (시작), 중앙 또는 맨 아래 (끝)에 스냅시키고 싶은지를 의미합니다. 이는 scroll-snap-align 속성을 통해 가능합니다.

<div class="content-ad"></div>

```js
.scroll-item {
  scroll-snap-align: start;
}
```

데모를 한 번 살펴보세요. 정말 멋진 기능이며 CSS만 사용했어요 😊

저의 머리 속에 떠오른 몇 가지 꿀팁을 공유했지만, 이것들은 얼음산의 일각에 불과해요. 제가 두 번째 부분을 공유하길 원한다면 "박수" 버튼을 클릭해주세요. 저에게 귀여운 관심을 보여주는 것이죠 😊

이 기사가 도움이 되었다면 망설이지 말고 박수를 보내주시고, 구독하고 의견을 남겨주세요 😊


<div class="content-ad"></div>

읽어 주셔서 감사합니다!
안전하고 행복하시고, 평화가 함께 하길 바랍니다!