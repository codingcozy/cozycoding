---
title: "Nextjs 14에서 효율적인 인증 구현 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:51
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Authentication"
link: "https://nextjs.org/docs/app/building-your-application/authentication"
---


# 인증

Next.js에서 인증을 구현하려면 세 가지 기본 개념을 익숙해져야 합니다.

- **인증(Authentication)**: 사용자가 자신이 주장하는 사람인지 확인합니다. 사용자는 사용자 이름과 비밀번호와 같은 것을 사용하여 자신의 신원을 확인해야 합니다.
- **세션 관리(Session Management)**: 사용자의 상태(예: 로그인)를 여러 요청간에 유지합니다.
- **권한 부여(Authorization)**: 사용자가 애플리케이션의 어떤 부분에 액세스할 수 있는지를 결정합니다.

이 페이지에서는 Next.js 기능을 사용하여 공통 인증, 권한 부여 및 세션 관리 패턴을 구현하는 방법을 보여줍니다. 이를 통해 애플리케이션의 요구에 맞는 최상의 솔루션을 선택할 수 있습니다.

<div class="content-ad"></div>

## 인증

인증은 사용자의 신원을 확인하는 과정입니다. 사용자가 로그인할 때 사용자의 신원을 확인합니다. 이는 사용자가 사용자 이름과 암호를 사용하거나 Google과 같은 서비스를 통해 로그인할 때 발생합니다. 사용자가 주장하는 대로 사용자가 실제로 누군지 확인하는 것이 중요합니다. 이를 통해 사용자의 데이터와 응용 프로그램이 무단 접근이나 부정 행위로부터 보호됩니다.

### 인증 전략

현대 웹 응용 프로그램은 일반적으로 여러 인증 전략을 사용합니다:

<div class="content-ad"></div>

- OAuth/OpenID Connect (OIDC): 사용자 자격 증명을 공유하지 않고 제3자 액세스를 활성화합니다. 소셜 미디어 로그인 및 단일 로그인(SSO) 솔루션에 이상적입니다. OpenID Connect로 신원 계층을 추가합니다.
- 자격 증명 기반 로그인 (이메일 + 비밀번호): 이메일과 비밀번호로 사용자가 로그인하는 웹 애플리케이션에 대한 표준 선택사항입니다. 익숙하고 구현이 쉽지만, 피싱과 같은 위협에 대비하기 위해 견고한 보안 조치가 필요합니다. 
- 비밀번호 없이/토큰 기반 인증: 안전하고 비밀번호 없이 액세스할 수 있도록 이메일 매직 링크나 SMS 일회용 코드를 사용합니다. 편리하고 향상된 보안을 제공하여 비밀번호 피로를 줄이는 데 도움이 됩니다. 그러나 사용자의 이메일 또는 전화번호 이용 가능 여부에 의존하는 한계가 있습니다.
- 패스키/웹인: 각 사이트에 고유한 암호화 자격 증명을 사용하여 피싱에 대한 높은 보안성을 제공합니다. 안전하지만 새로운 방법으로, 구현이 어려울 수 있습니다.

인증 전략을 선택할 때는 애플리케이션의 특정 요구 사항, 사용자 인터페이스 고려 사항 및 보안 목표와 일치해야 합니다.

### 인증 구현

이 섹션에서는 웹 애플리케이션에 기본적인 이메일-비밀번호 인증을 추가하는 프로세스를 탐색합니다. 이 방법은 기본 수준의 보안을 제공하지만, 흔한 보안 위협에 대한 강화된 보호를 위해 OAuth나 비밀번호 없는 로그인과 같은 더 고급 옵션을 고려하는 것이 좋습니다. 다룰 인증 흐름은 다음과 같습니다:

<div class="content-ad"></div>

- 사용자는 로그인 양식을 통해 자격 증명을 제출합니다.
- 양식은 서버 액션을 호출합니다.
- 성공적인 확인 후, 사용자의 성공적인 인증을 나타내는 프로세스가 완료됩니다.
- 확인이 실패하면 오류 메시지가 표시됩니다.

사용자가 자격 증명을 입력할 수 있는 로그인 양식을 고려해 보세요:

```js
import { authenticate } from '@/app/lib/actions'
 
export default function Page() {
  return (
    <form action={authenticate}>
      <input type="email" name="email" placeholder="이메일" required />
      <input type="password" name="password" placeholder="비밀번호" required />
      <button type="submit">로그인</button>
    </form>
  )
}
```

위의 양식은 사용자의 이메일과 비밀번호를 입력받는 두 가지 입력 필드가 있습니다. 제출 시, authenticate 서버 액션을 호출합니다.

<div class="content-ad"></div>

서버 액션에서 인증 처리를 위해 Authentication Provider의 API를 호출할 수 있습니다.

```js
'use server'
 
import { signIn } from '@/auth'
 
export async function authenticate(_currentState: unknown, formData: FormData) {
  try {
    await signIn('credentials', formData)
  } catch (error) {
    if (error) {
      switch (error.type) {
        case 'CredentialsSignin':
          return '잘못된 자격 증명입니다.'
        default:
          return '문제가 발생했습니다.'
      }
    }
    throw error
  }
}
```

위 코드에서 signIn 메서드는 저장된 사용자 데이터와 자격 증명을 비교합니다. 인증 제공자가 자격 증명을 처리한 후, 다음 두 가지 결과가 가능합니다:

- 성공적인 인증: 이 결과는 로그인이 성공적이라는 것을 의미합니다. 보호된 경로에 접근하거나 사용자 정보를 가져오는 등의 추가 작업을 시작할 수 있습니다.
- 실패한 인증: 자격 증명이 잘못되었거나 오류가 발생한 경우, 함수는 해당하는 오류 메시지를 반환하여 인증 실패를 나타냅니다.

<div class="content-ad"></div>

마지막으로 login-form.tsx 컴포넌트에서 React의 useFormState를 사용하여 서버 액션을 호출하고 폼 오류를 처리하고 useFormStatus를 사용하여 폼의 대기 상태를 처리할 수 있습니다:

```js
'use client'
 
import { authenticate } from '@/app/lib/actions'
import { useFormState, useFormStatus } from 'react-dom'
 
export default function Page() {
  const [errorMessage, dispatch] = useFormState(authenticate, undefined)
 
  return (
    <form action={dispatch}>
      <input type="email" name="email" placeholder="이메일" required />
      <input type="password" name="password" placeholder="비밀번호" required />
      <div>{errorMessage && <p>{errorMessage}</p>}</div>
      <LoginButton />
    </form>
  )
}
 
function LoginButton() {
  const { pending } = useFormStatus()
 
  const handleClick = (event) => {
    if (pending) {
      event.preventDefault()
    }
  }
 
  return (
    <button aria-disabled={pending} type="submit" onClick={handleClick}>
      로그인
    </button>
  )
}
```

Next.js 프로젝트에서 특히 여러 로그인 방법을 제공할 때 보다 간단한 인증 설정을 위해 포괄적인 인증 솔루션을 사용해 보세요.

## 권한

<div class="content-ad"></div>

사용자가 인증되면 사용자가 특정 경로를 방문하거나 서버 액션을 통해 데이터를 변이하거나 라우트 핸들러를 호출할 수 있는지 확인해야 합니다.

### 미들웨어를 사용한 라우트 보호

Next.js에서의 미들웨어를 사용하면 웹사이트의 다른 부분에 누가 접근할 수 있는지 제어할 수 있습니다. 사용자 대시보드와 같은 영역을 보호하는 것이 중요하며 마케팅 페이지와 같은 다른 페이지는 공개되어야 합니다. 모든 라우트에 미들웨어를 적용하고 공개 액세스를 위해 예외를 지정하는 것이 좋습니다.

다음은 Next.js에서 인증을 위한 미들웨어를 구현하는 방법입니다:

<div class="content-ad"></div>

#### 미들웨어 설정하기:

- 프로젝트의 루트 디렉토리에 미들웨어.ts 또는 .js 파일을 만드세요.
- 인증 토큰을 확인하는 같은 사용자 액세스를 승인하는 로직을 포함하세요.

#### 보호된 라우트 정의하기:

- 모든 라우트가 권한 부여를 요구하지는 않습니다. Middleware에 matcher 옵션을 사용하여 권한 부여 검사가 필요하지 않은 라우트를 지정하세요.

<div class="content-ad"></div>

#### 미들웨어 로직:

- 사용자가 인증되었는지 확인하는 로직을 작성하세요. 경로 권한을 위해 사용자 역할이나 권한을 확인하세요.

#### 권한이없는 액세스 처리:

- 권한이 없는 사용자를 적절한 경우 로그인 또는 오류 페이지로 리디렉션하세요.

<div class="content-ad"></div>

예시 미들웨어 파일:

```js
import type { NextRequest } from 'next/server'
 
export function middleware(request: NextRequest) {
  const currentUser = request.cookies.get('currentUser')?.value
 
  if (currentUser && !request.nextUrl.pathname.startsWith('/dashboard')) {
    return Response.redirect(new URL('/dashboard', request.url))
  }
 
  if (!currentUser && !request.nextUrl.pathname.startsWith('/login')) {
    return Response.redirect(new URL('/login', request.url))
  }
}
 
export const config = {
  matcher: ['/((?!api|_next/static|_next/image|.*\\.png$).*)'],
}
```

이 예시는 요청 파이프라인에서 일찍 리디렉션을 처리하기 위해 Response.redirect를 사용합니다. 이를 통해 효율적으로 관리되는 접근 제어 중심을 구축할 수 있습니다.

특정 리디렉션이 필요한 경우 Server Components, Route Handlers 및 Server Actions에서 리디렉트 함수를 사용하여 더 많은 제어를 제공할 수 있습니다. 이는 역할 기반 탐색이나 컨텍스트에 민감한 시나리오에 유용합니다.

<div class="content-ad"></div>

```js
import { redirect } from 'next/navigation'
 
export default function Page() {
  // Redirect 필요 여부를 결정하는 로직
  const accessDenied = true
  if (accessDenied) {
    redirect('/login')
  }
 
  // 다른 경로 및 로직 정의
}
```

인증에 성공한 후에는 사용자 탐색을 사용자 역할에 기반하여 관리하는 것이 중요합니다. 예를 들어, 관리자 사용자는 관리자 대시보드로 리디렉션되고, 일반 사용자는 다른 페이지로 보내집니다. 이는 역할별 경험과 필요한 경우 사용자가 프로필 작성을 완료하도록 유도하기 위한 조건부 탐색에 중요합니다.

인가를 설정할 때, 앱이 데이터에 액세스하거나 데이터를 변경하는 곳에서 주요 보안 검사가 수행되어야 합니다. 미들웨어는 초기 유효성 검사에 유용할 수 있지만, 보안을 위한 유일한 방어선으로 사용해서는 안 됩니다. 보안 검사의 대부분은 데이터 액세스 레이어(DAL)에서 수행되어야 합니다.

이 접근 방식은 보안 블로그에서 강조되며, 모든 데이터 액세스를 전용 DAL(데이터 액세스 레이어) 내에서 통합하는 것을 지지합니다. 이 전략은 일관된 데이터 액세스를 보장하고, 인가 버그를 최소화하며, 유지 보수를 간단하게 합니다. 포괄적인 보안을 보장하기 위해 다음 주요 영역을 고려해 보세요:

<div class="content-ad"></div>

- 서버 작업: 서버 측 프로세스에서 보안 검사를 구현하세요. 특히 민감한 작업에 대해 중요합니다.
- 라우트 핸들러: 보안 조치를 취한 상태로 들어오는 요청을 관리하여 권한이 있는 사용자만 접근하도록 합니다.
- 데이터 액세스 레이어 (DAL): 데이터베이스와 직접 상호작용하며 데이터 거래를 확인하고 인가하는 데 중요합니다. DAL 내부에서 중요한 검사를 수행하여 데이터를 가장 중요한 상호작용 지점, 즉 액세스 또는 수정 시 안전하게 보호해야 합니다.

DAL 보안 가이드를 자세히 알아보고 예제 코드 스니펫 및 고급 보안 관행을 확인하려면 보안 가이드의 데이터 액세스 레이어 섹션을 참조하세요.

### 서버 작업 보호하기

서버 작업을 공개 API 엔드포인트와 동일한 보안 고려사항으로 다루는 것이 중요합니다. 각 작업에 대한 사용자 권한을 확인하는 것이 중요합니다. 서버 작업 내에서 사용자 권한을 결정하기 위해 확인을 구현하고 특정 작업을 관리자 사용자에게 제한하는 등의 조치를 취하세요.

<div class="content-ad"></div>

아래 예시에서는 사용자의 역할을 확인한 후에 액션이 진행되는지 여부를 확인합니다:

```js
'use server'

// ...

export async function serverAction() {
  const session = await getSession()
  const userRole = session?.user?.role

  // 사용자가 행동을 수행할 권한이 있는지 확인
  if (userRole !== 'admin') {
    throw new Error('사용 권한이 없음: 사용자가 관리자 권한을 가지고 있지 않습니다.')
  }

  // 권한이 있는 사용자의 경우에만 작업을 계속합니다
  // ... 작업 구현 코드
}
```

### 경로 핸들러 보호

Next.js의 라우트 핸들러는 들어오는 요청을 처리하는 데 중요한 역할을 합니다. 서버 액션과 마찬가지로, 특정 기능에만 허가된 사용자가 액세스할 수 있도록 보호되어야 합니다. 이는 주로 사용자의 인증 상태와 권한을 확인하는 작업을 포함합니다.

<div class="content-ad"></div>

여기에 Route Handler를 안전하게 보호하는 예제가 있습니다:

```js
export async function GET() {
  // 사용자 인증 및 역할 확인
  const session = await getSession()
 
  // 사용자가 인증되었는지 확인
  if (!session) {
    return new Response(null, { status: 401 }) // 사용자가 인증되지 않음
  }
 
  // 사용자가 'admin' 역할을 가지고 있는지 확인
  if (session.user.role !== 'admin') {
    return new Response(null, { status: 403 }) // 사용자는 인증되었지만 올바른 권한이 없음
  }
 
  // 권한이 있는 사용자를 위한 데이터 가져오기
}
```

이 예제는 인증 및 권한 부여에 대한 이중 보안 확인이 있는 Route Handler를 보여줍니다. 먼저 활성 세션을 확인하고, 그런 다음 로그인한 사용자가 `admin`인지 확인합니다. 이 접근 방식은 인증된 사용자에게 제한된 안전한 액세스를 보장하며, 요청 처리에 대한 견고한 보안을 유지합니다.

### 서버 컴포넌트 사용을 통한 권한 부여

<div class="content-ad"></div>

Next.js의 서버 컴포넌트는 서버 측 실행을 위해 설계되었고 권한 부여와 같은 복잡한 로직을 통합할 수 있는 안전한 환경을 제공합니다. 이를 통해 백엔드 리소스에 직접 액세스하여 데이터 중심 작업의 성능을 최적화하고 민감한 작업에 대한 보안을 강화할 수 있습니다.

서버 컴포넌트에서는 사용자의 역할에 따라 UI 요소를 조건부로 렌더링하는 것이 일반적입니다. 이 접근 방식을 통해 사용자 경험과 보안을 향상시키며 사용자가 볼 수 있는 콘텐츠에 대해 인가된 사용자만 액세스할 수 있도록 보장합니다.

예시:

```js
export default async function Dashboard() {
  const session = await getSession()
  const userRole = session?.user?.role // 세션 객체에 'role'이 포함되어 있다고 가정
 
  if (userRole === 'admin') {
    return <AdminDashboard /> // 관리자 사용자용 컴포넌트
  } else if (userRole === 'user') {
    return <UserDashboard /> // 일반 사용자용 컴포넌트
  } else {
    return <AccessDenied /> // 권한이 없는 액세스에 대한 표시되는 컴포넌트
  }
}
```

<div class="content-ad"></div>

이 예제에서 대시보드 컴포넌트는 `admin`, `user`, 및 비승인 역할에 대해 다른 UI를 렌더링합니다. 이 패턴은 각 사용자가 자신의 역할에 적합한 컴포넌트와 상호 작용하도록 보장하여 보안과 사용자 경험을 향상시킵니다.

### Best Practices

- 안전한 세션 관리: 세션 데이터의 보안을 우선시하여 무단 액세스와 데이터 침해를 방지하세요. 암호화 및 안전한 저장 관행을 사용하세요.
- 동적 역할 관리: 사용자 역할에 대한 유연한 시스템을 사용하여 권한과 역할의 변경에 쉽게 대응하고 하드코딩된 역할을 피하세요.
- 보안 우선 접근: 인가 논리의 모든 측면에서 보안을 우선시하여 사용자 데이터의 안전을 보호하고 응용 프로그램의 무결성을 유지하세요. 이는 철저한 테스트와 잠재적인 보안 취약점을 고려하는 것을 포함합니다.

## 세션 관리

<div class="content-ad"></div>

세션 관리는 사용자가 애플리케이션과 상호 작용하는 방식을 추적하고 관리하며, 사용자의 인증된 상태가 애플리케이션의 다른 부분에서도 유지되도록 보장하는 것을 말합니다.

이를 통해 반복 로그인을 필요로하지 않게 되어 보안과 사용자 편의성이 모두 향상됩니다. 세션 관리에는 쿠키 기반과 데이터베이스 세션 두 가지 주요 방법이 사용됩니다.

### 쿠키 기반 세션

> 🎥 시청: Next.js를 사용한 쿠키 기반 세션 및 인증에 대해 자세히 알아보기 → YouTube (11분)

<div class="content-ad"></div>

쿠키 기반 세션은 브라우저 쿠키에 암호화된 세션 정보를 저장하여 사용자 데이터를 관리합니다. 사용자가 로그인하면 이 암호화된 데이터가 쿠키에 저장됩니다. 각 후속 서버 요청에는 이 쿠키가 포함되어 반복적인 서버 쿼리 요청이 줄어들고 클라이언트 측 효율성이 향상됩니다.

그러나 이 방법은 민감한 데이터를 보호하기 위해 신중한 암호화가 필요하며, 쿠키는 클라이언트 측 보안 위험에 취약합니다. 쿠키에 세션 데이터를 암호화하는 것은 사용자 정보를 무단으로 액세스하는 것으로부터 보호하는 데 중요합니다. 쿠키가 도난당해도 내부 데이터가 해독 불가능하게 만드는 것이 보장됩니다.

또한, 개별 쿠키의 크기는 제한되어 있지만(일반적으로 약 4KB), 쿠키 청킹과 같은 기술을 사용하면 큰 세션 데이터를 여러 개의 쿠키로 분할하여 이 제한을 극복할 수 있습니다.

Next.js 프로젝트에서 쿠키를 설정하는 방법은 다음과 같을 것입니다:

<div class="content-ad"></div>

서버에서 쿠키 설정하기:

```js
'use server'
 
import { cookies } from 'next/headers'
 
export async function handleLogin(sessionData) {
  const encryptedSessionData = encrypt(sessionData) // 세션 데이터를 암호화합니다
  cookies().set('session', encryptedSessionData, {
    httpOnly: true,
    secure: process.env.NODE_ENV === 'production',
    maxAge: 60 * 60 * 24 * 7, // 일주일
    path: '/',
  })
  // 쿠키 설정 후에 리디렉션하거나 응답을 처리합니다
}
```

서버 컴포넌트에서 쿠키에 저장된 세션 데이터에 액세스하기:

```js
import { cookies } from 'next/headers'
 
export async function getSessionData(req) {
  const encryptedSessionData = cookies().get('session')?.value
  return encryptedSessionData ? JSON.parse(decrypt(encryptedSessionData)) : null
}
```

<div class="content-ad"></div>

### 데이터베이스 세션

데이터베이스 세션 관리는 세션 데이터를 서버에 저장하고 사용자 브라우저는 세션 ID만 수신하는 것을 의미합니다. 이 ID는 데이터 자체를 포함하지 않고 서버 측에 저장된 세션 데이터를 참조합니다. 이 방법은 보안을 강화하여 민감한 세션 데이터를 클라이언트 측 환경으로부터 격리시키고 클라이언트 측 공격에 노출될 위험을 줄입니다. 데이터베이스 세션은 더 많은 데이터 저장 요구를 수용하여 확장성이 더 뛰어납니다.

그러나 이 방법에는 상충관계가 있습니다. 각 사용자 상호 작용마다 데이터베이스 조회가 필요하여 성능 오버헤드가 증가할 수 있습니다. 세션 데이터 캐싱과 같은 전략을 통해 이를 완화할 수 있습니다. 또한 데이터베이스에 의존하는 것은 세션 관리가 데이터베이스의 성능 및 가용성만큼 신뢰성이 있다는 것을 의미합니다.

다음은 Next.js 애플리케이션에서 데이터베이스 세션을 구현하는 간단한 예제입니다:

<div class="content-ad"></div>

서버에서 세션 생성하기:

```js
import db from './lib/db'
 
export async function createSession(user) {
  const sessionId = generateSessionId() // 고유한 세션 ID 생성
  await db.insertSession({ sessionId, userId: user.id, createdAt: new Date() })
  return sessionId
}
```

미들웨어 또는 서버 측 로직에서 세션 검색하기:

```js
import { cookies } from 'next/headers'
import db from './lib/db'
 
export async function getSession() {
  const sessionId = cookies().get('sessionId')?.value
  return sessionId ? await db.findSession(sessionId) : null
}
```

<div class="content-ad"></div>

### Next.js에서 세션 관리 선택하기

Next.js에서 쿠키 기반 또는 데이터베이스 세션을 선택하는 것은 애플리케이션의 요구에 따라 다릅니다. 쿠키 기반 세션은 더 간단하며 서버 부하가 낮은 소규모 애플리케이션에 적합하지만 보안성이 덜 할 수 있습니다. 반면에 데이터베이스 세션은 더 복잡하지만 더 나은 보안성과 확장성을 제공하여 대규모이거나 데이터에 민감한 애플리케이션에 이상적입니다.

NextAuth.js와 같은 인증 솔루션을 사용하면 세션 관리가 더욱 효율적해지며 쿠키나 데이터베이스 저장소를 사용할 수 있습니다. 이러한 자동화는 개발 프로세스를 간소화하지만 선택한 솔루션이 사용하는 세션 관리 방법을 이해하는 것이 중요합니다. 애플리케이션의 보안과 성능 요구 사항과 일치하는지 확인하십시오.

선택하든지 간에 세션 관리 전략에서 보안을 우선시하세요. 쿠키 기반 세션의 경우 안전하고 HTTP-만 쿠키 사용이 세션 데이터를 보호하는 데 중요합니다. 데이터베이스 세션의 경우, 정기적인 백업 및 세션 데이터 안전한 처리가 필수적입니다. 무단 액세스를 방지하고 애플리케이션의 성능 및 신뢰성을 유지하기 위해 세션 만료 및 정리 메커니즘을 구현하는 것이 중요합니다.

<div class="content-ad"></div>

## 예시

다음은 Next.js와 호환되는 인증 솔루션입니다. 아래 빠른 시작 가이드를 참조하여 귀하의 Next.js 애플리케이션에 이러한 솔루션을 구성하는 방법을 알아보세요:

- Auth0
- Clerk
- Kinde
- Lucia
- NextAuth.js
- Supabase
- Stytch
- Iron Session

## 추가 자료

<div class="content-ad"></div>

인증 및 보안에 대한 학습을 계속하려면 다음 자료들을 확인해보세요:

- XSS 공격 이해
- CSRF 공격 이해