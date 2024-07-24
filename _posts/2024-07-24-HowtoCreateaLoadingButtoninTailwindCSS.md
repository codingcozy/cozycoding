---
title: "Tailwind CSS로 로딩 버튼 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-24-HowtoCreateaLoadingButtoninTailwindCSS_0.png"
date: 2024-07-24 11:41
ogImage: 
  url: /assets/img/2024-07-24-HowtoCreateaLoadingButtoninTailwindCSS_0.png
tag: Tech
originalTitle: "How to Create a Loading Button in Tailwind CSS"
link: "https://dev.to/saim_ansari/how-to-create-a-loading-button-in-tailwind-css-3hdo"
---


사용자 경험을 향상시키는 데 시각적인 피드백을 제공하는 것이 중요합니다. 이 튜토리얼에서는 Tailwind CSS를 사용하여 로딩 버튼을 만드는 방법을 보여드릴 거에요. 이 방법은 간단하고 웹 프로젝트에 완벽할 거예요. 시작해볼까요?

파란색 버튼을 만들고 돌아가는 로딩 아이콘과 '로딩 중...' 텍스트를 표시합니다. 버튼은 로딩 중일 때 비활성화됩니다.

```js
<button class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline flex items-center" disabled>
  <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
  </svg>
  로딩 중...
</button>
```

![로딩 버튼 만들기](/assets/img/2024-07-24-HowtoCreateaLoadingButtoninTailwindCSS_0.png)


<div class="content-ad"></div>

### 알파인JS를 사용한 로딩 버튼 Tailwind CSS

이 버튼은 상태와 모양을 관리하기 위해 Alpine.js를 사용합니다. 클릭하면 로딩 스피너가 표시되고 2초 동안 텍스트가 "로딩 중..."으로 변경됩니다.

```js
<button
  x-data="{ loading: false }"
  x-on:click="loading = true; setTimeout(() => loading = false, 2000)"
  :class="{ 'opacity-50 cursor-not-allowed': loading }"
  :disabled="loading"
  class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline flex items-center"
>
  <svg
    x-show="loading"
    class="animate-spin -ml-1 mr-3 h-5 w-5 text-white"
    xmlns="http://www.w3.org/2000/svg"
    fill="none"
    viewBox="0 0 24 24"
  >
    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
  </svg>
  <span x-text="loading ? 'Loading...' : 'Submit'"></span>
</button>
```

<img src="/assets/img/2024-07-24-HowtoCreateaLoadingButtoninTailwindCSS_1.png" />