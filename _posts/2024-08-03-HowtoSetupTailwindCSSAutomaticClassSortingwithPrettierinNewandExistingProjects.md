---
title: "새 프로젝트와 기존 프로젝트에서 Tailwind CSS 자동 클래스 정렬을 설정하는 방법 Prettier 사용"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 19:32
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "How to Setup Tailwind CSS Automatic Class Sorting with Prettier in New and Existing Projects"
link: "https://dev.to/iamsheye/how-to-setup-tailwind-css-automatic-class-sorting-with-prettier-in-new-and-existing-projects-2ob8"
---


## 소개

Tailwind CSS는 직관적인 CSS 프레임워크로, 로우레벨의 유틸리티 클래스를 제공하여 스타일을 마크업에 직접 적용할 수 있게 해 주어 더 신속한 개발 주기를 제공합니다.

한편 Prettier는 많이 사용되는 코드 형식 지정기로, 코드를 파싱하고 자체 규칙에 따라 다시 작성함으로써 일관된 형식으로 유지될 수 있게 합니다. 이를 통해 전체 프로젝트에서 일관된 코드 스타일을 유지함으로써 코드베이스를 보다 깔끔하고 가독성이 높게 유지할 수 있습니다.

Tailwind CSS를 사용할 때 자주 마주치는 문제 중 하나는 유틸리티 클래스의 순서를 관리하는 것입니다, 특히 스타일과 HTML의 복잡성이 증가할수록 클래스를 수동으로 정렬하는 것은 귀찮고 실수를 유발할 수 있습니다. 여기에서 자동 클래스 정렬이 필요해집니다. Prettier와 prettier-plugin-tailwindcss와 같은 플러그인을 활용하여 Tailwind CSS 클래스의 정렬을 자동화함으로써 일관된 순서를 유지하고 클래스의 가독성과 유지보수성을 향상시킬 수 있습니다.

<div class="content-ad"></div>

이 기사의 목적은 Tailwind CSS 자동 클래스 정렬과 Prettier 설정 과정을 새로운 프로젝트와 기존 프로젝트 모두에 대해 안내하는 데 있습니다. 새 프로젝트를 시작하거나 진행 중인 프로젝트에 통합할 때 차근차근 따라할 수 있는 포괄적인 안내서이니 안심하세요.

## 목차

- 새 프로젝트에서 Tailwind CSS 및 Prettier 설정하기
프로젝트 초기화 및 Tailwind CSS 설치
Prettier 설치 및 구성
- 프로젝트 초기화 및 Tailwind CSS 설치
- Prettier 설치 및 구성
- 기존 Tailwind CSS 프로젝트에서 prettier-plugin-tailwindcss 설정하기
- 결론

## 새 프로젝트에서 Tailwind CSS 및 Prettier 설정하기

<div class="content-ad"></div>

시작하기 전에 다음이 설치되어 있는지 확인해주세요:

- Node.js
- 패키지 매니저 (이 프로젝트에서는 bun을 사용할 것이지만, 원하는 경우 npm, yarn 또는 pnpm을 사용할 수 있습니다)
- 코드 편집기 (예: VS Code)

### 프로젝트 초기화 및 Tailwind CSS 설치

새 프로젝트를 생성하여 시작해보세요. 특정 단계는 선호하는 프레임워크나 설정에 따라 달라질 수 있습니다. 자세한 지침은 Tailwind CSS 설치 가이드를 참조해주세요. 이미 Tailwind CSS 설치 단계를 완료했다면, "기존 Tailwind CSS 프로젝트에 prettier-plugin-tailwindcss 설정하기" 섹션으로 이동할 수 있습니다. Vite를 사용하여 다음과 같이 진행해보세요:

<div class="content-ad"></div>

```js
bun create vite my-app --template react-ts
cd my-app
bun install
```

이제 Tailwind CSS를 설치하고 구성해 봅시다.

```js
bun install -D tailwindcss postcss autoprefixer
bunx tailwindcss init -p
```

Tailwind CSS 구성 파일인 tailwind.config.js가 나타날 것입니다. 다음 내용을 그대로 복사하여 붙여넣으세요.

<div class="content-ad"></div>

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

다음과 같이 Tailwind 지시문을 CSS 파일 상단에 추가해주세요 (예: src/index.css):

```js
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Prettier 설치 및 구성하기


<div class="content-ad"></div>

```js
bun install -D prettier prettier-plugin-tailwindcss
```

프로젝트 루트에 prettier 설정 파일을 생성하고 prettier-plugin-tailwindcss 플러그인을 추가하세요. 이를 위해 .prettierrc를 사용할 것인데, 여기서 다른 허용되는 prettier 설정 파일 유형을 확인할 수 있습니다.

```js
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

이제 설정을 테스트해보겠습니다. src/App.tsx 파일을 수정하고 저장하세요.

<div class="content-ad"></div>

```js
import { useState } from "react";
import "./App.css";

const App = () => {
  const [count, setCount] = useState(0);

  return (
    <>
      <div className="card">
        <button
          className="custom-btn border-2 border-teal-700 bg-white text-slate-800 transition-colors duration-300 hover:border-white hover:bg-teal-700 hover:text-white"
          onClick={() => setCount((count) => count + 1)}
        >
          Count is {count}
        </button>
      </div>
    </>
  );
};

export default App;
```

결과:

```js
import { useState } from "react";
import "./App.css";

const App = () => {
  const [count, setCount] = useState(0);

  return (
    <>
      <div className="card">
        <button
          className="custom-btn border-2 border-teal-700 bg-white text-slate-800 transition-colors duration-300 hover:border-white hover:bg-teal-700 hover:text-white"
          onClick={() => setCount((count) => count + 1)}
        >
          Count is {count}
        </button>
      </div>
    </>
  );
};

export default App;
```

## 기존 Tailwind CSS 프로젝트에서 prettier-plugin-tailwindcss 설정하기


<div class="content-ad"></div>

프로젝트에 이미 Prettier가 설정되어 있다면, prettier-plugin-tailwindcss 플러그인을 통합하는 것은 간단합니다. 플러그인을 설치하고 구성하기만 하면 됩니다. 아직 Prettier가 설치되어 있지 않다면 플러그인과 함께 설정해야 합니다.

기존의 Prettier 설정이 있는 프로젝트의 경우 다음을 실행하십시오:

```js
bun add -D prettier-plugin-tailwindcss
```

Prettier 설정이 없는 프로젝트의 경우 다음을 실행하십시오:

<div class="content-ad"></div>

```js
bun add -D prettier prettier-plugin-tailwindcss
```

기존 Prettier 구성에 플러그인을 추가하세요. 만약 기존 Prettier 구성이 없는 경우, 프로젝트의 루트에 .prettierrc 파일을 만드세요. 그 후 다음을 추가하세요:

```js
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

Tailwind CSS 클래스가 포함된 파일을 수정하여 Prettier가 제대로 작동하는지 확인하세요. 파일을 저장하고 Tailwind CSS 클래스가 자동으로 정렬되는지 확인하세요.


<div class="content-ad"></div>


![image](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fatvku52mb31k10mafly2.gif)

## 결론

prettier-plugin-tailwindcss를 새로운 Tailwind CSS 프로젝트와 기존 프로젝트에 통합하면 일관된 및 정리된 클래스 정렬을 보장하여 개발 작업 흐름을 개선할 수 있습니다. 새 프로젝트의 경우 Prettier와 플러그인을 동시에 설정하여 초기 구성을 간소화할 수 있습니다. 기존 프로젝트의 경우 기존 Prettier 설정에 플러그인을 추가하거나 Prettier 구성이 아직 완료되지 않았다면 Prettier와 플러그인을 모두 설치하면 됩니다.

비표준 속성에서 클래스를 정렬하는 추가 구성 옵션과 관련된 내용은 prettier-plugin-tailwindcss 문서를 참조하십시오.

