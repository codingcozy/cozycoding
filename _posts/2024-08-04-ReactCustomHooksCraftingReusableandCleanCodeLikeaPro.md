---
title: "재사용 가능한 React 커스텀 훅을 만드는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-04 19:40
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "React Custom Hooks Crafting Reusable and Clean Code Like a Pro"
link: "https://dev.to/gboladetrue/react-custom-hooks-crafting-reusable-and-clean-code-like-a-pro-3kol"
isUpdated: true
updatedAt: 1724246234082
---


리액트 세계에서 훅은 함수형 컴포넌트에서 상태와 사이드 이펙트를 관리하는 방법을 혁신적으로 변화시켰어요. 그러나 응용 프로그램이 복잡해지면 다른 컴포넌트 간에 로직을 반복하는 경우가 종종 있어요. 여기서 커스텀 훅이 유용하게 사용될 수 있어요. 커스텀 훅을 사용하면 재사용 가능한 로직을 추출하여 컴포넌트를 보다 깔끔하고 유지보수하기 쉬운 상태로 만들 수 있어요.

이 게시물에서는 커스텀 훅을 만드는 방법과 코드 재사용성 및 추상화를 향상하는 실제 예제를 살펴보겠어요. 이 게시물을 최대한 활용하려면 리액트의 기본 개념에 대해 이미 알고 있는 것이 도움이 될 거예요. 함수형 컴포넌트, useState 및 useEffect와 같은 내장 훅의 사용, 그리고 프로미스와 비동기 작업과 같은 기본적인 JavaScript 개념 등에 익숙하다면 커스텀 훅을 따라가고 프로젝트에 구현하기 쉬울 거예요.

커스텀 훅을 사용하면 다음과 같은 이점이 있어요:

- 로직 캡슐화: UI 컴포넌트에서 로직을 분리하여 코드를 더 모듈식으로 만들고 관리하기 쉽게 만들어요.
- 로직 재사용: 여러 컴포넌트 사이에서 공통 로직을 공유하여 중복을 줄여요.
- 컴포넌트 단순화: 비지니스 로직을 훅으로 옮기면 컴포넌트가 UI 렌더링에 집중하도록 유지하면서 더 간결하게 유지할 수 있어요.

<div class="content-ad"></div>

## 커스텀 훅 만들기: 실용적인 예제

일반적인 사용 사례인 API에서 데이터 가져오고 관리하는 것을 생각해 봅시다. 우리는 데이터 가져오기, 로딩 상태, 그리고 오류를 처리하는 useFetch라는 커스텀 훅을 만들 것입니다.

### 단계 1: 기본 훅 구조 설정

```js
import { useState, useEffect } from 'react';

const useFetch = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error('네트워크 응답이 정상이 아닙니다.');
        }
        const result = await response.json();
        setData(result);
      } catch (error) {
        setError(error.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
};

export default useFetch;
```

<div class="content-ad"></div>

### 단계 2: 컴포넌트에서 사용자 지정 훅 사용하기

이제 우리가 만든 사용자 지정 useFetch 훅을 컴포넌트에서 사용해 봅시다.

```js
import React from 'react';
import useFetch from './useFetch';

const UserList = () => {
  const { data: users, loading, error } = useFetch('https://jsonplaceholder.typicode.com/users');

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};

export default UserList;
```

### 단계 3: 커스텀 훅 개선하기

<div class="content-ad"></div>

우리의 useFetch 훅을 더 다양하게 만들기 위해, 데이터를 필요에 따라 다시 가져올 수 있는 기능을 추가할 거예요. 사용자 액션이나 기타 이벤트에 응답하여 데이터를 새로 고칠 필요가 있는 경우에 유용할 수 있어요.

이를 위해 useEffect 훅에서 fetchData 함수를 분리하여 useCallback 훅으로 감쌀 거예요. 이 접근 방식은 두 가지 목적을 가지고 있어요:

- 재사용성: fetchData를 추출함으로써, 우리는 훅을 사용해 컴포넌트에서 직접 호출하여 필요할 때 데이터를 다시 가져올 수 있어요.
- 메모리 최적화: useCallback으로 fetchData를 감싸면, 의존성이 변경되지 않는 한 함수 인스턴스가 다시 렌더링되더라도 안정적으로 유지돼요. 이를 통해 함수를 불필요하게 반복해서 생성하지 않고, 메모리 사용량을 최적화하고 효과를 의도치 않게 다시 트리거하는 가능성을 줄일 수 있어요.

여기에 업데이트된 구현이 있어요:

<div class="content-ad"></div>

```js
import { useState, useEffect, useCallback } from 'react';

const useFetch = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const fetchData = useCallback(async (abortController) => {
    setLoading(true);
    try {
      const response = await fetch(url, {
        signal: abortController.signal
      });
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      const result = await response.json();
      setData(result);
    } catch (error) {
      if (error.name === 'AbortError') {
        console.log('Request was cancelled');
      } else {
        setError(error.message);
      }
    } finally {
      setLoading(false);
    }
  }, [url]);

  useEffect(() => {
    // AbortController is used to abort ongoing fetch requests when the component unmounts or the URL changes
    const abortController = new AbortController();

    fetchData(abortController);

    // Cleanup function to cancel the request when the component unmounts or the URL changes
    return () => {
      abortController.abort();
    };
  }, [fetchData]);

  return { data, loading, error, refetch: fetchData };
};

export default useFetch;
```

이제 useFetch 훅은 데이터를 다시 가져 오려면 호출할 수있는 refetch 함수를 제공합니다.

## 사용자 지정 훅이 이상적인 다른 사용 사례

여기에서 사용자 정의 훅이 특히 유익 할 수있는 3 가지 일반적인 사용 사례가 있습니다:

<div class="content-ad"></div>

- useLocalStorage: 세션 간에 지속되는 상태를 관리하는 것은 어려울 수 있습니다. useLocalStorage 커스텀 훅은 localStorage API에서 값 저장 및 검색 로직을 추상화하여이를 간단화할 수 있습니다. 이를 통해 구성 요소 상태를 로컬 스토리지와 동기화시키는 방법을 제공하여 사용자가 브라우저를 닫거나 새로 고침해도 데이터를 저장할 수 있도록 합니다.

예시 구현:

```js
// https://usehooks.com/useLocalStorage
import { useState } from 'react';

// Hook
function useLocalStorage(key, initialValue) {
  // 값을 저장할 상태
  // 초기 상태 함수를 useState에 전달하여 로직이 한 번만 실행되도록 함
  const [storedValue, setStoredValue] = useState(() => {
    if (typeof window === 'undefined') {
      return initialValue;
    }

    try {
      // 키로부터 로컬 스토리지에서 가져오기
      const item = window.localStorage.getItem(key);
      // 저장된 JSON을 구문 분석하거나 없는 경우 initialValue 반환
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      // 오류가 발생하면 initialValue도 반환
      console.log(error);
      return initialValue;
    }
  });

  // 새 값을 로컬 스토리지에 지속적으로 저장하는 useState의 setter 함수를 래핑된 버전으로 반환
  const setValue = (value) => {
    try {
      // 값을 함수로 사용하여 useState와 동일한 API를 가지도록 함
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      // 상태 저장
      setStoredValue(valueToStore);
      // 로컬 스토리지에 저장
      if (typeof window !== 'undefined') {
        window.localStorage.setItem(key, JSON.stringify(valueToStore));
      }
    } catch (error) {
      // 더 고급 구현은 오류 케이스를 처리할 것임
      console.log(error);
    }
  };

  return [storedValue, setValue];
}
```

- useWindowSize: 반응형 디자인을 처리하려면 종종 창 크기를 추적하여 레이아웃이나 요소를 동적으로 조정해야 합니다. useWindowSize 훅은 창 크기 변경 이벤트를 감지하고 반응하여 대응하기 위한 로직을 추상화하여 반응형 UI 요소를 구현하기 쉽게합니다.


<div class="content-ad"></div>

예시 구현:

```js
import { useState, useEffect } from 'react';

const useWindowSize = () => {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  useEffect(() => {
    const handleResize = () => {
      setSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    };
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return size;
};
```

- useDebounce: useDebounce 훅은 일정 기간 후에 함수 실행을 지연시켜주어, 일반적으로 연속해서 비싼 작업을 여러 번 호출하는 것을 피하는 데 유용합니다. 이는 사용자가 입력을 멈출 때까지 기다려 API 호출을 수행하고 싶은 검색 입력 처리에 특히 유용합니다.

예시 구현:

<div class="content-ad"></div>

```js
import { useState, useEffect } from 'react';

const useDebounce = (value, delay) => {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);

  return debouncedValue;
};

// 컴포넌트에서 사용
import { useDebounce } from './useDebounce';

const SearchComponent = () => {
   const [searchTerm, setSearchTerm] = useState('')
   const debouncedSearchTerm = useDebounce(searchTerm, 500)  // 사용자가 입력을 멈춘 후 500ms 뒤에만 API 호출됨!

   useEffect(() => {
      if(debouncedSearchTerm) {
         // API 호출 수행
      }
   }, [debouncedSearchTerm])

   return <input onChange={e => setSearchTerm(e.target.value)} />
}
```

각 커스텀 훅은 특정 문제를 해결하며, 공통 로직을 재사용 가능한 컴포넌트로 추상화하여 코드를 크게 단순화할 수 있습니다. 이로 인해 중복이 감소하며 React 애플리케이션의 명확성과 유지 보수성이 향상됩니다.

## 커스텀 훅에 대한 최상의 관행

- use 접두사 사용: 커스텀 훅의 이름이 use로 시작하도록하여 React 관례를 따르고 훅 규칙을 활성화합니다.
- 훅을 집중화: 커스텀 훅은 한 가지 일을 잘 처리해야 합니다. 너무 많은 로직을 과다하게 집어넣지 않도록 합니다.
- 정리 처리: useEffect 정리 함수를 사용하여 네트워크 요청 취소와 같은 필요한 정리 작업을 처리합니다.


<div class="content-ad"></div>

## 결론

커스텀 훅은 React에서 강력한 도구로, 더 깔끔하고 유지보수가 쉬운 코드를 작성하는 데 도움을 줄 수 있습니다. 재사용 가능한 훅으로 로직을 캡슐화하면 컴포넌트가 UI 렌더링에만 집중하고 애플리케이션 확장성을 향상시킬 수 있습니다.

위에서 소개한 훅들을 자유롭게 사용하고 확장해보세요. 다음 프로젝트에서 커스텀 훅을 만들어보고 코드를 간소화하는 데 어떻게 도움이 되는지 확인해보세요! 다양한 유용한 훅을 제공하는 useHooks도 살펴보세요. 또한 React 19를 주목해주세요. 2024 컨퍼런스가 최근에 열렸고 새로운 훅들이 놀라울 정도로 좋았답니다. 다음에 또 만나요. 혹시 시간이 맞으면...

![Placeholder](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExZzZpNTF5djZjZHRhZWxvN3JyZmJiNXlvODhyZ2tkdDcwcHNrcHl5bSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/5ziLJimPfiiQ3yrZIJ/giphy.gif)