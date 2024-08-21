---
title: "최대 성능을 위한 React 애플리케이션 최적화 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-21 18:50
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Optimizing React Applications for Maximum Performance"
link: "https://dev.to/surajondev/optimizing-react-applications-for-maximum-performance-5epm"
isUpdated: false
---


# 소개

지금까지 3년 이상 React 코드를 작성해 왔습니다. 그러나 처음에는 React 성능 최적화에 중점을 두지 않았습니다. 대부분의 경우 기술적 부채가 누적되어 성능을 최적화하는 것이 어려워졌습니다.

시작부터 최적화에 집중하는 것은 꽤 어렵지만, 주기적으로 최적화를 일정표에 포함시켜서 큰 기술적 부채를 피할 수 있습니다.

이제 React의 최적화 기술 몇 가지를 살펴보겠습니다. 이 기술은 코드를 작성하는 동안에 적용할 수 있습니다. 이는 다른 방법 대신 이 방법을 선택하는 문제입니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

그럼, 시작해 봅시다.

# 1. 대규모 목록 최적화

목록 렌더링은 React에서 컴포넌트가 있기 때문에 매우 흔합니다. 대규모 목록을 렌더링하는 것은 렌더링 속도가 느려지고 메모리 사용량이 증가할 수 있어 도전적입니다. 가상화는 이러한 문제를 처리하는 가장 좋은 방법입니다. 가상화는 단순히 보이는 목록만 렌더링하고 필요할 때 다른 항목들을 렌더링하는 방식입니다.

React Window와 React Virtualized는 목록을 가상화하는 데 인기 있는 라이브러리입니다. 이들은 뷰포트에서 보이는 항목만 렌더링하여 언제나 렌더링되는 DOM 노드의 수를 크게 줄입니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

React Window를 사용한 예제입니다:

```js
    import { FixedSizeList as List } from 'react-window';

    const MyList = ({ items }) => (
      <List
        height={500} // 컨테이너의 높이
        itemCount={items.length} // 전체 아이템 수
        itemSize={35} // 각 항목의 높이
        width={300} // 컨테이너의 너비
      >
        {({ index, style }) => (
          <div style={style}>
            {items[index]}
          </div>
        )}
      </List>
    );
```

# 2. useMemo

useMemo는 계산 결과를 기억하는 React 훅입니다. 따라서 의존성이 변경되지 않는 한 계산을 다시 처리하지 않습니다. 이것은 비용이 많이 드는 함수나 계산이 각 렌더링에서 다시 실행되면 안 되는 경우 성능을 최적화하는 데 유용할 수 있습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
useMemo 함수의 구문은 다음과 같습니다:

const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);

보시다시피, useMemo는 두 개의 인자를 사용합니다:

- 값을 반환하는 함수(왜냐하면 그 값이 기억되어야 하기 때문입니다).
- 기억된 값이 다시 계산되어야 하는 시기를 결정하는 의존성 배열입니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이런 방식으로 useMemo를 사용할 수 있습니다:

import React, { useState, useMemo } from 'react';

const ExpensiveComponent = ({ a, b }) => {
  const computeExpensiveValue = (a, b) => {
    console.log('Computing expensive value...');
    return a + b;
  };

  const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);

  return (
    <div>
      <p>Computed Value: {memoizedValue}</p>
    </div>
  );
};

const ParentComponent = () => {
  const [a, setA] = useState(1);
  const [b, setB] = useState(2);
  const [count, setCount] = useState(0);

  return (
    <div>
      <ExpensiveComponent a={a} b={b} />
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
    </div>
  );
};

# 3. 코드 분할

일반적인 설정에서는 응용프로그램의 모든 구성 요소가 하나의 파일로 번들링됩니다. 코드 분할은 응용프로그램을 작은 청크로 분해하는 최적화 기술입니다. 더 작은 구성 요소를 로드하고 필요하지 않은 다른 구성 요소를 피함으로써 응용프로그램의 로드 시간을 줄이며 최적화합니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

코드 분할(Code Splitting)의 예시입니다:

    import React, { useState } from 'react';

    function App() {
      const [component, setComponent] = useState(null);

      const loadComponent = async () => {
        const { default: LoadedComponent } = await import('./MyComponent');
        setComponent(<LoadedComponent />);
      };

      return (
        <div>
          <h1>코드 분할 예시</h1>
          <button onClick={loadComponent}>컴포넌트 불러오기</button>
          {component}
        </div>
      );
    }

    export default App;

## 4. React Lazy Load

React.Lazy는 컴포넌트 로딩을 최적화하는 데 중요한 메서드입니다. 이를 사용하면 컴포넌트를 지연 로딩할 수 있습니다. 즉, 해당 컴포넌트는 필요할 때만 로드됩니다. 이를 사용하면 애플리케이션을 작은 컴포넌트로 분할하고 필요할 때에만 로드할 수 있습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

React.lazy()는 컴포넌트를 동적으로 가져오는 데 사용됩니다. 해당 컴포넌트가 필요할 때 비동기적으로 로드되며 그 전까지는 대체 UI(로딩 스피너와 같은)가 표시될 수 있습니다.

다음은 Lazy Load의 예시입니다:

    import React, { Suspense } from 'react';

    const LazyComponent = React.lazy(() => import('./MyComponent'));

    const App = () => {
      return (
        <div>
          <h1>나의 앱</h1>
          <Suspense fallback={<div>Loading...</div>}>
            <LazyComponent />
          </Suspense>
        </div>
      );
    };

    export default App;

# 스로틀링과 디바운싱

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

리액트에 특정한 것이 아니라 함수를 호출할 때 발생하는 일반적인 프로그래밍 기법입니다. 쓰로틀링은 함수가 실행되는 빈도를 정의하는 기술입니다. 함수를 쓰로틀링하면 이벤트가 몇 번 발생했는지와 무관하게 지정된 시간 간격 내에 한 번만 실행됩니다. 예를 들어, 버튼 클릭에 쓰로틀링을 추가하여 버튼이 너무 자주 호출되지 않도록 합니다.

쓰로틀링 예제:

    import React, { useState } from 'react';

    function ThrottledButton() {
      const [count, setCount] = useState(0);

      const throttle = (func, delay) => {
        let lastCall = 0;
        return () => {
          const now = new Date().getTime();
          if (now - lastCall >= delay) {
            lastCall = now;
            func();
          }
        };
      };

      const incrementCount = () => {
        setCount((prevCount) => prevCount + 1);
      };

      const throttledIncrement = throttle(incrementCount, 2000);

      return (
        <div>
          <h1>Count: {count}</h1>
          <button onClick={throttledIncrement}>Click Me</button>
        </div>
      );
    }

    export default ThrottledButton;

디바운싱은 함수가 호출된 후 일정 시간이 지난 후에 실행되어야 함을 보장하는 데 사용됩니다. 이벤트가 반복적으로 발생할 때, 디바운스 함수는 이벤트가 지정된 지연 기간 동안 더 이상 발생하지 않은 후에만 실행됩니다. 예를 들어, 사용자가 검색 입력란에 타이핑하는 동안 일부 밀리초를 기다려 함수를 호출하여 사용자가 타이핑을 완료할 수 있도록 합니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

디바운싱의 예:

    import React, { useState } from 'react';

    function debounce(func, delay) {
      let timeoutId;
      return function (...args) {
        if (timeoutId) {
          clearTimeout(timeoutId);
        }
        timeoutId = setTimeout(() => {
          func(...args);
        }, delay);
      };
    }

    const DebouncedSearch = () => {
      const [query, setQuery] = useState('');

      const handleSearch = (event) => {
        setQuery(event.target.value);
        console.log('Searching for:', event.target.value);
        // 여기서는 일반적으로 쿼리를 기반으로 API 호출을 트리거하거나 목록을 필터링합니다.
      };

      const debouncedSearch = debounce(handleSearch, 500);

      return (
        <div>
          <h1>검색</h1>
          <input
            type="text"
            placeholder="검색하려면 입력하세요..."
            onChange={debouncedSearch}
          />
          <p>검색 쿼리: {query}</p>
        </div>
      );
    };

    export default DebouncedSearch;

# 함께 소통하세요

저와 연결하여 테크, 혁신 및 더 나아간 모든 것에 대해 최신 정보를 받아보세요! 🚀

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

- Twitter
- LinkedIn

또한, 문의 사항이 있으시면 이메일이나 소셜을 통해 연락하시면 됩니다.

# 결론

리액트 애플리케이션을 최적화하는 것은 중요합니다. 특히 애플리케이션이 복잡해지고 규모가 커질수록 더 중요합니다. 리스트 가상화, useMemo를 사용한 메모이제이션, 코드 분할, 레이지 로딩, 스로틀링, 디바운싱과 같은 기법을 활용하면 리액트 애플리케이션의 성능을 크게 향상시킬 수 있습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이 방법이 React 애플리케이션의 성능을 최적화하는 데 도움이 될 것을 기대합니다. 기사를 읽어주셔서 감사합니다!