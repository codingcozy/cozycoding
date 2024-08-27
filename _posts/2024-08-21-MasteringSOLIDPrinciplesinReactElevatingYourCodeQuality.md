---
title: "React에서 SOLID 원칙으로 코드 퀄리티 높히는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-21 18:52
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Mastering SOLID Principles in React Elevating Your Code Quality"
link: "https://dev.to/vyan/mastering-solid-principles-in-react-elevating-your-code-quality-2c6h"
isUpdated: true
updatedAt: 1724245714404
---


강력하고 유지보수가 용이하며 확장 가능한 React 애플리케이션을 개발할 때는 SOLID 원칙을 적용하는 것이 중요합니다. 객체 지향 설계 원칙은 깔끔하고 효율적인 코드를 작성하기 위한 견고한 기반을 제공하여 React 구성 요소가 기능적이면서도 관리와 확장이 쉬운지를 보장합니다.

이 블로그에서는 각 SOLID 원칙을 React 개발에 적용하는 방법에 대해 다뤄볼 것이며, 이러한 개념을 설명하는 코드 예제도 함께 제시할 예정입니다.

## 1. 단일 책임 원칙 (SRP)

정의: 클래스 또는 구성 요소는 변경할 이유가 하나여야 하며, 즉 단일 책임에 집중해야 합니다.

<div class="content-ad"></div>

리액트에서: 각 컴포넌트는 특정 기능을 처리해야 합니다. 이렇게 하면 컴포넌트를 더 재사용 가능하고 디버그 또는 업데이트하기 쉬워집니다.

### 예시:

```js
// UserProfile.js
const UserProfile = ({ user }) => (
  <div>
    <h1>{user.name}</h1>
    <p>{user.bio}</p>
  </div>
);

// AuthManager.js
const AuthManager = () => (
  <div>
    {/* Authentication logic here */}
    로그인 폼
  </div>
);
```

이 예시에서 UserProfile은 사용자 프로필을 표시하는 것에만 관여하며, AuthManager는 인증 프로세스를 처리합니다. 이러한 책임을 분리함으로써 SRP를 따르며, 각 컴포넌트를 관리하고 테스트하기 쉽게 만듭니다.

<div class="content-ad"></div>

## 2. 오픈/폐쇄 원칙 (OCP)

정의: 소프트웨어 엔티티는 확장에는 열려 있지만 수정에는 닫혀 있어야 합니다.

React에서: 기능을 수정하지 않고도 새로운 기능으로 확장할 수 있는 컴포넌트를 디자인하세요. 이것은 대규모 애플리케이션의 안정성을 유지하는 데 중요합니다.

### 예시:

<div class="content-ad"></div>

```js
// Button.js
const Button = ({ label, onClick }) => (
  <button onClick={onClick}>{label}</button>
);

// IconButton.js
const IconButton = ({ icon, label, onClick }) => (
  <Button label={label} onClick={onClick}>
    <span className="icon">{icon}</span>
  </Button>
);
```

여기서 Button 컴포넌트는 간단하고 재사용 가능하며, IconButton은 아이콘을 추가하여 기존 Button 컴포넌트를 수정하지 않고 확장합니다. 이는 OCP를 준수하며 새로운 컴포넌트를 통해 확장을 허용합니다.

## 3. Liskov Substitution Principle (LSP)

정의: 수퍼 클래스의 객체는 하위 클래스의 객체로 대체될 수 있어야 하며 프로그램의 정확성에 영향을 미치지 않아야 합니다.


<div class="content-ad"></div>

리액트에서: 컴포넌트를 생성할 때 파생된 컴포넌트가 애플리케이션을 망가뜨리지 않고 부모 컴포넌트를 매끄럽게 대체할 수 있도록 해야 합니다.

### 예시:

```js
// Button.js
const Button = ({ label, onClick, className = '' }) => (
  <button onClick={onClick} className={`button ${className}`}>
    {label}
  </button>
);

// PrimaryButton.js
const PrimaryButton = ({ label, onClick, ...props }) => (
  <Button label={label} onClick={onClick} className="button-primary" {...props} />
);

// SecondaryButton.js
const SecondaryButton = ({ label, onClick, ...props }) => (
  <Button label={label} onClick={onClick} className="button-secondary" {...props} />
);
```

PrimaryButton과 SecondaryButton은 Button 컴포넌트를 확장하여 특정 스타일을 추가하지만 여전히 Button 컴포넌트와 상호 교체하여 사용할 수 있습니다. LSP를 준수함으로써 이러한 컴포넌트는 대체될 때 애플리케이션이 지속적으로 일관되고 버그가 없음을 보장합니다.

<div class="content-ad"></div>

## 4. Interface Segregation Principle (ISP)

**정의:** 클라이언트는 사용하지 않는 메소드에 의존하도록 강제해서는 안 됩니다.

**React에서:** 컴포넌트에 대해 하나의 큰, 단일 인터페이스 대신 더 작고 구체적인 인터페이스(props)를 만듭니다. 이렇게 하면 컴포넌트가 필요로 하는(props)만 수신하도록 할 수 있습니다.

### 예시:

<div class="content-ad"></div>

```js
// TextInput.js
const TextInput = ({ label, value, onChange }) => (
  <div>
    <label>{label}</label>
    <input type="text" value={value} onChange={onChange} />
  </div>
);

// CheckboxInput.js
const CheckboxInput = ({ label, checked, onChange }) => (
  <div>
    <label>{label}</label>
    <input type="checkbox" checked={checked} onChange={onChange} />
  </div>
);

// UserForm.js
const UserForm = ({ user, setUser }) => {
  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setUser((prevUser) => ({ ...prevUser, [name]: value }));
  };

  const handleCheckboxChange = (e) => {
    const { name, checked } = e.target;
    setUser((prevUser) => ({ ...prevUser, [name]: checked }));
  };

  return (
    <>
      <TextInput label="Name" value={user.name} onChange={handleInputChange} />
      <TextInput label="Email" value={user.email} onChange={handleInputChange} />
      <CheckboxInput label="Subscribe" checked={user.subscribe} onChange={handleCheckboxChange} />
    </>
  );
};
```

위 예시에서는 TextInput과 CheckboxInput이라는 특정 컴포넌트가 각각의 props를 가지고 있어서 UserForm은 ISP를 따라 필요한 props만 각 입력 컴포넌트로 전달하고 있습니다.

## 5. 의존 역전 원칙 (DIP)

정의: 고수준 모듈은 저수준 모듈에 의존해서는 안되며 둘 다 추상화에 의존해야 합니다.


<div class="content-ad"></div>

리액트에서는 훅스(hooks)와 컨텍스트(context)를 사용하여 의존성과 상태를 관리하고, 컴포넌트가 특정 구현에 강하게 결합되지 않도록 합니다.

### 예시:

#### 단계 1: 인증 서비스 인터페이스 정의

```js
// AuthService.js
class AuthService {
  login(email, password) {
    throw new Error("Method not implemented.");
  }
  logout() {
    throw new Error("Method not implemented.");
  }
  getCurrentUser() {
    throw new Error("Method not implemented.");
  }
}
export default AuthService;
```

<div class="content-ad"></div>

#### 단계 2: 특정 인증 서비스 구현

```js
// FirebaseAuthService.js
import AuthService from './AuthService';

class FirebaseAuthService extends AuthService {
  login(email, password) {
    console.log(`Firebase을 사용하여 ${email}으로 로그인 중`);
    // Firebase에 특화된 로그인 코드 추가
  }
  logout() {
    console.log("Firebase에서 로그아웃 중");
    // Firebase에 특화된 로그아웃 코드 추가
  }
  getCurrentUser() {
    console.log("Firebase에서 현재 사용자 가져오는 중");
    // Firebase에 특화된 현재 사용자 가져오는 코드 추가
  }
}

export default FirebaseAuthService;
```

```js
// AuthOService.js
import AuthService from './AuthService';

class AuthOService extends AuthService {
  login(email, password) {
    console.log(`AuthO를 사용하여 ${email}으로 로그인 중`);
    // AuthO에 특화된 로그인 코드 추가
  }
  logout() {
    console.log("AuthO에서 로그아웃 중");
    // AuthO에 특화된 로그아웃 코드 추가
  }
  getCurrentUser() {
    console.log("AuthO에서 현재 사용자 가져오는 중");
    // AuthO에 특화된 현재 사용자 가져오는 코드 추가
  }
}

export default AuthOService;
```

#### 단계 3: Auth Context 및 Provider 생성

<div class="content-ad"></div>

```js
// AuthContext.js
import React, { createContext, useContext } from 'react';

const AuthContext = createContext();

const AuthProvider = ({ children, authService }) => (
  <AuthContext.Provider value={authService}>
    {children}
  </AuthContext.Provider>
);

const useAuth = () => useContext(AuthContext);

export { AuthProvider, useAuth };
```

#### 단계 4: 로그인 컴포넌트에서 인증 서비스 사용하기

```js
// Login.js
import React, { useState } from 'react';
import { useAuth } from './AuthContext';

const Login = () => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const authService = useAuth();

  const handleLogin = () => {
    authService.login(email, password);
  };

  return (
    <div>
      <h1>로그인</h1>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="이메일을 입력하세요"
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        placeholder="비밀번호를 입력하세요"
      />
      <button onClick={handleLogin}>로그인</button>
    </div>
  );
};

export default Login;
```

#### 단계 5: 앱에 프로바이더 통합하기


<div class="content-ad"></div>

```js
// App.js
import React from 'react';
import { AuthProvider } from './AuthContext';
import FirebaseAuthService from './FirebaseAuthService';
import Login from './Login';

const authService = new FirebaseAuthService();

const App = () => (
  <AuthProvider authService={authService}>
    <Login />
  </AuthProvider>
);

export default App;
```

### React에서 DIP(Dependency Inversion Principle) 적용의 장점:

- Decoupling: 높은 수준의 컴포넌트(예: Login)와 낮은 수준의 구현 코드(FirebaseAuthService 및 AuthOService)가 분리됩니다. 이들은 추상화(AuthService)에 의존하여 코드가 유연해지고 유지보수가 쉬워집니다.
- 유연성: 다양한 인증 서비스 간의 전환이 간편합니다. AuthProvider에 전달되는 구현만 변경하면 됩니다. Login 컴포넌트를 수정할 필요가 없습니다.
- 테스트 용이성: 추상화의 사용으로 테스트에서 서비스를 가로채기 쉬워져 컴포넌트를 격리해서 테스트할 수 있습니다.


<div class="content-ad"></div>

## 결론

React에서 SOLID 원칙을 구현하면 코드의 품질을 높일 뿐만 아니라 애플리케이션의 유지 보수성과 확장성을 향상시킬 수 있습니다. 소규모 프로젝트든 대규모 애플리케이션이든, 이러한 원칙은 깨끗하고 효율적이며 견고한 React 개발을 위한 지침 역할을 합니다.

SOLID 원칙을 받아들이면 이해하기 쉽고 테스트하고 확장할 수 있는 컴포넌트를 만들어 개발 프로세스를 보다 효율적으로 만들고 애플리케이션을 더 신뢰할 수 있게 만들 수 있습니다. 그러니 다음에 React로 코드를 작성할 때는 이러한 원칙을 기억하고 어떤 변화를 가져다 주는지 확인해보세요!