---
title: "기술면접에서 Redux 관련 질문과 답변 17가지"
description: ""
coverImage: "/assets/img/2024-09-10-Top17MostCrucialReduxInterviewQuestionsthatIwasaskedthemost_0.png"
date: 2024-09-10 18:33
ogImage: 
  url: /assets/img/2024-09-10-Top17MostCrucialReduxInterviewQuestionsthatIwasaskedthemost_0.png
tag: Tech
originalTitle: "Top 17 Most Crucial Redux Interview Questions that I was asked the most"
link: "https://medium.com/@code-geass/top-17-most-crucial-redux-interview-questions-that-i-was-asked-the-most-bbe29989ba37"
isUpdated: true
updatedAt: 1726041483867
---


<img src="/assets/img/2024-09-10-Top17MostCrucialReduxInterviewQuestionsthatIwasaskedthemost_0.png" />

리덕스는 리액트와 함께 사용되는 인기 있는 상태 관리 라이브러리로, 확장 가능하고 유지보수가 쉬운 프론트엔드 애플리케이션을 개발하고자 하는 개발자들에게 필수적입니다. 그러나 리덕스 인터뷰 질문은 때로는 까다로울 수 있습니다. 특히 엣지 케이스, 성능 최적화 및 고급 패턴이 관련된 경우에는 더 그렇습니다. 아래에는 17가지 어려운 리덕스 인터뷰 질문이 나열되어 있습니다. 논리적 엣지 케이스와 코드 예제를 통해 더 잘 이해하도록 설명되어 있습니다.

참고: GPT-4가 이 기사를 작성하는 데 도움을 주었으며, 개인 실험 결과와 그 결론이 통합되었습니다.

# 1. 리덕스는 무엇이며 어떻게 작동합니까?

<div class="content-ad"></div>

해석: Redux는 JavaScript 앱을 위한 예측 가능한 상태 컨테이너입니다. 앱의 전체 상태를 단일 저장소에서 관리함으로써 작동합니다. 액션은 업데이트를 트리거하기 위해 통지되며, 리듀서는 이러한 액션에 대응하여 상태가 어떻게 변경되어야 하는지 정의합니다.

```js
const initialState = { count: 0 };
function counterReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'DECREMENT':
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
}
```

# 2. Redux에서 비동기 액션을 처리할 때의 엣지 케이스는 무엇인가요?

해석: Redux의 가장 어려운 부분 중 하나는 비동기 액션(예: API 호출)을 처리하는 것입니다. Redux Thunk는 종종 비동기 로직에 사용되지만, 여러 비동기 액션이 동시에 실행되어 경합 상태가 발생할 수 있는 엣지 케이스가 발생할 수 있습니다.

<div class="content-ad"></div>

에지 케이스: 두 개의 API 호출이 동시에 발생하여 동일한 상태를 변경하는 경우, 후자의 조치가 이전 조치의 결과를 덮어쓸 수 있습니다.

해결책:

```js
const fetchUserData = () => async (dispatch, getState) => {
  const response = await fetch('/user');
  const data = await response.json();
  
  // 상태가 여전히 가져온 데이터를 필요로 하는지 확인
  if (!getState().user.loaded) {
    dispatch({ type: 'SET_USER', payload: data });
  }
};
```

### 3. Redux에서 깊게 중첩된 상태를 다루는 방법은 무엇인가요?

<div class="content-ad"></div>

Edge Case: 리덕스에서 깊게 중첩된 상태를 업데이트하는 것은 복잡하고 유지보수가 어려운 코드를 야기할 수 있습니다.

해결책: immer를 사용하여 간편한 불변성 처리를 수행합니다.

```js
import produce from 'immer';
const initialState = { user: { profile: { address: { city: '' } } } };
const userReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'UPDATE_CITY':
      return produce(state, draft => {
        draft.user.profile.address.city = action.payload;
      });
    default:
      return state;
  }
};
```

## 4. 대규모 애플리케이션에서 Redux를 성능 최적화하는 방법은 무엇인가요?

<div class="content-ad"></div>

Edge Case: 대규모 상태 객체를 다룰 때, 컴포넌트로 전달되는 전체 상태 트리로 인해 다시 렌더링 속도가 느려질 수 있습니다. connect를 사용하여 상태의 일부에 선택적으로 구독하는 방식으로 이를 해결할 수 있습니다.

해결책: reselect를 사용하여 복잡한 상태 셀렉터의 결과를 메모이제이션하세요.

```js
import { createSelector } from 'reselect';
```

```js
const selectItems = state => state.items;
const selectFilteredItems = createSelector(
  [selectItems],
  items => items.filter(item => item.visible)
);
```

<div class="content-ad"></div>

# 5. Redux에서 데이터를 캐싱할 때 중복 요청을 어떻게 처리하나요?

특이사항: 현재 상태를 확인하지 않고 동일한 액션이 여러 번 디스패치되면 실수로 중복 API 요청을 보낼 수 있습니다.

해결책: 중복 API 호출을 피하기 위해 캐싱 메커니즘을 구현하세요.

```js
const fetchIfNeeded = () => (dispatch, getState) => {
  const { isLoaded } = getState().data;
  if (!isLoaded) {
    dispatch(fetchData());
  }
};
```

<div class="content-ad"></div>

# 6. Redux 액션에서 오류를 처리하는 경우의 엣지 케이스는 무엇인가요?

엣지 케이스: API 요청에서 발생한 오류가 제대로 처리되지 않으면 UI에서 무한 루프나 일관성 없는 상태로 이어질 수 있습니다.

해결책: 상태에 오류를 저장하는 메커니즘을 도입하고, 성공적인 요청 후에는 초기화되도록 보장합니다.

```js
const apiReducer = (state = {}, action) => {
  switch (action.type) {
    case 'FETCH_SUCCESS':
      return { ...state, data: action.payload, error: null };
    case 'FETCH_ERROR':
      return { ...state, error: action.error };
    default:
      return state;
  }
};
```

<div class="content-ad"></div>

# 7. 동일한 state 조각을 수정하는 여러 개의 리듀서를 어떻게 처리하나요?

특별한 경우: 때때로 두 개의 리듀서가 동일한 state 조각을 수정하려고 시도하여 충돌이 발생할 수 있습니다.

해결책: 리듀서가 특정 state 조각만을 처리하도록 하여 관심사를 분리합니다.

```javascript
const rootReducer = combineReducers({
  auth: authReducer,
  profile: profileReducer,
});
```

<div class="content-ad"></div>

# 8. Redux에서 미들웨어가 작동하는 방식을 설명하세요. 어떤 논리적 예외 상황이 발생하나요?

설명: Redux의 미들웨어는 액션이 리듀서에 도달하기 전에 가로채서 로깅, API 호출 또는 비동기 논리와 같은 부작용을 처리할 수 있는 장소를 제공합니다.

예외 상황: 미들웨어가 신중한 제어 없이 액션을 디스패치하는 경우, 무한 루프나 예상치 못한 동작으로 이어질 수 있습니다.

해결책: 적절한 디스패치 체인과 액션 처리를 보장하세요.

<div class="content-ad"></div>

```js
const loggingMiddleware = store => next => action => {
  console.log('Dispatching action:', action);
  next(action);
};
```

# 9. 대형 Redux 애플리케이션을 어떻게 구조화합니까?

특이사항: 대형 애플리케이션에서 적절한 모듈화 없이 Redux 상태를 관리하는 것은 혼란스러운 코드베이스로 이어질 수 있습니다.

해결책: 피쳐 기반 폴더 구조를 사용하고 리듀서, 액션 및 셀렉터를 기능별로 구성합니다.

<div class="content-ad"></div>

```js
src/
  features/
    user/
      userActions.js
      userReducer.js
      userSelectors.js
```

# 10. 리듀서가 유효한 상태를 반환하지 않는 경우에는 어떻게 될까요?

특이 케이스: 리듀서가 undefined나 올바르지 않은 유형의 상태를 반환하면 앱이 오작동할 수 있습니다.

해결책: 항상 기본 상태를 제공하고 액션 유형을 유효성 검사하세요.

<div class="content-ad"></div>

```js
const counterReducer = (state = { count: 0 }, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    default:
      return state;
  }
};
```

# 11. Redux에서 낙관적 업데이트를 다루는 방법은 무엇인가요?

특별한 경우: 낙관적 업데이트는 업데이트가 실패하는 경우 불일치가 발생할 수 있습니다.

해결책: 실패 시 상태를 되돌리는 롤백 액션을 구현합니다.

<div class="content-ad"></div>

```js
const updateUser = user => dispatch => {
  dispatch({ type: 'UPDATE_USER_OPTIMISTIC', payload: user });
  try {
    // API 호출 시뮬레이션
    dispatch({ type: 'UPDATE_USER_SUCCESS', payload: user });
  } catch (error) {
    dispatch({ type: 'UPDATE_USER_FAILURE' });
  }
};
```

# 12. Redux에서 상태 수화를 다루는 방법은 무엇인가요?

특이 케이스: 로컬 스토리지에서 상태를 수화할 때, 일관되지 않거나 오래된 데이터를 병합하는 것이 까다로울 수 있습니다.

해결책: 수화할 상태 조각들의 화이트리스트를 사용하고 주의 깊게 병합하세요.

<div class="content-ad"></div>

```js
const persistedState = localStorage.getItem('reduxState')
  ? JSON.parse(localStorage.getItem('reduxState'))
  : {};
const store = createStore(rootReducer, persistedState);
```

# 13. 리덕스에서 경합 조건을 방지하는 방법은 무엇인가요?

특수 상황: 여러 비동기 액션이 동시에 동일한 상태를 업데이트하면 경합 조건이 발생할 수 있습니다.

해결책: 동시 변경을 하기 전에 플래그나 체크를 구현하여 경합 상황을 방지하세요.

<div class="content-ad"></div>

```js
상태를 가져와서 확인하세요.
만약 데이터가 로딩 중이 아니라면,
FETCH_DATA_START action을 디스패치합니다.
'/api/data'로 요청을 보내고,
응답을 JSON으로 변환하여 FETCH_DATA_SUCCESS action을 디스패치합니다.
```

# 14. Redux 액션에서 순환 종속성을 어떻게 처리하나요?

특이 케이스: 액션이나 리듀서에서의 순환 종속성은 무한 반복을 일으킬 수 있습니다.

해결책: 코드를 주의 깊게 구성하여 순환 논리를 피하세요.

<div class="content-ad"></div>

# 15. 복잡한 앱에서 Redux 액션을 디버그하는 방법은 무엇인가요?

특이 케이스: 너무 많은 액션이 동시에 디스패치될 때 디버깅하기 어려울 수 있어요.

해결책: Redux DevTools나 로깅 미들웨어를 사용하여 가시성을 높이세요.

# 16. Redux에서 셀렉터의 역할은 무엇인가요?

<div class="content-ad"></div>

에지 케이스: 셀렉터 없이 컴포넌트를 사용하면 상태 모양과 강하게 결합될 수 있어서 상태를 리팩토링할 때 문제가 발생할 수 있습니다.

해결 방법: 셀렉터를 사용하여 상태 엑세스를 추상화하세요.

```js
const selectUser = state => state.user;
```

# 17. Redux를 사용할 때 흔히 발생하는 함정은 무엇인가요?

<div class="content-ad"></div>

에지 케이스: 상태 관리를 지나치게 복잡하게 만들거나 불변성을 처리하지 않거나 지역 컴포넌트 상태에 Redux에 지나치게 의존하는 것입니다.

해결 방법: 전역 또는 공유 상태에만 Redux를 사용하고 적절한 곳에 지역 상태를 사용하며 immer와 같은 라이브러리를 통해 불변성을 보장합니다.

# 결론

이 인터뷰 질문 및 에지 케이스는 대규모 응용 프로그램을 구축할 때 Redux에서 발생하는 복잡성을 강조합니다. 이러한 시나리오를 탐색하는 방법을 이해하고 효율적이고 확장 가능한 코드를 작성하는 능력은 Redux 인터뷰와 실제 프로젝트에서 성공하는 데 도움이 됩니다.