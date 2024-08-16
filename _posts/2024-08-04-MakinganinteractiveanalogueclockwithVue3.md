---
title: "Vue3로 인터랙티브 아날로그 시계 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_0.png"
date: 2024-08-04 20:03
ogImage: 
  url: /assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_0.png
tag: Tech
originalTitle: "Making an interactive analogue clock with Vue3"
link: "https://medium.com/@nikolai.davydov96/making-a-fully-interactive-clock-with-vue3-d7ab1a492a94"
---



![2024-08-04-MakinganinteractiveanalogueclockwithVue3_0](/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_0.png)

지금까지 많은 사람들로부터 Vue.js를 사용하여 멋진 대화형 위젯을 손쉽게 만드는 방법에 대한 자습서를 요청받아왔습니다. 그것은 실제로 제 열정이었습니다. 구성 API 덕분에 Vue를 사용하여 아름다운 것들을 매우 간단하게 만들 수 있으며, 창의적인 비전은 어떠한 제약도 받지 않습니다. 오늘은 핵심 라이브러리와 기본 HTML, JavaScript, CSS를 사용하여 플레인 Vue 3로 모든 것을 사용하여 완전히 대화형 시계를 만드는 방법을 가르쳐 드릴 것입니다.

# 프로젝트 설정

먼저, 프로젝트를 생성하고 (모든 선택지에서 엔터키를 눌러 아무것도 선택하지 않고) 시작해봅시다:


<div class="content-ad"></div>

```js
$ npm create vue@latest --name vue-masterclass-clock
$ cd vue-masterclass-clock
$ npm install
$ npm run dev
```

이제 기본 프로젝트가 생성되었습니다. 자, 이제 위젯에 집중할 수 있도록 정리해보겠습니다. 먼저 src 폴더 안에 모든 불필요한 파일을 제거하여 파일 시스템이 다음과 같이 보이도록 합시다:

![Files to remain in the src folder](/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_1.png)

reset.css는 기본 CSS 초기화 파일입니다.

<div class="content-ad"></div>

```js
/* http://meyerweb.com/eric/tools/css/reset/ 
   v2.0 | 20110126
   License: none (public domain)
*/

html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
 margin: 0;
 padding: 0;
 border: 0;
 font-size: 100%;
 font: inherit;
 vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section {
 display: block;
}
body {
 line-height: 1;
}
ol, ul {
 list-style: none;
}
blockquote, q {
 quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
 content: '';
 content: none;
}
table {
 border-collapse: collapse;
 border-spacing: 0;
}
```

main.css는 그 파일을 단순히 가져옵니다:

```js
@import './reset.css';
```

App.vue는 우리 구성 요소를 감싸는 래퍼일 뿐입니다:


<div class="content-ad"></div>

```js
<script setup>
import McClock from './components/clock/McClock.vue'
</script>

<template>
    <div class="app-wrapper">
        <McClock></McClock>
    </div>
</template>

<style>
.app-wrapper {
    max-width: 90vh;
}
</style>
```

main.js가 변경되지 않았습니다:

```js
import './assets/main.css'

import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

마지막으로, 우리의 간단한 컴포넌트 McClock.vue는 다음과 같습니다:

<div class="content-ad"></div>

```js
<template>
    <div>CLOCK</div>
</template>
```

그냥 아무것도 추가되지 않은 템플릿입니다. 따라서 초기 애플리케이션은 다음과 같습니다:

<img src="/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_2.png" />

# 기본 시계


<div class="content-ad"></div>

이제 프로젝트 설정을 살펴보았으니, 이제 시계 작업에 착수할 수 있습니다. 이런 프로젝트의 경우, 일반적으로 기본 기능부터 시작한 다음 점진적으로 개선하는 방식으로 작업합니다. 따라서 시간을 표시하는 3개의 시계 바늘이 있는 기본 "시계"를 만들어봅시다. 이를 위해 처음부터 해결해야 할 두 가지 문제가 있습니다:

- 현재 시간, 분 및 초 값을 반응 변수에 바인딩하는 방법.
- 숫자 값에 기반한 위치를 가진 시계 바늘.

## 먼저 첫 번째 문제부터 시작합시다:

우선, 초기 Date에 따라 시, 분 및 초를 계산하는 매우 간단한 로직을 만들겠습니다:

<div class="content-ad"></div>

```js
<script setup>
import { ref, computed } from "vue";

const currentDate = ref(new Date());
const hours = computed(() => currentDate.value.getHours());
const minutes = computed(() => currentDate.value.getMinutes());
const seconds = computed(() => currentDate.value.getSeconds());
</script>

<template>
    <div>CLOCK</div>
    <div>{ hours }</div>
    <div>{ minutes }</div>
    <div>{ seconds }</div>
</template>
```

위의 논리의 문제는 시간이 지남에 따라 업데이트되지 않는다는 점입니다. 따라서 주기적으로 업데이트하는 함수를 만들어야 합니다:

```js
import { ref, computed, onMounted, onBeforeUnmount } from "vue";

//...

function updateCurrentDate() {
    currentDate.value = new Date();
    getNextTick();
}
let timeout = null;
function getNextTick() {
    const milliseconds = (new Date()).getMilliseconds();
    timeout = setTimeout(updateCurrentDate, 1000 - milliseconds);
}
onMounted(() => {
    getNextTick();
});
onBeforeUnmount(() => {
    clearTimeout(timeout);
});
```

콘솔을 확인하면 함수가 초가 변경된 직후에 거의 확실히 호출된다는 것을 볼 수 있습니다. 이제 콘솔 로그를 제거하겠습니다. 이렇게 하면 함수가 초가 시작된 직후에 호출되며 시작한 지 최대 1밀리초가 지났음을 보여줍니다. 이미 지난 밀리초를 확인하고 남은 초만큼을 기다리기 때문에 1000에서 빼냅니다.


<div class="content-ad"></div>

## 그러면 제일 처음 문제를 해결하고 두 번째 문제로 넘어갈 수 있게 되었어요:

우리가 의도한 대로 시간을 표시해주는 시계 바늘 컴포넌트를 만들어야 해요.

먼저, 시계의 윤곽을 만들어봅시다:

```js
<script setup>

//...

const clockMargin = ref("24px");
</script>

<template>
    <div>CLOCK</div>
    <div>{ hours }</div>
    <div>{ minutes }</div>
    <div>{ seconds }</div>

    <div class="mc-clock-area">

    </div>
</template>

<style scoped>
.mc-clock-area {
    margin: v-bind(clockMargin);
    position: relative;
    width: calc(100% - v-bind(clockMargin) * 2);
    aspect-ratio: 1;
    background-color: green;
    border-radius: 50%;
    border-width: 16px;
    border-style: solid;
    border-color: blue;
    box-sizing: border-box;
}
</style>
```

<div class="content-ad"></div>

그렇게 하면 다음과 같이 보일 겁니다:

![크기조정한 아날로그 시계](/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_3.png)

이 시점에서 몇 가지가 눈에 띄는데요:

- clockMargin은 JavaScript 부분에서 설정되었습니다. 이것은 특히 멋지며 위젯에 여백을 속성으로 설정할 수 있게 해줍니다. 이를 통해 나중에 동일한 구성 요소를 사용하여 여러 유연한 시계를 만들 수 있게 됩니다.
- position은 상대적으로 설정되었습니다. 이후에 오는 시계 바늘에는 absolute 위치 지정을 사용할 것입니다.
- 구성 요소의 너비는 calc 함수를 사용합니다. calc는 당신의 친구입니다! calc를 두려워하지 마세요. CSS의 가장 놀라운 부분 중 하나이며, calc를 사용하면 다른 방식으로는 어렵게 처리해야 하는 작업을 쉽게 할 수 있습니다.
- 가로세로비는 CSS의 숨겨진 보석 중 하나입니다. 이를 사용하면 구성 요소가 정적인 경우 높이를 걱정할 필요가 없습니다.
- 테두리 속성은 잘 알려져 있어 설명할 필요가 없겠지만, box-sizing을 잊지 마세요! box-sizing을 사용하면 테두리가 이전에 설정한 크기에 딱 맞게 들어맞게 되고 그 반대로 되지 않습니다. 이 속성을 발견하기 전에 테두리와 크기 계산에 얼마나 귀찮은 일을 했는지 말씀드리기 어렵습니다.

<div class="content-ad"></div>

이제 시계 바늘을 만들어보겠습니다!

우리는 McClock.vue 옆에 McClockHand.vue라는 새 파일을 만들 것입니다.

```js
<script setup>
const props = defineProps({
    position: Number,
    positionMax: Number
})
</script>

<template>
    <div class="mc-clock-hand"></div>
</template>

<style scoped>
.mc-clock-hand {
    position: absolute;
    width: 10%;
    height: 50%;
    background-color: red;
    top: 50%;
    left: 50%;
    transform-origin: top center;
    transform: translate(-50%, 0%) rotate(180deg);
    border-radius: 12px;
    border-width: 4px;
    border-style: solid;
    border-color: black;
    box-sizing: border-box;
}
</style>
```

또한, 이를 메인 컴포넌트에 포함시킵니다:

<div class="content-ad"></div>

```js
<script setup>
import McClockHand from "./McClockHand.vue"
// ...
</script>

<template>
    <div>CLOCK</div>
    <div>{ hours }</div>
    <div>{ minutes }</div>
    <div>{ seconds }</div>

    <div class="mc-clock-area">
        <McClockHand></McClockHand>
    </div>
</template>
```

이렇게 하면 결과가 아래와 같이 나타납니다:

<img src="/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_4.png" />

다시 말씀드리면 꽤 간단합니다. 여기서 중요한 부분들을 하나씩 살펴보도록 하겠습니다.

<div class="content-ad"></div>

- Position absolute를 사용하면 시계 바늘을 원하는 대로 이동시킬 수 있습니다. 그러나 회전하기 위해서는 아직 부족합니다. 그래서 변환을 사용해야 합니다. 이 경우에는 다음 조합으로 잘 처리할 수 있습니다:
- 시계 바늘의 크기를 조절하여 필요한 크기로 만듭니다. 너비: 10%, 높이: 50%. 이렇게 하면 시계 바늘이 올바른 크기가 됩니다.
- 다음으로 대략적으로 위치를 조정합니다. 시계를 시계 중심에 고정하려면 top: 50%, left: 50%가 되어야 합니다.
- 그런 다음 회전 중심을 조정하여 시계의 회전이 얇은 부분 중심을 기준으로 하도록 합니다. 이것은 transform-origin: top center로 수행됩니다.
- 마지막으로 시계 바늘을 회전시켜 위치에 맞추도록 변환합니다: transform: translate(-50%, 0%) rotate(180deg).

이러면 거의 끝난 것입니다. 이제 해야 할 일은 시간에 따라 시계 바늘을 이동시키는 것뿐입니다. 이를 위해 데이터 흐름을 통해 2개의 추가 시계 바늘을 만들어야 합니다. 또한 한 가지 더 변경할 것이 있습니다: 시계 바늘 길이도 속성으로 만들 것입니다.

McCock.vue의 내용은 이제 다음과 같습니다:

```js
<div class="mc-clock-area">
    <McClockHand :position="hours" :positionMax="24" length="41%" />
    <McClockHand :position="minutes" :positionMax="60" length="48%" />
    <McClockHand :position="seconds" :positionMax="60" length="55%" />
</div>
```

<div class="content-ad"></div>

McClocckHand.vue 파일에 대한 코드입니다:

```js
<script setup>
import { computed } from "vue";
const props = defineProps({
    position: Number,
    positionMax: Number,
    length: String
})

const rotation = computed(() =>
    180 + Math.round((360 * props.position) / props.positionMax) + "deg"
);
</script>

<template>
    <div class="mc-clock-hand"></div>
</template>

<style scoped>
.mc-clock-hand {
    position: absolute;
    width: 10%;
    height: v-bind(length);
    background-color: red;
    top: 50%;
    left: 50%;
    transform-origin: top center;
    transform: translate(-50%, 0%) rotate(v-bind(rotation));
    border-radius: 12px;
    border-width: 4px;
    border-style: solid;
    border-color: black;
    box-sizing: border-box;
}
</style>
```

이렇게하면 시계가 다음과 같이 보입니다:

<img src="/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_5.png" />

<div class="content-ad"></div>

여기서 특히 주목할 점 하나가 있어요: 회전을 생성하는 계산이 "도(Degree)"로 끝나야 합니다. 바인딩할 때 CSS에 삽입할 수 있도록 변수는 문자열이어야 합니다.

이렇게 해서 초보적인 시계가 완성되었으니, 두 번째 장에서 이 시계에 상호 작용성을 추가해보겠습니다:

# 시계를 상호 작용적으로 만들기

지금 시간을 표시하는 방법이 생겼으니, 시간을 설정하고 조작하는 방법도 원할겠죠. 우선, 계속되는 시간 틱을 비활성화할 수 있는 확인란을 추가하고, 시간 값을 편집할 수 있도록 만들겠습니다.

<div class="content-ad"></div>

```js
<script setup>
import McClockHand from "./McClockHand.vue"
import { ref, computed, onMounted, onBeforeUnmount } from "vue";

const currentDate = ref(new Date());
const hours = computed({
    get: () => currentDate.value.getHours(),
    set: (value) => {
        if (value == "") return;
        const date = currentDate.value;
        date.setHours(value);
        currentDate.value = new Date(date);
        tick.value = false;
    }
});
const minutes = computed({
    get: () => currentDate.value.getMinutes(),
    set: (value) => {
        if (value == "") return;
        const date = currentDate.value;
        date.setMinutes(value)
        currentDate.value = new Date(date);
        tick.value = false;
    }
});
const seconds = computed({
    get: () => currentDate.value.getSeconds(),
    set: (value) => {
        if (value == "") return;
        const date = currentDate.value;
        date.setSeconds(value)
        currentDate.value = new Date(date);
        tick.value = false;
    }
});
const tick = ref(true);

function updateCurrentDate() {
    if (tick.value) currentDate.value = new Date();
    getNextTick();
}

// ...
</script>

<template>
    <div class="mc-clock">
        <div>CLOCK </div>
        <div>
            <label><input v-model="tick" type="checkbox"></input> Tick </label>
        </div>
        <input v-model="hours" type="number" min="0" max="24"></input>
        <input v-model="minutes" type="number" min="0" max="60"></input>
        <input v-model="seconds" type="number" min="0" max="60"></input>

        <div class="mc-clock-area">
            <McClockHand :position="hours" :positionMax="24" length="41%" />
            <McClockHand :position="minutes" :positionMax="60" length="48%" />
            <McClockHand :position="seconds" :positionMax="60" length="55%" />
        </div>
    </div>
</template>

<style scoped>
.mc-clock {
    overflow: hidden;
    padding-bottom: 48px;
    background-color: beige;
}

.mc-clock-area {
    margin: v-bind(clockMargin);
    position: relative;
    width: calc(100% - v-bind(clockMargin) * 2);
    aspect-ratio: 1;
    background-color: green;
    border-radius: 50%;
    border-width: 16px;
    border-style: solid;
    border-color: blue;
    box-sizing: border-box;
}
</style>
```

우리 시계는 이제 다음과 같이 보입니다:

<img src="/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_6.png" />

여기서 주목할 점은 시계 바늘 위치의 모든 정수 값을 양방향 계산된 값으로 만들었다는 것입니다. 이렇게 하면 입력값을 가져오고 설정할 수 있습니다. 중요한 점은 기존 날짜를 간단히 수정할 수 없다는 것입니다. 그렇게 되면 계산된 getter가 다시 트리거되지 않고 애니메이션이 제대로 렌더링되지 않을 것입니다. 반응성 시스템에 변화를 알리는 가장 쉬운 방법은 수정된 값으로 새로운 Date를 만드는 것입니다. 어떠한 값이 수동으로 변경되면 틱을 비활성화하여 즉시 재설정되지 않도록 합니다. 물론, 그 값을 다시 수동으로 변경하거나 체크박스를 사용하여 비활성화할 수도 있습니다.


<div class="content-ad"></div>

또한, 우리는 배경을 추가하고 옆을 잘라내어 넘치는 부분을 가린 것입니다. 그렇지 않으면 곧 우리의 수정사항이 구성 요소를 이동하고 오류를 초래할 수 있습니다.

이 단계가 멋진 이유는 다음 단계로 자연스럽게 이어진다는 것입니다: 커서로 손을 이동할 수 있게 만드는 것입니다.

"뭐? 그건 정말 어렵지 않은 건가요? 그냥 그렇게 이동 가능한 컨트롤을 추가할 거에요?" — 이 글을 읽는 사람 중에 누군가 물을지도 모릅니다. 네, 맞아요. 실제로 Vue 3로는 꽤 쉽습니다. 시작해봅시다.

## 이제 터치 및 마우스 이벤트를 기록해 보겠습니다!

<div class="content-ad"></div>

이제 시계에 팁을 추가하여 드래그 앤 드롭이 가능하도록 만들려고 합니다. 이를 위해 커서 구성 요소에 팁 요소를 추가하고 이벤트 리스너를 추가합니다.

우리의 McClockHand.vue 파일은 다음과 같습니다:

```js
<script setup>
import { computed, ref, onMounted, onBeforeUnmount } from "vue";
const props = defineProps({
    position: Number,
    positionMax: Number,
    length: String
})

const rotation = computed(() =>
    180 + Math.round((360 * props.position) / props.positionMax) + "deg"
);

const dragging = ref(false);

function startDrag() {
    dragging.value = true
}

function endDrag() {
    dragging.value = false;
}

function mouseDrag(event) {
    if (!dragging.value) return;
    console.log("drag", event);
}

function touchDrag(event) {
    mouseDrag(event.changedTouches[0]);
}

onMounted(() => {
    document.addEventListener("mouseup", endDrag);
    document.addEventListener("touchend", endDrag);
    document.addEventListener("mousemove", mouseDrag)
    document.addEventListener("touchmove", touchDrag)
})

onBeforeUnmount(() => {
    document.removeEventListener("mouseup", endDrag);
    document.removeEventListener("touchend", endDrag);
    document.removeEventListener("mousemove", mouseDrag)
    document.removeEventListener("touchmove", touchDrag)
})
</script>

<template>
    <div class="mc-clock-hand">
        <div class="mc-clock-hand-tip" @mousedown="startDrag" @touchstart="startDrag"></div>
    </div>
</template>

<style scoped>
.mc-clock-hand {
    position: absolute;
    width: 10%;
    height: v-bind(length);
    background-color: red;
    top: 50%;
    left: 50%;
    transform-origin: top center;
    transform: translate(-50%, 0%) rotate(v-bind(rotation));
    border-radius: 12px;
    border-width: 4px;
    border-style: solid;
    border-color: black;
    box-sizing: border-box;
    user-drag: none;
    -webkit-user-drag: none;
    user-select: none;
    -moz-user-select: none;
    -webkit-user-select: none;
    -ms-user-select: none;
    touch-action: none;
}

.mc-clock-hand-tip {
    position: absolute;
    width: 140%;
    aspect-ratio: 1;
    background-color: blueviolet;
    bottom: -10%;
    left: -20%;
    border-radius: 50%;
    border-width: 4px;
    border-style: solid;
    border-color: black;
    box-sizing: border-box;
    cursor: pointer;
}

.mc-clock-hand-tip:hover {
    transform: scale(1.1);
}
</style>
```

현재 결과는 이와 같이 나옵니다:

<div class="content-ad"></div>

Markdown 형식으로 테이블 태그를 변경하세요.

<div class="content-ad"></div>

## 터치 이벤트에 반응하기

이제 필요한 계산을 하면 됩니다. 이번에는 McClockHand.vue 코드에 주석을 달아 계산 과정을 단계별로 안내해 드렸어요:

```js
<script setup>
// ...
const emit = defineEmits(["update:position"])
// ...
const mcClockHandRef = ref();
function mouseDrag(event) {
    if (!dragging.value) return;
    
    // 우리 ref의 부모인 시계 전체의 경계 사각형을 가져옵니다.
    const rect = mcClockHandRef.value.parentElement.getBoundingClientRect();

    // 마우스 위치를 시계의 얼굴 내에서의 상대 위치로 계산합니다.
    const x = event.clientX - rect.x;
    const y = event.clientY - rect.y;

    // 마우스 위치를 시계의 중심을 기준으로 한 상대 위치로 계산합니다.
    const dx = 2 * x - rect.width;
    const dy = 2 * y - rect.height;

    // 마우스 위치와 중심을 기준으로 한 각도를 계산합니다.
    const angle = 180 - Math.atan2(dx, dy) * 180 / Math.PI;

    // 필요한 단계 수에 따라 각도를 정규화하고 반올림합니다.
    const anglePosition = Math.round((angle * props.positionMax) / 360);

    // 업데이트된 위치를 발생시킵니다 - flip over를 방지하기 위해 max 대신 0을 반환합니다.
    if (anglePosition === props.positionMax) emit("update:position", 0)
    else emit("update:position", anglePosition)
}
// ...
</script>

<template>
    <div class="mc-clock-hand" ref="mcClockHandRef">
        <div class="mc-clock-hand-tip" @mousedown="startDrag" @touchstart="startDrag"></div>
    </div>
</template>
```

그리고 우연히 emit으로 인해 이것은 양방향 바인딩이 됩니다. 이제 이를 고려하여 McClock.vue를 수정하면 됩니다:

<div class="content-ad"></div>

```js
<template>
    <div class="mc-clock">
        <!-- ... -->
        <div class="mc-clock-area" ref="mcClockAreaRef">
            <McClockHand v-model:position="hours" :positionMax="24" length="41%" />
            <McClockHand v-model:position="minutes" :positionMax="60" length="48%" />
            <McClockHand v-model:position="seconds" :positionMax="60" length="55%" />
        </div>
    </div>
</template>
```

이제 모든 것이 우리가 원하는 대로 작동합니다:

<img src="https://miro.medium.com/v2/resize:fit:1400/1*BC5LMbxzTo2Jjk8giaCMlA.gif" />

기술 부분을 완료했으니 인정해야 할 한 가지가 있습니다. 우리 시계는 정말 추해요! 걱정 마세요. 이 자습서의 마지막 부분이 올 차례입니다: 이쁘게 만들기.


<div class="content-ad"></div>

# 이쁘게 만들기

기술적으로 우리는 해냈지만, 물론 클라이언트는 그런 추악한 시계를 절대 받아들일까요? 그래서 우리는 지금 당장 반짝이는 제품을 얻어 다른 사람들에게 자랑스럽게 보여줄 수 있도록 예쁘게 꾸미는 작업이 필요합니다. 그리고 실제로, 아마 당신은 썸네일에 넣은 예쁜 시계를 보러 왔을 것입니다.

걱정하지 마세요! 우리는 이제 예쁘게 꾸밀 거에요.

하나 확실히 말하자면, 나는 기술 부분을 먼저 완벽하게 만든 다음에 아름다운 디자인을 하는 것을 지지하는 입장입니다. 아름다움을 먼저 처리하면 기술적 제약을 마주할 때 만들어놓은 것을 파괴할 위험이 있습니다. 그런 제약이 해결되면 우리가 원하는대로 할 수 있습니다.

<div class="content-ad"></div>

먼저, 예쁜 시계 바늘부터 시작해 봅시다.

## 시계 바늘 그리기

저는 모두 Inkscape로 만들었어요:

![Clockhands](/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_8.png)

<div class="content-ad"></div>


![image1](/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_9.png)

![image2](/assets/img/2024-08-04-MakinganinteractiveanalogueclockwithVue3_10.png)

벡터 그래픽은 웹 개발자의 가장 친한 친구입니다. 종종 Inkscape로 기본적인 벡터 그래픽을 만드는 것이 네이티브 HTML로 뭔가 멋진 것을 시도하는 것보다 훨씬 쉽습니다. 저는 그냥 빠르게 만들었습니다. 이제 이것들을 public 폴더에 넣으면 됩니다!

이제 ClockHand의 코드를 변경하여 이미지를 포함시키기만 하면 됩니다:


<div class="content-ad"></div>

```js
<script setup>
// ...
const props = defineProps({
  // ...
  imgSrc: String,
  imgClass: String,
});

</script>

<template>
  <div class="mc-clock-hand" ref="mcClockHandRef">
    <img :src="props.imgSrc" :class="props.imgClass" />
    <div
      :class="{
        'mc-clock-hand-tip': true,
        'mc-clock-hand-tip-shining': dragging,
      }"
      @mousedown="startDrag"
      @touchstart="startDrag"
    ></div>
  </div>
</template>

<style scoped>
.mc-clock-hand {
  position: absolute;
  width: 10%;
  height: v-bind(length);
  top: 50%;
  left: 50%;
  transform-origin: top center;
  transform: translate(-50%, 0%) rotate(v-bind(rotation));
  user-drag: none;
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  touch-action: none;
}

.mc-clock-hand-tip {
  position: absolute;
  width: 200%;
  aspect-ratio: 1;
  background-color: yellow;
  bottom: -10%;
  left: -50%;
  border-radius: 50%;
  box-sizing: border-box;
  cursor: pointer;
  opacity: 0;
  filter: blur(5vw);
}

.mc-clock-hand-tip-shining,
.mc-clock-hand-tip:hover {
  transform: scale(1.1);
  opacity: 0.4;
}
</style>
```

이번 단계에서는 몇 가지를 수행했습니다:

- img의 src 및 class를 외부에서 받아와서 각 시계 바늘을 개별적으로 구성할 수 있게 했습니다. 이렇게 하면 모든 바늘에 동일한 코드를 사용하면서도 각각을 구성할 수 있습니다!
- 시계 바늘의 색상을 제거하고 대신 img를 추가했습니다.
- 시계 바늘 팁을 기본적으로 투명하게 만들고 마우스를 가져가면 밝은 노란색으로 흐리게 만듭니다.
- 또한 드래그할 때 상시로 반짝이는 효과를 주기 위해 "mc-clock-hand-tip-shining"을 두 번째 CSS 속성으로 추가했습니다. 주목할 점은 우리가 사용하는 표기법입니다. 이는 Vue를 사용하여 변수나 조건에 따라 CSS 클래스를 쉽게 활성화하거나 비활성화할 수 있는 매우 효율적인 방법입니다!

동시에 McClock.vue에서는 모든 것을 연결시키고 색상을 시계 바늘에 맞추도록 변경했습니다.

<div class="content-ad"></div>

```js
<template>
  <div class="mc-clock">
    <div>CLOCK</div>
    <div>
      <label><input v-model="tick" type="checkbox" /> Tick </label>
    </div>
    <input v-model="hours" type="number" min="0" max="24" />
    <input v-model="minutes" type="number" min="0" max="60" />
    <input v-model="seconds" type="number" min="0" max="60" />

    <div class="mc-clock-area" ref="mcClockAreaRef">
      <McClockHand
        v-model:position="hours"
        :positionMax="24"
        length="41%"
        imgSrc="hours.svg"
        imgClass="mc-clock-hand-hours"
      />
      <McClockHand
        v-model:position="minutes"
        :positionMax="60"
        length="48%"
        imgSrc="minutes.svg"
        imgClass="mc-clock-hand-minutes"
      />
      <McClockHand
        v-model:position="seconds"
        :positionMax="60"
        length="55%"
        imgSrc="seconds.svg"
        imgClass="mc-clock-hand-seconds"
      />
    </div>
  </div>
</template>

<style scoped>
.mc-clock {
  overflow: hidden;
  padding-bottom: 48px;
  background-color: beige;
}

.mc-clock-area {
  margin: v-bind(clockMargin);
  position: relative;
  width: calc(100% - v-bind(clockMargin) * 2);
  aspect-ratio: 1;
  background-color: gray;
  border-radius: 50%;
  border-width: 16px;
  border-style: solid;
  border-color: balck;
  box-sizing: border-box;
}
</style>
```

주로 src 및 클래스만 전달하고 있습니다. 하지만 각 시계 바늘에 대한 클래스는 어디에 있을까요? main.css에 있습니다. 이는 SVG와 밀접하게 연결되어 해상도에 맞춰진 클래스들로 전역입니다.

```js
@import "./reset.css";

.mc-clock-hand-hours {
  width: 600%;
  transform: translate(-44%, -42%) rotate(180deg);
  pointer-events: none;
}

.mc-clock-hand-minutes {
  width: 750%;
  transform: translate(-45%, -40%) rotate(180deg);
  pointer-events: none;
}

.mc-clock-hand-seconds {
  width: 1300%;
  transform: translate(-47%, -53%) rotate(180deg);
  pointer-events: none;
}
```

각 클래스는 원하는 대로 표시되도록 자체 SVG 이미지를 확대/축소합니다!

<div class="content-ad"></div>

지금까지 우리의 결과물을 살펴보겠습니다:

![image](https://miro.medium.com/v2/resize:fit:1400/1*37ElcRGPRtomFbHujdzKNA.gif)

내가 말하길, 정말 멋지게 보이네요. 스타일이 모두에게 맞지 않을 수도 있지만, 어쨌든 — 이것은 기술적인 튜토리얼이니까요!

이제 마지막 조각을 해결해봅시다:

<div class="content-ad"></div>

## 시계에 숫자 표시하기

새로운 숫자 파일을 만들어봅시다. 파일명은 McClockDigit.vue이고 내용은 다음과 같습니다:

```js
<script setup>
const props = defineProps({
  digit: Number,
});
</script>

<template>
  <div class="mc-clock-digit-line"></div>
  <div class="mc-clock-digit-container">
    <div class="mc-clock-digit">
      { digit }
    </div>
  </div>
</template>

<style scoped>
.mc-clock-digit-line {
  position: absolute;
  width: 1%;
  height: 20%;
  top: 50%;
  left: 50%;
  transform-origin: bottom center;
  transform: translate(-50%, -100%) rotate(calc(v-bind(digit) * 30deg)) translate(0%, -100%);
  background-color: black;
}

.mc-clock-digit-container {
  position: absolute;
  width: 1%;
  height: 50%;
  top: 50%;
  left: 50%;
  transform-origin: bottom center;
  transform: translate(-50%, -100%) rotate(calc(v-bind(digit) * 30deg));
}

.mc-clock-digit {
  position: absolute;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 7.6vw;
  width: 10vw;
  height: 10vw;
  line-height: 10vw;
  text-align: center;
  background-color: darkgrey;
  border-radius: 50%;
  border-width: 1px;
  border-style: solid;
  transform: translate(-50%, 0%) rotate(calc(v-bind(digit) * -30deg));
}
</style>
```

주의할 점들:

<div class="content-ad"></div>

- 우리는 단 하나의 속성을 전달하고 있습니다; 그 외의 모든 것은 CSS 삽입과 HTML에 의해 처리됩니다.
- 이는 정적인 구성 요소입니다. 숫자가 한 번 전달되고 나중에 여러 숫자가 렌더링됩니다.
- 구성 요소가 렌더링되는 방식은 변환을 적극적으로 사용하여 숫자를 올바른 각도에 배치합니다.
- 숫자의 실제 표시는 그 후 올바른 위치에 회전하여 세워진 상태로 변경됩니다.

McClock.vue에 대해, 숫자를 추가하는 방법은 다음과 같습니다:

```js
<template>
  <div class="mc-clock">
    <div>CLOCK</div>
    <div>
      <label><input v-model="tick" type="checkbox" /> Tick </label>
    </div>
    <input v-model="hours" type="number" min="0" max="24" />
    <input v-model="minutes" type="number" min="0" max="60" />
    <input v-model="seconds" type="number" min="0" max="60" />

    <div class="mc-clock-area" ref="mcClockAreaRef">
      <McClockDigit
        v-for="index in 12"
        :key="index"
        :digit="index"
      ></McClockDigit>

      <McClockHand
        v-model:position="hours"
        :positionMax="12"
        length="41%"
        imgSrc="hours.svg"
        imgClass="mc-clock-hand-hours"
      />
      <McClockHand
        v-model:position="minutes"
        :positionMax="60"
        length="48%"
        imgSrc="minutes.svg"
        imgClass="mc-clock-hand-minutes"
      />
      <McClockHand
        v-model:position="seconds"
        :positionMax="60"
        length="55%"
        imgSrc="seconds.svg"
        imgClass="mc-clock-hand-seconds"
      />
    </div>
  </div>
</template>

<style scoped>
.mc-clock {
  overflow: hidden;
  background-color: beige;
}

.mc-clock-area {
  margin: v-bind(clockMargin);
  position: relative;
  width: calc(100% - v-bind(clockMargin) * 2);
  aspect-ratio: 1;
  background-color: gray;
  border-radius: 50%;
  border-width: 1vw;
  border-style: solid;
  border-color: black;
  box-sizing: border-box;
}
</style>
```

우리는 v-for 루프를 사용하여 숫자 위에 반복하고 있습니다. 몇 가지 미적인 변경 사항이 있었음을 알 수 있습니다 — 그 중 하나는 시계에는 숫자가 12개만 있는데, 시침에는 24개의 위치가 있었다는 점입니다. 우리의 아키텍처가 유연하게 구성되어 있어서 단순히 숫자를 12로 변경하여 문제를 해결할 수 있다는 점이 다행입니다.

<div class="content-ad"></div>

다음은 최종 결과물입니다:

<img src="https://miro.medium.com/v2/resize:fit:1400/1*B5jRr_mJ6OvF1cKDSS5xPg.gif" />

# Vue.js에 대한 첫 번째 튜토리얼을 읽어 주셔서 정말 감사합니다! 오늘 무언가를 배우셨길 바랍니다. 피드백과 아이디어에 대해 매우 열려 있으니 망설이지 마세요.

이런 종류의 시계에 대한 튜토리얼을 만들고 싶었던 아이디어가 있었는데, 만들면서 정말 즐거웠어요. 시계의 시간이 움직이는 것을 주목하신 분들이 있을 텐데 — 네, 제 컴퓨터의 시간이 움직이는 것이기 때문에 저는 오늘 밤에 늦게 마무리하게 되었어요. 하지만 하루 동안 이 프로젝트와 튜토리얼을 완성할 수 있었음에 정말 기뻐요.

<div class="content-ad"></div>

이런 것을 만드는 것이 정말 즐거웠어요! 이 프로젝트의 전체 소스 코드는 다음에서 찾을 수 있습니다: https://github.com/AdSkipper1337/vue-masterclass-clock. 시계를 확인하려면 https://adskipper1337.github.io/vue-masterclass-clock/으로 이동해주세요. 즐겁게 사용해보세요!

감사합니다,
니콜라이