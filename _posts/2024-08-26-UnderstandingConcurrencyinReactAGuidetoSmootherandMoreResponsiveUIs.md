---
title: "더 부드러운 웹 사이트를 위한 리액트에서 동시성 이해하기 "
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 20:05
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Understanding Concurrency in React A Guide to Smoother and More Responsive UIs"
link: "https://dev.to/rinonten/understanding-concurrency-in-react-a-guide-to-smoother-and-more-responsive-uis-1p70"
isUpdated: true
updatedAt: 1724742502401
---


웹 앱이 점점 복잡해지면 사용자들에게 빠르고 부드러운 경험을 제공하는 것이 어려울 수 있어요. 그래서 React에서 동시성이 필요한 거죠. React가 한 번에 여러 작업을 처리하도록 도와주어 앱이 더 잘 실행되고 반응이 더 빠르게 느껴질 수 있습니다.

## React에서 동시성이란 무엇인가요?

React에서 동시성의 간단한 정의는 React가 동시에 여러 일을 처리할 수 있다는 것이에요. 특히 새로운 기능인 자동 일괄 처리와 전환과 같은 기능은 앱이 더 부드럽게 느껴지도록 도와줍니다.

동시성 뒤에 숨겨진 주요 아이디어들을 살펴보겠습니다:

<div class="content-ad"></div>

1. 동시 렌더링:
동시 렌더링을 통해 React는 동시에 여러 업데이트를 처리할 수 있습니다. 사용자가 버튼을 클릭하거나 탭 간을 전환하는 등 더 중요한 작업이 발생했을 때, React는 하나의 업데이트를 일시 중지하거나 중단하여 더 중요한 작업에 집중할 수 있습니다. 예를 들어, 사용자가 실수로 한 탭을 클릭한 후 빠르게 다른 탭으로 전환한다면, React는 첫 번째 탭이 로딩을 마치기를 기다리지 않습니다. 대신 최신 탭으로 전환하여 앱이 빠르고 응답성이 유지됩니다. React 팀이 제공한 예제를 확인해보세요.

2. 자동 배치:
React 18에서 자동 배치 기능을 통해 React는 여러 상태 변경을 그룹화하고 한 번에 적용할 수 있습니다. 이는 각 개별 상태 변경마다 앱을 업데이트하는 대신 React가 이를 하나의 업데이트로 결합합니다. 이를 통해 렌더 수를 줄이고 앱을 더 효율적이고 빠르게 만들 수 있습니다.

예시:
- 양식 입력:
여러 입력란이 있는 양식을 가정해봅시다. 여러 입력값을 빠르게 변경하면 React가 이러한 변경 사항을 하나의 업데이트로 묶어줍니다. 자동 배치 기능이 없는 경우 각 입력값 변경마다 별도의 다시 렌더링을 발생시킬 수 있어 앱이 느려질 수 있습니다.

```js
function MyForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  const handleChange = (e) => {
    setName(e.target.value);
    setEmail('newemail@example.com');
  };

  return (
    <div>
      <input value={name} onChange={handleChange} />
      <input value={email} onChange={handleChange} />
    </div>
  );
}
```

<div class="content-ad"></div>

이 예제에서 setName과 setEmail은 함께 batched되어 React가 두 번이 아닌 한 번만 다시 렌더링됩니다.

b. 비동기 작업:
여러 비동기 작업을 처리할 때 데이터를 가져오고 결과에 따라 상태를 업데이트한다면, 자동 배칭은 이러한 업데이트를 하나의 렌더링으로 결합할 수 있습니다.

```js
import { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState(null); // 가져온 데이터를 저장할 상태
  const [loading, setLoading] = useState(true); // 로딩 상태를 추적하는 상태
  const [error, setError] = useState(null); // 에러 처리하는 상태
  const [retry, setRetry] = useState(false); // 재시도를 트리거하는 상태

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true); // 로딩 시작
      setError(null); // 이전 에러 지우기

      try {
        const response = await fetch('/api/data');

        if (!response.ok) {
          throw new Error('네트워크 응답이 올바르지 않습니다.');
        }

        const result = await response.json();
        setData(result); // 가져온 결과로 데이터 상태 업데이트
      } catch (error) {
        setError(error.message); // 실패하면 에러 상태 설정
      } finally {
        setLoading(false); // 로딩 종료
      }
    };

    fetchData();
  }, [retry]); // 의존성 배열에 retry가 포함되어 retry가 변경될 때 데이터를 다시 가져옵니다.

  const handleRetry = () => {
    setRetry(prevRetry => !prevRetry); // 재시도 상태를 토글하여 다시 불러오기 트리거
  };

  return (
    <div>
      {loading && <p>로딩 중...</p>}
      {error && <p>에러: {error}</p>}
      {data && <p>데이터: {JSON.stringify(data)}</p>}
      {!loading && !error && !data && <p>사용 가능한 데이터가 없습니다.</p>}
      {error && <button onClick={handleRetry}>재시도</button>}
    </div>
  );
}

export default DataFetcher;
```

데이터를 가져오는 과정에서 여러 상태 업데이트가 발생합니다.

<div class="content-ad"></div>

- fetch를 시작하기 전에 setLoading(true)를 호출하세요.
- 이전 오류를 지우기 위해 setError(null)를 호출하세요.
- fetch가 성공하면 setData(result)를 호출하세요.
- 에러가 발생하면 setError(error.message)를 호출하세요.
- 로딩이 완료되었음을 나타내기 위해 setLoading(false)를 호출하세요.

자동 일괄 처리는 이러한 업데이트를 단일 렌더링 주기로 결합합니다. 이는 React가 이러한 변경 사항을 한 번에 처리하여 여러 번의 렌더링을 피할 수 있음을 의미합니다.

3. 전환:
전환을 통해 일부 업데이트를 덜 긴급하게 표시할 수 있습니다. 예를 들어 새 페이지로 이동할 때 React는 새로운 페이지를 준비하는 동안 현재 페이지를 계속 표시할 수 있습니다. 이렇게 하면 애플리케이션이 더 부드럽게 느껴집니다.

전환의 예시:
a. 페이지 이동:
페이지 간 이동 시 React는 이동을 전환으로 표시할 수 있습니다. 이는 React가 새 페이지를 로드하고 준비하는 동안 현재 페이지를 계속 표시할 수 있음을 의미합니다.

<div class="content-ad"></div>

```js
import { useTransition, useState } from 'react';

function App() {
  const [isPending, startTransition] = useTransition();
  const [page, setPage] = useState('home');

  const goToPage = (newPage) => {
    startTransition(() => {
      setPage(newPage); // Mark page change as a transition
    });
  };

  return (
    <div>
      <button onClick={() => goToPage('home')}>Home</button>
      <button onClick={() => goToPage('about')}>About</button>
      <button onClick={() => goToPage('contact')}>Contact</button>

      {isPending ? <p>Loading...</p> : <PageContent page={page} />}
    </div>
  );
}

function PageContent({ page }) {
  if (page === 'home') return <p>Home Page</p>;
  if (page === 'about') return <p>About Page</p>;
  if (page === 'contact') return <p>Contact Page</p>;
  return null;
}
```

이 예제에서 다른 버튼을 클릭하면 페이지가 변경됩니다. React는 현재 페이지를 보이게 유지하면서 새 페이지를 준비하는 전환으로 처리합니다.

b. 목록 필터링:
목록에 필터를 적용할 때 필터링 작업을 전환으로 표시할 수 있습니다. 이렇게 하면 React가 백그라운드에서 새로운 필터 기준을 처리하고 적용하는 동안 현재 목록을 계속 표시할 수 있습니다.

```js
import { useTransition, useState } from 'react';

function ItemList() {
  const [isPending, startTransition] = useTransition();
  const [filter, setFilter] = useState('');
  const [items, setItems] = useState(allItems);
  const allItems = ['사과', '바나나', '체리', '데이트', '엘더베리'];

  const handleFilterChange = (e) => {
    const newFilter = e.target.value;
    startTransition(() => {
      setFilter(newFilter);
      const filteredItems = allItems.filter(item => item.includes(newFilter));
      setItems(filteredItems);
    });
  };

  return (
    <div>
      <input value={filter} onChange={handleFilterChange} placeholder="항목 필터링" />
      {isPending ? <p>필터링 중...</p> : <ul>{items.map(item => <li key={item}>{item}</li>)}</ul>}
    </div>
  );
}
```

<div class="content-ad"></div>

여기에서는 필터 입력란에 텍스트를 입력하면 React가 필터링 프로세스를 전환으로 표시합니다. 목록은 백그라운드에서 업데이트되며 UI는 반응성을 유지합니다.

4. Suspense:
Suspense를 사용하면 React가 데이터를 로드하는 동안 일부 UI를 일시 중지할 수 있습니다. 동시성을 사용하면 React가 여전히 다른 부분의 UI를 업데이트할 수 있으므로 사용자가 모든 것이 로드될 때까지 기다릴 필요가 없습니다.

예시: (React 문서에서)

```js
import { Suspense } from 'react';
import Albums from './Albums.js';

export default function ArtistPage({ artist }) {
  return (
    <>
      <h1>{artist.name}</h1>
      <Suspense fallback={<Loading />}>
        <Albums artistId={artist.id} />
      </Suspense>
    </>
  );
}

function Loading() {
  return <h2>🌀 Loading...</h2>;
}
```

<div class="content-ad"></div>

이 예시에서는 앨범 컴포넌트에서 아티스트 세부 정보를 가져오는 척을 합니다. 데이터를 가져오기 위한 프로미스를 해결하는 동안, UI에는 로딩 텍스트가 표시됩니다. 이것은 사용자들이 아티스트의 세부 정보가 로드될 때까지 검은 페이지/UI를 보지 못하게 하는 것을 의미합니다.

5. 우선순위 레벨:
React는 다른 작업에 다른 우선순위 레벨을 부여합니다. 사용자 입력과 같은 중요한 작업은 먼저 수행되고, 배경 데이터 로딩과 같은 중요하지 않은 작업은 나중에 수행됩니다. 이를 통해 앱이 빠르고 반응적인 상태를 유지할 수 있습니다.

React의 동시성은 앱이 더 복잡해지더라도 앱이 반응적이고 빠르게 유지할 수 있도록 도와줍니다. 이러한 기능을 사용하여 사용자 경험을 향상시키는 빠르고 부드러운 앱을 구축할 수 있습니다.

여기까지입니다! 항상 읽어주셔서 감사합니다. React에서의 동시성에 대해 더 많이 배우셨기를 바랍니다. 이번 포스트를 너무 길어지지 않도록 아직 다루지 않은 기능들이 더 있습니다.

<div class="content-ad"></div>

당신의 의견과 피드백은 저에게 소중합니다! 만약 제안 사항, 수정 사항 또는 개선 사항이 있으시면 자유롭게 공유해 주세요.