---
title: "React에서 forwardRef API를 사용하는 방법 현실적인 예제로 살펴보는 포괄적인 가이드"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 18:28
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "How to use forwardRef API in React A Comprehensive Guide with Real-Life Examples"
link: "https://medium.com/zestgeek/how-to-use-forwardref-api-in-react-a-comprehensive-guide-with-real-life-examples-6a3f33fe7ebd"
isUpdated: false
---


React의 forwardRef API는 구성 요소가 참조를 자식 구성 요소로 전달 할 수있게하는 강력한 도구입니다. 재사용 가능한 구성 요소를 작성하거나 제3 자 라이브러리를 다룰 때 특히 유용 할 수 있습니다. 이 문서에서는 forwardRef API에 대해 자세히 살펴 보고, 그 유용성을 이해하고, 여러 실제 예제를 살펴보면서 그 힘을 설명하겠습니다.

# forwardRef란?

React에서 ref는 렌더 메서드에서 만들어진 DOM 노드 또는 React 요소에 액세스하는 방법을 제공합니다. 일반적으로 ref는 클래스 구성 요소에서 사용하지만 훅의 도입으로 ref를 함수 구성 요소에서도 사용할 수 있습니다. forwardRef는 부모 구성 요소에서 자식 구성 요소 중 하나로 ref를 전달할 수 있도록하는 유틸리티 함수입니다.

# 왜 forwardRef를 사용해야하나요?

<div class="content-ad"></div>

- HOC(고차 컴포넌트)에서 Ref 전달: HOC는 종종 다른 컴포넌트를 감싸고 해당 컴포넌트의 DOM 노드를 숨길 수 있습니다. forwardRef를 사용하면 HOC를 통해 ref를 전달하여 DOM 노드에 직접 액세스할 수 있습니다.
- 향상된 컴포넌트 상호 운용성: 라이브러리나 UI 키트를 구축할 때 ref 액세스를 제공하는 것은 외부에서 입력 필드에 초점을 맞추는 것과 같은 고급 사용에 중요할 수 있습니다.
- 포커스 및 애니메이션 관리: Ref 전달은 포커스 관리 및 애니메이션이 중요한 시나리오에서 필수적일 수 있으며, 기본 DOM 요소와의 원활한 상호 작용을 보장합니다.

# forwardRef가 작동하는 방법은?

forwardRef 함수는 React 컴포넌트를 인수로 받아 새 컴포넌트를 반환합니다. 이 새 컴포넌트는 ref prop을 사용하여 내부 컴포넌트로 전달합니다.

다음은 기본 구문입니다:

<div class="content-ad"></div>

```js
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="fancy-button">
    {props.children}
  </button>
));
```

# 실제 예시

## 1. 커스텀 입력 컴포넌트 생성

부모 컴포넌트가 포커스를 관리할 수 있는 재사용 가능한 입력 컴포넌트를 만들고 싶다고 상상해보세요.

<div class="content-ad"></div>

```js
import React, { useRef } from 'react';

const CustomInput = React.forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});

function App() {
  const inputRef = useRef(null);

  const handleFocus = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <CustomInput ref={inputRef} placeholder="여기에 입력하세요..." />
      <button onClick={handleFocus}>입력란에 초점 맞추기</button>
    </div>
  );
}

export default App;
```

이 예제에서 버튼을 클릭하면 프로그래밍 방식으로 입력란에 초점이 맞추어지며, ref forwarding의 힘을 실제로 보여줍니다.

## 2. 고차 컴포넌트에서 Ref Forwarding

고차 컴포넌트(Higher-Order Component, HOC)로 래핑된 버튼이 있다고 가정해봅시다. Ref가 실제 버튼을 가리키도록 하고 싶습니다.


<div class="content-ad"></div>

```js
import React, { useRef } from 'react';

function withLog(Component) {
  class WithLog extends React.Component {
    render() {
      return <Component {...this.props} />;
    }
  }

  return React.forwardRef((props, ref) => (
    <WithLog {...props} forwardedRef={ref} />
  ));
}

const Button = React.forwardRef((props, ref) => (
  <button ref={ref} {...props}>
    Click me
  </button>
));

const LogButton = withLog(Button);

function App() {
  const buttonRef = useRef();

  const handleClick = () => {
    console.log(buttonRef.current); // Logs the actual button DOM node
  };

  return (
    <div>
      <LogButton ref={buttonRef} onClick={handleClick} />
    </div>
  );
}

export default App;
```

여기에서 withLog는 컴포넌트를 래핑하고 상호 작용을 기록하는 HOC입니다. forwardRef를 사용하여 LogButton에 전달된 ref가 HOC가 아닌 실제 버튼을 가리키도록합니다.

## 3. 다른 훅과 forwardRef를 결합하기

forwardRef는 다른 훅과 결합하여 더 복잡한 컴포넌트를 구축하는 데 사용될 수 있습니다. 예를 들어, ref를 전달뿐만 아니라 내부 상태를 사용하는 컴포넌트를 원할 수 있습니다.


<div class="content-ad"></div>

```js
import React, { useRef, useState } from 'react';

const PasswordInput = React.forwardRef((props, ref) => {
  const [showPassword, setShowPassword] = useState(false);

  return (
    <div>
      <input
        type={showPassword ? 'text' : 'password'}
        ref={ref}
        {...props}
      />
      <button onClick={() => setShowPassword(!showPassword)}>
        {showPassword ? 'Hide' : 'Show'}
      </button>
    </div>
  );
});

function App() {
  const passwordRef = useRef();

  const handleShowPassword = () => {
    passwordRef.current.focus();
  };

  return (
    <div>
      <PasswordInput ref={passwordRef} />
      <button onClick={handleShowPassword}>Focus Password Input</button>
    </div>
  );
}

export default App;
```

이 예제는 가시성을 토글하는 비밀번호 입력 필드를 보여줍니다. 동시에 부모 구성 요소가 포커스를 제어할 수 있습니다.

# forwardRef 사용 시의 최상의 방법

- 의미 있는 Ref 이름 사용: inputRef나 buttonRef와 같이 의미 있는 이름을 지정하여 코드를 더 읽기 쉽고 유지보수하기 쉽게 만듭니다.
- Ref 사용 제한: Ref를 절약해서 사용하세요. 대부분의 데이터 흐름에는 props와 상태를 의존하세요. Ref는 DOM 노드와 인터랙션하거나 직접적인 DOM 액세스가 필요한 써드파티 라이브러리 통합 시에 사용하세요.
- useImperativeHandle과 결합: forwardRef와 함께 useImperativeHandle 훅을 사용하여 ref를 사용할 때 노출되는 인스턴스 값을 사용자 정의하세요. focus나 scrollTo와 같은 명령형 메서드를 캡슐화할 수 있습니다.


<div class="content-ad"></div>

# 결론

React의 forwardRef API는 컴포넌트의 유연성과 기능성을 향상시킬 수 있는 강력한 기능입니다. 재사용 가능한 컴포넌트를 구축하거나 타사 라이브러리를 통합하거나 복잡한 포커스 시나리오를 관리하는 경우, forwardRef는 가치 있는 도구가 될 수 있습니다. 이 API를 이해하고 활용함으로써 더 강력하고 유지보수가 쉬운 React 애플리케이션을 개발할 수 있습니다. 즐거운 코딩하세요!