---
title: "Nextjs 14 및 Nextjs App Router - 꼭 알아야 할 패턴과 모범 사례"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:19
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Patterns and Best Practices"
link: "https://nextjs.org/docs/app/building-your-application/data-fetching/patterns"
isUpdated: true
---




# 패턴 및 권장 사항

React와 Next.js에서 데이터를 가져오는 데 권장되는 몇 가지 패턴과 최상의 방법이 있습니다. 이 페이지에서는 가장 일반적인 패턴 몇 가지와 그 사용 방법에 대해 알아볼 것입니다.

## 서버에서 데이터 가져오기

가능한 경우, 서버 컴포넌트를 사용하여 서버에서 데이터를 가져오는 것을 권장합니다. 이를 통해 다음과 같은 이점을 얻을 수 있습니다:

<div class="content-ad"></div>

- 백엔드 데이터 리소스에 직접 액세스할 수 있습니다(예: 데이터베이스).
- 클라이언트에 민감한 정보(액세스 토큰 및 API 키와 같은)가 노출되는 것을 방지하여 응용 프로그램을 더 안전하게 유지할 수 있습니다.
- 동일한 환경에서 데이터를 가져와 렌더링합니다. 이렇게 하면 클라이언트와 서버 간의 왕복 통신 및 클라이언트의 주 쓰레드에서의 작업이 줄어듭니다.
- 클라이언트에서 개별 요청이 아니라 단일 왕복 통신으로 여러 데이터를 가져올 수 있습니다.
- 클라이언트-서버 간의 지연을 줄입니다.
- 지역에 따라 데이터 가져오기가 데이터 원본에 가까운 곳에서 처리되어 지연 시간이 줄고 성능이 향상될 수도 있습니다.

그런 다음, Server Actions로 데이터를 변형하거나 업데이트할 수 있습니다.

## 필요한 곳에서 데이터 가져오기

트리 내의 여러 구성 요소에서 동일한 데이터(예: 현재 사용자)를 사용해야 하는 경우, 전역으로 데이터를 가져올 필요가 없으며 구성 요소 간에 props를 전달할 필요도 없습니다. 대신, 동일한 데이터에 대해 여러 요청을 만들어 성능에 영향을주는 문제에 대해 걱정할 필요없이, 데이터가 필요한 컴포넌트에서 fetch 또는 React 캐시를 사용할 수 있습니다.

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해 드릴게요.

<div class="content-ad"></div>

서버 컴포넌트 및 중첩 레이아웃을 사용하면 데이터가 명시적으로 필요하지 않은 페이지 부분을 즉시 렌더링할 수 있으며, 데이터를 가져오는 페이지 부분에는 로딩 상태를 표시할 수 있습니다. 이는 사용자가 전체 페이지가 완전히 로드될 때까지 기다릴 필요가 없음을 의미합니다.

<img src="/assets/img/2024-07-23-PatternsandBestPractices_0.png" />

스트리밍 및 서스펜스에 대해 더 알아보려면 로딩 UI 및 스트리밍 및 서스펜스 페이지를 참조하세요.

## 병렬 및 순차적 데이터 가져오기

<div class="content-ad"></div>

리액트 컴포넌트 내에서 데이터를 가져올 때, 두 가지 데이터 가져오기 패턴인 병렬(Parallel) 및 순차(Sequential)에 대해 알아야 합니다.

![이미지](/assets/img/2024-07-23-PatternsandBestPractices_1.png)

- 순차 데이터 가져오기는 루트(route) 내의 요청들이 서로 의존하고 있어서 워터폴(waterfall)을 생성합니다. 다른 요청의 결과에 의존하거나 다음 요청 전에 조건을 충족시키길 원하는 경우가 있을 수 있습니다. 그러나 이 행동은 의도치 않게 발생할 수도 있고 데이터 로딩 시간이 더 길어질 수도 있습니다.
- 병렬 데이터 가져오기는 루트 내의 요청들이 즉시 시작되어 동시에 데이터를 로드합니다. 이는 클라이언트-서버 간의 워터폴을 줄이고 데이터 로딩에 소요되는 총 시간을 줄입니다.

### 순차 데이터 가져오기

<div class="content-ad"></div>

중첩 구성 요소가 있는 경우 각 구성 요소가 자체 데이터를 가져오면, 데이터 가져오기는 서로 다른 경우 순차적으로 발생합니다 (동일한 데이터에 대한 요청은 자동으로 메모이제이션되므로 해당 사항이 해당되지 않습니다).

예를 들어, Playlists 구성 요소는 artistID 프롭에 종속되기 때문에 Playlists 구성 요소는 Artist 구성 요소가 데이터를 가져 오기를 완료 할 때에만 데이터 가져 오기를 시작합니다.

```js
// ...

async function Playlists({ artistID }: { artistID: string }) {
  // 플레이리스트를 기다립니다.
  const playlists = await getArtistPlaylists(artistID);
 
  return (
    <ul>
      {playlists.map((playlist) => (
        <li key={playlist.id}>{playlist.name}</li>
      ))}
    </ul>
  );
}
 
export default async function Page({
  params: { username },
}: {
  params: { username: string }
}) {
  // 아티스트를 기다립니다.
  const artist = await getArtist(username);
 
  return (
    <>
      <h1>{artist.name}</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <Playlists artistID={artist.id} />
      </Suspense>
    </>
  );
}
```

이와 같은 경우에는 route 세그먼트에는 loading.js(라우트 세그먼트용) 또는 중첩 구성 요소용으로 React `Suspense`를 사용하여 React가 결과를 스트리밍하는 동안 즉시 로딩 상태를 표시할 수 있습니다.

<div class="content-ad"></div>

이렇게 하면 데이터 가져오기가 전체 경로를 차단하는 것을 방지하여 사용자가 차단되지 않은 페이지 부분과 상호 작용할 수 있습니다.

> 데이터 요청 차단:
폭포 모양이 생기지 않도록 하는 대안적인 방법은 애플리케이션의 루트에서 전역으로 데이터를 가져오는 것이지만, 이 방법은 데이터 로딩이 완료될 때까지 그 아래의 모든 경로 세그먼트에 대한 렌더링을 차단합니다. 이를 "모두 또는 아무것도" 데이터 가져오기라고 설명할 수 있습니다. 페이지나 애플리케이션의 전체 데이터를 가지거나 아무 것도 가지지 않습니다.
`await`를 사용하는 모든 요청은 렌더링 및 데이터 가져오기를 해당 요청을 포함하는 트리 전체에 대해 차단합니다. `Suspense` 경계 또는 `loading.js`를 사용하지 않는 한 이들은 포장되어야 합니다. 병렬 데이터 가져오기 또는 사전 로드 패턴을 사용하는 다른 대안이 있습니다.

### 병렬 데이터 가져오기

병렬로 데이터를 가져오려면 데이터를 사용하는 구성 요소 외부에서 정의하여 요청을 즉시 시작한 다음 구성 요소 내부에서 호출할 수 있습니다. 이렇게 하면 두 요청을 병렬로 시작하여 시간을 절약할 수 있지만, 두 프로미스가 모두 해결될 때까지 사용자가 렌더링된 결과를 보지 못할 수 있습니다.

<div class="content-ad"></div>

아래 예제에서는 getArtist와 getArtistAlbums 함수가 Page 컴포넌트 외부에서 정의되고 컴포넌트 내에서 호출되며, 두 프로미스가 해결될 때까지 기다립니다:

```js
import Albums from './albums'
 
async function getArtist(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}`)
  return res.json()
}
 
async function getArtistAlbums(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}/albums`)
  return res.json()
}
 
export default async function Page({
  params: { username },
}: {
  params: { username: string }
}) {
  // 두 요청을 병렬로 시작합니다
  const artistData = getArtist(username)
  const albumsData = getArtistAlbums(username)
 
  // 프로미스가 해결될 때까지 기다립니다
  const [artist, albums] = await Promise.all([artistData, albumsData])
 
  return (
    <>
      <h1>{artist.name}</h1>
      <Albums list={albums}></Albums>
    </>
  )
}
```

사용자 경험을 향상시키기 위해 렌더링 작업을 분할하고 가능한 빨리 일부 결과를 표시하기 위해 Suspense Boundary를 추가할 수 있습니다.

## 데이터 미리 불러오기

<div class="content-ad"></div>

다른 방법으로 폭포를 방지하는 방법은 preload 패턴을 사용하는 것입니다. 병렬 데이터 가져 오기를 더 최적화하기 위해 preload 함수를 선택적으로 생성할 수 있습니다. 이 접근 방식을 사용하면 프라미스를 props로 전달할 필요가 없습니다. preload 함수의 이름은 패턴이므로 임의로 지정할 수 있습니다.

```js
import { getItem } from '@/utils/get-item'

export const preload = (id: string) => {
  // void는 주어진 표현식을 평가하고 undefined를 반환합니다
  // https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void
  void getItem(id)
}
export default async function Item({ id }: { id: string }) {
  const result = await getItem(id)
  // ...
}
```

```js
import Item, { preload, checkIsAvailable } from '@/components/Item'

export default async function Page({
  params: { id },
}: {
  params: { id: string }
}) {
  // 아이템 데이터 로딩 시작
  preload(id)
  // 다른 비동기 작업 수행
  const isAvailable = await checkIsAvailable()

  return isAvailable ? <Item id={id} /> : null
}
```

### React 캐시, 서버 전용 및 Preload 패턴 사용하기

<div class="content-ad"></div>

캐시 기능, 사전로드 패턴 및 서버 전용 패키지를 결합하여 앱 전반에 사용할 수 있는 데이터 가져오기 유틸리티를 만들 수 있어요.

Markdown 형식으로 테이블 태그를 변경하고 싶다면 이렇게 하면 돼요.

```js
import { cache } from 'react'
import 'server-only'

export const preload = (id: string) => {
  void getItem(id)
}

export const getItem = cache(async (id: string) => {
  // ...
})
```

이 접근 방식을 사용하면 데이터를 즉시 가져오고 응답을 캐시하며, 데이터 가져오기가 서버에서만 발생하도록 보장할 수 있어요.

utils/get-item을 레이아웃, 페이지 또는 다른 구성 요소에서 사용하여 항목 데이터를 가져오는 시기를 제어할 수 있어요.

<div class="content-ad"></div>

> 좋은 정보:
서버 데이터 가져오기 함수가 클라이언트에서 사용되지 않도록 서버 전용 패키지를 사용하는 것을 권장합니다.

## 클라이언트에 민감한 데이터 노출 방지

우리는 React의 taint API, taintObjectReference와 taintUniqueValue를 사용하여 전체 객체 인스턴스나 민감한 값이 클라이언트에 전달되는 것을 방지하는 것을 권장합니다.

애플리케이션에서 tainting을 활성화하려면 Next.js Config experimental.taint 옵션을 true로 설정하세요:

<div class="content-ad"></div>


module.exports = {
  experimental: {
    taint: true,
  },
}


그런 다음 experimental_taintObjectReference 또는 experimental_taintUniqueValue 함수에 오염시키고자 하는 객체 또는 값을 전달하십시오:

```javascript
import { queryDataFromDB } from './api'
import {
  experimental_taintObjectReference,
  experimental_taintUniqueValue,
} from 'react'

export async function getUserData() {
  const data = await queryDataFromDB()
  experimental_taintObjectReference(
    '클라이언트에 전체 사용자 객체를 전달하지 마세요',
    data
  )
  experimental_taintUniqueValue(
    '클라이언트에 사용자의 주소를 전달하지 마세요',
    data,
    data.address
  )
  return data
}
```

```javascript
import { getUserData } from './data'

export async function Page() {
  const userData = getUserData()
  return (
    <ClientComponent
      user={userData} // taintObjectReference 때문에 오류가 발생합니다
      address={userData.address} // taintUniqueValue 때문에 오류가 발생합니다
    />
  )
}
```

<div class="content-ad"></div>

보안 및 서버 작업에 대해 더 알아보세요.