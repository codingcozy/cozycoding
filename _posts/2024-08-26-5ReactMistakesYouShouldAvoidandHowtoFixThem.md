---
title: "피해야 할 5가지 React 실수와 그 해결 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 20:00
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "5 React Mistakes You Should Avoid (and How to Fix Them)"
link: "https://dev.to/vyan/5-react-mistakes-you-should-avoid-and-how-to-fix-them-339m"
isUpdated: false
---


리액트 개발자로서, 처음에는 편리해 보일 수 있지만 결국 문제를 일으킬 수 있는 특정 코딩 패턴에 빠지기 쉽습니다. 이 블로그 포스트에서는 5가지 일반적인 리액트 실수를 살펴보고, 이를 피하는 방법에 대해 논의하여 코드가 효율적이고 유지보수 가능하며 확장 가능하도록 보장합니다.

### 1. Key 속성 오용

잘못된 예시:

```js
{myList.map((item, index) => <div key={index}>{item}</div>)}
```

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

리스트에서 색인을 키로 사용하면 성능 문제와 버그가 발생할 수 있으니 주의해야 해요, 특히 리스트가 변할 수 있는 경우 더욱 그렇습니다.

옳은 방법:

```js
{myList.map(item => <div key={item.id}>{item.name}</div>)}
```

데이터에서 고유 식별자인 id 필드와 같은 값을 key prop으로 사용해야 합니다.

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

### 2. State 과도한 사용

해당 실수:

```js
function MyComponent() {
  const [value, setValue] = useState(0);
  // 변화가 없음
  return <div>{value}</div>;
}
```

변화가 없는 상태도 모두 상태에 넣으면 불필요한 다시 렌더링과 복잡성을 야기할 수 있습니다.

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

올바른 방법:

```js
function MyComponent({ value }) {
  return <div>{value}</div>;
}
```

실제로 변경되는 데이터에 대해서만 상태를 사용하세요. 정적 데이터에는 프롭스나 컨텍스트를 사용하세요.

### 3. useEffect를 올바르게 활용하지 않은 경우

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

잘못된 부분:

```js
useEffect(() => {
  fetchData();
}, []);
```

useEffect에 종속성을 지정하지 않으면 예상치 못한 동작이나 무한 루프가 발생할 수 있습니다.

올바른 방법:

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
useEffect(() => {
  fetchData();
}, []);
```

효과가 작동하는 시기를 제어하기 위해 항상 종속성 배열을 명시하세요.

### 4. Prop Drilling

실수:

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

```jsx
<Grandparent>
  <Parent>
    <Child prop={value} />
  </Parent>
</Grandparent>
```

여러 레이어의 컴포넌트를 통해 props를 전달하면 코드를 유지하기 어렵게 만듭니다.

올바른 방법: (Context API 예시)

```jsx
const ValueContext = React.createContext();
<ValueContext.Provider value={value}>
  <Child />
</ValueContext.Provider>

function Child() {
  const value = useContext(ValueContext);
  return <div>{value}</div>;
}
```

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

컨텍스트 API 또는 상태 관리 라이브러리를 사용하여 프롭 드릴링을 피하세요.

### 5. 구성 무시

실수:

```js
function UserProfile({ user }) {
  return (
    <div>
      <Avatar src={user.avatar} />
      <Username name={user.name} />
      {/* 더 많은 사용자 세부 정보 */}
    </div>
  );
}
```  

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

단일하고 유연한 구조로 구성 요소를 생성하는 대신에 재사용 가능한 요소를 만들어보세요.

올바른 방법:

```js
function UserProfile({ children }) {
  return <div>{children}</div>;
}

<UserProfile>
  <Avatar src={user.avatar} />
  <Username name={user.name} />
  {/* 더 많은 사용자 세부 정보 또는 다른 레이아웃 */}
</UserProfile>
```

유연성을 위해 구성 요소를 자식 또는 렌더 프롭으로 받을 수 있도록 디자인하세요.

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

이 5가지 React 코딩 실수를 이해하고 피함으로써 더 효율적이고 유지보수가 쉬우며 확장 가능한 React 애플리케이션을 작성할 수 있습니다. React 기술을 발전시키는 동안 이러한 교훈을 기억하고, 필요할 때 언제든지 이 블로그 글을 다시 확인하도록 해보세요.

결론
이러한 흔한 React 실수를 피함으로써 더 효율적이고 유지보수가 쉽고 확장 가능한 코드를 작성할 수 있습니다. 고유한 키를 사용하고, 상태를 현명하게 관리하고, useEffect를 올바르게 활용하며, 프롭 드릴링을 피하고, 유연한 UI 디자인을 위해 합성을 채용하세요. 이러한 모범 사례를 적용하면 React 애플리케이션이 더 견고하고 작업하기 쉬워질 것입니다.