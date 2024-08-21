---
title: "리액트 개발자를 위한 신기술 Chrome에서 출시한 새로운 CSS 앵커 위치 지정 API"
description: ""
coverImage: "/assets/img/2024-07-19-AnchorAwayTheNewCSSAnchorPositioningAPIReleasedbyChromeABoonforReactDevelopers_0.png"
date: 2024-07-19 11:20
ogImage: 
  url: /assets/img/2024-07-19-AnchorAwayTheNewCSSAnchorPositioningAPIReleasedbyChromeABoonforReactDevelopers_0.png
tag: Tech
originalTitle: "Anchor Away, The New CSS Anchor Positioning API Released by Chrome A Boon for React Developers"
link: "https://medium.com/stackademic/anchor-away-the-new-css-anchor-positioning-api-released-by-chrome-a-boon-for-react-developers-adda957666a3"
isUpdated: true
---




Chrome의 새로운 CSS Anchor Positioning API가 React 개발자가 곤경에서 벗어나도록 어떻게 도와주는지 살펴보세요😉

![이미지](/assets/img/2024-07-19-AnchorAwayTheNewCSSAnchorPositioningAPIReleasedbyChromeABoonforReactDevelopers_0.png)

웹 개발의 끊임없이 변화하는 환경에서는 개발을 더 효율적으로 만들고 사용자 경험을 향상시키기 위해 지속적으로 새로운 기능과 API가 소개됩니다. 그 중 하나인 최근의 혁신적인 기능은 Chrome에서 출시된 CSS Anchor Positioning API입니다. 이 API는 요소를 다른 요소와의 관계로 배치하는 데 새로운 기능을 제공하여 레이아웃 작업을 더 유연하고 강력하게 만듭니다. 이 글에서는 CSS Anchor Positioning API의 세부사항을 살펴보고 해당 기능을 통해 React 개발자들이 어떻게 크게 이점을 얻을 수 있는지 알아보겠습니다.

# CSS Anchor Positioning API란 무엇인가요?

<div class="content-ad"></div>

CSS Anchor Positioning API는 개발자가 요소를 "앵커" 요소와 관련하여 위치시킬 수 있는 새로운 명세입니다. 이는 복잡한 레이아웃 및 툴팁, 드롭다운, 모달, 그리고 다른 UI 구성 요소를 사용자 상호작용이나 페이지의 다른 요소에 기반하여 동적으로 위치시킬 때 특히 유용합니다.

# CSS Anchor Positioning API의 주요 기능

- 상대 위치 설정: 요소를 포함 블록뿐만 아니라 모든 다른 요소에 상대적으로 위치시킬 수 있습니다.
- 동적 재위치: 뷰포트 변경 및 다른 동적 요소에 기반하여 요소를 자동으로 재배치합니다.
- 향상된 레이아웃 제어: 오프셋 및 정렬을 지정하여 요소가 어떻게 앵커되고 위치할지에 대한 더 큰 제어를 제공합니다.

# 리액트 개발자들을 위한 혜택

<div class="content-ad"></div>

React 개발자들은 특히 CSS 앵커 위치 지정 API를 활용하여 UI 구성 요소를 향상시키고 레이아웃 관리를 간소화할 수 있습니다. 여기 몇 가지 주요 이점이 있습니다:

## 1. 간단화된 툴팁 및 드롭다운 위치 지정

툴팁 및 드롭다운은 종종 버튼이나 아이콘과 같은 다른 요소를 기준으로 배치되어야 합니다. CSS 앵커 위치 지정 API를 사용하면 개발자들은 JavaScript 계산이나 서드파티 라이브러리에 의존하지 않고 이러한 구성 요소를 쉽게 배치할 수 있습니다.

예시:

<div class="content-ad"></div>

```javascript
import React from 'react';
import './Tooltip.css';

const Tooltip = ({ anchorId, content }) => {
  return (
    <div className="tooltip" anchor={anchorId}>
      {content}
    </div>
  );
};

export default Tooltip;

/* Tooltip.css */
.tooltip {
  position: anchor;
  anchor-name: var(--anchor-id);
  top: 0;
  left: 100%;
  margin-left: 10px;
  background-color: #333;
  color: white;
  padding: 5px;
  border-radius: 3px;
}
``` 

## 2. Modal과 팝업 관리 개선

Modal 및 팝업을 관리하는 것은 다양한 화면 크기와 방향을 다룰 때 복잡할 수 있습니다. API는 더 반응형이고 적응 가능한 위치 지정을 가능하게 하여 일관된 사용자 경험을 보장합니다.

예시:

<div class="content-ad"></div>

```js
import React from 'react';
import './Modal.css';

const Modal = ({ anchorId, children }) => {
  return (
    <div className="modal" anchor={anchorId}>
      {children}
    </div>
  );
};

export default Modal;

/* Modal.css */
.modal {
  position: anchor;
  anchor-name: var(--anchor-id);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
```

## 3. 동적 레이아웃 조정

React 애플리케이션은 종종 동적 콘텐츠와 상호 작용하는 요소를 포함합니다. CSS Anchor Positioning API를 사용하면 사용자 상호작용이나 상태 변경에 따라 레이아웃을 동적으로 조정할 수 있으며, 수동 계산 및 업데이트의 필요성을 줄일 수 있습니다.

예시:


<div class="content-ad"></div>

```js
import React, { useState } from 'react';
import './DynamicLayout.css';

const DynamicLayout = () => {
  const [anchorId, setAnchorId] = useState('initial-anchor');

  return (
    <div>
      <button id="initial-anchor" onClick={() => setAnchorId('new-anchor')}>Change Anchor</button>
      <div id="new-anchor" style={{ marginTop: '100px' }}>New Anchor Point</div>
      <div className="dynamic-box" anchor={anchorId}>Dynamic Box</div>
    </div>
  );
};

export default DynamicLayout;

/* DynamicLayout.css */
.dynamic-box {
  position: relative;
  top: 10px;
  left: 10px;
  background-color: lightblue;
  padding: 10px;
  border: 1px solid #007BFF;
}
```

# 흔한 실수와 수정 사항

## 1. 잘못된 Anchor 참조

실수: anchor 속성을 잘못 참조하여 위치 설정 오류가 발생합니다.


<div class="content-ad"></div>

예시:

```js
// 잘못된 사용법: anchorId가 어떤 요소 ID와도 일치하지 않습니다
const Tooltip = ({ anchorId, content }) => {
  return (
    <div className="tooltip" anchor={anchorId}>
      {content}
    </div>
  );
};
```

수정: anchor 속성이 의도된 앵커로 사용할 요소의 ID와 일치하도록 확인하세요.

수정된 예시:

<div class="content-ad"></div>

```js
const Tooltip = ({ anchorId, content }) => {
  return (
    <div className="tooltip" style={ '--anchor-id': anchorId }>
      {content}
    </div>
  );
};
```

## 2. Not Handling Viewport Changes

Mistake: 화면 크기나 방향 변경 시 요소가 올바르게 위치하지 않게되는 뷰포트 변경을 고려하지 않는 것.

Example:

<div class="content-ad"></div>

```js
// 도구 설명이 뷰포트 변경 시 깨질 수 있습니다
<div className="tooltip" anchor={anchorId}>
  도구 설명 내용
</div>
```

해결 방법: 반응형 위치 지정을 위해 vh, vw와 미디어 쿼리와 같은 CSS 단위를 사용해주세요. 또한, 다양한 기기와 화면 크기에서 애플리케이션을 철저히 테스트해주세요.

수정된 예시:

```js
/* Tooltip.css */
.tooltip {
  position: anchor;
  anchor-name: var(--anchor-id);
  top: 0;
  left: 100%;
  margin-left: 10px;
  background-color: #333;
  color: white;
  padding: 5px;
  border-radius: 3px;
  @media (max-width: 600px) {
    left: 50%;
    transform: translateX(-50%);
  }
}
```

<div class="content-ad"></div>

## 3. 겹치는 요소와 Z-Index 문제

실수: 적절하지 않은 z-index 관리로 인해 겹치는 요소들이 일부 요소에 접근할 수 없거나 시각적으로 올바르지 않게 표시될 수 있습니다.

예시:

```js
// 겹치는 툴팁이 올바르게 표시되지 않을 수 있음
<div className="tooltip" anchor={anchorId}>
  툴팁 내용
</div>
```

<div class="content-ad"></div>

수정: 요소가 올바르게 쌓이도록 적절한 z-index 값을 할당하세요.

올바른 예시:

```js
/* Tooltip.css */
.tooltip {
  position: anchor;
  anchor-name: var(--anchor-id);
  z-index: 1000; /* 툴팁이 다른 요소 위에 표시되도록 */
}
```

## 4. 지원되지 않는 브라우저에 대한 예외 처리하기

<div class="content-ad"></div>

잘못된 점: 모든 사용자가 CSS Anchor Positioning API를 지원하는 브라우저를 사용한다고 가정하여 API를 지원하지 않는 브라우저에서 레이아웃이 깨지는 문제가 발생했습니다.

수정 방법: API를 지원하지 않는 브라우저에 대해 전통적인 CSS 위치 지정을 사용하여 대체 방안을 제공해야 합니다.

수정된 예시:

```css
/* Tooltip.css */
.tooltip {
  position: anchor;
  anchor-name: var(--anchor-id);
  /* Fallback */
  position: absolute;
  top: 0;
  left: 100%;
}
```

<div class="content-ad"></div>

## 5. useEffect에서 부작용을 정리하지 않기

실수: useEffect에서 부작용을 정리하지 않아 메모리 누수와 원치 않는 동작이 발생할 수 있습니다.

예시:

```js
useEffect(() => {
  const handleResize = () => {
    // 리사이징 로직 처리
  };
  window.addEventListener('resize', handleResize);
  // 정리 함수가 누락됨
}, []);
```

<div class="content-ad"></div>

수정: 항상 useEffect에서 클린업 함수를 반환하여 이벤트 리스너나 다른 부수 효과를 제거하세요.

예시 수정:

```js
useEffect(() => {
  const handleResize = () => {
    // 리사이즈 로직 처리
  };
  window.addEventListener('resize', handleResize);
  return () => {
    window.removeEventListener('resize', handleResize);
  };
}, []);
```

## 6. 상태(State) 및 효과(Effect) 의존성 잘못 사용하기

<div class="content-ad"></div>

잘못: useEffect에서 상태나 의존성을 올바르게 관리하지 않아 불필요한 다시 렌더링이나 누락된 업데이트가 발생하는 경우가 있습니다.

예시:

```js
const [data, setData] = useState(null);
useEffect(() => {
  fetch('/api/data')
    .then(response => response.json())
    .then(data => setData(data));
}, []); // 필요한 의존성이 누락된 상태
```

해결: 이전 데이터가 사용되지 않도록 useEffect 의존성 배열에 필요한 모든 의존성을 포함해야 합니다.

<div class="content-ad"></div>

친구야, 여기 수정된 예제야:

```js
const [data, setData] = useState(null);
useEffect(() => {
  fetch('/api/data')
    .then(response => response.json())
    .then(data => setData(data));
}, [setData]); // setData를 의존성으로 포함
```

# CSS 앵커 위치 지정 API 사용의 최상의 방법
- 명확한 앵커 정의하기:

<div class="content-ad"></div>

**친절한말투로 번역:**

- 의미 있는 ID 및 변수를 사용하여 앵커를 정의하면 명확성과 유지 관리가 가능합니다.

2. 호환성을 위한 대비책:

- 모든 사용자에게 일관된 경험을 보장하기 위해 API를 지원하지 않는 브라우저에 대비책을 제공하세요.

3. 반응형 디자인:

<div class="content-ad"></div>

- API를 활용하여 반응형 디자인 기술과 함께 사용하여 요소가 다른 기기에서 올바르게 재배치되도록합니다.

4. 성능 고려 사항:

- 잠재적인 성능 병목 현상을 피하기 위해 동적으로 위치 지정된 요소의 수를 최소화하세요.

5. 철저히 테스트하세요:

<div class="content-ad"></div>

- 서로 다른 브라우저, 장치 및 시나리오에서 애플리케이션을 철저히 테스트하여 위치 지정이 예상대로 작동하는지 확인하십시오.

# 할 일과 하지 말아야 할 일

## 할 일

- 동적 UI 요소에 사용: 툴팁, 모달, 팝업 및 기타 동적 요소를 위해 API를 활용하십시오.
- 반응형 테스트: 요소가 다른 장치 및 화면 크기에 맞게 올바르게 위치하도록 확인하십시오.
- 상태 관리와 결합: React의 상태 관리를 사용하여 앵커와 위치를 동적으로 조정하십시오.

<div class="content-ad"></div>

## 주의할 점

- 정적 레이아웃을 복잡하게 만들지 마세요: 정적 레이아웃의 경우, 전통적인 CSS 위치 지정 방법이 여전히 적합할 수 있습니다.
- 접근성 무시하지 마세요: 동적으로 위치 지정된 요소가 모든 사용자에게 접근 가능하도록 하세요. 스크린 리더 사용자를 포함해 모든 사용자들이 이용할 수 있도록 해야 합니다.
- 과용하지 마세요: 전통적인 위치 지정 방법으로 충분한 간단한 작업에 API를 지나치게 사용하지 마세요.

## 주의할 점과 일반적인 오류

- 뷰포트 변경: 뷰포트 변경 시 요소들이 올바르게 재배치되는지 확인하세요. 서로 다른 기기에서 테스트를 철저히 진행해 보세요.
- 성능 문제: API는 위치 지정을 단순화하지만, 특히 복잡한 UI에서 성능 병목 현상을 일으키지 않도록 주의하세요.
- 호환성: 새로운 API와 마찬가지로, 브라우저 호환성을 확인하고 호환되지 않는 브라우저에 대비책을 마련하세요.

<div class="content-ad"></div>

# 결론

CSS Anchor Positioning API는 웹 개발 툴킷에 강력한 기능을 추가하여 위치 지정 및 레이아웃 관리에 새로운 가능성을 제공합니다. React 개발자에게는 동적이고 반응형 UI 컴포넌트를 간편하게 만들어줍니다. 장점과 잠재적인 함정을 이해함으로써, 개발자들은 이 API를 활용하여 응용 프로그램을 향상시키고 사용자 경험을 개선할 수 있습니다.

이 새로운 API는 요소의 위치 지정을 보다 직관적이고 오류 가능성이 적은 방식으로 처리할 수 있게 하므로, 개발자들은 훌륭한 기능을 구축하는 데 더 집중하고 복잡한 위치 지정 로직을 다루는 데는 덜 시간을 써야 합니다.

# Stackademic 🎓

<div class="content-ad"></div>

끝까지 읽어 주셔서 감사합니다. 이제 가시기 전에:

- 작가를 응원하고 팔로우해 주시면 감사하겠습니다! 👏
- 저희를 팔로우해 주세요: X | LinkedIn | YouTube | Discord
- 다른 플랫폼에서도 만나보세요: In Plain English | CoFeed | Differ
- Stackademic.com에서 더 많은 콘텐츠를 만나보세요.