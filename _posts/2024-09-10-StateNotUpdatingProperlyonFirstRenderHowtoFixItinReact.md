---
title: "리액트에서 처음 렌더링 시 상태가 제대로 업데이트되지 않을 때  해결하는 방법"
description: ""
coverImage: "/assets/img/2024-09-10-StateNotUpdatingProperlyonFirstRenderHowtoFixItinReact_0.png"
date: 2024-09-10 18:32
ogImage: 
  url: /assets/img/2024-09-10-StateNotUpdatingProperlyonFirstRenderHowtoFixItinReact_0.png
tag: Tech
originalTitle: "State Not Updating Properly on First Render  How to Fix It in React"
link: "https://medium.com/javascript-in-plain-english/state-not-updating-properly-on-first-render-how-to-fix-it-in-react-f8ae88b80240"
isUpdated: false
---


<img src="/assets/img/2024-09-10-StateNotUpdatingProperlyonFirstRenderHowtoFixItinReact_0.png" />

React에서 첫 번째 렌더링에서 상태가 올바르게 업데이트되지 않는-frustrating한 문제를 겪어 본 적이 있나요? 😩 새 데이터를 기존 상태와 병합하기 위해 전개 연산자를 사용했는데, 대신에 데이터가 예상대로 업데이트되지 않는 것을 어려워하고 있을 수 있습니다. 이 블로그에서는 이러한 문제가 발생하는 이유와 간단하면서도 효과적인 접근 방법을 통해 어떻게 해결할 수 있는지 살펴보겠습니다. 시작해 봅시다! 🚀

# 문제: 첫 번째 렌더링 시 상태가 올바르게 업데이트되지 않음

다음 시나리오를 고려해보세요: React 구성요소에서 전개 연산자를 사용하여 상태를 업데이트하려고 합니다. 아래는 전형적인 예시입니다:

<div class="content-ad"></div>

```js
const [user, setUser] = useState({ name: "Nirali", age: 20 });
useEffect(() => {
  setUser({ ...user, age: 21});
}, []);
```

이론적으로 나이를 20에서 21로 업데이트할 것으로 예상하지만 이름은 "Nirali"로 유지할 것으로 예상됩니다. 그러나 첫 번째 렌더링에서 올바르게 업데이트되지 않을 수 있으며, 나이는 여전히 20일 수 있고, 컴포넌트가 변경 사항을 반영하지 않을 수 있습니다. 😮

# 원인: 스프레드 연산자의 직접 사용

이 문제는 React가 상태 업데이트를 처리하는 방식 때문에 발생합니다. React의 useState와 useEffect는 비동기적으로 작동하기 때문에, 이전 상태가 완전히 처리되기 전에 직접적으로 상태를 업데이트하려고 할 때 React가 아직 전 상태를 완전히 처리하지 못할 수 있습니다.

<div class="content-ad"></div>

위의 예시에서:

```js
setUser({ ...user, age: 21 });
```

스프레드 연산자가 사용된 것은 React가 상태를 업데이트하는 작업을 완료하지 못한 구버전의 사용자 상태를 기준으로 한 것입니다. 이로 인해 첫 번째 렌더링에서 나이가 동일한 채로 유지됩니다. 이 문제는 이전 상태를 기반으로 상태를 업데이트할 때 특히 발생합니다.

# 해결책: 임시 변수 사용하기 🛠️

<div class="content-ad"></div>

이 문제를 피하기 위한 간단한 해결책은 이전 상태와 새 데이터를 함께 저장하는 임시 변수를 사용하는 것입니다. 이렇게 하면 첫 번째 렌더 중에 오래된 상태에 의존하지 않도록 할 수 있습니다.

다음은 문제를 해결하는 방법입니다:

```js
const [user, setUser] = useState({ name: "Nirali", age: 20 });

useEffect(() => {
  const updatedUser = { ...user, age: 21 };  // 이전 상태를 임시 변수에 저장
  setUser(updatedUser);  // 새 데이터로 상태 업데이트
}, []);
```

임시 변수(updatedUser)를 생성함으로써 상태가 올바르게 캡처되고 업데이트되기 전에 정확하게 포착되도록 보장합니다. 이제 첫 번째 렌더에서 상태가 문제없이 업데이트된 나이 값이 반영됩니다.

<div class="content-ad"></div>

# 왜 이렇게 동작하는지 🌟

React의 상태 업데이트는 비동기적으로 이루어지며, 이전 상태를 올바르게 캡처하지 않고 상태를 직접 업데이트하는 것은 예기치 못한 동작을 일으킬 수 있습니다. spread 연산을 임시 변수에 저장함으로써 React가 상태 업데이트를 올바른 순서로 처리하도록 하여, 컴포넌트가 시작부터 예상한 데이터를 렌더링하게 합니다.

# 결론 🌈

React의 useState와 spread 연산자는 가끔 첫 렌더링에서 문제를 일으킬 수 있으며, 상태 업데이트가 올바르게 반영되지 않을 때 특히 문제를 일으킬 수 있습니다. 상태를 설정하기 전에 데이터를 임시 변수에 저장하면 이러한 문제를 방지하고 상태 업데이트가 부드럽고 예측 가능하게 이루어지도록 할 수 있습니다.

<div class="content-ad"></div>

# React State 처리에 대한 빠른 팁 💡

- 임시 변수 사용하기: 전개 연산자를 사용할 때는 항상 결합된 상태를 정확하게 저장하기 위해 임시 변수에 저장하세요.
- 비동기 동작에 주의하기: React는 상태 업데이트를 비동기적으로 처리하므로 타이밍이 컴포넌트 동작에 영향을 줄 수 있습니다.
- 상태 업데이트 재확인하기: 특히 복잡한 상태에서 첫 번째 렌더링 시, 상태가 예상대로 업데이트되는지 확인하세요.

이러한 관례를 따르면 useState를 사용할 때 흔히 발생하는 문제를 피하고 React 앱을 원활하게 유지할 수 있습니다. 🚀

좋은 코딩 되세요! 👩‍💻👨‍💻

<div class="content-ad"></div>

# 친절히 참여해 주셔서 감사합니다! 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

- 작가를 박수치고 팔로우해주세요 ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 다양한 콘텐츠 만나보세요