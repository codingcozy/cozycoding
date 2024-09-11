---
title: "신입 개발자가 알아야 할 React.js 팁 및 기능 11가지"
description: ""
coverImage: "/assets/img/2024-09-10-11UsefulReactjsHackEveryDeveloperShouldKnow_0.png"
date: 2024-09-10 18:31
ogImage: 
  url: /assets/img/2024-09-10-11UsefulReactjsHackEveryDeveloperShouldKnow_0.png
tag: Tech
originalTitle: "11 Useful React.js Hack Every Developer Should Know"
link: "https://medium.com/javascript-in-plain-english/11-useful-react-js-hack-every-developer-should-know-b635c3af48bd"
isUpdated: true
updatedAt: 1726022542781
---


React JS는 웹 프론트엔드 개발을 위해 널리 사용되는 프레임워크 중 하나에요. 많은 새로운 학습자들이 React JS로 경력을 시작하고 있어요. 애플리케이션 성능을 향상시키고 코드 품질을 향상시킬 수 있는 몇 가지 최상의 실천 방법을 알아두는 것이 중요해요.

그래서 여기에는 React 개발자라면 반드시 알아야 할 11가지 React JS 팁과 코드 예제가 있어요—

## 1. 함수형 컴포넌트 사용하기

클래스 컴포넌트와 비교했을 때, 함수형 컴포넌트는 더 간단하고 간결해요. 함수형 컴포넌트가 제공하는 여러 장점들이 있어요 — 재사용성, 간결성, 성능, 테스트, 그리고 함수형 프로그래밍.

<div class="content-ad"></div>

- 재사용성: 함수 컴포넌트를 작은 재사용 가능한 구성 요소로 쉽게 추출할 수 있습니다. 또한 로직을 별도의 함수로 추출하여 여러 곳에서 재사용할 수 있습니다.
- 간결성: 함수 컴포넌트는 읽고 구현하기 쉽고 더 적은 코드 줄이 필요합니다.
- 성능: 리액트 훅을 사용하면 함수 컴포넌트가 더 최적화된 상태 관리 및 부작용을 제공합니다. 이로 인해 클래스 컴포넌트보다 우수한 성능을 제공합니다.
- 테스트: 내부 상태와 생명주기 메서드가 없기 때문에 함수 컴포넌트를 테스트하는 것이 쉽습니다. 이들은 프롭을 기반으로 간단한 입출력을 사용합니다.
- 함수형 프로그래밍: 함수 컴포넌트는 함수형 프로그래밍을 제공하여 응용 프로그램을 더 모듈식으로 만들고 읽고 디버깅하기 쉽게 도와줍니다.

## 2. 외부 div 대신 React 프래그먼트 사용

<img src="/assets/img/2024-09-10-11UsefulReactjsHackEveryDeveloperShouldKnow_0.png" />

요소를 ``Fragment`` 안에 래핑하여 그룹화한 다음 반환할 수 있습니다. 이렇게 하면 불필요한 div 사용을 방지할 수 있습니다. 프래그먼트는 인라인 블록인 div에 영향을 주지 않으므로 UI가 깨지지 않습니다.

<div class="content-ad"></div>

예시:

```js
// 프래그먼트 없이
 return (
   <div>
     <Header />
     <Content />
   </div>
 );

// 프래그먼트 사용 시
 return (
   <>
     <Header />
     <Content />
   </>
 );
```

![React.js useful hack](/assets/img/2024-09-10-11UsefulReactjsHackEveryDeveloperShouldKnow_1.png)

## 3. `useMemo`로 비용이 많이 드는 계산을 메모이제하기

<div class="content-ad"></div>

React JS에서 `useMemo` 훅을 사용하여 계산을 최적화할 수 있습니다. 이 훅은 함수의 계산 결과를 기억하고 캐시에서 다시 계산하는 대신 반환하여 다시 계산하는 것을 방지합니다. 이를 통해 다시 계산을 줄일 수 있습니다.

```js
import React, { useMemo } from 'react';

const ExpensiveComponent = ({ day }) => {
  const expensiveCalculation = useMemo(() => {
    return day * 24 * 60 * 60;
  }, [day]);

  return <div>Result: {expensiveCalculation}</div>;
};

export default ExpensiveComponent;
```

## 4. 안정적인 함수에 `useCallback` 사용하기

![image](/assets/img/2024-09-10-11UsefulReactjsHackEveryDeveloperShouldKnow_2.png)

<div class="content-ad"></div>

`useCallback` 훅은 함수를 메모이징하여 컴포넌트의 렌더링을 줄이는 데 도움이 됩니다.

`useCallback`은 함수 객체를 저장하므로 부모 컴포넌트가 다시 렌더링될 때 동일한 콜백 객체가 자식 컴포넌트로 전달됩니다. 이렇게 함으로써 자식 컴포넌트가 다시 렌더링되는 것을 방지하고 성능을 향상시키며 더 부드러운 사용자 경험을 만들어줍니다[출처]

```jsx
import React, { useCallback } from 'react';

const ParentComponent = () => {
  const handleClick = useCallback(() => {
    console.log('버튼이 클릭되었습니다');
  }, []);

  return <ChildComponent onClick={handleClick} />;
};

export default ParentComponent;
```

## 5. 컴포넌트의 지연 로딩

<div class="content-ad"></div>


![Lazy Loading](/assets/img/2024-09-10-11UsefulReactjsHackEveryDeveloperShouldKnow_3.png)

레이지 로딩은 애플리케이션 성능을 향상시키기 위해 따라야 할 최고의 디자인 패턴 중 하나입니다. React.lazy와 Suspense는 리액트 컴포넌트를 로딩할 때 레이지 로딩을 구현하는 데 사용됩니다.

```js
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}

export default App;
```

## 6. `PropTypes`를 사용하여 타입 체크하기


<div class="content-ad"></div>

<img src="/assets/img/2024-09-10-11UsefulReactjsHackEveryDeveloperShouldKnow_4.png" />

React JS에서 PropType은 프로퍼티에 전달된 데이터 유형을 유효성 검사하는 데 사용되는 메서드로, 초기 버그를 잡는 데 도움이 됩니다. React propTypes는 전달된 프로퍼티의 데이터를 유효성 검사하여 예상되는 값의 유형을 정의합니다.

Prop Type 설치 방법

```js
npm i prop-types
```

<div class="content-ad"></div>

```js
import React from 'react';
import PropTypes from 'prop-types';

const Button = ({ label }) => (
  <button>{label}</button>
);

Button.propTypes = {
  label: PropTypes.string.isRequired,
};

export default Button;
```

## 7. Complex State Management using `useReducer`

![UseReducer](/assets/img/2024-09-10-11UsefulReactjsHackEveryDeveloperShouldKnow_5.png)

Instead of using multiple `useState`, we can use `useReducer` to manage complex states. Check out the example below 👇


<div class="content-ad"></div>

```js
import React, { useReducer } from 'react';

const initialState = { count: 0 };

const reducer = (state, action) => {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <span>{state.count}</span>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
    </>
  );
};

export default Counter;
```

## 8. JSX에서 인라인 함수 사용을 피하자

JSX에서 인라인 함수 사용을 피하는 것은 좋은 습관으로 여겨지며 불필요한 다시 렌더링을 줄일 수 있습니다.

```js
import React, { useCallback } from 'react';

const Parent = () => {
  const handleClick = useCallback(() => {
    console.log('Clicked');
  }, []);

  return <button onClick={handleClick}>Click Me</button>;
};

export default Parent;
```

<div class="content-ad"></div>

## 9. `React.memo`을 사용하여 성능 최적화하기

![image](/assets/img/2024-09-10-11UsefulReactjsHackEveryDeveloperShouldKnow_6.png)

부품이 변하지 않을 때 컴포넌트를 다시 읽지 않으려면 React.memo를 사용하여 컴포넌트를 감싸면 됩니다.

```js
import React from 'react';
import PropTypes from 'prop-types';

const ChildComponent = React.memo(({ name }) => {
  console.log('Rendering Child');
  return <div>{name}</div>;
});

ChildComponent.propTypes = {
  name: PropTypes.string.isRequired,
};

ChildComponent.displayName = 'ChildComponent';

const Parent = () => {
  return <ChildComponent name="John" />;
};

export { ChildComponent, Parent };
```

<div class="content-ad"></div>

## 10. 글로벌 상태 관리를 위해 `Context` 사용하기

React JS에서 프롭 드릴링 없이 글로벌 상태를 관리하기 위해 Context API를 사용할 수 있습니다.

```js
const UserContext = React.createContext();

const App = () => {
  const [user, setUser] = useState("John Doe");

  return (
    <UserContext.Provider value={user}>
      <Profile />
    </UserContext.Provider>
  );
};

const Profile = () => {
  const user = useContext(UserContext);
  return <div>사용자 이름: {user}</div>;
};
```

## 11. 에러 처리를 위해 `Error Boundaries` 사용하기

<div class="content-ad"></div>

에러 경계를 사용하면 컴포넌트 트리에서 발생한 오류를 잡고 해결할 수 있어요.

```js
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError() {
    return { hasError: true };
  }
  
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

const App = () => (
  <ErrorBoundary>
     <MyComponent /> 
  </ErrorBoundary>
);
```

이러한 팁들을 통해 React 코드를 효율적이고 유지보수가 쉽게 작성할 수 있고 성능을 최적화하며 더 나은 사용자 경험을 제공할 수 있어요.

이 블로그 포스트를 읽어주셔서 감사합니다. 웹 개발에 관심이 있다면, 저의 트위터(X)와 Medium을 팔로우해보세요.

<div class="content-ad"></div>

# 쉽게 이해할 수 있는 영어 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

- 작가를 clapping하고 팔로우하고 ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 만나보세요