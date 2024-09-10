---
title: "타입스크립트 면접 준비하며 자주 묻히는 이슈에 도전해보세요"
description: ""
coverImage: "/assets/img/2024-09-10-ThinkYouKnowTypeScriptTackleTheseCommonInterviewQuestions_0.png"
date: 2024-09-10 18:44
ogImage: 
  url: /assets/img/2024-09-10-ThinkYouKnowTypeScriptTackleTheseCommonInterviewQuestions_0.png
tag: Tech
originalTitle: "Think You Know TypeScript Tackle These Common Interview Questions"
link: "https://medium.com/frontend-canteen/think-you-know-typescript-tackle-these-common-interview-questions-4a811d50bb2a"
isUpdated: false
---


<img src="/assets/img/2024-09-10-ThinkYouKnowTypeScriptTackleTheseCommonInterviewQuestions_0.png" />

TypeScript은 현대 웹 개발에서 프론트엔드 및 백엔드 엔지니어 모두에게 필수 요소가 되었습니다. TypeScript 인터뷰에서 가장 자주 묻히는 질문 중에는 두 가지 주제가 돋보입니다: 제네릭과 유틸리티 타입입니다. 이러한 개념을 이해하는 것은 유연하고 유지보수 가능하며 타입 안전한 코드를 작성하려는 모든 개발자에게 중요합니다.

# 제네릭이란 무엇인가요?

TypeScript의 제네릭은 다양한 데이터 유형과 함께 작동할 수 있는 컴포넌트(함수, 클래스 또는 인터페이스)를 만드는 방법을 제공합니다. 제네릭을 사용하면 다양한 데이터 유형에 적응할 수 있는 재사용 가능한 코드를 작성할 수 있어 코드를 더 유연하고 유지보수 가능하게 만들 수 있습니다.

<div class="content-ad"></div>

# `T` 구문

`T` 구문은 TypeScript에서 제네릭 함수, 클래스 또는 인터페이스를 정의하는 데 사용됩니다. T는 호출자가 제공하거나 TypeScript 자체에서 추론할 유형을 나준하는 자리 표시자로 작용합니다. 이를 통해 함수 또는 클래스가 유형 안전성을 잃지 않고 다양한 유형을 처리할 수 있습니다.

```javascript
function deepCopy<T>(obj: T): T {
  return JSON.parse(JSON.stringify(obj));
}

const original = { name: "Alice", details: { age: 25, country: "USA" } };
const copied = deepCopy(original);

copied.details.age = 30;
console.log(original.details.age); // 출력: 25
console.log(copied.details.age); // 출력: 30
```

# 제네릭의 장점

<div class="content-ad"></div>

- 재사용성: 한 번 코드를 작성하고 어떤 데이터 유형이든 사용할 수 있습니다.
- 유형 안전성: 실행 시간이 아닌 컴파일 시간에 오류를 감지합니다.
- 유연성: 여러 유형과 작동하는 유연한 데이터 구조와 알고리즘을 만듭니다.

## TypeScript의 유틸리티 유형

TypeScript는 기존 유형을 더 유연하게 조작하고 변환할 수 있게 해주는 여러 내장 유틸리티 유형을 제공합니다. 이러한 유틸리티 유형은 기존 유형에서 새로운 유형을 만들거나 응용 프로그램의 유형 안전성을 향상하는 데 귀중합니다.

## 자주 사용되는 유틸리티 유형

<div class="content-ad"></div>

## 부분 `Type`:

부분 유틸리티 타입은 Type의 모든 속성을 선택적으로 만드는 유형을 구성합니다. 이는 유형에서 정의된 속성 중 일부만 필요한 객체를 만들고 싶을 때 유용합니다.

```js
interface Config {
  host: string;
  port: number;
  useSSL: boolean;
}

function createConfig(overrides: Partial<Config>): Config {
  const defaultConfig: Config = { host: "localhost", port: 8080, useSSL: false };
  return { ...defaultConfig, ...overrides };
}

const config1 = createConfig({ port: 3000 });
const config2 = createConfig({ host: "example.com", useSSL: true });

console.log(config1); // 출력: { host: "localhost", port: 3000, useSSL: false }
console.log(config2); // 출력: { host: "example.com", port: 8080, useSSL: true }
```

## 선택 `Type, Keys`

<div class="content-ad"></div>

`Pick` 유틸리티 타입은 기존 타입(Type)에서 일부 프로퍼티(키)를 선택하여 새로운 타입을 생성합니다. 이는 특정 프로퍼티만 포함하는 타입을 만들 때 유용합니다.

```js
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}

type SafeUser = Pick<User, "id" | "name" | "email">;

function logUser(user: SafeUser): void {
  console.log(`ID: ${user.id}, 이름: ${user.name}, 이메일: ${user.email}`);
}

const user: User = { id: 1, name: "Alice", email: "alice@example.com", password: "securePass123" };
logUser({ id: user.id, name: user.name, email: user.email });
// 출력: ID: 1, 이름: Alice, 이메일: alice@example.com
```

## `Omit<Type, Keys>`

`Omit` 유틸리티 타입은 기존 타입(Type)에서 일부 프로퍼티(키)를 제거하여 새로운 타입을 구성합니다. 이는 특정 프로퍼티를 제외한 타입을 만들 때 유용합니다.

<div class="content-ad"></div>

```js
인터페이스 User {
  id: number;
  name: string;
  email: string;
  password: string; 
}

// User 타입에서 'password' 필드를 제외한 UserWithoutPassword 타입을 정의합니다
type UserWithoutPassword = Omit<User, "password">;

const user: UserWithoutPassword = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};

// 사용 예
console.log(user); // 출력: { id: 1, name: 'Alice', email: 'alice@example.com' }
```

## Readonly`Type`

Readonly 유틸리티 타입은 Type의 모든 속성을 읽기 전용으로 설정된 타입을 구성합니다. 이는 구성된 타입의 속성을 재할당할 수 없다는 것을 의미합니다.

```js
인터페이스 Todo {
  title: string;
  description: string;
  completed: boolean;
}

// 모든 Todo 속성을 읽기 전용으로 만듭니다
const todo: Readonly<Todo> = {
  title: "Learn TypeScript",
  description: "Study generics and utility types",
  completed: false,
};


console.log(todo); // 출력: { title: 'Learn TypeScript', description: 'Study generics and utility types', completed: false }
```