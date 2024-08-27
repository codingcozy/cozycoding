---
title: "리액트 프로젝트에 Redux Toolkit을 설정하는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-HowcanIintegrateReduxToolkitintomyReactproject_0.png"
date: 2024-08-26 18:29
ogImage: 
  url: /assets/img/2024-08-26-HowcanIintegrateReduxToolkitintomyReactproject_0.png
tag: Tech
originalTitle: "How can I integrate Redux Toolkit into my React project"
link: "https://medium.com/@rashmipatil24/redux-toolkit-in-react-project-10624b2ee8ad"
isUpdated: true
updatedAt: 1724743522156
---


## 단계별 안내

대규모 React 애플리케이션을 만들 때 상태를 관리하는 것은 쉽지 않을 수 있습니다. 애플리케이션이 커질수록 상태의 복잡성도 증가합니다. 그럴 때 Redux가 필요한데요, Redux는 애플리케이션의 상태를 보다 조직적이고 확장 가능한 방식으로 관리할 수 있는 예측 가능한 상태 컨테이너를 제공합니다. 하지만 대규모 코드베이스를 다룰 때는 Redux조차 복잡해질 수 있습니다. 이때 Redux Toolkit이 등장하는데요, Redux로 상태를 관리하는 프로세스를 단순화하고 최적화하기 위해 설계된 일련의 도구 및 모범 사례를 제공합니다.

이 블로그에서는 Redux Toolkit이 대규모 React 애플리케이션에서 복잡한 상태를 관리하는 데 어떻게 도움이 되는지 자세히 살펴볼 것입니다. Redux Toolkit이 무엇인지, 사용해야 하는 이유, 공통 Redux 작업을 어떻게 간편화하는지를 다룰 것입니다. 마지막에는 Redux Toolkit을 활용하여 상태 관리를 더 효율적이고 유지보수 가능하게 하는 방법에 대해 확고한 이해를 갖게 될 것입니다.

<img src="/assets/img/2024-08-26-HowcanIintegrateReduxToolkitintomyReactproject_0.png" />

<div class="content-ad"></div>

## 레덕스 툴킷이란?

레덕스 툴킷은 레덕스 로직을 작성하는 공식적이고 권장하는 방법입니다. 이는 레덕스 애플리케이션에서 발생하는 일반적인 문제인 보일러플레이트 코드, 번거로운 구성, 그리고 대규모 애플리케이션에서 상태 관리를 확장하는 어려움 등을 해결하기 위해 고안되었습니다.

레덕스 툴킷은 다음을 보다 쉽게할 수 있도록 하는 일련의 도구를 제공합니다:

- 합리적인 기본값으로 레덕스 스토어를 설정합니다.
- 자동 액션 생성자와 리듀서를 가진 상태 조각들을 생성합니다.
- 부수 효과, 미들웨어, 비동기 작업과 같은 복잡한 상태 로직을 처리합니다.
- 최상의 관행을 따르면서도 보일러플레이트의 양을 줄입니다.

<div class="content-ad"></div>

# Redux Toolkit을 사용하는 이유

Redux Toolkit을 사용해야 하는 이유에 대해 알아보기 전에, 다음 React 프로젝트에 고려해야 하는 이유에 대해 이야기해보겠습니다.

- 더 적은 보일러플레이트 코드: 전통적인 Redux 설정에는 많은 보일러플레이트 코드가 포함됩니다. 액션, 리듀서를 수동으로 만들고, 상태와 연결해야 하는 번거로움이 있습니다. Redux Toolkit은 대부분의 작업을 추상화하여 응용 프로그램 로직 작성에 집중할 수 있도록 합니다.

- 간편화된 설정: Redux Toolkit은 추천하는 기본값과 함께 스토어를 설정하는 configureStore 함수를 제공합니다. Redux DevTools 통합과 미들웨어 설정과 같은 작업을 자동화하여 설정을 간소화합니다.

- 개발자 경험의 향상: Redux Toolkit은 디버깅 및 테스트를 단순화하는 내장 도구를 제공하여 응용 프로그램의 유지 및 확장을 쉽게할 수 있습니다.

- 성능 향상: Redux Toolkit의 자동 작업 일괄 처리와 같은 기능을 통해 응용 프로그램의 성능을 개선할 수 있습니다. 특히 상태 업데이트가 많은 시나리오에서 성능을 향상시킬 수 있습니다.

# React 애플리케이션에 Redux Toolkit 설정하기

<div class="content-ad"></div>

기본적인 Redux Toolkit 설정을 React 애플리케이션에서 안내해 드리겠습니다.

## 단계 1: Redux Toolkit 설치하기

먼저 Redux Toolkit 패키지와 함께 React-Redux를 설치해야 합니다:

```js
npm install @reduxjs/toolkit react-redux
```

<div class="content-ad"></div>

## 단계 2: 상태 슬라이스 생성하기

Redux Toolkit에서는 슬라이스를 생성하여 상태를 관리합니다. 슬라이스는 응용 프로그램의 단일 기능을 위한 Redux 리듀서 로직과 액션의 모음입니다. 다음은 슬라이스를 만드는 방법입니다:

```js
import { createSlice } from '@reduxjs/toolkit';
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
});
export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

이 예시에서는 초기 상태가 0인 counterSlice를 만들었습니다. 이 슬라이스에는 increment, decrement, incrementByAmount 세 가지 리듀서 함수가 포함되어 있습니다. 이 함수들은 전달된 액션에 따라 상태를 업데이트합니다.

<div class="content-ad"></div>

## 단계 3: 스토어 구성

다음으로는 Redux Toolkit에서 제공하는 configureStore 함수를 사용하여 Redux 스토어를 구성하겠습니다:

```js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});

export default store;
```

이렇게 하면 Redux 스토어가 구성되고 카운터 슬라이스가 연결됩니다.

<div class="content-ad"></div>

## 단계 4: React 컴포넌트에서 저장소 사용하기

마지막으로, react-redux의 useSelector 및 useDispatch 훅을 사용하여 React 컴포넌트에서 Redux 저장소를 사용할 수 있습니다:

```js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement, incrementByAmount } from './counterSlice';
function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();
  return (
    <div>
      <p>{count}</p>
      <button onClick={() => dispatch(increment())}>증가</button>
      <button onClick={() => dispatch(decrement())}>감소</button>
      <button onClick={() => dispatch(incrementByAmount(5))}>5만큼 증가</button>
    </div>
  );
}
export default Counter;
```

이 예제에서 Counter 컴포넌트는 현재 카운트를 표시하고 증가, 감소 또는 특정 양 만큼 증가하는 버튼을 제공합니다.

<div class="content-ad"></div>

# 발전된 사용법: createAsyncThunk를 사용하여 비동기 논리 처리하기

상태 관리의 가장 어려운 측면 중 하나는 API 호출과 같은 비동기 작업을 처리하는 것입니다. Redux Toolkit은 createAsyncThunk를 사용하여 비동기 작업을 정의하고 처리할 수 있는 유틸리티로 이를 간편화합니다.

createAsyncThunk를 사용하는 예제입니다:

```js
import { createAsyncThunk, createSlice } from '@reduxjs/toolkit';
import axios from 'axios';
export const fetchUser = createAsyncThunk('user/fetchUser', async (userId) => {
  const response = await axios.get(`/api/user/${userId}`);
  return response.data;
});
const userSlice = createSlice({
  name: 'user',
  initialState: { user: null, status: 'idle' },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUser.pending, (state) => {
        state.status = 'loading';
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.user = action.payload;
      })
      .addCase(fetchUser.rejected, (state) => {
        state.status = 'failed';
      });
  },
});
export default userSlice.reducer;
```

<div class="content-ad"></div>

이 예제에서는 createAsyncThunk를 사용하여 비동기 작업 fetchUser를 정의합니다. userSlice는 다른 상태(pending, fulfilled, rejected)를 처리하기 위해 extraReducers를 사용합니다.

Redux Toolkit은 대규모 React 애플리케이션에서 상태 관리를 크게 단순화할 수 있는 강력한 도구입니다. 보일러플레이트 코드를 줄이고 구성을 간소화하며 복잡한 로직을 처리하기 위한 강력한 도구를 제공함으로써, Redux Toolkit을 사용하면 정말 중요한 부분에 집중할 수 있습니다: 훌륭한 애플리케이션을 만드는 것입니다.

Redux를 처음 시작하거나 코드베이스를 개선하려는 경험이 많은 개발자라면 Redux Toolkit을 살펴볼 가치가 있습니다. 올바른 방법으로 접근하면 응용 프로그램에서 심지어 가장 복잡한 상태도 쉽게 관리할 수 있습니다.

본 가이드가 도움이 되었기를 바라겠습니다! 질문이나 인사이트가 있으시면 아래에 댓글을 남겨주시기 바랍니다 - 대화를 이어가요. 이 게시물이 마음에 든다면 박수를 보내고 네트워크와 공유하는 것을 잊지 마세요. 더 많은 웹 개발 팁, 자습서 및 산업 동향을 보려면 Medium에서 저를 팔로우하고 최신 기사를 확인하세요. 즐거운 코딩하세요!