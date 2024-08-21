---
title: "React Query로 데이터 가져오기 useEffect 대신 더 나은 방법"
description: ""
coverImage: "/assets/img/2024-08-21-GoodbyeuseEffectHelloReactQueryAbetterwaytohandledatafetchinginReact_0.png"
date: 2024-08-21 17:24
ogImage: 
  url: /assets/img/2024-08-21-GoodbyeuseEffectHelloReactQueryAbetterwaytohandledatafetchinginReact_0.png
tag: Tech
originalTitle: "Goodbye useEffect Hello React Query A better way to handle data fetching in React."
link: "https://medium.com/@muhammad-lamine92/goodbye-useeffect-hello-react-query-a-better-way-to-handle-data-fetching-in-react-1c988596aa6f"
isUpdated: false
---


useEffect와 씨름하지 마시고 React Query를 활용하여 더 우아하고 효율적인 방법을 발견해 보세요.

많은 사람들처럼 React 앱을 개발하기 시작할 때 useEffect 훅을 사용하여 데이터를 가져오는 것을 권장했던 React 예전 문서를 참고했습니다.
데이터 가져오기는 어려운 일이지만... 2년 전 저의 초보 시절에는, 그냥 함수(axios나 fetch를 사용한)를 useEffect 안에 감싸는 것이라고 생각했습니다.

본 기사(제 첫 작품)에서는 useEffect 사용 대신 데이터를 가져오는 인기 있는 방식으로부터 이탈하고 React Query 또는 Tanstack Query와 같은 것을 포함하여 취하는 이유와 애플리케이션에 대한 좋은 핵심 데이터 가져오기 솔루션을 가지는 것이 가져다주는 혜택에 대해 알아보겠습니다. 그럼 더 이상 말이 필요 없으니, 시작해 봅시다.

## 1. useEffect의 문제점은 무엇인가요?

<div class="content-ad"></div>

useEffect은 React에서 부작용을 처리하는 데 강력하고 필수적인 후크이지만, 데이터 가져오기에 사용하면 여러 가지 단점이 있습니다.

- 상태 관리의 복잡성
useEffect를 사용하면 로딩, 성공 및 오류 상태를 수동으로 관리해야 합니다. 개발자들은 종종 관리하기 어렵고 버그가 발생하기 쉬운 장황하고 중복된 코드를 작성하게 됩니다. 그것은 그다지 편리하지 않죠.

![이미지](/assets/img/2024-08-21-GoodbyeuseEffectHelloReactQueryAbetterwaytohandledatafetchinginReact_0.png)

그리고 이것은 기본적인 검색에 대한 것 뿐입니다. 다시 시도, 페이지네이션 또는 기타 기능을 추가한다고 상상해보세요. 금방 정신이 혼미해질 것입니다.

<div class="content-ad"></div>

- 복잡한 정리 로직
useEffect에서 정리를 제대로 하지 않으면, 메모리 누수나 컴포넌트가 언마운트된 후에 상태를 업데이트하려는 등의 미묘한 버그가 발생할 수 있습니다. 이러한 문제들은 진단하고 고치기 어렵기 때문에, 귀찮은 디버깅 세션과 악몽을 야기할 수 있습니다.

![이미지](/assets/img/2024-08-21-GoodbyeuseEffectHelloReactQueryAbetterwaytohandledatafetchinginReact_1.png)

여기서는 컴포넌트가 언마운트된 경우 상태 업데이트를 방지하기 위해 isMounted 플래그를 도입했습니다. 이러한 정리 로직을 수동으로 관리하는 것은 지루하고 실수하기 쉽습니다.

- 기능 한정
useEffect에는 캐싱, 백그라운드 데이터 동기화, 자동 재시도와 같은 내장 기능이 없습니다. 이는 이러한 유용한 기능을 포기하거나 직접 구현해야 한다는 것을 의미합니다. 작업량을 더 늘리게 됩니다. React-query나 SWR과 같은 도구가 이를 처리하도록 설계되었습니다.
예: 인터넷이 1초 정도 끊기면, 자동으로 다시 시도하지 않고 앱이 포기하는 일이 생길 수 있습니다. 우리가 원하는 사용자 경험이 아닙니다.
- 개발자들은 게으릅니다 😅 (그리고 이것은 좋은 일입니다)
솔직하게 말해서, 개발자들은 누구든지 그들이 하는 일을 가장 쉽고 효율적으로 처리하는 방법을 찾는 것을 좋아합니다. 그게 나쁜 것은 아닙니다! 그래서 그들은 반드시 해야 할 일보다 더 많은 코드를 작성하길 원하지 않습니다. 부정적인 의미의 게으르지 않다는 것입니다. 효율적으로 하는 것입니다. 최고의 개발자(적어도 제 생각에는)는 불필요한 복잡성을 피하는 방법을 아는 사람들입니다. React에서 데이터 가져오기를 다룰 때, useEffect를 사용하면 반복적이고 지루한 작업이 필요하지 않다고 생각합니다.

<div class="content-ad"></div>

우리 작은 "데이터를 가져오고 싶은데 얼마나 힘들까?"라는 마음으로 시작한 useEffect 훅은 엣지 케이스와 상태 관리를 고려해야 할 때 순식간에 마치 스파게티 코드처럼 엉망이 되었어요. 그래서 여기서 얻을 수 있는 교훈은 무엇일까요?

결론

확장 가능하고 유지 보수가 쉬운 애플리케이션을 구축하기 위해, 보일러플레이트를 추상화하고 강력한 기능을 제공하는 더 나은 데이터 가져오기 솔루션이 필요합니다. 이것이 React Query가 나오는 곳입니다.

## 2. React Query로 전환하기

<div class="content-ad"></div>

<img src="/assets/img/2024-08-21-GoodbyeuseEffectHelloReactQueryAbetterwaytohandledatafetchinginReact_2.png" />

예를 들어, API와 작업할 때 로딩 상태와 데이터 상태와 같이 추적하려는 많은 것이 있습니다. 대부분의 전통적인 상태 관리 라이브러리는 클라이언트 상태에서 작동하는 데 훌륭하지만 비동기 또는 서버 상태에서 작동하는 데는 그렇게 좋지 않습니다.

React Query란?
React Query는 개발자와 사용자 경험을 향상시키기 위해 만들어진 비동기/서버 상태 관리를 위한 라이브러리입니다. 데이터 가져오기를 간소화하고 원격 데이터를 관리하고 UI와 동기화하는 간단한 방법을 제공합니다.

왜 사용해야 하는가?
작은 앱의 경우 useEffect를 사용할 수 있지만 앱이 커지거나 복잡한 데이터 가져오기 요구를 처리하기 시작하면 useEffect와 그 사촌 😅인 useState를 사용하는 것이 까다로워질 수 있습니다. 그리고 여러 까다로운 질문을 마주하게 될지도 모릅니다 🥵 

<div class="content-ad"></div>

- 요청을 취소해야 할까요?
- 경쟁 조건은 어떻게 처리할까요?
- 정확히 동일한 엔드포인트에서 데이터를 가져와야 하는 여러 구성 요소가 있을 경우 어떻게 해야 할까요?
- 데이터를 캐시해야 할까요? 얼마나 오래 캐시해야 할까요?
- 데이터가 오래되었는지 확인하는 방법은 무엇인가요?
- 등등

이런 질문들은 React에 특정한 것이 아니라 데이터를 가져오는 일반적인 문제입니다. 이러한 문제를 해결하고 더 많은 문제를 해결하려면 두 가지 옵션이 있습니다:

- 많은 코드를 쓰는 것을 통해 바퀴를 재발명하십시오 (그러니 귀찮은 개발자는 아닌 것 같네요 😅)
- 몇 년간 문제를 해결해 온 React Query와 같은 기존 라이브러리에 의존하십시오.

인생은 짧으니 React Query를 사용하세요… 😂

<div class="content-ad"></div>

아래는 사용하면 아마 맛있는 느낌을 줄 것으로 예상되는 몇 가지 기능입니다😅:

- useQuery로 간편한 데이터 가져오기

React Query는 데이터를 가져 오는 데 useEffect가 필요한 필요성을 useQuery 훅으로 대체합니다. 이 훅은 데이터 가져오기, 로딩 상태, 오류 처리 및 캐싱 로직을 캡슐화하여 컴포넌트를 훨씬 더 깔끔하고 유지 관리하기 쉽게 만듭니다.

- 스마트한 다시 가져오기

<div class="content-ad"></div>

캐시 무효화는 꽤 어려운 작업이죠 (아마도 컴퓨터 과학에서 네이밍과 함께 가장 어려운 일일 것입니다 - 그런데, 이 기사 제목을 찾는 데 어려움을 겪었네요). 그럼 언제 백엔드에 새 데이터를 요청할지 결정해야 할까요? useQuery를 호출하는 컴포넌트가 리렌더링될 때마다 매번 이 작업을 수행하면 끔찍하게 많은 자원을 소비하게 될 것입니다. 그래서 React Query는 리패치를 트리거할 전략적인 시점을 선택하는 데 똑똑합니다. "예, 지금 신선한 데이터를 가져오기 좋은 시점일 것 같다"고 말할 수 있는 좋은 지표는 다음과 같습니다:

- refetchOnMount: useQuery를 호출하는 새 컴포넌트가 마운트될 때마다, React Query가 재유효화를 수행합니다.
- refetchOnWindowFocus: 브라우저 탭에 포커스를 맞출 때마다 데이터를 다시 가져옵니다.
- refetchOnReconnect: 네트워크 연결을 잃고 다시 연결했을 때도 좋은 지표입니다.

마지막으로, 개발자로서 데이터를 재유효화할 좋은 시기를 알고 있다면 queryClient.invalidateQueries를 사용하여 수동으로 수행할 수 있습니다 (아마도 변경 후에).

- 자동 캐싱

<div class="content-ad"></div>

데이터를 가져와서 1분 후에 같은 데이터를 다시 가져오면 React Query 캐시로 인해 백엔드와 다시 통신할 필요가 없어요. 내가 소중히 저장한 데이터가 내 손 안에 빠르게 들어오죠. 그래서 React 개발자로서 걱정할 필요 없어요. 이것은 당신의 손이 더럽혀지지 않으면서 API 호출을 줄일 수 있음을 의미해요.

- 페이지네이션 및 무한 쿼리

페이지별 리스트나 무한 스크롤과 같은 대규모 데이터셋을 다루는 시나리오에서, React Query는 useInfiniteQuery라는 특수한 훅을 제공해요. 이 훅은 사용자가 페이지를 스크롤하거나 탐색할 때 데이터를 추가로 로드하는데 내장된 지원을 제공해요.

예시: 사용자가 끝도 없이 스크롤할 수 있는 게시물 목록이 있다고 상상해보세요. useInfiniteQuery를 사용하면 사용자가 스크롤할 때 추가 게시물을 로드하는 것을 React Query가 처리해서 당신은 페이지 번호나 로딩 상태를 수동으로 추적할 필요가 없어요.

<div class="content-ad"></div>

어떻게 사용하나요?
React Query를 사용하는 것은 매우 쉽습니다. 구성 파일이 필요하지 않습니다.
먼저 설치하세요. 설치 방법은 문서의 설치 섹션을 참조하세요.
세팅:

![이미지1](/assets/img/2024-08-21-GoodbyeuseEffectHelloReactQueryAbetterwaytohandledatafetchinginReact_3.png)

그런 다음 쿼리 또는 뮤테이션을 작성할 수 있습니다. 다음은 예시입니다:

![이미지2](/assets/img/2024-08-21-GoodbyeuseEffectHelloReactQueryAbetterwaytohandledatafetchinginReact_4.png)

<div class="content-ad"></div>

마지막으로, "게으른" 접근 방식은 실제로 올바른 접근 방식입니다. React Query와 같은 도구를 사용함으로써, useEffect를 사용한 수동 데이터 가져오기의 함정을 피하고 더 깔끔한 코드를 작성하며 기능을 더 빨리 배포할 수 있습니다.

참고: React Query는 데이터 가져오기 라이브러리가 아닙니다. 실제로 어떤 데이터도 가져오지 않으며, 이것은 queryFn을 작성할 때 명확해집니다. queryFn을 작성할 때 axios, fetch 또는 ky와 같은 라이브러리를 사용하는 함수를 제공해야 합니다.

위에서 데이터 가져오기가 어렵다고 말했지만... 데이터 가져오기 자체는 어렵지 않습니다. 다만 그 데이터를 시간이 지남에 따라 관리하는 것이 어렵습니다. React Query가 데이터 가져오기와 작동하기 때문에, 이를 더 잘 설명하는 방법은 비동기 상태 관리자 또는 서버 상태 관리자로 설명하는 것입니다.

## 참고:

<div class="content-ad"></div>

- React Query 문서: https://tanstack.com/query/latest/docs/framework/react/overview?from=reactQueryV3
- https://tkdodo.eu/blog/why-you-want-react-query