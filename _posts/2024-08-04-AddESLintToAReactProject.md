---
title: "React 프로젝트에 ESLint 추가하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-04 19:44
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Add ESLint To A React Project"
link: "https://dev.to/jenesh/add-eslint-to-a-react-project-1m32"
isUpdated: true
updatedAt: 1724246258807
---


리액트 프로젝트에 린트 규칙을 추가하는 것은 코드 품질을 향상시키고 코드를 일관되게 유지하며 버그를 피하는 데 중요합니다.

자바스크립트 코드에서 발견된 잘못된 패턴을 자동으로 감지하는 인기 있는 오픈 소스 자바스크립트 린팅 도구인 ESLint가 있습니다.

리액트 프로젝트에 린팅 규칙을 추가하는 단계별 방법은 다음과 같습니다:

## ESLint 설치하기

<div class="content-ad"></div>

우선, 우리 React 프로젝트에 ESLint를 설치해야 합니다. 이것은 우리가 프로덕션 환경에서는 필요하지 않기 때문에 개발 종속성으로 설치해야 합니다.

설치하기 위해서 아래 명령어를 사용할 것입니다.

```js
npm i -D eslint
```

## ESLint 초기화

<div class="content-ad"></div>

그 다음으로 ESLint 구성을 초기화해야 합니다. 프로젝트의 루트 폴더에 .eslintrc.json 구성 파일을 추가하면 됩니다.

여기 샘플 구성 예제가 있습니다.

```js
{
  "extends": [
    "eslint:recommended",
    "plugin:import/errors",
    "plugin:react/recommended",
    "plugin:jsx-a11y/recommended"
  ],
  "plugins": ["react", "import", "jsx-a11y"],
  "rules": {
    // 여기에 우리가 사용자 정의 규칙을 추가할 수 있습니다.
  },
  "parserOptions": {
    "ecmaVersion": 2021,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "env": {
    "es6": true,
    "browser": true,
    "node": true
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  }
}
```

.eslintrc.json 파일 안에 extends와 plugin 속성을 추가하세요.

<div class="content-ad"></div>

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:import/errors",
    "plugin:react/recommended",
    "plugin:jsx-a11y/recommended"
  ],
  "plugins": ["react", "import", "jsx-a11y"]
}
```

다양한 플러그인을 추가했기 때문에 먼저 아래 명령을 실행하여 해당 플러그인들을 devDependencies로 설치해야 합니다.

```js
npm install -D eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react
```

## 규칙 추가하기

<div class="content-ad"></div>

설정 목적으로 규칙을 사용합니다. 규칙의 오류 수준을 세 가지 다른 방법으로 설정할 수 있습니다.

- off 또는 0: 규칙을 비활성화합니다.
- warn 또는 1: 규칙을 경고로 설정합니다.
- error 또는 2: 규칙을 오류로 설정합니다.

우리의 구성 파일에 몇 가지 규칙을 추가해 봅시다. 위에 언급된 모든 규칙 목록에서 선택하여 다른 규칙을 추가할 수 있습니다.

```js
"rules": {
  "react/prop-types": 0,
  "indent": ["error", 2],
  "linebreak-style": 1,
  "quotes": ["error", "double"]
}
```

<div class="content-ad"></div>

## 린트 스크립트 추가

마지막으로, ESLint를 실행하기 위해 package.json의 "scripts" 속성에 몇 가지 명령을 추가합시다.

```js
"scripts": {
  "lint": "eslint \"src/**/*.{js,jsx}\"",
  "lint:fix": "eslint \"src/**/*.{js,jsx}\" --fix"
}
```

축하해요! 이제 이 명령 중 하나를 실행해보면 잘 동작할 거에요!

<div class="content-ad"></div>

이제 이후에 코드 일관성과 품질을 보장하기 위해 린팅 규칙을 원하는 대로 사용자 정의할 수 있어요.

Junior Software Engineer이시면 Junior to Senior Software Engineer: Helpful Tips 라는 기사를 꼭 확인해보세요!