---
title: "npm 살펴보기 Node.js 패키지 관리자"
description: ""
coverImage: "/assets/img/2024-08-26-Gettingtoknownpmthenodejspackagemanager_0.png"
date: 2024-08-26 18:40
ogImage: 
  url: /assets/img/2024-08-26-Gettingtoknownpmthenodejspackagemanager_0.png
tag: Tech
originalTitle: "Getting to know npm  the nodejs package manager"
link: "https://medium.com/@wide4head/getting-to-know-npm-the-nodejs-package-manager-d0ee477f7571"
isUpdated: false
---


# npm이란 무엇인가요?

npm은 nodejs와 함께 설치되는 node 패키지 관리자로, nodejs 프로젝트의 종속성을 관리합니다. 이 글에서는 npm에 대해 알아보고, 사용하는 방법 및 사용법에 대해 최적의 사례를 살펴볼 것입니다.

# npm의 주요 구성 요소

## npm 레지스트리

<div class="content-ad"></div>

수많은 개발자와 기관에 의해 출판된 수천 개의 자바스크립트 패키지가 공개 데이터베이스에 있습니다. 다른 개발자들이 이 패키지를 자신의 노드제이에스 및 자바스크립트 프로젝트에 사용할 수 있습니다.

## npm CLI (Command Line Interface)

npm CLI 또는 시스템의 npm 명령어는 개발자들이 로컬 시스템에서 노드제이에스/자바스크립트 프로젝트를 관리할 수 있게 해주는 프로그램입니다. npm 레지스트리와 상호 작용하여 로컬 프로젝트나 시스템에 패키지를 가져오고 설치/업그레이드합니다. 의존성 관리의 여러 측면을 간단하게 해줍니다. 다음 섹션에서 자세한 내용을 살펴보겠습니다.

# npm 설치하기

<div class="content-ad"></div>

npm은 노드JS 패키지와 함께 제공되므로 노드JS를 설치할 때 기본적으로 설치됩니다.

노드JS를 설치한 후에는 npm -v를 실행하여 npm 명령어의 가용성을 확인할 수 있습니다.

![npm](/assets/img/2024-08-26-Gettingtoknownpmthenodejspackagemanager_0.png)

# npm 사용하기

<div class="content-ad"></div>

## 프로젝트 초기화하기

프로젝트를 초기화하는 데는 npm init을 사용합니다.

![이미지](/assets/img/2024-08-26-Gettingtoknownpmthenodejspackagemanager_1.png)

이 명령어를 실행하면 package.json 파일이 생성됩니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-08-26-Gettingtoknownpmthenodejspackagemanager_2.png" />

## 패키지 설치

특정 패키지를 프로젝트에 설치하려면 npm install `패키지_이름` 명령을 사용하세요.

<img src="/assets/img/2024-08-26-Gettingtoknownpmthenodejspackagemanager_3.png" />

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 바꿔주시면 됩니다.

<div class="content-ad"></div>

이렇게하면 패키지가 활성 유지 모드인지 또는 지원이 중단된 상태인지 알 수 있습니다.

## 패키지 제거하기

`package_name`을(를) 사용하여 필요하지 않은 패키지를 npm uninstall 명령어로 제거할 수 있습니다.

![이미지](/assets/img/2024-08-26-Gettingtoknownpmthenodejspackagemanager_5.png)

<div class="content-ad"></div>

# npm 구성 파일 — package.json

![이미지](/assets/img/2024-08-26-Gettingtoknownpmthenodejspackagemanager_6.png)

`package.json` 파일은 프로젝트 및 해당 종속성에 대한 중요한 정보를 포함합니다. script 필드는 테스트 실행 및 프로젝트 시작과 같은 일반 작업을 위한 바로 가기를 정의합니다.

# 종속성 관리

<div class="content-ad"></div>

## 의존성 추가

npm install `package_name` 명령어를 사용하여 패키지를 설치할 수 있습니다. 이 명령어는 패키지를 package.json에 추가하고 node_modules 하위 디렉토리에 패키지를 다운로드 및 설치합니다. --save 옵션을 사용하여 이를 수행하도록 할 수 있으나, 이것은 자동으로 수행됩니다.

## 의존성 업데이트

npm update 명령어를 사용하여 package.json 파일에 나열된 최신 버전의 패키지로 패키지를 업그레이드할 수 있습니다.

<div class="content-ad"></div>

## 의존성 제거

`package_name`을 제거하려면 npm uninstall을 사용할 수 있어요. 이 명령은 package.json 파일을 업데이트하고 node_modules 하위 디렉토리에서 해당 패키지를 제거할 거예요.

# 의미 있는 버전 관리

npm은 의미 있는 버전 관리를 위해 사용되며, major.minor.patch 형식으로 이루어져요. 예: 2.0.1

<div class="content-ad"></div>

주요: 중요 변경사항

마이너: 역호환성을 깨지 않고 새로운 기능 추가

패치: 버그 수정, 새로운 기능은 추가하지 않음

시맨틱 버전은 업데이트가 사용자 프로젝트를 실수로 깨뜨리지 않도록 보장합니다.

<div class="content-ad"></div>

# 자주 사용하는 npm 명령어

자주 사용되는 npm 명령어 몇 가지입니다.

npm install : package.json에 정의된 모든 종속성을 설치합니다.

npm outdated : 프로젝트에서 오래된 패키지를 나열합니다.

<div class="content-ad"></div>

npm audit 명령어: 프로젝트의 종속성에 대한 취약점을 스캔합니다.

npm publish 명령어: npm 레지스트리에 패키지를 게시합니다.

# 결론

npm을 잘 다루는 것은 백엔드 / 풀스택 개발자에게 필수가 되었습니다. 이 글이 npm을 전반적으로 이해하는 데 도움이 되었기를 바라며, 아래 댓글 섹션에 피드백을 자유롭게 남겨주세요.