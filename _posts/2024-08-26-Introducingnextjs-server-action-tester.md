---
title: "Next.js 서버 액션 사용 방법 정리"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 17:32
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Introducing nextjs-server-action-tester"
link: "https://medium.com/@bijish.ob/introducing-nextjs-server-action-tester-6ab4e4123c5f"
isUpdated: true
updatedAt: 1724743437488
---


![image](https://miro.medium.com/v2/resize:fit:1400/1*eRV52WQT_MNLi6yoQqUZ5g.gif)

# nextjs-server-action-tester를 만든 이유 💡

최근 프로젝트에서 저는 Next.js를 사용하여 프론트엔드를 개발하고 동료는 백엔드 로직을 처리하였습니다. 핵심 Node.js 개발자인 동료는 API 엔드포인트를 테스트하기 위해 주로 Postman을 사용해왔습니다. 그러나 Next.js에서 서버 액션을 테스트하는 것은 전통적인 도구인 Postman과 같은 방법으로 시행하기 어려웠습니다.

이 도전은 Next.js 코드베이스를 스캔하고 모든 서버 액션을 식별하며 이러한 액션들을 테스트할 수 있는 사용자 인터페이스를 제공하는 도구를 만들기로 하는 아이디어를 불러일으켰습니다 🎯. 목표는 서버 액션의 테스트 프로세스를 간소화하는 것이었으며, 특히 전통적인 API 테스트 방법에 익숙한 개발자들을 위한 것이었습니다. 이로 인해 nextjs-server-action-tester가 탄생하게 되었습니다 🚀.

<div class="content-ad"></div>

# nextjs-server-action-tester이 무엇인가요? 🤖

nextjs-server-action-tester는 Next.js 프로젝트에서 서버 액션을 테스트하기를 가능한 간단하게 만들기 위해 설계된 npm 패키지입니다. 프로젝트를 스캔하는 프로세스를 자동화하고 메타데이터를 생성하여 사용자 친화적인 UI를 만들어 서버 함수를 나열, 검색 및 실행할 수 있게 합니다 🎨. 이 도구는 JavaScript와 TypeScript를 모두 지원하며 사용자 정의 구성 및 내장 라이트/다크 모드를 제공합니다.

# 주요 기능 ✨

- 자동 스캐닝: Next.js 프로젝트를 스캔하여 서버 액션을 식별합니다.
- 테스트용 UI: 프로젝트 내에서 페이지를 생성하여 서버 액션을 나열하고 테스트할 수 있습니다.
- 사용자 정의 구성: 도구가 프로젝트 내에서 어떻게 구성되고 사용되는지 유연성을 제공합니다.
- JavaScript 및 TypeScript 지원: 두 언어와 원활하게 작동합니다.
- 라이트/다크 모드: 선호하는 테마에 맞춰 UI가 적응됩니다.

<div class="content-ad"></div>

# 사용된 라이브러리 🛠️

- babel/parser: JavaScript/TypeScript 코드를 추상 구문 트리(AST)로 파싱하는 데 사용됩니다.
- babel/traverse: AST를 순회하고 서버 작업을 식별하는 데 사용됩니다.
- fs-extra: 설정 및 스캔 프로세스 중 파일 작업을 처리하는 데 사용됩니다.

# nextjs-server-action-tester 사용 방법 🚀

- 설치: 먼저 Next.js 프로젝트에서 패키지를 설치하세요:

<div class="content-ad"></div>

```js
npm install nextjs-server-action-tester
```

2. 도구 실행: 설치 후 다음 명령을 실행하여 스캔 및 테스트 프로세스를 시작합니다:

```js
npx actions-scan
```

이 명령은 필요한 파일을 자동으로 설정하고 스캔 프로세스를 시작하며 서버 액션 테스트를 위한 UI를 생성합니다.

<div class="content-ad"></div>

3. 서버 동작 테스트: 생성된 페이지를 브라우저에서 열어보세요 (예: http://localhost:3000/list-actions). 여기에서 모든 감지된 서버 동작을 볼 수 있습니다. 특정 동작을 검색하고 입력 매개변수를 실행할 수 있습니다 🔍.

# 개발자 경험 향상하는 이유 🚀

nextjs-server-action-tester는 Postman과 Next.js의 서버 동작 기능과 같은 전통적인 API 테스트 도구 간의 간극을 줄입니다. 특히 Node.js와 API 테스트에 익숙한 개발자들이 서버 동작을 쉽게 테스트할 수 있도록 돕습니다. 이 도구는 프로젝트에 직접 통합되어 전체적인 제어권을 제공하면서 개발 프로세스를 원활하고 효율적으로 유지합니다.

# 링크 🔗

<div class="content-ad"></div>

- npm 패키지: nextjs-server-action-tester
- 데모 비디오: YouTube에서 시청하세요

# 기여하기 🤝

기여를 환영합니다! 개선 아이디어가 있거나 문제를 발견하신 경우, 언제든지 풀 리퀘스트를 제출하거나 GitHub 저장소에서 이슈를 열어주세요.

이 도구를 공유함으로써, Next.js 프로젝트에서 서버 액션을 개발하고 테스트하는 것이 더욱 쉽고 효율적으로 이루어질 수 있기를 희망합니다. 혼자 프로젝트를 진행하거나 팀원들과 협업 중이던 중이던, nextjs-server-action-tester는 당신의 개발 툴킷에 가치 있는 도움이 될 수 있습니다 💼.