---
title: "양방향 바인딩이 한 방향 도로가 될 수 있는 이유"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-04 19:42
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Two-way Binding can be a One-way Street"
link: "https://dev.to/mlrawlings/two-way-binding-can-be-a-one-way-street-1o3"
---


라이언이 양방향 데이터 바인딩에 대해 멋진 글을 썼는데... 그러나 데이터 흐름에 대한 접근 방식에서 Marko가 Vue와 같은 범주로 투영되는 것에 약간의 문제가 있다고 느끼는군요. 그래서 정확한 사실을 말하고 싶었습니다.

그리고 아마도 다른 프레임워크가 채택할 수 있는 더 나은 방법을 보여줄 수 있을지도 모릅니다. 🤷

## 요약: 양방향 바인딩의 문제점

라이언은 양방향 바인딩의 문제점을 설명하며, 이는 손쉬운 몇 가지 문제로 요약됩니다:

<div class="content-ad"></div>

### 1. 예측 불가능한 데이터 흐름

### 2. 무한 루프의 가능성

### 3. 예측할 수 없는 성능

### 4. 리팩토링 제한

<div class="content-ad"></div>

## 이 문제들 해결하기

이 문제들은 Marko 팀에서 철저히 고민해 본 것입니다. 그리고 우리가 양방향 바인딩의 간결함을 유지하면서 단방향 데이터 흐름을 유지하고 위에 나열된 문제를 피할 수 있는 해결책이 있습니다.

해결책: 규칙 🤝 & 수간 🍬.

하지만 처음부터 시작해보죠. "Read/Write Segregation Everywhere"란 제목 아래 Ryan이 보여준 Solid 예시를 살펴보세요.

<div class="content-ad"></div>

```js
function App() {
  const [name, setName] = createSignal("world");

  // 어떻게 이것을 양방향으로 바인딩하나요? 그렇지 않아요...
  return <Input value={name()} onUpdate={setName} />
}

function Input(props) {
  return <input
    value={props.value}
    onInput={e => props.onUpdate(e.target.value)}
  />
}
```

여기에서 우리는 데이터의 명시적인 일방향 흐름을 가지고 있어요:

```js
onInput → onUpdate → setName → <Input>.value → <input>.value
```

### Marko는 어떨까요?

<div class="content-ad"></div>

Marko에서는 동일한 컴포넌트가 매우 유사하게 보입니다:

```js
<let/name="world" />
<Input value=name onUpdate(v) { name = v } />
```

```js
// Input.marko
<input
  value=input.value
  onInput(e) { input.onUpdate(e.target.value) }
>
```

### 컨벤션 추가하기 🤝

<div class="content-ad"></div>

이것은 일반적인 패턴이기 때문에 주변에 일부 관례를 추가해봅시다. 값 전파만을 목적으로 하는 이벤트가 존재한다면, 해당 이벤트의 이름을 그대로 지정해봅시다:

```js
<let/name="world" />
<Input value=name valueChange(v) { name = v } />
```

```js
// Input.marko
<input
  value=input.value
  onInput(e) { input.valueChange(e.target.value) }
>
```

이전 코드와 정확히 동일하지만, onChange가 이제 valueChange로 변경되었습니다 (값 속성과 일치시키기 위해). 이 관례는 Marko에서 데이터를 트리 상위로 전파하는 권장 방법입니다: 다른 속성에 대한 변경 핸들러를 전달할 때 속성 이름 뒤에 Change를 추가하세요.

<div class="content-ad"></div>

아마도 특정한 NameInput을 만들어서 이렇게 사용할 수 있을 거에요:

```js
<NameInput name=name nameChange(v) { name = v } />
```

### 설탕 추가 🍬

이제 우리가 이런 규칙을 가지고 있어서 someAttribute와 someAttributeChange가 서로 대응하는 것을 쉽게 볼 수 있습니다. 컴파일러에게도 쉽게 보일 거에요.

<div class="content-ad"></div>

마르코는 := 단축키를 소개했어요. 이 두 줄을 동일하게 만들어요:

```js
<Input value:=name />
<Input value=name valueChange(v) { name = v } />
```

:=를 사용하면 더 간결해지지만 여전히 자식에게 값을 업데이트하는 함수를 명시적으로 제공합니다. 단지 구문 설탕일 뿐이에요. 자식은 부모가 이 단축키를 사용했는지 여부에 관심이 없어요. 우리는 사고의 국소화를 잃지 않았어요.

### DOM 업그레이드

<div class="content-ad"></div>

이 컨벤션은 정말 좋은 것 같아요, 하지만 `input`을 사용하는 간단한 경우에 대해서는 어떻게 할까요?

Marko는 네이티브 HTML 요소에 여러 *변경* 속성을 추가합니다. 그래서 onInput 이벤트를 사용하는 대신, valueChange를 사용할 수 있어요:

```js
<input
  value=input.value
  valueChange=input.valueChange
>
```

또한, 아래의 두 가지 방법은 위의 코드와 동일해요:

<div class="content-ad"></div>

```js
<input value={input.value}>
<input {...input}>
```

정말 좋아요! 일반 전파 케이스에 대해 :=를 사용할 수 있어요. 이렇게 하면 자식에게 변경을 요청할 수 있는 방법을 제공하는 것이 분명하고, 필요하다면 고유한 valueChange 함수를 추가할 수 있어요. 내 앱의 다른 곳을 다시 작성할 필요가 없는 장점이 있어요.

### 추가 혜택: 제어 가능한 컴포넌트

이전에 컴포넌트에 대한 "제어된" 및 "제어되지 않은" 용어를 들어본 적이 있을 수 있어요. 간단히 복습해 볼게요:

<div class="content-ad"></div>

- 제어 컴포넌트는 상태를 부모로부터 받습니다. 데이터 변경을 요청할 수는 있지만 (이벤트를 통해), 실제로는 제어하지 않습니다.

```js
<button onClick() { input.countChange(input.count+1) }>
    ${input.count}    
</button>
```

- 비제어 컴포넌트는 자체 상태를 소유하며 직접 업데이트할 수 있습니다.

```js
<let/count=0 />
<button onClick() { count += 1 }>
    ${count}    
</button>
```

<div class="content-ad"></div>

일반적으로 네이티브 HTML 요소는 제어되지 않습니다. `input`에 초기값을 설정할 수 있지만 한 번 입력을 시작하면 `input`은 자체 상태를 유지합니다.

그러나 어플리케이션 상태에 의해 제어되는 폼 요소(또는 기타 네이티브 요소)를 가지는 것이 종종 유용합니다.

#### 다른 프레임워크

React는 이를 인식합니다: `input`에서 `value`(제어됨)와 `defaultValue`(제어되지 않음)을 모두 지원합니다.

<div class="content-ad"></div>

만약 리스너 없이 값을 사용한다면, 읽기 전용 입력을 얻게 됩니다:

```js
<input value="world">
```

대부분의 다른 프레임워크들이 부분적으로 제어된 상태에서 작동하는 반면, `input`은 자체 내부 상태를 유지하지만 값을 업데이트할 수 있습니다. 그래서 두 가지가 항상 동기화되어 있는 것을 보장할 수 없습니다.

하지만 React도 일관성이 없습니다. 예를 들어 React의 `dialog`은 open과 defaultOpen을 가지고 있지 않습니다. 이것 또한 부분적으로 제어된 상태에서 작동합니다.

<div class="content-ad"></div>

#### 마르코의 해결책

마르코에서는 변경 핸들러를 사용하여 제어를 원하는 의사를 전달합니다. 변경 사항을 듣지 않으면 제어되지 않는 구성 요소가 됩니다. 변경 사항을 듣는다면 해당 값에 대한 모든 책임을 집게 됩니다.

이를 설명하기 위해, 마르코에서는 다음과 같이 `input`을 생성하여 사용자의 키 스트로크를 무시합니다:

```js
<input value="world" valueChange() {}>
```

<div class="content-ad"></div>

`valueChange`를 전달했기 때문에 입력을 제어할 수 있지만 빈 함수이기 때문에 상태가 업데이트되지 않습니다. `input`은 우리의 키 입력을 무시합니다.

#### 컴포넌트로 확장하기

이 제어 또는 비제어 모드로 작동하는 능력은 기본 태그에만 유용한 것은 아닙니다. 우리만의 제어 가능한 컴포넌트를 작성할 수 있기를 원합니다!

Marko는 핵심 상태 원시 요소인 `let` 태그를 제어할 수 있도록하여 이를 가능케 합니다.

<div class="content-ad"></div>

위 예시는 다음과 같이 표현됩니다:

```js
<let/count=0 />
<button onClick() { count += 1 }>
  ${count}    
</button>
```

Marko에서는 값으로 기본 설정된 이름 없는 속성이 있으므로 다음 두 예시는 동일합니다:

```js
<let/count=0 />
<let/count value=0 />
```

<div class="content-ad"></div>

이 사용 방식에서 `let`은 제어되지 않습니다: 자체 내부 상태를 유지하고 우리에게 제공합니다.

하지만 값 변경 핸들러를 전달하면 자체 상태를 유지하지 않고 전달된 값에 반영됩니다. 예를 들어 이 카운터는 클릭할 때마다 count를 업데이트하지 않고 경고(1)을 표시할 것입니다.

```js
<let/count value=0 valueChange(v) { alert(v) } />
<button onClick() { count += 1 }>
  ${count}    
</button>
```

그래요, 이게 어떻게 유용한가요? 부모로부터 선택적 변경 핸들러를 전달할 수 있습니다:

<div class="content-ad"></div>

```js
<let/count value=input.value valueChange=input.valueChange />
```

이제, 부모가 valueChange를 전달하면 내부 count를 제어합니다. 전달하지 않으면 `let`이 count를 유지합니다.

물론, 우리는 여전히 := 단축형을 사용할 수 있으므로 제어 가능한 카운터를 만들어보겠습니다:

```js
<let/count:=input.count />
<button onClick() { count += 1 }>
  ${count}    
</button>
```

<div class="content-ad"></div>

## 결론

Marko는 실제로 단방향 데이터 흐름인 것처럼 보이지만 무료 추상화인 두 방향 데이터 바인딩을 소개했습니다:

```js
<let/name="world" />
<Input value:=name />
```

```js
// Input.marko
<input value:=input.value>
```

<div class="content-ad"></div>

아래는 동일한 기능을 합니다:

```js
<let/name="world" />
<Input value=name valueChange(v) { name = v } />
```

```js
// Input.marko
<input
  value=input.value
  onInput(e) { input.valueChange(e.target.value) }
>
```

...

<div class="content-ad"></div>

- 데이터 플로우가 명확합니다.
- 암시적 루프를 도입할 방법이 없습니다.
- 성능이 우수합니다.
- 언제든지 수준별로 설탕을 포기할 수 있습니다.
- (보너스) 규칙이 조절 가능한 컴포넌트의 문을 엽니다.

모두가 이깁니다! 🎉

## 마르코

우리가 토론한 모든 것들은 현재 사전 릴리스 중인 Marko 6에서 제공되며, 매일 더욱 안정적으로 발전하고 있습니다.

<div class="content-ad"></div>

제가 기대하고 있어요!