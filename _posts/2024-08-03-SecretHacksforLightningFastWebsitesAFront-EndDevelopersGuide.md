---
title: " 번개처럼 빠른 웹사이트를 만드는 비밀 팁 프론트엔드 개발자를 위한 가이드"
description: ""
coverImage: "/assets/img/2024-08-03-SecretHacksforLightningFastWebsitesAFront-EndDevelopersGuide_0.png"
date: 2024-08-03 19:41
ogImage: 
  url: /assets/img/2024-08-03-SecretHacksforLightningFastWebsitesAFront-EndDevelopersGuide_0.png
tag: Tech
originalTitle: " Secret Hacks for Lightning Fast Websites A Front-End Developers Guide"
link: "https://medium.com/javascript-in-plain-english/secret-hacks-for-lightning-fast-websites-a-front-end-developers-guide-d9e4e916717d"
---


![2024-08-03-SecretHacksforLightningFastWebsitesAFront-EndDevelopersGuide_0](/assets/img/2024-08-03-SecretHacksforLightningFastWebsitesAFront-EndDevelopersGuide_0.png)

웹사이트가 왜 번개처럼 빠르게 로드되는지 궁금했던 적이 있나요? 🤔 마법이 아니라, 효율적인 코드 작성 덕분이죠. HTML, CSS, 및 DOM 조작에 입문한 적이 있다면, 올바른 길을 걸어가고 있습니다! 이제 최적화 기술에 대해 알아볼까요?

# 브라우저의 뒷담화 작업 🕵️‍♀️

브라우저를 일하는 열심히 일하는 사람들로 생각해보세요. 여기에 그들의 작업 과정이 있습니다:

<div class="content-ad"></div>

- HTML 구문 분석: 그들은 당신의 HTML을 청사진처럼 읽고 DOM (문서 객체 모델) 트리를 만듭니다. 🌳
- CSS 스타일링: 그리고 그들은 당신의 CSS 스타일을 가져와 CSSOM (CSS 객체 모델) 트리를 만듭니다. 🎨
- 협력하는 힘: DOM과 CSSOM 트리가 강력한 "렌더 트리"로 통합됩니다. 🤝
- 레이아웃 계획: 이제 브라우저는 각 요소의 정확한 위치와 크기를 계산하여 레이아웃을 만듭니다. 📐
- 대단한 결말: 마침내, 당신의 웹사이트가 화면에 픽셀 단위로 그려집니다! ✨

그런데 여기 한 가지 문제가 있어요: 복잡한 CSS는 렌더링 프로세스를 느리게 만들 수 있어요. 그것을 최적화해봐요!

# CSS 최적화 팁들 🧰

1. 와일드카드 선택자 떨어뜨리기! 🚫

<div class="content-ad"></div>

우리 모두 다 거쳐 본 적이 있습니다:

```js
* {
    margin: 0px;
    padding: 0px;
}
```

이렇게 모든 스타일을 초기화하면 브라우저에게 페이지의 각 요소를 확인하도록 요청하는 것과 같습니다. 큰 웹사이트에 대해서는 성능이 제대로 나오지 않을 수 있습니다. 🐢

전문가 팁: CSS Tools: Reset CSS와 같은 CSS 초기화 스타일 시트를 사용하세요.

<div class="content-ad"></div>

2. 선택자를 명확하게 지정하세요 🎯

다음을 고려해보세요:

```js
<ul class="list">
  <li class="list-item"></li>
</ul>
```

리스트 항목에 스타일을 적용하려면, 어떤 CSS 선택자가 더 효율적인가요? 🤔

<div class="content-ad"></div>

A: .list li
B: .list .list-item

B가 우승자입니다! 🎉 전체 DOM을 검색하지 않고도 브라우저에 정확히 어떤 요소를 스타일링해야 하는지 알려줍니다.

3. Reflow 최소화하기 — 성능 하락 요인 📉

Reflow는 브라우저가 페이지의 레이아웃을 다시 계산해야 할 때 발생합니다. 이는 새로운 항목을 추가할 때마다 가구를 재배치하는 것과 같습니다. Reflow가 적을수록 좋습니다!

<div class="content-ad"></div>

리플로우 트리거:

- DOM 요소를 추가, 제거 또는 변경함.
- 요소 크기에 영향을 주는 스타일을 수정함.
- 브라우저 창 크기를 조정함.

예시:

다음처럼 하는 대신에:

<div class="content-ad"></div>

```js
const list_item = document.querySelector('.list');
const fragment = document.createDocumentFragment();

for(let i = 0; i < 10000; i++){
    const li = document.createElement('li');
    li.textContent = 'I am Xiuer Old';
    fragment.appendChild(li);
}

list_item.appendChild(fragment);
```

<div class="content-ad"></div>

4. DocumentFragment의 힘을 믿어 보세요 🦸

DocumentFragment을 DOM 요소의 가상 컨테이너로 생각해보세요. 이 안에 요소를 추가, 제거, 수정할 수 있습니다. 이것은 실제 페이지에서 reflows를 유발하지 않고 작업할 수 있습니다. 마치 퍼즐을 프레임에 넣기 전에 따로 판에서 조립하는 것과 비슷해요. 🖼️

```js
let content = document.createDocumentFragment();
const list_item = document.querySelector('.list');
for (let i = 0; i < 10000; i++) {
    let li = document.createElement('li');
    li.innerHTML = 'I am Xiuer Old';
    content.appendChild(li);
}
container.appendChild(content);
```

# 마무리하며:

<div class="content-ad"></div>

웹 브라우저가 웹 페이지를 렌더링하는 방식을 이해하면 CSS에 대한 결정을 내릴 수 있습니다. 이런 기술들은 프론트엔드 최적화에 관한 아이스버그의 일각에 불과합니다. 계속 실험해 보면, 여러분의 웹사이트는 언제나 이전보다 더 빠르게 로딩될 것입니다! ⚡️

# 간단한 영어로 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 작가를 박수로 응원하고 팔로우해 주세요 ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼도 방문해 보세요: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 만나보세요.