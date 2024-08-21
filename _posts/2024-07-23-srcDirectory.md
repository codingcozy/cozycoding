---
title: "Nextjs 14 src 디렉토리 활용법 최신 기능 총정리"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:48
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "src Directory"
link: "https://nextjs.org/docs/app/building-your-application/configuring/src-directory"
isUpdated: true
---




# src 디렉토리

당신의 프로젝트 루트에 특별한 Next.js 앱 또는 페이지 디렉토리를 갖는 대신, Next.js는 또한 일반적인 패턴인 애플리케이션 코드를 src 디렉토리 아래에 배치하는 것을 지원합니다.

이렇게 하면 대부분 프로젝트 루트에 위치하는 프로젝트 구성 파일과 애플리케이션 코드가 분리되어 관리됩니다. 개인 또는 팀에서 선호하는 방식입니다.

src 디렉토리를 사용하려면, app Router 폴더나 pages Router 폴더를 각각 src/app 또는 src/pages로 이동시키세요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-23-srcDirectory_0.png" />

> 유용한 정보
/public 디렉토리는 프로젝트의 루트에 유지해야 합니다.
package.json, next.config.js 및 tsconfig.json과 같은 구성 파일은 프로젝트의 루트에 유지해야 합니다.
.env.* 파일은 프로젝트의 루트에 유지해야 합니다.
src/app 또는 src/pages는 루트 디렉토리에 app 또는 pages가 있는 경우 무시됩니다.
src를 사용하는 경우, /components 또는 /lib와 같은 다른 애플리케이션 폴더를 이동해야 할 수 있습니다.
Middleware를 사용하는 경우 src 디렉토리 내에 배치해야 합니다.
Tailwind CSS를 사용하는 경우 tailwind.config.js 파일의 content 섹션에 /src 접두사를 추가해야 합니다.
TypeScript 경로를 사용하여 @/*와 같이 가져오는 경우, tsconfig.json의 paths 객체를 업데이트하여 src/을 포함해야 합니다.