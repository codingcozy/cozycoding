---
title: "300줄의 코드로 Figma 플러그인을 만들기"
description: ""
coverImage: "/assets/img/2024-08-26-HowWeBuiltaFigmaPlugininJust300LinesofCodeStep-by-StepGuide_0.png"
date: 2024-08-26 17:11
ogImage: 
  url: /assets/img/2024-08-26-HowWeBuiltaFigmaPlugininJust300LinesofCodeStep-by-StepGuide_0.png
tag: Tech
originalTitle: "How We Built a Figma Plugin in Just 300 Lines of Code (Step-by-Step Guide)"
link: "https://medium.com/@techwithgeeky/how-we-built-a-figma-plugin-in-just-300-lines-of-code-step-by-step-guide-043cc2fdd3fa"
isUpdated: true
updatedAt: 1724743525904
---


![이미지](/assets/img/2024-08-26-HowWeBuiltaFigmaPlugininJust300LinesofCodeStep-by-StepGuide_0.png)

피그마 플러그인은 피그마 기능에 추가 기능을 제공하는 프로그램 또는 애플리케이션의 모음입니다. 피그마 플러그인은 대부분 개발자 커뮤니티에 의해 만들어집니다. 제품 디자이너의 전반적인 경험을 개선하고 시간 관리를 향상시키는 데 도움이 됩니다. 피그마 보드와 완전히 상호 작용할 수 있으며 필요에 따라 수정할 수도 있습니다.

더 많은 정보를 찾으려면, 피그마 플러그인 문서를 확인해보세요.

# 왜 이 피그마 플러그인이 개발되었나요?

몇 달 전, 제 친구인 프로페셔널 제품 디자이너인 Sam이 피그마 보드에서 레이어와 텍스트를 검색하는 방법이 필요했습니다. 그리고 개발자이자 친구인 저에게 한 번 연락해 이를 만들어달라고 했어요 :).

<div class="content-ad"></div>

이 순간까지 Figma 플러그인을 개발하는 방법에 대해 전혀 알지 못했기 때문에 아래의 미미가 있었습니다.

![Figma Plugin 개발](/assets/img/2024-08-26-HowWeBuiltaFigmaPlugininJust300LinesofCodeStep-by-StepGuide_1.png)

# 마주한 도전 과제

- Figma 플러그인을 개발하는 데 사용되는 프로그래밍 스택은 무엇인가요?
- Figma 플러그인에 어떤 이름을 지어야 할까요?
- Figma 플러그인을 최종적으로 어떻게 발행해야 할까요?

<div class="content-ad"></div>

피그마 플러그인은 HTML과 JavaScript만을 사용하여 개발됩니다. TypeScript(권장)를 사용하는 옵션이 있으며 HTML 요소를 CSS로 스타일링하여 더 나은 및 직관적인 사용자 인터페이스를 만들 수 있습니다.

먼저 컴퓨터에 Node.js가 설치되어 있어야 합니다. 명령줄 또는 터미널을 통해 컴퓨터의 Node.js 버전을 표시하여 설치를 테스트할 수 있습니다.

```js
node --version
```

컴퓨터에 TypeScript 설치하기
모든 곳에서 TypeScript를 실행할 수 있도록 TypeScript를 전역으로 설치하세요. 좋아하는 코드 편집기나 터미널에서 실행할 수 있어요.

<div class="content-ad"></div>

```js
npm install -g typescript
```

피그마 데스크톱 애플리케이션을 다운로드하세요
플러그인을 테스트하려면 데스크톱 애플리케이션을 다운로드해야 합니다. 피그마는 코드베이스를 파싱하고 지역에서 실행하여 피그마 보드에서 테스트할 수 있도록 도와줍니다.
피그마 데스크톱 애플리케이션을 웹사이트에서 다운로드하세요.

# 새로운 플러그인 만들기

데스크톱 애플리케이션에 로그인하고 파일 편집기를 열어 새 문서를 만들거나 기존 문서를 엽니다.


<div class="content-ad"></div>

다음에는 메뉴로 이동한 다음, 아래에 표시된 것처럼 플러그인 `개발` 새 플러그인으로 이동하십시오:

![Step 1](/assets/img/2024-08-26-HowWeBuiltaFigmaPlugininJust300LinesofCodeStep-by-StepGuide_2.png)

"플러그인 생성"이라는 모달이 열립니다. 이름을 지정한 다음 "Figma 디자인"을 선택하고 "다음"을 클릭하십시오. 다음 화면에서 "UI 및 브라우저 API 사용"을 선택한 다음 "다른 이름으로 저장"을 클릭하면 디스크의 원하는 위치에 플러그인을 저장하라는 프롬프트가 나타납니다.

![Step 2](/assets/img/2024-08-26-HowWeBuiltaFigmaPlugininJust300LinesofCodeStep-by-StepGuide_3.png)

<div class="content-ad"></div>


<img src="/assets/img/2024-08-26-HowWeBuiltaFigmaPlugininJust300LinesofCodeStep-by-StepGuide_4.png" />

# 프로젝트 설정 및 샘플 플러그인 실행

Visual Studio Code(추천)을 사용하여 만든 폴더를 엽니 다. 원하는 코드 편집기를 사용할 수도 있습니다.

NPM을 사용하여 Figma 플러그인 타입 패키지를 설치하세요.


<div class="content-ad"></div>

```js
npm install --save-dev @figma/plugin-typings
```

이제 터미널을 사용하여 TypeScript 컴파일을 시작하세요.

npm run build -- --watch 명령어를 실행하세요.

플러그인을 테스트하려면 Figma 데스크톱 앱으로 전환한 다음 메뉴로 이동하여 플러그인 "개발" 메뉴에 있는 "플러그인 이름"으로 이동하세요.

<div class="content-ad"></div>

한 번 플러그인을 실행하면 샘플 플러그인이 표시되는 팝업 모달이 나타날 것입니다. 그리고 이와 상호 작용할 수 있습니다.


<img src="/assets/img/2024-08-26-HowWeBuiltaFigmaPlugininJust300LinesofCodeStep-by-StepGuide_5.png" />

<img src="/assets/img/2024-08-26-HowWeBuiltaFigmaPlugininJust300LinesofCodeStep-by-StepGuide_6.png" />

# Let’s Build FigFinder


<div class="content-ad"></div>

네, 플러그인 이름을 FigFinder로 지었습니다. 이 글에서는 FigFinder가 어떻게 개발되었는지 설명하겠습니다.

FigFinder는 Figma 보드에서 레이어나 텍스트를 검색하는 데 도움이 되며, 검색 쿼리와 레이어 유형을 양식을 통해 받습니다. 그리고 양식 데이터는 다시 Figma API로 전송되어 검색 쿼리와 일치하는 컴포넌트 목록을 반환합니다. 이 목록은 결과 페이지에 표시됩니다. 목록의 각 항목을 클릭하면 Figma API로 이벤트가 전송되어 해당 컴포넌트가 Figma 보드 어디에 있는지 찾아 이동합니다.

이 플러그인을 사용하면 Figma 보드에 수천 개의 컴포넌트가 있더라도 디자이너들이 이름으로 쉽게 컴포넌트를 검색할 수 있습니다.

# 플러그인 인터페이스 구축

<div class="content-ad"></div>

ui.html 파일을 열고 내용을 지운 후 새로운 div와 script 요소를 만드세요. div 요소에는 플러그인 인터페이스를 넣고 script 요소에는 비즈니스 로직을 넣으세요. 

div 요소 안에 두 개의 새로운 div 요소를 만들고 각각 "search-page"와 "result-page" 라는 id를 부여하세요.

```js
<div>
  <div id="search-page" style="padding: 24px;">
  </div>
  <div id="result-page" class="hide" style="padding: 0px 24px">
  </div>
</div>
```

"id"가 "result-page"인 div에 "hide" 클래스를 부여하세요. 이 클래스는 검색 페이지와 결과 페이지의 가시성을 변경하는 데 사용될 것입니다.

이제 아래 코드를 "search-page" div요소 내에 붙여 넣으세요.

<div class="content-ad"></div>

```js
<div style="margin-bottom: 24px;">
      <label for="search">키워드</label>
      <div class="input-icons">
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg" class="icon-1">
          <path
            d="M7.66659 14.0002C11.1644 14.0002 13.9999 11.1646 13.9999 7.66683C13.9999 4.16903 11.1644 1.3335 7.66659 1.3335C4.16878 1.3335 1.33325 4.16903 1.33325 7.66683C1.33325 11.1646 4.16878 14.0002 7.66659 14.0002Z"
            stroke="#999999" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" />
          <path d="M14.6666 14.6668L13.3333 13.3335" stroke="#999999" stroke-width="1.5" stroke-linecap="round"
            stroke-linejoin="round" />
        </svg>
        <input id="search" type="text" class="text-field" placeholder="검색" />
      </div>
</div>

<div style="margin-bottom: 16px;">
      <label for="layer-type">레이어 유형</label>
      <select class="select-field" id="layer-type">
        <option value="text">텍스트</option>
        <option value="frame">프레임</option>
      </select>
</div>
<div class="info-box">
      <svg width="36" height="36" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path
          d="M7.99992 14.6668C11.6666 14.6668 14.6666 11.6668 14.6666 8.00016C14.6666 4.3335 11.6666 1.3335 7.99992 1.3335C4.33325 1.3335 1.33325 4.3335 1.33325 8.00016C1.33325 11.6668 4.33325 14.6668 7.99992 14.6668Z"
          stroke="#474747" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" />
        <path d="M8 5.3335V8.66683" stroke="#474747" stroke-width="1.5" stroke-linecap="round"
          stroke-linejoin="round" />
        <path d="M7.99634 10.6665H8.00233" stroke="#292D32" stroke-width="2" stroke-linecap="round"
          stroke-linejoin="round" />
      </svg>
      <p>키워드 구문을 "키워드" 입력란에 입력하고 레이어 유형을 선택하세요. 검색 버튼을 클릭하여 플러그인을 실행하세요.👌</p>
</div>
<button type="button" class="search-btn" id="search-btn">검색</button>
```

<div class="content-ad"></div>

```js
<div class="back-button" id="back-button">
      <svg width="20" height="20" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M6.38016 3.95312L2.3335 7.99979L6.38016 12.0465" stroke="#7D7D7D" stroke-width="1.5"
          stroke-miterlimit="10" stroke-linecap="round" stroke-linejoin="round" />
        <path d="M13.6668 8H2.44678" stroke="#7D7D7D" stroke-width="1.5" stroke-miterlimit="10" stroke-linecap="round"
          stroke-linejoin="round" />
      </svg>
      <p style="margin: 0;">검색으로 돌아가기</p>
    </div>
    <div id="empty-result" style="padding: 48px 24px; text-align:center;" class="hide">
      <svg width="110" height="102" viewBox="0 0 110 102" fill="none" xmlns="http://www.w3.org/2000/svg">
        <circle cx="49.6812" cy="49.6812" r="49.6812" fill="#F1F1F1" />
        <circle cx="49.275" cy="49.7657" r="28.7081" fill="#F1F1F1" stroke="#C4C4C4" stroke-width="2" />
        <circle cx="49.2748" cy="49.766" r="20.2201" fill="#FFFEFF" stroke="#C4C4C4" stroke-width="2" />
        <path
          d="M49.2749 34.1653C49.2749 33.8537 49.0221 33.6 48.7106 33.6109C44.8118 33.747 41.0862 35.2895 38.2287 37.964C35.3712 40.6385 33.5858 44.254 33.1923 48.1353C33.1609 48.4453 33.3973 48.7143 33.7082 48.7349C34.0191 48.7555 34.2868 48.5199 34.319 48.21C34.693 44.6145 36.3514 41.2667 38.9998 38.7879C41.6482 36.3091 45.0983 34.8756 48.7107 34.7401C49.0221 34.7284 49.2749 34.4769 49.2749 34.1653Z"
          fill="#C4C4C4" />
        <rect x="71.4574" y="81.2695" width="10.3062" height="29.6873" rx="5.15309"
          transform="rotate(-45 71.4574 81.2695)" fill="#F1F1F1" stroke="#C4C4C4" stroke-width="2" />
        <rect x="82.1211" y="83.6025" width="1.34862" height="11.3078" rx="0.674309"
          transform="rotate(-45 82.1211 83.6025)" fill="white" />
        <rect x="70.9087" y="74.4121" width="1.38436" height="5.53746" transform="rotate(-45 70.9087 74.4121)"
          fill="#C4C4C4" />
        <rect x="85.5972" y="46.2798" width="13.8436" height="1.38436" rx="0.692182" fill="#C4C4C4" />
        <rect y="57.7319" width="13.8436" height="1.38436" rx="0.692182" fill="#C4C4C4" />
        <rect x="102.209" y="46.2798" width="6.92182" height="1.38436" rx="0.692182" fill="#C4C4C4" />
        <rect x="89.0581" y="51.3589" width="6.92182" height="1.38436" rx="0.692182" fill="#C4C4C4" />
        <rect x="3.46094" y="62.811" width="6.92182" height="1.38436" rx="0.692182" fill="#C4C4C4" />
      </svg>
      <p style="color: #7D7D7D; margin:16px;">
        "search"에 맞는 결과를 찾을 수 없습니다.
      </p>
    </div>
    <div id="result" class="hide"></div>
```

<div class="content-ad"></div>

우리는 검색 페이지, 버튼 및 결과 페이지 요소들을 스타일링했어요.

스타일링은 Figma 플러그인에 전체적으로 화려한 룩을 주었어요! 🔥🔥

Figma 플러그인을 실행하고 어떻게 보이는지 확인해보세요. 아래의 이미지처럼 보이게 될 거에요.

![Figma Plugin](/assets/img/2024-08-26-HowWeBuiltaFigmaPlugininJust300LinesofCodeStep-by-StepGuide_7.png)

<div class="content-ad"></div>

# 플러그인 기능 작성하기

다음으로, code.ts 파일을 열어주세요. 이 파일에 Figma 플러그인을 위한 모든 비즈니스 로직을 작성할 것입니다.

Figma API를 사용하여 요소가 어떻게 표시될지 구조화된 인터페이스를 만듭니다. `figma.showUI(__html__, { title: 'FigFinder', height: 460, width: 490 });` 그리고 나서 ui.html에서 메시지를 받는 리스너를 생성하고 받은 메시지에 기반한 일련의 명령을 실행합니다.

```js
figma.showUI(__html__, { title: 'FigFinder', height: 460, width: 490 });
figma.ui.onmessage = msg => {
};
```

<div class="content-ad"></div>

onmessage 리스너 내부에서, 검색 쿼리를 처리하는 조건문과 Figma 보드에서 선택한 요소를 검색하는 두 가지 조건문을 생성합니다.

```js
if(msg.type === 'process-search') {
    const { searchQuery, layerType } = msg.query;
    if (layerType == 'all') {
        console.log(searchQuery)
        const node = figma.currentPage.findAll(node =>
            node.name.toLowerCase().includes(searchQuery.toLowerCase()));
        console.log(node.map(n => n.name))
        const result = node.map(n => n.name);
        figma.ui.postMessage({ count: result.length, type: 'all', result })
    } else if (layerType == 'text') {
        console.log(searchQuery)
        const node = figma.currentPage.findAll(node => node.type === "TEXT"
            && node.characters.toLowerCase().includes(searchQuery.toLowerCase())) as TextNode[];
        console.log(node.map(n => n.characters))
        const result = node.map(n => n.characters);
        figma.ui.postMessage({ count: result.length, type: 'text', result })
    } else if (layerType == 'frame') {
        console.log(searchQuery)
        const node = figma.currentPage.findAll(node => node.type === "FRAME"
            && node.name.toLowerCase().includes(searchQuery.toLowerCase())) as TextNode[];
        console.log(node.map(n => n.name))
        const result = node.map(n => n.name);
        figma.ui.postMessage({ count: result.length, type: 'frame', result })
    } else {
        figma.ui.postMessage({ count: 0, type: layerType, result: [] })
    }
} else if (msg.type === 'search-item') {
    const { searchQuery, layerType } = msg.query;
    let nodesFound ;
    if (layerType == 'text') {
        nodesFound = figma.currentPage.findChildren((node) => node.type === 'TEXT' && node.characters.toLowerCase().includes(searchQuery.toLowerCase()));
    }
    if (layerType == 'frame') {
        nodesFound = figma.currentPage.findChildren((node) => node.type === 'FRAME' && node.name.toLowerCase().includes(searchQuery.toLowerCase()));
    }
    if (nodesFound) {
        figma.currentPage.selection = nodesFound;
        figma.viewport.scrollAndZoomIntoView(nodesFound);
    }
} else {
    figma.closePlugin('Command not available');
}
```

현재 Figma 보드와 상호작용하는데 figma.currentPage를 사용하고 figma.postMessage를 사용하여 ui.html로 이벤트를 전송했습니다.

# 모든 것을 함께 작동하게 만들기

<div class="content-ad"></div>

다음으로, ui.html 내부의 스크립트 엘리먼트를 업데이트하여 code.ts에서 오는 메시지를 수신하는 리스너를 추가합니다. 이 리스너 내에서 수신한 메시지를 처리하고 그 정보를 기반으로 요소 목록을 표시합니다.

```js
var layer_type = '';
onmessage = (event) => {
    const pluginMessageResult = event.data.pluginMessage;
    if (pluginMessageResult.count && pluginMessageResult.count > 0) {
      const resultDiv = document.getElementById('result');
      resultDiv.classList.remove('hide');
      layer_type = pluginMessageResult.type;
      resultDiv.innerHTML = `<p>검색 결과: ${pluginMessageResult.count} 개</p>`
      for (let index = 0; index < pluginMessageResult.count; index++) {
        const element = pluginMessageResult.result[index];
        const parentDivNode = document.createElement("div");

        const node = document.createElement("p");
        const textnode = document.createTextNode(element);
        node.appendChild(textnode);
        parentDivNode.setAttribute('content', element)
        parentDivNode.onclick = handleClick;
        parentDivNode.appendChild(node)
        resultDiv.appendChild(parentDivNode)
        node.insertAdjacentHTML("beforebegin", getIcon());
      }
    } else {
      document.getElementById('empty-result').classList.remove('hide');
    }
    console.log("플러그인 코드로부터 이를 받음", event.data.pluginMessage)
  }
  const textSvg = `<svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
<path d="M1.78003 4.77986V3.56652C1.78003 2.79986 2.40003 2.18652 3.16003 2.18652H12.84C13.6067 2.18652 14.22 2.80652 14.22 3.56652V4.77986" stroke="#292D32" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
<path d="M8 13.8136V2.74023" stroke="#292D32" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
<path d="M5.37329 13.8135H10.6266" stroke="#292D32" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
</svg>
`;
  const frameSvg = `<svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
<line x1="4.5" y1="1.3335" x2="4.5" y2="14.6668" stroke="black"/>
<line x1="14.6667" y1="5.1665" x2="1.33341" y2="5.1665" stroke="black"/>
<line x1="14.6667" y1="11.1665" x2="1.33341" y2="11.1665" stroke="black"/>
<line x1="11.8333" y1="1.3335" x2="11.8333" y2="14.6668" stroke="black"/>
</svg>
`;

  function getIcon() {
    if (layer_type == 'text') {
      return textSvg;
    }
    return frameSvg;
  }

  function handleClick() {
    const query = { searchQuery: this.getAttribute('content'), layerType: layer_type };
    parent.postMessage({ pluginMessage: { type: 'search-item', query } }, '*')
    const resultDiv = document.getElementById('result');
    const divElement = resultDiv.querySelector('div.active');
    if (divElement) {
      divElement.classList.remove('active');
    }
    this.classList.add('active')
  }
```

handleClick 함수는 Figma 보드에서 선택한 요소를 검색할 수 있도록 코드.ts에 이벤트를 보냅니다.

다음으로, 사용자 입력에 따라 검색 쿼리와 일치하는 요소를 가져오기 위해 코드.ts에 이벤트를 보내는 검색 로직을 처리합니다.

<div class="content-ad"></div>

버튼에 ID "search-btn"을 클릭 이벤트를 추가하고 parent.postMessage를 사용하여 form 데이터를 code.ts로 보냅니다. 이 데이터는 code.ts의 onmessage 리스너 내에서 처리될 것입니다.

```js
document.getElementById('search-btn').onclick = (event) => {
    const inputFieldText = document.getElementById('search').value;
    const selectFieldText = document.getElementById('layer-type').value;
    console.log(inputFieldText, selectFieldText);
    const query = { searchQuery: inputFieldText, layerType: selectFieldText };
    parent.postMessage({ pluginMessage: { type: 'process-search', query } }, '*');
    document.getElementById('search-page').classList.add('hide');
    document.getElementById('result-page').classList.remove('hide');
  }
```

마지막으로, 사용자가 검색 페이지로 돌아갈 때 결과 페이지를 닫는 논리를 추가합니다.

```js
document.getElementById('back-button').onclick = () => {
    document.getElementById('search-page').classList.remove('hide');
    document.getElementById('result').innerHTML = '';
    document.getElementById('result-page').classList.add('hide');
    document.getElementById('empty-result').classList.add('hide');
  }
```

<div class="content-ad"></div>

# 피그마 플러그인 테스트

피그마 플러그인을 실행하고 상호 작용하세요. 입력 필드에 프레임이나 텍스트를 검색하려고 입력할 수 있습니다. 마지막으로 검색 버튼을 클릭하세요. 다음 페이지에 컴포넌트 목록이 표시됩니다. 결과 항목을 클릭하면 해당 이벤트가 피그마 API로 전송되어 피그마 보드의 특정 피그마 컴포넌트로 이동합니다.

![이미지](https://miro.medium.com/v2/resize:fit:852/0*5TfePzNy-OAkMVG-.gif)

피그마 플러그인은 ui.html과 code.ts 파일을 결합할 때 약 300줄 이상의 코드로 작성되었습니다.

<div class="content-ad"></div>

# 이 프로젝트에서 배운 것

- Figma 플러그인을 개발하기 위한 자료가 거의 없었기 때문에, 문서와 Figma 커뮤니티 외에는 한정적인 자원을 활용하며 비판적 사고력을 발휘하게 되었습니다.
- 순수 자바스크립트의 지식을 되짚어보게 되었습니다.
- 앞으로 더 복잡한 Figma 플러그인을 개발하기 위한 튼튼한 기반을 다졌습니다.

# Figma 플러그인 개발의 현재 제한 사항

Figma 플러그인 개발은 흥미로운 일이지만, 재미를 좀 덜어주는 몇 가지 제한 사항이 있습니다. 이에 대해 아래에서 논의해보겠습니다:

<div class="content-ad"></div>

- 외부 자산을 사용하는 것이 작동하지 않는 것으로 보입니다. 현재 이 문서를 작성하는 시점에는 외부 CSS, 이미지 및 JavaScript가 허용되지 않습니다.
- Figma 플러그인을 구축하기 위해 JavaScript 프레임워크를 사용하는 것은 현재 불가능하기 때문에 순수 JavaScript를 작성할 준비가 되어 있어야 합니다. jQuery와 같은 라이브러리를 가져오기 위해 CDN을 사용할 수는 있지만, 제가 직접 테스트한 적은 없습니다.
- 마지막으로, 프로젝트 구조에 국한되어 선호하는 아키텍처를 탐구하거나 설정할 수 없습니다.

만약 Figma 플러그인을 구축하고 배포한다면, Figma 창작자 커뮤니티에서 해당 플러그인을 배포하고 사용하는 단계를 설명하는 공식 웹사이트를 참조해 주세요.

# 결론

본 문서에서는 Figma 플러그인과 그 사용법에 대해 설명했습니다. 또한 FigFinder(피그마 보드에서 구성 요소를 찾는 Figma 플러그인)를 구축한 방법도 설명했습니다. 여러분이 어떤 것을 개발할지 기대됩니다. 또한 Twitter에서 저에게 연락할 수도 있습니다.
이 Figma 플러그인의 전체 코드베이스는 GitHub에서 사용할 수 있습니다. 해당 저장소에 GitHub 스타를 부탁드립니다.

<div class="content-ad"></div>

도움이 되었다면 반응을 남겨주세요 ❤️