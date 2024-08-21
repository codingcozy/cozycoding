---
title: "서버 액션 및 뮤테이션 사용법 NextJS 14 App Router와 함께"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:17
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Server Actions and Mutations"
link: "https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations"
isUpdated: true
---




# 서버 작업 및 변이

서버 작업은 서버에서 실행되는 비동기 함수들입니다. Next.js 애플리케이션에서 양식 제출 및 데이터 변이를 처리하기 위해 Server 및 Client Components에서 사용할 수 있습니다.

> 🎥 시청: 서버 작업을 사용한 양식 및 변이에 대해 더 알아보세요 → YouTube (10분)
.

## 형식

<div class="content-ad"></div>

서버 액션은 React의 "use server" 지시어로 정의할 수 있습니다. 해당 지시어를 비동기 함수의 맨 위에 배치하여 함수를 서버 액션으로 표시하거나, 별도의 파일의 맨 위에 배치하여 해당 파일의 모든 내보내기를 서버 액션으로 표시할 수 있습니다.

### 서버 컴포넌트

서버 컴포넌트는 인라인 함수 레벨 또는 모듈 레벨의 "use server" 지시어를 사용할 수 있습니다. 서버 액션을 인라인으로 사용하려면 함수 본문 맨 위에 "use server"를 추가하십시오:

```js
// 서버 컴포넌트
export default function Page() {
  // 서버 액션
  async function create() {
    'use server'
 
    // ...
  }
 
  return (
    // ...
  )
}
```

<div class="content-ad"></div>

### 클라이언트 구성 요소

클라이언트 구성 요소는 모듈 레벨 "서버 사용" 지시문을 사용하는 작업만 가져올 수 있습니다.

클라이언트 구성 요소에서 서버 작업을 호출하려면 새 파일을 만들고 상단에 "use server" 지시문을 추가하십시오. 파일 내의 모든 함수는 클라이언트 및 서버 구성 요소에서 재사용할 수있는 서버 작업으로 표시됩니다:

```js
'use server'

export async function create() {
  // ...
}
```

<div class="content-ad"></div>

```js
import { create } from '@/app/actions'
 
export function Button() {
  return (
    // ...
  )
}
```

서버 액션을 클라이언트 컴포넌트로 prop으로 전달할 수도 있어요:

```js
<ClientComponent updateItem={updateItem} />
```

```js
'use client'
 
export default function ClientComponent({ updateItem }) {
  return <form action={updateItem}>{/* ... */}</form>
}
```

<div class="content-ad"></div>

## 행동

- 서버 액션은 `form` 요소의 action 속성을 사용하여 호출할 수 있습니다.
서버 구성 요소는 기본적으로 점진적 향상을 지원합니다. 즉, JavaScript가 아직로드되지 않았거나 비활성화되었더라도 폼이 제출됩니다.
클라이언트 구성 요소에서는 JavaScript가 아직 로드되지 않은 경우 서버 액션을 호출하는 폼이 제출 대기열에 추가되며, 클라이언트 하이드레이션을 우선시합니다.
하이드레이션 이후 브라우저는 폼 제출 시 새로 고침되지 않습니다.
- 서버 구성 요소는 기본적으로 점진적 향상을 지원합니다. 즉, JavaScript가 아직로드되지 않았거나 비활성화되었더라도 폼이 제출됩니다.
- 클라이언트 구성 요소에서는 JavaScript가 아직 로드되지 않은 경우 서버 액션을 호출하는 폼이 제출 대기열에 추가되며, 클라이언트 하이드레이션을 우선시합니다.
- 하이드레이션 이후 브라우저는 폼 제출 시 새로 고침되지 않습니다.
- 서버 액션은 `form` 이외의 요소에서도 이벤트 핸들러, useEffect, 타사 라이브러리 및 `button`과 같은 다른 폼 요소에서 호출할 수 있습니다.
- 서버 액션은 Next.js의 캐싱 및 재유효화 아키텍처와 통합됩니다. 액션이 호출되면 Next.js는 단일 서버 라운드트립에서 업데이트된 UI 및 새 데이터를 모두 반환할 수 있습니다.
- 내부적으로 액션은 POST 메소드를 사용하며, 이 HTTP 메소드만이 호출할 수 있습니다.
- 서버 액션의 매개변수와 반환 값은 React에서 직렬화 가능해야 합니다. 직렬화 가능한 매개변수와 값 목록은 React 문서를 참조하십시오.
- 서버 액션은 함수입니다. 즉, 응용 프로그램의 어디에서든 재사용할 수 있습니다.
- 서버 액션은 사용 중인 페이지나 레이아웃에서 실행 시간을 상속합니다.
- 서버 액션은 사용 중인 페이지나 레이아웃에서 가져온 라우트 세그먼트 구성을 상속합니다. 최대 지속 시간과 같은 필드를 포함합니다.

## 예시

### 폼

<div class="content-ad"></div>

React는 HTML `form` 요소를 확장하여 action 속성을 사용하여 Server Actions를 호출할 수 있습니다.

양식에서 호출될 때, action은 자동으로 FormData 객체를 받습니다. React의 useState를 사용하여 필드를 관리할 필요가 없으며 대신에 네이티브 FormData 메서드를 사용하여 데이터를 추출할 수 있습니다:

```js
export default function Page() {
  async function createInvoice(formData: FormData) {
    '서버 사용'

    const rawFormData = {
      customerId: formData.get('customerId'),
      amount: formData.get('amount'),
      status: formData.get('status'),
    }

    // 데이터 변경
    // 캐시 재유효화
  }

  return <form action={createInvoice}>...</form>
}
```

> 알아두면 좋은 정보:
예: 로딩 및 오류 상태가 있는 양식
많은 필드가 있는 양식을 다룰 때, JavaScript의 Object.fromEntries() 메서드를 사용하는 것을 고려해 볼 수 있습니다. 예를 들어 const rawFormData = Object.fromEntries(formData). 주의할 점은 formData에 추가적인 $ACTION_ 속성이 포함될 것입니다.
더 자세한 내용은 React `form` 문서를 참조하세요.

<div class="content-ad"></div>

#### 추가 인수 전달

JavaScript의 bind 메서드를 사용하여 Server Action에 추가 인수를 전달할 수 있습니다.

```js
'use client'

import { updateUser } from './actions'

export function UserProfile({ userId }: { userId: string }) {

  return (
    <form action={updateUserWithId}>
      <input type="text" name="name" />
      <button type="submit">Update User Name</button>
    </form>
  )
}
```

서버 액션이 폼 데이터에 추가하여 userId 인수를 받게 될 것입니다:

<div class="content-ad"></div>

```js
'use server'

export async function updateUser(userId, formData) {
  // ...
}
```

> 좋은 정보:
다른 방법은 폼의 숨겨진 입력 필드로 인수를 전달하는 것입니다 (예: `input type="hidden" name="userId" value='userId' /`). 그러나 값은 렌더링된 HTML의 일부가 되며 인코딩되지 않습니다.
.bind는 서버 및 클라이언트 컴포넌트에서 모두 작동합니다. 또한 점진적인 향상을 지원합니다.

#### 보류 중인 상태

React useFormStatus 훅을 사용하여 폼이 제출 중일 때 보류 중인 상태를 표시할 수 있습니다.


<div class="content-ad"></div>

- useFormStatus는 특정 `form`의 상태를 반환하므로 `form` 요소의 자식으로 정의되어야 합니다.
- useFormStatus는 React 훅이므로 Client Component에서 사용되어야 합니다.

```js
'use client'

import { useFormStatus } from 'react-dom'

export function SubmitButton() {
  const { pending } = useFormStatus()

  return (
    <button type="submit" disabled={pending}>
      Add
    </button>
  )
}
```

`SubmitButton`은 이후에 어떤 형식에도 중첩될 수 있습니다:

```js
import { SubmitButton } from '@/app/submit-button'
import { createItem } from '@/app/actions'

// Server Component
export default async function Home() {
  return (
    <form action={createItem}>
      <input type="text" name="field-name" />
      <SubmitButton />
    </form>
  )
}
```

<div class="content-ad"></div>

#### 서버 측 유효성 검사 및 오류 처리

기본 클라이언트 측 양식 유효성 검사로 required 및 type="email"과 같은 HTML 유효성 검사를 사용하는 것을 권장합니다.

더 고급적인 서버 측 유효성 검사를 위해, 데이터를 변이하기 전에 양식 필드를 검증하는 zod와 같은 라이브러리를 사용할 수 있습니다:

```js
'use server'
 
import { z } from 'zod'
 
const schema = z.object({
  email: z.string({
    invalid_type_error: 'Invalid Email',
  }),
})
 
export default async function createUser(formData: FormData) {
  const validatedFields = schema.safeParse({
    email: formData.get('email'),
  })
 
  // 양식 데이터가 유효하지 않은 경우 빨리 반환합니다.
  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
    }
  }
 
  // 데이터 변이
}
```

<div class="content-ad"></div>

서버에서 필드가 검증된 후에는 작업에서 직렬화 가능한 개체를 반환하고 React의 useFormState 훅을 사용하여 사용자에게 메시지를 표시할 수 있습니다.

- useFormState에 작업을 전달하면 작업의 함수 시그니처가 변경되어 첫 번째 인수로 새로운 prevState 또는 initialState 매개변수를 받습니다.
- useFormState은 React 훅이므로 클라이언트 컴포넌트에서 사용해야 합니다.

```js
'use server'

export async function createUser(prevState: any, formData: FormData) {
  // ...
  return {
    message: '유효한 이메일을 입력하세요',
  }
}
```

그런 다음 작업을 useFormState 훅에 전달하고 반환된 상태를 사용하여 오류 메시지를 표시할 수 있습니다.

<div class="content-ad"></div>

```javascript
'클라이언트 사용'
 
import { useFormState } from 'react-dom'
import { createUser } from '@/app/actions'
 
const initialState = {
  message: '',
}
 
export function Signup() {
  const [state, formAction] = useFormState(createUser, initialState)
 
  return (
    <form action={formAction}>
      <label htmlFor="email">이메일</label>
      <input type="text" id="email" name="email" required />
      {/* ... */}
      <p aria-live="polite" className="sr-only">
        {state?.message}
      </p>
      <button>가입</button>
    </form>
  )
}
```

> 알아두면 좋아요:
데이터를 변경하기 전에 항상 사용자가 해당 작업을 수행할 권한이 있는지 확인해야 합니다. 인증과 권한 부여를 참조해주세요.

#### 낙관적 업데이트

응담 서버 액션이 완료되기를 기다리지 않고 UI를 낙관적으로 업데이트하려면 React의 `useOptimistic` 훅을 사용할 수 있습니다.

<div class="content-ad"></div>

```js
'use client'
 
import { useOptimistic } from 'react'
import { send } from './actions'
 
type Message = {
  message: string
}
 
export function Thread({ messages }: { messages: Message[] }) {
  const [optimisticMessages, addOptimisticMessage] = useOptimistic<
    Message[],
    string
  >(messages, (state, newMessage) => [...state, { message: newMessage }])
 
  return (
    <div>
      {optimisticMessages.map((m, k) => (
        <div key={k}>{m.message}</div>
      ))}
      <form
        action={async (formData: FormData) => {
          const message = formData.get('message')
          addOptimisticMessage(message)
          await send(message)
        }
      >
        <input type="text" name="message" />
        <button type="submit">Send</button>
      </form>
    </div>
  )
}
```

#### 중첩된 요소

`form` 내에 중첩된 요소들인 `button`, `input type="submit"`, `input type="image"` 등에서 서버 액션을 호출할 수 있습니다. 이러한 요소들은 formAction prop 또는 이벤트 핸들러를 허용합니다.

여러 서버 액션을 한 번에 호출해야 하는 경우에 유용합니다. 예를 들어, 글 초안을 저장하는 동시에 게시하는 특정 `button` 요소를 생성할 수 있습니다. 더 자세한 정보는 React `form` 문서를 참조하세요.

<div class="content-ad"></div>

#### 프로그래밍 방식으로 양식 제출

requestSubmit() 메서드를 사용하여 양식 제출을 트리거할 수 있습니다. 예를 들어 사용자가 ⌘ + Enter를 누르면 onKeyDown 이벤트를 수신할 수 있습니다.

```js
'use client'

export function Entry() {
  const handleKeyDown = (e: React.KeyboardEvent<HTMLTextAreaElement>) => {
    if (
      (e.ctrlKey || e.metaKey) &&
      (e.key === 'Enter' || e.key === 'NumpadEnter')
    ) {
      e.preventDefault()
      e.currentTarget.form?.requestSubmit()
    }
  }

  return (
    <div>
      <textarea name="entry" rows={20} required onKeyDown={handleKeyDown} />
    </div>
  )
}
```

이는 작성한 양식의 가장 가까운 `form` 조상을 제출하며, 서버 Action을 호출합니다.

<div class="content-ad"></div>

### 비-형식 요소

보통 `form` 요소 내에서 Server Actions를 사용하는 것이 일반적이지만, 이벤트 핸들러나 useEffect와 같은 코드의 다른 부분에서도 호출할 수 있습니다.

#### 이벤트 핸들러

`onClick`과 같은 이벤트 핸들러에서 Server Action을 호출할 수 있습니다. 예를 들어, 좋아요 수를 증가시키려면:

<div class="content-ad"></div>

```js
'사용자용'
 
import { incrementLike } from './actions'
import { useState } from 'react'
 
export default function LikeButton({ initialLikes }: { initialLikes: number }) {
  const [likes, setLikes] = useState(initialLikes)
 
  return (
    <>
      <p>전체 좋아요: {likes}</p>
      <button
        onClick={async () => {
          const updatedLikes = await incrementLike()
          setLikes(updatedLikes)
        }
      >
        좋아요
      </button>
    </>
  )
}
```

사용자 경험을 개선하기 위해, React의 다른 API인 useOptimistic나 useTransition을 활용하여
서버에서의 실행이 완료되기 전에 UI를 업데이트하거나 대기 상태를 보여주는 것을 추천합니다.

또한, form 요소에 이벤트 핸들러를 추가할 수 있습니다. 예를들어, 폼 필드의 변경 사항을 저장할 때 onChange 이벤트 핸들러를 사용할 수 있습니다:

```js
'사용자용'
 
import { publishPost, saveDraft } from './actions'
 
export default function EditPost() {
  return (
    <form action={publishPost}>
      <textarea
        name="content"
        onChange={async (e) => {
          await saveDraft(e.target.value)
        }
      />
      <button type="submit">게시</button>
    </form>
  )
}
```

<div class="content-ad"></div>

이와 같은 경우, 여러 이벤트가 빠르게 연속으로 발생할 수 있는 경우에는 불필요한 서버 액션 호출을 방지하기 위해 디바운싱을 권장합니다.

#### useEffect

React의 `useEffect` 훅을 사용하여 컴포넌트가 마운트되거나 의존성이 변경될 때 서버 액션을 호출할 수 있습니다. 이는 전역 이벤트에 의존하거나 자동으로 트리거되어야 하는 변이에 유용합니다. 예를 들어, 앱 단축키를 위한 `onKeyDown`, 무한 스크롤을 위한 교차 옵저버 훅, 혹은 뷰 카운트를 업데이트하기 위해 컴포넌트가 마운트될 때:

```js
'use client'
 
import { incrementViews } from './actions'
import { useState, useEffect } from 'react'
 
export default function ViewCount({ initialViews }: { initialViews: number }) {
  const [views, setViews] = useState(initialViews)
 
  useEffect(() => {
    const updateViews = async () => {
      const updatedViews = await incrementViews()
      setViews(updatedViews)
    }
 
    updateViews()
  }, [])
 
  return <p>Total Views: {views}</p>
}
```

<div class="content-ad"></div>

useEffect의 동작과 주의사항을 고려해 보세요.

### 에러 처리

에러가 발생하면 클라이언트의 가장 가까운 error.js 또는 `Suspense` 경계에서 잡힐 것입니다. 우리는 try/catch를 사용하여 UI에서 처리할 수 있도록 에러를 반환하는 것을 권장합니다.

예를 들어, 새 항목을 생성하는 중에 에러가 발생하면 서버 액션에서 메시지를 반환하여 처리할 수 있습니다.

<div class="content-ad"></div>

```js
'use server'

export async function createTodo(prevState: any, formData: FormData) {
  try {
    // 데이터 변경
  } catch (e) {
    throw new Error('작업 생성에 실패했습니다')
  }
}
```

> 알아두면 좋은 사실:
에러를 throw하는 것 외에도 useFormState에서 처리할 객체를 반환할 수도 있습니다. 서버 측 유효성 검사 및 에러 처리를 참조하세요.

### 데이터 재유효성 검사

revalidatePath API를 사용하여 서버 액션 내에서 Next.js 캐시를 다시 유효화할 수 있습니다.

<div class="content-ad"></div>

```js
'서버 사용'

import { revalidatePath } from 'next/cache'

export async function createPost() {
  try {
    // ...
  } catch (error) {
    // ...
  }

  revalidatePath('/posts')
}
```

또는 revalidateTag를 사용하여 캐시 태그로 특정 데이터 가져오기를 무효화합니다:

```js
'서버 사용'

import { revalidateTag } from 'next/cache'

export async function createPost() {
  try {
    // ...
  } catch (error) {
    // ...
  }

  revalidateTag('posts')
}
```

### 페이지 리디렉션하기

<div class="content-ad"></div>

서버 액션 완료 후 사용자를 다른 경로로 리디렉션하려면 리디렉션 API를 사용할 수 있어요. 리디렉션은 try/catch 블록 바깥에서 호출해야 해요:

```js
'use server'
 
import { redirect } from 'next/navigation'
import { revalidateTag } from 'next/cache'
 
export async function createPost(id: string) {
  try {
    // ...
  } catch (error) {
    // ...
  }
 
  revalidateTag('posts') // 캐시된 게시물 업데이트
  redirect(`/post/${id}`) // 새 게시물 페이지로 이동
}
```

### 쿠키

서버 액션 내에서 쿠키를 얻거나 설정하거나 삭제할 수 있어요. 쿠키 API를 사용하세요:

<div class="content-ad"></div>

```js
'server'를 'serve'로 수정하세요.

import { cookies } from 'next/headers'로 변경하세요.

export async function exampleAction() {
  // 쿠키 가져오기
  const value = cookies().get('name')?.value;

  // 쿠키 설정하기
  cookies().set('name', 'Delba');

  // 쿠키 삭제하기
  cookies().delete('name');
}
```

서버 액션에서 쿠키를 삭제하는 추가적인 예제를 확인하세요.

## 보안

### 인증과 권한 부여

<div class="content-ad"></div>

서버 액션은 외부 API 엔드포인트와 마찬가지로 취급해야하며 사용자가 해당 작업을 수행할 권한이 있는지 확인해야 합니다. 예를 들어:

```js
'use server'
 
import { auth } from './lib'
 
export function addItem() {
  const { user } = auth()
  if (!user) {
    throw new Error('이 작업을 수행하려면 로그인해야합니다')
  }
 
  // ...
}
```

### 클로저 및 암호화

컴포넌트 내부에 Server Action을 정의하면 해당 작업이 외부 함수 범위에 액세스 할 수있는 클로저가 생성됩니다. 예를 들어, publish 액션은 publishVersion 변수에 액세스할 수 있습니다:

<div class="content-ad"></div>

```js
export default function Page() {
  const publishVersion = await getLatestVersion();
 
  async function publish(formData: FormData) {
    "use server";
    if (publishVersion !== await getLatestVersion()) {
      throw new Error('The version has changed since pressing publish');
    }
    ...
  }
 
  return <button action={publish}>Publish</button>;
}
```

클로저는 렌더링할 때 데이터 스냅샷(예: publishVersion)를 캡처하여 조치가 호출될 때 나중에 사용할 수 있는 경우에 유용합니다.

그러나 이를 위해 캡처된 변수들은 조치가 호출될 때 클라이언트로 보내지고 서버로 다시 전송됩니다. 민감한 데이터가 클라이언트에 노출되는 것을 방지하기 위해 Next.js는 자동으로 클로즈 오버 변수를 암호화합니다. 각 조치마다 Next.js 애플리케이션이 빌드될 때마다 새로운 개인 키가 생성됩니다. 이는 특정 빌드에 대해만 조치가 호출될 수 있음을 의미합니다.

> 이런 것도 알아두면 좋아요: 민감한 값이 클라이언트에 노출되는 것을 방지하기 위해 단독으로 암호화에 의존하는 것을 권장하지 않습니다. 대신, React 택트 API를 사용하여 특정 데이터가 클라이언트로 전송되는 것을 예방하는 것이 좋습니다.


<div class="content-ad"></div>

### 암호화 키 덮어쓰기 (고급)

여러 서버에 자체 호스팅하는 경우 Next.js 애플리케이션의 각 서버 인스턴스는 서로 다른 암호화 키를 가질 수 있어 잠재적 불일치가 발생할 수 있습니다.

이를 완화하기 위해 process.env.NEXT_SERVER_ACTIONS_ENCRYPTION_KEY 환경 변수를 사용하여 암호화 키를 덮어쓸 수 있습니다. 이 변수를 지정하면 빌드 간에 암호화 키가 지속되고 모든 서버 인스턴스가 동일한 키를 사용하도록 보장할 수 있습니다.

이는 여러 배포 간에 일관된 암호화 동작이 애플리케이션에 중요한 고급 사용 사례입니다. 키 교체 및 서명과 같은 표준 보안 관행을 고려해야 합니다.

<div class="content-ad"></div>

> 좋은 정보: Vercel에 배포된 Next.js 애플리케이션은 이를 자동으로 처리합니다.

### 허용된 출처 (고급)

서버 액션은 `form` 요소에서 호출될 수 있기 때문에 CSRF 공격에 노출될 수 있습니다.

서버 액션은 내부적으로 POST 방식을 사용하며, 오직 이 HTTP 방법만을 통해 호출할 수 있습니다. 이로써 대부분의 현대 브라우저에서 CSRF 취약점을 방지하며, 특히 SameSite 쿠키가 기본값인 경우 더욱 그렇습니다.

<div class="content-ad"></div>

추가 보호를 위해 Next.js의 Server Actions은 Origin 헤더를 Host 헤더 (또는 X-Forwarded-Host)와 비교합니다. 이 두 값이 일치하지 않으면 요청이 중단됩니다. 즉, Server Actions은 호스트 페이지와 동일한 호스트에서만 호출할 수 있습니다.

대규모 애플리케이션의 경우 역방향 프록시나 다층 백엔드 아키텍처(서버 API가 운영 도메인과 다른 경우)를 사용하는 경우, serverActions.allowedOrigins 옵션을 사용하여 안전한 원본 목록을 지정하는 것이 좋습니다. 이 옵션은 문자열 배열을 허용합니다.

```js
/** @type {import('next').NextConfig} */
module.exports = {
  experimental: {
    serverActions: {
      allowedOrigins: ['my-proxy.com', '*.my-proxy.com'],
    },
  },
}
```

보안 및 Server Actions에 대해 더 알아보기.

<div class="content-ad"></div>

## 추가 자료

서버 액션에 대한 자세한 정보를 원하신다면 다음 React 문서를 확인해 보세요:

- "use server"
- `form`
- useFormStatus
- useFormState
- useOptimistic