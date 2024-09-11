---
title: "Angular에서 데이터 캐싱하는 방법 정리"
description: ""
coverImage: "/assets/img/2024-09-10-CachingDatainAngular_0.png"
date: 2024-09-10 18:37
ogImage: 
  url: /assets/img/2024-09-10-CachingDatainAngular_0.png
tag: Tech
originalTitle: "Caching Data in Angular"
link: "https://medium.com/@pawan-kumawat/caching-data-in-angular-ffc80dca39b5"
isUpdated: true
updatedAt: 1726022706758
---


가끔은 클라이언트 측에 데이터를 캐시해야 할 때가 있습니다. 이를 달성하는 몇 가지 방법이 있습니다. 그러니 함께 자세히 살펴보겠습니다.

![image](/assets/img/2024-09-10-CachingDatainAngular_0.png)

그러나 코딩을 시작하기 전에 데이터를 캐시할 때에 대해 이해해야 합니다. 몇 가지 시나리오를 설명하겠습니다.

컴포넌트에서 observable 스트림을 선언하고 이를 async 파이프를 사용하여 HTML에서 사용하는 경우를 가정해 봅시다. async 파이프의 장점은 자동으로 구독하고 구독을 해제한다는 것입니다. 우리는 메모리 관리를 걱정할 필요가 없습니다. 그러나 async 파이프를 사용하여 동일한 스트림을 두 번 구독하면 두 번 호출되어 처리하는 최적화되지 않은 방법이 됩니다. 아래 코드는 동일한 문제를 가지고 있습니다.

<div class="content-ad"></div>

두 번 구독하여 스트림을 두 번 사용하고 있습니다. 아래 스냅샷과 같이 두 번 호출되는 현상이 발생합니다.

![image](/assets/img/2024-09-10-CachingDatainAngular_1.png)

이제 share() 또는 shareReplay(1) 오퍼레이터를 사용하여 데이터를 캐시할 수 있습니다.

이제 API는 한 번만 호출되며 async pipe의 두 번째 구독 시 캐시된 데이터가 읽혀지고 HTML에서 사용됩니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-09-10-CachingDatainAngular_2.png)

이해되셨나요?

저는 Udemy에서 Angular 코스를 만들었어요. 이 코스는 Angular에서의 실전 문제들과 해결책을 다루고 있어요. 여러분의 Angular 전문가 여정에 한 발자국이 될 수도 있을 거예요. 한 번 살펴보세요.

![이미지](/assets/img/2024-09-10-CachingDatainAngular_3.png)


<div class="content-ad"></div>

제 무료 YouTube 채널도 구독해보세요.

구독/팔로우/좋아요/박수 눌러주세요.