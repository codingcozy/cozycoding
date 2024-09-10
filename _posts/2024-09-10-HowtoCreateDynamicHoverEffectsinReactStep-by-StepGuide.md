---
title: "React에서 동적 호버 효과를 만드는 방법 (단계별 가이드)"
description: ""
coverImage: "/assets/img/2024-09-10-HowtoCreateDynamicHoverEffectsinReactStep-by-StepGuide_0.png"
date: 2024-09-10 17:50
ogImage: 
  url: /assets/img/2024-09-10-HowtoCreateDynamicHoverEffectsinReactStep-by-StepGuide_0.png
tag: Tech
originalTitle: "How to Create Dynamic Hover Effects in React (Step-by-Step Guide)"
link: "https://medium.com/@pinjarirehan/how-to-create-dynamic-hover-effects-in-react-step-by-step-guide-aa40098acee6"
isUpdated: false
---



![Hover](https://miro.medium.com/v2/resize:fit:1400/1*jNq5tta4ofDzmI7ARPUIWA.gif)

"호버"에 대해 이야기해봐요! 아니, 드론으로 하는 것이 아니에요. 호버 이벤트는 버튼, 시각적 요소 및 툴팁을 작동시킵니다.

호버 효과는 정적인 객체를 활기차게 만들고 상호작용 경험을 만들어 사용자 인터페이스를 개선합니다.

버튼에 피드백을 제공하거나 카드에 추가 세부 정보를 표시하는 경우, 모든 웹 개발자에게 호버 이벤트를 알고 있는 것이 중요합니다.


<div class="content-ad"></div>

이 게시물에서는 React의 hover 이벤트를 살펴보겠습니다. 기본 인라인 핸들러에서 고급 사용자 지정 훅까지 모두 다룰 것입니다.

React에 익숙한 새내기든 전문가든 모두에게 도움이 되는 내용이 있을 것입니다. 또한, 중간에 재미를 느낄 수도 있을지도 모릅니다.

시작해보죠!

# Hover 이벤트의 기본들

<div class="content-ad"></div>

## 호버 이벤트 이해하기

호버 이벤트는 클릭 없이 반응을 제공하여 등을 쓰다듬는 것과 유사합니다. 마우스가 요소 위에 올라가면 이벤트를 트리거하여 사용자 경험을 향상시키는 데 도움이 됩니다.

호버 효과는 UI의 불꽃놀이와 같습니다; 직관적이지만 매력적입니다.

## 전통적 방식 대 React 접근법

<div class="content-ad"></div>

과거에는 CSS (:hover) 또는 JavaScript (안녕, jQuery!)을 사용하여 호버 효과를 다루었습니다. 하지만 React에서는 어떻게 처리할까요? React는 조금 다르게 처리하고 싶어합니다.

React에서는 컴포넌트 내에서 직접 호버 이벤트를 처리할 수 있어 기본적인 것을 넘어서 더 다이나믹하고 상호작용을 위한 작업을 할 수 있습니다.

# 인라인 이벤트 핸들러를 사용한 호버 이벤트 처리

## 기본 예제

<div class="content-ad"></div>

기초부터 시작해봐요. React의 인라인 이벤트 핸들러는 마우스 호버 효과의 "Hello World"와 유사합니다.

다음은 onMouseEnter 및 onMouseLeave 이벤트를 사용한 기본 예제입니다:

```js
import React from 'react';

// HoverButton이라는 함수형 컴포넌트 정의
function HoverButton() {
  return (
    // 버튼 요소 렌더링
    <button
      // 마우스가 버튼에 들어왔을 때 콘솔에 메시지 기록
      onMouseEnter={() => console.log('Hovering!')}
      // 마우스가 버튼을 떠났을 때 다른 메시지 기록
      onMouseLeave={() => console.log('Left the hover!')}
    >
      Hover Over Me!
    </button>
  );
}

// 다른 파일에서 가져와서 사용할 수 있도록 HoverButton 컴포넌트 내보내기
export default HoverButton;
```

## 코드 설명

<div class="content-ad"></div>

- onMouseEnter: 마우스가 요소 안으로 들어갈 때 트리거됩니다.
- onMouseLeave: 마우스가 요소를 벗어날 때 트리거됩니다.

쉽죠? 실제 동작을 보기 위해 이 라이브 데모를 확인해보세요!

# CSS-in-JS 라이브러리로 호버 이벤트 다루기

## Styled-components와 Emotion

<div class="content-ad"></div>

내부에서 모든 것을 처리하는 것을 좋아하는 사람들을 위해, styled-components나 Emotion과 같은 CSS-in-JS 라이브러리를 사용하면 JavaScript 파일에서 hover 이벤트를 처리할 수 있습니다. 이것은 마치 케이크를 먹으면서도 스타일링을 할 수 있는 것과 같아요.

```js
// styled-components 라이브러리를 불러옵니다
import styled from 'styled-components';

// 기본 스타일 및 hover 스타일이 있는 styled 버튼 컴포넌트를 생성합니다
const HoverButton = styled.button`
  background-color: lightblue; // 기본 배경색
  color: black; // 기본 텍스트 색상

  // hover 스타일을 정의합니다
  &:hover {
    background-color: darkblue; // 호버 시 배경색
    color: white; // 호버 시 텍스트 색상
  }
`;

// 주 앱 컴포넌트
function App() {
  // hover 효과가 있는 HoverButton 컴포넌트를 렌더링합니다
  return <HoverButton>Hover Over Me!</HoverButton>;
}

// App 컴포넌트를 기본 내보내기로 내보냅니다
export default App;
```

## 장점

CSS-in-JS를 사용하면 스타일을 컴포넌트에 스코프로 제한할 수 있을 뿐만 아니라 (우연한 전역 스타일 사고 없음!), React의 컴포넌트 기반 아키텍처와 원활하게 통합됩니다.

<div class="content-ad"></div>

# 호버 효과를 관리하는 데 상태를 사용하기

## 상태 관리

여기서 React가 빛을 발합니다 — 상태를 사용하여 호버 효과를 제어합니다. useState 훅을 사용하면 호버 상태에 따라 동적으로 스타일을 추가, 제거 또는 변경할 수 있습니다.

## 훅 동작

<div class="content-ad"></div>

```js
import React, { useState } from 'react';

function HoverCard() {
  // 카드에 호버 중인지 추적하는 상태 변수 'isHovered'를 선언합니다.
  const [isHovered, setIsHovered] = useState(false);

  return (
    <div
      // div 요소에 인라인 스타일을 적용합니다. 'isHovered' 상태에 따라 배경색을 변경합니다.
      style={{
        padding: '20px',
        backgroundColor: isHovered ? 'yellow' : 'lightgray',
        cursor: 'pointer', // 상호작용을 나타내기 위해 포인터 커서를 추가합니다.
        borderRadius: '8px', // 더 부드러운 외관을 위해 소규모 테두리 반지름을 추가합니다.
        transition: 'background-color 0.3s ease', // 배경색이 변경될 때 부드러운 전환을 적용합니다.
      }}
      // 마우스가 div 영역에 들어왔을 때의 이벤트 핸들러입니다. 'isHovered'를 true로 설정합니다.
      onMouseEnter={() => setIsHovered(true)}
      // 마우스가 div 영역을 벗어났을 때의 이벤트 핸들러입니다. 'isHovered'를 false로 설정합니다.
      onMouseLeave={() => setIsHovered(false)}
    >
      Hover Over This Card!
    </div>
  );
}

export default HoverCard;
```

## Best Practices

- 상호작용을 실제로 향상시키는 경우에만 상태 관리를 유지하세요.
- CSS만으로 처리 가능한 순수한 시각적 효과에 상태를 사용하지 마세요.

# React Hooks를 사용한 호버 처리


<div class="content-ad"></div>

## 사용자 지정 훅

상태에서 멈추지 마세요! 코드를 재사용 가능하고 깨끗하게 만들기 위해 호버 이벤트를 처리하는 사용자 지정 훅을 작성하세요.

## 재사용 가능한 코드

```js
import { useState } from 'react'; // 리액트에서 useState 훅을 가져오기

// 호버 상태를 처리하는 사용자 지정 훅
function useHover() {
  // 요소가 호버 상태인지 추적하는 상태
  const [hovered, setHovered] = useState(false);

  // 마우스 진입 이벤트를 처리하는 함수
  const onMouseEnter = () => setHovered(true);

  // 마우스 떠남 이벤트를 처리하는 함수
  const onMouseLeave = () => setHovered(false);

  // 상태 및 이벤트 핸들러 반환
  return { hovered, onMouseEnter, onMouseLeave };
}

// 사용자 지정 useHover 훅을 사용하는 컴포넌트
function HoverBox() {
  // useHover 훅에서 반환된 값을 구조분해 할당
  const { hovered, onMouseEnter, onMouseLeave } = useHover();

  return (
    <div
      // 호버 박스에 대한 인라인 스타일링
      style={{
        padding: '20px',
        backgroundColor: hovered ? 'green' : 'red', // 호버 상태인 경우 초록색, 그렇지 않으면 빨간색
      }}
      onMouseEnter={onMouseEnter} // 마우스 진입 시 호버 상태를 true로 설정
      onMouseLeave={onMouseLeave} // 마우스 떠날 때 호버 상태를 false로 설정
    >
      이 상자 위로 호버하세요!
    </div>
  );
}

export default HoverBox; // HoverBox 컴포넌트를 기본으로 내보내기
```

<div class="content-ad"></div>

## 고급 예시

사용자 정의 훅은 중첩된 호버 효과, 툴팁, 심지어 애니메이션과 같이 복잡한 상호작용을 처리할 수 있어요.

하늘의 한계가 없어요!

# Ref를 사용하여 직접 DOM 조작하기

<div class="content-ad"></div>

## 호버 처리에서 Ref 사용

가끔은 DOM과 가까이서 작업해야 할 때가 있습니다. 이때 ref가 필요합니다. useRef 훅을 사용하면 DOM 요소를 직접 조작할 수 있습니다. 상태만으로는 처리할 수 없는 상황에 완벽하게 적합합니다.

## 장단점

- 장점: DOM에 직접 액세스할 수 있어 재렌더링이 필요하지 않습니다.
- 단점: React의 선언적 특성을 우회하므로 조심해서 사용해야 합니다!

<div class="content-ad"></div>

```js
import React, { useRef } from 'react';

const HoverWithRef = () => {
  // Div 엘리먼트에 연결할 ref 생성
  const divRef = useRef(null);

  // Div에 마우스가 진입했을 때 처리하는 함수
  const handleMouseEnter = () => {
    // 마우스가 진입했을 때 div의 배경색을 변경
    divRef.current.style.backgroundColor = 'lightcoral';
  };

  // Div에서 마우스가 빠져나갈 때 처리하는 함수
  const handleMouseLeave = () => {
    // 마우스가 빠져나갈 때 div의 배경색을 원래대로 되돌림
    divRef.current.style.backgroundColor = 'lightgrey';
  };

  return (
    <div
      ref={divRef} // Div 엘리먼트에 ref 연결
      onMouseEnter={handleMouseEnter} // 마우스 진입 이벤트 핸들러 설정
      onMouseLeave={handleMouseLeave} // 마우스 빠져나감 이벤트 핸들러 설정
      style={{
        padding: '20px', // Div 내부에 간격을 추가
        borderRadius: '10px', // Div의 모서리를 둥글게 처리
        backgroundColor: 'lightgrey' // 초기 배경색 설정
      }}
    >
      이 Div 위로 마우스를 가져가 보세요!
    </div>
  );
};

export default HoverWithRef;
```

위 예시에서는 useRef를 사용하여 호버 이벤트가 발생할 때 DOM을 직접 조작하는 방법을 보여주고 있습니다. 이 접근 방식은 주로 서드 파티 라이브러리와 함께 작업하거나 상태만으로 간단히 처리하기 어려운 복잡한 애니메이션을 트리거해야 할 때 특히 유용합니다.

그러나 기억하세요! 큰 권한을 가졌다면 큰 책임을 져야 합니다. ref를 잘 관리하세요!

# 함수형 컴포넌트와 클래스 컴포넌트에서 호버 이벤트 처리하기


<div class="content-ad"></div>

## 기능 컴포넌트

현대적인 React는 기능 컴포넌트와 훅에 관한 것입니다. 기능 컴포넌트에서 호버 이벤트를 다루는 것은 간단하며, 코드를 깔끔하고 간결하게 유지하려면 useState와 useRef와 같은 훅을 활용합니다.

```js
import React, { useState } from 'react'; // 리액트에서 React 및 useState를 가져옵니다

// FunctionalHover라는 기능 컴포넌트를 정의합니다
const FunctionalHover = () => {
  // 'false'가 기본 값인 'hovered' 상태 변수를 선언합니다
  const [hovered, setHovered] = useState(false);

  // 컴포넌트는 호버 시 배경색을 변경하는 div를 반환합니다
  return (
    <div
      onMouseEnter={() => setHovered(true)} // 마우스가 div에 들어가면 'true'로 hovered를 설정합니다
      onMouseLeave={() => setHovered(false)} // 마우스가 div를 떠나면 'false'로 hovered를 설정합니다
      style={{
        backgroundColor: hovered ? 'lightblue' : 'lightgray', // 호버 상태에 따른 조건부 스타일링
        padding: '20px',
        borderRadius: '5px',
        transition: 'background-color 0.3s ease', // 배경색에 대한 부드러운 전환 효과
      }}
    >
      I'm a functional component!
    </div>
  );
};

// FunctionalHover 컴포넌트를 기본 내보내기로 내보냅니다
export default FunctionalHover;
```

## 클래스 컴포넌트

<div class="content-ad"></div>

레거시 코드를 유지 보수하는 경우에는 아직 클래스 컴포넌트를 다루고 있을 수 있습니다. 클래스 컴포넌트에서는 마우스 호버 이벤트를 동일한 이벤트 핸들러를 사용하여 처리할 수 있지만, 상태 관리 방법이 약간 다릅니다.

```js
import React, { Component } from 'react';

// ClassHover라는 클래스 컴포넌트 정의
class ClassHover extends Component {
  // 생성자에서 상태 초기화
  constructor(props) {
    super(props);
    this.state = { hovered: false }; // 'hovered' 상태를 기본적으로 false로 설정
  }

  // 마우스 진입 이벤트를 처리하는 메서드
  handleMouseEnter = () => {
    this.setState({ hovered: true }); // 마우스가 들어오면 'hovered' 상태를 true로 설정
  };

  // 마우스 떠남 이벤트를 처리하는 메서드
  handleMouseLeave = () => {
    this.setState({ hovered: false }); // 마우스가 나가면 'hovered' 상태를 false로 설정
  };

  // 컴포넌트를 표시하는 렌더 메서드
  render() {
    return (
      <div
        onMouseEnter={this.handleMouseEnter} // 마우스 진입 이벤트 핸들러를 연결
        onMouseLeave={this.handleMouseLeave} // 마우스 떠남 이벤트 핸들러를 연결
        style={{
          backgroundColor: this.state.hovered ? 'lightblue' : 'lightgray', // 'hovered' 상태에 따라 배경색 변경
          padding: '20px', // div에 padding 추가
          borderRadius: '5px', // div 모서리를 둥글게 처리
        }}
      >
        I'm a class component!
      </div>
    );
  }
}

// ClassHover 컴포넌트를 내부 앱의 다른 부분에서 사용할 수 있도록 내보냄
export default ClassHover;
```

함수형 컴포넌트가 React의 미래이며 코드를 더 쉽게 읽고 유지할 수 있지만, 레거시 프로젝트에서는 클래스 컴포넌트가 여전히 중요합니다.

두 방식 모두 유효하지만 새롭게 시작하는 경우 함수형 컴포넌트와 훅을 사용하는 것이 좋습니다.

<div class="content-ad"></div>

# 마무리 말씀

오늘은 hover 이벤트의 기본부터 사용자 정의 후크, 상태 관리, 그리고 직접 DOM 조작을 활용한 고급 기술까지 많은 내용을 다뤘어요.

함수형 컴포넌트, 클래스 컴포넌트 또는 둘 다 사용 중이더라도, 이제는 전문가처럼 hover 이벤트를 다룰 수 있는 도구를 가졌어요.

그리고, 만약 hover 이벤트 해킹이나 공포 이야기가 있다면, 아래 댓글에 남겨주세요! 여러분의 이야기를 들어보고 싶어요!

<div class="content-ad"></div>


![How to Create Dynamic Hover Effects in React - Step-by-Step Guide](/assets/img/2024-09-10-HowtoCreateDynamicHoverEffectsinReactStep-by-StepGuide_0.png)
