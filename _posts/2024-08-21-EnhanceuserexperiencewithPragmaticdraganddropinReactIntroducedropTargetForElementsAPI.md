---
title: "React에서 실용적인 드래그 앤 드롭을 통해 사용자 경험 향상하기 dropTargetForElements API 소개"
description: ""
coverImage: "/assets/img/2024-08-21-EnhanceuserexperiencewithPragmaticdraganddropinReactIntroducedropTargetForElementsAPI_0.png"
date: 2024-08-21 17:25
ogImage: 
  url: /assets/img/2024-08-21-EnhanceuserexperiencewithPragmaticdraganddropinReactIntroducedropTargetForElementsAPI_0.png
tag: Tech
originalTitle: "Enhance user experience with Pragmatic drag and drop in React Introduce dropTargetForElements API"
link: "https://medium.com/itnext/enhance-user-experience-with-pragmatic-drag-and-drop-in-react-introduce-droptargetforelements-api-8316afc397a7"
isUpdated: false
---


저희 이전 기사에서는 Pragmatic Drag and Drop 라이브러리를 사용하여 요소를 드래그할 수 있도록 만드는 방법을 살펴보았습니다. 이제는 작업의 두 번째 단계로 이동해보죠: 요소를 드롭할 수 있는 대상으로 만드는 것입니다.

Pragmatic Drag and Drop은 요소를 드롭할 수 있는 대상으로 지정하기 위한 유연하고 강력한 API를 제공합니다. 이를 통해 드롭된 항목을 수신하고 특정 작업을 실행할 수 있습니다. 또한, 요소는 소스로서와 대상으로서 모두 작동할 수 있어 동적 상호작용을 가능하게 합니다.

![Pragmatic Drag and Drop](/assets/img/2024-08-21-EnhanceuserexperiencewithPragmaticdraganddropinReactIntroducedropTargetForElementsAPI_0.png)

# 요소를 드롭할 수 있게 만들기

<div class="content-ad"></div>

요소를 드롭 가능한 대상으로 만들려면 드래그할 수 있도록 만드는 것과 비슷한 접근 방식을 따라야 합니다. 먼저 useRef를 사용하여 기본 HTML 요소에 대한 참조를 가져와야 하고, 그런 다음 필요할 때 Pragmatic Drag and Drop에서 호출할 이벤트 핸들러를 등록할 수 있습니다.

다음은 우리의 카드 컴포넌트 내에서 이를 설정하는 방법입니다:

```js
const dropConfig = {
  element,
  onDrag() {
    console.log('drag...');
  },
  onDrop() {
    console.log('drop');
  }
};

dropTargetForElements(dropConfig);
```

이 구성에서:

<div class="content-ad"></div>

- onDrag은 끌어 올린 요소가 대상 위로 이동할 때 반복적으로 호출됩니다.
- onDrop은 끌어 올린 요소가 대상 영역 내에서 놓여졌을 때에만 트리거됩니다.

한 카드를 다른 카드 위로 끌어 올릴 때, 콘솔에 이러한 로그가 표시됩니다:

![이미지](/assets/img/2024-08-21-EnhanceuserexperiencewithPragmaticdraganddropinReactIntroducedropTargetForElementsAPI_1.png)

드래그된 요소가 대상 위에 있을 때 드래그... 로그가 여러 번 나타나며, 드롭은 마우스를 대상 영역 내에서 놓을 때에만 기록됩니다.

<div class="content-ad"></div>

# 소스와 타겟 간의 관계 이해하기

소스(드래그되는 요소)와 타겟(드롭되는 요소를 받는 요소) 간의 관계를 명확히 하기 위해 다음 다이어그램을 참고해 보세요:

![다이어그램](/assets/img/2024-08-21-EnhanceuserexperiencewithPragmaticdraganddropinReactIntroducedropTargetForElementsAPI_2.png)

이 다이어그램에서:

<div class="content-ad"></div>

- 먼저, React 컴포넌트는 DOM 요소로 렌더링됩니다.
- 드래그 기능이 적용되면 미리 보기가 생성되어 드래깅 중에도 요소를 볼 수 있습니다.
- 데이터를 노드에 첨부하여 소스를 나타낼 수 있습니다.
- 요소가 dropTargetForElement으로 정의된 다른 컴포넌트 위로 드래그되면 해당 컴포넌트가 대상이 되어 onDrag 및 onDrop과 같은 이벤트를 수신합니다.

노드가 소스와 대상으로 모두 사용될 수 있다는 점이 중요합니다. 이는 동일한 요소가 드래그를 시작하고 드롭을받을 수 있는 복잡한 드래그 앤 드롭 시나리오에서 특히 강력합니다. JavaScript 클로저는 여기서 중요한 역할을 합니다. 드래그 작업이 진행되는 동안 이벤트 콜백에서 정확히 액세스되도록 노드에 바인딩된 데이터가 올바르게 액세스되도록 보장합니다.

# 소스와 대상 간 데이터 전달

이제 컴포넌트를 드롭할 수 있게 만드는 방법을 이해했으므로, 다음 단계는 소스와 대상을 연결하여 통신할 수 있도록 하는 것입니다. 실용적인 드래그 앤 드롭을 사용하면 드래그 앤 드롭 작업 중에 소스와 대상 간에 데이터를 공유할 수 있습니다.

<div class="content-ad"></div>

아이디어는 간단해요:

- 드래그가 시작될 때 소스 노드에 데이터를 첨부합니다.
- 소스가 이동된 위치 위로 드래그되면 대상 노드에 데이터를 첨부합니다.
- 드롭이 발생할 때 소스와 대상으로부터 데이터에 액션을 수행합니다.

이제 우리 보드 애플리케이션으로 돌아가서 한 카드를 다른 위치로 이동하는 방법을 살펴보죠:

```js
useEffect(() => {
  const element = ref.current;

  const dragConfig = {
    element,
    getInitialData() {
      return card;
    },
  };

  const dropConfig = {
    element,
    getData() {
      return card;
    },
    onDrop({ self, source }) {
      console.log(`${source.data.columnId}에서 ${source.data.id} 카드를 ${self.data.columnId}으로 이동`);
    }
  };

  return combine(draggable(dragConfig), dropTargetForElements(dropConfig));
}, [card]);
```

<div class="content-ad"></div>

<img src="/assets/img/2024-08-21-EnhanceuserexperiencewithPragmaticdraganddropinReactIntroducedropTargetForElementsAPI_3.png" />

여기 예시:

- dragConfig의 getInitialData는 원본을 나타내는 카드 데이터를 반환합니다.
- dropConfig의 getData는 대상 데이터를 반환합니다.
- onDrop 함수는 대상(self)과 원본에 모두 접근하여 카드의 이동 작업을 출력합니다.

# 만약 비디오 형식을 선호한다면

<div class="content-ad"></div>

비디오 형식을 선호하고 자신의 속도에 맞춰 학습하는 분들을 위해 YouTube 비디오도 만들었습니다.

# 결론

이제 드롭 대상과 소스와 대상 간에 데이터를 전달하는 방법에 대해 이해했으므로 애플리케이션에서 전체 드래그 앤 드롭 작업을 구현할 수 있는 튼튼한 기반을 갖게 되었습니다. 요소를 동적으로 이동하고 그 이동에 기반한 작업을 트리거할 수 있는 능력은 상호 작용 인터페이스에 대한 다양한 가능성을 열어줍니다.

다음 기사에서는 중첩된 드롭 대상을 처리하고 드롭 효과를 사용자 정의하여 더 정교한 사용자 경험을 만들기 위한 고급 주제 등을 더 살펴볼 예정입니다. 기대해주세요!

<div class="content-ad"></div>

만약 읽는 것을 좋아한다면, 제 메일링 목록에 가입해주세요. 매주 블로그, 책, 강의, 그리고 비디오를 통해 Clean Code 및 Refactoring 기술을 공유하고 있습니다.