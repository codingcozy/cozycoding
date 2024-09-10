---
title: "TypeScript의  꺾쇠 괄호와 제네릭 타입 사용하는 이유"
description: ""
coverImage: "/assets/img/2024-09-10-WhyYouShouldUseTypeScriptsAngleBracketsandGenericTypes_0.png"
date: 2024-09-10 18:12
ogImage: 
  url: /assets/img/2024-09-10-WhyYouShouldUseTypeScriptsAngleBracketsandGenericTypes_0.png
tag: Tech
originalTitle: "Why You Should Use TypeScripts  Angle Brackets and Generic Types"
link: "https://medium.com/totally-typescript/why-you-should-use-typescripts-angle-brackets-and-generic-types-b768abfb2d15"
isUpdated: false
---



![이미지](/assets/img/2024-09-10-WhyYouShouldUseTypeScriptsAngleBracketsandGenericTypes_0.png)

TypeScript의 \`<>\` 화살표 괄호는 "제네릭 타입" 개념을 이해했다고 생각했더라도 매일 제게 두려움을 안겨주곤 했어요.

이제 \`<>\` 화살표 괄호가 제 코드 안의 모든 유형 몬스터를 물리칠 수 있는 힘이 있다는 것을 이해했으니, 그것들이 꼭 필요하다고 느껴져요! 🥳

TypeScript의 \`<>\` 화살표 괄호는 제네릭 TypeScript 타입을 나타내며, 다른 유형을 감싸는 "타입 내부의 타입"을 생성할 수 있게 해줍니다. 이들은 어떤 데이터 유형을 사용해도 작동하도록 타입을 확장할 수 있게 해줍니다.


<div class="content-ad"></div>

제네릭은 사용하지 않을 때 어렵게 접근할 수 있는 유연성을 제공합니다. 특히 React Hooks와 React Query를 좋아하는 경우에 특히 그렇습니다.

본 문서에서는 TypeScript의 제네릭을 왜 좋아하는지 그리고 코드베이스에서 사용해야 하는 이유를 설명할 것입니다. 적어도 코드 안전성을 목표로 한다면 그렇게 해야 합니다.

실용적인 예제를 통해 함께 알아보겠습니다. 전문 개발자로서의 경험을 토대로 진행하겠습니다.

최종적으로 `T` 제네릭 유형을 사용하여 TypeScript 코드베이스의 유형 안전성, 유지 관리성, 가독성을 향상시키는 방법을 이해할 것입니다.

<div class="content-ad"></div>

바로 Beowulf의 괴물부터 시작해보겠습니다 — TypeScript에서의 any 타입들 — 이를 어떻게 `` 꺽쇠 괄호로 죽일지 배우기 전에요.

# TypeScript에서 any 타입 사용에 문제가 있어요

저는 TypeScript에서 any 타입을 사용하는 것이 일반적으로 많은 문제를 야기한다는 것을 발견했어요. JavaScript를 사용하는 게 더 좋았을지도 모른다고 생각해요.

결국 JavaScript는 100% any 타입을 사용할 때 TypeScript와 같으니까, any 타입이 타입 안전성을 위협하는 측면이 있다고 생각해요.

<div class="content-ad"></div>

코드 리뷰 중에 이런 것들을 잡는 게 정말 어려워요. GitHub의 Pull Request(PR) 리뷰 환경에는 실제 VS Code가 없기 때문에 여러분의 코드를 가져와서 확인할 때 타입 유추가 없어요.

부분적으로 TypeScript 코드베이스를 다루는 것은 매우 어렵지만, 타입 안전성을 잃게 될 경우 런타임 오류의 위협이 발생할 수 있어요.

"❌ JavaScript 프로젝트를 TypeScript로 이관하는 과정이 아닌 이상 'any'를 타입으로 사용하지 마세요. 컴파일러는 '이것에 대한 타입 검사를 끄라'고 간주하는 식으로 'any'를 다룹니다." — TypeScript 문서

반면에, 제네릭을 사용하면 알 수없는 타입 문제와 'any' 타입 문제를 해결할 수 있어요, 특히 백엔드 타입을 참조하는 프론트엔드 코드에서.

<div class="content-ad"></div>

당연히 유형 안정성을 유지하면서 유연성을 제공하는 것이 목표이므로 제네릭이 어떻게 작동하는지 소개하겠습니다.

# TypeScript에서 제네릭 소개

우선, TypeScript에 명시적인 유형을 너무 많이 추가하는 것을 피하려고 노력한다는 점을 강조하고 싶습니다. 저는 유형 추론을 선호합니다.

그럼에도 불구하고, 백엔드 API가 동일한 요구 사항을 갖춘 강력하게 유형화된 SQL 데이터베이스와 같은 것을 알면 제네릭 유형이 많이 필요합니다. 프론트엔드 코드에서는 알 수 없는 유형으로 끝나게 됩니다.

<div class="content-ad"></div>

그래서 이 기사 전체는 제네릭 또는 일반적인 유형을 사용하여 이러한 도전을 극복하는 방법에 대해 다룹니다. "확장 가능성은 시스템을 확장하고 확장을 구현하는 데 필요한 노력의 수준을 측정하는 것입니다." - 위키피디아

제네릭 유형은 일반적으로 유형(Type의 줄임말인 T로 일반적으로 지칭되는)을 사용하여 모든 데이터 유형과 작동할 수 있습니다. 그리고 2차 유형이 있는 경우 알파벳순으로 U라고 불립니다.

"소프트웨어 공학의 주요 부분 중 하나는 잘 정의되고 일관된 API를 갖는 컴포넌트를 구축하는 것뿐만 아니라 재사용 가능한 것입니다. 오늘날의 데이터뿐만 아니라 내일의 데이터에서도 작동할 수 있는 컴포넌트는 대형 소프트웨어 시스템을 구축하는 데 가장 유연한 기능을 제공할 것입니다."

<div class="content-ad"></div>

C# 및 Java와 같은 언어에서 재사용 가능한 구성 요소를 만들기 위한 주요 도구 중 하나는 제네릭입니다. 즉, 단일 유형이 아닌 여러 유형에서 작동할 수 있는 구성 요소를 만들 수 있는 기능입니다. 이를 통해 사용자는 이러한 구성 요소를 소비하고 자체 유형을 사용할 수 있습니다. — TypeScript 문서

제네릭을 사용하고 있는지 아닌지 알아보는 방법은 계속해서 나타나는 귀찮은 `<T>`, `<U>`와 같은 `각도 괄호`입니다.

다음은 제네릭 함수의 간단한 예시입니다:

```js
/**
 * 전달된 값을 반환하는 TypeScript 제네릭 함수.
 */
const identity = <T>(arg: T): T => arg
// 제네릭 화살표 함수에 이상한 `,` 쉼표는 필수입니다.

// 이는 `3`이 `number`이기 때문에 작동합니다:
console.log(identity<number>(3))

// 이는 대신 유형 오류를 발생시킵니다:
console.log(identity<string>(3))
```

<div class="content-ad"></div>

이 함수는 위에서 보여준 원시 숫자와 문자열 유형 뿐만 아니라 전달된 모든 데이터 유형과 함께 작동합니다.

제네릭은 클래스, 인터페이스 및 다른 복잡한 데이터 구조에서도 사용할 수 있지만, 함수 및 객체와 함께 가장 일반적으로 사용됩니다:

```js
/**
 * 전달된 값을 반환하는 TypeScript 제네릭 함수입니다.
 */
const identity = <T,>(arg: T): T => arg
// 제네릭 화살표 함수에는 이상한 `,` 콤마가 필요합니다.

// 이것은 `number` 유형이므로 작동합니다:
console.log(identity<number>(3))

// 대신에 유형 오류가 발생합니다:
console.log(identity<string>(3))


/**
 * `contents`가 유형인 `Box` 객체의 제네릭 유형.
 */
type Box<T> = {contents: T}

type Boxy = Box<Box<string>>

/** 이것은 올바르게 작동합니다. */
const boxy: Boxy = {contents: {contents: "Yeah!"}
/** 다른 유형 오류 예제가 여기 있습니다. */
const foxy: Boxy = {contents: {contents: 3}
```

그러면 Box`Box`string``와 같은 이 유형의 인스턴스를 만들 수 있습니다. 이는 내부 내용이 문자열 유형의 다른 상자인 상자를 포함하는 상자에 사용됩니다.

<div class="content-ad"></div>

(해당 예제들을 직접 확인하고, 에러 유형을 호버하여 볼 수 있는 TypeScript Playground에서 노는 걸로요.)

이런 장난감들로 유형 안전성과 유연성을 보장하지만, 항상 제네릭을 사용할 필요는 없기 때문에 오랫동안 무시해왔어요.

제네릭은 리액트 훅에서도 중요한 역할을 하며, useState와 같은 자주 사용되는 훅들 뿐만 아니라 사용자 정의 훅에 대한 유형 안전성을 제공합니다.

ReturnType, React Query, React Hook Form을 논의하기 전에 이에 대해 이야기해보죠; 아직 알려드려야 할 멋진 제네릭 'T' 유형들이 많이 남아 있거든요!

<div class="content-ad"></div>

# 제네릭 React 훅 TypeScript `T`를 사용한 예시

저는 Hoang Ha가 Medium에 작성한 제네릭 React useLocalStorage Hook 예시를 확장하여 이 React TSX 코드를 작성했습니다:

```js
import {useState, useEffect} from "react"

/** localStorage에서 타입 `<T>`의 저장된 데이터를 읽어옵니다. */
const getStorageValue = <T,>(key: string, defaultValue: T): T => {
  const saved = localStorage.getItem(key)
  if (!saved) return defaultValue // JSON.stringify()에서 오류가 나지 않도록 함.
  return JSON.parse(saved) ?? defaultValue // `defaultValue`로 대체
}

/** TypeScript 제네릭을 사용한 사용자 정의 React Hook. */
const useLocalStorage = <T,>(
  key: string,
  defaultValue: T,
): [T, (newValue: T) => void] => {
  const [value, setValue] = useState<T>(() =>
    getStorageValue(key, defaultValue),
  )

  useEffect(() => {
    /** 값이 변경될 때 localStorage에 값을 씁니다. */
    localStorage.setItem(key, JSON.stringify(value))
  }, [key, value])

  return [value, setValue]
}

/** `<Task>` 컴포넌트를 위한 사용자 정의 `Task`를 정의합니다. */
export type Task = {
  id: number
  title: string
  completed: boolean
}

export const LOCAL_STORAGE_KEY = "Tasks를 위한 localStorage 키"

/** `<Task>` 컴포넌트는 사용자 정의 제네릭 컴포넌트를 사용합니다. */
export default function Task({id, title, completed}: Task) {
  /** React의 `useState`를 `Task[]` 타입의 `<T>`로 사용합니다. */
  const [value, setValue] = useLocalStorage<Task[]>(LOCAL_STORAGE_KEY, [])
  const [tasks, setTasks] = useState<Task[]>(value)

  useEffect(() => {
    setValue(tasks)
  }, [tasks])

  const addTask = () =>
    setTasks((currentTasks) => {
      const id = (currentTasks?.length || 0) + 1
      return [{id, title: `Task ${id}`, completed: false}, ...currentTasks]
    })

  return (
    <button
      style={{width: "100%", height: "100%", backgroundColor: "skyblue"}}
      onClick={() => addTask()}
    >
      어디를 클릭해도 작업을 추가할 수 있습니다.
      <br />
      <br />
      <pre>{JSON.stringify(tasks, null, 2)}</pre>
    </button>
  )
}
```

또한, 해당 CodeSandbox DevBox를 열어서 실제로 작업을 localStorage에 저장하는지 확인할 수 있습니다.

<div class="content-ad"></div>

(몇 번 클릭해서 일부 새 작업을 추가해보면 확인할 수 있어요. 페이지 전체를 새로 고침하면 여전히 그대로 남아 있는 것을 볼 수 있어요.)

이제 TypeScript에서 ReturnType을 사용하여 typeof가 필요한 이유에 대해 알아보겠습니다.

# ReturnType에 대해 이야기해봅시다: `typeof someFunction`

최근 TypeScript에 대해 배운 가장 유용한 기능 중 하나는 ReturnType 유틸리티 타입을 통해 이미 좋아하는 타입 추론을 활용할 수 있다는 것이었어요. 명시적인 타입의 불필요하고 반복적인 중복을 피할 수 있다는 점이 너무 유용하다고 생각해요.

<div class="content-ad"></div>

ReturnType은 함수의 반환 유형을 직접 추론할 수 있게 해줍니다. 이는 "이 함수가 반환하는 유형"에 대해 명시적으로 작성하지 않고도 필요한 모든 경우에 굉장히 유용할 수 있습니다.

예를 들어, 함수가 복잡한 객체를 반환하고, 코드베이스 다른 곳에서 해당 유형을 참조하고 싶을 때가 있습니다. 다음은 예시입니다:

```js
/**
 * 주어진 함수 유형의 반환 유형을 추출합니다. `typeof`에 주목하세요.
 */
type ReturnTypeOfSomeFunction = ReturnType<typeof someFunction>
```

이 유형은 실제로 어떤 유형인지 알 필요 없이 someFunction의 반환 유형을 조회할 것입니다.

<div class="content-ad"></div>

이렇게 하면 여러 곳에서 명시적으로 타입을 입력하는 것보다 빠릅니다. 나중에 리팩토링하기도 훨씬 쉬워요. 

위의 typeof 키워드는 someFunction의 타입을 가리키는 것이죠. 값이 아닙니다. 왜냐하면 someFunction은 typeof someFunction 타입을 갖는 값이기 때문이죠.

“자바스크립트에는 표현식 컨텍스트에서 사용할 수 있는 typeof 연산자가 이미 있습니다. ... TypeScript는 변수나 프로퍼티의 타입을 가리키기 위해 타입 컨텍스트에서 사용할 수 있는 typeof 연산자를 추가했습니다.” — TypeScript 문서

이것은 API나 다른 외부 데이터 소스와 작업할 때 특히 유용할 수 있습니다. 반환 타입이 곧바로 명확하지 않은 경우가 있으니까요.

<div class="content-ad"></div>

당연히 백엔드 코드(그리고 따라서 함수)가 TypeScript 모노레포를 통해 프론트엔드와 공유되지 않는다면, 그 테이블 태그를 변경하는 것은 작동하지 않을 거에요.

그러나 코드를 공유하는 경우, 단순히 "백엔드 함수가 반환하는 것"을 참조하는 것만으로도 타이핑을 많이 줄일 수 있어요.

가끔 백엔드의 반환 타입이 명시적으로 제공되어도, 저는 항상 ReturnType`typeof whateverFunction`을 사용합니다.

TypeScript를 리팩토링하는 것이 JavaScript를 리팩토링하는 것보다 이미 빠르다는 것은 사실이지만, 내장된 타입 추론에 의존해서 TypeScript를 단순히 올바르게 작동하도록 하는 것은 더 쉽습니다. 오예!

<div class="content-ad"></div>

다음으로, TypeScript의 ``꺾쇠 괄호가 React Hook의 또 다른 일반적인 세트를 처리하는 데 어떻게 도와주는지 살펴보겠습니다, 이번엔 React Query npm 라이브러리입니다.

# React Query와 TypeScript 제네릭 타입 사용하기

제네릭은 React Query와 같은 TypeScript 기반의 npm 패키지에서 널리 사용되며, 이미 알고 계시는 것처럼 React Query는 React 앱에서 서버 데이터를 가져오고 캐싱 및 동기화하는 Hooks를 제공합니다.

사용자 목록을 가져오는 React Query 훅을 고려해보세요:

<div class="content-ad"></div>

```js
/**
 * React Query를 사용하여 `User[]` 타입의 사용자 데이터를 가져옵니다.
 */
const { data } = useQuery<User[]>("users", fetchUsers)
```

여기서 useQuery Hook은 User[]으로 타입이 지정되어 있어서 React Query가 반환하는 데이터가 User 객체의 배열임을 보장합니다.

ReturnType`typeof fetchUsers`로 User[] 타입을 대체할 수 있음을 즉시 알 수 있습니다. 때로는 `U` 제네릭 타입을 완전히 생략할 수도 있습니다.

useState도 마찬가지입니다. TypeScript의 타입 추론 시스템은 주로 정확한 타입을 추측할 수 있습니다. 예를 들어, useState(3)와 같은 경우입니다.


<div class="content-ad"></div>

그러나 배열 및 기타 복잡한 객체 유형으로 들어가면, VS Code Intellisense를 두 번 확인하여 유형이 올바르게 설정되었는지 확인해야 합니다. `U` 유형이 필요한지 여부에 상관없이 정확한 데이터 유형을 전달받는지 확인하기 위해 React Query와 앵귤러 괄호를 사용하는 아이디어 전체는 초기에 오류를 발견하고 프론트엔드 리액트 컴포넌트가 백엔드 서버의 API 응답에서 올바른 데이터 유형을 수신하도록 보장하는 것입니다.

다른 예시를 살펴보겠습니다. 실제 세계 코드와 좀 더 유사하여 TypeScript 제네릭을 사용하여 React Query 변이를 좁히는 경우입니다:
```js
import {useQuery, useMutation, useQueryClient} from "react-query"

/** 응용프로그램에서 사용자를 표현합니다. */
type User = {
  id: number
  name: string
  email: string
}

/** API에서 사용자 데이터를 가져옵니다. */
const fetchUser = async (userId: number): Promise<User> => {
  const response = await fetch(`/api/users/${userId}`)
  if (!response.ok) {
    throw new Error("네트워크 응답이 OK가 아닙니다")
  }
  return response.json()
}

/** 기존 사용자의 정보를 업데이트합니다. */
const updateUser = async (user: User): Promise<User> => {
  const response = await fetch(`/api/users/${user.id}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(user),
  })
  if (!response.ok) {
    throw new Error("네트워크 응답이 OK가 아닙니다")
  }
  return response.json()
}

/** React Query를 사용하여 사용자 데이터를 가져오고 업데이트합니다. */
const UserComponent = ({userId}: {userId: number}) => {
  const queryClient = useQueryClient()

  const {data, error, isLoading} = useQuery<User>(["user", userId], () =>
    fetchUser(userId),
  )

  const mutation = useMutation<User, Error, User>(updateUser, {
    onSuccess: () => {
      queryClient.invalidateQueries(["user", userId])
    },
  })

  const handleUpdateUser = () => {
    if (data) {
      const updatedUser = {...data, name: "업데이트된 이름"}
      mutation.mutate(updatedUser)
    }
  }

  if (isLoading) return <span>로딩 중...</span>
  if (error) return <span>오류 발생: {error.message}</span>

  return (
    <div>
      <h1>{data?.name}</h1>
      <button onClick={handleUpdateUser}>사용자 업데이트</button>
      {mutation.isLoading && <span>사용자 업데이트 중...</span>}
      {mutation.isError && <span>오류 발생: {mutation.error.message}</span>}
      {mutation.isSuccess && <span>사용자가 성공적으로 업데이트되었습니다!</span>}
    </div>
  )
}
```

<div class="content-ad"></div>

이 예제에서는 API에서 사용자 데이터를 가져오기 위한 fetchUser 함수의 구현을 볼 수 있습니다. 해당 코드는 이전에 보았던 useQuery 예제입니다.

updateUser 함수는 PUT 요청을 통해 사용자 정보를 업데이트하며, 이는 컴포넌트 내에서 useMutation으로 감싸져 있습니다.

또한 React Query 특정 캐싱 코드(invalidateQueries)가 있어 사용자 데이터가 최신 변경 사항을 반영하기 위해 다시 가져오도록 보장합니다.

마지막으로, 많은 종류의 일반적인 유형을 지정할 수 있음을 강조하고 싶습니다. 지정하지 않은 유형은 가능한 경우 자동으로 유추됩니다.

<div class="content-ad"></div>

위 코드 예제에서 `useMutation<User, Error, User>`가 나오는데, 이는 React Query의 GitHub 소스 코드 일부에 해당합니다. 마우스를 가져가면 VS Code Intellisense 힌트로 볼 수도 있습니다:

```js
export function useMutation<
  TData = unknown,
  TError = DefaultError,
  TVariables = void,
  TContext = unknown,
>(
  options: UseMutationOptions<TData, TError, TVariables, TContext>,
  queryClient?: QueryClient,
): UseMutationResult<TData, TError, TVariables, TContext> {
```

안타깝게도 React Query의 TypeScript 문서는 특히 좋지 않습니다. 따라서 사용 방법을 이해하려면 코드를 살펴봐야 할 수도 있습니다.

제가 말하고 싶은 것은 `` 꺾쇠 괄호를 사용하여 React Query에서 사용할 유형을 변경할 수 있다는 것입니다. 예를 들어, 위의 TError 유형은 처음에는 unknown으로 시작할 수 있지만, 아마도 백엔드에서 반환되는 것에 대해 더 잘 알고 있을 수도 있습니다.

<div class="content-ad"></div>

자, TypeScript 제네릭을 사랑하게 된 또 다른 실제 예제로 넘어가봅시다. React Hook Form과 함께 사용하는 방법이에요.

# React Hook Form과 제네릭 타입 사용하기

React 앱에서 폼 상태를 관리하는 React Hook Form이라는 npm 패키지도 사용하는 편인데, 이 패키지도 제네릭을 널리 활용해요.

React Hook Form과 제네릭을 함께 사용하는 것은 선택 사항이지만, ``다이아몬드 기호 "제네릭 타입" 패턴이 React Query와도 완벽하게 어울리죠.

<div class="content-ad"></div>

예를 들어, 사용자 데이터를 수집하는 양식의 일부로 앱에서 useForm 호출을 고려해보세요:

```js
import { useForm } from "react-hook-form"

/**
 * 사용자 등록을 위한 양식 데이터를 나타냅니다.
 */
type FormData = {
  name: string
  email: string
  password: string
}

/**
 * React Hook Form을 사용하여 FormData 타입으로 양식 상태를 관리합니다.
 */
const UserRegistrationForm = () => {
  const { register, handleSubmit } = useForm<FormData>()

  const onSubmit = (data: FormData) => {
    console.log(data)
  }

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("name")} placeholder="이름" />
      <input {...register("email")} type="email" placeholder="이메일" />
      <input {...register("password")} type="password" placeholder="비밀번호" />
      <button type="submit">등록</button>
    </form>
  )
}
```

여기서 useForm 훅은 `FormData`로 타입이 지정되어 있어, 양식 상태가 FormData 타입으로 올바르게 지정됩니다.

당연히, 프론트엔드 앱의 사용자 인터페이스 디자인과 일치하는 타입을 지정하면 양식 유효성 검사 로직이 타입 안전하게 처리됩니다.

<div class="content-ad"></div>

`useForm` 함수에 defaultValues 옵션을 전달하여 초기값을 지정하면 `<table>` 태그를 Markdown 형식으로 변경할 수 있습니다.

```js
const { register, handleSubmit } = useForm({
  defaultValues: {
    name: "",
    email: "",
    password: ""
  }
})
```

React Hook Form을 올바르게 작동하려면 defaultValues를 지정해야 하지만, 다른 코드에서도 해당 폼의 유형(FormData)이 필요하기 때문에 두 가지를 종종 동시에 사용하게 됩니다.

```js
const { register, handleSubmit } = useForm<FormData>({
  defaultValues: {
    name: "",
    email: "",
    password: ""
  }
})
```

<div class="content-ad"></div>

그리고 — 추론을 좋아하는 경우에는 — 생성한 defaultValues 객체를 기반으로 FormData를 만드는 방법도 있습니다; 다음과 같이 보일 수 있습니다:

```js
const defaultValues = {
  name: "",
  email: "",
  password: ""
}

type FormData = typeof defaultValues

const { register, handleSubmit } = useForm<FormData>({
  defaultValues
})
```

React / TypeScript 개발자들의 "꿈"은 "알 수 없는" (문서화되지 않은 백엔드 API)과 상호 작용할 때에도 유형 안전성을 갖는 것인데, 저는 React Query 및 React Hook Form을 결합하여 그것을 달성합니다.

# 제네릭 유형의 다른 실제 응용 프로그램

<div class="content-ad"></div>

일반적인 유형은 간단한 유틸리티 함수부터 복잡한 데이터 구조까지 다양한 실제 응용 프로그램에서 사용될 수 있습니다.

시간이 지남에 따라 점점 읽기 어려워지지만, TypeScript의 일반화는 XState과 같은 npm 패키지에서 복잡한 "추론" 능력을 제공하는 원동력입니다.

배열 정렬과 관련된 예제를 살펴보겠습니다. 각 객체의 값에 따라 배열을 정렬하는 함수를 고려해보세요.

```js
/**
 * 객체 배열을 해당 obj[`key`] 값으로 정렬합니다.
 */
const sortArray = <T,>(arr: T[], key: keyof T): T[] =>
  arr.sort((a, b) => (a[key] > b[key] ? 1 : -1))

// [{a:"3"},{a:"4"},{a:"5"}]를 출력합니다.
console.log(sortArray([{a:"5"},{a:"4"},{a:"3"}],"a"))
```

<div class="content-ad"></div>

여기서는 정말 "당신이 여기서 왜 하는 건지" 영역으로 들어가지만, 아이디어는 어떤 방식으로든 `T`를 실제로 재사용할 수 있다는 것입니다.

제네릭의 또 다른 일반적인 사용 사례는 API 응답에서 발생하며, API가 반환하는 데이터가 올바르게 유형화되었음을 보장하고 싶을 때 사용됩니다.

```js
/**
 * 데이터 유형이 T인 API 응답의 제네릭 유형.
 */
type ApiResponse<T> = { data: T, error: string }
```

이것은 앞서 본 React Query의 패턴과 비슷한데, React Query는 'data, error' 부분을 당신의 `T` 제네릭 유형으로 감싸줍니다.

<div class="content-ad"></div>

요기에는 Axios가 TypeScript에서 어떻게 처리되는지 볼게요. Geshan Manandhar의 블로그 글에서 Axios와 TypeScript에 대해 찾은 예시를 기반으로 한 것이에요:

```js
import axios, {
  AxiosResponse,
  AxiosRequestConfig,
  RawAxiosRequestHeaders,
} from "axios"

const client = axios.create({
  baseURL: "https://api.github.com",
})

type GitHubFoundUser = {
  login: string
  id: number
}

type GitHubUser = {
  login: string
  id: number
  followers: number
}

const config: AxiosRequestConfig = {
  headers: {
    Accept: "application/vnd.github+json",
  } as RawAxiosRequestHeaders,
}

const q = `q=${encodeURIComponent("followers:>=100000")}&sort=followers&order=desc`

try {
  const searchResponse: AxiosResponse<GithubFoundUser[]> = await client.get(
    `/search/users?${q}`,
    config,
  )
  /** 이곳에서 foundUsers는 GithubFoundUser[] 타입일 것입니다 */
  const foundUsers = searchResponse.data?.items || [] as GithubFoundUser[]
  const username = foundUsers?.[0]?.login || "ERROR"

  console.log(
    `가장 인기 있는 GitHub 사용자는 "${username}"입니다.`,
  )
} catch (error) {
  console.log(error)
}
```

제 실수로 AxiosResponse TypeScript 코드를 직접 확인해야만 일반화하는 방법을 이해할 수 있었어요. 여기서 `AxiosResponse<GitHubFoundUser[]>`를 사용했죠.

```js
export interface AxiosResponse<T = never>  {
  data: T;
  status: number;
  statusText: string;
  headers: Record<string, string>;
  config: AxiosRequestConfig<T>;
  request?: any;
}
```

<div class="content-ad"></div>

클라이언트.get 대신에 ``을 사용해서 추가할 수도 있었는데, 이 방법이 권장된 접근 방식인 것 같아요: client.get`GithubFoundUser[]`.

다른 예시를 보겠어요. 2018년 GitHub 게시물에서 발견한 Stephan S의 조언이에요. 정확히 그런 말을 하고 있어요:

```js
const response = axios.request<ServerData>({
  url: "https://example.com/path/to/data",
  transformResponse: (r: ServerResponse) => r.data
}).then((response) => {
  // `response`은 `AxiosResponse<ServerData>` 유형입니다
  const { data } = response
  // `data`는 ServerData 유형이고 정확히 유추됩니다
})
```

제네릭은 Redux와 같은 상태 관리 라이브러리에서도 사용할 수 있어요. 이렇게 하면 액션과 리듀서의 유형을 올바르게 지정할 수 있어요:

<div class="content-ad"></div>

```js
/**
 * Redux action의 일반적인 유형으로, payload가 T 유형입니다.
 */
유형 Action<T> = { type: string, payload: T }
```

이 유형은 Redux 액션 및 리듀서가 유형 안전성을 보장하는 데 사용할 수 있습니다. 그러나 TypeScript에서 Redux Toolkit (RTK)를 사용하지 않고 Redux를 건드리지 않을 것을 권합니다. RTK가 대부분의 작업을 처리해줄 것입니다.

제네릭은 UI 구성 요소에서도 사용할 수 있습니다. 불필요한 복잡성을 추가하지 않도록 주의하세요:

```js
/**
 * 일반적인 모달 구성 요소를 위한 속성.
 */
유형 ModalProps<T> = { data: T, isOpen: boolean, onClose: () => void }
```

<div class="content-ad"></div>

저는 개인적으로 제가 일반적인 `T` 사용자 정의 React Hooks를 만드는 빈도가 일반적인 `T` React 컴포넌트보다 훨씬 많다고 생각해요.

# 결론: 왜 나는 TypeScript에서 각도 괄호를 사랑하는가

TypeScript에서의 제네릭 타입은 바로 그것이 귀찮은 `` 각도 괄호에 대해 이야기하는 것입니다. 항상 필요한 것은 아니지만, 저처럼 코드 재사용성과 유지보수성을 강조할 때 유용합니다.

간단하고 이해하기 쉬운 제네릭 타입을 사용하는 것의 목표는 높은 코드 품질, 더 적은 버그, 그리고 더 나은 개발 경험을 제공하는 것입니다.

<div class="content-ad"></div>

이 ``각도 꺽쇠 유형들을 완벽하게 이해하고 숙달함으로써, 특히 React Query 및 React Hook Form과 같은 다른 라이브러리와 함께 사용할 때, 당신은 세계적 수준의 TypeScript 개발자가 될 수 있을 거에요.

단지 `T`와 `U`를 너무 과하게 사용하지는 마세요!

코드를 명확하게 만들고 특정 방식으로 작업하는 이유에 대해 문서화하며, 자동으로 추론될 수 있는 추가적인 유형들 (:React.FC`PropTypes`와 같은)을 가능한 한 모두 제거하려고 노력해보세요.

아, `T` 제네릭 유형과 관련된 마지막 작은 조언이었나요?

<div class="content-ad"></div>

당신에게는 분명 잘하셨어요!

코딩 즐기세요! ⌨️

# 더 읽을 거리: 나아가세요, 젊은 "꺽쇠 괄호들"

한 번 TypeScript의 제네릭 `T` 타입을 마스터했다면, 지도된 매핑 및 조건부 타입과 같은 복잡한 주제에 대해 공부할 준비가 되었습니다.

<div class="content-ad"></div>

저는 이 주제를 연구하면서 온라인에서 발견한 4가지 훌륭한 기사들이에요. 계속 배우고 마스터하기를 도와줄 거라고 희망합니다:

정말 좋은 자료들이죠!

당신은 이  구문을 계속 사용하면서 눈이 출혈할 것 같아요. 다른 분들은 당신이 C++, Java 또는 다른 일반 변수형 언어를 코딩하고 있다고 생각할 거에요. 즐겁게 학습하세요!!!