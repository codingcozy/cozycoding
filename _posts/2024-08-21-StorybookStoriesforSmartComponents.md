---
title: "스마트 컴포넌트를 위한 Storybook 스토리북"
description: ""
coverImage: "/assets/img/2024-08-21-StorybookStoriesforSmartComponents_0.png"
date: 2024-08-21 18:53
ogImage: 
  url: /assets/img/2024-08-21-StorybookStoriesforSmartComponents_0.png
tag: Tech
originalTitle: "Storybook Stories for Smart Components"
link: "https://dev.to/bymarsel/storybook-stories-for-smart-components-35g5"
isUpdated: false
---


알림: 본 문서는 Storybook을 잘 아는 고급 사용자를 대상으로 합니다. 복잡하고 기능이 풍부한 컴포넌트에 대한 Storybook 스토리를 어떻게 만들어야 할지 설명하겠습니다.

## 동기

컴포넌트에 대한 이상적인 Storybook 스토리는 "덤" 컴포넌트를 중심으로 구축된 경우입니다. 여기서는 Storybook 인터페이스를 통해 프롭스(속성)를 제어할 수 있습니다. 이 접근 방식은 컴포넌트 라이브러리에 잘 작동하지만, 애플리케이션 상태와 통합된 컴포넌트에 대한 Storybook 스토리를 만들어야 한다면 어떻게 해야 할까요? 라우터에서 데이터를 가져오며, 백엔드 요청을 만들며, 여러 언어를 지원하는 컴포넌트에 대한 Storybook 스토리를 만들어야 한다면 처음에는 불가능해 보일 수 있지만, 실제로는 매우 가능합니다.

## 데코레이터 및 애드온 메커니즘

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

이제 들어가기 전에, Storybook 기능을 확장하는 데 도움이 되는 두 가지 Storybook 메커니즘을 살펴보겠습니다.

데코레이터
데코레이터는 간단한 함수로, Storybook에 표시하려는 컴포넌트인 Story를 입력으로 받아 해당 Story의 수정된 버전을 반환합니다. 데코레이터 안에서는 Story에 필요한 수정 사항을 가하고 이러한 변경 사항의 결과를 반환해야 합니다. 부작용을 실행해야 할 경우, 데코레이터는 전달된 원본 Story를 그대로 반환할 수 있습니다.

아래의 데코레이터를 자세히 살펴봅시다:

```js
// decorators.js
import React from 'react';

export const withBackground = (Story) => (
  <div style={{ padding: '20px', backgroundColor: '#f0f0f0' }}>
    <Story />
  </div>
);
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

여기서는 이야기를 div로 감싸줄 수 있습니다. padding: 20px 및 backgroundColor: #f0f0f0으로 설정하면 Storybook에서 이 decorator로 감싼 구성요소의 표시에 영향을 줍니다.

이제 어떻게 이를 이야기에 적용할 수 있는지 살펴봅시다:

```js
import React from 'react';
import { withBackground } from './decorators';
import { Button } from './Button';

export default {
  title: 'Example/Button',
  component: Button,
  decorators: [withBackground]
};
```

버튼 구성요소에이 decorator를 적용하려면 이전에 정의한 withBackground decorator를 해당 이야기 설명의 decorators 속성에 전달하면 됩니다.

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

그 결과로, Storybook에서 우리의 Button을 렌더링할 때 컴포넌트에는 padding과 배경이 있게 됩니다.

이전에 "특히 우리 컴포넌트를 위한 것"이라고 언급했습니다. 사실 데코레이터는 개인적이거나 전역적일 수 있습니다. 즉, Storybook의 모든 이야기에 데코레이터를 적용할 수 있습니다.

이를 어떻게 할 수 있는지 살펴봅시다:

```js
//.storybook/preview.js
import React from 'react';
import { withBackground } from './decorators';

const previewConfig = {
  //...
  decorators: [withBackground],
  //...
};

export default previewConfig;
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


요렇게 해 보세요: `.storybook/preview.js` 파일을 열고 Storybook 구성에서 데코레이터 속성에 withBackground 데코레이터 함수를 추가하십시오. 그런 다음 모든 컴포넌트가 자동으로 이 데코레이터로 둘러싸여 패딩 및 배경이 추가됩니다.

애드온
Storybook의 애드온은 컴포넌트와 함께 작업하는 데 추가 기능과 기능을 추가하는 확장입니다. 이러한 애드온은 컴포넌트 상호 작용, 사용자 정의 및 시각화를 위한 도구를 제공하여 개발 및 테스트 과정을 강화합니다.
이 기사에서는 스마트 컴포넌트를 Storybook에 표시하는 데 도움이 되는 주요 애드온과 구성 방법을 살펴볼 것입니다. 모든 애드온을 구성하는 일반적인 방법은 없으므로 특정 애드온 예제를 사용하여 이를 설명하겠습니다.

## 애플리케이션 스토어와 스토리 통합: Redux를 사용한 예시

Redux를 예시로 사용하여 스토리 주변의 상태 관리를 설정하는 방법을 살펴보겠습니다. 데코레이터 메커니즘을 통해 이를 구현할 수 있습니다. 이를 한 단계씩 따라가 보겠습니다.

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

### 단계 1: 간단한 리듀서 정의

```js
const reducer = (state = {}, action) => {
  switch (action.type) {
    case 'INITIALIZE_STATE':
      return { ...state, ...action.payload };
    default:
      return state;
  }
}
```

이것은 간단합니다: 타입이 INITIALIZE_STATE인 액션을 처리하고 해당 페이로드를 현재 상태와 병합합니다.

### 단계 2: 스토리를 Redux 컨텍스트로 감싸는 데코레이터 생성

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

그 다음으로, 우리의 이야기를 컨텍스트로 래핑하고 컴포넌트의 상태를 초기화할 데코레이터를 만들어봅시다:

```js
import React from 'react';
import { Provider } from 'react-redux';
import { createStore } from 'redux';

export const withReduxState = (initialState) => (Story) => {
  const store = createStore(reducer);

  useEffect(() => {
    store.dispatch({ type: 'INITIALIZE_STATE', payload: initialState });
  }, [initialState]);

  return (
    <Provider store={store}>
      <Story />
    </Provider>
  );
};
```

여기에서 발생하는 일들을 살펴보겠습니다:

- 우리는 앞서 정의한 리듀서를 사용하여 Redux 스토어를 만듭니다.
- 그런 다음 store.dispatch와 INITIALIZE_STATE 액션을 사용하여 초기 데이터로 스토어를 초기화합니다.
- 마지막으로 우리는 Redux Provider로 컴포넌트(Story)를 래핑하여 스토어를 프롭으로 전달하여 컴포넌트가 Redux 상태에 액세스할 수 있도록 합니다.

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

스텝 3: 스토리 컴포넌트에 데코레이터 적용하기

이제 컴포넌트에 이 데코레이터를 적용하는 방법을 살펴보겠습니다.

```js
// Button.stories.js
import React from 'react';
import { withReduxState } from './decorators';
import { Button } from './Button';

export default {
  title: 'Example/Button',
  component: Button,
  decorators: [withReduxState(initialState)]
};
```

이 예제는 배경 데코레이터와 비슷하지만 한 가지 주요 차이가 있습니다. withReduxState는 초기 상태(initialState)를 가진 데코레이터를 반환하는 함수입니다. 이 접근 방식은 특히 동일한 컴포넌트에 대해 다른 상태에서 여러 스토리를 만들고 싶을 때 유용합니다.

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

이 예제에서는 데코레이터를 사용하여 Redux 상태 관리자를 Storybook에 통합하는 방법을 시연했습니다. 비슷한 접근 방식을 다른 상태 관리자와 성공적으로 결합할 수 있다고 확신합니다. 이를 통해 여러 시나리오에서 컴포넌트의 상태를 쉽게 관리할 수 있습니다.

## React Router를 Story에 통합하기

이제 브라우저 주소 표시줄의 현재 URL에 따라 컴포넌트의 동작이 달라지는 상황을 고려해봅시다. 예를 들어, 컴포넌트는 /path1에서 빨간색으로 표시되고 /path2에서는 초록색으로 표시되어야 합니다. 이러한 동작을 Storybook에서 어떻게 구현할 수 있을까요? storybook-addon-remix-react-router 애드온이 이를 도와줄 것입니다.

프로젝트에 플러그인 설정하기
먼저, Storybook 구성에서 플러그인을 활성화해봅시다.

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
// .storybook/main.ts
export default {
   //...
  addons: ['storybook-addon-remix-react-router'],
  //...
}
```

이렇게 하려면 `.storybook/main.ts` 파일의 addons 속성에 애드온의 이름을 추가하십시오.

스토리에서 플러그인 구성하기

이제 UserProfiles.stories.js 파일로 넘어가서 컴포넌트에 대한 가상 라우팅을 설정해 봅시다:

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
import { withRouter, reactRouterParameters } from 'storybook-addon-remix-react-router';

export default {
  title: '사용자 프로필',
  component: UserProfile,
  decorators: [withRouter],
  parameters: {
    reactRouter: reactRouterParameters({
      location: {
        pathParams: { userId: '42' },
      },
      routing: { path: '/users/:userId' },
    }),
  },
};
```

우리의 컴포넌트 주변에 라우터 컨텍스트를 만들기 위해 컴포넌트를 내보내고 withRouter 데코레이터로 래핑해야 합니다. 그런 다음 스토리의 구성에서 라우터 설정을 parameters 속성에 전달해야 합니다. 

parameters는 애드온에 전달되는 속성이 포함된 객체이며, 각 애드온은 설정에 액세스하기 위한 고유한 파라미터 키를 갖습니다. 우리의 경우에는 parameters.reactRouter입니다.

reactRouter 설정을 더 자세히 살펴보겠습니다:

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
  //...
  parameters: {
    reactRouter: reactRouterParameters({
      location: {
        pathParams: { userId: '42' },
      },
      routing: { path: '/users/:userId' },
    }),
  },
  //...
```

reactRouterParameters('...')을 사용하여 구성을 만들고 다음과 같은 매개변수를 추가했습니다:

- location.pathParams.userId — 이는 구성 요소가 위치한 현재 위치를 설명합니다. 구성 요소가 라우터에 접근하고 userId 매개변수를 가져오려고 할 때 값 42를 받게 됩니다.
- routing.path — 이는 구성 요소가 위치한 가상 경로를 설명합니다. 브라우저에서는 한 위치가 표시되지만 구성 요소는 마치 /users/:userId 경로에 있는 것처럼 행동합니다. 이를 통해 시각적 표현이 라우팅에 따라 달라지는 구성 요소를 위해 가상 경로를 추가할 수 있습니다.

전역 라우터 활성화
특정 구성 요소 및 개별 스토리에 대한 가상 라우터 설정 방법을 알아보았습니다. 그러나 모든 스토리에 대해 전역 가상 라우터를 설정하고 싶다면 어떻게 해야 할까요? 이 또한 가능합니다 — .storybook/preview.js 파일로 이동하여 비슷하게 구성하시면 됩니다.


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
// .storybook/preview.js
export default {
  decorators: [withRouter],
  parameters: {
    reactRouter: reactRouterParameters({ ... }),
  }
}
```

이 원리는 이야기 설정과 동일합니다: reactRouter 매개변수를 정의하고 라우터 구성을 reactRouterParameters(' /* ... */ ')에 전달해야 합니다.

주의 깊게 읽는 독자라면 새 플러그인을 추가하는 대신 이야기를 라우터로 래핑하는 데코레이터를 간단히 구현할 수 있지 않을까 라고 물어볼 수 있습니다. 실제로 그렇게 할 수 있지만, 플러그인을 사용하는 두 가지 이점이 있습니다.

![이미지](/assets/img/2024-08-21-StorybookStoriesforSmartComponents_0.png)


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

스토리북 애드온 리믹스 리액트 라우터 플러그인은 스토리북 인터페이스에 탭을 추가하여 컴포넌트의 현재 경로와 네비게이션 이벤트를 확인할 수 있습니다.

이 섹션에서는 가상 라우팅을 사용하여 컴포넌트의 이야기를 구성하는 방법에 대해 자세히 다루었습니다. 이를 통해 라우트에 의존하는 컴포넌트를 쉽게 테스트할 수 있습니다. 이 애드온의 기능과 설정에 대해 더 알고 싶다면 아래 링크에서 제공되는 상세 문서를 확인하는 것을 권장합니다: storybook-addon-remix-react-router.

## 요청 가로채기

이 섹션에서는 컴포넌트 이야기에서 요청을 가로채고 원하는 데이터로 응답하는 방법을 살펴보겠습니다. 이를 위해 msw-storybook-addon을 사용할 것입니다.

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

설정
처음으로 해야 할 일은 다음 몤련 명령어를 실행하는 것입니다:

```js
npx msw init public/
```

이게 왜 필요한 걸까요? 이 명령은 `public` 폴더를 생성하고 거기에 서비스 워커를 배치하기 때문입니다. 이 서비스 워커는 요청을 가로챌 때 사용됩니다.
서비스 워커를 로딩한 후에는 Storybook에서 전역으로 실행하도록 구성해야 합니다. 이를 위해 `.storybook/preview.js` 파일을 열고 다음 단계를 따르세요:

```js
import { initialize, mswLoader } from 'msw-storybook-addon'

initialize()

const preview = {
    //...
  loaders: [mswLoader],
}

export default preview
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

초기화 함수는 서비스 워커를 설정하고 시작하며, mswLoader는 서비스 워커가 준비되기를 기다리는 로더로서 요청 핸들러를 적용한 후에 대화할 것입니다.

이제 애드온을 구성했으니, 이를 스토리 안에서 활성화해 보겠습니다:

```js
import { http, HttpResponse } from 'msw'
import { Profile } from './profile'

export const SuccessBehavior = {
    component: Profile,
    parameters: {
        msw: {
            handlers: [
                http.get('/user', () => {
                    return HttpResponse.json({
                        firstName: 'Neil',
                        lastName: 'Maverick',
                    })
                }),
            ],
        },
    },
}
```

이전에 언급했듯이, 각 애드온에는 해당 설정이 전달되는 매개변수가 있습니다. msw-storybook-addon의 경우 이 매개변수는 parameters.msw입니다. 이 매개변수 안에는 요청을 가로채는 요청 핸들러인 handlers를 지정해야 합니다.
우리의 예제에서는 /user URL로의 GET 요청을 가로채고 JSON 형식으로 응답을 반환합니다. 비슷하게, 다른 유형의 요청을 처리하거나 POST를 다뤄서 HTTP 상태만 반환하거나 몇 초간의 응답 지연을 시뮬레이트할 수도 있습니다.

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

이 섹션에서는 msw-storybook-addon을 사용하여 스토리북에서 스토리에 대한 요청 가로채기를 설정하는 방법을 상세히 살펴보았습니다. 이 애드온을 사용하면 API와의 컴포넌트 상호작용에 대한 현실적인 시나리오를 만들 수 있어 테스트 및 개발 프로세스를 더 효율적으로 만들 수 있습니다. 다양한 유형의 HTTP 요청에 대한 요청 핸들러를 구성하고, 서버 응답을 시뮬레이션하며, 응답 지연도 모델링할 수 있습니다. 이는 다양한 조건 하에서 컴포넌트 동작을 테스트하는 데 특히 유용합니다. 이 애드온에 대한 자세한 정보 및 전체 문서는 다음을 참조할 수 있습니다: msw-storybook-addon.

## 스토리북 인터페이스에서 스토리 언어 전환

주요 애드온을 이미 살펴보았으므로 이제 스토리북 인터페이스에서 컴포넌트 언어를 직접 전환해야 하는 시나리오를 살펴보겠습니다. 이 방법은 대부분 클라이언트 측 번역 솔루션에 탁월합니다.

첫 번째로 해야 할 일은 스토리북 인터페이스에서 사용할 새로운 언어 스위처를 정의하는 것입니다:

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
export const preview = {
  // ...
  globalTypes: {
    locale: {
      name: "Locale",
      description: "언어",
      toolbar: {
        icon: "globe",
        items: [
          { value: "en", title: "영어" },
          { value: "fr", title: "프랑스어" },
        ],
      },
    },
  },
  //...
};
```

여기에서 무슨 일이 일어나고 있는지 알아보겠습니다:

- 먼저, globalTypes 섹션에서 새로운 전역 변수 locale을 정의합니다.
- 둘째로, 해당 변수의 디스플레이 설정을 구성합니다.

<img src="/assets/img/2024-08-21-StorybookStoriesforSmartComponents_1.png" />


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

이제 Storybook 인터페이스에서 스위처를 설정했으니, 컴포넌트가 언어 변경에 대응하는 방법을 알아봅시다. 이를 위해 데코레이터 메커니즘을 사용할 것입니다:

```js
const langDecorator = (Story, context) => {
  const locale = context.globals.locale;

  useEffect(() => {
    i18n.changeLanguage(locale);
  }, [locale]);

  return <Story />;
};
```

데코레이터 함수의 두 번째 매개변수인 context에 주목해주세요. 이를 통해 현재 선택된 언어를 저장하는 전역 변수 globals.locale에 접근할 수 있습니다. 이 값을 사용하여 예시처럼 i18n이나 다른 번역 라이브러리에 전달하여 인터페이스 언어를 업데이트합니다.

이 섹션에서는 Storybook 인터페이스에 다국어 컴포넌트를 지원하는 기능을 추가하여 확장하는 방법을 상세히 살펴보았습니다. 이 접근 방식은 Storybook 인터페이스에서 직접 언어를 전환하여 여러 언어를 테스트하고 표시하는 것을 쉽게 만듭니다. 이런 설정은 프로젝트가 여러 언어를 지원하는 경우 개발 및 테스트 프로세스를 더 편리하고 유연하게 만들어줍니다.

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

## 보너스: 스토리북에서의 모듈 연합 지원

스토리북에서 마이크로 프론트엔드를 렌더링해야 하는 상황을 상상하기 어렵지만, 그런 지원을 추가하는 애드온에 대해 알려드리겠습니다. 이는 애드온이 얼마나 강력하고 스토리북에서 가장 일반적이지 않은 작업조차 가능하게 하는 좋은 예입니다.
모듈 연합 지원을 활성화하는 것은 매우 간단합니다. 이를 위해서 @module-federation/storybook-addon 패키지를 설치해야 합니다. 구성을 시작해보겠습니다:

```js
// .storybook/main.ts
export default {
  //...
  addons: [
    {
      name: "@module-federation/storybook-addon",
      options: {
        moduleFederationConfig: moduleFederationConfig(),
      },
    },
  ],
  //...
};
```

모듈 연합 지원을 위한 애드온을 연결하는 것은 정말 간단합니다. moduleFederationConfig는 웹팩 구성에서 사용하는 구성과 동일해야 합니다. 모듈 연합에 대해 더 자세히 알아보려면 이곳에서 확인할 수 있습니다: 웹팩 모듈 연합.
문서와 플러그인 자체는 다음 주소에서 찾을 수 있습니다: @module-federation/storybook-addon.

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

## 결론

이 글에서는 Storybook에서 응용 프로그램 상태, 라우팅 및 API와 통합된 복잡한 컴포넌트와 함께 작동할 수 있는 고급 방법을 어떻게 사용하는지를 보여주었습니다. 데코레이터와 애드온이 어떻게 이러한 컴포넌트를 테스트하기 위해 Storybook을 적응시키는 데 도움이 되는지 설명했습니다. Redux 및 React Router와의 통합 예제뿐만 아니라 요청 모킹과 언어 전환에 대해 다루었습니다. 게다가, 모듈 연합을 통해 마이크로 프론트엔드를 지원할 수 있는 가능성에 대해서도 언급했습니다. 마지막으로, Storybook은 다양한 사용 사례에 맞게 구성할 수 있는 강력한 도구로, 개발 및 테스트 프로세스를 보다 유연하고 효율적으로 만들어줄 수 있음을 입증했습니다.