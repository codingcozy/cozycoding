---
title: "브라우저에서 디버깅하는 15가지 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 19:19
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "15 Powerful Browser Debugging Techniques"
link: "https://dev.to/nilebits/15-powerful-browser-debugging-techniques-c3n"
isUpdated: true
updatedAt: 1724245449272
---


브라우저 디버깅 기술은 모든 웹 개발자에게 필수적인 능력입니다. 올바른 도구와 절차를 사용하면 개발 프로세스를 크게 간소화하고 괴로움을 피할 수 있습니다. 현대 브라우저에는 여러 디버깅 도구가 내장되어 있어 온라인 앱의 문제를 식별하고 해결하는 데 도움이 됩니다. 이 철저한 튜토리얼에서는 각 브라우저가 제공해야 할 15가지 효과적인 디버깅 방법을 설명하고 코드 예제를 통해 사용 방법을 보여줍니다.

브라우저 디버깅 기술 목록

- 요소 검사하기

요소 검사 도구는 브라우저 디버깅의 중요한 요소입니다. HTML 및 CSS를 실시간으로 확인하고 편집할 수 있습니다.

<div class="content-ad"></div>

사용 방법

웹 페이지에서 어떤 요소든 마우스 오른쪽 버튼을 클릭합니다.

컨텍스트 메뉴에서 "검사" 또는 "요소 검사"를 선택합니다.

개발자 도구 패널이 열려서 HTML 구조와 관련된 CSS 스타일이 표시됩니다.

<div class="content-ad"></div>

예시

버튼의 색상을 동적으로 변경하고 싶다면 다음과 같이 하세요.

```js
<button id="myButton" style="color: blue;">Click Me!</button>
```

버튼을 마우스 오른쪽 버튼으로 클릭하고 "검사"를 선택하세요.

<div class="content-ad"></div>

Styles 패널에서 color: blue; 를 color: red; 로 변경해주세요.

버튼 색상은 즉시 업데이트됩니다.

- 콘솔 로깅

콘솔은 정보, 오류 및 경고를 로깅하는 데 가장 유용한 도구입니다.

<div class="content-ad"></div>

사용 방법

개발자 도구를 열어주세요 (보통 F12을 누르거나 마우스 우클릭 후 "검사"를 선택합니다).

"콘솔" 탭으로 이동해주세요.

JavaScript 코드에서 console.log(), console.error(), console.warn()을 사용해주세요.

<div class="content-ad"></div>

예시

```js
console.log("이것은 로그 메시지입니다.");
console.error("이것은 오류 메시지입니다.");
console.warn("이것은 경고 메시지입니다.");
```

- 중단점

중단점을 설정하여 코드 실행을 일시 중지시킬 수 있습니다. 변수와 호출 스택을 검사할 수 있습니다.

<div class="content-ad"></div>

사용 방법

개발자 도구를 엽니다.

"소스" 탭으로 이동합니다.

중단점을 설정하고 싶은 줄 번호를 클릭합니다.

<div class="content-ad"></div>

예제

```js
function calculateSum(a, b) {
    let sum = a + b;
    console.log(sum);
    return sum;
}

calculateSum(5, 3);
```

let sum = a + b; 에 브레이크포인트를 설정해주세요. 

해당 함수를 실행해주세요.

<div class="content-ad"></div>

실행이 일시 중단되어 변수를 검사할 수 있습니다.

- 네트워크 패널

네트워크 패널을 사용하면 상태 코드, 헤더 및 페이로드를 포함한 네트워크 요청과 응답을 모니터링할 수 있습니다.

사용 방법

<div class="content-ad"></div>

개발자 도구를 열어주세요.

"네트워크" 탭으로 이동해주세요.

네트워크 활동을 확인하려면 페이지를 새로고침해주세요.

예시

<div class="content-ad"></div>

```js
fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => console.log(data));
```

네트워크 패널을 열어주세요.

fetch 요청을 실행해주세요.

요청 및 응답 세부 정보를 검사해주세요.

<div class="content-ad"></div>

- 소스 맵

소스 맵은 축소된 코드를 원본 소스 코드로 연결하여 디버깅을 쉽게 만듭니다.

사용 방법

빌드 도구가 소스 맵을 생성하도록 확인하세요 (예: Webpack을 사용).

<div class="content-ad"></div>

개발자 도구를 열어주세요.

원본 소스 코드를 보려면 "소스" 탭으로 이동하세요.

예시 (Webpack 구성)

```js
module.exports = {
    mode: 'development',
    devtool: 'source-map',
    // 다른 구성
};
```

<div class="content-ad"></div>

- 로컬 오버라이드

로컬 오버라이드를 사용하면 네트워크 자원에 변경 사항을 즉시 볼 수 있게 하고 소스 파일을 수정하지 않고도 결과를 확인할 수 있습니다.

사용 방법

개발자 도구를 엽니다.

<div class="content-ad"></div>

"Sources" 탭으로 이동해 주세요.

파일을 우클릭하고 "재정의로 저장"을 선택하세요.

예시

CSS 파일을 재정의하여 div의 배경색을 변경하세요.

<div class="content-ad"></div>

```js
<div id="myDiv" style="background-color: yellow;">Hello World!</div>
```

파일을 덮어쓰기하고 background-color: white;를 background-color: yellow;로 변경하세요.

- 성능 패널

성능 패널은 런타임 성능을 분석하는 데 도움이 되며 JavaScript 실행, 레이아웃 렌더링 등을 포함합니다.

<div class="content-ad"></div>

사용 방법

개발자 도구를 엽니다.

"성능" 탭으로 이동합니다.

성능 데이터를 캡처하기 시작하려면 "녹화"를 클릭합니다.

<div class="content-ad"></div>

예시

함수 실행 결과를 기록하세요.

```js
function performHeavyTask() {
    for (let i = 0; i < 1000000; i++) {
        // 번거로운 작업 시뮬레이션
    }
    console.log("작업 완료");
}

performHeavyTask();
```

기록된 데이터를 분석하여 병목 현상을 식별하세요.

<div class="content-ad"></div>

- 메모리 패널

메모리 패널은 메모리 누수를 감지하고 메모리 사용량을 분석하는 데 도움을 줍니다.

사용 방법

개발자 도구를 열어주세요.

<div class="content-ad"></div>

"Memory" 탭으로 이동해주세요.

메모리 사용량을 분석하기 위해 힙 스냅샷을 찍어보세요.

예시

객체를 생성하고 메모리 사용량을 모니터링하세요.

<div class="content-ad"></div>

```js
let arr = [];

function createObjects() {
    for (let i = 0; i < 100000; i++) {
        arr.push({ index: i });
    }
}

createObjects();
```

createObjects()를 실행하기 전후에 힙 스냅샷을 찍어서 메모리 사용량을 비교해보세요.

- 애플리케이션 패널

애플리케이션 패널은 로컬 저장소, 세션 저장소, 쿠키 등에 대한 정보를 제공합니다.

<div class="content-ad"></div>

사용 방법

개발자 도구를 열어주세요.

"Application" 탭으로 이동해주세요.

"Storage" 아래의 저장 옵션을 탐색해보세요.

<div class="content-ad"></div>

예시

로컬 스토리지에 데이터 저장하고 확인해보세요.

```js
localStorage.setItem('key', 'value');
console.log(localStorage.getItem('key'));
```

"Application" 패널에서 "로컬 스토리지" 섹션을 확인해보세요.

<div class="content-ad"></div>

- Lighthouse

라이트하우스는 웹 페이지의 품질을 향상시키기 위한 오픈 소스 도구입니다. 성능, 접근성, SEO 등에 대한 감사를 제공합니다.

사용 방법

개발자 도구를 열어주세요.

<div class="content-ad"></div>

"Lighthouse" 탭으로 이동해주세요.

"보고서 생성"을 클릭해주세요.

예시

샘플 웹페이지에 Lighthouse 감사를 실행하고 개선 제안 사항을 확인해주세요.

<div class="content-ad"></div>

- 모바일 장치 에뮬레이션

모바일 장치 에뮬레이션을 사용하면 웹 응용 프로그램이 다양한 장치에서 어떻게 작동하는지 테스트할 수 있습니다.

사용 방법

개발자 도구를 엽니다.

<div class="content-ad"></div>

디바이스 툴바 버튼(핸드폰 아이콘)을 클릭하여 디바이스 모드를 전환하세요.

드롭다운에서 디바이스를 선택하세요.

예시

모바일 장치를 모방하여 반응형 레이아웃이 어떻게 적응되는지 확인하세요.

<div class="content-ad"></div>

```js
<div class="responsive-layout">반응형 콘텐츠</div>
```

- CSS 그리드 및 플렉스박스 디버깅

최신 브라우저는 CSS 그리드 및 플렉스박스 레이아웃을 시각화하고 디버깅할 수 있는 도구를 제공합니다.

사용 방법

<div class="content-ad"></div>

개발자 도구를 엽니다.

"요소" 탭으로 이동합니다.

레이아웃을 시각화하기 위해 "Grid" 또는 "Flexbox" 아이콘을 클릭합니다.

예시

<div class="content-ad"></div>

```js
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
}
.item {
    background-color: lightblue;
    padding: 20px;
}
```

`<div class="container">
    <div class="item">Item 1</div>
    <div class="item">Item 2</div>
    <div class="item">Item 3</div>
</div>`

<div class="content-ad"></div>

접근성 검사 도구를 사용하면 웹 애플리케이션에서 발생하는 접근성 문제를 식별하고 해결할 수 있어요.

사용 방법

1. 개발자 도구를 엽니다.
2. "Elements" 탭 아래의 "Accessibility" 패널로 이동합니다.

<div class="content-ad"></div>

접근성 위반 여부를 확인하세요.

예시

버튼 요소의 접근성을 확인하세요.

```js
<button id="myButton">Click Me!</button>
```

<div class="content-ad"></div>

"접근성 창은 통찰과 제안을 제공합니다.

- JavaScript 프로파일러

JavaScript 프로파일러는 런타임 성능 데이터를 수집하여 JavaScript 코드의 성능을 분석하는 데 도움을 줍니다.

사용 방법"

<div class="content-ad"></div>

개발자 도구를 열어주세요.

"Profiler" 탭으로 이동해주세요.

프로파일링을 시작하려면 "시작"을 클릭해주세요.

예시

<div class="content-ad"></div>

함수 실행을 프로파일링하여 성능 병목 현상을 찾아봅시다.

```js
function complexCalculation() {
    for (let i = 0; i < 1000000; i++) {
        // 복잡한 계산을 모의로 수행
    }
    console.log("계산이 완료되었습니다");
}

complexCalculation();
```

프로파일링 결과를 분석하여 함수를 최적화하세요.

- 비동기 코드 디버깅

<div class="content-ad"></div>

비동기 코드를 디버깅하는 것은 어렵지만, 현대적인 브라우저는 효과적으로 처리할 수 있는 도구를 제공합니다.

사용 방법

1. 개발자 도구를 엽니다.
2. "소스" 탭의 "async" 체크박스를 사용하여 비동기 코드에 중단점을 설정합니다.

<div class="content-ad"></div>

"호출 스택(Call Stack)"패널을 사용하여 비동기 호출을 추적하세요.

예시

비동기 fetch 요청을 디버깅하세요.

```js
async function fetchData() {
    try {
        let response = await fetch('https://api.example.com/data');
        let data = await response.json();
        console.log(data);
    } catch (error) {
        console.error("데이터를 가져오는 동안 오류 발생:", error);
    }
}

fetchData();
```

<div class="content-ad"></div>

fetchData 함수 내부에 중단점을 설정하고 비동기 실행을 추적해보세요.

결론

이 15가지 강력한 디버깅 기술을 숙달하면 웹 개발자로서 생산성과 효율성을 크게 향상시킬 수 있습니다. Inspect Element 및 Console Logging과 같은 기본 도구부터 JavaScript Profiler 및 비동기 디버깅과 같은 고급 기능까지, 각 기술은 독특한 통찰력과 기능을 제공하여 더 나은 웹 애플리케이션을 구축하는 데 도움을 줍니다.

<div class="content-ad"></div>

이 브라우저 디버깅 기술을 활용하여 어떤 어려움이든 대처할 준비가 되어 웹 응용 프로그램이 견고하고 효율적이며 사용자 친화적임을 보장할 수 있습니다. 행운을 빕니다!