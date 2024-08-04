---
title: "React 19의 새로운 기능 최신 기능 심층 탐구"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-04 19:39
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Whats New in React 19 A Deep Dive into the Latest Features"
link: "https://dev.to/vyan/whats-new-in-react-19-a-deep-dive-into-the-latest-features-3gp2"
---


리액트는 계속 발전하며, 개발자들에게 다이내믹하고 효율적인 웹 애플리케이션을 구축할 수 있는 강력한 도구를 제공합니다. 리액트 19의 릴리즈가 예정된 가운데, 몇 가지 혁신적인 기능들이 도입될 예정이며, 이는 리액트로 개발하는 방식을 혁신할 것입니다. 이 블로그 포스트에서는 이러한 새로운 기능들을 탐색하고, 각 개념을 설명하는 예시와 함께 혜택을 강조할 것입니다.

## 1. 리액트 컴파일러: 자동 재렌더링 최적화

### 현재 문제:

useMemo, useCallback 및 memo API를 사용한 재렌더링의 수동 최적화는 번거롭고 실수를 유발할 수 있습니다.

<div class="content-ad"></div>

### 극복:

새로운 React 컴파일러는 다시 렌더링의 최적화를 자동화하여 더 깨끗하고 효율적인 코드를 만들 수 있습니다. 이 기능은 이미 Instagram에서 활용되고 있으며, 제작 환경에서 효과적임을 입증하고 있습니다.

### 혜택:

- 자동 최적화: React 컴파일러는 다시 렌더링을 자동으로 처리하여 수동 개입이 줄어듭니다.
- 깨끗한 코드: 개발자들은 성능 최적화를 걱정하지 않고 간단하고 가독성이 좋은 코드를 작성할 수 있습니다.
- 제작 환경에서 검증됨: 이미 Instagram에서 사용 중이며, 신뢰성과 성능 이점을 보장합니다.

<div class="content-ad"></div>

### 예시:

React 19 이전:

```js
import React, { useMemo, useCallback } from 'react';

function ExpensiveComponent({ data, onItemClick }) {
  const processedData = useMemo(() => {
    // 비용이 많이 드는 계산
    return data.map(item => item * 2);
  }, [data]);

  const handleClick = useCallback((item) => {
    onItemClick(item);
  }, [onItemClick]);

  return (
    <ul>
      {processedData.map(item => (
        <li key={item} onClick={() => handleClick(item)}>{item}</li>
      ))}
    </ul>
  );
}
```

React 19에서:

<div class="content-ad"></div>

```js
import React from 'react';

function ExpensiveComponent({ data, onItemClick }) {
  const processedData = data.map(item => item * 2);

  return (
    - [ // key={item} & onClick={() => onItemClick(item)} 구문 선택
    -   <ul>
         {processedData.map(item => (
    -       <li key={item} onClick={() => onItemClick(item)}>{item}</li>
    +       - {item}
         ))}
    -   </ul>
    - ]
  );
}
```

이 예시에서 React 19 컴파일러는 자동으로 최적화하여 다시 렌더링을 줄여, 수동으로 useMemo와 useCallback을 사용할 필요가 없어졌습니다.

## 2. Server Components: SEO 및 초기 로드 속도 향상

### 현재 문제:

<div class="content-ad"></div>

클라이언트 측 컴포넌트는 SEO 효과를 제한하고 초기로드 시간을 증가시킬 수 있습니다.

### 극복하는 방법:

서버 컴포넌트를 사용하면 React가 서버 측에서 컴포넌트를 실행할 수 있어 초기 페이지 로드가 빨라지고 SEO가 향상됩니다.

### 혜택:

<div class="content-ad"></div>

- 초기로드 속도 향상: 서버에서 컴포넌트를 렌더링하여 초기로드 시간이 크게 단축됩니다.
- SEO 개선: 서버 측 렌더링은 완전히 렌더링된 HTML을 검색 엔진에 제공하여 SEO를 향상시킵니다.
- 원활한 실행: 서버 측 및 클라이언트 측 렌더링 간의 전환이 원활하여 사용자 경험을 쾌적하게 제공합니다.

### 예시:

```js
// UserProfile.server.js
import { db } from './database';

async function UserProfile({ userId }) {
  const user = await db.user.findUnique({ where: { id: userId } });

  return (
    <div>
      <h1>{user.name}</h1>
      <p>Email: {user.email}</p>
      <p>가입일: {user.createdAt}</p>
    </div>
  );
}

export default UserProfile;
```

이 예시에서 UserProfile 컴포넌트는 서버 컴포넌트입니다. 서버에서 데이터베이스에서 사용자 데이터를 직접 가져와 HTML을 렌더링하고 클라이언트로 보냅니다. 이를 통해 초기로드 시간과 SEO가 개선되며, 내용이 즉시 HTML에서 사용 가능하게 됩니다.

<div class="content-ad"></div>

## 3. 동작: 양식 처리 간소화

### 현재 문제:

폼 제출은 클라이언트 측 이벤트로 제한되어 있어 동기적 및 비동기적 작업 처리를 복잡하게 만들 수 있습니다.

### 극복 방안:

<div class="content-ad"></div>

예제:

동작은 onSubmit 이벤트를 대체하여 서버 측에서 양식 처리를 실행할 수 있도록 합니다. 이 단순화로 인해 강력하고 효율적인 데이터 제출 관리가 가능해집니다.

### 이점:
- 서버 측 실행: 양식이 이제 서버 쪽에서 처리될 수 있어 데이터 제출을 더 쉽게 관리할 수 있습니다.
- 관리 단순화: 동작은 양식 처리를 최적화하며 클라이언트 측 양식 제출과 관련된 복잡성을 줄입니다.
- 비동기 작업 지원: 양식 내에서 동기식 및 비동기식 작업을 쉽게 처리할 수 있습니다.

<div class="content-ad"></div>

```js
// LoginForm.js
import { useFormState } from 'react-dom';

function LoginForm() {
  const [state, formAction] = useFormState(loginAction, { error: null });

  return (
    <form action={formAction}>
      <input type="email" name="email" required />
      <input type="password" name="password" required />
      <button type="submit">Log in</button>
      {state.error && <p>{state.error}</p>}
    </form>
  );
}

// loginAction.js
async function loginAction(prevState, formData) {
  const email = formData.get('email');
  const password = formData.get('password');

  try {
    await authenticateUser(email, password);
    return { error: null };
  } catch (error) {
    return { error: 'Invalid credentials' };
  }
}
```

이 예시에서는 loginAction이 서버에서 실행되어 인증 프로세스를 처리하고 그 결과를 반환하여 양식 상태를 업데이트합니다.

## 4. 웹 컴포넌트: React로 쉽게 통합하기

### 현재 이슈:

<div class="content-ad"></div>

웹 컴포넌트를 React에 통합하는 것은 복잡했습니다. 종종 추가 패키지와 코드 변환이 필요했죠.

### 극복:

React 19은 웹 컴포넌트를 간단하게 통합할 수 있도록 하여 추가 패키지 없이도 원활한 통합이 가능합니다.

### 혜택:

<div class="content-ad"></div>

- 원활한 통합: 추가 부담 없이 웹 구성 요소를 React 프로젝트에 쉽게 통합할 수 있습니다.
- 추가 패키지 불필요: 추가 패키지나 복잡한 코드 변환 없이 처리할 수 있습니다.
- 기존 구성 요소 활용: 개발자들은 React 애플리케이션 내에서 기존 웹 구성 요소를 손쉽게 사용할 수 있습니다.

### 예시:

```js
import React from 'react';

function App() {
  return (
    <div>
      <h1>내 앱</h1>
      <my-custom-element>
        React 내 웹 구성 요소 사용하기
      </my-custom-element>
    </div>
  );
}

export default App;
```

이 예시에서 `my-custom-element`는 React 컴포넌트 내에서 추가 래퍼나 라이브러리 없이 직접 사용할 수 있는 웹 구성 요소입니다.

<div class="content-ad"></div>

## 5. 문서 메타데이터: React 컴포넌트에서 직접 관리

### 현재 문제:

제목 및 메타 태그와 같은 메타데이터를 관리하는 것은 종종 추가 라이브러리가 필요하며, 프로젝트에 복잡성을 추가하는 일이 다반사입니다.

### 극복:

<div class="content-ad"></div>

React 19가 개발자가 React 컴포넌트 내에서 문서 메타데이터를 직접 관리할 수 있도록 해 SEO 및 접근성을 향상시키는 과정을 간소화합니다.

### 장점:

- 간소화된 메타데이터 관리: 외부 라이브러리에 의존하지 않고 React 컴포넌트 내에서 메타데이터를 직접 관리할 수 있습니다.
- 향상된 SEO: 메타데이터의 개선된 관리는 SEO를 향상시킬 수 있습니다.
- 더 나은 접근성: 문서 메타데이터의 직접적인 관리는 애플리케이션의 접근성을 향상시킬 수 있습니다.

### 예시:

<div class="content-ad"></div>

```js
import React from 'react';
import { Metadata } from 'react';

function ProductPage({ product }) {
  return (
    <>
      <Metadata>
        <title>{product.name} | My Store</title>
        <meta name="description" content={product.description} />
        <meta property="og:title" content={product.name} />
        <meta property="og:description" content={product.description} />
        <meta property="og:image" content={product.imageUrl} />
      </Metadata>
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      <img src={product.imageUrl} alt={product.name} />
    </>
  );
}
```

이 예시에서 Metadata 컴포넌트는 React 컴포넌트 내에서 문서 메타데이터를 직접 관리할 수 있게 해주어 SEO와 접근성을 개선하며 외부 라이브러리 없이 작업할 수 있게 합니다.

## 결론

React 19는 개발을 간편화하고 응용 프로그램 성능을 향상시키기 위한 다양한 새로운 기능을 소개합니다. 새로운 React 컴파일러를 통한 자동 다시 렌더링 최적화부터 웹 컴포넌트와 Actions를 통한 개선된 폼 처리의 원활한 통합까지, 이러한 기능들은 React로 작업하는 방식을 변화시킬 것입니다. 현재의 문제를 해결하고 견고한 솔루션을 제공함으로써, React 19는 프로젝트가 효율적이고 확장 가능하며 유지보수하기 좋도록 보장합니다.


<div class="content-ad"></div>

최종 릴리스를 기다려주시고 React 개발을 더 발전시킬 수 있는 흥미로운 새로운 기능을 탐험해 보세요!