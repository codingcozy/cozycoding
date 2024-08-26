---
title: "esbuild로 빠른 React 빌드 언락하기 완전한 설정 가이드"
description: ""
coverImage: "/assets/img/2024-08-26-UnlockLightning-FastReactBuildswithesbuildACompleteSetupGuide_0.png"
date: 2024-08-26 18:27
ogImage: 
  url: /assets/img/2024-08-26-UnlockLightning-FastReactBuildswithesbuildACompleteSetupGuide_0.png
tag: Tech
originalTitle: "Unlock Lightning-Fast React Builds with esbuild A Complete Setup Guide"
link: "https://medium.com/@adityagarg2198/unlock-lightning-fast-react-builds-with-esbuild-a-complete-setup-guide-32f4432af4c8"
isUpdated: false
---


<img src="/assets/img/2024-08-26-UnlockLightning-FastReactBuildswithesbuildACompleteSetupGuide_0.png" />

소개:

현대 소프트웨어 개발에서 효율성과 속도는 생산성과 프로젝트 성공에 영향을 미치는 중요한 요소입니다. React 개발자들에게는 빌드 과정이 종종 중요한 병목 현상이 될 수 있습니다.

이 글에서는 빌드를 최적화하기 위해 설계된 혁신적인 빌드 도구인 esbuild를 활용하는 방법을 탐색해 보겠습니다. 우리는 React 프로젝트를 esbuild로 설정하는 포괄적인 안내서를 제공할 것이며, 놀라운 빌드 시간과 개발 효율성 향상을 목표로 할 것입니다.

<div class="content-ad"></div>

빈 Node.js 프로젝트를 생성하려면 npm init을 실행해주세요.

```js
npm init
```

<img src="/assets/img/2024-08-26-UnlockLightning-FastReactBuildswithesbuildACompleteSetupGuide_1.png" />

<div class="content-ad"></div>

npm init 명령을 실행한 후에 나타나는 매개변수에 대한 값을 제공해주세요. 필요하다면 지금은 비워 두고 나중에 업데이트할 수도 있어요.

필요한 npm 종속성 설치

```js
npm install @web/dev-server chokidar-cli esbuild react react-dom @mui/material @emotion/styled @emotion/react
```

패키지 목록을 설치한 후에는 다음 종속성이 포함된 package.json이 업데이트될 거에요.

<div class="content-ad"></div>

안녕하세요! 위의 텍스트를 친절하고 친근하게 한국어로 번역해 드리겠습니다.

표 태그를 Markdown 형식으로 변경해 주세요.

<div class="content-ad"></div>

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

App.tsx

```js
import { Button } from '@mui/material';
import React from 'react';

const App = () => {
  return <Button variant='contained'>Click Me</Button>;
};

export default App;
```

프로젝트 빌드를 위한 esbuild 설정하기


<div class="content-ad"></div>

루트 디렉토리에 esbuild.config.js라는 파일을 만들어서 다음 내용을 복사해 붙여넣으세요:

```js
const esbuild = require('esbuild');

esbuild
  .build({
    entryPoints: ['src/index.tsx'],
    bundle: true,
    outfile: 'dist/index.js',
    loader: {
      '.tsx': 'tsx',
      '.ts': 'tsx',
      '.png': 'file',
      '.svg': 'file',
    },
    sourcemap: true,
    minify: false,
  })
  .catch(() => process.exit(1));
```

- esbuild를 import하여 빌드 기능을 사용합니다.
- entryPoints를 src/index.tsx로 정의하여 메인 파일을 지정합니다.
- bundle을 활성화하여 모든 종속성을 단일 파일로 결합합니다.
- outfile를 dist/index.js로 설정하여 출력 위치를 지정합니다.
- .tsx, .ts, .png 및 .svg 파일을 처리하기 위한 loader를 구성합니다.
- 디버깅을 쉽게 하기 위해 소스맵 생성을 활성화합니다.
- 코드를 가독성 있게 유지하기 위해 미니파이를 비활성화합니다.

<div class="content-ad"></div>

```js
module.exports = {
  rootDir: '.',
  port: 3000,
  open: true,
  watch: true,
};
```

아래 스크립트를 사용하여 package.json 파일을 업데이트하십시오:

```js
"scripts": {
  "build": "node esbuild.config.js",
  "watch": "chokidar 'src/**/*.{ts,tsx,css}' -c 'npm run build' & web-dev-server --config server.config.js"
},
```

- build: 이 스크립트는 esbuild.config.js 파일을 사용하여 빌드 프로세스를 실행합니다.
- watch: 이 스크립트는 src 폴더의 .ts, .tsx 및 .css 파일의 변경 사항을 감시합니다. 변경이 감지되면 프로젝트를 자동으로 다시 빌드하고 server.config.js에서 설정한 내용을 사용하여 포트 3000에서 개발 서버를 시작합니다.

<div class="content-ad"></div>

그걸로 끝입니다 - 이제 React 프로젝트가 localhost:3000에서 실행 중입니다. 즐거운 코딩하세요!