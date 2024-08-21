---
title: "5분 만에 이해하는 모든 React 개념"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 19:25
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Every React Concept Explained in 5 Minutes"
link: "https://dev.to/jitendrachoudhary/every-react-concept-explained-in-5-minutes-39b9"
isUpdated: true
updatedAt: 1724246398136
---


리액트는 몇 분 안에 프론트엔드 코드를 개발할 수 있게 해주는 자바스크립트 라이브러리입니다. 특정 작업을 수행하는 데 필요한 미리 만들어진 메서드와 함수가 있습니다. 리액트 라이브러리에는 조정, 상태, 속성 등과 같은 복잡한 용어가 포함되어 있습니다. 그게 정확히 무슨 뜻인지 궁금하시죠?

이 글에서는 이 과장된 개념에 대해 더 간단하게 배워보세요.

## 1. 컴포넌트

컴포넌트는 웹페이지에 렌더링할 리액트 엘리먼트를 반환하는 재사용 가능한 작은 코드 조각입니다. 버튼, 네비게이션 바, 카드 등과 같이 웹페이지의 한 부분을 구성하는 코드 그룹입니다. JavaScript 함수와 비슷하지만 렌더링된 엘리먼트를 반환합니다. "Props"라고 불리는 매개변수를 받습니다. 컴포넌트 이름은 대문자로 작성합니다.

<div class="content-ad"></div>

함수 컴포넌트 예시

```js
function Heading(props) {
  return <h1>Join us, {props.name}!</h1>;
}
```

참고:

- 클래스 기반 컴포넌트 대신 함수 컴포넌트를 권장합니다.
- 함수 컴포넌트는 종종 stateless 컴포넌트라고 불립니다.

<div class="content-ad"></div>

## 2. JSX

JSX는 JavaScript XML의 약자로, React에서 HTML을 작성할 수 있게 해줍니다. XML과 비슷한 태그와 속성을 도입하여 React 엘리먼트를 만들 수 있습니다. .jsx 파일에서 HTML과 유사한 코드를 작성할 수 있기 때문에 React 컴포넌트를 쉽게 작성할 수 있습니다. 복잡한 JavaScript를 사용하는 대신 JSX를 사용하면 코드가 가독성 있고 깨끗해집니다. React DOM은 htmlFor, onClick과 같이 속성의 네이밍에 camelCase를 사용합니다.

JSX의 예시

```js
<h1 className="head">This is H1!</h1>
```

<div class="content-ad"></div>

이제 TSX는 JSX 구문을 포함하는 TypeScript 파일의 파일 확장자입니다. TSX를 사용하면 기존의 JSX 구문을 사용하여 유형을 확인할 수 있는 코드를 작성할 수 있습니다. TypeScript는 다른 언어가 아니라 JavaScript에 선택적 정적 타이핑을 추가한 슈퍼셋입니다.

더 간단히 말하면 TSX 파일을 사용하여 TypeScript와 JSX를 함께 사용하여 React 컴포넌트를 작성할 수 있습니다.

TSX 예제

```js
interface AgeProps {
  age: number;
}

const GreetAge = (props: AgeProps) => {
  return (
    <div>
      Hello, you are {props.age} old.
    </div>
  );
};
```

<div class="content-ad"></div>

알림:

- JSX 파일은 `.jsx` 파일 확장자를 사용합니다.
- TSX 파일은 `.tsx` 파일 확장자를 사용합니다.
- TypeScript의 타입 시스템은 개발 초기에 잠재적인 오류를 미리 찾아줍니다.

## 3. 프래그먼트

React의 프래그먼트는 컴포넌트에서 여러 요소를 반환할 수 있게 해줍니다. 추가적인 DOM 노드를 생성하지 않고 요소 목록을 그룹화합니다. 이는 DOM에서 모든 추가 div를 제거하여 UI를 빠르게 렌더링합니다.

<div class="content-ad"></div>

Fragment 예시

```js
const App = () => {
  return (
    <>
      <h1>Eat</h1>
      <button>Learn more</button>
      <p>Code is Fun</p>
      <button>Repeat</button>
    </>
  );
}
```

참고:

- Fragment를 사용하면 코드가 더 깨끗하고 가독성이 높아집니다.
- 메모리를 효율적으로 사용할 수 있습니다.
- CSS 스타일을 가질 수 없습니다.

<div class="content-ad"></div>

## 4. Props

"Props"은 React에서 특별한 키워드로서 속성을 나타냅니다. 컴포넌트 간에 데이터를 전송하는 데 사용됩니다. 데이터 전송의 흐름은 단방향이며, 부모 컴포넌트에서 자식 컴포넌트로 전달됩니다.

Props의 예시

```js
function Head(props) {
  return <p>{props.children}</p>;
}
```

<div class="content-ad"></div>

참고: Props은 읽기 전용이며, 자식 컴포넌트가 부모 컴포넌트에서 전달되는 값을 조작하지 못하도록 합니다.

## 5. 상태

사용자가 상호작용할 때 컴포넌트는 특정 값을 추적해야 합니다. 예를 들어, 라이트/다크 모드 테마 전환 버튼이 사용자가 클릭할 때 해당 값이 변경됩니다(라이트에서 다크로 또는 그 반대로). 컴포넌트는 현재 테마의 값을 기억해야 합니다. React에서 이러한 컴포넌트별 메모리를 상태(state)라고 합니다.

상태는 useState() 훅을 사용하여 정의됩니다. 이에 대해 자세히 설명하겠습니다.

<div class="content-ad"></div>

상태를 정의하는 예제

```js
const [index, setIndex] = useState(0)
```

참고: 다른 자식 컴포넌트와 쉽게 공유하고 단일 소스의 진실을 보장하기 위해 항상 최상위 컴포넌트에서 상태를 정의하는 것이 좋은 실천 방법입니다.

## 6. 라이프사이클 메서드

<div class="content-ad"></div>

Lifecycle 메서드는 React 클래스 내에서 사용할 수 있는 특별한 함수로, 컴포넌트의 존재하는 여러 단계에서 작업을 수행할 수 있습니다. 이러한 단계는 다음과 같습니다:

- Mounting: 컴포넌트가 처음 생성되어 DOM에 삽입될 때 발생합니다.
- Updating: 컴포넌트의 props 또는 state가 변경되어 컴포넌트가 다시 렌더링될 때 발생합니다.
- Unmounting: 컴포넌트가 DOM에서 제거될 때 발생합니다.

## 7. Purity

함수형 프로그래밍에서의 순도(Purity)는 주어진 동일한 입력이 항상 동일한 출력을 반환할 때 발생합니다. 입력이 출력을 결정하는 유일한 요소인 경우 함수가 순수하다고 말합니다.

<div class="content-ad"></div>

리액트에서 컴포넌트가 순수하다고 말할 때는 동일한 입력(예: props)에 대해 동일한 출력을 반환할 때를 의미합니다.

## 8. Strict Mode

Strict Mode은 리액트의 개발 기능 중 하나로, 코드 품질을 높이기 위해 추가적인 안전 기능을 활성화하는 기능입니다. 잠재적인 오류와 버그에 대한 경고를 표시하며, 브라우저 콘솔에 경고를 기록합니다.

Strict Mode의 예시



<div class="content-ad"></div>

```js
import { StrictMode } from 'react';

function App() {
  return (
    <>
     <StrictMode>
        <Header />
        <Sidebar />
        <Content />
        <Footer />
     </StrictMode>
    </>
  )
}
```

## 9. 훅(Hooks)

리액트의 훅은 클래스 컴포넌트를 작성하지 않고도 상태 및 다른 리액트 기능을 사용할 수 있게 해줍니다. 훅은 리액트의 상태 관리, 부작용 및 기타 기능에 액세스할 수 있는 함수입니다.

일반적으로 사용되는 훅은 useState, useMemo, useRef 등이 있습니다.

<div class="content-ad"></div>

훅의 예시

```js
import React, { useState } from "react"; // useState 훅을 불러오기

function FavoriteColor() {
  const [color, setColor] = useState("red"); // 상태와 setter 함수 초기화

  return (
    <>
      <h1>내가 좋아하는 색은 {color}입니다!</h1>
      <button
        type="button"
        onClick={() => setColor("blue")} // 상태 업데이트
      >파랑</button>
      <button
        type="button"
        onClick={() => setColor("red")} // 상태 업데이트
      >빨강</button>
      <button
        type="button"
        onClick={() => setColor("yellow")} // 상태 업데이트
      >노랑</button>
    </>
  );
}
```

참고:

- 훅은 React 함수 컴포넌트 내에서만 호출할 수 있습니다.
- 훅은 컴포넌트의 최상위 레벨에서만 호출할 수 있습니다.
- 훅은 조건문 내에서 사용할 수 없습니다.

<div class="content-ad"></div>

## 10. Context API

Context API는 prop을 수동으로 각 레벨에서 전달하지 않고 컴포넌트 트리 전체에서 상태, 함수와 같은 데이터를 공유하기 위해 사용됩니다. 이는 prop을 천천히 내려보내는 것을 피하며 상태 관리를 간소화하고 컴포넌트 전체에서 데이터를 공유함으로써 prop 전달을 피합니다. Context API를 사용하면 데이터가 직접 소비할 자식 컴포넌트와 공유됩니다.

createContext() 메서드는 컨텍스트를 생성하는 데 사용됩니다. 이 함수는 두 개의 컴포넌트 - Provider와 Consumer를 가진 컨텍스트 객체를 반환합니다.

Provider는 컨텍스트를 사용할 부분을 감싸기 위해 사용됩니다. 다른 컴포넌트와 공유하고 싶은 데이터를 보관하는 필수 value prop을 허용합니다.

<div class="content-ad"></div>

useContext 훅을 사용하여 데이터에 접근합니다.

Context API 예제

createContext() 메서드를 사용하여 컨텍스트를 생성합니다. 자식 컴포넌트를 Context Provider로 감싸고 상태 값을 제공합니다.

```js
import { useState, createContext } from "react";

const UserContext = createContext();

function ParentCounter() {
  const [count, setCount] = useState(10);

  return (
    <UserContext.Provider value={count}>
      <h1>{`Current Count: ${count}!`}</h1>
      <Button />
    </UserContext.Provider>
  );
}
```

<div class="content-ad"></div>

컨텍스트 훅인 useContext를 사용하여 나이의 값을 액세스하세요.

```js
import { useContext } from "react";

function GrandChildConsumer() {
  const age = useContext(UserContext);

  return (
    <>
      <h1>This is GrandChildConsumer</h1>
      <h2>{`Current Age: ${age}`}</h2>
    </>
  );
}
```

## 11. 리스트와 키

키(Key)는 React에서 리스트 아이템에 대한 특별한 종류의 속성입니다. 업데이트, 삭제 또는 추가될 때 각 항목을 고유하게 식별하는 역할을 합니다.

<div class="content-ad"></div>

항목의 색인을 키로 지정하는 것은 권장되지 않습니다. 왜냐하면 항목이 재정렬되면 예상되는 동작에 영향을 미칠 수 있습니다.

장바구니에 10개의 항목이 추가되어 있고 각 항목은 고유한 색인을 키로 가지고 있는 상황을 상상해보세요. 이제 장바구니에서 첫 번째와 다섯 번째 항목을 제거하기로 결정했습니다. 항목이 제거되면 색인이 변경될 것입니다. 두 번째 항목이 첫 번째로, 여섯 번째 항목이 다섯 번째로 변경될 것입니다.

리스트와 키의 예시

```js
const fruits = ["apple", "banana", "orange"];

function FruitList() {
  return (
    - 리스트와 키의 예시

const fruits = ["apple", "banana", "orange"];

function FruitList() {
  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}> {fruit} </li>
      ))}
    </ul>
  );
}

<div class="content-ad"></div>

- 목록에서 항목을 고유하게 식별하는 'Key'로 문자열을 사용하는 것이 좋습니다.
- UUID와 같은 서드파티 라이브러리는 고유한 키를 생성하는 기능을 제공합니다.

## 12. 폼: 제어 및 비제어 컴포넌트

React 폼은 전통적인 HTML 폼보다 사용자 입력을 수집하고 관리하는 데 더 나은 기능을 제공합니다. 이러한 폼은 컴포넌트를 사용하여 구축되며 사용자 입력을 상태에 저장합니다. 두 가지 유형의 컴포넌트가 있습니다:

### 제어 컴포넌트

<div class="content-ad"></div>

통제 구성요소(Controlled components)에서는 폼의 상태가 컴포넌트 자체에서 관리됩니다. 이것은 React에서 폼 데이터를 관리하는 권장 방법입니다. 사용자가 폼과 상호 작용할 때(예: 입력 필드에 입력하는 경우), 컴포넌트의 상태가 변경 사항을 반영하도록 업데이트됩니다.

통제 구성요소의 예시

function ControlledInput() {
  const [name, setName] = useState('');

  const handleChange = (e) => {
    setName(e.target.value);
  }

  return (
    <div>
      <label htmlFor="name">Name: </label>
      <input type="text" id="name" value={name} onChange={handleChange} />
      <p>Your name is: {name}</p>
    </div>
  );
}

### 통제되지 않는 구성요소(Uncontrolled Components)

<div class="content-ad"></div>

Uncontrolled components는 DOM을 사용하여 양식 데이터를 관리합니다. 컴포넌트는 양식의 상태를 직접 제어하지 않지만 ref와 같은 DOM 메서드를 사용하여 값을 액세스할 수 있습니다.

Uncontrolled Component의 예시

function UncontrolledInput() {
  const nameRef = useRef(null);

  const handleClick = () => {
    console.log(nameRef.current.value);
  }

  return (
    <div>
      <label htmlFor="name">이름: </label>
      <input type="text" id="name" ref={nameRef} />
      <button onClick={handleClick}>이름 가져오기</button>
    </div>
  );
}

참고:

<div class="content-ad"></div>

- 제어 컴포넌트는 상태 사용으로 사용자 입력이 즉시 반영되므로 양식 유효성을 제공합니다.
- 제어 컴포넌트에서는 양식 유효성을 제공할 수 없습니다. 양식은 제출된 후에만 사용자 입력에 액세스할 수 있습니다.

## 13. React Router

- 네비게이션을 위한 React Router 소개
- 기본 설정 및 사용 방법
- 예시: 라우트 생성 및 페이지 간 이동

React Router는 React의 라우팅을 위한 표준 라이브러리입니다. React 앱 내에서 다양한 컴포넌트 간에 이동할 수 있도록 합니다. 브라우저 URL을 변경하고 UI를 URL과 동기화하는 기능을 제공합니다. React Router는 네비게이션 기능을 갖춘 싱글 페이지 애플리케이션(SPA)을 만드는 데 중요합니다.

<div class="content-ad"></div>

먼저 터미널에서 React Router를 설치해야 합니다.

React Router 설치하기

# npm을 사용하는 경우
npm install react-router-dom

# yarn을 사용하는 경우
yarn add react-router-dom

React Router 예시

<div class="content-ad"></div>

import { BrowserRouter, Routes, Route } from "react-router-dom";
import Home from "./pages/Home";
import About from "./pages/About";
import Contact from "./pages/Contact";
import NoPage from "./pages/NoPage";

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
          <Route path="/" element={<Home />} />
          <Route path="about" element={<About />} />
          <Route path="contact" element={<Contact />} />
          <Route path="*" element={<NoPage />} />
      </Routes>
    </BrowserRouter>
  );
}

먼저 내용을 `BrowserRouter` 태그로 둘러싸세요. 그런 다음 내비게이션을 위해 `Routes`를 정의하고 그 안에 `Route`를 소개합니다. `Route`에는 페이지의 URL을 지정하는 path와 지정된 경로에서 렌더링해야 하는 컴포넌트를 지정하는 element 속성이 있습니다.

참고:

- 앱에는 여러 개의 `Routes`가 있을 수 있습니다.
- `Route`는 중첩될 수 있습니다.
- `react-router-dom`에는 내비게이션을 위한 `Link`와 `Outlet` 컴포넌트도 있습니다.

<div class="content-ad"></div>

## 결론

어떤 프로그래밍 언어를 배우는 가장 좋은 방법은 더 많은 프로젝트를 실습하는 것입니다. 작은 프로젝트를 만들고 개념을 실험해보세요.

만약 이 글이 도움이 되었다면 사랑을 계속 보여주지 않으시면 안돼요. 다음에 만나요! 좋아요를 누르고 공유하고 배우세요.

또한 저를 팔로우하여 연결 유지할 수 있습니다. X와 GitHub, LinkedIn에서도 저를 따라와주세요.