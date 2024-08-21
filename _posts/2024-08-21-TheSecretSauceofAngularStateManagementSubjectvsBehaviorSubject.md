---
title: "Angular 상태 관리의 비밀 소스 Subject vs BehaviorSubject, 무엇을 사용해야 할까요"
description: ""
coverImage: "/assets/img/2024-08-21-TheSecretSauceofAngularStateManagementSubjectvsBehaviorSubject_0.png"
date: 2024-08-21 17:13
ogImage: 
  url: /assets/img/2024-08-21-TheSecretSauceofAngularStateManagementSubjectvsBehaviorSubject_0.png
tag: Tech
originalTitle: "The Secret Sauce of Angular State Management Subject vs. BehaviorSubject"
link: "https://medium.com/stackademic/the-secret-sauce-of-angular-state-management-subject-vs-behaviorsubject-b624acee13cf"
isUpdated: false
---



![Image](/assets/img/2024-08-21-TheSecretSauceofAngularStateManagementSubjectvsBehaviorSubject_0.png)

Angular을 다루고 계셨군요! RxJS에서 Subject와 BehaviorSubject를 만나셨군요. 이 둘은 Angular 상태 관리의 배트맨과 로빈 같은 존재죠. 그렇다면 어떤 것을 언제 사용해야 할지 어떻게 알 수 있을까요? 순서대로 알려드리며 이 강력한 도구에 대해 빠르게 익히도록 하죠.

## 옵저버블이 왜 중요한가요?

먼저, Angular 반응형 프로그래밍의 기초인 옵저버블에 대해 이야기해봅시다. 옵저버블은 데이터를 운반하는 강(강물)으로 상상해보세요. 아무 떨기 잡거나 튀어들어가도(구독해도) 되고, 그 물(데이터)은 당신이 주목하든 말든 계속 흐르죠.


<div class="content-ad"></div>

앵귤러는 HTTP 요청, 이벤트 처리 및 컴포넌트 간 데이터 공유와 같은 많은 기능에 옵저버블을 사용합니다. 그런데 여기 한가지 문제가 있습니다: 전통적인 옵저버블은 'cold'입니다. 누군가 구독을 시작할 때만 데이터를 방출하기 시작합니다. 이때 Subject와 BehaviorSubject가 등장해 여러분을 구해줍니다.

# Subject이란 무엇인가요?

Subject는 실시간 뉴스 방송과 같습니다. 처음부터 시청하든 중간에 합류하든 상관없이 업데이트는 실시간으로 전달됩니다. 여러 구독자에 동시에 데이터를 푸시하여 앱 전체에서 실시간 업데이트를 공유하고 싶을 때 완벽한 선택지가 됩니다.

## 언제 Subject를 사용해야 할까요?

<div class="content-ad"></div>

앱에서 알림 시스템을 구축 중이라고 상상해보세요. 사용자들은 새로운 메시지나 업데이트와 같이 중요한 일이 발생할 때 실시간 알림을 받아야 합니다. 이전 알림을 추적할 필요는 없고, 현재 일어나고 있는 일에만 신경을 쓰면 됩니다.

다음은 간단한 예시입니다:

![notification example](/assets/img/2024-08-21-TheSecretSauceofAngularStateManagementSubjectvsBehaviorSubject_1.png)

이 설정에서 ComponentB는 새로운 메시지가 도착하자마자 알림을 받습니다. 그러나 ComponentA가 메시지를 보낸 시점에서 ComponentB가 듣고 있지 않았다면 메시지를 받지 못할 수도 있습니다. 그래도 괜찮습니다. 왜냐하면 이 상황에서 우리는 가장 최근 소식에만 관심이 있기 때문이죠.

<div class="content-ad"></div>

# BehaviorSubject에 인사를 해보세요: 기억 저장소

이제 BehaviorSubject에 대해 이야기해 봅시다. 미팅에 늦게 도착했을 때, 누군가가 이미 토론한 내용을 알려준다면 상상해보세요. 바로 BehaviorSubject가 하는 일입니다. 마지막으로 발행된 값을 보유하고 새로운 구독자에게 즉시 제공합니다.

## 언제 BehaviorSubject를 사용해야 할까요?

현재 로그인한 사용자를 추적하는 사용자 서비스를 구축 중이라고 가정해 보죠. 사용자가 로그인한 후에도 사용자가 누군지 알아야 하는 앱의 모든 부분이 알아야 합니다.

<div class="content-ad"></div>

요렇게 코딩할 수 있어요:


![Image](/assets/img/2024-08-21-TheSecretSauceofAngularStateManagementSubjectvsBehaviorSubject_2.png)


여기서 ComponentB는 항상 현재 사용자가 누군지 알 수 있습니다. 이벤트 서브젝트는 모두가 최신 정보를 놓치지 않도록 합니다.

# 그래서, 어떤 것을 사용해야 할까요?

<div class="content-ad"></div>

- 실시간 데이터를 다룰 때 저장하거나 재생할 필요가 없는 경우 Subject를 선택하세요. 현재 일어나고 있는 일에 중점을 두는 것입니다.
- 모두가 같은 페이지에 있음을 확실히 해야 할 때 BehaviorSubject를 선택하세요. 나중에 가입하는 사용자라도 모두가 일치해야 하는 상태를 관리하는 데 이상적입니다.

# 마치며

Subject와 BehaviorSubject를 사용할 때의 이점을 이해하면 Angular 앱을 더 견고하고 유지보수가 쉬워질 수 있습니다. 알림 시스템을 구축하거나 사용자 데이터를 관리하거나 컴포넌트를 동기화하려는 경우, 이 두 도구가 모든 것을 원할하게 유지하는 데 도움이 될 것입니다.

그럼 이것으로 Subject와 BehaviorSubject에 대해 알아보았습니다. AI 용어 없이 Angular에 대한 솔직한 이야기였습니다. 즐거운 코딩하세요! 🚀

<div class="content-ad"></div>

# Stackademic 🎓

끝까지 읽어 주셔서 감사합니다. 가기 전에:

- 작가를 응원하고 팔로우해 주시면 감사하겠습니다! 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord
- 다른 플랫폼 방문: In Plain English | CoFeed | Differ
- Stackademic.com에서 더 많은 콘텐츠 확인하기