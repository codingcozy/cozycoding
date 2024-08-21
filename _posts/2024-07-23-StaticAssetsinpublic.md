---
title: "Nextjs 14에서 public 폴더를 활용한 정적 자산 관리 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:42
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Static Assets in `public`"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/static-assets"
isUpdated: true
---




# `public` 폴더에 있는 정적 에셋

Next.js는 루트 디렉토리에서 public이라는 폴더 아래와 같이 이미지와 같은 정적 파일을 제공할 수 있습니다. public 내부의 파일은 기본 URL(/)을 기준으로 코드에서 참조할 수 있습니다.

예를 들어, public/avatars/me.png 파일은 /avatars/me.png 경로를 방문하여 볼 수 있습니다. 해당 이미지를 표시하는 코드는 다음과 같을 수 있습니다:

```js
import Image from 'next/image'

export function Avatar({ id, alt }) {
  return <Image src={`/avatars/${id}.png`} alt={alt} width="64" height="64" />
}

export function AvatarOfMe() {
  return <Avatar id="me" alt="A portrait of me" />
}
```

<div class="content-ad"></div>

## 캐싱

Next.js는 public 폴더에 있는 에셋을 안전하게 캐시할 수 없습니다. 왜냐하면 그들은 변할 수 있기 때문이죠. 적용된 기본 캐싱 헤더는 아래와 같습니다:

```js
Cache-Control: public, max-age=0
```

## 로봇, 파비콘 및 기타들

<div class="content-ad"></div>

정적 메타데이터 파일(예: robots.txt, favicon.ico 등)의 경우, 앱 폴더 내에 특별한 메타데이터 파일을 사용해야 합니다.

> 알아두면 좋은 사항:
디렉토리는 반드시 public로 이름지어야 합니다. 이름을 바꿀 수 없으며 정적 에셋을 제공하는 유일한 디렉터리입니다.
빌드 시점에 public 디렉토리 내에 있는 에셋만 Next.js가 제공합니다. 요청 시간에 추가된 파일은 사용할 수 없습니다. 지속적인 파일 저장을 위해 Vercel Blob과 같은 서드파티 서비스를 사용하는 것을 권장합니다.