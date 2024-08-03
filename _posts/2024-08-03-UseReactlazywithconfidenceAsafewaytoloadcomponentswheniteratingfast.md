---
title: "빠른 반복 시에도 안전하게 컴포넌트를 로드하는 방법 Reactlazy 자신 있게 사용하기"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 19:28
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Use Reactlazy with confidence A safe way to load components when iterating fast"
link: "https://dev.to/logto/use-reactlazy-with-confidence-a-safe-way-to-load-components-when-iterating-fast-1gkh"
---


React.lazy는 컴포넌트를 필요할 때 로드하는 좋은 방법이며 앱의 성능을 향상시킵니다. 하지만 때로는 "ChunkLoadError"와 "Loading chunk failed" 같은 문제가 발생할 수 있습니다.

# 딜레마

요즘에는 소프트웨어 개발이 인기있는 "빨리 움직이고 문제를 발생시키자" 철학 아래에서 더 빠르게 진행되고 있습니다. 여기서 판단은 하지 않을 거예요 - 그냥 상황이 그렇다는 거죠. 하지만 이 빠른 진행 속도는 때로는 문제를 일으킬 수 있습니다, 특히 React에서 컴포넌트를 로드할 때 관련된 경우에는요.

React.lazy를 사용하여 컴포넌트를 필요할 때 로드하는 프로젝트를 진행 중이라면 ChunkLoadError와 Loading chunk failed와 같은 문제를 경험했을 수 있습니다. 가능한 이유는 다음과 같습니다:

<div class="content-ad"></div>

- 네트워크 문제가 발생할 수 있습니다. 예를 들어, 사용자의 인터넷 연결이 느리거나 불안정할 수 있습니다.
- 사용자가 앱의 구버전을 사용하고 있고, 브라우저가 더 이상 존재하지 않는 청크를로드하려고 시도합니다.

보통 페이지를 간단히 새로고침하면 문제가 해결될 수 있지만 사용자에게는 좋지 않은 경험이 될 수 있습니다. 사용자가 다른 경로로 이동하는 동안 하얀 화면이 나타나면 앱에 좋지 않은 모습이 될 수 있습니다.

속도와 부드러운 사용자 경험의 필요성을 균형있게 유지할 수 있을까요? 물론입니다. 저를 따라와보세요 (물론 TypeScript로!).

# 해결책

<div class="content-ad"></div>

포스트 서버에 청크 버전을 모두 저장하는 것은 브루트 포스 솔루션이 될 수 있습니다. 이렇게 함으로써 "누락된 청크" 문제가 해결될 수 있습니다. 그러나 앱이 성장함에 따라 이 솔루션은 디스크 공간 요구사항이 계속해서 증가하기 때문에 실현 불가능해질 수 있고, 네트워크 문제는 여전히 해결되지 않습니다.

다시 시도하거나 새로 고침을 통해 문제를 해결할 수 있다는 사실을 고려하면, 우리는 이러한 솔루션을 코드에 구현할 수 있습니다. 이 문제는 사용자가 다른 경로로 이동할 때 주로 발생하기 때문에, 사용자가 인지하지 못하게 문제를 해결할 수 있습니다. 우리가 해야 할 일은 React.lazy 함수 주변에 래퍼를 구축하여 다시 시도와 새로 고침을 처리하는 것뿐입니다.

이러한 종류의 솔루션을 구현하는 방법에 대한 좋은 기사들이 이미 있으므로, 나는 솔루션의 아이디어와 내부 작업에 초점을 맞출 것입니다.

### 래퍼 생성

<div class="content-ad"></div>

첫 번째 단계는 React.lazy 함수를 감싸는 래퍼를 만드는 것입니다:

```js
import { lazy, type ComponentType } from 'react';

// 정확한 타입 추론을 위해 제네릭을 사용하세요
const safeLazy = <T>(importFunction: () => Promise<{ default: ComponentType<T> }>) => {
  return lazy(async () => {
    return await importFunction();
  });
};
```

### 재시도 처리하기

네트워크 문제로 인한 경우, importFunction을 tryImport 함수로 감싸어 재시도를 처리할 수 있습니다:

<div class="content-ad"></div>

```js
const safeLazy = <T>(importFunction: () => Promise<{ default: ComponentType<T> }>) => {
  let retries = 0;
  const tryImport = async () => {
    try {
      return await importFunction();
    } catch (error) {
      // Retry 3 times max
      if (retries < 3) {
        retries++;
        return tryImport();
      }
      throw error;
    }
  };

  return lazy(async () => {
    return await tryImport();
  });
};
```

간단하죠? 재시도를 더 효율적으로 처리하기 위해 지수 백오프 알고리즘을 구현할 수도 있어요.

### 새로 고쳐 보세요

버전 오래된 문제를 해결하기 위해 오류를 잡고 페이지를 새로 고치는 방법도 있어요:

<div class="content-ad"></div>

```js
const safeLazy = <T>(importFunction: () => Promise<{ default: ComponentType<T> }>) => {
  // ...tryImport 함수 내용

  return lazy(async () => {
    try {
      const component = await tryImport();

      // 성공적으로 컴포넌트를 불러온 경우에 sessionStorage를 비웁니다.
      sessionStorage.removeItem('refreshed');

      return component;
    } catch (error) {
      if (!sessionStorage.getItem('refreshed')) {
        sessionStorage.setItem('refreshed', 'true');
        window.location.reload();
        return { default: () => null };
      }

      // 새로 고침 후에도 컴포넌트를 불러올 수 없는 경우에는 에러를 throw합니다.
      throw error;
    }
  });
};
```

이제 safeLazy 함수에서 에러를 catch할 때, 새로 고침으로는 해결할 수 없는 문제임을 알 수 있습니다.

<div class="content-ad"></div>

### 동일한 페이지에 여러 개의 지연 로드 구성 요소

현재 구현에서 아직도 숨겨진 함정이 있습니다. 동일한 페이지에 여러 개의 지연 로드 구성 요소가 있는 경우, refresh 무한 루프 문제가 발생할 수 있습니다. 다른 구성 요소가 sessionStorage 값을 재설정할 수 있기 때문입니다. 이 문제를 해결하기 위해 각 구성 요소마다 고유한 키를 사용할 수 있습니다:

```js
const safeLazy = <T>(importFunction: () => Promise<{ default: ComponentType<T> }>) => {
  // ...tryImport 함수

  // 키는 각 구성 요소마다 고유할 수 있습니다
  const storageKey = importFunction.toString();
  return lazy(async () => {
    try {
      const component = await tryImport();

      // 구성 요소가 성공적으로 로드되면 sessionStorage를 지웁니다
      sessionStorage.removeItem(storageKey);

      return component;
    } catch (error) {
      if (!sessionStorage.getItem(storageKey)) {
        sessionStorage.setItem(storageKey, 'true');
        window.location.reload();
        return { default: () => null };
      }

      // 새로 고침 후에도 구성 요소를 로드할 수 없는 경우 오류 발생
      throw error;
    }
  });
};
```

이제 각 구성 요소는 고유한 sessionStorage 키를 가지고 있으며, refresh 무한 루프가 피해집니다. 솔루션을 계속해서 세심하게 검토할 수 있습니다. 예를 들어:

<div class="content-ad"></div>

- 배열에서 모든 키를 수집하여 하나의 저장 키만 필요합니다.
- 오류가 발생하기 전에 페이지를 여러 번 새로 고칠 수 있는 새로 고침 제한을 설정합니다.

하지만 제 생각에는 이해하셨을 거라고 생각합니다. GitHub 저장소에서 테스트 및 구성이 완비된 TypeScript 솔루션이 제공됩니다. 또한, react-safe-lazy 패키지를 NPM에 게시했으므로 프로젝트에서 즉시 사용할 수 있습니다.

# 결론

소프트웨어 개발은 섬세한 작업이며, 가장 작은 세부 사항도 해결하기 위해 노력해야 합니다. 이 글이 React.lazy의 문제를 우아하게 처리하고 앱의 사용자 경험을 향상시키는 데 도움이 되기를 바랍니다.

<div class="content-ad"></div>

무료로 Logto Cloud를 사용해보세요!