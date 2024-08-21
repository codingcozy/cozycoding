---
title: "자바스크립트에서 require와 import 사용 방법 정리"
description: ""
coverImage: "/assets/img/2024-08-21-UnderstandingrequireandimportinJavaScriptAComprehensiveGuide_0.png"
date: 2024-08-21 17:22
ogImage: 
  url: /assets/img/2024-08-21-UnderstandingrequireandimportinJavaScriptAComprehensiveGuide_0.png
tag: Tech
originalTitle: "Understanding require and import in JavaScript A Comprehensive Guide"
link: "https://medium.com/@naveenyaduvanshi007/understanding-require-and-import-in-javascript-a-comprehensive-guide-413765bc5e44"
isUpdated: true
updatedAt: 1724244577641
---


![이미지](/assets/img/2024-08-21-UnderstandingrequireandimportinJavaScriptAComprehensiveGuide_0.png)

자바스크립트는 몇 년 동안 급속히 발전해 왔으며, 이로 인해 코드 내에서 모듈을 다루는 방법이 크게 변화했습니다. 자바스크립트에서 모듈을 가져오는 두 가지 주요 방법은 require과 import입니다. 자바스크립트 생태계에서 작업해 온 경우, 특히 Node.js의 이전 버전에서 넘어가거나 React 또는 Next.js와 통합할 때 이 두 가지를 만난 적이 있을 것입니다.

이 기사에서는 require과 import의 차이점, 각각을 언제 사용해야 하는지, 그리고 자바스크립트의 모듈 시스템이 어떻게 발전하고 있는지 살펴볼 것입니다.

# 기초: require

<div class="content-ad"></div>

`require`는 Node.js를 위해 도입된 CommonJS 모듈 시스템의 일부입니다. ES6 모듈(import/export)이 표준화되기 전에는 CommonJS가 Node.js 환경에서 코드를 모듈화하는 데 자주 사용되었습니다. 다음은 작동 방식입니다:

```js
const fs = require('fs');
const myModule = require('./myModule');
```

`require`를 사용하면 모듈을 동적으로 가져올 수 있어서 런타임에 조건부로 모듈을 요구할 수 있습니다. 예를 들어:

```js
if (condition) {
  const someModule = require('some-module');
}
```

<div class="content-ad"></div>

require의 주요 특징:

- 동적로딩: 모듈을 조건부로 또는 실행 중에 로드할 수 있습니다.
- 동기식: require 함수는 동기적으로 실행되어 스크립트가 모듈이 완전히 로드될 때까지 기다립니다.
- Node.js에서 일반적: require는 Node.js에서 모듈을 로드하는 기본 방법입니다 (ES6 모듈 이전).

# 현대적인 접근 방식: import

# 현대적인 접근 방식: import

<div class="content-ad"></div>

ES6(ESMAScript 2015)의 도입으로 JavaScript는 import 및 export를 사용한 모듈 구문을 표준화했습니다. 이 새로운 시스템은 모듈에 더 깔끔하고 구조화된 접근 방식을 제공하여 특히 프론트엔드 개발을 보다 쉽게 관리할 수 있게 합니다.

아래는 import를 사용하는 기본적인 예시입니다:

```js
import fs from 'fs';
import { myFunction } from './myModule';
```

import의 주요 특성:

<div class="content-ad"></div>

- 정적 Imports: 모듈은 정적으로 가져와집니다. 즉, import 문은 파일의 맨 위로 끌어올려지고 나머지 코드가 실행되기 전에 평가됩니다.
- ES6 표준: import는 ES6 표준의 일부이며 React, Vue, Angular와 같은 현대 JavaScript 프레임워크에서 널리 사용됩니다.
- 비동기적 특성: import 문은 정적이지만, ES6 모듈 시스템은 비동기 코드를 고려하여 설계되었습니다. 브라우저는 모듈 종속성을 미리 가져와서 성능을 향상시킬 수 있습니다.

과거 버전의 Node.js에서 import의 한 가지 주요한 제한은 require처럼 동적으로 사용할 수 없었습니다. 그러나 Node.js의 새로운 버전(12.x 이상)에서는 동적 import를 사용할 수 있습니다:

```js
if (condition) {
  const { someFunction } = await import('some-module');
}
```

# require와 import 간의 주요 차이점

<div class="content-ad"></div>

문법 및 표준:

- require는 CommonJS 모듈 시스템을 기반으로 하며, import는 ES6 모듈을 기반으로 합니다.
- require는 module.exports 및 exports 객체를 사용하여 내용을 내보내지만, import는 export 및 export default에 의존합니다.

동기 대화 비동기:

- require는 동기적으로 작동하여 모듈이 완전히 로드될 때까지 코드가 일시 중지됩니다.
- import는 브라우저에서 비동기적으로 로딩되고 현대 JavaScript 엔진의 비동기 기능을 위해 설계되었습니다.

<div class="content-ad"></div>

환경 호환성:

- require은 Node.js 환경에서 널리 지원되며, 기존 JavaScript 응용 프로그램에서 사용됩니다.
- import는 현대 JavaScript (ES6)에서의 표준이며, "package.json"에서 "type": "module"로 설정된 현대 브라우저 및 Node.js 버전에서 기본적으로 지원됩니다.

정적 vs 동적 Imports:

- require은 동적 로딩을 허용하여 런타임 중 상황에 따라 모듈을 쉽게 로드할 수 있습니다.
- import은 기본적으로 정적이며, 파일의 최상위 수준에 있어야 합니다. import()를 통한 동적 imports도 가능하지만 적게 사용됩니다.

<div class="content-ad"></div>

# `require`과 `import`을 언제 사용해야 할까요?

Node.js 프로젝트 (레거시 및 모던):

- 오래된 Node.js 프로젝트나 라이브러리에서는 `require`를 많이 볼 수 있습니다. 환경이 ES6 모듈을 지원하지 않는 경우에는 `require`를 사용하세요.
- 더 최신 버전인 Node.js 응용 프로그램에서는, 특히 12.x 버전 이상에서는 새 코드를 작성하거나 현대적인 라이브러리를 사용하는 경우 `import`를 선호합니다.

프론트엔드 개발:

<div class="content-ad"></div>

- 프론트엔드 JavaScript에서, 특히 React, Vue 및 Angular와 같은 현대 프레임워크를 사용할 때는 import가 표준입니다. 이는 더 나은 도구 지원, 트리 쉐이킹 및 모듈 관리를 보장합니다.

하이브리드 환경:

- 프로젝트가 프론트엔드와 백엔드 코드를 모두 포함하는 경우 (예: Next.js 또는 다른 풀스택 프레임워크), require 및 import를 혼합하여 사용해야할 수 있습니다. 백엔드 코드는 여전히 require를 사용할 수 있지만, 프론트엔드는 import를 채택할 수 있습니다. 시간이 흘러 일관성을 유지하고 미래를 준비하기 위해 모든 것을 import로 전환하는 것이 바람직합니다.

# require를 import로 전환하기

<div class="content-ad"></div>

프로젝트 코드베이스에서 require에서 import로 전환하는 것은 종종 간단한 프로세스입니다. 아래는 리팩토링하는 방법 예시입니다:

require를 사용하는 경우:

```js
const express = require('express');
const app = express();

const myModule = require('./myModule');
myModule.doSomething();
```

import를 사용하는 경우:

<div class="content-ad"></div>

```js
import express from 'express';
import { doSomething } from './myModule';

const app = express();
doSomething();
```

중요한 주의사항 하나: Node.js 프로젝트를 ES6 모듈로 전환하는 경우 package.json에 "type": "module"을 추가하여 파일을 모듈로 처리하도록 Node.js에 알려야 합니다.

```js
{
  "type": "module"
}
```

이렇게 하면 Node.js 코드에서 import/export를 사용할 수 있습니다.

<div class="content-ad"></div>

# 결론

자바스크립트가 계속 발전함에 따라, require에서 import로의 전환은 더 나은 표준화와 성능을 향한 중요한 변화를 의미합니다. require은 기존 시스템 및 특정 Node.js 환경에서 여전히 유효하지만, 특히 최신 애플리케이션에서는 import가 JavaScript 모듈 관리의 미래입니다.

프론트엔드나 백엔드에서 작업하더라도 각각의 차이를 이해하고 언제 사용해야 하는지 파악하는 것은 더 깨끗하고 유지보수 가능한 코드를 작성하는 데 도움이 됩니다. require에서 import로의 여정은 작은 단계처럼 보일 수 있지만, 이는 JavaScript 생태계 전반에서 더 모듈식이고 확장 가능한 미래로의 변화를 반영합니다.

import/export를 활용하여 현대적인 ES6 모듈 시스템을 받아들이면 현대적인 JavaScript 개발의 복잡성을 다루는 데 더 잘 대비할 수 있을 것입니다.

<div class="content-ad"></div>

행복한 코딩하세요!